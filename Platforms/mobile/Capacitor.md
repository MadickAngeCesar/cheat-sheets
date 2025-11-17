# Capacitor Cheat Sheet - Beginner to Pro

## Table of Contents
- [Setup & Installation](#setup--installation)
- [Project Configuration](#project-configuration)
- [Core Plugins](#core-plugins)
- [Camera](#camera)
- [Geolocation](#geolocation)
- [Storage](#storage)
- [Filesystem](#filesystem)
- [Network](#network)
- [Device Info](#device-info)
- [App & Splash Screen](#app--splash-screen)
- [Status Bar](#status-bar)
- [Keyboard](#keyboard)
- [Share & Browser](#share--browser)
- [Push Notifications](#push-notifications)
- [Custom Plugins](#custom-plugins)
- [Platform Specific](#platform-specific)
- [Build & Deploy](#build--deploy)
- [Best Practices](#best-practices)

---

## Setup & Installation

### Install Capacitor
```bash
# Create new project
npm create vite@latest my-app
cd my-app
npm install

# Install Capacitor
npm install @capacitor/core @capacitor/cli

# Initialize Capacitor
npx cap init
# App name: My App
# Package ID: com.example.app

# Add platforms
npm install @capacitor/android @capacitor/ios
npx cap add android
npx cap add ios
```

### With Existing Project
```bash
# Install Capacitor
npm install @capacitor/core @capacitor/cli

# Initialize
npx cap init [appName] [appId]

# Add platforms
npx cap add android
npx cap add ios
```

---

## Project Configuration

### capacitor.config.ts
```typescript
import { CapacitorConfig } from '@capacitor/cli';

const config: CapacitorConfig = {
  appId: 'com.example.app',
  appName: 'My App',
  webDir: 'dist',
  bundledWebRuntime: false,
  server: {
    // For development
    url: 'http://192.168.1.100:3000',
    cleartext: true,
  },
  plugins: {
    SplashScreen: {
      launchShowDuration: 3000,
      backgroundColor: '#ffffff',
      showSpinner: false,
    },
    Camera: {
      saveToGallery: true,
    },
  },
  android: {
    buildOptions: {
      keystorePath: 'release.keystore',
      keystoreAlias: 'key0',
    },
  },
  ios: {
    scheme: 'App',
  },
};

export default config;
```

### Common Commands
```bash
# Build web assets
npm run build

# Copy web assets to native projects
npx cap copy

# Update native dependencies
npx cap update

# Sync (copy + update)
npx cap sync

# Open native IDE
npx cap open android
npx cap open ios

# Run on device
npx cap run android
npx cap run ios

# Check configuration
npx cap doctor
```

---

## Core Plugins

### Installation
```bash
npm install @capacitor/camera @capacitor/geolocation @capacitor/storage
```

---

## Camera

### Installation
```bash
npm install @capacitor/camera
npx cap sync
```

### Usage
```typescript
import { Camera, CameraResultType, CameraSource } from '@capacitor/camera';

// Take photo
const takePicture = async () => {
  try {
    const image = await Camera.getPhoto({
      quality: 90,
      allowEditing: true,
      resultType: CameraResultType.Uri,
      source: CameraSource.Camera,
    });
    
    // image.webPath will contain a path that can be set as an image src
    console.log('Image URI:', image.webPath);
    return image.webPath;
  } catch (error) {
    console.error('Error taking photo:', error);
  }
};

// Pick from gallery
const pickImage = async () => {
  try {
    const image = await Camera.getPhoto({
      quality: 90,
      allowEditing: false,
      resultType: CameraResultType.DataUrl,
      source: CameraSource.Photos,
    });
    
    return image.dataUrl;
  } catch (error) {
    console.error('Error picking image:', error);
  }
};

// Get multiple photos
const pickImages = async () => {
  const images = await Camera.pickImages({
    quality: 90,
    limit: 5,
  });
  
  return images.photos;
};
```

### Permissions (Android)
```xml
<!-- android/app/src/main/AndroidManifest.xml -->
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### Permissions (iOS)
```xml
<!-- ios/App/App/Info.plist -->
<key>NSCameraUsageDescription</key>
<string>We need camera access to take photos</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>We need photo library access</string>
```

---

## Geolocation

### Installation
```bash
npm install @capacitor/geolocation
npx cap sync
```

### Usage
```typescript
import { Geolocation } from '@capacitor/geolocation';

// Get current position
const getCurrentPosition = async () => {
  try {
    const coordinates = await Geolocation.getCurrentPosition();
    console.log('Latitude:', coordinates.coords.latitude);
    console.log('Longitude:', coordinates.coords.longitude);
    return coordinates;
  } catch (error) {
    console.error('Error getting location:', error);
  }
};

// Watch position
const watchPosition = () => {
  const watchId = Geolocation.watchPosition({}, (position, err) => {
    if (err) {
      console.error('Error watching position:', err);
      return;
    }
    console.log('New position:', position);
  });
  
  return watchId;
};

// Clear watch
const clearWatch = async (watchId: string) => {
  await Geolocation.clearWatch({ id: watchId });
};

// Check permissions
const checkPermissions = async () => {
  const permission = await Geolocation.checkPermissions();
  console.log('Permission:', permission.location);
};

// Request permissions
const requestPermissions = async () => {
  const permission = await Geolocation.requestPermissions();
  console.log('Permission:', permission.location);
};
```

### Permissions (Android)
```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

### Permissions (iOS)
```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>We need location access to show your position</string>
<key>NSLocationAlwaysUsageDescription</key>
<string>We need location access in background</string>
```

---

## Storage

### Installation
```bash
npm install @capacitor/preferences
npx cap sync
```

### Usage
```typescript
import { Preferences } from '@capacitor/preferences';

// Set value
const setName = async () => {
  await Preferences.set({
    key: 'name',
    value: 'John Doe',
  });
};

// Get value
const getName = async () => {
  const { value } = await Preferences.get({ key: 'name' });
  console.log('Name:', value);
  return value;
};

// Remove value
const removeName = async () => {
  await Preferences.remove({ key: 'name' });
};

// Clear all
const clearStorage = async () => {
  await Preferences.clear();
};

// Get all keys
const getAllKeys = async () => {
  const { keys } = await Preferences.keys();
  console.log('Keys:', keys);
  return keys;
};

// Store object
const setObject = async () => {
  await Preferences.set({
    key: 'user',
    value: JSON.stringify({
      name: 'John',
      email: 'john@example.com',
    }),
  });
};

// Get object
const getObject = async () => {
  const { value } = await Preferences.get({ key: 'user' });
  const user = value ? JSON.parse(value) : null;
  return user;
};
```

---

## Filesystem

### Installation
```bash
npm install @capacitor/filesystem
npx cap sync
```

### Usage
```typescript
import { Filesystem, Directory, Encoding } from '@capacitor/filesystem';

// Write file
const writeFile = async () => {
  await Filesystem.writeFile({
    path: 'secrets/text.txt',
    data: 'This is a test file',
    directory: Directory.Documents,
    encoding: Encoding.UTF8,
  });
};

// Read file
const readFile = async () => {
  const contents = await Filesystem.readFile({
    path: 'secrets/text.txt',
    directory: Directory.Documents,
    encoding: Encoding.UTF8,
  });
  console.log('File contents:', contents.data);
  return contents.data;
};

// Delete file
const deleteFile = async () => {
  await Filesystem.deleteFile({
    path: 'secrets/text.txt',
    directory: Directory.Documents,
  });
};

// Create directory
const createDirectory = async () => {
  await Filesystem.mkdir({
    path: 'secrets',
    directory: Directory.Documents,
    recursive: true,
  });
};

// List directory
const listDirectory = async () => {
  const contents = await Filesystem.readdir({
    path: 'secrets',
    directory: Directory.Documents,
  });
  console.log('Directory contents:', contents.files);
  return contents.files;
};

// Get URI
const getUri = async () => {
  const result = await Filesystem.getUri({
    path: 'secrets/text.txt',
    directory: Directory.Documents,
  });
  console.log('URI:', result.uri);
  return result.uri;
};
```

---

## Network

### Installation
```bash
npm install @capacitor/network
npx cap sync
```

### Usage
```typescript
import { Network } from '@capacitor/network';

// Get current status
const getNetworkStatus = async () => {
  const status = await Network.getStatus();
  console.log('Network status:', status);
  // status.connected - boolean
  // status.connectionType - 'wifi' | 'cellular' | 'none' | 'unknown'
  return status;
};

// Listen to network changes
const listenToNetworkChanges = () => {
  Network.addListener('networkStatusChange', status => {
    console.log('Network status changed:', status);
  });
};

// Remove listeners
const removeListeners = () => {
  Network.removeAllListeners();
};
```

---

## Device Info

### Installation
```bash
npm install @capacitor/device
npx cap sync
```

### Usage
```typescript
import { Device } from '@capacitor/device';

// Get device info
const getDeviceInfo = async () => {
  const info = await Device.getInfo();
  console.log('Device info:', info);
  // info.platform - 'ios' | 'android' | 'web'
  // info.model
  // info.operatingSystem
  // info.osVersion
  // info.manufacturer
  return info;
};

// Get device ID
const getDeviceId = async () => {
  const id = await Device.getId();
  console.log('Device ID:', id.identifier);
  return id.identifier;
};

// Get battery info
const getBatteryInfo = async () => {
  const info = await Device.getBatteryInfo();
  console.log('Battery level:', info.batteryLevel);
  console.log('Is charging:', info.isCharging);
  return info;
};

// Get language code
const getLanguageCode = async () => {
  const lang = await Device.getLanguageCode();
  console.log('Language:', lang.value);
  return lang.value;
};
```

---

## App & Splash Screen

### Installation
```bash
npm install @capacitor/app @capacitor/splash-screen
npx cap sync
```

### App
```typescript
import { App } from '@capacitor/app';

// Get app info
const getAppInfo = async () => {
  const info = await App.getInfo();
  console.log('App info:', info);
  return info;
};

// Get app state
const getAppState = async () => {
  const state = await App.getState();
  console.log('Is active:', state.isActive);
  return state;
};

// Listen to app state changes
App.addListener('appStateChange', state => {
  console.log('App state changed:', state.isActive);
});

// Listen to back button (Android)
App.addListener('backButton', () => {
  console.log('Back button pressed');
});

// Exit app
const exitApp = () => {
  App.exitApp();
};
```

### Splash Screen
```typescript
import { SplashScreen } from '@capacitor/splash-screen';

// Show splash screen
const showSplash = async () => {
  await SplashScreen.show({
    showDuration: 2000,
    autoHide: true,
  });
};

// Hide splash screen
const hideSplash = async () => {
  await SplashScreen.hide();
};
```

---

## Status Bar

### Installation
```bash
npm install @capacitor/status-bar
npx cap sync
```

### Usage
```typescript
import { StatusBar, Style } from '@capacitor/status-bar';

// Set style
const setDark = async () => {
  await StatusBar.setStyle({ style: Style.Dark });
};

const setLight = async () => {
  await StatusBar.setStyle({ style: Style.Light });
};

// Set background color (Android only)
const setBackgroundColor = async () => {
  await StatusBar.setBackgroundColor({ color: '#FF0000' });
};

// Show/hide status bar
const showStatusBar = async () => {
  await StatusBar.show();
};

const hideStatusBar = async () => {
  await StatusBar.hide();
};

// Get info
const getInfo = async () => {
  const info = await StatusBar.getInfo();
  console.log('Status bar visible:', info.visible);
  console.log('Status bar style:', info.style);
  return info;
};
```

---

## Keyboard

### Installation
```bash
npm install @capacitor/keyboard
npx cap sync
```

### Usage
```typescript
import { Keyboard } from '@capacitor/keyboard';

// Show keyboard
const showKeyboard = async () => {
  await Keyboard.show();
};

// Hide keyboard
const hideKeyboard = async () => {
  await Keyboard.hide();
};

// Listen to keyboard events
Keyboard.addListener('keyboardWillShow', info => {
  console.log('Keyboard height:', info.keyboardHeight);
});

Keyboard.addListener('keyboardWillHide', () => {
  console.log('Keyboard will hide');
});

// Set accessory bar visibility (iOS)
const setAccessoryBar = async () => {
  await Keyboard.setAccessoryBarVisible({ isVisible: true });
};
```

---

## Share & Browser

### Installation
```bash
npm install @capacitor/share @capacitor/browser
npx cap sync
```

### Share
```typescript
import { Share } from '@capacitor/share';

// Share content
const shareContent = async () => {
  await Share.share({
    title: 'Check this out!',
    text: 'Really awesome thing',
    url: 'https://example.com',
    dialogTitle: 'Share with friends',
  });
};

// Check if sharing is available
const canShare = async () => {
  const result = await Share.canShare();
  console.log('Can share:', result.value);
  return result.value;
};
```

### Browser
```typescript
import { Browser } from '@capacitor/browser';

// Open URL
const openBrowser = async () => {
  await Browser.open({
    url: 'https://example.com',
    presentationStyle: 'popover',
    toolbarColor: '#FF0000',
  });
};

// Close browser
const closeBrowser = async () => {
  await Browser.close();
};

// Listen to browser events
Browser.addListener('browserFinished', () => {
  console.log('Browser closed');
});
```

---

## Push Notifications

### Installation
```bash
npm install @capacitor/push-notifications
npx cap sync
```

### Usage
```typescript
import { PushNotifications } from '@capacitor/push-notifications';

// Request permissions
const requestPermissions = async () => {
  const result = await PushNotifications.requestPermissions();
  if (result.receive === 'granted') {
    await PushNotifications.register();
  }
};

// Listen for registration
PushNotifications.addListener('registration', token => {
  console.log('Push registration success, token:', token.value);
});

// Listen for registration errors
PushNotifications.addListener('registrationError', error => {
  console.error('Error on registration:', error);
});

// Listen for push notifications
PushNotifications.addListener('pushNotificationReceived', notification => {
  console.log('Push notification received:', notification);
});

// Listen for notification actions
PushNotifications.addListener('pushNotificationActionPerformed', notification => {
  console.log('Push notification action performed:', notification);
});
```

---

## Custom Plugins

### Create Plugin
```bash
npm init @capacitor/plugin
```

### Plugin Example
```typescript
// src/definitions.ts
export interface MyPluginPlugin {
  echo(options: { value: string }): Promise<{ value: string }>;
}

// src/web.ts
import { WebPlugin } from '@capacitor/core';
import type { MyPluginPlugin } from './definitions';

export class MyPluginWeb extends WebPlugin implements MyPluginPlugin {
  async echo(options: { value: string }): Promise<{ value: string }> {
    console.log('ECHO', options);
    return options;
  }
}

// src/index.ts
import { registerPlugin } from '@capacitor/core';
import type { MyPluginPlugin } from './definitions';

const MyPlugin = registerPlugin<MyPluginPlugin>('MyPlugin', {
  web: () => import('./web').then(m => new m.MyPluginWeb()),
});

export * from './definitions';
export { MyPlugin };
```

---

## Platform Specific

### Check Platform
```typescript
import { Capacitor } from '@capacitor/core';

const platform = Capacitor.getPlatform();
// 'ios' | 'android' | 'web'

if (Capacitor.isNativePlatform()) {
  // Native iOS or Android
}

if (platform === 'ios') {
  // iOS specific code
} else if (platform === 'android') {
  // Android specific code
}
```

---

## Build & Deploy

### Android
```bash
# Build web assets
npm run build

# Sync with native project
npx cap sync android

# Open Android Studio
npx cap open android

# Build APK
cd android
./gradlew assembleRelease

# Build AAB for Play Store
./gradlew bundleRelease
```

### iOS
```bash
# Build web assets
npm run build

# Sync with native project
npx cap sync ios

# Open Xcode
npx cap open ios

# Build from Xcode or use Fastlane
```

---

## Best Practices

### Configuration
```typescript
// ✅ Good: Use environment-specific configs
const config: CapacitorConfig = {
  appId: process.env.APP_ID,
  appName: process.env.APP_NAME,
  webDir: 'dist',
  server: {
    url: process.env.DEV_SERVER_URL,
    cleartext: process.env.NODE_ENV === 'development',
  },
};
```

### Error Handling
```typescript
// ✅ Good: Handle errors properly
const takePicture = async () => {
  try {
    const image = await Camera.getPhoto({
      quality: 90,
      resultType: CameraResultType.Uri,
    });
    return image.webPath;
  } catch (error) {
    if (error.message.includes('User cancelled')) {
      console.log('User cancelled photo capture');
    } else {
      console.error('Error taking photo:', error);
    }
    return null;
  }
};
```

### Permissions
```typescript
// ✅ Good: Check permissions before using features
const requestCameraPermissions = async () => {
  const permissions = await Camera.checkPermissions();
  
  if (permissions.camera !== 'granted') {
    const result = await Camera.requestPermissions();
    return result.camera === 'granted';
  }
  
  return true;
};
```

### Performance
```typescript
// ✅ Good: Remove listeners when not needed
useEffect(() => {
  const handler = Network.addListener('networkStatusChange', status => {
    console.log('Network changed:', status);
  });
  
  return () => {
    handler.remove();
  };
}, []);
```

---

**Pro Tip:** Always sync after installing plugins with `npx cap sync`, use TypeScript for better type safety, properly handle permissions before accessing device features, test on real devices for accurate results, and keep web assets and native projects in sync!