# Node.js Cheat Sheet - Beginner to Pro

## Table of Contents
- [Introduction](#introduction)
- [Setup & Installation](#setup--installation)
- [Modules](#modules)
- [NPM & Package Management](#npm--package-management)
- [File System](#file-system)
- [Path Module](#path-module)
- [HTTP Module](#http-module)
- [Events](#events)
- [Streams](#streams)
- [Buffer](#buffer)
- [Process & Child Process](#process--child-process)
- [OS Module](#os-module)
- [Crypto](#crypto)
- [Async Patterns](#async-patterns)
- [Error Handling](#error-handling)
- [Debugging](#debugging)
- [Environment Variables](#environment-variables)
- [Best Practices](#best-practices)

---

## Introduction

### What is Node.js?
```javascript
// Node.js is a JavaScript runtime built on Chrome's V8 engine
// - Event-driven, non-blocking I/O
// - Single-threaded with event loop
// - Perfect for I/O-intensive applications
// - Not ideal for CPU-intensive tasks
```

### Node.js Architecture
```javascript
// Event Loop Phases:
// 1. Timers (setTimeout, setInterval)
// 2. Pending callbacks
// 3. Idle, prepare
// 4. Poll (I/O events)
// 5. Check (setImmediate)
// 6. Close callbacks
```

---

## Setup & Installation

### Installation
```bash
# Download from nodejs.org

# Check version
node --version
node -v

# Check npm version
npm --version
npm -v

# Run REPL (Read-Eval-Print Loop)
node

# Exit REPL
.exit
# or Ctrl+C twice
```

### Running Node Files
```bash
# Execute a file
node app.js
node index.js

# With arguments
node app.js arg1 arg2

# Watch mode (Node 18.11+)
node --watch app.js
```

---

## Modules

### CommonJS (Default)
```javascript
// Exporting
// math.js
function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

module.exports = { add, subtract };

// Or export individually
exports.add = add;
exports.subtract = subtract;

// Importing
// app.js
const math = require('./math');
const { add, subtract } = require('./math');

console.log(add(5, 3)); // 8
```

### ES Modules
```javascript
// package.json
{
  "type": "module"
}

// Exporting
// math.mjs or math.js (with "type": "module")
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

export default function multiply(a, b) {
  return a * b;
}

// Importing
import multiply, { add, subtract } from './math.js';
import * as math from './math.js';
```

### Built-in Modules
```javascript
// Core modules (no installation needed)
const fs = require('fs');
const path = require('path');
const http = require('http');
const https = require('https');
const url = require('url');
const querystring = require('querystring');
const os = require('os');
const events = require('events');
const stream = require('stream');
const util = require('util');
const crypto = require('crypto');
const child_process = require('child_process');
```

### Module Caching
```javascript
// Modules are cached after first load
require('./myModule'); // Loads and caches
require('./myModule'); // Returns cached version

// Clear cache (rarely needed)
delete require.cache[require.resolve('./myModule')];
```

---

## NPM & Package Management

### Package.json
```bash
# Initialize package.json
npm init
npm init -y  # Skip questions

# Package.json example
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "My application",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.0"
  },
  "devDependencies": {
    "nodemon": "^2.0.0"
  }
}
```

### Installing Packages
```bash
# Install package
npm install express
npm i express

# Install as dev dependency
npm install --save-dev nodemon
npm i -D nodemon

# Install globally
npm install -g nodemon

# Install specific version
npm install express@4.17.1

# Install all dependencies
npm install

# Install from package-lock.json
npm ci
```

### Managing Packages
```bash
# Uninstall package
npm uninstall express
npm rm express

# Update packages
npm update
npm update express

# List installed packages
npm list
npm list --depth=0

# Check outdated packages
npm outdated

# View package info
npm info express
npm view express
```

### NPM Scripts
```javascript
// package.json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "test": "jest",
    "build": "webpack",
    "lint": "eslint .",
    "format": "prettier --write ."
  }
}

// Run scripts
// npm run <script-name>
// npm start (shortcut for npm run start)
// npm test (shortcut for npm run test)
```

### Package Versions
```javascript
// Semantic versioning: MAJOR.MINOR.PATCH
// "express": "^4.18.0" - Compatible with 4.x.x
// "express": "~4.18.0" - Compatible with 4.18.x
// "express": "4.18.0"  - Exact version
// "express": "*"       - Any version (not recommended)
```

---

## File System

### Reading Files
```javascript
const fs = require('fs');

// Synchronous
try {
  const data = fs.readFileSync('file.txt', 'utf8');
  console.log(data);
} catch (err) {
  console.error(err);
}

// Asynchronous (callback)
fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(data);
});

// Promise-based
const fsPromises = require('fs').promises;

async function readFile() {
  try {
    const data = await fsPromises.readFile('file.txt', 'utf8');
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}
```

### Writing Files
```javascript
// Synchronous
fs.writeFileSync('file.txt', 'Hello World');

// Asynchronous
fs.writeFile('file.txt', 'Hello World', (err) => {
  if (err) console.error(err);
});

// Promise-based
await fsPromises.writeFile('file.txt', 'Hello World');

// Append to file
fs.appendFileSync('file.txt', '\nNew line');
await fsPromises.appendFile('file.txt', '\nNew line');
```

### File Operations
```javascript
// Check if file exists
fs.existsSync('file.txt');

// Get file stats
const stats = fs.statSync('file.txt');
stats.isFile();
stats.isDirectory();
stats.size;
stats.mtime; // Modified time

// Rename/Move file
fs.renameSync('old.txt', 'new.txt');
await fsPromises.rename('old.txt', 'new.txt');

// Delete file
fs.unlinkSync('file.txt');
await fsPromises.unlink('file.txt');

// Copy file
fs.copyFileSync('source.txt', 'dest.txt');
await fsPromises.copyFile('source.txt', 'dest.txt');
```

### Directory Operations
```javascript
// Create directory
fs.mkdirSync('new-folder');
fs.mkdirSync('nested/folder', { recursive: true });

// Read directory
const files = fs.readdirSync('folder');
const filesWithTypes = fs.readdirSync('folder', { withFileTypes: true });

filesWithTypes.forEach(file => {
  console.log(file.name, file.isDirectory());
});

// Remove directory
fs.rmdirSync('folder');
fs.rmSync('folder', { recursive: true, force: true }); // Node 14.14+

// Watch directory
fs.watch('folder', (eventType, filename) => {
  console.log(`${filename} ${eventType}`);
});
```

### Advanced File Operations
```javascript
// Read file in chunks
const readable = fs.createReadStream('large-file.txt', { encoding: 'utf8' });
readable.on('data', (chunk) => {
  console.log(chunk);
});

// Write file in chunks
const writable = fs.createWriteStream('output.txt');
writable.write('First line\n');
writable.write('Second line\n');
writable.end();

// Pipe streams
fs.createReadStream('input.txt')
  .pipe(fs.createWriteStream('output.txt'));
```

---

## Path Module

### Basic Path Operations
```javascript
const path = require('path');

// Join paths
path.join('/users', 'john', 'docs', 'file.txt');
// /users/john/docs/file.txt

// Resolve absolute path
path.resolve('docs', 'file.txt');
// /current/working/directory/docs/file.txt

// Get directory name
path.dirname('/users/john/file.txt'); // /users/john

// Get file name
path.basename('/users/john/file.txt'); // file.txt
path.basename('/users/john/file.txt', '.txt'); // file

// Get extension
path.extname('file.txt'); // .txt

// Parse path
path.parse('/users/john/file.txt');
// {
//   root: '/',
//   dir: '/users/john',
//   base: 'file.txt',
//   ext: '.txt',
//   name: 'file'
// }

// Format path
path.format({
  root: '/',
  dir: '/users/john',
  base: 'file.txt'
}); // /users/john/file.txt

// Normalize path
path.normalize('/users/john/../jane/file.txt');
// /users/jane/file.txt

// Relative path
path.relative('/users/john', '/users/jane');
// ../jane

// Check if absolute
path.isAbsolute('/users/john'); // true
path.isAbsolute('docs/file.txt'); // false

// Path separator
path.sep; // / on Unix, \ on Windows
path.delimiter; // : on Unix, ; on Windows
```

---

## HTTP Module

### Basic HTTP Server
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

### Handling Routes
```javascript
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
  const parsedUrl = url.parse(req.url, true);
  const pathname = parsedUrl.pathname;
  
  if (pathname === '/') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end('<h1>Home Page</h1>');
  } else if (pathname === '/about') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end('<h1>About Page</h1>');
  } else {
    res.writeHead(404, { 'Content-Type': 'text/html' });
    res.end('<h1>404 Not Found</h1>');
  }
});

server.listen(3000);
```

### Handling Different Methods
```javascript
const server = http.createServer((req, res) => {
  if (req.method === 'GET') {
    res.end('GET request');
  } else if (req.method === 'POST') {
    let body = '';
    
    req.on('data', chunk => {
      body += chunk.toString();
    });
    
    req.on('end', () => {
      console.log('Body:', body);
      res.end('POST request received');
    });
  }
});
```

### Making HTTP Requests
```javascript
const http = require('http');

// GET request
http.get('http://api.example.com/data', (res) => {
  let data = '';
  
  res.on('data', (chunk) => {
    data += chunk;
  });
  
  res.on('end', () => {
    console.log(JSON.parse(data));
  });
}).on('error', (err) => {
  console.error(err);
});

// POST request
const options = {
  hostname: 'api.example.com',
  port: 80,
  path: '/data',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  }
};

const req = http.request(options, (res) => {
  res.on('data', (chunk) => {
    console.log(chunk.toString());
  });
});

req.write(JSON.stringify({ name: 'John' }));
req.end();
```

---

## Events

### EventEmitter
```javascript
const EventEmitter = require('events');

// Create emitter
const emitter = new EventEmitter();

// Register listener
emitter.on('event', (arg) => {
  console.log('Event fired with:', arg);
});

// Emit event
emitter.emit('event', 'some data');

// One-time listener
emitter.once('event', () => {
  console.log('This runs only once');
});

// Remove listener
const listener = () => console.log('Event');
emitter.on('event', listener);
emitter.removeListener('event', listener);
emitter.off('event', listener); // Alias

// Remove all listeners
emitter.removeAllListeners('event');

// Get listener count
emitter.listenerCount('event');

// Get listeners
emitter.listeners('event');
```

### Custom EventEmitter
```javascript
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {
  constructor() {
    super();
  }
  
  doSomething() {
    // Do work
    this.emit('done', { status: 'success' });
  }
}

const myEmitter = new MyEmitter();

myEmitter.on('done', (data) => {
  console.log('Task completed:', data);
});

myEmitter.doSomething();
```

### Error Events
```javascript
const emitter = new EventEmitter();

// Always handle error events
emitter.on('error', (err) => {
  console.error('Error occurred:', err);
});

emitter.emit('error', new Error('Something went wrong'));
```

---

## Streams

### Types of Streams
```javascript
// 1. Readable - read data from source
// 2. Writable - write data to destination
// 3. Duplex - both readable and writable
// 4. Transform - modify data while reading/writing
```

### Readable Stream
```javascript
const fs = require('fs');

const readable = fs.createReadStream('file.txt', {
  encoding: 'utf8',
  highWaterMark: 16 * 1024 // 16KB chunks
});

// Data event
readable.on('data', (chunk) => {
  console.log('Received chunk:', chunk);
});

// End event
readable.on('end', () => {
  console.log('No more data');
});

// Error event
readable.on('error', (err) => {
  console.error(err);
});

// Pause and resume
readable.pause();
readable.resume();
```

### Writable Stream
```javascript
const writable = fs.createWriteStream('output.txt');

writable.write('First line\n');
writable.write('Second line\n');
writable.end('Final line\n');

writable.on('finish', () => {
  console.log('Write completed');
});

writable.on('error', (err) => {
  console.error(err);
});
```

### Pipe
```javascript
// Pipe readable to writable
const readable = fs.createReadStream('input.txt');
const writable = fs.createWriteStream('output.txt');

readable.pipe(writable);

// Chain pipes
const zlib = require('zlib');
fs.createReadStream('input.txt')
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream('input.txt.gz'));
```

### Transform Stream
```javascript
const { Transform } = require('stream');

const upperCase = new Transform({
  transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  }
});

process.stdin.pipe(upperCase).pipe(process.stdout);
```

### Custom Readable Stream
```javascript
const { Readable } = require('stream');

class MyReadable extends Readable {
  constructor(data) {
    super();
    this.data = data;
    this.index = 0;
  }
  
  _read() {
    if (this.index < this.data.length) {
      this.push(this.data[this.index]);
      this.index++;
    } else {
      this.push(null); // Signal end
    }
  }
}

const readable = new MyReadable(['a', 'b', 'c']);
readable.on('data', (chunk) => console.log(chunk.toString()));
```

---

## Buffer

### Creating Buffers
```javascript
// From string
const buf1 = Buffer.from('Hello');
const buf2 = Buffer.from('Hello', 'utf8');

// Allocate size
const buf3 = Buffer.alloc(10); // Filled with zeros
const buf4 = Buffer.allocUnsafe(10); // Uninitialized (faster)

// From array
const buf5 = Buffer.from([1, 2, 3, 4]);
```

### Buffer Operations
```javascript
const buf = Buffer.from('Hello World');

// Length
buf.length; // 11

// Convert to string
buf.toString(); // 'Hello World'
buf.toString('hex'); // Hexadecimal
buf.toString('base64'); // Base64

// Read/Write
buf.write('Hi', 0); // Write at position
buf[0] = 72; // Set byte value

// Slice
const slice = buf.slice(0, 5); // Buffer('Hello')

// Copy
const target = Buffer.alloc(11);
buf.copy(target);

// Compare
const buf1 = Buffer.from('abc');
const buf2 = Buffer.from('abc');
buf1.equals(buf2); // true
Buffer.compare(buf1, buf2); // 0 (equal)

// Concat
const buf3 = Buffer.concat([buf1, buf2]);

// Check if buffer
Buffer.isBuffer(buf); // true

// JSON
buf.toJSON(); // { type: 'Buffer', data: [...] }
```

---

## Process & Child Process

### Process Object
```javascript
// Current directory
process.cwd();

// Change directory
process.chdir('/path/to/dir');

// Environment variables
process.env.NODE_ENV;
process.env.PORT;

// Command line arguments
process.argv; // ['node', 'script.js', 'arg1', 'arg2']
process.argv.slice(2); // ['arg1', 'arg2']

// Exit
process.exit(0); // 0 = success, 1 = failure

// Platform
process.platform; // 'darwin', 'linux', 'win32'

// Architecture
process.arch; // 'x64', 'arm'

// Process ID
process.pid;

// Uptime
process.uptime();

// Memory usage
process.memoryUsage();
// {
//   rss: 123456,
//   heapTotal: 234567,
//   heapUsed: 123456,
//   external: 12345
// }

// CPU usage
process.cpuUsage();

// Standard I/O
process.stdin.on('data', (data) => {
  console.log('Input:', data.toString());
});

process.stdout.write('Output\n');
process.stderr.write('Error\n');
```

### Process Events
```javascript
// Before exit
process.on('beforeExit', (code) => {
  console.log('Process will exit with code:', code);
});

// Exit
process.on('exit', (code) => {
  console.log('Process exited with code:', code);
});

// Uncaught exception
process.on('uncaughtException', (err) => {
  console.error('Uncaught exception:', err);
  process.exit(1);
});

// Unhandled promise rejection
process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled rejection:', reason);
});

// Signals
process.on('SIGINT', () => {
  console.log('Received SIGINT');
  process.exit(0);
});

process.on('SIGTERM', () => {
  console.log('Received SIGTERM');
  process.exit(0);
});
```

### Child Process
```javascript
const { exec, execFile, spawn, fork } = require('child_process');

// exec - run command in shell
exec('ls -la', (err, stdout, stderr) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(stdout);
});

// execFile - run executable directly
execFile('node', ['--version'], (err, stdout, stderr) => {
  console.log(stdout);
});

// spawn - stream data
const child = spawn('ls', ['-la']);

child.stdout.on('data', (data) => {
  console.log(`stdout: ${data}`);
});

child.stderr.on('data', (data) => {
  console.error(`stderr: ${data}`);
});

child.on('close', (code) => {
  console.log(`Child process exited with code ${code}`);
});

// fork - run Node.js script
const forked = fork('child.js');

forked.send({ message: 'Hello child' });

forked.on('message', (msg) => {
  console.log('Message from child:', msg);
});
```

---

## OS Module

### System Information
```javascript
const os = require('os');

// Platform
os.platform(); // 'darwin', 'linux', 'win32'

// Architecture
os.arch(); // 'x64', 'arm'

// CPU info
os.cpus(); // Array of CPU cores

// Total memory
os.totalmem(); // Bytes

// Free memory
os.freemem(); // Bytes

// Hostname
os.hostname();

// Home directory
os.homedir();

// Temp directory
os.tmpdir();

// Network interfaces
os.networkInterfaces();

// Uptime
os.uptime(); // Seconds

// User info
os.userInfo();
// {
//   uid: 1000,
//   gid: 1000,
//   username: 'user',
//   homedir: '/home/user',
//   shell: '/bin/bash'
// }

// EOL (end of line)
os.EOL; // '\n' on Unix, '\r\n' on Windows
```

---

## Crypto

### Hashing
```javascript
const crypto = require('crypto');

// Create hash
const hash = crypto.createHash('sha256');
hash.update('password');
const hashed = hash.digest('hex');

// One-liner
const hashed2 = crypto.createHash('sha256')
  .update('password')
  .digest('hex');

// MD5
const md5 = crypto.createHash('md5')
  .update('data')
  .digest('hex');
```

### HMAC
```javascript
// Hash-based Message Authentication Code
const hmac = crypto.createHmac('sha256', 'secret-key');
hmac.update('message');
const signature = hmac.digest('hex');
```

### Random Data
```javascript
// Random bytes
const randomBytes = crypto.randomBytes(16);
console.log(randomBytes.toString('hex'));

// Random UUID
const uuid = crypto.randomUUID();
console.log(uuid); // '550e8400-e29b-41d4-a716-446655440000'

// Random integer
const randomInt = crypto.randomInt(1, 100); // 1-99
```

### Encryption & Decryption
```javascript
const algorithm = 'aes-256-cbc';
const key = crypto.randomBytes(32);
const iv = crypto.randomBytes(16);

// Encrypt
function encrypt(text) {
  const cipher = crypto.createCipheriv(algorithm, key, iv);
  let encrypted = cipher.update(text, 'utf8', 'hex');
  encrypted += cipher.final('hex');
  return encrypted;
}

// Decrypt
function decrypt(encrypted) {
  const decipher = crypto.createDecipheriv(algorithm, key, iv);
  let decrypted = decipher.update(encrypted, 'hex', 'utf8');
  decrypted += decipher.final('utf8');
  return decrypted;
}

const encrypted = encrypt('Hello World');
const decrypted = decrypt(encrypted);
```

### Password Hashing
```javascript
const crypto = require('crypto');
const util = require('util');

const scrypt = util.promisify(crypto.scrypt);

// Hash password
async function hashPassword(password) {
  const salt = crypto.randomBytes(16).toString('hex');
  const derivedKey = await scrypt(password, salt, 64);
  return salt + ':' + derivedKey.toString('hex');
}

// Verify password
async function verifyPassword(password, hash) {
  const [salt, key] = hash.split(':');
  const derivedKey = await scrypt(password, salt, 64);
  return key === derivedKey.toString('hex');
}
```

---

## Async Patterns

### Callbacks
```javascript
// Callback pattern
function fetchData(callback) {
  setTimeout(() => {
    callback(null, 'data');
  }, 1000);
}

fetchData((err, data) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(data);
});
```

### Promises
```javascript
// Promise pattern
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('data');
    }, 1000);
  });
}

fetchData()
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

### Async/Await
```javascript
// Async/await pattern
async function getData() {
  try {
    const data = await fetchData();
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}
```

### Promisify
```javascript
const util = require('util');
const fs = require('fs');

// Convert callback-based function to promise-based
const readFile = util.promisify(fs.readFile);

async function readMyFile() {
  const data = await readFile('file.txt', 'utf8');
  console.log(data);
}

// Custom promisify
function promisify(fn) {
  return function(...args) {
    return new Promise((resolve, reject) => {
      fn(...args, (err, result) => {
        if (err) reject(err);
        else resolve(result);
      });
    });
  };
}
```

### Async Iteration
```javascript
// Async iterators
async function* asyncGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

(async () => {
  for await (const num of asyncGenerator()) {
    console.log(num);
  }
})();

// Async iteration with streams
const fs = require('fs');
const readline = require('readline');

const rl = readline.createInterface({
  input: fs.createReadStream('file.txt')
});

for await (const line of rl) {
  console.log(line);
}
```

---

## Error Handling

### Try-Catch
```javascript
// Synchronous error handling
try {
  const data = fs.readFileSync('file.txt', 'utf8');
  console.log(data);
} catch (err) {
  console.error('Error reading file:', err.message);
} finally {
  console.log('Cleanup');
}

// Async error handling
async function readFile() {
  try {
    const data = await fsPromises.readFile('file.txt', 'utf8');
    console.log(data);
  } catch (err) {
    console.error('Error:', err.message);
  }
}
```

### Custom Errors
```javascript
// Custom error class
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = 'ValidationError';
    this.statusCode = 400;
  }
}

class NotFoundError extends Error {
  constructor(message) {
    super(message);
    this.name = 'NotFoundError';
    this.statusCode = 404;
  }
}

// Usage
function validateUser(user) {
  if (!user.email) {
    throw new ValidationError('Email is required');
  }
}

try {
  validateUser({});
} catch (err) {
  if (err instanceof ValidationError) {
    console.error('Validation error:', err.message);
  }
}
```

### Error-First Callbacks
```javascript
// Node.js convention: (err, result)
function readFile(path, callback) {
  fs.readFile(path, 'utf8', (err, data) => {
    if (err) {
      callback(err);
      return;
    }
    callback(null, data);
  });
}

readFile('file.txt', (err, data) => {
  if (err) {
    console.error('Error:', err);
    return;
  }
  console.log(data);
});
```

---

## Debugging

### Console Debugging
```javascript
// Basic logging
console.log('Log message');
console.error('Error message');
console.warn('Warning message');
console.info('Info message');

// Object inspection
console.dir(obj, { depth: null, colors: true });

// Table
console.table([{ name: 'John', age: 30 }]);

// Timing
console.time('operation');
// Do something
console.timeEnd('operation');

// Trace
console.trace('Trace point');

// Assert
console.assert(1 === 2, 'Values are not equal');
```

### Debug Module
```javascript
const debug = require('debug')('app:main');

debug('This is a debug message');

// Enable in terminal
// DEBUG=app:* node app.js
// DEBUG=* node app.js
```

### Node.js Debugger
```bash
# Start with debugger
node inspect app.js

# Debugger commands:
# cont, c - Continue
# next, n - Next line
# step, s - Step into
# out, o - Step out
# pause - Pause
# watch('expr') - Watch expression
# repl - Enter REPL
```

### VS Code Debugging
```json
// .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "program": "${workspaceFolder}/app.js",
      "skipFiles": ["<node_internals>/**"]
    }
  ]
}
```

---

## Environment Variables

### Using .env Files
```bash
# .env
NODE_ENV=development
PORT=3000
DATABASE_URL=mongodb://localhost:27017/mydb
API_KEY=secret123
```

```javascript
// Install dotenv
// npm install dotenv

// Load environment variables
require('dotenv').config();

// Access variables
const port = process.env.PORT || 3000;
const dbUrl = process.env.DATABASE_URL;
const apiKey = process.env.API_KEY;

console.log(`Server running on port ${port}`);
```

### Cross-env
```bash
# Install cross-env (for cross-platform compatibility)
npm install --save-dev cross-env

# package.json
{
  "scripts": {
    "start": "cross-env NODE_ENV=production node server.js",
    "dev": "cross-env NODE_ENV=development nodemon server.js"
  }
}
```

---

## Best Practices

### Project Structure
```
my-app/
├── src/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   ├── middleware/
│   ├── utils/
│   └── app.js
├── tests/
├── config/
├── public/
├── .env
├── .gitignore
├── package.json
└── README.md
```

### Error Handling
```javascript
// ✅ Good: Handle all errors
async function getData() {
  try {
    const data = await fetchData();
    return data;
  } catch (err) {
    console.error('Error fetching data:', err);
    throw err; // Re-throw if needed
  }
}

// ✅ Good: Use error-first callbacks
function readFile(path, callback) {
  fs.readFile(path, (err, data) => {
    if (err) return callback(err);
    callback(null, data);
  });
}

// ❌ Bad: Swallow errors
async function getData() {
  try {
    const data = await fetchData();
    return data;
  } catch (err) {
    // Silent failure - don't do this!
  }
}
```

### Async Patterns
```javascript
// ✅ Good: Use async/await
async function processUsers() {
  const users = await getUsers();
  const processed = await Promise.all(
    users.map(user => processUser(user))
  );
  return processed;
}

// ❌ Bad: Callback hell
function processUsers(callback) {
  getUsers((err, users) => {
    if (err) return callback(err);
    processUser(users[0], (err, result) => {
      if (err) return callback(err);
      // More nesting...
    });
  });
}
```

### Security
```javascript
// ✅ Good: Validate input
function createUser(data) {
  if (!data.email || !data.password) {
    throw new Error('Invalid input');
  }
  // Process...
}

// ✅ Good: Use environment variables for secrets
const apiKey = process.env.API_KEY;

// ❌ Bad: Hardcode secrets
const apiKey = 'abc123secret'; // Don't do this!

// ✅ Good: Sanitize user input
const clean = userInput.trim().toLowerCase();

// ✅ Good: Use HTTPS for external requests
const https = require('https');
```

### Performance
```javascript
// ✅ Good: Use streams for large files
fs.createReadStream('large-file.txt')
  .pipe(processStream)
  .pipe(fs.createWriteStream('output.txt'));

// ❌ Bad: Load entire file into memory
const data = fs.readFileSync('large-file.txt');

// ✅ Good: Cache expensive operations
const cache = new Map();

async function getData(key) {
  if (cache.has(key)) {
    return cache.get(key);
  }
  const data = await fetchData(key);
  cache.set(key, data);
  return data;
}

// ✅ Good: Use worker threads for CPU-intensive tasks
const { Worker } = require('worker_threads');

const worker = new Worker('./heavy-task.js');
```

### Code Organization
```javascript
// ✅ Good: Modular code
// users.js
module.exports = {
  getUser,
  createUser,
  updateUser
};

// ✅ Good: Single responsibility
function calculateTotal(items) {
  return items.reduce((sum, item) => sum + item.price, 0);
}

function applyDiscount(total, discount) {
  return total * (1 - discount);
}

// ❌ Bad: Do everything in one function
function processOrder(items, discount) {
  // Calculate total, apply discount, save to DB, send email...
}
```

---

**Pro Tip:** Master async/await for clean asynchronous code, use streams for large data, leverage the event-driven architecture, and always handle errors properly. Node.js excels at I/O-bound tasks!