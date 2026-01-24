# Testing & Dependency Injection Skill

Expert guidance for unit testing, dependency injection, and testing async code in Swift and SwiftUI.

## Rules

### 1. Design for Testability
- Use protocol abstractions for dependencies
- Inject dependencies through initializers
- Avoid singletons and global state
- Separate business logic from UI
- Keep functions small and focused

### 2. Follow the Testing Pyramid
- Many unit tests (fast, isolated)
- Some integration tests (moderate speed)
- Few UI tests (slow, brittle)
- Focus on business logic testing

### 3. Test Behavior, Not Implementation
- Test public interfaces, not private details
- Focus on inputs and outputs
- Avoid testing internal state
- Test what the code does, not how it does it

### 4. Use Proper Test Doubles
- **Stub**: Returns predefined data (for queries)
- **Mock**: Verifies interactions (for commands)
- **Fake**: Working implementation (in-memory DB)
- **Spy**: Records calls for verification

### 5. Test Async Code Properly
- Use `async`/`await` in tests
- Test cancellation behavior
- Use `expectation` for callback-based code
- Test timing and race conditions

### 6. Structure Tests with Arrange-Act-Assert
- **Arrange**: Set up test data and dependencies
- **Act**: Execute the code under test
- **Assert**: Verify expected outcomes
- Keep tests readable and maintainable

## Tradeoffs

### Constructor Injection vs Property Injection
**Constructor Injection:**
- Dependencies are explicit
- Cannot create object without dependencies
- Better for required dependencies
- Immutable after creation

**Property Injection:**
- Optional dependencies
- Can set defaults
- More flexible
- Risk of forgetting to inject

### Protocol-Based DI vs Closure-Based DI
**Protocol-Based:**
- More structured
- Better for complex dependencies
- Easier to mock entire services
- More boilerplate

**Closure-Based:**
- Less boilerplate
- Good for simple functions
- More flexible
- Can be harder to test

### Manual DI vs DI Frameworks
**Manual DI:**
- No external dependencies
- Full control
- Simple to understand
- More boilerplate for large apps

**DI Frameworks (Swinject, etc.):**
- Less boilerplate
- Automatic resolution
- Learning curve
- Additional dependency

## Output Template

```swift
import XCTest
@testable import MyApp

// MARK: - Protocol Abstraction
protocol UserServiceProtocol {
    func fetchUser(id: String) async throws -> User
    func updateUser(_ user: User) async throws
    func deleteUser(id: String) async throws
}

// MARK: - Production Service
class UserService: UserServiceProtocol {
    private let networkClient: NetworkClient
    
    init(networkClient: NetworkClient) {
        self.networkClient = networkClient
    }
    
    func fetchUser(id: String) async throws -> User {
        try await networkClient.get("/users/\(id)")
    }
    
    func updateUser(_ user: User) async throws {
        try await networkClient.put("/users/\(user.id)", body: user)
    }
    
    func deleteUser(id: String) async throws {
        try await networkClient.delete("/users/\(id)")
    }
}

// MARK: - Mock Service for Testing
class MockUserService: UserServiceProtocol {
    var fetchUserCallCount = 0
    var updateUserCallCount = 0
    var deleteUserCallCount = 0
    
    var fetchUserResult: Result<User, Error> = .failure(TestError.notImplemented)
    var updateUserResult: Result<Void, Error> = .success(())
    var deleteUserResult: Result<Void, Error> = .success(())
    
    var lastUpdatedUser: User?
    var lastDeletedUserId: String?
    
    func fetchUser(id: String) async throws -> User {
        fetchUserCallCount += 1
        return try fetchUserResult.get()
    }
    
    func updateUser(_ user: User) async throws {
        updateUserCallCount += 1
        lastUpdatedUser = user
        try updateUserResult.get()
    }
    
    func deleteUser(id: String) async throws {
        deleteUserCallCount += 1
        lastDeletedUserId = id
        try deleteUserResult.get()
    }
}

enum TestError: Error {
    case notImplemented
    case testFailure
}

// MARK: - View Model Under Test
@MainActor
class UserViewModel: ObservableObject {
    @Published var user: User?
    @Published var isLoading = false
    @Published var error: Error?
    
    private let userService: UserServiceProtocol
    
    init(userService: UserServiceProtocol) {
        self.userService = userService
    }
    
    func loadUser(id: String) async {
        isLoading = true
        error = nil
        
        do {
            user = try await userService.fetchUser(id: id)
        } catch {
            self.error = error
        }
        
        isLoading = false
    }
    
    func updateUser(_ user: User) async -> Bool {
        do {
            try await userService.updateUser(user)
            self.user = user
            return true
        } catch {
            self.error = error
            return false
        }
    }
}

// MARK: - Unit Tests
@MainActor
class UserViewModelTests: XCTestCase {
    var sut: UserViewModel!
    var mockService: MockUserService!
    
    override func setUp() {
        super.setUp()
        mockService = MockUserService()
        sut = UserViewModel(userService: mockService)
    }
    
    override func tearDown() {
        sut = nil
        mockService = nil
        super.tearDown()
    }
    
    // MARK: - Test Loading User Success
    func testLoadUser_Success_SetsUser() async {
        // Arrange
        let expectedUser = User(id: "123", name: "John Doe", email: "john@example.com")
        mockService.fetchUserResult = .success(expectedUser)
        
        // Act
        await sut.loadUser(id: "123")
        
        // Assert
        XCTAssertEqual(sut.user?.id, expectedUser.id)
        XCTAssertEqual(sut.user?.name, expectedUser.name)
        XCTAssertEqual(mockService.fetchUserCallCount, 1)
        XCTAssertFalse(sut.isLoading)
        XCTAssertNil(sut.error)
    }
    
    // MARK: - Test Loading User Failure
    func testLoadUser_Failure_SetsError() async {
        // Arrange
        mockService.fetchUserResult = .failure(TestError.testFailure)
        
        // Act
        await sut.loadUser(id: "123")
        
        // Assert
        XCTAssertNil(sut.user)
        XCTAssertNotNil(sut.error)
        XCTAssertFalse(sut.isLoading)
    }
    
    // MARK: - Test Update User
    func testUpdateUser_Success_UpdatesUser() async {
        // Arrange
        let user = User(id: "123", name: "Jane Doe", email: "jane@example.com")
        mockService.updateUserResult = .success(())
        
        // Act
        let result = await sut.updateUser(user)
        
        // Assert
        XCTAssertTrue(result)
        XCTAssertEqual(sut.user?.id, user.id)
        XCTAssertEqual(mockService.lastUpdatedUser?.id, user.id)
        XCTAssertEqual(mockService.updateUserCallCount, 1)
    }
    
    // MARK: - Test Update User Failure
    func testUpdateUser_Failure_ReturnsFalse() async {
        // Arrange
        let user = User(id: "123", name: "Jane Doe", email: "jane@example.com")
        mockService.updateUserResult = .failure(TestError.testFailure)
        
        // Act
        let result = await sut.updateUser(user)
        
        // Assert
        XCTAssertFalse(result)
        XCTAssertNotNil(sut.error)
    }
}

// MARK: - Testing with XCTestExpectation (callback-based)
class CallbackBasedTests: XCTestCase {
    func testAsyncOperation_WithExpectation() {
        // Arrange
        let expectation = expectation(description: "Callback called")
        var result: String?
        
        // Act
        performAsyncOperation { value in
            result = value
            expectation.fulfill()
        }
        
        // Assert
        wait(for: [expectation], timeout: 1.0)
        XCTAssertEqual(result, "expected")
    }
    
    private func performAsyncOperation(completion: @escaping (String) -> Void) {
        DispatchQueue.main.asyncAfter(deadline: .now() + 0.1) {
            completion("expected")
        }
    }
}
```

## Testing Patterns

### Pattern 1: Test Fixtures / Builders
```swift
extension User {
    static func fixture(
        id: String = "123",
        name: String = "Test User",
        email: String = "test@example.com"
    ) -> User {
        User(id: id, name: name, email: email)
    }
}

// Usage in tests
func testSomething() {
    let user = User.fixture(name: "Custom Name")
    // Test with user
}
```

### Pattern 2: Fake In-Memory Repository
```swift
class InMemoryUserRepository: UserRepositoryProtocol {
    private var users: [String: User] = [:]
    
    func save(_ user: User) async throws {
        users[user.id] = user
    }
    
    func fetch(id: String) async throws -> User {
        guard let user = users[id] else {
            throw RepositoryError.notFound
        }
        return user
    }
    
    func delete(id: String) async throws {
        users.removeValue(forKey: id)
    }
    
    func clear() {
        users.removeAll()
    }
}

// Usage
class UserServiceTests: XCTestCase {
    var repository: InMemoryUserRepository!
    var sut: UserService!
    
    override func setUp() {
        super.setUp()
        repository = InMemoryUserRepository()
        sut = UserService(repository: repository)
    }
}
```

### Pattern 3: Testing Actor Isolation
```swift
actor Counter {
    private var value = 0
    
    func increment() {
        value += 1
    }
    
    func getValue() -> Int {
        value
    }
}

class CounterTests: XCTestCase {
    func testIncrement_IncreasesValue() async {
        // Arrange
        let counter = Counter()
        
        // Act
        await counter.increment()
        await counter.increment()
        
        // Assert
        let value = await counter.getValue()
        XCTAssertEqual(value, 2)
    }
    
    func testConcurrentIncrements_AllComplete() async {
        // Arrange
        let counter = Counter()
        
        // Act - Run 100 concurrent increments
        await withTaskGroup(of: Void.self) { group in
            for _ in 0..<100 {
                group.addTask {
                    await counter.increment()
                }
            }
        }
        
        // Assert
        let value = await counter.getValue()
        XCTAssertEqual(value, 100)
    }
}
```

### Pattern 4: Testing Task Cancellation
```swift
class CancellationTests: XCTestCase {
    func testLongRunningOperation_IsCancellable() async throws {
        // Arrange
        let task = Task {
            try await longRunningOperation()
        }
        
        // Act - Cancel after short delay
        try await Task.sleep(nanoseconds: 100_000_000) // 0.1s
        task.cancel()
        
        // Assert
        do {
            _ = try await task.value
            XCTFail("Should have thrown cancellation error")
        } catch is CancellationError {
            // Expected
        } catch {
            XCTFail("Unexpected error: \(error)")
        }
    }
    
    private func longRunningOperation() async throws -> String {
        for i in 0..<100 {
            try Task.checkCancellation()
            try await Task.sleep(nanoseconds: 100_000_000)
        }
        return "completed"
    }
}
```

### Pattern 5: Spy for Recording Calls
```swift
class NavigationCoordinatorSpy: NavigationCoordinatorProtocol {
    private(set) var navigateCallCount = 0
    private(set) var navigatedRoutes: [Route] = []
    
    func navigate(to route: Route) {
        navigateCallCount += 1
        navigatedRoutes.append(route)
    }
}

class ViewModelTests: XCTestCase {
    func testButtonTap_NavigatesToDetail() {
        // Arrange
        let spy = NavigationCoordinatorSpy()
        let sut = MyViewModel(coordinator: spy)
        
        // Act
        sut.onButtonTap()
        
        // Assert
        XCTAssertEqual(spy.navigateCallCount, 1)
        XCTAssertEqual(spy.navigatedRoutes.first, .detail)
    }
}
```

### Pattern 6: Testing SwiftUI Views with ViewInspector
```swift
import ViewInspector
import XCTest

class UserViewTests: XCTestCase {
    func testView_ShowsUserName() throws {
        // Arrange
        let user = User.fixture(name: "John Doe")
        let sut = UserView(user: user)
        
        // Act
        let text = try sut.inspect().find(text: "John Doe")
        
        // Assert
        XCTAssertNotNil(text)
    }
}
```

### Pattern 7: Dependency Container
```swift
protocol DependencyContainer {
    var userService: UserServiceProtocol { get }
    var authService: AuthServiceProtocol { get }
    var networkClient: NetworkClient { get }
}

class ProductionDependencies: DependencyContainer {
    lazy var networkClient = NetworkClient()
    lazy var userService: UserServiceProtocol = UserService(networkClient: networkClient)
    lazy var authService: AuthServiceProtocol = AuthService(networkClient: networkClient)
}

class TestDependencies: DependencyContainer {
    var userService: UserServiceProtocol = MockUserService()
    var authService: AuthServiceProtocol = MockAuthService()
    var networkClient: NetworkClient = MockNetworkClient()
}

// Usage
class MyViewModel {
    private let dependencies: DependencyContainer
    
    init(dependencies: DependencyContainer) {
        self.dependencies = dependencies
    }
    
    func loadData() async {
        let user = try? await dependencies.userService.fetchUser(id: "123")
    }
}

// In tests
func testLoadData() {
    let deps = TestDependencies()
    let sut = MyViewModel(dependencies: deps)
    // Test...
}
```

## Anti-Patterns to Avoid

### ❌ Don't Use Real Network in Unit Tests
```swift
// Bad: Tests depend on network
class UserViewModelTests: XCTestCase {
    func testLoadUser() async {
        let service = UserService() // Real network calls!
        let sut = UserViewModel(userService: service)
        await sut.loadUser(id: "123")
    }
}

// Good: Use mock
class UserViewModelTests: XCTestCase {
    func testLoadUser() async {
        let mockService = MockUserService()
        let sut = UserViewModel(userService: mockService)
        await sut.loadUser(id: "123")
    }
}
```

### ❌ Don't Test Private Methods
```swift
// Bad: Testing implementation details
class MyClassTests: XCTestCase {
    func testPrivateHelperMethod() {
        // Accessing private methods via reflection or tricks
    }
}

// Good: Test public behavior
class MyClassTests: XCTestCase {
    func testPublicMethod_UsesHelperCorrectly() {
        // Test through public interface
    }
}
```

### ❌ Don't Use Singletons
```swift
// Bad: Hard to test
class NetworkManager {
    static let shared = NetworkManager()
    private init() {}
}

class MyService {
    func fetch() async throws -> Data {
        try await NetworkManager.shared.request() // Can't inject!
    }
}

// Good: Dependency injection
protocol NetworkManagerProtocol {
    func request() async throws -> Data
}

class MyService {
    private let networkManager: NetworkManagerProtocol
    
    init(networkManager: NetworkManagerProtocol) {
        self.networkManager = networkManager
    }
}
```

### ❌ Don't Have Multiple Assertions for Different Concerns
```swift
// Bad: Testing too much in one test
func testEverything() async {
    await sut.loadUser()
    XCTAssertNotNil(sut.user)
    
    await sut.deleteUser()
    XCTAssertNil(sut.user)
    
    await sut.loadPosts()
    XCTAssertFalse(sut.posts.isEmpty)
}

// Good: Separate tests for each concern
func testLoadUser_SetsUser() async {
    await sut.loadUser()
    XCTAssertNotNil(sut.user)
}

func testDeleteUser_ClearsUser() async {
    await sut.deleteUser()
    XCTAssertNil(sut.user)
}
```
