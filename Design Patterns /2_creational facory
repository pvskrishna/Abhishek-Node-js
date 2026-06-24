# FACTORY METHOD PATTERN - COMPREHENSIVE GUIDE

---

## 📌 DEFINITION

**Factory Method** is a creational pattern that **creates objects without specifying exact classes**. Instead of using `new` directly, you call a factory function that returns the appropriate object based on parameters.

### Core Concept
```
Problem: How to create objects without hard-coding class names?
Solution: Use factory methods that decide which class to instantiate
Benefits: Loose coupling, easy to add new types, centralized creation logic
```

---

## 🎯 WHEN TO USE

### ✅ Use Factory When:
- **Multiple similar objects** with different implementations
- **Need abstraction from creation** (don't want to know exact class)
- **May add new types later** without changing existing code
- **Creation logic is complex** (parameter validation, setup)
- **Want to decouple client from concrete classes**

### ❌ Don't Use When:
- **Single simple object** (just use constructor)
- **Creation logic is trivial** (no benefit)
- **Overcomplicating** simple code
- **No plan to add new types** (premature abstraction)

---

## 🔧 HOW IT WORKS

### Implementation Steps:
1. **Define interface/abstract class** - all objects follow this
2. **Create concrete classes** - implement interface
3. **Factory method** - accepts parameter, returns appropriate object
4. **Client calls factory** - doesn't know exact class

---

## 💻 CODE EXAMPLES

### ❌ WRONG - Hard-coded Object Creation

```javascript
// PROBLEM: New shape means changing existing code (OPEN/CLOSED VIOLATION)
function calculateArea() {
  const shapeType = 'rectangle';
  
  let shape;
  if (shapeType === 'rectangle') {
    shape = new Rectangle(10, 20);
  } else if (shapeType === 'circle') {
    shape = new Circle(5);
  } else if (shapeType === 'triangle') {
    shape = new Triangle(10, 15, 20);
  } else if (shapeType === 'pentagon') {
    shape = new Pentagon(5);
  }
  // Adding new shape? Must modify this code AND update all callers
  
  return shape.area();
}

// PROBLEM: Tight coupling to concrete classes
function renderShapes() {
  const shapes = [
    new Rectangle(10, 20),
    new Circle(5),
    new Triangle(5, 6, 7),
    new Pentagon(3)
  ];
  
  shapes.forEach(shape => shape.draw());
}

// Need to import and know all shape classes!
// If add new shape, must update all places that create objects
```

---

### ✅ CORRECT - Factory Pattern

**Simple Factory (Single Method)**
```javascript
// Step 1: Define interface (abstract concept)
class Shape {
  area() {
    throw new Error('Must implement area()');
  }
  
  draw() {
    throw new Error('Must implement draw()');
  }
}

// Step 2: Concrete implementations
class Circle extends Shape {
  constructor(radius) {
    super();
    this.radius = radius;
  }
  
  area() {
    return Math.PI * this.radius ** 2;
  }
  
  draw() {
    return `Drawing circle with radius ${this.radius}`;
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }
  
  area() {
    return this.width * this.height;
  }
  
  draw() {
    return `Drawing rectangle ${this.width}x${this.height}`;
  }
}

class Triangle extends Shape {
  constructor(a, b, c) {
    super();
    this.a = a;
    this.b = b;
    this.c = c;
  }
  
  area() {
    const s = (this.a + this.b + this.c) / 2;
    return Math.sqrt(s * (s - this.a) * (s - this.b) * (s - this.c));
  }
  
  draw() {
    return `Drawing triangle ${this.a}-${this.b}-${this.c}`;
  }
}

// Step 3: FACTORY METHOD - centralized creation
class ShapeFactory {
  static createShape(type, ...args) {
    switch(type) {
      case 'circle':
        return new Circle(...args);
      case 'rectangle':
        return new Rectangle(...args);
      case 'triangle':
        return new Triangle(...args);
      default:
        throw new Error(`Unknown shape: ${type}`);
    }
  }
}

// Step 4: Client uses factory
const shapes = [
  ShapeFactory.createShape('circle', 5),
  ShapeFactory.createShape('rectangle', 10, 20),
  ShapeFactory.createShape('triangle', 3, 4, 5)
];

shapes.forEach(shape => {
  console.log(`Area: ${shape.area().toFixed(2)}`);
  console.log(shape.draw());
});

// ✅ Adding new shape is EASY and doesn't break existing code!
class Pentagon extends Shape {
  constructor(side) {
    super();
    this.side = side;
  }
  
  area() {
    return (this.side * this.side * Math.sqrt(25 + 10 * Math.sqrt(5))) / 4;
  }
  
  draw() {
    return `Drawing pentagon with side ${this.side}`;
  }
}

// Only add to factory - client code doesn't change!
ShapeFactory.createShape('pentagon', 5); // Works!
```

---

**Factory as Class Method (Method Chaining)**
```javascript
class DatabaseConnection {
  constructor(type) {
    this.type = type;
  }
  
  static create(type) {
    switch(type) {
      case 'mysql':
        return new MySQLConnection();
      case 'mongodb':
        return new MongoDBConnection();
      case 'postgres':
        return new PostgresConnection();
      default:
        throw new Error(`Unknown database: ${type}`);
    }
  }
}

class MySQLConnection extends DatabaseConnection {
  constructor() {
    super('mysql');
  }
  
  connect(config) {
    console.log(`Connecting to MySQL: ${config.host}:${config.port}`);
    return this;
  }
  
  query(sql) {
    console.log(`Executing MySQL query: ${sql}`);
    return this;
  }
}

class MongoDBConnection extends DatabaseConnection {
  constructor() {
    super('mongodb');
  }
  
  connect(config) {
    console.log(`Connecting to MongoDB: ${config.url}`);
    return this;
  }
  
  query(query) {
    console.log(`Executing MongoDB query:`, query);
    return this;
  }
}

class PostgresConnection extends DatabaseConnection {
  constructor() {
    super('postgres');
  }
  
  connect(config) {
    console.log(`Connecting to PostgreSQL: ${config.host}:${config.port}`);
    return this;
  }
  
  query(sql) {
    console.log(`Executing PostgreSQL query: ${sql}`);
    return this;
  }
}

// Usage - no need to know exact class!
const db = DatabaseConnection.create(process.env.DB_TYPE)
  .connect({ host: 'localhost', port: 5432 })
  .query('SELECT * FROM users');

// Easy to add new database type without changing this code!
```

---

**Parameter-based Factory with Configuration**
```javascript
class PaymentProcessor {
  constructor(gateway) {
    this.gateway = gateway;
  }
  
  processPayment(amount, currency = 'USD') {
    return this.gateway.charge(amount, currency);
  }
}

// Concrete payment gateways
class StripeGateway {
  charge(amount, currency) {
    console.log(`Charging $${amount} via Stripe (${currency})`);
    return { success: true, provider: 'Stripe', txnId: Math.random() };
  }
}

class PayPalGateway {
  charge(amount, currency) {
    console.log(`Charging ${currency} ${amount} via PayPal`);
    return { success: true, provider: 'PayPal', txnId: Math.random() };
  }
}

class SquareGateway {
  charge(amount, currency) {
    console.log(`Charging ${currency} ${amount} via Square`);
    return { success: true, provider: 'Square', txnId: Math.random() };
  }
}

// FACTORY: Creates right processor based on config
function createPaymentProcessor(gatewayType) {
  const gateways = {
    'stripe': () => new StripeGateway(),
    'paypal': () => new PayPalGateway(),
    'square': () => new SquareGateway()
  };
  
  const gateway = gateways[gatewayType]?.();
  if (!gateway) {
    throw new Error(`Unknown gateway: ${gatewayType}`);
  }
  
  return new PaymentProcessor(gateway);
}

// Client code - independent of payment provider!
const processor = createPaymentProcessor(process.env.PAYMENT_GATEWAY || 'stripe');
processor.processPayment(99.99);

// Change environment variable = different gateway!
// No code changes needed!
```

---

**Advanced: Factory with Complex Logic**
```javascript
class UserFactory {
  static createUser(userData) {
    const { role, subscriptionLevel } = userData;
    
    // Complex creation logic
    let user;
    
    if (role === 'admin') {
      user = new AdminUser(userData);
      user.permissions = ['read', 'write', 'delete', 'manage_users'];
    } else if (role === 'moderator') {
      user = new ModeratorUser(userData);
      user.permissions = ['read', 'write', 'delete'];
    } else if (role === 'premium') {
      user = new PremiumUser(userData);
      user.permissions = ['read', 'write'];
      user.storageLimit = subscriptionLevel === 'pro' ? 1000 : 100;
    } else {
      user = new BasicUser(userData);
      user.permissions = ['read'];
      user.storageLimit = 5; // 5GB free
    }
    
    // Logging
    console.log(`Created ${user.constructor.name} for ${userData.email}`);
    
    return user;
  }
}

class BaseUser {
  constructor(data) {
    this.id = Math.random();
    this.email = data.email;
    this.name = data.name;
    this.createdAt = Date.now();
  }
  
  getProfile() {
    return { email: this.email, name: this.name };
  }
}

class AdminUser extends BaseUser {
  deleteUser(userId) {
    return `Admin ${this.name} deleted user ${userId}`;
  }
}

class PremiumUser extends BaseUser {
  uploadFile(filename, size) {
    if (size > this.storageLimit) {
      throw new Error('Storage limit exceeded');
    }
    return `Premium user uploaded ${filename}`;
  }
}

class BasicUser extends BaseUser {
  uploadFile(filename, size) {
    if (size > this.storageLimit) {
      throw new Error('Free tier: only 5GB allowed');
    }
    return `Free user uploaded ${filename}`;
  }
}

// Usage - factory handles all complexity
const user1 = UserFactory.createUser({ 
  email: 'admin@app.com', 
  name: 'Admin', 
  role: 'admin' 
});

const user2 = UserFactory.createUser({ 
  email: 'user@app.com', 
  name: 'User', 
  role: 'premium',
  subscriptionLevel: 'pro'
});

console.log(user1.permissions); // ['read', 'write', 'delete', 'manage_users']
console.log(user2.storageLimit); // 1000 (pro tier)
```

---

## 🌍 REAL-TIME USAGE (PRODUCTION EXAMPLES)

### Example 1: UI Component Factory (React/Vue)
```javascript
// Common pattern in UI frameworks
class ComponentFactory {
  static createComponent(type, props) {
    const components = {
      'button': () => new Button(props),
      'input': () => new Input(props),
      'dropdown': () => new Dropdown(props),
      'modal': () => new Modal(props),
      'spinner': () => new Spinner(props)
    };
    
    const creator = components[type];
    if (!creator) throw new Error(`Unknown component: ${type}`);
    return creator();
  }
}

// Usage in framework
const button = ComponentFactory.createComponent('button', { label: 'Click me' });
button.render();
```

---

### Example 2: Logger Factory (Multiple Output Types)
```javascript
class LoggerFactory {
  static createLogger(type) {
    switch(type) {
      case 'console':
        return new ConsoleLogger();
      case 'file':
        return new FileLogger('/var/logs/app.log');
      case 'database':
        return new DatabaseLogger('mongodb://localhost');
      case 'external':
        return new SentryLogger('https://sentry.io/');
      default:
        return new ConsoleLogger();
    }
  }
}

class ConsoleLogger {
  log(message) { console.log(message); }
}

class FileLogger {
  constructor(filepath) { this.filepath = filepath; }
  log(message) { /* write to file */ }
}

class DatabaseLogger {
  constructor(dbUrl) { this.dbUrl = dbUrl; }
  log(message) { /* write to DB */ }
}

class SentryLogger {
  constructor(url) { this.url = url; }
  log(message) { /* send to Sentry */ }
}

// Production systems use different loggers without code changes
const logger = LoggerFactory.createLogger(process.env.LOG_TYPE || 'console');
```

---

### Example 3: Authentication Strategies (Netflix/Auth0 Pattern)
```javascript
class AuthFactory {
  static createAuthStrategy(type) {
    const strategies = {
      'jwt': () => new JWTStrategy(),
      'oauth': () => new OAuthStrategy(),
      'api-key': () => new ApiKeyStrategy(),
      'session': () => new SessionStrategy(),
      'mfa': () => new MFAStrategy()
    };
    
    const strategy = strategies[type]?.();
    if (!strategy) throw new Error(`Unknown auth: ${type}`);
    return strategy;
  }
}

class JWTStrategy {
  authenticate(token) {
    // Verify JWT
    return { authenticated: true, user: { id: 123 } };
  }
}

class OAuthStrategy {
  authenticate(accessToken) {
    // Verify via OAuth provider
    return { authenticated: true, user: { id: 456 } };
  }
}

// Easy to add new auth method!
class MFAStrategy {
  authenticate(code, token) {
    // Verify code + token
    return { authenticated: true, user: { id: 789 }, mfaVerified: true };
  }
}
```

---

### Example 4: Notification Factory (Airbnb/Twilio Pattern)
```javascript
class NotificationFactory {
  static createNotification(type) {
    const types = {
      'email': () => new EmailNotification(),
      'sms': () => new SMSNotification(),
      'push': () => new PushNotification(),
      'slack': () => new SlackNotification(),
      'webhook': () => new WebhookNotification()
    };
    
    return types[type]?.();
  }
}

class EmailNotification {
  send(recipient, message) {
    console.log(`Email to ${recipient}: ${message}`);
    // Use SendGrid, AWS SES, etc.
  }
}

class SMSNotification {
  send(phone, message) {
    console.log(`SMS to ${phone}: ${message}`);
    // Use Twilio
  }
}

class PushNotification {
  send(deviceId, message) {
    console.log(`Push to device ${deviceId}: ${message}`);
    // Use Firebase
  }
}

// In application
function notifyUser(userId, message, channels) {
  channels.forEach(channel => {
    const notifier = NotificationFactory.createNotification(channel);
    notifier.send(userId, message);
  });
}

// Usage
notifyUser('user123', 'Order shipped!', ['email', 'sms', 'push']);
// Easy to add new notification type!
```

---

## 📊 DIAGRAM

```mermaid
graph TD
    Client["Client Code<br/>createShape('circle')"]
    Factory["ShapeFactory<br/>Factory Method"]
    Decision{"type?"}
    Circle["Circle<br/>implements Shape"]
    Rectangle["Rectangle<br/>implements Shape"]
    Triangle["Triangle<br/>implements Shape"]
    
    Client -->|calls| Factory
    Factory --> Decision
    Decision -->|'circle'| Circle
    Decision -->|'rectangle'| Rectangle
    Decision -->|'triangle'| Triangle
    
    Circle -.implements.-> Shape["Shape<br/>Interface"]
    Rectangle -.implements.-> Shape
    Triangle -.implements.-> Shape
    
    Circle -->|draw()| Client
    Rectangle -->|draw()| Client
    Triangle -->|draw()| Client
    
    style Factory fill:#90EE90
    style Decision fill:#FFD700
    style Shape fill:#87CEEB
```

---

## ⚖️ PROS & CONS

| Aspect | Pros | Cons |
|--------|------|------|
| **Flexibility** | Easy to add new types | Extra classes to maintain |
| **Coupling** | Loose coupling (client doesn't know concrete class) | Added complexity |
| **OCP** | Open/Closed Principle - open for extension | Overkill for simple cases |
| **Code** | Creation logic centralized | Extra indirection |
| **Maintenance** | Easy to modify creation logic | More files to manage |
| **Scalability** | Scales well with new types | Harder to trace execution |

---

## 🎤 TOP INTERVIEW QUESTIONS & ANSWERS

### Q1: "What is Factory Pattern and why use it?"
**Answer:**
"Factory Pattern creates objects without specifying exact classes. Instead of `new Shape()`, you call `ShapeFactory.create('circle')`.

Why?
- Loose coupling - client doesn't know concrete classes
- Easy to add new types without changing existing code
- Centralized creation logic
- Supports Open/Closed Principle

Example: Payment gateways - switch from Stripe to PayPal without code changes."

---

### Q2: "How is Factory different from Singleton?"
**Answer:**
"Singleton: ONE instance of a class
Factory: CREATE objects (potentially many)

```javascript
// Singleton - one instance
const logger = Logger.getInstance();

// Factory - creates new objects
const shape1 = ShapeFactory.create('circle');
const shape2 = ShapeFactory.create('rectangle');
// shape1 and shape2 are DIFFERENT objects
```

Singleton controls how many instances exist (one).
Factory controls HOW objects are created."

---

### Q3: "Simple Factory vs Factory Method vs Abstract Factory?"
**Answer:**
"**Simple Factory:**
```javascript
// Single factory method
class ShapeFactory {
  static create(type) {
    if(type === 'circle') return new Circle();
    if(type === 'rect') return new Rectangle();
  }
}
```

**Factory Method:**
```javascript
// Each creator has its own method
class ShapeCreator {
  createShape() {
    throw new Error('Override this');
  }
}

class CircleCreator extends ShapeCreator {
  createShape() { return new Circle(); }
}
```

**Abstract Factory:**
Creates FAMILIES of related objects
```javascript
class UIFactory {
  createButton() { return new Button(); }
  createInput() { return new Input(); }
  createDropdown() { return new Dropdown(); }
}
```"

---

### Q4: "When NOT to use Factory?"
**Answer:**
"Don't use Factory when:
1. Only one type (just use constructor)
2. Creation logic is trivial
3. No plan to add new types
4. Would complicate simple code
5. Static utility doesn't fit

Example - DON'T use factory:
```javascript
// No point - just use constructor
const date = DateFactory.create(2024, 1, 1);
// Should be:
const date = new Date(2024, 1, 1);
```"

---

### Q5: "How to handle errors in Factory?"
**Answer:**
"Options:
1. Throw error (fail fast)
2. Return null/undefined (client checks)
3. Return default/fallback
4. Chain factories

```javascript
class DatabaseFactory {
  static create(type) {
    // Option 1: Throw
    if(!type) throw new Error('Type required');
    
    // Option 2: Use Map with fallback
    const factories = {
      'mysql': () => new MySQL(),
      'mongo': () => new Mongo(),
      'default': () => new MySQL() // fallback
    };
    
    return (factories[type] || factories['default'])();
  }
}
```"

---

## 🚨 ANTI-PATTERNS & MISTAKES

### ❌ Mistake 1: Factory that Hides Complexity
```javascript
// WRONG - factory becomes a God object
class ObjectFactory {
  static create(type, config) {
    // 1000 lines of if-else statements
    // Multiple responsibilities
    // Hard to test
    // Hard to maintain
  }
}

// BETTER - separate concerns
class DatabaseFactory { /* handles DB */ }
class LoggerFactory { /* handles logging */ }
class PaymentFactory { /* handles payments */ }
```

---

### ❌ Mistake 2: Ignoring Parameters
```javascript
// WRONG - ignores config parameters
class UserFactory {
  static create(role) {
    // Only uses role, ignores other params
    if(role === 'admin') return new AdminUser();
    return new User();
  }
}

// BETTER
class UserFactory {
  static create(role, permissions, storageLimit) {
    const user = this.createByRole(role);
    user.permissions = permissions;
    user.storageLimit = storageLimit;
    return user;
  }
}
```

---

### ❌ Mistake 3: Tight Coupling Inside Factory
```javascript
// WRONG - factory is tightly coupled
class PaymentFactory {
  static create(type) {
    // Hard-coded classes
    if(type === 'stripe') return new Stripe('secret_key_hardcoded');
    if(type === 'paypal') return new PayPal('key_hardcoded');
  }
}

// BETTER - inject dependencies
class PaymentFactory {
  static create(type, config) {
    if(type === 'stripe') return new Stripe(config.stripeKey);
    if(type === 'paypal') return new PayPal(config.paypalKey);
  }
}
```

---

## 🎯 REAL-WORLD SCENARIO

### Scenario: E-commerce Shipping (Stripe/Shopify Use Case)
```javascript
// All shipping providers implement same interface
class ShippingProvider {
  calculateCost(weight, destination) {
    throw new Error('Implement');
  }
  
  track(trackingId) {
    throw new Error('Implement');
  }
}

class FedexProvider extends ShippingProvider {
  calculateCost(weight, destination) {
    return weight * 0.5 + (destination === 'intl' ? 50 : 0);
  }
  
  track(trackingId) {
    return `Tracking ${trackingId} on FedEx`;
  }
}

class UPSProvider extends ShippingProvider {
  calculateCost(weight, destination) {
    return weight * 0.4 + (destination === 'intl' ? 40 : 0);
  }
  
  track(trackingId) {
    return `Tracking ${trackingId} on UPS`;
  }
}

class DHLProvider extends ShippingProvider {
  calculateCost(weight, destination) {
    return weight * 0.6 + (destination === 'intl' ? 60 : 0);
  }
  
  track(trackingId) {
    return `Tracking ${trackingId} on DHL`;
  }
}

// FACTORY - centralized provider selection
class ShippingFactory {
  static getProvider(providerType) {
    const providers = {
      'fedex': () => new FedexProvider(),
      'ups': () => new UPSProvider(),
      'dhl': () => new DHLProvider()
    };
    
    const provider = providers[providerType]?.();
    if (!provider) {
      throw new Error(`Unknown provider: ${providerType}`);
    }
    return provider;
  }
  
  // Smart selection based on criteria
  static selectBestProvider(weight, destination, providers) {
    const cheapest = providers
      .map(type => ({
        provider: ShippingFactory.getProvider(type),
        cost: ShippingFactory.getProvider(type).calculateCost(weight, destination)
      }))
      .sort((a, b) => a.cost - b.cost)[0];
    
    return cheapest.provider;
  }
}

// Usage
function shipOrder(order) {
  const provider = ShippingFactory.selectBestProvider(
    order.weight,
    order.destination,
    ['fedex', 'ups', 'dhl']
  );
  
  const cost = provider.calculateCost(order.weight, order.destination);
  console.log(`Shipping via ${provider.constructor.name}: $${cost}`);
}

shipOrder({ weight: 5, destination: 'domestic' });
// Dynamically chooses cheapest provider!
```

---

## ✅ KEY TAKEAWAYS

1. **Factory** creates objects without hard-coding class names
2. **Benefits**: Loose coupling, easy to extend, centralized logic
3. **When**: Multiple similar objects or frequent new types
4. **Don't overuse**: Can complicate simple code
5. **Perfect for**: Payment gateways, auth, notifications, DB connections
6. **Pattern**: Accept parameter → decide which class → return instance
7. **SOLID**: Open/Closed Principle - open for new types, closed for modification

---

## 📚 NEXT PATTERN

Ready to learn **Builder Pattern**? Go to `03_CREATIONAL_BUILDER.md`

Pattern for constructing complex objects step-by-step (vs all-at-once).

