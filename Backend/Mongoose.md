# Mongoose Cheat Sheet - Beginner to Pro

## Table of Contents
- [Setup & Installation](#setup--installation)
- [Connection](#connection)
- [Schemas](#schemas)
- [Models](#models)
- [CRUD Operations](#crud-operations)
- [Queries](#queries)
- [Validation](#validation)
- [Middleware (Hooks)](#middleware-hooks)
- [Population](#population)
- [Virtuals](#virtuals)
- [Indexes](#indexes)
- [Plugins](#plugins)
- [Transactions](#transactions)
- [Aggregation](#aggregation)
- [Best Practices](#best-practices)

---

## Setup & Installation

### Installation
```bash
# Install Mongoose
npm install mongoose

# With TypeScript
npm install mongoose @types/mongoose
```

---

## Connection

### Basic Connection
```javascript
const mongoose = require('mongoose');

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/mydb')
  .then(() => console.log('Connected to MongoDB'))
  .catch(err => console.error('Connection error:', err));

// With options
mongoose.connect('mongodb://localhost:27017/mydb', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

// Using async/await
async function connectDB() {
  try {
    await mongoose.connect('mongodb://localhost:27017/mydb');
    console.log('Connected to MongoDB');
  } catch (error) {
    console.error('Connection error:', error);
    process.exit(1);
  }
}

connectDB();
```

### Connection Events
```javascript
const db = mongoose.connection;

db.on('connected', () => {
  console.log('Mongoose connected to MongoDB');
});

db.on('error', (err) => {
  console.error('Mongoose connection error:', err);
});

db.on('disconnected', () => {
  console.log('Mongoose disconnected');
});

// Close connection
process.on('SIGINT', async () => {
  await mongoose.connection.close();
  process.exit(0);
});
```

### Multiple Connections
```javascript
const conn1 = mongoose.createConnection('mongodb://localhost:27017/db1');
const conn2 = mongoose.createConnection('mongodb://localhost:27017/db2');

const User = conn1.model('User', userSchema);
const Post = conn2.model('Post', postSchema);
```

---

## Schemas

### Basic Schema
```javascript
const mongoose = require('mongoose');
const { Schema } = mongoose;

const userSchema = new Schema({
  name: String,
  email: String,
  age: Number,
  createdAt: Date,
});

const User = mongoose.model('User', userSchema);
```

### Schema Types
```javascript
const schema = new Schema({
  // String
  name: String,
  email: { type: String, required: true },
  
  // Number
  age: Number,
  price: { type: Number, min: 0, max: 1000 },
  
  // Boolean
  isActive: Boolean,
  
  // Date
  createdAt: Date,
  birthDate: { type: Date, default: Date.now },
  
  // ObjectId
  userId: Schema.Types.ObjectId,
  
  // Array
  tags: [String],
  friends: [{ type: Schema.Types.ObjectId, ref: 'User' }],
  
  // Nested object
  address: {
    street: String,
    city: String,
    zipCode: String,
  },
  
  // Mixed (any type)
  metadata: Schema.Types.Mixed,
  
  // Buffer
  binary: Buffer,
  
  // Map
  socialLinks: {
    type: Map,
    of: String,
  },
  
  // Decimal128 (for precise decimal values)
  price: Schema.Types.Decimal128,
});
```

### Schema Options
```javascript
const userSchema = new Schema({
  name: {
    type: String,
    required: true,
    unique: true,
    trim: true,
    lowercase: true,
    uppercase: false,
    minlength: 3,
    maxlength: 50,
    match: /^[a-zA-Z]+$/,
    enum: ['user', 'admin', 'moderator'],
    default: 'user',
  },
  email: {
    type: String,
    required: [true, 'Email is required'],
    unique: true,
    validate: {
      validator: function(v) {
        return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v);
      },
      message: props => `${props.value} is not a valid email!`
    },
  },
  age: {
    type: Number,
    min: [18, 'Must be at least 18'],
    max: 100,
  },
}, {
  timestamps: true,        // Adds createdAt and updatedAt
  collection: 'users',     // Custom collection name
  versionKey: false,       // Remove __v field
});
```

### Nested Schemas
```javascript
const addressSchema = new Schema({
  street: String,
  city: String,
  zipCode: String,
});

const userSchema = new Schema({
  name: String,
  address: addressSchema,
  addresses: [addressSchema],  // Array of subdocuments
});
```

---

## Models

### Creating Models
```javascript
const User = mongoose.model('User', userSchema);

// Model name and collection name
// 'User' -> 'users' collection
// 'Person' -> 'people' collection
```

### Instance Methods
```javascript
userSchema.methods.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};

userSchema.methods.comparePassword = async function(candidatePassword) {
  return await bcrypt.compare(candidatePassword, this.password);
};

// Usage
const user = await User.findById(id);
console.log(user.getFullName());
```

### Static Methods
```javascript
userSchema.statics.findByEmail = function(email) {
  return this.findOne({ email });
};

userSchema.statics.findActiveUsers = function() {
  return this.find({ isActive: true });
};

// Usage
const user = await User.findByEmail('test@example.com');
const activeUsers = await User.findActiveUsers();
```

### Query Helpers
```javascript
userSchema.query.byName = function(name) {
  return this.where({ name: new RegExp(name, 'i') });
};

userSchema.query.active = function() {
  return this.where({ isActive: true });
};

// Usage
const users = await User.find().byName('john').active();
```

---

## CRUD Operations

### Create
```javascript
// Method 1: Using constructor
const user = new User({
  name: 'John Doe',
  email: 'john@example.com',
  age: 30,
});
await user.save();

// Method 2: Using create
const user = await User.create({
  name: 'John Doe',
  email: 'john@example.com',
  age: 30,
});

// Create multiple documents
const users = await User.create([
  { name: 'John', email: 'john@example.com' },
  { name: 'Jane', email: 'jane@example.com' },
]);

// Insert many
const users = await User.insertMany([
  { name: 'John', email: 'john@example.com' },
  { name: 'Jane', email: 'jane@example.com' },
]);
```

### Read
```javascript
// Find all
const users = await User.find();

// Find with conditions
const users = await User.find({ age: { $gte: 18 } });

// Find one
const user = await User.findOne({ email: 'john@example.com' });

// Find by ID
const user = await User.findById('60d5ec49f1b2c8b1f8e4e1a1');

// Select specific fields
const users = await User.find().select('name email');
const users = await User.find().select('-password');  // Exclude password

// Limit and skip
const users = await User.find().limit(10).skip(20);

// Sort
const users = await User.find().sort({ createdAt: -1 });  // Descending
const users = await User.find().sort('name');  // Ascending

// Count
const count = await User.countDocuments({ isActive: true });

// Exists
const exists = await User.exists({ email: 'john@example.com' });
```

### Update
```javascript
// Update one
const result = await User.updateOne(
  { email: 'john@example.com' },
  { $set: { age: 31 } }
);

// Update many
const result = await User.updateMany(
  { isActive: false },
  { $set: { isActive: true } }
);

// Find and update
const user = await User.findOneAndUpdate(
  { email: 'john@example.com' },
  { $set: { age: 31 } },
  { new: true, runValidators: true }  // Return updated doc, run validators
);

// Find by ID and update
const user = await User.findByIdAndUpdate(
  id,
  { age: 31 },
  { new: true }
);

// Update using instance
const user = await User.findById(id);
user.age = 31;
await user.save();
```

### Delete
```javascript
// Delete one
const result = await User.deleteOne({ email: 'john@example.com' });

// Delete many
const result = await User.deleteMany({ isActive: false });

// Find and delete
const user = await User.findOneAndDelete({ email: 'john@example.com' });

// Find by ID and delete
const user = await User.findByIdAndDelete(id);

// Delete using instance
const user = await User.findById(id);
await user.deleteOne();
```

---

## Queries

### Query Operators
```javascript
// Comparison
await User.find({ age: { $gt: 18 } });        // Greater than
await User.find({ age: { $gte: 18 } });       // Greater than or equal
await User.find({ age: { $lt: 65 } });        // Less than
await User.find({ age: { $lte: 65 } });       // Less than or equal
await User.find({ age: { $ne: 30 } });        // Not equal
await User.find({ age: { $in: [20, 25, 30] } });   // In array
await User.find({ age: { $nin: [20, 25, 30] } });  // Not in array

// Logical
await User.find({
  $and: [
    { age: { $gte: 18 } },
    { age: { $lte: 65 } }
  ]
});

await User.find({
  $or: [
    { email: 'john@example.com' },
    { name: 'John' }
  ]
});

await User.find({ age: { $not: { $lt: 18 } } });

// Element
await User.find({ middleName: { $exists: true } });
await User.find({ age: { $type: 'number' } });

// Array
await User.find({ tags: { $all: ['javascript', 'nodejs'] } });
await User.find({ tags: { $size: 3 } });
await User.find({ 'tags.0': 'javascript' });  // First element

// String
await User.find({ name: /john/i });  // Regex (case-insensitive)
await User.find({ name: { $regex: 'john', $options: 'i' } });
```

### Query Chaining
```javascript
const users = await User
  .find({ age: { $gte: 18 } })
  .where('isActive').equals(true)
  .where('age').gt(18).lt(65)
  .select('name email')
  .sort({ createdAt: -1 })
  .limit(10)
  .skip(20)
  .exec();
```

### Lean Queries
```javascript
// Returns plain JavaScript objects (faster, no Mongoose features)
const users = await User.find().lean();

// With lean, you can't use:
// - Instance methods
// - Virtuals
// - Getters/setters
// - save() method
```

---

## Validation

### Built-in Validators
```javascript
const userSchema = new Schema({
  name: {
    type: String,
    required: true,
    minlength: 3,
    maxlength: 50,
    trim: true,
  },
  email: {
    type: String,
    required: [true, 'Email is required'],
    unique: true,
    lowercase: true,
  },
  age: {
    type: Number,
    min: [18, 'Must be at least 18'],
    max: 100,
  },
  role: {
    type: String,
    enum: ['user', 'admin', 'moderator'],
    default: 'user',
  },
});
```

### Custom Validators
```javascript
const userSchema = new Schema({
  email: {
    type: String,
    validate: {
      validator: function(v) {
        return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v);
      },
      message: props => `${props.value} is not a valid email!`
    },
  },
  password: {
    type: String,
    validate: {
      validator: function(v) {
        return v.length >= 8;
      },
      message: 'Password must be at least 8 characters long'
    },
  },
});

// Async validator
const userSchema = new Schema({
  email: {
    type: String,
    validate: {
      validator: async function(v) {
        const user = await mongoose.models.User.findOne({ email: v });
        return !user;
      },
      message: 'Email already exists'
    },
  },
});
```

---

## Middleware (Hooks)

### Pre Middleware
```javascript
// Pre save
userSchema.pre('save', async function(next) {
  // Hash password before saving
  if (this.isModified('password')) {
    this.password = await bcrypt.hash(this.password, 10);
  }
  next();
});

// Pre validate
userSchema.pre('validate', function(next) {
  console.log('Validating...');
  next();
});

// Pre remove
userSchema.pre('deleteOne', { document: true }, async function(next) {
  // Delete related documents
  await Post.deleteMany({ author: this._id });
  next();
});

// Pre find
userSchema.pre(/^find/, function(next) {
  this.populate('author');
  next();
});
```

### Post Middleware
```javascript
// Post save
userSchema.post('save', function(doc, next) {
  console.log('User saved:', doc.name);
  next();
});

// Post find
userSchema.post('find', function(docs, next) {
  console.log(`Found ${docs.length} users`);
  next();
});

// Error handling
userSchema.post('save', function(error, doc, next) {
  if (error.name === 'MongoServerError' && error.code === 11000) {
    next(new Error('Email already exists'));
  } else {
    next(error);
  }
});
```

---

## Population

### Basic Population
```javascript
const postSchema = new Schema({
  title: String,
  content: String,
  author: {
    type: Schema.Types.ObjectId,
    ref: 'User'
  },
});

const Post = mongoose.model('Post', postSchema);

// Populate author
const post = await Post.findById(id).populate('author');
console.log(post.author.name);  // Access author name

// Populate specific fields
const post = await Post.findById(id).populate('author', 'name email');

// Populate multiple fields
const post = await Post.findById(id)
  .populate('author')
  .populate('comments');

// Nested population
const post = await Post.findById(id)
  .populate({
    path: 'comments',
    populate: {
      path: 'author',
      select: 'name'
    }
  });
```

### Advanced Population
```javascript
// Conditional population
const posts = await Post.find()
  .populate({
    path: 'author',
    match: { isActive: true },
    select: 'name email',
    options: { sort: { name: 1 } }
  });

// Populate virtuals
const userSchema = new Schema({
  name: String,
});

userSchema.virtual('posts', {
  ref: 'Post',
  localField: '_id',
  foreignField: 'author',
});

const user = await User.findById(id).populate('posts');
```

---

## Virtuals

### Virtual Properties
```javascript
const userSchema = new Schema({
  firstName: String,
  lastName: String,
});

// Virtual getter
userSchema.virtual('fullName').get(function() {
  return `${this.firstName} ${this.lastName}`;
});

// Virtual setter
userSchema.virtual('fullName').set(function(v) {
  const parts = v.split(' ');
  this.firstName = parts[0];
  this.lastName = parts[1];
});

// Usage
const user = new User({ firstName: 'John', lastName: 'Doe' });
console.log(user.fullName);  // 'John Doe'

user.fullName = 'Jane Smith';
console.log(user.firstName);  // 'Jane'

// Include virtuals in JSON
userSchema.set('toJSON', { virtuals: true });
userSchema.set('toObject', { virtuals: true });
```

---

## Indexes

### Creating Indexes
```javascript
const userSchema = new Schema({
  email: {
    type: String,
    unique: true,      // Creates unique index
    index: true,       // Creates regular index
  },
  name: {
    type: String,
    index: true,
  },
});

// Compound index
userSchema.index({ email: 1, name: 1 });

// Text index
userSchema.index({ name: 'text', bio: 'text' });

// 2dsphere index (for geospatial queries)
userSchema.index({ location: '2dsphere' });

// Create indexes
await User.createIndexes();

// List indexes
const indexes = await User.collection.getIndexes();
```

---

## Plugins

### Creating Plugins
```javascript
// plugin.js
function timestampPlugin(schema) {
  schema.add({
    createdAt: Date,
    updatedAt: Date,
  });
  
  schema.pre('save', function(next) {
    if (this.isNew) {
      this.createdAt = new Date();
    }
    this.updatedAt = new Date();
    next();
  });
}

module.exports = timestampPlugin;

// Usage
const timestampPlugin = require('./plugin');
userSchema.plugin(timestampPlugin);

// Global plugin
mongoose.plugin(timestampPlugin);
```

---

## Transactions

### Using Transactions
```javascript
const session = await mongoose.startSession();
session.startTransaction();

try {
  const user = await User.create([{
    name: 'John',
    email: 'john@example.com'
  }], { session });
  
  const post = await Post.create([{
    title: 'My Post',
    author: user[0]._id
  }], { session });
  
  await session.commitTransaction();
  console.log('Transaction successful');
} catch (error) {
  await session.abortTransaction();
  console.error('Transaction failed:', error);
} finally {
  session.endSession();
}
```

---

## Aggregation

### Aggregation Pipeline
```javascript
// Match, group, sort
const results = await User.aggregate([
  { $match: { isActive: true } },
  { $group: {
    _id: '$age',
    count: { $sum: 1 },
    avgAge: { $avg: '$age' }
  }},
  { $sort: { count: -1 } },
  { $limit: 10 }
]);

// Lookup (join)
const results = await Post.aggregate([
  {
    $lookup: {
      from: 'users',
      localField: 'author',
      foreignField: '_id',
      as: 'authorDetails'
    }
  },
  { $unwind: '$authorDetails' },
  { $project: {
    title: 1,
    'authorDetails.name': 1,
    'authorDetails.email': 1
  }}
]);
```

---

## Best Practices

### Schema Design
```javascript
// ✅ Good: Use timestamps option
const schema = new Schema({}, { timestamps: true });

// ✅ Good: Use lean for read-only queries
const users = await User.find().lean();

// ✅ Good: Use select to limit fields
const users = await User.find().select('name email');

// ✅ Good: Index frequently queried fields
schema.index({ email: 1, createdAt: -1 });
```

### Error Handling
```javascript
// ✅ Good: Handle validation errors
try {
  await user.save();
} catch (error) {
  if (error.name === 'ValidationError') {
    console.error('Validation failed:', error.message);
  } else if (error.code === 11000) {
    console.error('Duplicate key error');
  } else {
    console.error('Error:', error);
  }
}
```

### Performance
```javascript
// ✅ Good: Use select to limit returned fields
const users = await User.find().select('name email');

// ✅ Good: Use lean for better performance
const users = await User.find().lean();

// ✅ Good: Use pagination
const page = 1;
const limit = 10;
const users = await User.find()
  .limit(limit)
  .skip((page - 1) * limit);

// ✅ Good: Create indexes for frequently queried fields
schema.index({ email: 1 });
```

---

**Pro Tip:** Use schemas with proper validation, leverage middleware for common operations, use indexes for better query performance, and always use lean() for read-only operations!