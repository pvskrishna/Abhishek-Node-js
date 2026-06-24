# 🚀 DESIGN PATTERNS - QUICK REFERENCE & CHEAT SHEET

---

## 📊 ALL 15 PATTERNS AT A GLANCE

### CREATIONAL PATTERNS (Object Creation)

| Pattern | Purpose | When Use | Key Benefit | Example |
|---------|---------|----------|-----------|---------|
| **Singleton** | Only ONE instance | Global state, Logger, Config | Single point of access | Database connection pool |
| **Factory** | Create without specifying class | Multiple similar objects | Abstraction from creation | Payment gateways |
| **Builder** | Build complex objects step-by-step | Many parameters, optional fields | Readable code, validation | User with 15 fields |

### STRUCTURAL PATTERNS (Object Composition)

| Pattern | Purpose | When Use | Key Benefit | Example |
|---------|---------|----------|-----------|---------|
| **Adapter** | Make incompatible interfaces work | Third-party libraries | Isolation of incompatibility | Stripe/PayPal conversion |
| **Decorator** | Add behavior dynamically | Optional features | Avoid class explosion | Coffee with toppings |
| **Facade** | Simplify complex subsystem | Hide complexity | Single entry point | Media converter |
| **Proxy** | Control access to object | Lazy loading, caching | Access control, optimization | Database lazy loading |
| **Composite** | Tree hierarchies | Parent-child relationships | Uniform interface | File system folders/files |

### BEHAVIORAL PATTERNS (Object Collaboration)

| Pattern | Purpose | When Use | Key Benefit | Example |
|---------|---------|----------|-----------|---------|
| **Observer** | Notify multiple objects | State changes, pub/sub | Loose coupling | Event emitters |
| **Strategy** | Swap algorithms | Runtime algorithm change | Avoid if/else chains | Payment methods |
| **Command** | Encapsulate request | Undo/redo, task queue | Request as object | Text editor undo |
| **Iterator** | Traverse collections | Hide structure | Uniform access | Tree traversal |
| **State** | Change behavior by state | State machines | Clean state logic | Player states |
| **Template Method** | Algorithm skeleton | Reuse algorithm steps | DRY principle | Data processor |
| **Chain of Responsibility** | Pass request along chain | Flexible handling | Dynamic handlers | Support tickets |

---

## 🎯 PATTERN SELECTION FLOWCHART

```
PROBLEM: Object Creation
├─ Need ONE instance? → SINGLETON
├─ Need different types? → FACTORY
└─ Build complex step-by-step? → BUILDER

PROBLEM: Object Composition
├─ Incompatible interfaces? → ADAPTER
├─ Add optional behavior? → DECORATOR
├─ Simplify complexity? → FACADE
├─ Control access? → PROXY
└─ Tree hierarchy? → COMPOSITE

PROBLEM: Object Collaboration
├─ Multiple observers? → OBSERVER
├─ Swap algorithms? → STRATEGY
├─ Undo/redo? → COMMAND
├─ Traverse collection? → ITERATOR
├─ State machine? → STATE
├─ Reuse algorithm? → TEMPLATE METHOD
└─ Pass along chain? → CHAIN OF RESPONSIBILITY
```

---

## 💻 QUICK CODE TEMPLATES

### Singleton
```javascript
class Singleton {
  static #instance = null;
  static getInstance() {
    if (!Singleton.#instance) {
      Singleton.#instance = new Singleton();
    }
    return Singleton.#instance;
  }
}
```

### Factory
```javascript
class Factory {
  static create(type) {
    const types = {
      'typeA': () => new ClassA(),
      'typeB': () => new ClassB()
    };
    return types[type]?.();
  }
}
```

### Builder
```javascript
class Builder {
  setField1(val) { this.field1 = val; return this; }
  setField2(val) { this.field2 = val; return this; }
  build() { return new Object(this); }
}
```

### Adapter
```javascript
class Adapter extends TargetInterface {
  constructor(incompatible) {
    this.incompatible = incompatible;
  }
  targetMethod() {
    return this.incompatible.differentMethod();
  }
}
```

### Decorator
```javascript
class Decorator {
  constructor(obj) { this.obj = obj; }
  method() {
    // Add behavior before
    const result = this.obj.method();
    // Add behavior after
    return result;
  }
}
```

### Observer
```javascript
class Subject {
  subscribe(observer) { this.observers.push(observer); }
  notify() { this.observers.forEach(o => o.update()); }
}
```

### Strategy
```javascript
class Context {
  constructor(strategy) { this.strategy = strategy; }
  execute() { return this.strategy.execute(); }
}
```

### Command
```javascript
class Command {
  execute() { /* do */ }
  undo() { /* undo */ }
}
class Invoker {
  execute(cmd) { cmd.execute(); this.history.push(cmd); }
}
```

---

## 🎤 TOP 50 INTERVIEW QUESTIONS

### Creational (15 Q's)

1. What is Singleton? When use?
2. Singleton vs static class?
3. Thread-safe Singleton?
4. Singleton problems?
5. What is Factory?
6. Simple vs Abstract Factory?
7. Factory vs Singleton?
8. Builder pattern purpose?
9. Builder vs constructor?
10. Required vs optional in Builder?
11. Fluent interface?
12. Builder disadvantages?
13. When NOT to use Builder?
14. Singleton vs Factory?
15. How to test Singleton?

### Structural (15 Q's)

1. What is Adapter?
2. Adapter vs Facade?
3. Class vs Object Adapter?
4. Two-way adapter?
5. What is Decorator?
6. Decorator vs Inheritance?
7. Order matters in Decorator?
8. Decorator overhead?
9. What is Facade?
10. Facade purpose?
11. What is Proxy?
12. Proxy types (lazy, cache, control)?
13. Proxy vs Decorator?
14. What is Composite?
15. Composite tree structure?

### Behavioral (20 Q's)

1. What is Observer?
2. Observer vs callback?
3. Pub/sub pattern?
4. Observer memory leaks?
5. What is Strategy?
6. Strategy vs Factory?
7. When to use Strategy?
8. What is Command?
9. Command for undo/redo?
10. Command queue?
11. What is Iterator?
12. Iterator vs forEach?
13. What is State?
14. State vs Strategy?
15. What is Template Method?
16. Template Method benefits?
17. What is Chain of Responsibility?
18. Chain vs if/else?
19. Handler order matters?
20. How to implement CoR?

---

## ✅ MASTER CHECKLIST

Before interview, verify:

### Knowledge
- [ ] Define each 15 patterns (2 sentences each)
- [ ] Explain when to use (real conditions)
- [ ] When NOT to use (anti-patterns)
- [ ] Pros and cons each pattern
- [ ] Trade-offs vs benefits

### Implementation
- [ ] Code each pattern in 5 minutes
- [ ] Explain every line
- [ ] Alternative approaches
- [ ] Common mistakes
- [ ] Production examples

### Interview Ready
- [ ] Have 1-2 real-world examples per pattern
- [ ] Can answer "how would you refactor this?"
- [ ] Know pattern relationships (which combine)
- [ ] Understand trade-offs
- [ ] Can code live interview

### Advanced (7-year candidate)
- [ ] Recognize code smells suggesting pattern
- [ ] Combine multiple patterns
- [ ] Know when patterns hurt
- [ ] Custom variations
- [ ] System design with patterns

---

## 🚨 COMMON MISTAKES TO AVOID

### ❌ WRONG
```javascript
// Over-engineering with patterns
class SimpleObject {
  getValue() { return 5; }
}
// Wrapped in Factory, Decorator, Proxy... OVERKILL!

// Using Singleton everywhere
class User { static #instance; }
class Post { static #instance; }
// Only ONE user and ONE post? NO!

// Factory with 200 if/else statements
class Factory {
  static create(type) {
    if(type === 'a') return new A();
    if(type === 'b') return new B();
    // ... 200 more
  }
}
```

### ✅ CORRECT
```javascript
// Use patterns when they solve real problems
// Only One instance needed → Singleton

// Different instances needed → Factory

// Many features → Builder

// Incompatible interfaces → Adapter
```

---

## 📈 PATTERN USAGE IN PRODUCTION

### Netflix Uses
- **Observer** - Event streaming
- **Strategy** - Encoding strategies
- **Adapter** - Multiple device APIs
- **Decorator** - Middleware pipeline
- **Facade** - API gateway

### Stripe Uses
- **Factory** - Payment processor
- **Adapter** - Different payment gateways
- **Builder** - Complex charge object
- **Command** - Transaction history
- **Observer** - Webhook notifications

### React Uses
- **Observer** - State subscriptions
- **Decorator** - HOCs (Higher-Order Components)
- **Strategy** - Rendering strategies
- **Command** - Action dispatching
- **Composite** - Component trees

---

## 🎯 INTERVIEW STRATEGY

### First 2 Minutes
"Design patterns are proven solutions to recurring problems. I focus on solving actual problems rather than forcing patterns. Each pattern has specific trade-offs."

### Show Pattern Knowledge
Give real-world example, not textbook definition:
- "Singleton: I used it for database connection pool so multiple requests share one pool"
- Not: "Singleton restricts class to one instance"

### When Asked to Refactor Code
1. Identify code smell (too many if/else → Strategy)
2. Propose pattern (solve specific problem)
3. Show before/after code
4. Discuss benefits and trade-offs

### Red Flags (Problem Areas)
- ❌ Don't memorize definitions
- ❌ Don't force patterns on simple code
- ❌ Don't mix up similar patterns
- ❌ Don't ignore trade-offs
- ✅ DO focus on problem-solving
- ✅ DO explain WHY pattern helps
- ✅ DO show production code

---

## 🚀 FINAL TIPS FOR 7-YEAR CANDIDATES

### Beyond Basics
1. **Know limitations** - every pattern has trade-offs
2. **Combine patterns** - use multiple patterns together
3. **Recognize patterns** in existing code
4. **Create custom variations** for specific problems
5. **Know when NOT to use** - this shows wisdom

### Interview Confidence
- Master 1-2 patterns deeply
- Have strong example for each
- Can code under pressure
- Explain trade-offs
- Show production experience

### What Interviewers Want to Hear
"I've used this pattern at [company] to solve [specific problem]. It helped us [benefit]. The trade-off was [overhead]."

Not: "The Factory pattern creates objects without specifying the exact class."

---

## 📚 LEARNING RESOURCES

**File Structure:**
- `00_DESIGN_PATTERNS_INDEX.md` - Master overview
- `01_CREATIONAL_SINGLETON.md` - Singleton deep dive
- `02_CREATIONAL_FACTORY.md` - Factory deep dive
- `03_CREATIONAL_BUILDER.md` - Builder deep dive
- `04_STRUCTURAL_ADAPTER.md` - Adapter deep dive
- `05_STRUCTURAL_DECORATOR.md` - Decorator deep dive
- `06-08_STRUCTURAL_PATTERNS_FACADE_PROXY_COMPOSITE.md` - Remaining structural
- `09-15_BEHAVIORAL_PATTERNS_BUNDLE.md` - All behavioral patterns
- This file - Quick reference

---

## 🎓 STUDY SCHEDULE

### Week 1 (Creational)
- Day 1-2: Singleton deep dive + interview questions
- Day 3-4: Factory deep dive + interview questions  
- Day 5-7: Builder deep dive + interview questions

### Week 2 (Structural)
- Day 1-2: Adapter + Decorator + Facade + Proxy
- Day 3-5: Composite pattern
- Day 6-7: Review + practice coding

### Week 3 (Behavioral)
- Day 1-2: Observer + Strategy + Command
- Day 3-4: Iterator + State + Template Method
- Day 5-7: Chain of Responsibility + Review

### Review Week
- Mock interview questions
- Live coding sessions
- System design with patterns
- Refactoring exercises

---

## ✨ YOU'RE NOW READY!

With this comprehensive guide:
- ✅ Know all 15 design patterns
- ✅ Understand when/why/how to use each
- ✅ Can code each from memory
- ✅ Have production examples
- ✅ Know trade-offs
- ✅ Ready for senior-level interviews

**Questions? Revisit specific pattern files for deep dives!**

---

**Last updated: 2024 | For 7-year senior candidates | Production-focused**
