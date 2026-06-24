# STRUCTURAL PATTERNS - FACADE, PROXY, COMPOSITE

## 6. FACADE PATTERN

**Definition:** Provide unified, simplified interface to complex subsystem.

### When to Use
✅ Complex system with multiple components
✅ Want to hide complexity from client
✅ Simplify public API
✅ Create entry point to subsystem
❌ Already simple system

### Code Example

```javascript
// Complex subsystem - many parts
class AudioMixer {
  mix(audio) { console.log('Mixing audio'); return audio; }
}

class VideoProcessor {
  process(video) { console.log('Processing video'); return video; }
}

class Encoder {
  encode(data) { console.log('Encoding'); return data; }
}

class AudioEncoder {
  encode(audio) { console.log('Encoding audio'); return audio; }
}

// FACADE - Simplifies access to complex subsystem
class MediaConverter {
  constructor() {
    this.audioMixer = new AudioMixer();
    this.videoProcessor = new VideoProcessor();
    this.encoder = new Encoder();
    this.audioEncoder = new AudioEncoder();
  }
  
  // Single simple method instead of using all components
  convertToMp4(mediaFile) {
    console.log('Starting conversion to MP4...');
    
    // Client doesn't need to know about these steps
    const processedVideo = this.videoProcessor.process(mediaFile.video);
    const mixedAudio = this.audioMixer.mix(mediaFile.audio);
    const encodedVideo = this.encoder.encode(processedVideo);
    const encodedAudio = this.audioEncoder.encode(mixedAudio);
    
    console.log('Conversion complete!');
    return { video: encodedVideo, audio: encodedAudio };
  }
}

// Usage - simple!
const converter = new MediaConverter();
const result = converter.convertToMp4({ 
  video: 'video.mov', 
  audio: 'audio.wav' 
});
// Starting conversion to MP4...
// Processing video
// Mixing audio
// Encoding
// Encoding audio
// Conversion complete!
```

### Real-World Examples
```javascript
// DOM API is Facade
document.getElementById('btn').addEventListener('click', handler);
// Hides: event delegation, bubbling, capture phases, etc.

// jQuery is Facade
$('#btn').click(handler);
// Simplifies cross-browser compatibility

// Node.js fs module
fs.readFile('file.txt', callback);
// Hides: file descriptors, buffer management, permissions
```

### Interview Questions

**Q1: "Facade vs Adapter?"**
A: 
- Facade: Simplifies complex subsystem (INSIDE)
- Adapter: Makes incompatible interfaces compatible (BETWEEN)

**Q2: "When is Facade useful?"**
A: "Large systems with many components. Client should only know one entry point, not 10 classes."

---

## 7. PROXY PATTERN

**Definition:** Control access to another object by providing substitute/placeholder.

### When to Use
✅ Lazy loading (create expensive object on first access)
✅ Access control (check permissions before access)
✅ Caching (cache expensive computation)
✅ Logging/monitoring access
✅ Remote objects
❌ Simple direct access

### Code Examples

**Lazy Loading Proxy**
```javascript
// Real subject - expensive to create
class ExpensiveDatabase {
  constructor() {
    console.log('Creating expensive database connection');
    this.connected = true;
  }
  
  query(sql) {
    return `Result: ${sql}`;
  }
}

// PROXY - Lazy loads the real database
class DatabaseProxy {
  constructor() {
    this.database = null; // Not created yet
  }
  
  query(sql) {
    // Create on first access only
    if (!this.database) {
      this.database = new ExpensiveDatabase();
    }
    return this.database.query(sql);
  }
}

// Usage
const db = new DatabaseProxy();
// Database not created yet

db.query('SELECT * FROM users');
// Creating expensive database connection
// Result: SELECT * FROM users
// NOW database exists
```

**Access Control Proxy**
```javascript
class FileSystem {
  readFile(path) { return `Content of ${path}`; }
  deleteFile(path) { return `Deleted ${path}`; }
}

// PROXY with access control
class SecureFileSystemProxy {
  constructor(user, fileSystem) {
    this.user = user;
    this.fileSystem = fileSystem;
  }
  
  readFile(path) {
    if (this.user.hasReadPermission(path)) {
      return this.fileSystem.readFile(path);
    }
    throw new Error('Access denied');
  }
  
  deleteFile(path) {
    if (this.user.hasDeletePermission(path)) {
      return this.fileSystem.deleteFile(path);
    }
    throw new Error('Access denied');
  }
}

// Usage
const user = { 
  hasReadPermission: () => true, 
  hasDeletePermission: () => false 
};

const fs = new SecureFileSystemProxy(user, new FileSystem());
fs.readFile('/docs/report.pdf'); // OK
fs.deleteFile('/docs/report.pdf'); // ERROR: Access denied
```

**Caching Proxy**
```javascript
class DataService {
  fetchUser(id) {
    console.log(`Fetching user ${id} from server...`);
    return { id, name: `User ${id}`, timestamp: Date.now() };
  }
}

// PROXY with caching
class CachingDataServiceProxy {
  constructor(service) {
    this.service = service;
    this.cache = new Map();
  }
  
  fetchUser(id) {
    // Check cache first
    if (this.cache.has(id)) {
      console.log(`Cache hit for user ${id}`);
      return this.cache.get(id);
    }
    
    // Not in cache - fetch and store
    console.log(`Cache miss for user ${id}`);
    const user = this.service.fetchUser(id);
    this.cache.set(id, user);
    return user;
  }
}

// Usage
const service = new CachingDataServiceProxy(new DataService());
service.fetchUser(1); // Cache miss, fetches from server
service.fetchUser(1); // Cache hit, returns cached
service.fetchUser(2); // Cache miss, fetches from server
```

---

## 8. COMPOSITE PATTERN

**Definition:** Compose objects into tree structures to treat individual and composite uniformly.

### When to Use
✅ Tree-like hierarchies (folders/files, org charts)
✅ Treat leaf and composite same way
✅ Recursive composition
❌ Flat structures

### Code Example

```javascript
// COMPONENT interface
class FileSystemItem {
  getSize() { throw new Error('Implement'); }
  display() { throw new Error('Implement'); }
}

// LEAF - individual item
class File extends FileSystemItem {
  constructor(name, size) {
    super();
    this.name = name;
    this.size = size;
  }
  
  getSize() {
    return this.size;
  }
  
  display(indent = '') {
    console.log(`${indent}📄 ${this.name} (${this.size}KB)`);
  }
}

// COMPOSITE - can contain children
class Folder extends FileSystemItem {
  constructor(name) {
    super();
    this.name = name;
    this.children = [];
  }
  
  add(item) {
    this.children.push(item);
  }
  
  remove(item) {
    this.children = this.children.filter(c => c !== item);
  }
  
  getSize() {
    // Recursive sum of all children
    return this.children.reduce((total, child) => total + child.getSize(), 0);
  }
  
  display(indent = '') {
    console.log(`${indent}📁 ${this.name}`);
    this.children.forEach(child => child.display(indent + '  '));
  }
}

// Usage - treat files and folders uniformly
const root = new Folder('root');

const docFolder = new Folder('Documents');
docFolder.add(new File('resume.pdf', 200));
docFolder.add(new File('coverletter.doc', 100));

const photoFolder = new Folder('Photos');
photoFolder.add(new File('vacation.jpg', 2000));
photoFolder.add(new File('family.jpg', 1500));

root.add(docFolder);
root.add(photoFolder);
root.add(new File('readme.txt', 50));

// Display tree
root.display();
// 📁 root
//   📁 Documents
//     📄 resume.pdf (200KB)
//     📄 coverletter.doc (100KB)
//   📁 Photos
//     📄 vacation.jpg (2000KB)
//     📄 family.jpg (1500KB)
//   📄 readme.txt (50KB)

// Get total size - works for both files and folders!
console.log(`Total: ${root.getSize()}KB`); // 3850KB
```

### Interview Question

**Q: "How is Composite different from Decorator?"**
A:
- Composite: Tree hierarchy (parent-child)
- Decorator: Linear chain (wrapper)

```javascript
// Composite - tree
folder.add(file1);
folder.add(folder2);
folder2.add(file2);

// Decorator - chain
decorator1(decorator2(decorator3(object)))
```

---

## 📊 COMPARISON TABLE

| Pattern | Purpose | Structure | When |
|---------|---------|-----------|------|
| **Facade** | Simplify complex system | One entry point | Hide complexity |
| **Proxy** | Control access | Substitution | Lazy/cache/control |
| **Composite** | Tree hierarchy | Recursive | Files/folders/org |

---

## 🎤 INTERVIEW TIPS

### All Structural Patterns
**Q: "What's the main difference between structural patterns?"**
A:
- Adapter: Makes incompatible work together
- Decorator: Adds behavior dynamically
- Facade: Simplifies complex system
- Proxy: Controls access
- Composite: Tree hierarchies

### Code Interview Red Flags
- ❌ Using `new` everywhere (should use Factory)
- ❌ God object doing everything (should use Facade)
- ❌ Many if/else statements (should use Strategy)
- ❌ No way to undo (should use Command)
- ❌ Tight coupling to concrete classes (should use Adapter/Factory)

---

## ✅ KEY TAKEAWAYS

1. **Facade** - single entry point to complex subsystem
2. **Proxy** - control access (lazy, cache, permissions)
3. **Composite** - tree structures with uniform interface
4. **All three** solve different composition problems
5. **Know when** each one solves the problem
6. **Production examples** for each pattern
7. **Trade-offs** - complexity vs benefit

---

**Now master all 15 patterns for senior-level interviews! 🚀**
