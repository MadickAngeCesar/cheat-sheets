# ElectronJs Cheat Sheet

## Table of Contents
- [Introduction](#introduction)
- [Installation & Setup](#installation--setup)
- [Project Structure](#project-structure)
- [Main Process vs Renderer Process](#main-process-vs-renderer-process)
- [Creating Windows](#creating-windows)
- [IPC Communication](#ipc-communication)
- [Menu & Tray](#menu--tray)
- [File System & Dialogs](#file-system--dialogs)
- [Context Bridge & Security](#context-bridge--security)
- [Notifications](#notifications)
- [Auto Updates](#auto-updates)
- [Native Features](#native-features)
- [Debugging](#debugging)
- [Build & Distribution](#build--distribution)
- [Performance Optimization](#performance-optimization)
- [Advanced Patterns](#advanced-patterns)
- [Electron with React](#electron-with-react)
- [Electron with Next.js](#electron-with-nextjs)

---

## Introduction

**Electron** is a framework for building cross-platform desktop applications using web technologies (HTML, CSS, JavaScript). It combines Chromium and Node.js into a single runtime.

### Key Concepts
- **Main Process**: Backend Node.js process (one per app)
- **Renderer Process**: Frontend browser window (one per window)
- **IPC**: Inter-Process Communication between main and renderer

---

## Installation & Setup

### Install Electron
```bash
npm install --save-dev electron
```

### Basic package.json
```json
{
  "name": "my-electron-app",
  "version": "1.0.0",
  "main": "main.js",
  "scripts": {
    "start": "electron .",
    "dev": "electron . --inspect=5858"
  },
  "devDependencies": {
    "electron": "^28.0.0"
  }
}
```

### Quick Start Template
```bash
npm init -y
npm install --save-dev electron
```

---

## Project Structure

### Recommended Structure
```
my-electron-app/
├── main.js                 # Main process
├── preload.js             # Preload script
├── index.html             # Entry HTML
├── renderer.js            # Renderer script
├── package.json
├── assets/
│   └── icon.png
└── src/
    ├── main/
    │   ├── ipc-handlers.js
    │   └── menu.js
    └── renderer/
        ├── index.html
        └── renderer.js
```

---

## Main Process vs Renderer Process

### Main Process (main.js)
```javascript
const { app, BrowserWindow } = require('electron');
const path = require('path');

function createWindow() {
  const mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js'),
      nodeIntegration: false,
      contextIsolation: true
    }
  });

  mainWindow.loadFile('index.html');
}

app.whenReady().then(() => {
  createWindow();

  app.on('activate', () => {
    if (BrowserWindow.getAllWindows().length === 0) {
      createWindow();
    }
  });
});

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});
```

### Renderer Process (renderer.js)
```javascript
// Runs in browser context
document.getElementById('btn').addEventListener('click', () => {
  console.log('Button clicked!');
});
```

---

## Creating Windows

### Basic Window
```javascript
const { BrowserWindow } = require('electron');

const win = new BrowserWindow({
  width: 1000,
  height: 800,
  title: 'My App'
});

win.loadURL('https://example.com');
// or
win.loadFile('index.html');
```

### Window Options
```javascript
const win = new BrowserWindow({
  width: 800,
  height: 600,
  minWidth: 400,
  minHeight: 300,
  maxWidth: 1200,
  maxHeight: 900,
  resizable: true,
  movable: true,
  minimizable: true,
  maximizable: true,
  closable: true,
  focusable: true,
  alwaysOnTop: false,
  fullscreen: false,
  kiosk: false,
  frame: true,              // Window frame
  show: false,              // Show later for smooth loading
  backgroundColor: '#FFF',
  transparent: false,
  opacity: 1.0,
  icon: path.join(__dirname, 'assets/icon.png'),
  webPreferences: {
    nodeIntegration: false,
    contextIsolation: true,
    preload: path.join(__dirname, 'preload.js')
  }
});
```

### Window Events
```javascript
win.on('ready-to-show', () => {
  win.show();
});

win.on('closed', () => {
  win = null;
});

win.on('blur', () => console.log('Window lost focus'));
win.on('focus', () => console.log('Window gained focus'));
win.on('maximize', () => console.log('Window maximized'));
win.on('minimize', () => console.log('Window minimized'));
win.on('restore', () => console.log('Window restored'));
```

### Window Methods
```javascript
win.setTitle('New Title');
win.center();
win.maximize();
win.minimize();
win.restore();
win.close();
win.destroy();
win.reload();
win.setFullScreen(true);
win.setBounds({ x: 100, y: 100, width: 800, height: 600 });
win.setSize(1024, 768);
win.setPosition(100, 100);
```

### Frameless Window
```javascript
const win = new BrowserWindow({
  frame: false,
  titleBarStyle: 'hidden', // macOS only
  transparent: true
});
```

### Parent-Child Windows
```javascript
const parent = new BrowserWindow();
const child = new BrowserWindow({ 
  parent: parent,
  modal: true 
});
```

---

## IPC Communication

### Main to Renderer (Send)

**Main Process:**
```javascript
const { ipcMain } = require('electron');

// Send to specific window
mainWindow.webContents.send('channel-name', 'data');

// Send to all windows
BrowserWindow.getAllWindows().forEach(win => {
  win.webContents.send('channel-name', 'data');
});
```

**Preload:**
```javascript
const { contextBridge, ipcRenderer } = require('electron');

contextBridge.exposeInMainWorld('electron', {
  onChannelName: (callback) => ipcRenderer.on('channel-name', callback)
});
```

**Renderer:**
```javascript
window.electron.onChannelName((event, data) => {
  console.log(data);
});
```

### Renderer to Main (Invoke/Handle)

**Main Process:**
```javascript
const { ipcMain } = require('electron');

ipcMain.handle('get-data', async (event, arg) => {
  const result = await someAsyncOperation(arg);
  return result;
});

ipcMain.on('sync-message', (event, arg) => {
  console.log(arg);
  event.reply('sync-reply', 'Response data');
});
```

**Preload:**
```javascript
const { contextBridge, ipcRenderer } = require('electron');

contextBridge.exposeInMainWorld('api', {
  getData: (arg) => ipcRenderer.invoke('get-data', arg),
  sendMessage: (arg) => ipcRenderer.send('sync-message', arg),
  onReply: (callback) => ipcRenderer.on('sync-reply', callback)
});
```

**Renderer:**
```javascript
// Async invoke
const result = await window.api.getData('argument');

// One-way send
window.api.sendMessage('Hello from renderer');

window.api.onReply((event, data) => {
  console.log(data);
});
```

### Advanced IPC Patterns

**Request-Response Pattern:**
```javascript
// Main
ipcMain.handle('fetch-user', async (event, userId) => {
  const user = await database.getUser(userId);
  return user;
});

// Renderer
const user = await window.api.fetchUser(123);
```

**Event Streaming:**
```javascript
// Main
let interval;
ipcMain.on('start-stream', (event) => {
  interval = setInterval(() => {
    event.sender.send('stream-data', { timestamp: Date.now() });
  }, 1000);
});

ipcMain.on('stop-stream', () => {
  clearInterval(interval);
});
```

---

## Menu & Tray

### Application Menu
```javascript
const { Menu } = require('electron');

const template = [
  {
    label: 'File',
    submenu: [
      {
        label: 'Open',
        accelerator: 'CmdOrCtrl+O',
        click: () => { console.log('Open clicked'); }
      },
      {
        label: 'Save',
        accelerator: 'CmdOrCtrl+S',
        click: () => { console.log('Save clicked'); }
      },
      { type: 'separator' },
      { role: 'quit' }
    ]
  },
  {
    label: 'Edit',
    submenu: [
      { role: 'undo' },
      { role: 'redo' },
      { type: 'separator' },
      { role: 'cut' },
      { role: 'copy' },
      { role: 'paste' },
      { role: 'delete' },
      { type: 'separator' },
      { role: 'selectAll' }
    ]
  },
  {
    label: 'View',
    submenu: [
      { role: 'reload' },
      { role: 'forceReload' },
      { role: 'toggleDevTools' },
      { type: 'separator' },
      { role: 'resetZoom' },
      { role: 'zoomIn' },
      { role: 'zoomOut' },
      { type: 'separator' },
      { role: 'togglefullscreen' }
    ]
  }
];

const menu = Menu.buildFromTemplate(template);
Menu.setApplicationMenu(menu);
```

### Context Menu
```javascript
const { Menu, MenuItem } = require('electron');

const contextMenu = new Menu();
contextMenu.append(new MenuItem({ label: 'Cut', role: 'cut' }));
contextMenu.append(new MenuItem({ label: 'Copy', role: 'copy' }));
contextMenu.append(new MenuItem({ label: 'Paste', role: 'paste' }));

// In renderer (via IPC)
window.addEventListener('contextmenu', (e) => {
  e.preventDefault();
  window.api.showContextMenu();
});

// Main process
ipcMain.on('show-context-menu', (event) => {
  const template = [
    { label: 'Item 1', click: () => { console.log('Item 1 clicked'); } },
    { type: 'separator' },
    { label: 'Item 2', click: () => { console.log('Item 2 clicked'); } }
  ];
  const menu = Menu.buildFromTemplate(template);
  menu.popup(BrowserWindow.fromWebContents(event.sender));
});
```

### System Tray
```javascript
const { Tray, Menu, nativeImage } = require('electron');
let tray = null;

app.whenReady().then(() => {
  const icon = nativeImage.createFromPath(path.join(__dirname, 'icon.png'));
  tray = new Tray(icon.resize({ width: 16, height: 16 }));
  
  const contextMenu = Menu.buildFromTemplate([
    { label: 'Show App', click: () => { mainWindow.show(); } },
    { label: 'Quit', click: () => { app.quit(); } }
  ]);
  
  tray.setToolTip('My Electron App');
  tray.setContextMenu(contextMenu);
  
  tray.on('click', () => {
    mainWindow.isVisible() ? mainWindow.hide() : mainWindow.show();
  });
});
```

---

## File System & Dialogs

### Native Dialogs
```javascript
const { dialog } = require('electron');

// Open File Dialog
const openFile = async () => {
  const result = await dialog.showOpenDialog({
    title: 'Select a file',
    defaultPath: app.getPath('documents'),
    buttonLabel: 'Open',
    filters: [
      { name: 'Images', extensions: ['jpg', 'png', 'gif'] },
      { name: 'Videos', extensions: ['mkv', 'avi', 'mp4'] },
      { name: 'All Files', extensions: ['*'] }
    ],
    properties: ['openFile', 'multiSelections']
  });
  
  if (!result.canceled) {
    console.log(result.filePaths);
  }
};

// Save Dialog
const saveFile = async () => {
  const result = await dialog.showSaveDialog({
    title: 'Save file',
    defaultPath: path.join(app.getPath('documents'), 'file.txt'),
    buttonLabel: 'Save',
    filters: [
      { name: 'Text Files', extensions: ['txt'] },
      { name: 'All Files', extensions: ['*'] }
    ]
  });
  
  if (!result.canceled) {
    console.log(result.filePath);
    // Write file here
  }
};

// Message Box
const showMessage = async () => {
  const result = await dialog.showMessageBox({
    type: 'info', // 'none', 'info', 'error', 'question', 'warning'
    title: 'Title',
    message: 'Main message',
    detail: 'Additional details',
    buttons: ['OK', 'Cancel'],
    defaultId: 0,
    cancelId: 1
  });
  
  console.log(result.response); // Button index
};

// Error Box (synchronous)
dialog.showErrorBox('Error Title', 'Error message');
```

### File Operations
```javascript
const fs = require('fs').promises;
const path = require('path');

// Read file
ipcMain.handle('read-file', async (event, filePath) => {
  try {
    const data = await fs.readFile(filePath, 'utf-8');
    return { success: true, data };
  } catch (error) {
    return { success: false, error: error.message };
  }
});

// Write file
ipcMain.handle('write-file', async (event, filePath, content) => {
  try {
    await fs.writeFile(filePath, content, 'utf-8');
    return { success: true };
  } catch (error) {
    return { success: false, error: error.message };
  }
});

// Get app paths
const userDataPath = app.getPath('userData');
const documentsPath = app.getPath('documents');
const desktopPath = app.getPath('desktop');
const downloadsPath = app.getPath('downloads');
const tempPath = app.getPath('temp');
```

---

## Context Bridge & Security

### Secure Preload Script
```javascript
// preload.js
const { contextBridge, ipcRenderer } = require('electron');

// Expose protected methods that allow the renderer process to use
// the ipcRenderer without exposing the entire object
contextBridge.exposeInMainWorld('api', {
  // Invoke methods
  openFile: () => ipcRenderer.invoke('dialog:openFile'),
  saveFile: (content) => ipcRenderer.invoke('dialog:saveFile', content),
  
  // Send methods (one-way)
  minimize: () => ipcRenderer.send('window:minimize'),
  maximize: () => ipcRenderer.send('window:maximize'),
  close: () => ipcRenderer.send('window:close'),
  
  // Receive methods (from main to renderer)
  onUpdateDownloaded: (callback) => {
    ipcRenderer.on('update:downloaded', callback);
  },
  
  // Remove listener
  removeListener: (channel) => {
    ipcRenderer.removeAllListeners(channel);
  }
});

// Expose selective Node.js APIs
contextBridge.exposeInMainWorld('node', {
  platform: process.platform,
  version: process.version
});
```

### Security Best Practices
```javascript
const win = new BrowserWindow({
  webPreferences: {
    // ✅ REQUIRED SECURITY SETTINGS
    nodeIntegration: false,        // Don't integrate Node.js in renderer
    contextIsolation: true,        // Isolate preload script context
    sandbox: true,                 // Enable Chromium sandbox
    
    // ✅ RECOMMENDED SETTINGS
    webSecurity: true,             // Enable web security
    allowRunningInsecureContent: false,
    
    // Preload script
    preload: path.join(__dirname, 'preload.js'),
    
    // ✅ DISABLE DANGEROUS FEATURES
    enableRemoteModule: false,     // Remote module is deprecated
    nodeIntegrationInWorker: false,
    nodeIntegrationInSubFrames: false
  }
});

// ✅ Validate navigation
win.webContents.on('will-navigate', (event, navigationUrl) => {
  const parsedUrl = new URL(navigationUrl);
  
  if (parsedUrl.origin !== 'https://example.com') {
    event.preventDefault();
  }
});

// ✅ Prevent new window creation
win.webContents.setWindowOpenHandler(({ url }) => {
  // Only allow specific URLs
  if (url.startsWith('https://example.com')) {
    return { action: 'allow' };
  }
  return { action: 'deny' };
});

// ✅ Content Security Policy
win.webContents.session.webRequest.onHeadersReceived((details, callback) => {
  callback({
    responseHeaders: {
      ...details.responseHeaders,
      'Content-Security-Policy': ["default-src 'self'"]
    }
  });
});
```

---

## Notifications

### Desktop Notifications
```javascript
const { Notification } = require('electron');

// Check if supported
if (Notification.isSupported()) {
  const notification = new Notification({
    title: 'Notification Title',
    body: 'Notification body text',
    icon: path.join(__dirname, 'icon.png'),
    silent: false,
    urgency: 'normal', // 'normal', 'critical', 'low' (Linux only)
    timeoutType: 'default', // 'default', 'never'
    actions: [
      { type: 'button', text: 'Show' },
      { type: 'button', text: 'Dismiss' }
    ]
  });
  
  notification.show();
  
  notification.on('click', () => {
    console.log('Notification clicked');
  });
  
  notification.on('action', (event, index) => {
    console.log('Action clicked:', index);
  });
  
  notification.on('close', () => {
    console.log('Notification closed');
  });
}
```

### HTML5 Notifications (Renderer)
```javascript
// In renderer process
if ('Notification' in window) {
  Notification.requestPermission().then(permission => {
    if (permission === 'granted') {
      new Notification('Hello!', {
        body: 'This is a notification',
        icon: 'icon.png'
      });
    }
  });
}
```

---

## Auto Updates

### Using electron-updater
```bash
npm install electron-updater
```

**Main Process:**
```javascript
const { app } = require('electron');
const { autoUpdater } = require('electron-updater');
const log = require('electron-log');

// Configure logging
autoUpdater.logger = log;
autoUpdater.logger.transports.file.level = 'info';

// Check for updates on startup
app.whenReady().then(() => {
  autoUpdater.checkForUpdatesAndNotify();
});

// Auto-update events
autoUpdater.on('checking-for-update', () => {
  console.log('Checking for update...');
});

autoUpdater.on('update-available', (info) => {
  console.log('Update available:', info);
});

autoUpdater.on('update-not-available', (info) => {
  console.log('Update not available:', info);
});

autoUpdater.on('error', (err) => {
  console.log('Error in auto-updater:', err);
});

autoUpdater.on('download-progress', (progressObj) => {
  let message = `Download speed: ${progressObj.bytesPerSecond}`;
  message += ` - Downloaded ${progressObj.percent}%`;
  message += ` (${progressObj.transferred}/${progressObj.total})`;
  console.log(message);
});

autoUpdater.on('update-downloaded', (info) => {
  console.log('Update downloaded:', info);
  // Prompt user to restart
  dialog.showMessageBox({
    type: 'info',
    title: 'Update Ready',
    message: 'A new version has been downloaded. Restart to apply updates.',
    buttons: ['Restart', 'Later']
  }).then((result) => {
    if (result.response === 0) {
      autoUpdater.quitAndInstall();
    }
  });
});

// Manual check
ipcMain.handle('check-for-updates', () => {
  autoUpdater.checkForUpdates();
});
```

**package.json configuration:**
```json
{
  "build": {
    "appId": "com.example.app",
    "productName": "MyApp",
    "publish": [
      {
        "provider": "github",
        "owner": "username",
        "repo": "repo-name"
      }
    ]
  }
}
```

---

## Native Features

### System Information
```javascript
const { app, screen, powerMonitor, powerSaveBlocker } = require('electron');
const os = require('os');

// App info
console.log(app.getName());
console.log(app.getVersion());
console.log(app.getLocale());
console.log(app.getPath('userData'));

// System info
console.log(process.platform); // 'darwin', 'win32', 'linux'
console.log(process.arch); // 'x64', 'arm64'
console.log(os.cpus());
console.log(os.totalmem());
console.log(os.freemem());

// Screen info
const primaryDisplay = screen.getPrimaryDisplay();
console.log(primaryDisplay.bounds);
console.log(primaryDisplay.workArea);
console.log(primaryDisplay.scaleFactor);

const allDisplays = screen.getAllDisplays();
```

### Power Management
```javascript
const { powerMonitor, powerSaveBlocker } = require('electron');

// Power events
app.whenReady().then(() => {
  powerMonitor.on('suspend', () => {
    console.log('System is going to sleep');
  });
  
  powerMonitor.on('resume', () => {
    console.log('System is resuming');
  });
  
  powerMonitor.on('on-ac', () => {
    console.log('System is on AC power');
  });
  
  powerMonitor.on('on-battery', () => {
    console.log('System is on battery power');
  });
  
  // Check current state
  console.log('Idle time:', powerMonitor.getSystemIdleTime());
  console.log('Idle state:', powerMonitor.getSystemIdleState(60));
});

// Prevent sleep
const id = powerSaveBlocker.start('prevent-app-suspension');
console.log(powerSaveBlocker.isStarted(id));
powerSaveBlocker.stop(id);
```

### Shell Operations
```javascript
const { shell } = require('electron');

// Open external links
shell.openExternal('https://example.com');

// Open file/folder
shell.openPath('/path/to/file.txt');
shell.showItemInFolder('/path/to/file.txt');

// Move to trash
shell.trashItem('/path/to/file.txt');

// Beep
shell.beep();
```

### Clipboard
```javascript
const { clipboard, nativeImage } = require('electron');

// Text
clipboard.writeText('Hello from Electron');
const text = clipboard.readText();

// HTML
clipboard.writeHTML('<b>Bold text</b>');
const html = clipboard.readHTML();

// Image
const image = nativeImage.createFromPath('/path/to/image.png');
clipboard.writeImage(image);
const clipImage = clipboard.readImage();

// Clear
clipboard.clear();
```

### Global Shortcuts
```javascript
const { globalShortcut } = require('electron');

app.whenReady().then(() => {
  // Register shortcut
  const ret = globalShortcut.register('CommandOrControl+X', () => {
    console.log('CommandOrControl+X is pressed');
  });
  
  if (!ret) {
    console.log('Registration failed');
  }
  
  // Check if registered
  console.log(globalShortcut.isRegistered('CommandOrControl+X'));
});

app.on('will-quit', () => {
  // Unregister shortcuts
  globalShortcut.unregister('CommandOrControl+X');
  globalShortcut.unregisterAll();
});
```

---

## Debugging

### DevTools
```javascript
// Open DevTools
mainWindow.webContents.openDevTools();

// Open in detached mode
mainWindow.webContents.openDevTools({ mode: 'detach' });

// Close DevTools
mainWindow.webContents.closeDevTools();

// Toggle DevTools
mainWindow.webContents.toggleDevTools();

// Check if open
mainWindow.webContents.isDevToolsOpened();
```

### Main Process Debugging

**VS Code launch.json:**
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug Main Process",
      "type": "node",
      "request": "launch",
      "cwd": "${workspaceFolder}",
      "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/electron",
      "windows": {
        "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/electron.cmd"
      },
      "args": ["."],
      "outputCapture": "std"
    }
  ]
}
```

### Logging
```javascript
// Console logging
console.log('Main process log');
console.error('Error in main process');

// Electron-log
const log = require('electron-log');

log.info('Info message');
log.warn('Warning message');
log.error('Error message');

// Custom log levels
log.transports.file.level = 'info';
log.transports.console.level = 'debug';

// Log file location
console.log(log.transports.file.getFile().path);
```

### Error Handling
```javascript
// Main process errors
process.on('uncaughtException', (error) => {
  console.error('Uncaught Exception:', error);
  // Log error, show dialog, etc.
});

process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Rejection:', reason);
});

// Renderer process errors (in main)
mainWindow.webContents.on('crashed', (event, killed) => {
  console.error('Renderer process crashed');
  // Reload or restart
  mainWindow.reload();
});

// GPU process crashed
app.on('gpu-process-crashed', (event, killed) => {
  console.error('GPU process crashed');
});
```

---

## Build & Distribution

### Using electron-builder
```bash
npm install --save-dev electron-builder
```

**package.json:**
```json
{
  "name": "my-app",
  "version": "1.0.0",
  "main": "main.js",
  "scripts": {
    "start": "electron .",
    "build": "electron-builder",
    "build:win": "electron-builder --win",
    "build:mac": "electron-builder --mac",
    "build:linux": "electron-builder --linux"
  },
  "build": {
    "appId": "com.example.myapp",
    "productName": "My App",
    "copyright": "Copyright © 2024",
    "directories": {
      "output": "dist"
    },
    "files": [
      "main.js",
      "preload.js",
      "renderer.js",
      "index.html",
      "assets/**/*",
      "node_modules/**/*"
    ],
    "win": {
      "target": ["nsis", "portable"],
      "icon": "assets/icon.ico"
    },
    "mac": {
      "target": ["dmg", "zip"],
      "icon": "assets/icon.icns",
      "category": "public.app-category.productivity"
    },
    "linux": {
      "target": ["AppImage", "deb", "rpm"],
      "icon": "assets/icon.png",
      "category": "Utility"
    },
    "nsis": {
      "oneClick": false,
      "allowToChangeInstallationDirectory": true,
      "createDesktopShortcut": true,
      "createStartMenuShortcut": true
    }
  }
}
```

### Code Signing (macOS)
```json
{
  "build": {
    "mac": {
      "identity": "Developer ID Application: Your Name (TEAM_ID)",
      "hardenedRuntime": true,
      "gatekeeperAssess": false,
      "entitlements": "build/entitlements.mac.plist",
      "entitlementsInherit": "build/entitlements.mac.plist"
    },
    "afterSign": "scripts/notarize.js"
  }
}
```

### Code Signing (Windows)
```json
{
  "build": {
    "win": {
      "certificateFile": "path/to/certificate.pfx",
      "certificatePassword": "password",
      "signingHashAlgorithms": ["sha256"],
      "signDlls": true
    }
  }
}
```

---

## Performance Optimization

### Lazy Loading
```javascript
// Load window content only when needed
const win = new BrowserWindow({ show: false });
win.once('ready-to-show', () => {
  win.show();
});
```

### Offscreen Rendering
```javascript
const win = new BrowserWindow({
  webPreferences: {
    offscreen: true
  }
});

win.webContents.on('paint', (event, dirty, image) => {
  // Handle rendered frame
});

win.webContents.startPainting();
```

### Memory Management
```javascript
// Clear cache
session.defaultSession.clearCache();

// Limit cache size
app.commandLine.appendSwitch('disk-cache-size', '50000000'); // 50MB

// Disable GPU if not needed
app.disableHardwareAcceleration();

// Enable process reuse
app.commandLine.appendSwitch('disable-renderer-backgrounding');
```

### Web Performance
```javascript
// Enable resource throttling
win.webContents.setBackgroundThrottling(true);

// Preload scripts
const preloadScript = `
  // Preload critical resources
  const link = document.createElement('link');
  link.rel = 'preload';
  link.href = 'styles.css';
  link.as = 'style';
  document.head.appendChild(link);
`;

// Use V8 snapshot
app.commandLine.appendSwitch('v8-cache-options', 'code');
```

---

## Advanced Patterns

### Multi-Window Management
```javascript
class WindowManager {
  constructor() {
    this.windows = new Map();
  }
  
  create(name, options) {
    const win = new BrowserWindow(options);
    this.windows.set(name, win);
    
    win.on('closed', () => {
      this.windows.delete(name);
    });
    
    return win;
  }
  
  get(name) {
    return this.windows.get(name);
  }
  
  getAll() {
    return Array.from(this.windows.values());
  }
  
  closeAll() {
    this.windows.forEach(win => win.close());
  }
}

const windowManager = new WindowManager();
```

### State Management
```javascript
const Store = require('electron-store');

const store = new Store({
  defaults: {
    windowBounds: { width: 800, height: 600 },
    theme: 'light'
  }
});

// Save window state
function saveWindowState(win) {
  store.set('windowBounds', win.getBounds());
}

// Restore window state
function createWindow() {
  const bounds = store.get('windowBounds');
  const win = new BrowserWindow(bounds);
  
  win.on('close', () => saveWindowState(win));
  
  return win;
}
```

### Protocol Handling
```javascript
const { protocol } = require('electron');

app.whenReady().then(() => {
  // Register custom protocol
  protocol.registerFileProtocol('myapp', (request, callback) => {
    const url = request.url.substr(8); // Remove 'myapp://'
    callback({ path: path.normalize(`${__dirname}/${url}`) });
  });
});

// Handle deep links
app.setAsDefaultProtocolClient('myapp');

app.on('open-url', (event, url) => {
  event.preventDefault();
  console.log('Opened URL:', url);
  // Handle myapp:// URLs
});

// Windows/Linux
const gotTheLock = app.requestSingleInstanceLock();

if (!gotTheLock) {
  app.quit();
} else {
  app.on('second-instance', (event, commandLine, workingDirectory) => {
    // Handle second instance
    const url = commandLine.pop();
    console.log('Deep link:', url);
  });
}
```

### Worker Threads
```javascript
const { Worker } = require('worker_threads');

// Main process
function runWorker(data) {
  return new Promise((resolve, reject) => {
    const worker = new Worker('./worker.js');
    
    worker.postMessage(data);
    
    worker.on('message', resolve);
    worker.on('error', reject);
    worker.on('exit', (code) => {
      if (code !== 0) {
        reject(new Error(`Worker stopped with exit code ${code}`));
      }
    });
  });
}

// worker.js
const { parentPort } = require('worker_threads');

parentPort.on('message', (data) => {
  // Heavy computation
  const result = processData(data);
  parentPort.postMessage(result);
});
```

### Session Management
```javascript
const { session } = require('electron');

// Custom session
const customSession = session.fromPartition('persist:custom');

const win = new BrowserWindow({
  webPreferences: {
    session: customSession
  }
});

// Cookies
customSession.cookies.get({ url: 'https://example.com' })
  .then((cookies) => console.log(cookies));

customSession.cookies.set({
  url: 'https://example.com',
  name: 'session',
  value: 'token123'
});

// Network interception
customSession.webRequest.onBeforeRequest((details, callback) => {
  // Modify or cancel requests
  if (details.url.includes('ads')) {
    callback({ cancel: true });
  } else {
    callback({ cancel: false });
  }
});

// Download handling
customSession.on('will-download', (event, item, webContents) => {
  item.setSavePath('/path/to/save/file');
  
  item.on('updated', (event, state) => {
    if (state === 'interrupted') {
      console.log('Download interrupted');
    } else if (state === 'progressing') {
      if (item.isPaused()) {
        console.log('Download paused');
      } else {
        console.log(`Received: ${item.getReceivedBytes()}`);
      }
    }
  });
  
  item.once('done', (event, state) => {
    if (state === 'completed') {
      console.log('Download completed');
    } else {
      console.log(`Download failed: ${state}`);
    }
  });
});
```

### Native Modules
```javascript
// Using native Node.js addons
const nativeModule = require('./build/Release/native-module');

// Better SQLite3
const Database = require('better-sqlite3');
const db = new Database('app.db');

// Native dependencies with electron-rebuild
// package.json
{
  "scripts": {
    "rebuild": "electron-rebuild -f -w better-sqlite3"
  }
}
```

---

## Electron with React

### Setup with Create React App

**1. Create React App:**
```bash
npx create-react-app my-electron-app
cd my-electron-app
npm install --save-dev electron electron-is-dev concurrently wait-on cross-env
```

**2. Create main.js (Electron Main Process):**
```javascript
const { app, BrowserWindow } = require('electron');
const path = require('path');
const isDev = require('electron-is-dev');

function createWindow() {
  const mainWindow = new BrowserWindow({
    width: 1200,
    height: 800,
    webPreferences: {
      nodeIntegration: false,
      contextIsolation: true,
      preload: path.join(__dirname, 'preload.js')
    }
  });

  // Load from localhost in dev, or build folder in production
  mainWindow.loadURL(
    isDev
      ? 'http://localhost:3000'
      : `file://${path.join(__dirname, '../build/index.html')}`
  );

  if (isDev) {
    mainWindow.webContents.openDevTools();
  }
}

app.whenReady().then(createWindow);

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', () => {
  if (BrowserWindow.getAllWindows().length === 0) {
    createWindow();
  }
});
```

**3. Create preload.js:**
```javascript
const { contextBridge, ipcRenderer } = require('electron');

contextBridge.exposeInMainWorld('electron', {
  sendMessage: (channel, data) => {
    const validChannels = ['toMain'];
    if (validChannels.includes(channel)) {
      ipcRenderer.send(channel, data);
    }
  },
  receiveMessage: (channel, func) => {
    const validChannels = ['fromMain'];
    if (validChannels.includes(channel)) {
      ipcRenderer.on(channel, (event, ...args) => func(...args));
    }
  },
  invoke: (channel, data) => {
    const validChannels = ['getData'];
    if (validChannels.includes(channel)) {
      return ipcRenderer.invoke(channel, data);
    }
  }
});
```

**4. Update package.json:**
```json
{
  "name": "my-electron-app",
  "version": "1.0.0",
  "main": "public/main.js",
  "homepage": "./",
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "electron:dev": "concurrently \"cross-env BROWSER=none npm start\" \"wait-on http://localhost:3000 && electron .\"",
    "electron:build": "npm run build && electron-builder"
  },
  "build": {
    "appId": "com.example.myapp",
    "files": [
      "build/**/*",
      "node_modules/**/*",
      "public/main.js",
      "public/preload.js"
    ],
    "directories": {
      "buildResources": "assets"
    }
  }
}
```

**5. Move main.js and preload.js to public folder:**
```
my-electron-app/
├── public/
│   ├── main.js
│   ├── preload.js
│   └── index.html
├── src/
│   ├── App.js
│   └── index.js
└── package.json
```

### Using Electron API in React

**App.js:**
```javascript
import React, { useState, useEffect } from 'react';
import './App.css';

function App() {
  const [message, setMessage] = useState('');

  useEffect(() => {
    // Listen for messages from main process
    if (window.electron) {
      window.electron.receiveMessage('fromMain', (data) => {
        console.log('Received:', data);
        setMessage(data);
      });
    }
  }, []);

  const sendToElectron = () => {
    if (window.electron) {
      window.electron.sendMessage('toMain', 'Hello from React!');
    }
  };

  const fetchData = async () => {
    if (window.electron) {
      const result = await window.electron.invoke('getData', 'some-arg');
      console.log('Result:', result);
    }
  };

  return (
    <div className="App">
      <h1>Electron + React App</h1>
      <button onClick={sendToElectron}>Send to Main</button>
      <button onClick={fetchData}>Fetch Data</button>
      <p>{message}</p>
    </div>
  );
}

export default App;
```

### React Hooks for Electron

**useElectron.js:**
```javascript
import { useState, useEffect, useCallback } from 'react';

export const useElectron = () => {
  const [isElectron] = useState(
    window.electron !== undefined
  );

  const send = useCallback((channel, data) => {
    if (isElectron) {
      window.electron.sendMessage(channel, data);
    }
  }, [isElectron]);

  const invoke = useCallback(async (channel, data) => {
    if (isElectron) {
      return await window.electron.invoke(channel, data);
    }
  }, [isElectron]);

  return { isElectron, send, invoke };
};

export const useElectronListener = (channel, callback) => {
  useEffect(() => {
    if (window.electron) {
      window.electron.receiveMessage(channel, callback);
    }
    
    return () => {
      // Cleanup if needed
    };
  }, [channel, callback]);
};
```

**Usage:**
```javascript
import { useElectron, useElectronListener } from './hooks/useElectron';

function MyComponent() {
  const { isElectron, send, invoke } = useElectron();
  const [data, setData] = useState(null);

  useElectronListener('fromMain', (message) => {
    console.log('Received:', message);
  });

  const handleClick = async () => {
    const result = await invoke('getData', 'param');
    setData(result);
  };

  if (!isElectron) {
    return <div>This app requires Electron</div>;
  }

  return (
    <div>
      <button onClick={handleClick}>Get Data</button>
      {data && <pre>{JSON.stringify(data, null, 2)}</pre>}
    </div>
  );
}
```

### Electron Forge + React Setup

```bash
# Create new app with React template
npx create-electron-app my-app --template=webpack

# Or add to existing app
npm install --save-dev @electron-forge/plugin-webpack
```

**forge.config.js:**
```javascript
module.exports = {
  packagerConfig: {},
  makers: [
    {
      name: '@electron-forge/maker-squirrel',
      config: {}
    },
    {
      name: '@electron-forge/maker-zip',
      platforms: ['darwin']
    },
    {
      name: '@electron-forge/maker-deb',
      config: {}
    }
  ],
  plugins: [
    {
      name: '@electron-forge/plugin-webpack',
      config: {
        mainConfig: './webpack.main.config.js',
        renderer: {
          config: './webpack.renderer.config.js',
          entryPoints: [
            {
              html: './src/index.html',
              js: './src/renderer.js',
              name: 'main_window',
              preload: {
                js: './src/preload.js'
              }
            }
          ]
        }
      }
    }
  ]
};
```

### Electron + Vite + React

```bash
npm create vite@latest my-electron-app -- --template react
cd my-electron-app
npm install
npm install --save-dev electron electron-builder vite-plugin-electron
```

**vite.config.js:**
```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import electron from 'vite-plugin-electron';

export default defineConfig({
  plugins: [
    react(),
    electron([
      {
        entry: 'electron/main.js',
      },
      {
        entry: 'electron/preload.js',
        onstart(options) {
          options.reload();
        }
      }
    ])
  ]
});
```

**package.json:**
```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build && electron-builder",
    "preview": "vite preview"
  }
}
```

### TypeScript Support

**Install dependencies:**
```bash
npm install --save-dev @types/node @types/react @types/react-dom typescript
```

**electron.d.ts:**
```typescript
export interface IElectronAPI {
  sendMessage: (channel: string, data: any) => void;
  receiveMessage: (channel: string, func: (...args: any[]) => void) => void;
  invoke: (channel: string, data?: any) => Promise<any>;
}

declare global {
  interface Window {
    electron: IElectronAPI;
  }
}
```

**Usage in React component:**
```typescript
import React, { useState, useEffect } from 'react';

const App: React.FC = () => {
  const [message, setMessage] = useState<string>('');

  useEffect(() => {
    if (window.electron) {
      window.electron.receiveMessage('fromMain', (data: string) => {
        setMessage(data);
      });
    }
  }, []);

  const handleInvoke = async (): Promise<void> => {
    if (window.electron) {
      const result = await window.electron.invoke('getData');
      console.log(result);
    }
  };

  return (
    <div>
      <h1>Electron + React + TypeScript</h1>
      <button onClick={handleInvoke}>Invoke</button>
      <p>{message}</p>
    </div>
  );
};

export default App;
```

---

## Electron with Next.js

### Using Nextron

**Nextron** is the easiest way to integrate Next.js with Electron.

**1. Create a new Nextron app:**
```bash
npx create-nextron-app my-app
cd my-app
npm install
```

**2. Available templates:**
```bash
# Basic Next.js + Electron
npx create-nextron-app my-app --example basic-lang-javascript

# With TypeScript
npx create-nextron-app my-app --example basic-lang-typescript

# With Tailwind CSS
npx create-nextron-app my-app --example with-tailwindcss

# With TypeScript + Tailwind
npx create-nextron-app my-app --example with-typescript-tailwindcss
```

**3. Project structure:**
```
my-app/
├── main/
│   ├── background.js    # Electron main process
│   └── preload.js       # Preload script
├── renderer/
│   ├── pages/
│   │   ├── _app.js
│   │   ├── index.js
│   │   └── next.js
│   └── public/
├── resources/           # App icons
└── package.json
```

**4. Run the app:**
```bash
npm run dev    # Development mode
npm run build  # Build for production
```

### Manual Setup: Next.js + Electron

**1. Create Next.js app:**
```bash
npx create-next-app my-electron-nextjs-app
cd my-electron-nextjs-app
npm install --save-dev electron electron-builder electron-is-dev
```

**2. Create electron directory:**
```
my-app/
├── electron/
│   ├── main.js
│   └── preload.js
├── pages/
├── public/
└── package.json
```

**3. electron/main.js:**
```javascript
const { app, BrowserWindow } = require('electron');
const path = require('path');
const isDev = require('electron-is-dev');

let mainWindow;

function createWindow() {
  mainWindow = new BrowserWindow({
    width: 1200,
    height: 800,
    webPreferences: {
      nodeIntegration: false,
      contextIsolation: true,
      preload: path.join(__dirname, 'preload.js')
    }
  });

  const url = isDev
    ? 'http://localhost:3000'
    : `file://${path.join(__dirname, '../out/index.html')}`;

  mainWindow.loadURL(url);

  if (isDev) {
    mainWindow.webContents.openDevTools();
  }

  mainWindow.on('closed', () => {
    mainWindow = null;
  });
}

app.on('ready', createWindow);

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', () => {
  if (mainWindow === null) {
    createWindow();
  }
});
```

**4. electron/preload.js:**
```javascript
const { contextBridge, ipcRenderer } = require('electron');

contextBridge.exposeInMainWorld('electron', {
  platform: process.platform,
  sendMessage: (channel, data) => {
    ipcRenderer.send(channel, data);
  },
  receiveMessage: (channel, func) => {
    ipcRenderer.on(channel, (event, ...args) => func(...args));
  },
  invoke: (channel, data) => {
    return ipcRenderer.invoke(channel, data);
  }
});
```

**5. Update package.json:**
```json
{
  "name": "my-electron-nextjs-app",
  "version": "1.0.0",
  "main": "electron/main.js",
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "export": "next export",
    "start": "next start",
    "electron": "electron .",
    "electron:dev": "concurrently \"npm run dev\" \"wait-on http://localhost:3000 && electron .\"",
    "electron:build": "npm run build && npm run export && electron-builder",
    "dist": "npm run build && npm run export && electron-builder"
  },
  "build": {
    "appId": "com.example.nextron",
    "files": [
      "electron/**/*",
      "out/**/*"
    ],
    "directories": {
      "buildResources": "resources",
      "output": "dist"
    }
  }
}
```

**6. next.config.js (for static export):**
```javascript
module.exports = {
  output: 'export',
  images: {
    unoptimized: true,
  },
  // Optional: Change the output directory
  distDir: 'out',
};
```

### Using Next.js API Routes with Electron

Since Electron runs static files, API routes need special handling.

**Option 1: Use IPC instead of API routes**

**electron/main.js:**
```javascript
const { ipcMain } = require('electron');

// Handle API calls via IPC
ipcMain.handle('api:users', async (event, userId) => {
  // Your API logic here
  const user = await fetchUserFromDatabase(userId);
  return user;
});

ipcMain.handle('api:posts', async () => {
  const posts = await fetchPosts();
  return posts;
});
```

**pages/index.js:**
```javascript
import { useState, useEffect } from 'react';

export default function Home() {
  const [user, setUser] = useState(null);

  useEffect(() => {
    async function fetchUser() {
      if (window.electron) {
        const userData = await window.electron.invoke('api:users', 1);
        setUser(userData);
      }
    }
    fetchUser();
  }, []);

  return (
    <div>
      <h1>Next.js + Electron</h1>
      {user && <p>Welcome, {user.name}!</p>}
    </div>
  );
}
```

**Option 2: Run Next.js server in Electron**

```javascript
const { app, BrowserWindow } = require('electron');
const next = require('next');
const path = require('path');

const dev = process.env.NODE_ENV !== 'production';
const nextApp = next({ dev, dir: path.join(__dirname, '../') });
const handle = nextApp.getRequestHandler();

let mainWindow;

nextApp.prepare().then(() => {
  // Start Next.js server
  const express = require('express');
  const server = express();
  
  server.all('*', (req, res) => {
    return handle(req, res);
  });

  const PORT = 3000;
  server.listen(PORT, (err) => {
    if (err) throw err;
    console.log(`> Ready on http://localhost:${PORT}`);
    
    createWindow();
  });
});

function createWindow() {
  mainWindow = new BrowserWindow({
    width: 1200,
    height: 800,
    webPreferences: {
      nodeIntegration: false,
      contextIsolation: true,
      preload: path.join(__dirname, 'preload.js')
    }
  });

  mainWindow.loadURL('http://localhost:3000');
}
```

### Next.js Pages with Electron Features

**pages/_app.js:**
```javascript
import { useEffect } from 'react';
import '../styles/globals.css';

function MyApp({ Component, pageProps }) {
  useEffect(() => {
    // Check if running in Electron
    if (typeof window !== 'undefined' && window.electron) {
      console.log('Running in Electron');
      console.log('Platform:', window.electron.platform);
    }
  }, []);

  return <Component {...pageProps} />;
}

export default MyApp;
```

**pages/index.js:**
```javascript
import { useState, useEffect } from 'react';
import Head from 'next/head';
import styles from '../styles/Home.module.css';

export default function Home() {
  const [isElectron, setIsElectron] = useState(false);
  const [message, setMessage] = useState('');

  useEffect(() => {
    setIsElectron(typeof window !== 'undefined' && window.electron);

    if (window.electron) {
      window.electron.receiveMessage('update', (msg) => {
        setMessage(msg);
      });
    }
  }, []);

  const handleClick = async () => {
    if (window.electron) {
      const result = await window.electron.invoke('get-data', 'test');
      alert(result);
    }
  };

  return (
    <div className={styles.container}>
      <Head>
        <title>Electron + Next.js App</title>
      </Head>

      <main className={styles.main}>
        <h1>Electron + Next.js</h1>
        {isElectron ? (
          <>
            <p>Running in Electron ✅</p>
            <button onClick={handleClick}>Call Electron API</button>
            {message && <p>Message: {message}</p>}
          </>
        ) : (
          <p>Running in Browser 🌐</p>
        )}
      </main>
    </div>
  );
}
```

### TypeScript Setup

**Install dependencies:**
```bash
npm install --save-dev @types/node typescript
```

**electron/main.ts:**
```typescript
import { app, BrowserWindow } from 'electron';
import * as path from 'path';
import * as isDev from 'electron-is-dev';

let mainWindow: BrowserWindow | null;

function createWindow(): void {
  mainWindow = new BrowserWindow({
    width: 1200,
    height: 800,
    webPreferences: {
      nodeIntegration: false,
      contextIsolation: true,
      preload: path.join(__dirname, 'preload.js')
    }
  });

  const url = isDev
    ? 'http://localhost:3000'
    : `file://${path.join(__dirname, '../out/index.html')}`;

  mainWindow.loadURL(url);

  mainWindow.on('closed', () => {
    mainWindow = null;
  });
}

app.on('ready', createWindow);

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', () => {
  if (mainWindow === null) {
    createWindow();
  }
});
```

**types/electron.d.ts:**
```typescript
export interface IElectronAPI {
  platform: string;
  sendMessage: (channel: string, data: any) => void;
  receiveMessage: (channel: string, func: (...args: any[]) => void) => void;
  invoke: (channel: string, data?: any) => Promise<any>;
}

declare global {
  interface Window {
    electron: IElectronAPI;
  }
}
```

### Nextron with Advanced Features

**main/background.ts:**
```typescript
import { app, ipcMain } from 'electron';
import serve from 'electron-serve';
import { createWindow } from './helpers';

const isProd = process.env.NODE_ENV === 'production';

if (isProd) {
  serve({ directory: 'app' });
} else {
  app.setPath('userData', `${app.getPath('userData')} (development)`);
}

(async () => {
  await app.whenReady();

  const mainWindow = createWindow('main', {
    width: 1000,
    height: 600,
  });

  if (isProd) {
    await mainWindow.loadURL('app://./home.html');
  } else {
    const port = process.argv[2];
    await mainWindow.loadURL(`http://localhost:${port}/home`);
    mainWindow.webContents.openDevTools();
  }
})();

app.on('window-all-closed', () => {
  app.quit();
});

// IPC Handlers
ipcMain.handle('get-app-version', () => {
  return app.getVersion();
});

ipcMain.handle('get-user-data-path', () => {
  return app.getPath('userData');
});
```

**renderer/pages/home.tsx:**
```typescript
import React from 'react';
import { useEffect, useState } from 'react';
import Head from 'next/head';

export default function HomePage() {
  const [version, setVersion] = useState('');

  useEffect(() => {
    async function getVersion() {
      if (window.electron) {
        const ver = await window.electron.invoke('get-app-version');
        setVersion(ver);
      }
    }
    getVersion();
  }, []);

  return (
    <>
      <Head>
        <title>Home - Nextron</title>
      </Head>
      <div>
        <h1>Hello Nextron! ⚡️</h1>
        <p>App Version: {version}</p>
      </div>
    </>
  );
}
```

### Best Practices for Next.js + Electron

1. **Static Export**: Always use `output: 'export'` in next.config.js for production builds
2. **Image Optimization**: Disable Next.js image optimization for Electron (`unoptimized: true`)
3. **API Routes**: Use IPC instead of Next.js API routes in production
4. **Environment Variables**: Be careful with env vars - use different configs for dev/prod
5. **Build Output**: Export to `out` directory and include it in electron-builder files
6. **Development**: Use concurrently to run Next.js dev server and Electron together
7. **Routing**: Use Next.js router normally; it works with static export
8. **State Management**: Redux, Zustand, or Context API work normally
9. **SSG**: Use `getStaticProps` for data fetching at build time

---

## Common Patterns & Recipes

### Single Instance Lock
```javascript
const gotTheLock = app.requestSingleInstanceLock();

if (!gotTheLock) {
  app.quit();
} else {
  app.on('second-instance', (event, commandLine, workingDirectory) => {
    // Focus the existing window
    if (mainWindow) {
      if (mainWindow.isMinimized()) mainWindow.restore();
      mainWindow.focus();
    }
  });
  
  app.whenReady().then(createWindow);
}
```

### Splash Screen
```javascript
let splashWindow;
let mainWindow;

function createSplashScreen() {
  splashWindow = new BrowserWindow({
    width: 400,
    height: 300,
    transparent: true,
    frame: false,
    alwaysOnTop: true
  });
  
  splashWindow.loadFile('splash.html');
  splashWindow.center();
}

function createMainWindow() {
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    show: false
  });
  
  mainWindow.loadFile('index.html');
  
  mainWindow.once('ready-to-show', () => {
    setTimeout(() => {
      splashWindow.close();
      mainWindow.show();
    }, 2000);
  });
}

app.whenReady().then(() => {
  createSplashScreen();
  createMainWindow();
});
```

### Custom Title Bar
```javascript
// Main process
const win = new BrowserWindow({
  frame: false,
  titleBarStyle: 'hidden'
});

// Renderer HTML
<div id="titlebar">
  <div id="drag-region"></div>
  <div id="window-controls">
    <button id="minimize">−</button>
    <button id="maximize">□</button>
    <button id="close">×</button>
  </div>
</div>

// Renderer CSS
#titlebar {
  -webkit-app-region: drag;
  height: 32px;
}

#window-controls {
  -webkit-app-region: no-drag;
}

// Renderer JS
document.getElementById('minimize').addEventListener('click', () => {
  window.api.minimize();
});

document.getElementById('maximize').addEventListener('click', () => {
  window.api.maximize();
});

document.getElementById('close').addEventListener('click', () => {
  window.api.close();
});
```

---

## Resources

### Official Documentation
- [Electron Docs](https://www.electronjs.org/docs)
- [Electron API](https://www.electronjs.org/docs/api)
- [Electron Fiddle](https://www.electronjs.org/fiddle)

### Popular Frameworks
- **Electron Forge**: Complete toolkit for building Electron apps
- **Electron Builder**: Build and publish Electron apps
- **Electron React Boilerplate**: Electron + React + Redux + Webpack
- **Electron Vue**: Electron + Vue.js
- **Nextron**: Electron + Next.js integration

### Essential Packages
- `electron-store`: Persistent data storage
- `electron-updater`: Auto-update functionality
- `electron-log`: Logging library
- `electron-reload`: Auto-reload during development
- `electron-context-menu`: Easily create context menus
- `electron-window-state`: Save and restore window state

### Testing
- **Spectron**: End-to-end testing framework (deprecated)
- **Playwright**: Modern E2E testing for Electron
- **Jest**: Unit testing framework

---

## Quick Reference

### Lifecycle Events
```javascript
app.on('ready', () => {});
app.on('window-all-closed', () => {});
app.on('activate', () => {});
app.on('before-quit', () => {});
app.on('will-quit', () => {});
app.on('quit', () => {});
```

### Common Window Properties
```javascript
width, height, x, y, minWidth, minHeight, maxWidth, maxHeight
resizable, movable, minimizable, maximizable, closable
fullscreen, fullscreenable, kiosk, alwaysOnTop
frame, transparent, backgroundColor, show, modal
```

### Common IPC Channels Pattern
```javascript
// File operations
'file:open', 'file:save', 'file:read', 'file:write'

// Window operations
'window:minimize', 'window:maximize', 'window:close'

// App operations
'app:quit', 'app:relaunch', 'app:getVersion'

// Data operations
'data:get', 'data:set', 'data:delete'
```

---

*This cheat sheet covers ElectronJs from beginner to advanced levels. Practice building real applications to master these concepts!*
