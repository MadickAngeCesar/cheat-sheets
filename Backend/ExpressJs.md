# Express.js Cheat Sheet - Beginner to Pro

## Table of Contents
- [Setup & Installation](#setup--installation)
- [Basic Server](#basic-server)
- [Routing](#routing)
- [Middleware](#middleware)
- [Request & Response](#request--response)
- [Route Parameters](#route-parameters)
- [Query Parameters](#query-parameters)
- [Request Body](#request-body)
- [Static Files](#static-files)
- [Template Engines](#template-engines)
- [Error Handling](#error-handling)
- [RESTful APIs](#restful-apis)
- [Authentication & Authorization](#authentication--authorization)
- [Database Integration](#database-integration)
- [File Uploads](#file-uploads)
- [Sessions & Cookies](#sessions--cookies)
- [Security](#security)
- [Testing](#testing)
- [Best Practices](#best-practices)

---

## Setup & Installation

### Installation
```bash
# Initialize project
npm init -y

# Install Express
npm install express

# Install common dependencies
npm install dotenv cors helmet morgan

# TypeScript setup
npm install --save-dev @types/express @types/node typescript ts-node
```

### Package.json Scripts
```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "node --watch server.js",
    "test": "jest"
  }
}
```

---

## Basic Server

### Minimal Server
```javascript
const express = require('express');
const app = express();
const PORT = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

### With Environment Variables
```javascript
require('dotenv').config();
const express = require('express');
const app = express();

const PORT = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### ES6 Modules
```javascript
// package.json: "type": "module"
import express from 'express';

const app = express();
const PORT = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

---

## Routing

### Basic Routes
```javascript
// GET request
app.get('/', (req, res) => {
  res.send('GET request');
});

// POST request
app.post('/users', (req, res) => {
  res.send('POST request');
});

// PUT request
app.put('/users/:id', (req, res) => {
  res.send('PUT request');
});

// PATCH request
app.patch('/users/:id', (req, res) => {
  res.send('PATCH request');
});

// DELETE request
app.delete('/users/:id', (req, res) => {
  res.send('DELETE request');
});

// All HTTP methods
app.all('/secret', (req, res) => {
  res.send('All methods');
});
```

### Route Paths
```javascript
// String path
app.get('/about', (req, res) => {
  res.send('About');
});

// Pattern path
app.get('/ab?cd', (req, res) => {
  // Matches /acd and /abcd
  res.send('Pattern');
});

app.get('/ab+cd', (req, res) => {
  // Matches /abcd, /abbcd, /abbbcd, etc.
  res.send('Pattern');
});

app.get('/ab*cd', (req, res) => {
  // Matches /abcd, /abxcd, /abRANDOMcd, etc.
  res.send('Pattern');
});

// Regular expression
app.get(/.*fly$/, (req, res) => {
  // Matches /butterfly, /dragonfly, etc.
  res.send('Regex');
});
```

### Route Chaining
```javascript
app.route('/users')
  .get((req, res) => {
    res.send('Get all users');
  })
  .post((req, res) => {
    res.send('Create user');
  });

app.route('/users/:id')
  .get((req, res) => {
    res.send('Get user');
  })
  .put((req, res) => {
    res.send('Update user');
  })
  .delete((req, res) => {
    res.send('Delete user');
  });
```

### Express Router
```javascript
// routes/users.js
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
  res.send('Get all users');
});

router.get('/:id', (req, res) => {
  res.send(`Get user ${req.params.id}`);
});

router.post('/', (req, res) => {
  res.send('Create user');
});

module.exports = router;

// app.js
const usersRouter = require('./routes/users');
app.use('/users', usersRouter);
```

---

## Middleware

### Application-level Middleware
```javascript
// Execute for every request
app.use((req, res, next) => {
  console.log('Time:', Date.now());
  next();
});

// Execute for specific path
app.use('/user', (req, res, next) => {
  console.log('User route');
  next();
});

// Multiple middleware functions
app.use('/user/:id', 
  (req, res, next) => {
    console.log('First middleware');
    next();
  },
  (req, res, next) => {
    console.log('Second middleware');
    next();
  }
);
```

### Router-level Middleware
```javascript
const router = express.Router();

router.use((req, res, next) => {
  console.log('Router middleware');
  next();
});

router.get('/user/:id', (req, res) => {
  res.send('User info');
});

app.use('/api', router);
```

### Built-in Middleware
```javascript
// Parse JSON bodies
app.use(express.json());

// Parse URL-encoded bodies
app.use(express.urlencoded({ extended: true }));

// Serve static files
app.use(express.static('public'));
```

### Third-party Middleware
```javascript
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');
const compression = require('compression');

// CORS
app.use(cors());

// Security headers
app.use(helmet());

// Logging
app.use(morgan('dev'));

// Compression
app.use(compression());
```

### Custom Middleware
```javascript
// Logger middleware
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
};

app.use(logger);

// Authentication middleware
const authenticate = (req, res, next) => {
  const token = req.headers.authorization;
  
  if (!token) {
    return res.status(401).json({ error: 'No token provided' });
  }
  
  // Verify token
  try {
    // Token verification logic
    next();
  } catch (err) {
    res.status(401).json({ error: 'Invalid token' });
  }
};

// Use on specific routes
app.get('/protected', authenticate, (req, res) => {
  res.send('Protected route');
});
```

---

## Request & Response

### Request Object
```javascript
app.get('/user', (req, res) => {
  // Request properties
  console.log(req.method);        // HTTP method
  console.log(req.url);           // URL path
  console.log(req.path);          // Path
  console.log(req.query);         // Query parameters
  console.log(req.params);        // Route parameters
  console.log(req.body);          // Request body
  console.log(req.headers);       // Request headers
  console.log(req.cookies);       // Cookies
  console.log(req.ip);            // Client IP
  console.log(req.hostname);      // Host
  console.log(req.protocol);      // http or https
  console.log(req.secure);        // Is HTTPS
  
  // Get specific header
  const userAgent = req.get('User-Agent');
});
```

### Response Object
```javascript
app.get('/data', (req, res) => {
  // Send response
  res.send('Text response');
  res.send({ message: 'JSON response' });
  res.send(Buffer.from('Buffer'));
  
  // JSON response
  res.json({ name: 'John', age: 30 });
  
  // Status code
  res.status(404).send('Not Found');
  res.status(201).json({ created: true });
  
  // Status text
  res.sendStatus(200); // OK
  res.sendStatus(404); // Not Found
  
  // Set headers
  res.set('Content-Type', 'text/html');
  res.set({
    'Content-Type': 'text/plain',
    'X-Custom-Header': 'value'
  });
  
  // Redirect
  res.redirect('/other-page');
  res.redirect(301, '/moved-permanently');
  res.redirect('back'); // Back to referrer
  
  // Download file
  res.download('/path/to/file.pdf');
  res.download('/path/to/file.pdf', 'custom-name.pdf');
  
  // Send file
  res.sendFile('/path/to/file.html');
  
  // End response
  res.end();
});
```

### Response Chaining
```javascript
app.get('/user', (req, res) => {
  res
    .status(200)
    .set('Content-Type', 'application/json')
    .json({ name: 'John' });
});
```

---

## Route Parameters

### Basic Parameters
```javascript
// Single parameter
app.get('/users/:id', (req, res) => {
  const userId = req.params.id;
  res.send(`User ID: ${userId}`);
});

// Multiple parameters
app.get('/users/:userId/posts/:postId', (req, res) => {
  const { userId, postId } = req.params;
  res.send(`User: ${userId}, Post: ${postId}`);
});
```

### Optional Parameters
```javascript
// Using regex
app.get('/users/:id?', (req, res) => {
  if (req.params.id) {
    res.send(`User ID: ${req.params.id}`);
  } else {
    res.send('All users');
  }
});
```

### Parameter Validation
```javascript
// Validate parameter format
app.param('id', (req, res, next, id) => {
  if (!/^\d+$/.test(id)) {
    return res.status(400).json({ error: 'Invalid ID' });
  }
  req.userId = id;
  next();
});

app.get('/users/:id', (req, res) => {
  res.send(`User ID: ${req.userId}`);
});
```

---

## Query Parameters

### Accessing Query Parameters
```javascript
// URL: /search?q=express&limit=10
app.get('/search', (req, res) => {
  const query = req.query.q;
  const limit = req.query.limit || 20;
  
  res.json({
    query,
    limit,
    allParams: req.query
  });
});

// Array query parameters
// URL: /filter?tags=node&tags=express
app.get('/filter', (req, res) => {
  const tags = req.query.tags; // ['node', 'express']
  res.json({ tags });
});
```

---

## Request Body

### JSON Body
```javascript
// Enable JSON parsing
app.use(express.json());

app.post('/users', (req, res) => {
  const { name, email, age } = req.body;
  
  res.status(201).json({
    message: 'User created',
    user: { name, email, age }
  });
});
```

### URL-encoded Body
```javascript
// Enable URL-encoded parsing
app.use(express.urlencoded({ extended: true }));

app.post('/form', (req, res) => {
  const { username, password } = req.body;
  res.send(`Username: ${username}`);
});
```

### Body Size Limit
```javascript
// Set body size limit
app.use(express.json({ limit: '10mb' }));
app.use(express.urlencoded({ extended: true, limit: '10mb' }));
```

---

## Static Files

### Serving Static Files
```javascript
// Serve files from 'public' directory
app.use(express.static('public'));

// Multiple directories
app.use(express.static('public'));
app.use(express.static('files'));

// Virtual path prefix
app.use('/static', express.static('public'));

// Absolute path
const path = require('path');
app.use(express.static(path.join(__dirname, 'public')));
```

---

## Template Engines

### EJS
```javascript
// Install: npm install ejs
app.set('view engine', 'ejs');
app.set('views', './views');

app.get('/', (req, res) => {
  res.render('index', { title: 'Home', user: 'John' });
});

// views/index.ejs
/*
<!DOCTYPE html>
<html>
<head>
  <title><%= title %></title>
</head>
<body>
  <h1>Hello <%= user %></h1>
</body>
</html>
*/
```

### Pug (Jade)
```javascript
// Install: npm install pug
app.set('view engine', 'pug');
app.set('views', './views');

app.get('/', (req, res) => {
  res.render('index', { title: 'Home', user: 'John' });
});

// views/index.pug
/*
doctype html
html
  head
    title= title
  body
    h1 Hello #{user}
*/
```

---

## Error Handling

### Basic Error Handling
```javascript
// 404 handler
app.use((req, res, next) => {
  res.status(404).send('Page not found');
});

// Error handling middleware (must have 4 parameters)
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

### Custom Error Handler
```javascript
// Custom error class
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.status = `${statusCode}`.startsWith('4') ? 'fail' : 'error';
    this.isOperational = true;
    
    Error.captureStackTrace(this, this.constructor);
  }
}

// Use in routes
app.get('/user/:id', (req, res, next) => {
  const user = getUserById(req.params.id);
  
  if (!user) {
    return next(new AppError('User not found', 404));
  }
  
  res.json(user);
});

// Global error handler
app.use((err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  err.status = err.status || 'error';
  
  res.status(err.statusCode).json({
    status: err.status,
    message: err.message
  });
});
```

### Async Error Handling
```javascript
// Async wrapper
const asyncHandler = (fn) => {
  return (req, res, next) => {
    fn(req, res, next).catch(next);
  };
};

// Use with async routes
app.get('/users', asyncHandler(async (req, res) => {
  const users = await User.find();
  res.json(users);
}));
```

---

## RESTful APIs

### Complete REST API
```javascript
const express = require('express');
const app = express();

app.use(express.json());

// In-memory data store
let users = [
  { id: 1, name: 'John', email: 'john@example.com' },
  { id: 2, name: 'Jane', email: 'jane@example.com' }
];

// GET - Get all users
app.get('/api/users', (req, res) => {
  res.json(users);
});

// GET - Get single user
app.get('/api/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  
  if (!user) {
    return res.status(404).json({ error: 'User not found' });
  }
  
  res.json(user);
});

// POST - Create user
app.post('/api/users', (req, res) => {
  const { name, email } = req.body;
  
  if (!name || !email) {
    return res.status(400).json({ error: 'Name and email required' });
  }
  
  const newUser = {
    id: users.length + 1,
    name,
    email
  };
  
  users.push(newUser);
  res.status(201).json(newUser);
});

// PUT - Update user
app.put('/api/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  
  if (!user) {
    return res.status(404).json({ error: 'User not found' });
  }
  
  const { name, email } = req.body;
  
  if (name) user.name = name;
  if (email) user.email = email;
  
  res.json(user);
});

// DELETE - Delete user
app.delete('/api/users/:id', (req, res) => {
  const index = users.findIndex(u => u.id === parseInt(req.params.id));
  
  if (index === -1) {
    return res.status(404).json({ error: 'User not found' });
  }
  
  users.splice(index, 1);
  res.status(204).send();
});
```

### REST API with Router
```javascript
// routes/api/users.js
const express = require('express');
const router = express.Router();

router.get('/', getAllUsers);
router.get('/:id', getUser);
router.post('/', createUser);
router.put('/:id', updateUser);
router.delete('/:id', deleteUser);

module.exports = router;

// app.js
const usersRouter = require('./routes/api/users');
app.use('/api/users', usersRouter);
```

---

## Authentication & Authorization

### JWT Authentication
```javascript
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');

// Register
app.post('/register', async (req, res) => {
  try {
    const { username, password } = req.body;
    
    // Hash password
    const hashedPassword = await bcrypt.hash(password, 10);
    
    // Save user (pseudo-code)
    const user = await User.create({
      username,
      password: hashedPassword
    });
    
    res.status(201).json({ message: 'User created' });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Login
app.post('/login', async (req, res) => {
  try {
    const { username, password } = req.body;
    
    // Find user
    const user = await User.findOne({ username });
    
    if (!user) {
      return res.status(401).json({ error: 'Invalid credentials' });
    }
    
    // Verify password
    const validPassword = await bcrypt.compare(password, user.password);
    
    if (!validPassword) {
      return res.status(401).json({ error: 'Invalid credentials' });
    }
    
    // Generate JWT
    const token = jwt.sign(
      { userId: user.id, username: user.username },
      process.env.JWT_SECRET,
      { expiresIn: '24h' }
    );
    
    res.json({ token });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Auth middleware
const authenticateToken = (req, res, next) => {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({ error: 'Access token required' });
  }
  
  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) {
      return res.status(403).json({ error: 'Invalid token' });
    }
    
    req.user = user;
    next();
  });
};

// Protected route
app.get('/profile', authenticateToken, (req, res) => {
  res.json({ user: req.user });
});
```

### Role-based Authorization
```javascript
const authorize = (...roles) => {
  return (req, res, next) => {
    if (!req.user) {
      return res.status(401).json({ error: 'Unauthorized' });
    }
    
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Forbidden' });
    }
    
    next();
  };
};

// Use middleware
app.delete('/users/:id', 
  authenticateToken, 
  authorize('admin'), 
  deleteUser
);
```

---

## Database Integration

### MongoDB with Mongoose
```javascript
const mongoose = require('mongoose');

// Connect to MongoDB
mongoose.connect(process.env.MONGODB_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true
});

// Define schema
const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  age: Number
});

const User = mongoose.model('User', userSchema);

// Routes
app.get('/users', async (req, res) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

app.post('/users', async (req, res) => {
  try {
    const user = new User(req.body);
    await user.save();
    res.status(201).json(user);
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
});
```

### PostgreSQL with Prisma
```javascript
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();

app.get('/users', async (req, res) => {
  try {
    const users = await prisma.user.findMany();
    res.json(users);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

app.post('/users', async (req, res) => {
  try {
    const user = await prisma.user.create({
      data: req.body
    });
    res.status(201).json(user);
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
});
```

---

## File Uploads

### Using Multer
```javascript
const multer = require('multer');

// Configure storage
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },
  filename: (req, file, cb) => {
    cb(null, Date.now() + '-' + file.originalname);
  }
});

const upload = multer({ 
  storage,
  limits: { fileSize: 5 * 1024 * 1024 }, // 5MB
  fileFilter: (req, file, cb) => {
    if (file.mimetype.startsWith('image/')) {
      cb(null, true);
    } else {
      cb(new Error('Only images allowed'));
    }
  }
});

// Single file upload
app.post('/upload', upload.single('image'), (req, res) => {
  res.json({
    message: 'File uploaded',
    file: req.file
  });
});

// Multiple files
app.post('/upload-multiple', upload.array('images', 5), (req, res) => {
  res.json({
    message: 'Files uploaded',
    files: req.files
  });
});

// Multiple fields
app.post('/upload-fields', upload.fields([
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 5 }
]), (req, res) => {
  res.json({
    avatar: req.files['avatar'],
    gallery: req.files['gallery']
  });
});
```

---

## Sessions & Cookies

### Cookie Parser
```javascript
const cookieParser = require('cookie-parser');

app.use(cookieParser());

// Set cookie
app.get('/set-cookie', (req, res) => {
  res.cookie('username', 'John', {
    maxAge: 900000,
    httpOnly: true,
    secure: true
  });
  res.send('Cookie set');
});

// Read cookie
app.get('/get-cookie', (req, res) => {
  const username = req.cookies.username;
  res.send(`Username: ${username}`);
});

// Clear cookie
app.get('/clear-cookie', (req, res) => {
  res.clearCookie('username');
  res.send('Cookie cleared');
});
```

### Express Session
```javascript
const session = require('express-session');

app.use(session({
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  cookie: {
    secure: process.env.NODE_ENV === 'production',
    httpOnly: true,
    maxAge: 24 * 60 * 60 * 1000 // 24 hours
  }
}));

// Set session data
app.post('/login', (req, res) => {
  req.session.userId = user.id;
  req.session.username = user.username;
  res.send('Logged in');
});

// Read session data
app.get('/profile', (req, res) => {
  if (!req.session.userId) {
    return res.status(401).send('Not logged in');
  }
  res.json({ username: req.session.username });
});

// Destroy session
app.post('/logout', (req, res) => {
  req.session.destroy((err) => {
    if (err) {
      return res.status(500).send('Logout failed');
    }
    res.send('Logged out');
  });
});
```

---

## Security

### Essential Security Middleware
```javascript
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');
const mongoSanitize = require('express-mongo-sanitize');
const xss = require('xss-clean');
const hpp = require('hpp');

// Security headers
app.use(helmet());

// Rate limiting
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});
app.use('/api', limiter);

// Data sanitization against NoSQL injection
app.use(mongoSanitize());

// Data sanitization against XSS
app.use(xss());

// Prevent parameter pollution
app.use(hpp());

// CORS
const cors = require('cors');
app.use(cors({
  origin: 'https://example.com',
  credentials: true
}));
```

### Input Validation
```javascript
const { body, validationResult } = require('express-validator');

app.post('/users',
  body('email').isEmail(),
  body('password').isLength({ min: 6 }),
  body('name').trim().notEmpty(),
  (req, res) => {
    const errors = validationResult(req);
    
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    
    // Process valid data
    res.send('User created');
  }
);
```

---

## Testing

### Jest & Supertest
```javascript
// Install: npm install --save-dev jest supertest
const request = require('supertest');
const app = require('./app');

describe('User API', () => {
  test('GET /api/users should return all users', async () => {
    const response = await request(app)
      .get('/api/users')
      .expect('Content-Type', /json/)
      .expect(200);
    
    expect(response.body).toBeInstanceOf(Array);
  });
  
  test('POST /api/users should create a user', async () => {
    const newUser = {
      name: 'John',
      email: 'john@example.com'
    };
    
    const response = await request(app)
      .post('/api/users')
      .send(newUser)
      .expect(201);
    
    expect(response.body).toHaveProperty('id');
    expect(response.body.name).toBe('John');
  });
  
  test('GET /api/users/:id should return 404 for invalid ID', async () => {
    await request(app)
      .get('/api/users/999')
      .expect(404);
  });
});
```

---

## Best Practices

### Project Structure
```
my-app/
├── src/
│   ├── controllers/
│   │   └── userController.js
│   ├── models/
│   │   └── User.js
│   ├── routes/
│   │   └── userRoutes.js
│   ├── middleware/
│   │   ├── auth.js
│   │   └── errorHandler.js
│   ├── utils/
│   │   └── helpers.js
│   ├── config/
│   │   └── database.js
│   └── app.js
├── tests/
├── public/
├── .env
├── .gitignore
├── package.json
└── server.js
```

### Separation of Concerns
```javascript
// controllers/userController.js
exports.getAllUsers = async (req, res, next) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (err) {
    next(err);
  }
};

// routes/userRoutes.js
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');

router.get('/', userController.getAllUsers);

module.exports = router;

// app.js
const userRoutes = require('./routes/userRoutes');
app.use('/api/users', userRoutes);
```

### Environment Configuration
```javascript
// .env
PORT=3000
NODE_ENV=development
DATABASE_URL=mongodb://localhost:27017/myapp
JWT_SECRET=your-secret-key

// config/config.js
require('dotenv').config();

module.exports = {
  port: process.env.PORT || 3000,
  env: process.env.NODE_ENV || 'development',
  databaseUrl: process.env.DATABASE_URL,
  jwtSecret: process.env.JWT_SECRET
};
```

### Error Handling Pattern
```javascript
// ✅ Good: Centralized error handling
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.status = `${statusCode}`.startsWith('4') ? 'fail' : 'error';
    this.isOperational = true;
  }
}

const errorHandler = (err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  err.status = err.status || 'error';
  
  if (process.env.NODE_ENV === 'development') {
    res.status(err.statusCode).json({
      status: err.status,
      error: err,
      message: err.message,
      stack: err.stack
    });
  } else {
    res.status(err.statusCode).json({
      status: err.status,
      message: err.message
    });
  }
};

app.use(errorHandler);

// ❌ Bad: Inline error handling everywhere
app.get('/users', (req, res) => {
  User.find((err, users) => {
    if (err) {
      res.status(500).send('Error');
      return;
    }
    res.json(users);
  });
});
```

### Async/Await Pattern
```javascript
// ✅ Good: Async/await with error handling
const asyncHandler = fn => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

app.get('/users', asyncHandler(async (req, res) => {
  const users = await User.find();
  res.json(users);
}));

// ❌ Bad: Callback hell
app.get('/users', (req, res) => {
  User.find((err, users) => {
    if (err) return res.status(500).send(err);
    Post.find({ userId: users[0].id }, (err, posts) => {
      if (err) return res.status(500).send(err);
      // More nesting...
    });
  });
});
```

### Logging
```javascript
const morgan = require('morgan');
const winston = require('winston');

// Morgan for HTTP logging
app.use(morgan('combined'));

// Winston for application logging
const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}

// Use logger
logger.info('Server started');
logger.error('Error occurred', { error: err });
```

---

**Pro Tip:** Use middleware extensively for cross-cutting concerns, implement proper error handling with async wrappers, structure your code with MVC pattern, validate all inputs, and always use environment variables for sensitive data!