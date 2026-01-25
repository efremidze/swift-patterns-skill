# Code Review & Refactoring Skill

Expert guidance for identifying code smells, refactoring patterns, and conducting effective code reviews in Swift and SwiftUI projects.

## Rules

### 1. Look for Code Smells
- **Long methods** - Break down functions over 20-30 lines
- **Large classes** - Split classes with multiple responsibilities
- **Duplicate code** - Extract common functionality
- **Magic numbers** - Replace with named constants
- **Deep nesting** - Flatten with early returns
- **Poor naming** - Use descriptive, intention-revealing names

### 2. Follow SOLID Principles
- **Single Responsibility** - Each class/function does one thing
- **Open/Closed** - Open for extension, closed for modification
- **Liskov Substitution** - Subtypes must be substitutable
- **Interface Segregation** - Many specific interfaces over one general
- **Dependency Inversion** - Depend on abstractions, not concretions

### 3. Check for Common Issues
- **Force unwrapping** - Use optional binding or guard
- **Retain cycles** - Check weak/unowned references
- **Error handling** - Don't swallow errors silently
- **Thread safety** - Verify concurrent access patterns
- **Memory leaks** - Check with Instruments

### 4. Review for Maintainability
- **Clear intent** - Code should read like prose
- **Consistent style** - Follow project conventions
- **Appropriate comments** - Explain why, not what
- **Testability** - Can this code be tested?
- **Documentation** - Public APIs should be documented

### 5. Refactor Incrementally
- Make one change at a time
- Run tests after each change
- Commit working code frequently
- Don't mix refactoring with feature work
- Keep refactorings small and focused

### 6. Prioritize Readability
- Code is read more than written
- Favor clarity over cleverness
- Use Swift's expressive features appropriately
- Avoid premature optimization

## Tradeoffs

### Refactoring vs New Features
**Refactoring First:**
- Cleaner codebase
- Faster future development
- Immediate time investment
- Risk of breaking existing code

**Features First:**
- Faster time to market
- Growing technical debt
- Harder future changes
- Lower initial cost

### Abstraction vs Simplicity
**More Abstraction:**
- More flexible
- Better testability
- More complex
- Harder to understand

**Less Abstraction:**
- Simpler code
- Easier to understand
- Less flexible
- May need refactoring later

## Output Template

```swift
// MARK: - Code Smell: Long Method
// Before (BAD): Long method doing too much
class UserService {
    func processUser(_ user: User) async throws {
        // Validate user
        guard !user.email.isEmpty else { throw ValidationError.emptyEmail }
        guard user.email.contains("@") else { throw ValidationError.invalidEmail }
        guard user.age >= 18 else { throw ValidationError.underage }
        
        // Save to database
        let db = Database.shared
        try await db.connect()
        let query = "INSERT INTO users (name, email, age) VALUES (?, ?, ?)"
        try await db.execute(query, parameters: [user.name, user.email, user.age])
        try await db.disconnect()
        
        // Send welcome email
        let emailService = EmailService()
        let template = EmailTemplate.welcome
        let subject = "Welcome to our app!"
        let body = "Hello \(user.name), welcome!"
        try await emailService.send(to: user.email, subject: subject, body: body)
        
        // Log analytics
        Analytics.shared.track("user_registered", properties: [
            "user_id": user.id,
            "email": user.email,
            "age": user.age
        ])
    }
}

// After (GOOD): Extracted into focused methods
class UserService {
    private let database: DatabaseProtocol
    private let emailService: EmailServiceProtocol
    private let analytics: AnalyticsProtocol
    
    init(
        database: DatabaseProtocol,
        emailService: EmailServiceProtocol,
        analytics: AnalyticsProtocol
    ) {
        self.database = database
        self.emailService = emailService
        self.analytics = analytics
    }
    
    func processUser(_ user: User) async throws {
        try validateUser(user)
        try await saveUser(user)
        try await sendWelcomeEmail(to: user)
        await trackUserRegistration(user)
    }
    
    private func validateUser(_ user: User) throws {
        let validator = UserValidator()
        try validator.validate(user)
    }
    
    private func saveUser(_ user: User) async throws {
        try await database.save(user)
    }
    
    private func sendWelcomeEmail(to user: User) async throws {
        try await emailService.sendWelcome(to: user.email, name: user.name)
    }
    
    private func trackUserRegistration(_ user: User) async {
        await analytics.track(.userRegistered(userId: user.id))
    }
}

// MARK: - Code Smell: God Object
// Before (BAD): Class doing everything
class AppManager {
    var currentUser: User?
    var settings: Settings
    var networkClient: NetworkClient
    var database: Database
    var cache: Cache
    
    func login(email: String, password: String) async throws { }
    func logout() { }
    func fetchPosts() async throws -> [Post] { }
    func savePost(_ post: Post) async throws { }
    func updateSettings(_ settings: Settings) { }
    func clearCache() { }
    // ... 50+ more methods
}

// After (GOOD): Separated concerns
protocol AuthServiceProtocol {
    func login(email: String, password: String) async throws -> User
    func logout() async throws
}

protocol PostServiceProtocol {
    func fetchPosts() async throws -> [Post]
    func savePost(_ post: Post) async throws
}

protocol SettingsServiceProtocol {
    var currentSettings: Settings { get }
    func update(_ settings: Settings) async throws
}

class AuthService: AuthServiceProtocol {
    private let networkClient: NetworkClient
    private let database: Database
    
    init(networkClient: NetworkClient, database: Database) {
        self.networkClient = networkClient
        self.database = database
    }
    
    func login(email: String, password: String) async throws -> User {
        // Implementation
        fatalError()
    }
    
    func logout() async throws {
        // Implementation
    }
}

// MARK: - Code Smell: Force Unwrapping
// Before (BAD): Dangerous force unwraps
func getUsername() -> String {
    let user = UserDefaults.standard.string(forKey: "username")
    return user!  // Crashes if nil!
}

// After (GOOD): Safe optional handling
func getUsername() -> String? {
    UserDefaults.standard.string(forKey: "username")
}

// Or with default value
func getUsername() -> String {
    UserDefaults.standard.string(forKey: "username") ?? "Guest"
}

// Or with guard
func processUsername() {
    guard let username = UserDefaults.standard.string(forKey: "username") else {
        print("No username found")
        return
    }
    
    print("Hello, \(username)")
}

// MARK: - Code Smell: Pyramid of Doom
// Before (BAD): Deep nesting
func processData(_ data: Data?) {
    if let data = data {
        if let string = String(data: data, encoding: .utf8) {
            if let json = try? JSONDecoder().decode(Response.self, from: data) {
                if json.isValid {
                    if let result = json.result {
                        // Finally do something
                        print(result)
                    }
                }
            }
        }
    }
}

// After (GOOD): Guard statements
func processData(_ data: Data?) {
    guard let data = data else {
        print("No data")
        return
    }
    
    guard let string = String(data: data, encoding: .utf8) else {
        print("Invalid encoding")
        return
    }
    
    guard let json = try? JSONDecoder().decode(Response.self, from: data),
          json.isValid else {
        print("Invalid JSON")
        return
    }
    
    guard let result = json.result else {
        print("No result")
        return
    }
    
    print(result)
}

// MARK: - Code Smell: Magic Numbers
// Before (BAD): Unexplained constants
func calculateDiscount(for price: Double) -> Double {
    if price > 100 {
        return price * 0.15
    } else if price > 50 {
        return price * 0.10
    }
    return 0
}

// After (GOOD): Named constants
enum DiscountThreshold {
    static let premium: Double = 100
    static let standard: Double = 50
}

enum DiscountRate {
    static let premium: Double = 0.15
    static let standard: Double = 0.10
}

func calculateDiscount(for price: Double) -> Double {
    if price > DiscountThreshold.premium {
        return price * DiscountRate.premium
    } else if price > DiscountThreshold.standard {
        return price * DiscountRate.standard
    }
    return 0
}

// MARK: - Code Smell: Duplicate Code
// Before (BAD): Repeated logic
func validateEmail(_ email: String) -> Bool {
    return email.contains("@") && email.contains(".")
}

func validateBackupEmail(_ email: String) -> Bool {
    return email.contains("@") && email.contains(".")
}

func validateContactEmail(_ email: String) -> Bool {
    return email.contains("@") && email.contains(".")
}

// After (GOOD): Extracted common logic
struct EmailValidator {
    static func isValid(_ email: String) -> Bool {
        // Better validation
        let pattern = "[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,64}"
        let predicate = NSPredicate(format: "SELF MATCHES %@", pattern)
        return predicate.evaluate(with: email)
    }
}

// Usage
let isEmailValid = EmailValidator.isValid(email)
let isBackupValid = EmailValidator.isValid(backupEmail)
let isContactValid = EmailValidator.isValid(contactEmail)
```

## Refactoring Patterns

### Pattern 1: Extract Method
```swift
// Before: Inline complex logic
func generateReport() -> Report {
    var report = Report()
    
    // Complex calculation
    let total = items.reduce(0.0) { sum, item in
        let tax = item.price * 0.08
        let discount = item.discounted ? item.price * 0.10 : 0
        return sum + item.price + tax - discount
    }
    
    report.total = total
    return report
}

// After: Extracted calculation
func generateReport() -> Report {
    var report = Report()
    report.total = calculateTotal(for: items)
    return report
}

private func calculateTotal(for items: [Item]) -> Double {
    items.reduce(0.0) { sum, item in
        sum + calculateItemTotal(item)
    }
}

private func calculateItemTotal(_ item: Item) -> Double {
    let tax = calculateTax(for: item)
    let discount = calculateDiscount(for: item)
    return item.price + tax - discount
}

private func calculateTax(for item: Item) -> Double {
    item.price * TaxRate.standard
}

private func calculateDiscount(for item: Item) -> Double {
    item.discounted ? item.price * DiscountRate.standard : 0
}
```

### Pattern 2: Replace Conditional with Polymorphism
```swift
// Before: Type checking and branching
enum AnimalType {
    case dog, cat, bird
}

struct Animal {
    let type: AnimalType
    let name: String
}

func makeSound(for animal: Animal) -> String {
    switch animal.type {
    case .dog:
        return "Woof!"
    case .cat:
        return "Meow!"
    case .bird:
        return "Chirp!"
    }
}

// After: Protocol-based polymorphism
protocol Animal {
    var name: String { get }
    func makeSound() -> String
}

struct Dog: Animal {
    let name: String
    func makeSound() -> String { "Woof!" }
}

struct Cat: Animal {
    let name: String
    func makeSound() -> String { "Meow!" }
}

struct Bird: Animal {
    let name: String
    func makeSound() -> String { "Chirp!" }
}

// Usage
let animals: [Animal] = [Dog(name: "Rex"), Cat(name: "Whiskers")]
animals.forEach { print($0.makeSound()) }
```

### Pattern 3: Introduce Parameter Object
```swift
// Before: Too many parameters
func createUser(
    firstName: String,
    lastName: String,
    email: String,
    age: Int,
    address: String,
    city: String,
    state: String,
    zipCode: String
) {
    // Implementation
}

// After: Parameter object
struct UserInfo {
    let name: PersonName
    let email: String
    let age: Int
    let address: Address
}

struct PersonName {
    let first: String
    let last: String
}

struct Address {
    let street: String
    let city: String
    let state: String
    let zipCode: String
}

func createUser(info: UserInfo) {
    // Implementation
}
```

### Pattern 4: Replace Nested Conditionals with Guard
```swift
// Before: Nested conditions
func processOrder(_ order: Order?) -> String {
    if let order = order {
        if order.isValid {
            if order.items.count > 0 {
                if order.total > 0 {
                    return "Order processed"
                } else {
                    return "Invalid total"
                }
            } else {
                return "No items"
            }
        } else {
            return "Invalid order"
        }
    } else {
        return "Order not found"
    }
}

// After: Guard clauses
func processOrder(_ order: Order?) -> String {
    guard let order = order else {
        return "Order not found"
    }
    
    guard order.isValid else {
        return "Invalid order"
    }
    
    guard !order.items.isEmpty else {
        return "No items"
    }
    
    guard order.total > 0 else {
        return "Invalid total"
    }
    
    return "Order processed"
}
```

### Pattern 5: Extract Protocol
```swift
// Before: Concrete dependency
class UserViewController {
    let networkService: NetworkService
    
    init(networkService: NetworkService) {
        self.networkService = networkService
    }
    
    func loadUsers() async {
        let users = try? await networkService.fetchUsers()
        // ...
    }
}

// After: Protocol abstraction
protocol UserServiceProtocol {
    func fetchUsers() async throws -> [User]
}

class NetworkUserService: UserServiceProtocol {
    private let networkService: NetworkService
    
    init(networkService: NetworkService) {
        self.networkService = networkService
    }
    
    func fetchUsers() async throws -> [User] {
        try await networkService.fetchUsers()
    }
}

class UserViewController {
    let userService: UserServiceProtocol
    
    init(userService: UserServiceProtocol) {
        self.userService = userService
    }
    
    func loadUsers() async {
        let users = try? await userService.fetchUsers()
        // ...
    }
}
```

## Code Review Checklist

### Functionality
- [ ] Does the code do what it's supposed to?
- [ ] Are edge cases handled?
- [ ] Is error handling appropriate?
- [ ] Are there any obvious bugs?

### Design
- [ ] Is the code well-structured?
- [ ] Are responsibilities properly separated?
- [ ] Is it following SOLID principles?
- [ ] Is it testable?

### Readability
- [ ] Are names clear and descriptive?
- [ ] Is the code self-documenting?
- [ ] Are comments helpful and necessary?
- [ ] Is the formatting consistent?

### Performance
- [ ] Are there any obvious performance issues?
- [ ] Is memory managed properly?
- [ ] Are there any retain cycles?
- [ ] Is the main thread kept responsive?

### Security
- [ ] Is user input validated?
- [ ] Are sensitive data protected?
- [ ] Are API keys secure?
- [ ] Is network communication secure?

### Testing
- [ ] Are there appropriate tests?
- [ ] Do tests cover edge cases?
- [ ] Are tests readable and maintainable?
- [ ] Do all tests pass?

## Anti-Patterns to Avoid

### ❌ Don't Use Global Mutable State
```swift
// Bad: Global mutable state
var currentUser: User?

func login(user: User) {
    currentUser = user
}

// Good: Dependency injection
class AuthService {
    private(set) var currentUser: User?
    
    func login(user: User) {
        currentUser = user
    }
}
```

### ❌ Don't Ignore Error Handling
```swift
// Bad: Silently ignoring errors
func loadData() {
    _ = try? fetchData()
}

// Good: Handle errors appropriately
func loadData() async {
    do {
        let data = try await fetchData()
        processData(data)
    } catch {
        handleError(error)
    }
}
```

### ❌ Don't Write Untestable Code
```swift
// Bad: Untestable
class ViewController {
    func loadData() {
        let data = NetworkManager.shared.fetch() // Can't mock!
        self.data = data
    }
}

// Good: Testable
class ViewController {
    private let networkService: NetworkServiceProtocol
    
    init(networkService: NetworkServiceProtocol) {
        self.networkService = networkService
    }
    
    func loadData() async {
        let data = try? await networkService.fetch()
        self.data = data
    }
}
```

### ❌ Don't Repeat Yourself (DRY)
```swift
// Bad: Repeated validation
func saveUser(_ user: User) {
    guard user.email.contains("@") else { return }
    database.save(user)
}

func updateUser(_ user: User) {
    guard user.email.contains("@") else { return }
    database.update(user)
}

// Good: Extract common logic
func isValid(_ user: User) -> Bool {
    user.email.contains("@")
}

func saveUser(_ user: User) {
    guard isValid(user) else { return }
    database.save(user)
}

func updateUser(_ user: User) {
    guard isValid(user) else { return }
    database.update(user)
}
```

### ❌ Don't Over-Engineer
```swift
// Bad: Premature abstraction
protocol DataSourceProtocol {
    associatedtype Item
    func fetch() async throws -> [Item]
}

class GenericDataSource<T>: DataSourceProtocol {
    // Complex generic implementation
}

class CachedDataSource<T>: DataSourceProtocol {
    // Complex caching logic
}

// Good: Start simple, refactor when needed
class UserDataSource {
    func fetchUsers() async throws -> [User] {
        // Simple implementation
    }
}
```
