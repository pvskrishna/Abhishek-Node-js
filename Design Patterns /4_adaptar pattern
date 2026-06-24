# ADAPTER PATTERN - COMPREHENSIVE GUIDE

---

## 📌 DEFINITION

**Adapter** is a structural pattern that **converts one interface into another** that clients expect. It allows objects with incompatible interfaces to collaborate by wrapping one object with an adapter that translates calls.

### Core Concept
```
Problem: Two incompatible interfaces need to work together
Solution: Create adapter that translates between them
Like: Power adapter converting 110V to 220V, or USB-C to USB adapter
```

---

## 🎯 WHEN TO USE

### ✅ Use Adapter When:
- **Third-party library** has different interface than your app
- **Legacy code** needs to work with new code
- **Multiple similar objects** need common interface
- **Want to isolate** incompatible code
- **Testing** with mock objects of different interface

### ❌ Don't Use When:
- **Can modify source code** - just fix interface
- **Simple wrapper** not needed - use delegation
- **Overcomplicating** integration
- **Interfaces already compatible**

---

## 🔧 HOW IT WORKS

### Implementation Steps:
1. **Identify incompatible interfaces**
2. **Create adapter class** implementing target interface
3. **Adapter wraps** the incompatible object
4. **Translate method calls** in adapter
5. **Client uses adapter** without knowing complexity

---

## 💻 CODE EXAMPLES

### ❌ WRONG - Direct Usage of Incompatible Interface

```javascript
// Third-party library - different interface
class ExternalPaymentGateway {
  processTransaction(txn) { // Different method name!
    console.log(`Processing: ${txn.amount}`);
    return { status: 'success', id: Math.random() };
  }
}

// Our app expects this interface
class PaymentProcessor {
  pay(amount, currency) { // Different method signature!
    // Can't use ExternalPaymentGateway directly!
  }
}

// PROBLEM: Can't use them together!
const gateway = new ExternalPaymentGateway();
const processor = new PaymentProcessor();

// Method names don't match
processor.pay(100, 'USD'); // Our interface
gateway.processTransaction({amount: 100}); // Gateway interface

// Mixing different interfaces throughout code - nightmare!
```

---

### ✅ CORRECT - Adapter Pattern

**Class Adapter (Inheritance)**
```javascript
// External library interface - can't change
class ExternalPaymentGateway {
  processTransaction(txn) {
    console.log(`Processing ${txn.amount}`);
    return { status: 'success', id: Math.random() };
  }
}

// Our app's expected interface
class PaymentProcessor {
  pay(amount, currency) {
    throw new Error('Must implement');
  }
}

// ADAPTER: Bridges the gap!
class PaymentGatewayAdapter extends PaymentProcessor {
  constructor(gateway) {
    super();
    this.gateway = gateway; // Wrap external gateway
  }
  
  // Translate: our interface → external interface
  pay(amount, currency) {
    // Our format
    const transactionData = {
      amount: amount,
      currency: currency,
      timestamp: Date.now()
    };
    
    // Call external gateway's method
    const result = this.gateway.processTransaction(transactionData);
    
    // Return in our expected format
    return {
      success: result.status === 'success',
      transactionId: result.id,
      amount: amount,
      currency: currency
    };
  }
}

// Usage: Client uses adapter, not original
const externalGateway = new ExternalPaymentGateway();
const adapter = new PaymentGatewayAdapter(externalGateway);

// Now we can use our interface!
const result = adapter.pay(100, 'USD');
console.log(result); // { success: true, transactionId: ..., amount: 100, currency: 'USD' }
```

---

**Object Adapter (Composition - Preferred)**
```javascript
// External APIs - incompatible with each other
class StripeAPI {
  charge(amount, cardToken) {
    console.log(`Stripe charging $${amount}`);
    return { txnId: 'stripe_' + Math.random() };
  }
}

class PayPalAPI {
  makePayment(recipient, amount, currency) {
    console.log(`PayPal payment to ${recipient}: ${currency} ${amount}`);
    return { orderId: 'pp_' + Math.random() };
  }
}

class SquareAPI {
  pay(tenantId, request) {
    console.log(`Square payment for tenant ${tenantId}`);
    return { resultCode: 'SUCCESS', transactionId: 'sq_' + Math.random() };
  }
}

// Common interface we want
class PaymentProvider {
  pay(amount, currency) {
    throw new Error('Must implement');
  }
}

// ADAPTER 1: Stripe → Common interface
class StripeAdapter extends PaymentProvider {
  constructor(stripeAPI, cardToken) {
    super();
    this.api = stripeAPI;
    this.cardToken = cardToken;
  }
  
  pay(amount, currency) {
    const result = this.api.charge(amount * 100, this.cardToken); // in cents
    return { transactionId: result.txnId, amount, currency };
  }
}

// ADAPTER 2: PayPal → Common interface
class PayPalAdapter extends PaymentProvider {
  constructor(paypalAPI, recipient) {
    super();
    this.api = paypalAPI;
    this.recipient = recipient;
  }
  
  pay(amount, currency) {
    const result = this.api.makePayment(this.recipient, amount, currency);
    return { transactionId: result.orderId, amount, currency };
  }
}

// ADAPTER 3: Square → Common interface
class SquareAdapter extends PaymentProvider {
  constructor(squareAPI, tenantId) {
    super();
    this.api = squareAPI;
    this.tenantId = tenantId;
  }
  
  pay(amount, currency) {
    const result = this.api.pay(this.tenantId, { amount, currency });
    return { 
      transactionId: result.transactionId, 
      amount, 
      currency,
      success: result.resultCode === 'SUCCESS'
    };
  }
}

// Now all use same interface!
function processPayment(provider, amount, currency) {
  const result = provider.pay(amount, currency);
  console.log(`Payment: ${result.amount} ${result.currency}`);
  return result;
}

// Can swap providers easily - same interface!
const stripe = new StripeAdapter(new StripeAPI(), 'tok_visa');
const paypal = new PayPalAdapter(new PayPalAPI(), 'store@example.com');
const square = new SquareAdapter(new SquareAPI(), 'tenant123');

processPayment(stripe, 99.99, 'USD');
processPayment(paypal, 99.99, 'USD');
processPayment(square, 99.99, 'USD');

// All work the same way despite different underlying APIs!
```

---

**Data Format Adapter**
```javascript
// Legacy system returns XML
class LegacyXMLParser {
  getData() {
    return `
      <user>
        <id>123</id>
        <firstName>John</firstName>
        <lastName>Doe</lastName>
        <email>john@example.com</email>
      </user>
    `;
  }
}

// New system expects JSON
class JSONUser {
  constructor(id, firstName, lastName, email) {
    this.id = id;
    this.firstName = firstName;
    this.lastName = lastName;
    this.email = email;
  }
  
  getDisplayName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

// ADAPTER: XML to JSON transformation
class XMLToJSONAdapter {
  constructor(xmlParser) {
    this.xmlParser = xmlParser;
  }
  
  getUser() {
    const xmlString = this.xmlParser.getData();
    
    // Parse XML to JSON
    const xmlDoc = new DOMParser().parseFromString(xmlString, 'text/xml');
    const userEl = xmlDoc.getElementsByTagName('user')[0];
    
    return new JSONUser(
      parseInt(userEl.getElementsByTagName('id')[0].textContent),
      userEl.getElementsByTagName('firstName')[0].textContent,
      userEl.getElementsByTagName('lastName')[0].textContent,
      userEl.getElementsByTagName('email')[0].textContent
    );
  }
}

// Usage
const legacyParser = new LegacyXMLParser();
const adapter = new XMLToJSONAdapter(legacyParser);
const user = adapter.getUser();

console.log(user.getDisplayName()); // Works with new format!
```

---

**Two-Way Adapter (Bidirectional)**
```javascript
class OldUserSystem {
  // Old format: firstName, lastName
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
}

class NewUserSystem {
  // New format: fullName
  constructor(fullName) {
    this.fullName = fullName;
  }
}

// TWO-WAY ADAPTER
class UserSystemAdapter {
  constructor(user) {
    this.user = user;
  }
  
  // Adapt from old to new
  getAsNewFormat() {
    if (this.user.firstName && this.user.lastName) {
      const fullName = `${this.user.firstName} ${this.user.lastName}`;
      return new NewUserSystem(fullName);
    }
    throw new Error('Invalid old format');
  }
  
  // Adapt from new to old
  getAsOldFormat() {
    if (this.user.fullName) {
      const [firstName, lastName] = this.user.fullName.split(' ');
      return new OldUserSystem(firstName, lastName);
    }
    throw new Error('Invalid new format');
  }
  
  // Work with both formats
  getFullName() {
    if (this.user.fullName) return this.user.fullName; // New format
    if (this.user.firstName) return `${this.user.firstName} ${this.user.lastName}`; // Old
  }
}

// Usage
const oldUser = new OldUserSystem('John', 'Doe');
const adapter = new UserSystemAdapter(oldUser);

// Can work with both formats
console.log(adapter.getFullName()); // John Doe
const newFormatUser = adapter.getAsNewFormat();
console.log(newFormatUser.fullName); // John Doe
```

---

## 🌍 REAL-TIME USAGE (PRODUCTION EXAMPLES)

### Example 1: Payment Integration (Stripe Example)
```javascript
// Stripe's API
class StripePaymentAPI {
  createCharge(params) {
    return { id: 'ch_' + Math.random(), status: 'succeeded' };
  }
}

// Our system's interface
class InternalPaymentService {
  processPayment(amount, currency) { }
}

// ADAPTER
class StripePaymentAdapter extends InternalPaymentService {
  constructor(stripeAPI) {
    super();
    this.stripe = stripeAPI;
  }
  
  processPayment(amount, currency) {
    const result = this.stripe.createCharge({
      amount: Math.round(amount * 100), // Convert to cents
      currency: currency.toLowerCase(),
      source: 'tok_visa'
    });
    
    return {
      success: result.status === 'succeeded',
      transactionId: result.id,
      amount,
      currency
    };
  }
}

const stripeAPI = new StripePaymentAPI();
const paymentService = new StripePaymentAdapter(stripeAPI);
```

---

### Example 2: Database Adapter (Different Databases)
```javascript
// MongoDB interface
class MongoDBConnection {
  find(collection, query) {
    return [{ _id: 1, name: 'John' }];
  }
}

// Our app expects different interface
class DatabaseQuery {
  execute() { }
}

class MongoDBAdapter extends DatabaseQuery {
  constructor(mongo) {
    super();
    this.mongo = mongo;
  }
  
  execute(collection, query) {
    return this.mongo.find(collection, query);
  }
}
```

---

### Example 3: Media Player Adapter
```javascript
class MP3Player {
  playMP3(file) { console.log(`Playing MP3: ${file}`); }
}

class MediaPlayer {
  play(file, format) { }
}

class MP3Adapter extends MediaPlayer {
  constructor(mp3Player) {
    super();
    this.player = mp3Player;
  }
  
  play(file, format) {
    if (format === 'mp3') {
      this.player.playMP3(file);
    }
  }
}
```

---

## 📊 DIAGRAM

```mermaid
graph TD
    Client["Client<br/>expects PaymentProvider"]
    Target["PaymentProvider<br/>Interface"]
    Adapter["PaymentGatewayAdapter<br/>implements PaymentProvider"]
    External["ExternalPaymentGateway<br/>processTransaction()"]
    
    Client -->|uses| Adapter
    Adapter -.implements.-> Target
    Adapter -->|wraps| External
    Adapter -->|translates| External
    
    style Adapter fill:#90EE90
    style Target fill:#87CEEB
    style External fill:#FFB6C1
```

---

## ⚖️ PROS & CONS

| Aspect | Pros | Cons |
|--------|------|------|
| **Integration** | Easy to use incompatible code | Extra layer of abstraction |
| **Isolation** | Isolates incompatible code | Performance overhead |
| **Testing** | Easy to swap implementations | More code to maintain |
| **Maintenance** | Centralized translation | Added complexity |
| **Flexibility** | Support multiple interfaces | Not needed if compatible |

---

## 🎤 TOP INTERVIEW QUESTIONS & ANSWERS

### Q1: "What is Adapter and why use it?"
**Answer:**
"Adapter converts incompatible interfaces into compatible ones. Like a power adapter converting 110V to 220V.

Use when:
- Integrating third-party library with different interface
- Legacy code with new code
- Multiple objects need same interface

Example: Stripe, PayPal, Square payment gateways all have different APIs → create adapters for each → client uses common interface."

---

### Q2: "Difference between Adapter and Facade?"
**Answer:**
"Adapter: Makes incompatible interfaces compatible (TRANSLATION)
Facade: Simplifies complex subsystem (SIMPLIFICATION)

```javascript
// Adapter - translates between A and B
StripeAdapter translates between Stripe API and our PaymentProvider

// Facade - simplifies complex system
PaymentFacade handles Stripe, PayPal, Square all at once
```"

---

### Q3: "Class Adapter vs Object Adapter?"
**Answer:**
"Class Adapter: Inheritance-based
```javascript
class PaymentAdapter extends PaymentProcessor {
  // Inherits interface
}
```

Object Adapter: Composition-based (PREFERRED)
```javascript
class PaymentAdapter extends PaymentProcessor {
  constructor(gateway) { this.gateway = gateway; }
  // Wraps gateway
}
```

Object Adapter is flexible - can swap implementations without changing adapter."

---

### Q4: "How to implement bidirectional adapter?"
**Answer:**
"Support both directions of translation:

```javascript
class TwoWayAdapter {
  constructor(user) {
    this.user = user;
  }
  
  // Old to new
  getAsNewFormat() { ... }
  
  // New to old
  getAsOldFormat() { ... }
}
```"

---

### Q5: "When should you NOT use Adapter?"
**Answer:**
"Don't use when:
1. You can modify source code - just fix it
2. Interfaces already compatible - unnecessary
3. Overcomplicating simple mapping
4. Performance critical - adds overhead

Use when external library can't be modified and integration is necessary."

---

## ✅ KEY TAKEAWAYS

1. **Adapter** converts incompatible interfaces
2. **Use for** third-party libraries, legacy code
3. **Object Adapter** preferred over class (composition > inheritance)
4. **Isolates** incompatibility in one place
5. **Supports** Open/Closed Principle
6. **Perfect for** payment gateways, database drivers, APIs
7. **Trade-off**: Extra layer vs integration flexibility

---

## 📚 NEXT PATTERN

Ready to learn **Decorator Pattern**? Go to `05_STRUCTURAL_DECORATOR.md`

Adds behavior to objects dynamically without modifying them.

