# BUILDER PATTERN - COMPREHENSIVE GUIDE

---

## 📌 DEFINITION

**Builder** is a creational pattern that **constructs complex objects step-by-step** instead of all-at-once. Separates object construction from representation, allowing same construction process to create different representations.

### Core Concept
```
Problem: Complex objects with many parameters, optional fields, validation
Solution: Build object step-by-step with builder method calls
Benefits: Readable code, optional parameters, validation, immutability
```

---

## 🎯 WHEN TO USE

### ✅ Use Builder When:
- **Complex objects** with many parameters (10+)
- **Many optional parameters** (some needed, some not)
- **Multiple representation** of same data
- **Validation logic** before object creation
- **Immutable objects** with fluent API
- **Want readable code** instead of massive constructor

### ❌ Don't Use When:
- **Simple objects** (2-3 parameters) - use constructor
- **All parameters required** - use constructor
- **No optional fields** - overkill
- **Performance critical** - builder adds overhead

---

## 🔧 HOW IT WORKS

### Implementation Steps:
1. **Create builder class** - has all parameters as fields
2. **Builder methods** - set one or more parameters, return `this`
3. **build() method** - creates final object with validation
4. **Make original class immutable** - private constructor

---

## 💻 CODE EXAMPLES

### ❌ WRONG - Constructor Hell

```javascript
// PROBLEM: Too many parameters, unclear what's required
class User {
  constructor(id, email, password, name, age, phone, address, country, 
              subscriptionLevel, notificationsEnabled, darkMode, language, 
              twoFactorEnabled, verificationStatus) {
    this.id = id;
    this.email = email;
    this.password = password;
    this.name = name;
    this.age = age;
    this.phone = phone;
    this.address = address;
    this.country = country;
    this.subscriptionLevel = subscriptionLevel;
    this.notificationsEnabled = notificationsEnabled;
    this.darkMode = darkMode;
    this.language = language;
    this.twoFactorEnabled = twoFactorEnabled;
    this.verificationStatus = verificationStatus;
  }
}

// PROBLEM: Confusing what each parameter is
const user = new User(1, 'john@email.com', 'pass123', 'John', 30, 
                      '+1234567890', '123 Main St', 'USA', 'premium', 
                      true, false, 'en', true, 'verified');

// What is each parameter? Unmaintainable!
// What if add new parameter? Break all existing code!

// PROBLEM: Optional parameters
// What if user doesn't have phone? Pass null?
const user2 = new User(2, 'jane@email.com', 'pass456', 'Jane', null, 
                       null, null, 'USA', 'basic', 
                       true, true, 'fr', false, 'pending');
```

---

### ✅ CORRECT - Builder Pattern

**Basic Builder**
```javascript
// Step 1: Object class - immutable, no validation
class User {
  constructor(builder) {
    this.id = builder.id;
    this.email = builder.email;
    this.password = builder.password;
    this.name = builder.name;
    this.age = builder.age;
    this.phone = builder.phone;
    this.address = builder.address;
    this.country = builder.country;
    this.subscriptionLevel = builder.subscriptionLevel;
    this.notificationsEnabled = builder.notificationsEnabled;
    this.darkMode = builder.darkMode;
    this.language = builder.language;
    this.twoFactorEnabled = builder.twoFactorEnabled;
    this.verificationStatus = builder.verificationStatus;
  }
}

// Step 2: Builder class - fluent interface
class UserBuilder {
  constructor(id, email, password) {
    // Required fields in constructor
    this.id = id;
    this.email = email;
    this.password = password;
    
    // Optional fields with defaults
    this.name = null;
    this.age = null;
    this.phone = null;
    this.address = null;
    this.country = 'USA';
    this.subscriptionLevel = 'basic';
    this.notificationsEnabled = true;
    this.darkMode = false;
    this.language = 'en';
    this.twoFactorEnabled = false;
    this.verificationStatus = 'pending';
  }
  
  // Setter methods return 'this' for chaining
  setName(name) {
    this.name = name;
    return this;
  }
  
  setAge(age) {
    this.age = age;
    return this;
  }
  
  setPhone(phone) {
    this.phone = phone;
    return this;
  }
  
  setAddress(address) {
    this.address = address;
    return this;
  }
  
  setCountry(country) {
    this.country = country;
    return this;
  }
  
  setSubscriptionLevel(level) {
    this.subscriptionLevel = level;
    return this;
  }
  
  setNotificationsEnabled(enabled) {
    this.notificationsEnabled = enabled;
    return this;
  }
  
  setDarkMode(enabled) {
    this.darkMode = enabled;
    return this;
  }
  
  setLanguage(lang) {
    this.language = lang;
    return this;
  }
  
  setTwoFactorEnabled(enabled) {
    this.twoFactorEnabled = enabled;
    return this;
  }
  
  setVerificationStatus(status) {
    this.verificationStatus = status;
    return this;
  }
  
  // Step 3: Build method with validation
  build() {
    // Validation logic
    if (!this.email.includes('@')) {
      throw new Error('Invalid email');
    }
    if (this.password.length < 8) {
      throw new Error('Password must be at least 8 characters');
    }
    if (this.age && this.age < 13) {
      throw new Error('Must be at least 13 years old');
    }
    if (!['basic', 'premium', 'enterprise'].includes(this.subscriptionLevel)) {
      throw new Error('Invalid subscription level');
    }
    
    return new User(this);
  }
}

// Step 4: Usage - CLEAR and FLEXIBLE
const user1 = new UserBuilder(1, 'john@email.com', 'securePass123')
  .setName('John')
  .setAge(30)
  .setCountry('USA')
  .setSubscriptionLevel('premium')
  .setTwoFactorEnabled(true)
  .build();

// Only set what you need
const user2 = new UserBuilder(2, 'jane@email.com', 'securePass456')
  .setName('Jane')
  .setLanguage('fr')
  .build();

// Clear and maintainable!
```

---

**Builder with Validation & Complex Logic**
```javascript
class Order {
  constructor(builder) {
    this.id = builder.id;
    this.items = builder.items;
    this.total = builder.total;
    this.tax = builder.tax;
    this.shipping = builder.shipping;
    this.discountCode = builder.discountCode;
    this.shippingAddress = builder.shippingAddress;
    this.billingAddress = builder.billingAddress;
    this.paymentMethod = builder.paymentMethod;
    this.customerNotes = builder.customerNotes;
  }
  
  getFinalPrice() {
    return this.total + this.tax + this.shipping;
  }
}

class OrderBuilder {
  constructor(id) {
    this.id = id;
    this.items = [];
    this.total = 0;
    this.tax = 0;
    this.shipping = 0;
    this.discountCode = null;
    this.shippingAddress = null;
    this.billingAddress = null;
    this.paymentMethod = null;
    this.customerNotes = null;
  }
  
  addItem(item) {
    if (!item.price || item.price <= 0) {
      throw new Error('Item price must be positive');
    }
    this.items.push(item);
    this.total += item.price * item.quantity;
    return this;
  }
  
  applyDiscountCode(code) {
    const discounts = {
      'SAVE10': 0.10,
      'SAVE20': 0.20,
      'SAVE50': 0.50
    };
    
    if (!discounts[code]) {
      throw new Error('Invalid discount code');
    }
    
    this.discountCode = code;
    this.total -= this.total * discounts[code];
    return this;
  }
  
  setShippingAddress(address) {
    this.shippingAddress = address;
    return this;
  }
  
  setBillingAddress(address) {
    this.billingAddress = address;
    return this;
  }
  
  setPaymentMethod(method) {
    const valid = ['credit_card', 'debit_card', 'paypal', 'apple_pay'];
    if (!valid.includes(method)) {
      throw new Error('Invalid payment method');
    }
    this.paymentMethod = method;
    return this;
  }
  
  setCustomerNotes(notes) {
    this.customerNotes = notes;
    return this;
  }
  
  calculateTax() {
    this.tax = this.total * 0.08; // 8% tax
    return this;
  }
  
  calculateShipping() {
    if (this.total > 100) {
      this.shipping = 0; // Free shipping
    } else if (this.total > 50) {
      this.shipping = 5;
    } else {
      this.shipping = 10;
    }
    return this;
  }
  
  build() {
    // Validation
    if (this.items.length === 0) {
      throw new Error('Order must have at least one item');
    }
    if (!this.shippingAddress) {
      throw new Error('Shipping address required');
    }
    if (!this.paymentMethod) {
      throw new Error('Payment method required');
    }
    
    // Auto-calculations
    this.calculateTax();
    this.calculateShipping();
    
    return new Order(this);
  }
}

// Usage
const order = new OrderBuilder('ORD123')
  .addItem({ name: 'Laptop', price: 1000, quantity: 1 })
  .addItem({ name: 'Mouse', price: 50, quantity: 2 })
  .applyDiscountCode('SAVE10')
  .setShippingAddress({ street: '123 Main St', city: 'NYC' })
  .setPaymentMethod('credit_card')
  .setCustomerNotes('Gift wrapping please')
  .build();

console.log(`Total: $${order.getFinalPrice().toFixed(2)}`);
```

---

**Static Builder Method (Modern Approach)**
```javascript
class QueryBuilder {
  constructor() {
    this.select = '*';
    this.from = null;
    this.where = [];
    this.orderBy = null;
    this.limit = null;
  }
  
  static query() {
    return new QueryBuilder();
  }
  
  select(fields) {
    this.select = fields;
    return this;
  }
  
  from(table) {
    this.from = table;
    return this;
  }
  
  where(condition) {
    this.where.push(condition);
    return this;
  }
  
  orderBy(field, direction = 'ASC') {
    this.orderBy = `${field} ${direction}`;
    return this;
  }
  
  limit(count) {
    this.limit = count;
    return this;
  }
  
  build() {
    if (!this.from) {
      throw new Error('FROM clause required');
    }
    
    let query = `SELECT ${this.select} FROM ${this.from}`;
    
    if (this.where.length > 0) {
      query += ` WHERE ${this.where.join(' AND ')}`;
    }
    
    if (this.orderBy) {
      query += ` ORDER BY ${this.orderBy}`;
    }
    
    if (this.limit) {
      query += ` LIMIT ${this.limit}`;
    }
    
    return query;
  }
}

// Usage
const sql = QueryBuilder.query()
  .select('id, name, email')
  .from('users')
  .where('age > 18')
  .where('country = "USA"')
  .orderBy('name', 'ASC')
  .limit(10)
  .build();

console.log(sql);
// SELECT id, name, email FROM users WHERE age > 18 AND country = "USA" ORDER BY name ASC LIMIT 10
```

---

## 🌍 REAL-TIME USAGE (PRODUCTION EXAMPLES)

### Example 1: HTML/DOM Builder (jQuery, Cheerio Pattern)
```javascript
class HTMLBuilder {
  constructor(tag) {
    this.tag = tag;
    this.attributes = {};
    this.children = [];
    this.content = null;
  }
  
  static div() {
    return new HTMLBuilder('div');
  }
  
  static form() {
    return new HTMLBuilder('form');
  }
  
  attr(key, value) {
    this.attributes[key] = value;
    return this;
  }
  
  addClass(className) {
    this.attributes.class = this.attributes.class ? 
      `${this.attributes.class} ${className}` : className;
    return this;
  }
  
  text(content) {
    this.content = content;
    return this;
  }
  
  append(child) {
    this.children.push(child);
    return this;
  }
  
  build() {
    let html = `<${this.tag}`;
    
    for (const [key, value] of Object.entries(this.attributes)) {
      html += ` ${key}="${value}"`;
    }
    
    html += '>';
    
    if (this.content) {
      html += this.content;
    }
    
    this.children.forEach(child => {
      html += typeof child === 'string' ? child : child.build();
    });
    
    html += `</${this.tag}>`;
    return html;
  }
}

// Usage
const form = HTMLBuilder.form()
  .attr('method', 'POST')
  .attr('action', '/submit')
  .append(
    new HTMLBuilder('input')
      .attr('type', 'text')
      .attr('name', 'username')
      .attr('placeholder', 'Username')
      .build()
  )
  .append(
    new HTMLBuilder('input')
      .attr('type', 'password')
      .attr('name', 'password')
      .attr('placeholder', 'Password')
      .build()
  )
  .build();

console.log(form);
```

---

### Example 2: API Request Builder (Axios Pattern)
```javascript
class HttpRequestBuilder {
  constructor() {
    this.method = 'GET';
    this.url = null;
    this.headers = { 'Content-Type': 'application/json' };
    this.body = null;
    this.params = {};
    this.timeout = 30000;
    this.retries = 0;
  }
  
  setUrl(url) {
    this.url = url;
    return this;
  }
  
  setMethod(method) {
    this.method = method.toUpperCase();
    return this;
  }
  
  setBody(body) {
    this.body = body;
    return this;
  }
  
  setHeader(key, value) {
    this.headers[key] = value;
    return this;
  }
  
  addParam(key, value) {
    this.params[key] = value;
    return this;
  }
  
  setTimeout(ms) {
    this.timeout = ms;
    return this;
  }
  
  setRetries(count) {
    this.retries = count;
    return this;
  }
  
  async send() {
    if (!this.url) throw new Error('URL required');
    
    const config = {
      method: this.method,
      url: this.url,
      headers: this.headers,
      timeout: this.timeout,
      params: this.params,
      data: this.body,
      maxRetries: this.retries
    };
    
    // Use axios or fetch with config
    return config;
  }
}

// Usage
const request = new HttpRequestBuilder()
  .setUrl('https://api.example.com/users')
  .setMethod('POST')
  .setHeader('Authorization', 'Bearer token123')
  .setBody({ name: 'John', email: 'john@email.com' })
  .setTimeout(10000)
  .setRetries(3)
  .send();
```

---

### Example 3: Database Query Builder (Knex.js Pattern)
```javascript
class DatabaseQueryBuilder {
  constructor(table) {
    this.table = table;
    this.selectColumns = ['*'];
    this.whereConditions = [];
    this.joinTables = [];
    this.orderByFields = [];
    this.limitCount = null;
    this.offsetCount = null;
  }
  
  select(...columns) {
    this.selectColumns = columns;
    return this;
  }
  
  where(column, operator, value) {
    this.whereConditions.push({ column, operator, value });
    return this;
  }
  
  join(table, onCondition) {
    this.joinTables.push({ table, onCondition });
    return this;
  }
  
  orderBy(column, direction = 'ASC') {
    this.orderByFields.push({ column, direction });
    return this;
  }
  
  limit(count) {
    this.limitCount = count;
    return this;
  }
  
  offset(count) {
    this.offsetCount = count;
    return this;
  }
  
  async execute() {
    // Build and execute query
    const query = this.buildQuery();
    console.log('Executing:', query);
    // return await database.query(query);
  }
  
  buildQuery() {
    let query = `SELECT ${this.selectColumns.join(', ')} FROM ${this.table}`;
    
    this.joinTables.forEach(j => {
      query += ` JOIN ${j.table} ON ${j.onCondition}`;
    });
    
    this.whereConditions.forEach((w, i) => {
      query += i === 0 ? ' WHERE ' : ' AND ';
      query += `${w.column} ${w.operator} '${w.value}'`;
    });
    
    this.orderByFields.forEach((o, i) => {
      if (i === 0) query += ' ORDER BY ';
      query += `${o.column} ${o.direction}`;
    });
    
    if (this.limitCount) query += ` LIMIT ${this.limitCount}`;
    if (this.offsetCount) query += ` OFFSET ${this.offsetCount}`;
    
    return query;
  }
}

// Usage
const result = new DatabaseQueryBuilder('users')
  .select('id', 'name', 'email')
  .where('age', '>', 18)
  .where('country', '=', 'USA')
  .join('orders', 'users.id = orders.user_id')
  .orderBy('name', 'ASC')
  .limit(10)
  .buildQuery();
```

---

## 📊 DIAGRAM

```mermaid
graph TD
    Client["Client Code<br/>new UserBuilder()"]
    Builder["UserBuilder<br/>setName().setAge()..."]
    
    Client -->|creates| Builder
    Builder -->|setName()| B1["builder.name = 'John'<br/>return this"]
    Builder -->|setAge()| B2["builder.age = 30<br/>return this"]
    Builder -->|setEmail()| B3["builder.email = 'john@...'<br/>return this"]
    Builder -->|build()| B4["Validate all fields<br/>return new User(this)"]
    
    B1 -->|chainable| Builder
    B2 -->|chainable| Builder
    B3 -->|chainable| Builder
    B4 --> Result["User Object<br/>Complete & Valid"]
    
    style Builder fill:#90EE90
    style B4 fill:#FFB6C1
    style Result fill:#87CEEB
```

---

## ⚖️ PROS & CONS

| Aspect | Pros | Cons |
|--------|------|------|
| **Readability** | Clear, chainable, readable | Extra code to write |
| **Flexibility** | Optional parameters easy | More classes needed |
| **Validation** | Central validation point | Overhead for simple objects |
| **Immutability** | Supports immutable objects | Still need to create builder |
| **Defaults** | Easy to set defaults | Can be verbose |
| **Maintenance** | Easy to add new fields | More code to maintain |

---

## 🎤 TOP INTERVIEW QUESTIONS & ANSWERS

### Q1: "When should you use Builder over constructor?"
**Answer:**
"Use Builder when:
- More than 4-5 parameters
- Many optional parameters
- Need validation logic
- Want immutable objects
- Improving code readability

Constructor for:
- Simple objects (2-3 params)
- All required parameters
- Performance critical

Example: User with 15 fields → Builder. Coordinates (x, y) → Constructor."

---

### Q2: "What's the difference between Builder and Factory?"
**Answer:**
"Builder: Constructs SINGLE COMPLEX object step-by-step
Factory: Creates DIFFERENT TYPES of objects

```javascript
// Factory - WHICH type?
const shape = ShapeFactory.create('circle'); // Creates Circle

// Builder - HOW to construct?
const user = new UserBuilder()
  .setName('John')
  .setAge(30)
  .build(); // Constructs User step-by-step
```"

---

### Q3: "How to handle required vs optional parameters?"
**Answer:**
"Required: Pass to builder constructor
Optional: Set via builder methods with defaults

```javascript
class UserBuilder {
  constructor(email) { // REQUIRED
    this.email = email; // Must provide
    this.name = null;   // OPTIONAL - defaults to null
    this.age = null;    // OPTIONAL
  }
  
  setName(name) {
    this.name = name;
    return this;
  }
  
  build() {
    if (!this.email) throw new Error('Email required');
    // Other validations
    return new User(this);
  }
}
```"

---

### Q4: "How does Builder support immutability?"
**Answer:**
"Make class private, only expose through builder:

```javascript
class User {
  // Private - can't call directly
  constructor(builder) {
    this.name = builder.name;
    this.email = builder.email;
    // Object is immutable after creation
    Object.freeze(this);
  }
}

// User created through builder only
const user = new UserBuilder()
  .setName('John')
  .build();

// Can't modify
user.name = 'Jane'; // Fails - frozen object!
```"

---

### Q5: "Difference between Builder and Fluent Interface?"
**Answer:**
"Builder: Specific pattern for complex object construction
Fluent Interface: General technique of chainable methods

Builder IS a type of fluent interface.

```javascript
// Fluent interface (chainable)
class Logger {
  log(msg) { console.log(msg); return this; }
  error(err) { console.error(err); return this; }
  info(msg) { console.info(msg); return this; }
}

logger.log('Start').error('Issue').info('Done');

// Builder (specific for construction)
new UserBuilder()
  .setName('John')
  .setAge(30)
  .build(); // Must call build() to finish
```"

---

## 🎯 REAL-WORLD SCENARIO

### Scenario: Configuration Manager (Express App Setup)
```javascript
class AppConfig {
  constructor(builder) {
    this.port = builder.port;
    this.host = builder.host;
    this.environment = builder.environment;
    this.database = builder.database;
    this.corsOrigins = builder.corsOrigins;
    this.jwt = builder.jwt;
    this.logging = builder.logging;
    this.sessionConfig = builder.sessionConfig;
  }
}

class AppConfigBuilder {
  constructor() {
    this.port = 3000;
    this.host = 'localhost';
    this.environment = 'development';
    this.database = null;
    this.corsOrigins = [];
    this.jwt = null;
    this.logging = {};
    this.sessionConfig = {};
  }
  
  setPort(port) {
    if (port < 1 || port > 65535) throw new Error('Invalid port');
    this.port = port;
    return this;
  }
  
  setHost(host) {
    this.host = host;
    return this;
  }
  
  setEnvironment(env) {
    if (!['development', 'staging', 'production'].includes(env)) {
      throw new Error('Invalid environment');
    }
    this.environment = env;
    return this;
  }
  
  setDatabase(config) {
    this.database = config;
    return this;
  }
  
  setCorsOrigins(origins) {
    this.corsOrigins = origins;
    return this;
  }
  
  setJWT(secret) {
    this.jwt = secret;
    return this;
  }
  
  setLogging(level) {
    this.logging = { level };
    return this;
  }
  
  setSessionConfig(config) {
    this.sessionConfig = config;
    return this;
  }
  
  build() {
    if (!this.database) {
      throw new Error('Database config required');
    }
    
    return new AppConfig(this);
  }
}

// Usage
const config = new AppConfigBuilder()
  .setPort(process.env.PORT || 3000)
  .setEnvironment(process.env.NODE_ENV || 'development')
  .setDatabase({
    url: process.env.DB_URL,
    pool: 10
  })
  .setCorsOrigins(['http://localhost:3000', 'https://app.example.com'])
  .setJWT(process.env.JWT_SECRET)
  .setLogging('info')
  .build();

console.log(`Server on ${config.host}:${config.port}`);
```

---

## ✅ KEY TAKEAWAYS

1. **Builder** constructs complex objects step-by-step
2. **Use when** many parameters (10+) or optional fields
3. **Benefits**: Readable code, validation, immutability
4. **Pattern**: Builder class → setter methods → build()
5. **Chainable** methods return `this` for fluent interface
6. **Validation** in build() method before object creation
7. **Perfect for**: Configuration, queries, complex domain objects

---

## 📚 NEXT PATTERN

Ready to learn **Structural Patterns**? Go to `04_STRUCTURAL_ADAPTER.md`

Adapter makes incompatible interfaces work together.

