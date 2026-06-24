# DESIGN PATTERNS - ADVANCED INTERVIEW MASTERCLASS

---

## FOR 7-YEAR SENIOR CANDIDATES

This guide takes you from "knowing patterns" to "mastering patterns for high-level interviews."

---

## 🎯 WHAT SEPARATES SENIOR FROM JUNIOR

### Junior Developer
- ❌ Knows definitions
- ❌ Can identify patterns in code
- ❌ Memorized examples
- ❌ Uses patterns because "it's best practice"

### Senior Developer (7+ years)
- ✅ Recognizes code smells → suggests pattern
- ✅ Knows when patterns hurt performance
- ✅ Combines patterns for complex systems
- ✅ Can refactor without breaking production
- ✅ Teaches others about patterns
- ✅ Makes pattern trade-off decisions
- ✅ Implements custom variations

---

## 🚀 ADVANCED INTERVIEW QUESTIONS

### Level 1: Definition (Junior)
**Q: "What is Factory pattern?"**
A: "Creates objects without specifying exact class."

### Level 2: Application (Mid)
**Q: "How would you use Factory in a payment system?"**
A: "Create PaymentFactory.create('stripe'/'paypal'/'square') to support multiple gateways."

### Level 3: Trade-offs (Senior)
**Q: "Compare Factory vs Constructor for object creation. When use each?"**
A: 
"Factory when:
- Multiple types with different initialization
- May add types without changing code
- Complex creation logic
- Want to hide implementation

Constructor when:
- Single type
- Simple initialization  
- No likely to change
- Performance critical

At company X, we used Factory for payment gateways but Constructor for User objects. Trade-off: Factory adds abstraction vs Constructor's simplicity."

### Level 4: Architecture (Principal)
**Q: "How would you design a payment system at scale using patterns?"**
A:
"I'd use:
1. **Factory** for payment provider selection
2. **Strategy** for different payment algorithms
3. **Decorator** for middleware (auth, logging, retry)
4. **Observer** for transaction notifications
5. **Command** for transaction history/undo
6. **Adapter** for legacy provider integration

This gives flexibility, observability, and extensibility. Trade-off: Added complexity, but handles growth."

---

## 💡 RECOGNIZING CODE SMELLS & SUGGESTING PATTERNS

### Code Smell: Too Many If-Else
```javascript
// SMELL: Many conditionals
if (paymentType === 'stripe') {
  // stripe logic
} else if (paymentType === 'paypal') {
  // paypal logic
} else if (paymentType === 'square') {
  // square logic
}

// SOLUTION: Strategy Pattern
const strategies = {
  'stripe': new StripeStrategy(),
  'paypal': new PayPalStrategy(),
  'square': new SquareStrategy()
};
const strategy = strategies[paymentType];
```

---

### Code Smell: God Object
```javascript
// SMELL: One class doing everything
class UserService {
  createUser() { }
  validateUser() { }
  authenticateUser() { }
  authorizeUser() { }
  sendEmail() { }
  logActivity() { }
  cacheUser() { }
  // 20+ more methods
}

// SOLUTION: Facade + Decorator
class UserServiceFacade {
  createUser() { // Orchestrates other services
    const user = userRepository.create();
    notificationService.send();
    auditService.log();
  }
}
```

---

### Code Smell: Inheritance Explosion
```javascript
// SMELL: Many subclasses
Coffee → CoffeeWithMilk → CoffeeWithMilkAndSugar → CoffeeWithMilkAndSugarAndCinnamon

// SOLUTION: Decorator Pattern
let coffee = new Coffee();
coffee = new MilkDecorator(coffee);
coffee = new SugarDecorator(coffee);
coffee = new CinnamonDecorator(coffee);
```

---

### Code Smell: Tight Coupling
```javascript
// SMELL: Hardcoded dependencies
class PaymentProcessor {
  process() {
    const stripe = new StripeAPI(); // Tight coupling!
    return stripe.charge();
  }
}

// SOLUTION: Factory + Dependency Injection
class PaymentProcessor {
  constructor(paymentFactory) {
    this.factory = paymentFactory;
  }
  
  process(type) {
    const provider = this.factory.create(type);
    return provider.charge();
  }
}
```

---

## 🔄 PATTERN COMBINATIONS FOR COMPLEX SYSTEMS

### Payment System Architecture
```javascript
// Combination of patterns:

// 1. Factory - create payment providers
const paymentFactory = new PaymentFactory();

// 2. Strategy - select algorithm
const strategy = new PaymentStrategy(provider);

// 3. Decorator - add features
const decorated = new RetryDecorator(
  new LoggingDecorator(
    new AuthDecorator(strategy)
  )
);

// 4. Command - for undo/redo
const command = new PaymentCommand(decorated);

// 5. Observer - for notifications
eventEmitter.on('payment_complete', handler);

// 6. Adapter - for legacy systems
const legacyAdapter = new LegacyPaymentAdapter(oldSystem);

// Result: Flexible, maintainable, extensible system
```

---

### E-commerce Order Processing
```javascript
// 1. Builder - construct order
const order = new OrderBuilder(userId)
  .addItem(item1)
  .addItem(item2)
  .setShippingAddress(address)
  .setPaymentMethod(method)
  .build();

// 2. Facade - process order
const orderFacade = new OrderProcessingFacade();
orderFacade.processOrder(order);
// Hides: payment, shipping, notification, inventory

// 3. Chain of Responsibility - validation chain
validationChain
  .validatePayment()
  .validateInventory()
  .validateShipping()
  .process(order);

// 4. Observer - notify customers
observer.on('order_placed', sendConfirmation);
observer.on('shipped', sendShipment);
observer.on('delivered', sendReceipt);

// 5. Command - for order history
history.record(new PlaceOrderCommand(order));
history.record(new ShipOrderCommand(order));
```

---

## 🎓 TEACHING OTHERS ABOUT PATTERNS

As a senior, you explain patterns to juniors. Here's how:

### Teaching Singleton
"Singleton ensures ONE instance. Think of it like the President - only one at a time. We use it for database connections because we don't want 100 connections to same DB when one pool can handle it all."

### Teaching Factory
"Factory is like a restaurant - customer doesn't care if they order from Sysco or local suppliers. Manager decides. Our code should be same way - don't hardcode supplier, let factory decide based on environment."

### Teaching Observer
"Observer is like a mailing list. When something happens, all subscribers get notified automatically. Our payment system uses this - when payment completes, email, analytics, audit log all get notified without order system knowing about them."

---

## 🔥 ADVANCED PATTERNS INTERVIEW SCENARIOS

### Scenario 1: "Refactor This Code"

```javascript
// Given messy code
class ProductService {
  getProduct(id) {
    if (cache.has(id)) return cache.get(id);
    const product = db.query(`SELECT * FROM products WHERE id = ${id}`);
    cache.set(id, product);
    return product;
  }
  
  updateProduct(id, data) {
    db.update(id, data);
    cache.delete(id);
  }
  
  deleteProduct(id) {
    db.delete(id);
    cache.delete(id);
  }
}

// Senior answer:
/*
Issues:
1. SQL injection (not pattern issue, but important)
2. Mixed concerns (DB + caching)
3. Caching logic in service

Refactored with patterns:

1. Repository pattern - separate data access
2. Decorator - add caching as cross-cutting concern
3. Proxy - lazy load from cache
*/

class ProductRepository {
  constructor(db) {
    this.db = db;
  }
  
  getProduct(id) {
    return this.db.query('SELECT * FROM products WHERE id = ?', [id]);
  }
}

class CachingProxy {
  constructor(repository, cache) {
    this.repository = repository;
    this.cache = cache;
  }
  
  getProduct(id) {
    if (this.cache.has(id)) return this.cache.get(id);
    const product = this.repository.getProduct(id);
    this.cache.set(id, product);
    return product;
  }
}
```

---

### Scenario 2: "Design a Logging System"

**Senior Approach:**

```javascript
/*
Requirements:
- Log to console
- Log to file
- Log to external service (Sentry)
- Different log levels
- Can add new loggers without changing code
- Can combine loggers
*/

// 1. Strategy - different logging strategies
class Logger {
  log(message) { throw new Error('Implement'); }
}

class ConsoleLogger extends Logger {
  log(msg) { console.log(msg); }
}

class FileLogger extends Logger {
  log(msg) { fs.appendFileSync('app.log', msg); }
}

// 2. Factory - create appropriate logger
class LoggerFactory {
  static create(type) {
    const types = {
      'console': () => new ConsoleLogger(),
      'file': () => new FileLogger(),
      'sentry': () => new SentryLogger()
    };
    return types[type]?.();
  }
}

// 3. Decorator - add features (levels, timestamps, formatting)
class LogLevelDecorator {
  constructor(logger, level) {
    this.logger = logger;
    this.level = level;
  }
  
  log(msg, level) {
    if (this.level >= level) {
      this.logger.log(`[${level}] ${msg}`);
    }
  }
}

class TimestampDecorator {
  constructor(logger) {
    this.logger = logger;
  }
  
  log(msg) {
    this.logger.log(`[${new Date().toISOString()}] ${msg}`);
  }
}

// 4. Composite - multiple loggers at once
class CompositeLogger extends Logger {
  constructor() {
    this.loggers = [];
  }
  
  add(logger) {
    this.loggers.push(logger);
  }
  
  log(msg) {
    this.loggers.forEach(logger => logger.log(msg));
  }
}

// Usage
const composite = new CompositeLogger();
composite.add(new TimestampDecorator(LoggerFactory.create('console')));
composite.add(new LogLevelDecorator(LoggerFactory.create('file'), 'error'));
composite.add(LoggerFactory.create('sentry'));

composite.log('Application started'); // Goes to all 3
```

**Benefits:**
- Easy to add new logger types
- Combine loggers flexibly
- Levels, timestamps are separate concerns
- Production code vs logging decoupled

---

## 📈 QUESTIONS THAT REVEAL SENIOR LEVEL

### Q1: "What pattern would you NOT use?"
**Answer:** "I wouldn't use Factory for simple single-type objects. I wouldn't use Decorator for performance-critical code. I wouldn't use Composite for flat structures. Knowing when NOT to use patterns shows wisdom."

---

### Q2: "How do patterns relate to SOLID principles?"
**Answer:**
- Singleton/Factory: Single Responsibility Principle
- Adapter/Facade: Dependency Inversion
- Decorator/Strategy: Open/Closed Principle
- Observer: Dependency Inversion
- Factory: Open/Closed Principle
"Patterns implement SOLID, not replace it."

---

### Q3: "Give example of pattern harming performance"
**Answer:**
"Decorator chains add function call overhead. In real-time trading system, we couldn't use Decorator for every operation. We switched to direct method calls for performance-critical paths. Trade-off: performance vs abstraction."

---

### Q4: "How do you handle pattern mistakes in code review?"
**Answer:**
"Politely point out if pattern doesn't solve the problem:
- 'Why use Builder for 3 parameters?'
- 'This looks like overengineering - can we simplify?'
- 'Did we consider the trade-off here?'

Help juniors understand patterns solve PROBLEMS, not all code needs patterns."

---

### Q5: "Combine patterns to solve this real problem..."
**Answer:** Shows you can think beyond single patterns, design systems holistically.

---

## 🚨 COMMON TRAPS FOR SENIOR INTERVIEWS

### ❌ Trap 1: Over-engineering
Don't suggest patterns for everything. Interviewer wants to see judgment, not pattern obsession.

### ❌ Trap 2: Ignoring trade-offs
"This pattern is perfect" = Junior answer.
"This pattern helps but adds complexity" = Senior answer.

### ❌ Trap 3: Forgetting SOLID
Patterns support SOLID, not replace it. Show you understand both.

### ❌ Trap 4: No production experience
Don't just know theory. Share real examples from work.
"At [company], we used [pattern] because [business reason], which resulted in [positive outcome]."

### ❌ Trap 5: Can't code under pressure
Know patterns so well you code them in real interview live.

---

## ✅ STRATEGIES FOR CRUSHING INTERVIEW

### Before Interview
1. **Review quick reference** - solidify knowledge
2. **Code all patterns** - hands-on practice
3. **Prepare 2-3 stories** - "At company X, we used pattern Y to solve Z"
4. **Practice refactoring** - show code smell → pattern mapping
5. **Study trade-offs** - know benefits AND costs

### During Interview
1. **Ask clarifying questions** - before suggesting patterns
2. **Show multiple approaches** - don't just give one answer
3. **Explain trade-offs** - this shows maturity
4. **Code with confidence** - write clean code under pressure
5. **Ask about constraints** - performance, team size, etc.

### Answer Structure
"The problem here is [code smell]. I'd suggest [pattern] because [reason]. Trade-off: [cost]. Alternative: [option]. At [company], we did similar with [example]."

---

## 📊 REAL INTERVIEW SCORING

### ✅ High Score (Gets Offer)
```
Understanding: "I know all 15 patterns, when to use, when NOT to use"
Application: "I can code any pattern in 5 minutes"
Trade-offs: "I understand costs and benefits"
Experience: "I've used this in production at Netflix/Stripe"
Judgment: "I know when to NOT use patterns"
```

### 🤔 Medium Score (Maybe)
```
Understanding: "I know most patterns and definitions"
Application: "I can code with help/time"
Trade-offs: "I understand benefits mostly"
Experience: "I've seen this in code"
Judgment: "I use patterns when needed"
```

### ❌ Low Score (Rejected)
```
Understanding: "I know pattern definitions"
Application: "I struggle to code patterns"
Trade-offs: "Not really considered"
Experience: "Textbook knowledge only"
Judgment: "Tries to use patterns everywhere"
```

---

## 🎯 FINAL CHECKLIST FOR INTERVIEWS

Before any senior design interview:

- [ ] Can code all 15 patterns from memory
- [ ] Know 2-3 production examples for each
- [ ] Understand trade-offs for each pattern
- [ ] Can refactor code using patterns
- [ ] Know when NOT to use each pattern
- [ ] Can combine patterns for complex systems
- [ ] Understand SOLID principles connection
- [ ] Have personal stories from production
- [ ] Can explain to junior developers
- [ ] Can solve interview scenario using patterns

---

## 🚀 YOU'RE NOW SENIOR-LEVEL READY!

With 7+ years experience + this guide:
- ✅ Master all 15 design patterns
- ✅ Recognize patterns in code
- ✅ Suggest patterns for problems
- ✅ Code patterns under pressure
- ✅ Explain trade-offs confidently
- ✅ Design complex systems with patterns
- ✅ Teach others about patterns
- ✅ Know when patterns hurt
- ✅ Combine patterns effectively
- ✅ Pass senior-level interviews

---

**"A junior uses patterns. A senior DECIDES whether to use patterns." - Yours truly**

Good luck! You've got this! 🎉

