# DECORATOR PATTERN - COMPREHENSIVE GUIDE

---

## 📌 DEFINITION

**Decorator** adds behavior to objects dynamically **without modifying them**. Wraps object to extend functionality at runtime. Better than inheritance for flexible feature combinations.

### Core Concept
```
Problem: Need to add features to object, but inheritance creates class explosion
Solution: Wrap object with decorator that adds behavior
Like: Wrapping gift with different paper designs, or adding toppings to coffee
```

---

## 🎯 WHEN TO USE

✅ **Use when:**
- Multiple optional features/capabilities
- Avoid class explosion from inheritance
- Features can be combined in any way
- Need runtime flexibility
- Want single responsibility

❌ **Don't use when:**
- Fixed set of features (use inheritance)
- Performance critical (decorator adds overhead)
- Few variations (simple inheritance ok)

---

## 💻 CODE EXAMPLES

### ❌ WRONG - Inheritance Explosion

```javascript
// PROBLEM: Creates class for every combination
class Coffee { price = 5; }
class CoffeeWithMilk extends Coffee { price = 7; }
class CoffeeWithMilkAndSugar extends CoffeeWithMilk { price = 8; }
class CoffeeWithMilkAndSugarAndCinnamon extends CoffeeWithMilkAndSugar { price = 9; }

// With 5 toppings = 2^5 = 32 classes!
```

---

### ✅ CORRECT - Decorator Pattern

```javascript
// Base coffee class
class Coffee {
  getPrice() { return 5; }
  getDescription() { return 'Coffee'; }
}

// DECORATOR 1: Add milk
class MilkDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }
  
  getPrice() {
    return this.coffee.getPrice() + 2;
  }
  
  getDescription() {
    return this.coffee.getDescription() + ', Milk';
  }
}

// DECORATOR 2: Add sugar
class SugarDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }
  
  getPrice() {
    return this.coffee.getPrice() + 0.5;
  }
  
  getDescription() {
    return this.coffee.getDescription() + ', Sugar';
  }
}

// DECORATOR 3: Add cinnamon
class CinnamonDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }
  
  getPrice() {
    return this.coffee.getPrice() + 1;
  }
  
  getDescription() {
    return this.coffee.getDescription() + ', Cinnamon';
  }
}

// Usage - Combine any features!
let coffee = new Coffee();
console.log(`${coffee.getDescription()}: $${coffee.getPrice()}`); // Coffee: $5

coffee = new MilkDecorator(coffee);
console.log(`${coffee.getDescription()}: $${coffee.getPrice()}`); // Coffee, Milk: $7

coffee = new SugarDecorator(coffee);
console.log(`${coffee.getDescription()}: $${coffee.getPrice()}`); // Coffee, Milk, Sugar: $7.50

coffee = new CinnamonDecorator(coffee);
console.log(`${coffee.getDescription()}: $${coffee.getPrice()}`); // Coffee, Milk, Sugar, Cinnamon: $8.50

// Different combination - no new classes needed!
const fancyCoffee = new CinnamonDecorator(
  new SugarDecorator(
    new MilkDecorator(new Coffee())
  )
);
```

---

**Real-world: HTTP Request Decorators**
```javascript
class HttpRequest {
  execute() {
    return { data: 'response', status: 200 };
  }
}

// DECORATOR 1: Add authentication
class AuthDecorator {
  constructor(request) {
    this.request = request;
  }
  
  execute() {
    console.log('Adding Authorization header');
    const result = this.request.execute();
    result.headers = { ...result.headers, Authorization: 'Bearer token123' };
    return result;
  }
}

// DECORATOR 2: Add compression
class CompressionDecorator {
  constructor(request) {
    this.request = request;
  }
  
  execute() {
    console.log('Adding gzip compression');
    const result = this.request.execute();
    result.headers = { ...result.headers, 'Content-Encoding': 'gzip' };
    return result;
  }
}

// DECORATOR 3: Add retry logic
class RetryDecorator {
  constructor(request, retries = 3) {
    this.request = request;
    this.retries = retries;
  }
  
  execute() {
    for (let i = 0; i < this.retries; i++) {
      try {
        console.log(`Attempt ${i + 1}`);
        return this.request.execute();
      } catch (err) {
        if (i === this.retries - 1) throw err;
      }
    }
  }
}

// Usage - stack decorators
let request = new HttpRequest();
request = new RetryDecorator(
  new CompressionDecorator(
    new AuthDecorator(request)
  ),
  3
);

const response = request.execute();
// Adding Authorization header
// Adding gzip compression
// Attempt 1
// Done with auth, compression, and retry!
```

---

## 🌍 REAL-TIME USAGE

### Example 1: Express Middleware (Node.js)
```javascript
// Middleware IS decorator pattern!
app.use(cors()); // Adds CORS handling
app.use(express.json()); // Adds JSON parsing
app.use(authenticate); // Adds authentication
app.use(logging); // Adds logging
app.use(errorHandler); // Adds error handling

// Each middleware wraps request/response with additional behavior
```

---

### Example 2: Component Features in React
```javascript
// Base component
function Dashboard() { return <div>Dashboard</div>; }

// Decorators add features
const DashboardWithAuth = withAuth(Dashboard);
const DashboardWithAnalytics = withAnalytics(DashboardWithAuth);
const DashboardWithTheme = withTheme(DashboardWithAnalytics);

// Each decorator adds feature without modifying Dashboard
```

---

### Example 3: Stream Compression (Node.js)
```javascript
const fs = require('fs');
const zlib = require('zlib'); // Decorator for compression

// Base readable stream
const input = fs.createReadStream('file.txt');

// Decorator adds gzip compression
const compressed = input.pipe(zlib.createGzip());

// Another decorator adds base64 encoding
const encoded = compressed.pipe(transform);

// Can combine multiple decorators in pipeline
```

---

## 📊 DIAGRAM

```mermaid
graph TD
    Client["Client"]
    Coffee["Coffee<br/>getPrice(): 5"]
    MD["MilkDecorator<br/>wraps Coffee"]
    SD["SugarDecorator<br/>wraps MD"]
    CD["CinnamonDecorator<br/>wraps SD"]
    
    Client -->|calls getPrice()| CD
    CD -->|adds 1| SD
    SD -->|adds 0.5| MD
    MD -->|adds 2| Coffee
    Coffee -->|returns 5| MD
    
    style Coffee fill:#FFB6C1
    style MD fill:#90EE90
    style SD fill:#90EE90
    style CD fill:#90EE90
```

---

## ⚖️ PROS & CONS

| Aspect | Pros | Cons |
|--------|------|------|
| **Flexibility** | Any combination of features | Ordering matters (D1(D2(obj)) ≠ D2(D1(obj))) |
| **Single Responsibility** | Each decorator one concern | More objects in memory |
| **Runtime** | Add features at runtime | Hard to debug long chains |
| **Inheritance** | Avoid class explosion | More wrapper objects |
| **Reusability** | Decorators reusable | Performance overhead |

---

## 🎤 KEY INTERVIEW QUESTIONS

### Q1: "Decorator vs Inheritance?"
**Answer:**
"Decorator: Combines features at runtime, flexible combinations
Inheritance: Fixed hierarchy, limited combinations

With 5 features:
- Inheritance: Need 2^5 = 32 classes
- Decorator: Need 5 decorators, unlimited combinations"

---

### Q2: "Does order matter in decorators?"
**Answer:**
"YES! Different order = different result

```javascript
const obj1 = D2(D1(coffee)); // Sugar then Milk
const obj2 = D1(D2(coffee)); // Milk then Sugar

// Same components, different order, different behavior!
```"

---

### Q3: "When does decorator add too much overhead?"
**Answer:**
"When:
- Performance critical (added function calls)
- Simple object (just use inheritance)
- Few features (not complex enough)
- Real-time systems (microseconds matter)

Decorator best for complex feature combinations, not simple cases."

---

## ✅ KEY TAKEAWAYS

1. **Decorator** adds behavior without modifying object
2. **Avoids** inheritance class explosion
3. **Stacks** decorators for combined features
4. **Order matters** - different results
5. **Runtime** flexibility - add/remove decorators dynamically
6. **Perfect for** middleware, streams, optional features
7. **Trade-off** - flexibility vs complexity

---

## 📚 NEXT: FACADE PATTERN

Go to `06_STRUCTURAL_FACADE.md` - Simplify complex subsystems.
