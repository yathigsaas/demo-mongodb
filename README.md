# MongoDB: The Complete Guide

## Table of Contents
- [Introduction](#introduction)
- [Installation](#installation)
  - [Windows](#windows)
  - [Ubuntu/Debian](#ubuntudebian)
  - [macOS](#macos)
- [Basic Setup](#basic-setup)
- [Core MongoDB Commands](#core-mongodb-commands)
  - [Database Operations](#database-operations)
  - [Collection Operations](#collection-operations)
  - [Document Operations](#document-operations)
  - [Querying Documents](#querying-documents)
  - [Updating Documents](#updating-documents)
  - [Deleting Documents](#deleting-documents)
- [Indexes and Search](#indexes-and-search)
- [Relationships](#relationships)
- [Import/Export Data](#importexport-data)
- [Useful Tools](#useful-tools)
- [Best Practices](#best-practices)
- [Conclusion](#conclusion)

## Introduction

MongoDB is a powerful, open-source NoSQL database that provides high performance, high availability, and automatic scaling. Unlike traditional relational databases, MongoDB uses a document-based data model that makes it flexible and scalable for modern application development.

### Why Use MongoDB?
- **Flexible Schema**: Documents can vary in structure
- **Scalability**: Horizontal scaling through sharding
- **High Performance**: Indexing and embedded documents for faster queries
- **Rich Query Language**: Powerful querying and data aggregation
- **High Availability**: Built-in replication and failover

## Installation

### Windows
1. Download the MongoDB Community Server MSI from [MongoDB's official website](https://www.mongodb.com/try/download/community)
2. Run the installer and follow the installation wizard
3. Add MongoDB's `bin` directory to your system's PATH
4. Create the data directory: `C:\data\db`

### Ubuntu/Debian
```bash
# Import the public key
wget -qO - https://www.mongodb.org/static/pgp/server-7.0.asc | sudo apt-key add -

# Create list file
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu $(lsb_release -cs)/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

# Update packages
sudo apt-get update

# Install MongoDB
sudo apt-get install -y mongodb-org

# Start MongoDB
sudo systemctl start mongod

# Enable MongoDB on system startup
sudo systemctl enable mongod
```

### macOS
```bash
# Install using Homebrew
brew tap mongodb/brew
brew install mongodb-community

# Start MongoDB
brew services start mongodb-community
```

## Basic Setup

### Start MongoDB Service
- **Windows**: Run `mongod` in Command Prompt (Run as Administrator)
- **Linux/macOS**: `sudo systemctl start mongod` or `brew services start mongodb-community`

### Connect with mongosh
```bash
mongosh
```

### Check MongoDB Status
```bash
# In mongosh
db.serverStatus()

# Or from terminal
mongo --eval 'db.runCommand({ connectionStatus: 1 })'
```

## Core MongoDB Commands

### Database Operations
```javascript
// Show all databases
show dbs

// Create/switch to database
use mydb

// Check current database
db

// Drop current database
db.dropDatabase()
```

### Collection Operations
```javascript
// Create a collection
db.createCollection('users')

// Show collections
show collections

// Drop a collection
db.users.drop()
```

### Document Operations
```javascript
// Insert one document
db.users.insertOne({
  name: 'John Doe',
  email: 'john@example.com',
  age: 30,
  status: 'active'
})

// Insert multiple documents
db.users.insertMany([
  { name: 'Jane Doe', email: 'jane@example.com', age: 28 },
  { name: 'Bob Smith', email: 'bob@example.com', age: 35 }
])
```

### Querying Documents
```javascript
// Find all documents
db.users.find()

// Find with filter
db.users.find({ age: { $gt: 30 } })

// Find one document
db.users.findOne({ name: 'John Doe' })

// Projection (select specific fields)
db.users.find({}, { name: 1, email: 1 })

// Sort results
db.users.find().sort({ age: -1 })

// Limit results
db.users.find().limit(5)
```

### Updating Documents
```javascript
// Update one document
db.users.updateOne(
  { name: 'John Doe' },
  { $set: { age: 31 } }
)

// Update multiple documents
db.users.updateMany(
  { status: 'active' },
  { $set: { lastLogin: new Date() } }
)

// Increment a field
db.users.updateOne(
  { name: 'John Doe' },
  { $inc: { loginCount: 1 } }
)
```

### Deleting Documents
```javascript
// Delete one document
db.users.deleteOne({ name: 'John Doe' })

// Delete multiple documents
db.users.deleteMany({ status: 'inactive' })
```

## Indexes and Search

### Creating Indexes
```javascript
// Create a single field index
db.users.createIndex({ email: 1 })

// Create a compound index
db.users.createIndex({ name: 1, age: -1 })

// Create a text index for text search
db.articles.createIndex({ content: 'text' })

// Text search
db.articles.find({ $text: { $search: 'mongodb guide' } })
```

## Relationships

### Embedded Documents
```javascript
// Embedding documents
db.users.insertOne({
  name: 'John Doe',
  address: {
    street: '123 Main St',
    city: 'New York',
    country: 'USA'
  }
})
```

### Document References
```javascript
// Using references
// In users collection
db.users.insertOne({
  _id: 1,
  name: 'John Doe',
  posts: [101, 102]
})

// In posts collection
db.posts.insertMany([
  { _id: 101, title: 'First Post', content: '...' },
  { _id: 102, title: 'Second Post', content: '...' }
])
```

## Import/Export Data

### Export Data (mongoexport)
```bash
mongoexport --db=mydb --collection=users --out=users.json
```

### Import Data (mongoimport)
```bash
mongoimport --db=mydb --collection=users --file=users.json
```

## Useful Tools

### MongoDB Compass
MongoDB Compass is a GUI for MongoDB that provides:
- Schema visualization
- Query building
- Index management
- Real-time performance metrics

### MongoDB Atlas
MongoDB Atlas is a fully-managed cloud database service that handles:
- Deployment
- Scaling
- Backup
- Security

## Best Practices

### Naming Conventions
- Use lowercase collection names (e.g., `users`, `order_items`)
- Use camelCase for field names (e.g., `firstName`, `lastLogin`)
- Use `_id` as the primary key

### Schema Design
- Embed related data that is frequently accessed together
- Reference documents when data is not always needed
- Consider read/write patterns when designing schemas

### Security
- Enable authentication
- Use role-based access control (RBAC)
- Encrypt sensitive data
- Keep MongoDB updated
- Use firewalls and network security groups

## Conclusion

MongoDB is a powerful NoSQL database that offers flexibility, scalability, and performance for modern applications. This guide covers the basics to get you started, but there's much more to explore.

### Official Documentation
- [MongoDB Manual](https://docs.mongodb.com/manual/)
- [MongoDB University](https://university.mongodb.com/)
- [MongoDB Drivers](https://docs.mongodb.com/drivers/)

### Next Steps
- Explore aggregation framework
- Learn about transactions
- Set up replication and sharding
- Try MongoDB Atlas for a managed solution

Happy coding with MongoDB! 🚀