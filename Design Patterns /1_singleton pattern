# SINGLETON PATTERN - COMPREHENSIVE GUIDE

---

## 📌 DEFINITION

**Singleton** is a creational pattern that **restricts a class to have only ONE instance** and provides a **global point of access** to that instance.

### Core Concept
```
Problem: Need single shared instance across entire application
Solution: Prevent creating multiple instances, provide global accessor
```

---

## 🎯 WHEN TO USE

### ✅ Use Singleton When:
- **Only one instance should exist** (database connection pool)
- **Need global access point** (configuration manager)
- **Expensive to create** (logger, cache)
- **Lazy initialization** (on first access)
- **State is shared** globally (session manager)

### ❌ Don't Use When:
- Multiple instances needed (user objects)
- Stateless utility functions (math operations)
- Need dependency injection (testing, modularity)
- Frequent instantiation (one-time use objects)
- Can use static methods instead

---

## 🔧 HOW IT WORKS

### Implementation Steps:
1. **Private constructor** - prevent `new Instance()`
2. **Static instance** - hold single reference
3. **Static getter** - return instance, create if needed
4. **Lazy loading** - create on first access (optional)

---

## 💻 CODE EXAMPLES

### ❌ WRONG - No Singleton (Creates Multiple Instances)
```javascript
class Database {
  constructor() {
    console.log('Creating Database connection...');
    this.connection = Math.random(); // unique connection each time
  }
  
  query(sql) {
    return `Query on connection ${this.connection}: ${sql}`;
  }
}

// PROBLEM: Multiple instances created!
const db1 = new Database();
const db2 = new Database();
const db3 = new Database();

console.log(db1.connection === db2.connection); // false - different connections!
// Memory waste, multiple DB connections, state sync issues
```

---

### ✅ CORRECT - Singleton Pattern

**Method 1: Class-based with Static Method**
```javascript
class Database {
  // Step 1: Private constructor - prevent 'new'
  constructor() {
    console.log('Creating Database connection...');
    this.connection = Math.random();
    this.queries = [];
  }
  
  // Step 2: Static instance holder
  static #instance = null;
  
  // Step 3: Static getter with lazy loading
  static getInstance() {
    if (Database.#instance === null) {
      Database.#instance = new Database(); // Create only once
    }
    return Database.#instance;
  }
  
  query(sql) {
    this.queries.push(sql);
    return `Query on connection ${this.connection}: ${sql}`;
  }
  
  getQueryHistory() {
    return this.queries;
  }
}

// CORRECT: Same instance every time
const db1 = Database.getInstance();
const db2 = Database.getInstance();
const db3 = Database.getInstance();

console.log(db1 === db2 && db2 === db3); // true - same instance!
console.log(db1.connection === db2.connection); // true - same connection!

// All queries logged in same instance
db1.query('SELECT * FROM users');
db2.query('SELECT * FROM posts');
console.log(db1.getQueryHistory()); // both queries in same object
```

---

**Method 2: Module Pattern (Node.js - Most Common)**
```javascript
// database.js - Module-level singleton
let instance = null;

class Database {
  constructor() {
    console.log('Creating Database connection...');
    this.connection = Math.random();
    this.pool = [];
  }
  
  connect(connectionString) {
    console.log(`Connected to: ${connectionString}`);
    return this.connection;
  }
  
  getConnection() {
    return this.connection;
  }
}

// SINGLETON PATTERN: Export function that returns same instance
module.exports = {
  getInstance: () => {
    if (!instance) {
      instance = new Database();
    }
    return instance;
  }
};

// ===== USAGE =====
// In file 1:
const db = require('./database').getInstance();
db.connect('mongodb://localhost:27017');

// In file 2:
const db2 = require('./database').getInstance();
console.log(db2.getConnection() === db.getConnection()); // true!
```

---

**Method 3: IIFE Pattern (Immediately Invoked Function Expression)**
```javascript
const ConfigurationManager = (() => {
  let instance;
  
  return {
    getInstance: () => {
      if (!instance) {
        instance = {
          config: {},
          get: (key) => instance.config[key],
          set: (key, value) => {
            instance.config[key] = value;
          },
          getAll: () => instance.config
        };
      }
      return instance;
    }
  };
})();

// Usage
const config1 = ConfigurationManager.getInstance();
config1.set('apiUrl', 'https://api.example.com');
config1.set('timeout', 5000);

const config2 = ConfigurationManager.getInstance();
console.log(config2.get('apiUrl')); // https://api.example.com
console.log(config1 === config2); // true
```

---

**Method 4: Modern ES6 with Private Fields**
```javascript
class Logger {
  static #instance = null;
  #logs = []; // Private field
  
  private constructor() {
    console.log('Logger initialized');
  }
  
  static getInstance() {
    if (!Logger.#instance) {
      Logger.#instance = new Logger();
    }
    return Logger.#instance;
  }
  
  log(message) {
    const timestamp = new Date().toISOString();
    this.#logs.push({ timestamp, message });
    console.log(`[${timestamp}] ${message}`);
  }
  
  getLogs() {
    return [...this.#logs]; // Return copy
  }
}

const logger1 = Logger.getInstance();
const logger2 = Logger.getInstance();
logger1.log('Application started');
logger2.log('User logged in');
console.log(logger1 === logger2); // true
```

---

## 🌍 REAL-TIME USAGE (PRODUCTION EXAMPLES)

### Example 1: Database Connection Pool (Very Common)
```javascript
// src/database/connection.js
class DatabaseConnection {
  static #instance = null;
  
  constructor() {
    console.log('Initializing database connection pool...');
    this.pool = [];
    this.maxConnections = 10;
    this.activeConnections = 0;
  }
  
  static getInstance() {
    if (!DatabaseConnection.#instance) {
      DatabaseConnection.#instance = new DatabaseConnection();
    }
    return DatabaseConnection.#instance;
  }
  
  getConnection() {
    if (this.pool.length > 0) {
      return this.pool.pop(); // Reuse existing
    }
    if (this.activeConnections < this.maxConnections) {
      this.activeConnections++;
      return { id: Math.random(), status: 'new' };
    }
    throw new Error('No connections available');
  }
  
  releaseConnection(conn) {
    this.pool.push(conn);
  }
}

// Usage across entire app:
const db = DatabaseConnection.getInstance();
const conn1 = db.getConnection();
const conn2 = DatabaseConnection.getInstance().getConnection();
// conn1 and conn2 share the SAME pool!
```

---

### Example 2: Logger (Netflix, Stripe, Airbnb Use)
```javascript
// Logger ensures all logs go to same place
class Logger {
  static #instance = null;
  
  constructor() {
    this.logs = [];
    this.level = 'info'; // debug, info, warn, error
  }
  
  static getInstance() {
    if (!Logger.#instance) {
      Logger.#instance = new Logger();
    }
    return Logger.#instance;
  }
  
  info(msg, data) {
    this.logs.push({ level: 'info', msg, data, time: Date.now() });
    console.log(`[INFO] ${msg}`, data);
  }
  
  error(msg, error) {
    this.logs.push({ level: 'error', msg, error, time: Date.now() });
    console.error(`[ERROR] ${msg}`, error);
  }
  
  getLogs() {
    return this.logs;
  }
}

// Across multiple files:
// file1.js
Logger.getInstance().info('User signup', { userId: 123 });

// file2.js
Logger.getInstance().info('Order placed', { orderId: 456 });

// All logs in SAME instance!
console.log(Logger.getInstance().getLogs().length); // 2 logs total
```

---

### Example 3: Configuration Manager (Production Systems)
```javascript
class AppConfig {
  static #instance = null;
  
  constructor() {
    this.config = {
      environment: process.env.NODE_ENV || 'development',
      apiUrl: process.env.API_URL || 'http://localhost:3000',
      dbUrl: process.env.DB_URL || 'mongodb://localhost',
      port: process.env.PORT || 3000,
      features: {
        authentication: true,
        paymentProcessing: true,
        analytics: process.env.NODE_ENV === 'production'
      }
    };
  }
  
  static getInstance() {
    if (!AppConfig.#instance) {
      AppConfig.#instance = new AppConfig();
    }
    return AppConfig.#instance;
  }
  
  get(key) {
    return key.split('.').reduce((obj, k) => obj?.[k], this.config);
  }
  
  set(key, value) {
    const keys = key.split('.');
    let obj = this.config;
    for (let i = 0; i < keys.length - 1; i++) {
      obj = obj[keys[i]];
    }
    obj[keys[keys.length - 1]] = value;
  }
}

// Usage
const config = AppConfig.getInstance();
console.log(config.get('database.url'));
config.set('features.analytics', true);
```

---

### Example 4: Session Manager (Web Applications)
```javascript
class SessionManager {
  static #instance = null;
  
  constructor() {
    this.sessions = new Map();
  }
  
  static getInstance() {
    if (!SessionManager.#instance) {
      SessionManager.#instance = new SessionManager();
    }
    return SessionManager.#instance;
  }
  
  createSession(userId) {
    const sessionId = Math.random().toString(36);
    this.sessions.set(sessionId, {
      userId,
      createdAt: Date.now(),
      lastAccessed: Date.now()
    });
    return sessionId;
  }
  
  getSession(sessionId) {
    return this.sessions.get(sessionId);
  }
  
  destroySession(sessionId) {
    this.sessions.delete(sessionId);
  }
}

// All requests use same session store
app.post('/login', (req, res) => {
  const sessionId = SessionManager.getInstance().createSession(req.body.userId);
  res.json({ sessionId });
});

app.get('/profile', (req, res) => {
  const session = SessionManager.getInstance().getSession(req.headers.sessionId);
  // Same instance across all requests!
});
```

---

## 📊 DIAGRAM

```mermaid
graph TD
    A["Client Code"] -->|getInstance| B["Singleton Class"]
    B -->|Check #instance| C{instance == null?}
    C -->|Yes| D["Create new instance"]
    C -->|No| E["Return existing"]
    D --> F["Store in #instance"]
    F --> E
    E --> G["Return instance to Client"]
    
    H["Client Code 2"] -->|getInstance| B
    B --> E
    E --> I["Same instance returned!"]
    
    style B fill:#90EE90
    style F fill:#FFB6C1
    style I fill:#87CEEB
```

---

## ⚖️ PROS & CONS

| Aspect | Pros | Cons |
|--------|------|------|
| **Memory** | Uses less memory (one instance) | All state in one place |
| **Performance** | Lazy initialization | Thread safety issues (multithreading) |
| **Access** | Global point of access | Hidden dependency on singleton |
| **Testing** | Reusable (but problematic for tests) | Hard to mock for unit tests |
| **State** | Shared state across app | State pollution, side effects |
| **Thread Safety** | Works well with event-driven (Node.js) | Issues with multithreading |

---

## 🎤 TOP INTERVIEW QUESTIONS & ANSWERS

### Q1: "What is Singleton and why use it?"
**Answer:**
"Singleton is a pattern that ensures a class has only one instance with a global access point. We use it when:
- Need exactly one instance (database connections)
- Want to control creation (lazy loading)
- Need shared state across the app (configuration, logger)
- Want to prevent multiple expensive resources

Example: Database connection pool - one pool serves entire app."

---

### Q2: "How is Singleton different from static methods?"
**Answer:**
"Static methods are utility functions with no state. Singleton is a class with ONE INSTANCE and STATE:

```javascript
// Static - no state, utility
class Math {
  static add(a, b) { return a + b; } // No instance created
}

// Singleton - ONE instance, HAS state
class Logger {
  static #instance = null;
  logs = []; // STATE
  
  static getInstance() {
    if (!Logger.#instance) Logger.#instance = new Logger();
    return Logger.#instance;
  }
}
```

Singleton maintains state across calls, static doesn't."

---

### Q3: "What are issues with Singleton and how to fix them?"
**Answer:**
"Issues:
1. **Testing**: Hard to mock - FIX: Use dependency injection, interface
2. **Threading**: Not thread-safe - FIX: Eager initialization or synchronized
3. **Hidden dependency**: Tight coupling - FIX: Dependency injection
4. **State pollution**: All state in one object - FIX: Use composition

For testing:
```javascript
// Instead of getInstance() everywhere
class Logger {
  // Accept instance via constructor (DI)
  constructor(loggerImpl = Logger.getInstance()) {
    this.logger = loggerImpl;
  }
}
```"

---

### Q4: "Should we use Singleton for everything?"
**Answer:**
"NO! Only use when:
- Single instance truly needed
- Shared state is intentional
- Performance/resource critical

DON'T use for:
- Domain objects (User, Post) - need multiple
- Stateless utilities - use static/functions
- When testing matters - use dependency injection
- When you need flexibility - use factory + DI

Overusing creates 'Singleton Hell' - tight coupling, hard to test."

---

### Q5: "How to make Singleton thread-safe?"
**Answer:**
"In Node.js (single-threaded), module-level singleton IS thread-safe:

```javascript
// This IS thread-safe in Node.js
let instance = null;
module.exports = {
  getInstance: () => {
    if (!instance) instance = new Database();
    return instance;
  }
};
```

In multithreaded languages (Java, C#):
```java
// Double-checked locking (Java)
public class Singleton {
  private static volatile Singleton instance;
  
  public static Singleton getInstance() {
    if (instance == null) {
      synchronized (Singleton.class) {
        if (instance == null) {
          instance = new Singleton();
        }
      }
    }
    return instance;
  }
}
```

Node.js doesn't need this because it's single-threaded!"

---

## 🚨 ANTI-PATTERNS & MISTAKES

### ❌ Mistake 1: Using Singleton for Domain Objects
```javascript
// WRONG - creates Singleton Hell
class User {
  static #instance = null;
  static getInstance() {
    if (!User.#instance) User.#instance = new User();
    return User.#instance;
  }
}

// Need multiple users but only have one!
const user1 = User.getInstance();
const user2 = User.getInstance(); // Same as user1!
```

---

### ❌ Mistake 2: Not Making Constructor Private
```javascript
// WRONG - anyone can call new
class Database {
  static #instance = null;
  
  constructor() { // PUBLIC - anyone can call
    this.connection = Math.random();
  }
  
  static getInstance() {
    if (!Database.#instance) {
      Database.#instance = new Database();
    }
    return Database.#instance;
  }
}

const db = new Database(); // Oops! Created extra instance!
```

---

### ❌ Mistake 3: Too Much State in Singleton
```javascript
// WRONG - singleton grows uncontrollably
class AppState {
  constructor() {
    this.users = [];
    this.posts = [];
    this.comments = [];
    this.notifications = [];
    this.cache = {};
    this.logs = [];
    // ... 50 more properties
  }
}

// Result: God Object, hard to test, tight coupling
```

---

## 🎯 REAL-WORLD SCENARIO

### Scenario: E-commerce Platform (Stripe-like)
```javascript
// Logger - ensure all payment logs in one place
class PaymentLogger {
  static #instance = null;
  
  constructor() {
    this.logs = [];
  }
  
  static getInstance() {
    if (!PaymentLogger.#instance) {
      PaymentLogger.#instance = new PaymentLogger();
    }
    return PaymentLogger.#instance;
  }
  
  logTransaction(txn) {
    this.logs.push({
      ...txn,
      timestamp: Date.now()
    });
  }
  
  generateReport() {
    return {
      totalTransactions: this.logs.length,
      totalAmount: this.logs.reduce((sum, t) => sum + t.amount, 0)
    };
  }
}

// DatabaseConnection - one pool serves all requests
class DB {
  static #instance = null;
  
  constructor() {
    this.pool = Array.from({ length: 5 }, (_, i) => ({ id: i, inUse: false }));
  }
  
  static getInstance() {
    if (!DB.#instance) DB.#instance = new DB();
    return DB.#instance;
  }
  
  getConnection() {
    return this.pool.find(c => !c.inUse);
  }
  
  releaseConnection(conn) {
    conn.inUse = false;
  }
}

// Usage across multiple payment handlers
function processPayment(paymentData) {
  const logger = PaymentLogger.getInstance();
  const db = DB.getInstance();
  
  logger.logTransaction({ type: 'payment_start', ...paymentData });
  
  const conn = db.getConnection();
  // ... process payment
  db.releaseConnection(conn);
  
  logger.logTransaction({ type: 'payment_complete', ...paymentData });
}

function generatePaymentReport() {
  return PaymentLogger.getInstance().generateReport();
}
```

---

## ✅ KEY TAKEAWAYS

1. **Use Singleton** when you need exactly ONE instance
2. **Private constructor** prevents multiple instances
3. **Lazy initialization** creates on first access
4. **Common uses**: DB, Logger, Config, Cache, Session
5. **Avoid** for domain objects and when testing matters
6. **Node.js** module pattern IS thread-safe singleton
7. **Trade-off**: Convenience vs testability/coupling

---

## 📚 NEXT PATTERN

Ready to learn **Factory Pattern**? Go to `02_CREATIONAL_FACTORY.md`

Pattern creates objects without specifying exact classes (more flexible than Singleton).

