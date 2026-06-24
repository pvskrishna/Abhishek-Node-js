# BEHAVIORAL PATTERNS BUNDLE

## 9. OBSERVER PATTERN

**Definition:** Notifies multiple observers when object state changes (publish/subscribe).

### When to Use
✅ Multiple objects need updates on state change
✅ Event-driven systems, messaging, real-time updates
❌ One-time notifications, simple callbacks

### Code Example
```javascript
// Subject (observable)
class EventEmitter {
  constructor() {
    this.observers = [];
  }
  
  subscribe(observer) {
    this.observers.push(observer);
  }
  
  unsubscribe(observer) {
    this.observers = this.observers.filter(o => o !== observer);
  }
  
  notify(event) {
    this.observers.forEach(o => o.update(event));
  }
}

// Observers (subscribers)
class Logger {
  update(event) {
    console.log(`[LOG] ${event.type}: ${event.message}`);
  }
}

class Analytics {
  update(event) {
    console.log(`[ANALYTICS] Event tracked: ${event.type}`);
  }
}

// Usage
const emitter = new EventEmitter();
const logger = new Logger();
const analytics = new Analytics();

emitter.subscribe(logger);
emitter.subscribe(analytics);

emitter.notify({ type: 'user_signup', message: 'New user registered' });
// [LOG] user_signup: New user registered
// [ANALYTICS] Event tracked: user_signup
```

### Real-World Usage
- DOM events (click, onChange)
- Redux store subscription
- Socket.io messaging
- React hooks (useEffect dependencies)

### Interview Question
**Q: "How is Observer different from Callback?"**
A: "Callback: Single function called
Observer: Multiple subscribers notified, loose coupling, pub/sub model"

---

## 10. STRATEGY PATTERN

**Definition:** Encapsulate interchangeable algorithms so client can select at runtime.

### When to Use
✅ Multiple algorithms for same task
✅ Algorithm changes based on runtime conditions
✅ Want to avoid if/else statements
❌ Single algorithm, not changing

### Code Example
```javascript
// Define strategies
class PaymentStrategy {
  pay(amount) {
    throw new Error('Implement');
  }
}

class CreditCardStrategy extends PaymentStrategy {
  pay(amount) {
    console.log(`Charging credit card: $${amount}`);
    return { success: true, method: 'card' };
  }
}

class PayPalStrategy extends PaymentStrategy {
  pay(amount) {
    console.log(`PayPal payment: $${amount}`);
    return { success: true, method: 'paypal' };
  }
}

class CryptoStrategy extends PaymentStrategy {
  pay(amount) {
    console.log(`Bitcoin payment: ${amount} BTC`);
    return { success: true, method: 'crypto' };
  }
}

// Context uses strategy
class CheckoutService {
  constructor(strategy) {
    this.strategy = strategy;
  }
  
  setStrategy(strategy) {
    this.strategy = strategy;
  }
  
  checkout(amount) {
    return this.strategy.pay(amount);
  }
}

// Usage
const checkout = new CheckoutService(new CreditCardStrategy());
checkout.checkout(100);

// Switch strategy at runtime
checkout.setStrategy(new PayPalStrategy());
checkout.checkout(50);
```

### Interview Question
**Q: "Strategy vs Factory?"**
A: "Factory: Decides which CLASS to instantiate
Strategy: Swaps ALGORITHM/BEHAVIOR at runtime"

---

## 11. COMMAND PATTERN

**Definition:** Encapsulate request as object so you can parameterize, queue, undo.

### When to Use
✅ Undo/redo functionality
✅ Task queues, job scheduling
✅ Macro recording
✅ Deferred execution
❌ Simple function calls

### Code Example
```javascript
// Command interface
class Command {
  execute() { throw new Error('Implement'); }
  undo() { throw new Error('Implement'); }
}

// Concrete commands
class AddTextCommand extends Command {
  constructor(editor, text) {
    super();
    this.editor = editor;
    this.text = text;
  }
  
  execute() {
    this.editor.addText(this.text);
  }
  
  undo() {
    this.editor.removeText(this.text.length);
  }
}

class DeleteTextCommand extends Command {
  constructor(editor, length) {
    super();
    this.editor = editor;
    this.deletedText = '';
    this.length = length;
  }
  
  execute() {
    this.deletedText = this.editor.text.slice(-this.length);
    this.editor.removeText(this.length);
  }
  
  undo() {
    this.editor.addText(this.deletedText);
  }
}

// Invoker (TextEditor) manages command history
class TextEditor {
  constructor() {
    this.text = '';
    this.history = [];
  }
  
  addText(text) {
    this.text += text;
  }
  
  removeText(length) {
    this.text = this.text.slice(0, -length);
  }
  
  executeCommand(command) {
    command.execute();
    this.history.push(command);
  }
  
  undo() {
    const command = this.history.pop();
    if (command) command.undo();
  }
}

// Usage
const editor = new TextEditor();
editor.executeCommand(new AddTextCommand(editor, 'Hello '));
editor.executeCommand(new AddTextCommand(editor, 'World'));
console.log(editor.text); // Hello World

editor.undo();
console.log(editor.text); // Hello 
```

### Interview Question
**Q: "How to implement undo/redo?"**
A: "Keep command history stack. Undo pops and calls undo(). Redo pops from redo stack and calls execute()."

---

## 12. ITERATOR PATTERN

**Definition:** Access elements sequentially without exposing collection structure.

### When to Use
✅ Traverse different collections (array, tree, graph)
✅ Hide internal structure
✅ Multiple simultaneous iterations
❌ Simple arrays (use forEach)

### Code Example
```javascript
class Iterator {
  hasNext() { throw new Error('Implement'); }
  next() { throw new Error('Implement'); }
}

class ArrayIterator extends Iterator {
  constructor(array) {
    super();
    this.array = array;
    this.index = 0;
  }
  
  hasNext() {
    return this.index < this.array.length;
  }
  
  next() {
    return this.array[this.index++];
  }
}

class TreeNode {
  constructor(value) {
    this.value = value;
    this.children = [];
  }
}

class TreeIterator extends Iterator {
  constructor(root) {
    super();
    this.queue = [root];
  }
  
  hasNext() {
    return this.queue.length > 0;
  }
  
  next() {
    const node = this.queue.shift();
    this.queue.push(...node.children);
    return node.value;
  }
}

// Usage - same interface, different collections
const iterator1 = new ArrayIterator([1, 2, 3]);
while (iterator1.hasNext()) {
  console.log(iterator1.next());
}

const root = new TreeNode(1);
const iterator2 = new TreeIterator(root);
while (iterator2.hasNext()) {
  console.log(iterator2.next());
}
```

---

## 13. STATE PATTERN

**Definition:** Change object behavior based on internal state without big if/else.

### When to Use
✅ Object behavior varies by state
✅ State machines
✅ Multiple state-specific operations
❌ Few states, simple logic

### Code Example
```javascript
class State {
  enter() {}
  exit() {}
}

class Player {
  constructor() {
    this.state = new IdleState();
  }
  
  play() {
    this.state.enter(); // Can do state-specific setup
    this.currentState = 'playing';
  }
  
  pause() {
    this.state.exit(); // Cleanup
    this.currentState = 'paused';
  }
}

class IdleState extends State {
  enter() {
    console.log('Player idle - ready to play');
  }
}

class PlayingState extends State {
  enter() {
    console.log('Now playing...');
  }
  
  exit() {
    console.log('Stopping playback');
  }
}

class PausedState extends State {
  enter() {
    console.log('Paused - press play to resume');
  }
}
```

---

## 14. TEMPLATE METHOD PATTERN

**Definition:** Define algorithm skeleton in base class, subclasses override steps.

### Code Example
```javascript
class DataProcessor {
  process(data) {
    // Template method - skeleton
    const validated = this.validate(data);
    const transformed = this.transform(validated);
    const result = this.save(transformed);
    return result;
  }
  
  validate(data) {
    throw new Error('Implement');
  }
  
  transform(data) {
    throw new Error('Implement');
  }
  
  save(data) {
    throw new Error('Implement');
  }
}

class JSONProcessor extends DataProcessor {
  validate(data) {
    console.log('Validating JSON');
    return JSON.parse(data);
  }
  
  transform(data) {
    console.log('Transforming JSON');
    return data;
  }
  
  save(data) {
    console.log('Saving JSON');
    return data;
  }
}

// Usage
const processor = new JSONProcessor();
processor.process('{}');
```

---

## 15. CHAIN OF RESPONSIBILITY PATTERN

**Definition:** Pass request along chain of handlers until one handles it.

### Code Example
```javascript
class Handler {
  setNext(handler) {
    this.nextHandler = handler;
    return handler;
  }
  
  handle(request) {
    if (this.canHandle(request)) {
      return this.process(request);
    }
    
    if (this.nextHandler) {
      return this.nextHandler.handle(request);
    }
    
    throw new Error('No handler found');
  }
  
  canHandle(request) {
    throw new Error('Implement');
  }
  
  process(request) {
    throw new Error('Implement');
  }
}

class SupportLevel1 extends Handler {
  canHandle(request) {
    return request.priority === 'low';
  }
  
  process(request) {
    console.log('Level 1 support handling:', request);
    return 'Resolved by Level 1';
  }
}

class SupportLevel2 extends Handler {
  canHandle(request) {
    return request.priority === 'medium';
  }
  
  process(request) {
    console.log('Level 2 support handling:', request);
    return 'Resolved by Level 2';
  }
}

// Usage
const level1 = new SupportLevel1();
level1.setNext(new SupportLevel2());

level1.handle({ priority: 'medium', issue: 'bug' });
// Level 2 support handling: ...
```

---

## 📚 QUICK REFERENCE

| Pattern | Use When | Key Benefit |
|---------|----------|------------|
| Observer | Multiple subscribers | Loose coupling, pub/sub |
| Strategy | Runtime algorithm swap | Avoid if/else |
| Command | Undo/redo, task queue | Encapsulate request |
| Iterator | Traverse collections | Hide structure |
| State | State machine | Clean state logic |
| Template | Reuse algorithm | DRY principle |
| Chain | Pass request chain | Flexible handling |

---

## 🎤 KEY INTERVIEW TAKEAWAYS

**Observer:** "Notify multiple objects of state change"
**Strategy:** "Swap algorithms at runtime"
**Command:** "Encapsulate request for undo/redo"
**Iterator:** "Traverse without exposing structure"
**State:** "Change behavior based on state"
**Template Method:** "Define algorithm skeleton"
**Chain of Responsibility:** "Pass request along chain"

---

## ✅ MASTERY CHECKLIST

- ✅ Understand each pattern's purpose
- ✅ Know when NOT to use each
- ✅ Can implement in 5 minutes
- ✅ Have production example for each
- ✅ Explain trade-offs
- ✅ Answer 5 interview questions per pattern

**You're now ready for design pattern interviews!**
