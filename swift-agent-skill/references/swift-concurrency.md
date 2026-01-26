
# Swift Concurrency Skill

## Overview
Use this skill to implement, review, or improve Swift concurrency code following best practices for async/await, Tasks, Actors, and structured concurrency. This skill helps you write safe, efficient concurrent code and avoid common pitfalls.

## Workflow Decision Tree

### 1) Review existing concurrency code
- Verify async/await is used instead of completion handlers
- Check Task lifecycle management (cancellation, structured concurrency)
- Verify actors are used for mutable shared state
- Check MainActor is used for UI-related code
- Verify no blocking operations on MainActor
- Check for proper cancellation handling

### 2) Improve existing concurrency code
- Replace completion handlers with async/await using continuations
- Replace dispatch queues and locks with actors
- Add proper cancellation checks in long-running operations
- Mark UI-related classes with @MainActor
- Use structured concurrency (async let, TaskGroup) instead of unstructured tasks
- Remove unnecessary weak self captures in tasks

### 3) Implement new concurrent feature
- Use async/await for sequential async operations
- Use async let for fixed parallel operations
- Use TaskGroup for dynamic parallel operations
- Use actors to protect mutable shared state
- Mark UI-related code with @MainActor
- Use .task modifier in SwiftUI for automatic cancellation
- Handle cancellation in long-running operations

## Core Guidelines

### Prefer Async/Await Over Closures
- Use `async/await` instead of completion handlers for better readability and error handling
- Convert existing closure-based APIs using continuations when needed
- Always propagate errors with `throws` rather than Result types in async functions

### Use Structured Concurrency
- Prefer `async let` for parallel operations with known dependencies
- Use `TaskGroup` for dynamic parallel operations
- Ensure all child tasks complete before parent returns (automatic cancellation)
- Avoid creating detached tasks unless truly independent

### Handle Cancellation Properly
- Check `Task.isCancelled` in long-running operations
- Use `Task.checkCancellation()` to throw if cancelled
- Clean up resources in cancellation paths
- Use `.task` view modifier in SwiftUI for automatic cancellation

### Use Actors for Mutable State
- Protect mutable state with `actor` instead of locks/queues
- Use `@MainActor` for UI-related properties and methods
- Avoid `nonisolated` unless necessary for synchronous access
- Never mix actors with DispatchQueue

### Avoid Blocking the Main Thread
- Never use `.wait()` or blocking synchronization on MainActor
- Use `Task { @MainActor in }` to hop to main thread
- Mark view models and UI controllers with `@MainActor`

### Task Lifecycle Management
- Store tasks in properties to cancel them later
- Use `.task` view modifier in SwiftUI for automatic cancellation
- Avoid creating detached tasks unless truly independent
- Don't capture self weakly in structured concurrency (tasks are automatically cancelled)

## Tradeoffs

### When to Use Async/Await
**Use when:**
- Sequential operations with dependencies
- Error handling is important
- Integrating with async APIs

**Avoid when:**
- Simple fire-and-forget operations
- Interfacing with pure synchronous code

### When to Use Actors
**Use when:**
- Protecting mutable state across concurrency contexts
- Building thread-safe caches or managers
- Need automatic data race protection

**Avoid when:**
- State is immutable
- Performance-critical hot paths (consider Sendable structs)
- Simple, short-lived operations

### When to Use Task Groups
**Use when:**
- Number of parallel operations is dynamic
- Need to process results as they complete
- Implementing parallel map/reduce

**Avoid when:**
- Fixed number of parallel operations (use async let)
- Sequential operations

## Output Template

```swift
// MARK: - Async/Await Function
func fetchUserData() async throws -> User {
    let user = try await networkService.fetchUser()
    let profile = try await networkService.fetchProfile(for: user.id)
    return user.merged(with: profile)
}

// MARK: - Parallel Operations with async let
func fetchDashboard() async throws -> Dashboard {
    async let user = fetchUser()
    async let posts = fetchPosts()
    async let notifications = fetchNotifications()
    
    return try await Dashboard(
        user: user,
        posts: posts,
        notifications: notifications
    )
}

// MARK: - Task Group for Dynamic Parallelism
func fetchMultipleUsers(ids: [String]) async throws -> [User] {
    try await withThrowingTaskGroup(of: User.self) { group in
        for id in ids {
            group.addTask {
                try await fetchUser(id: id)
            }
        }
        
        var users: [User] = []
        for try await user in group {
            users.append(user)
        }
        return users
    }
}

// MARK: - Actor for Thread-Safe State
actor ImageCache {
    private var cache: [URL: UIImage] = [:]
    
    func image(for url: URL) -> UIImage? {
        cache[url]
    }
    
    func store(_ image: UIImage, for url: URL) {
        cache[url] = image
    }
}

// MARK: - MainActor View Model
@MainActor
class UserViewModel: ObservableObject {
    @Published var user: User?
    @Published var isLoading = false
    
    private let service: UserService
    
    init(service: UserService) {
        self.service = service
    }
    
    func loadUser() async {
        isLoading = true
        defer { isLoading = false }
        
        do {
            user = try await service.fetchUser()
        } catch {
            print("Failed to load user: \(error)")
        }
    }
}

// MARK: - Cancellation Handling
func processLargeDataset(_ items: [Item]) async throws -> [Result] {
    var results: [Result] = []
    
    for item in items {
        // Check for cancellation periodically
        try Task.checkCancellation()
        
        let result = await process(item)
        results.append(result)
    }
    
    return results
}

// MARK: - SwiftUI Integration
struct UserView: View {
    @StateObject private var viewModel = UserViewModel(service: .live)
    
    var body: some View {
        Group {
            if let user = viewModel.user {
                UserDetailView(user: user)
            } else {
                ProgressView()
            }
        }
        .task {
            // Automatically cancelled when view disappears
            await viewModel.loadUser()
        }
    }
}

// MARK: - Converting Closure to Async
func fetchData() async throws -> Data {
    try await withCheckedThrowingContinuation { continuation in
        legacyFetchData { result in
            switch result {
            case .success(let data):
                continuation.resume(returning: data)
            case .failure(let error):
                continuation.resume(throwing: error)
            }
        }
    }
}
```

## Common Patterns

### Pattern 1: Retry with Exponential Backoff
```swift
func fetchWithRetry<T>(
    maxAttempts: Int = 3,
    operation: @escaping () async throws -> T
) async throws -> T {
    var lastError: Error?
    
    for attempt in 0..<maxAttempts {
        do {
            return try await operation()
        } catch {
            lastError = error
            if attempt < maxAttempts - 1 {
                let delay = pow(2.0, Double(attempt))
                try await Task.sleep(nanoseconds: UInt64(delay * 1_000_000_000))
            }
        }
    }
    
    throw lastError ?? URLError(.unknown)
}
```

### Pattern 2: Timeout
```swift
func withTimeout<T>(
    seconds: TimeInterval,
    operation: @escaping () async throws -> T
) async throws -> T {
    try await withThrowingTaskGroup(of: T.self) { group in
        group.addTask {
            try await operation()
        }
        
        group.addTask {
            try await Task.sleep(nanoseconds: UInt64(seconds * 1_000_000_000))
            throw TimeoutError()
        }
        
        let result = try await group.next()!
        group.cancelAll()
        return result
    }
}
```

### Pattern 3: Stream Processing
```swift
func processStream() async throws {
    let stream = AsyncStream<Int> { continuation in
        Task {
            for i in 0..<100 {
                continuation.yield(i)
                try? await Task.sleep(nanoseconds: 100_000_000)
            }
            continuation.finish()
        }
    }
    
    for await value in stream {
        print("Processing: \(value)")
    }
}
```

## Anti-Patterns to Avoid

### ❌ Don't Create Unstructured Tasks Unnecessarily
```swift
// Bad: Unstructured task without cleanup
Task {
    await doSomething()
}

// Good: Use structured concurrency
await doSomething()
```

### ❌ Don't Mix Actors with DispatchQueue
```swift
// Bad: Mixing concurrency models
actor MyActor {
    func doWork() {
        DispatchQueue.main.async {  // Don't do this!
            updateUI()
        }
    }
}

// Good: Use MainActor
actor MyActor {
    func doWork() async {
        await MainActor.run {
            updateUI()
        }
    }
}
```

### ❌ Don't Capture Self Weakly in Tasks
```swift
// Bad: Weak self not needed in structured concurrency
.task { [weak self] in
    await self?.load()
}

// Good: Tasks are automatically cancelled
.task {
    await load()
}
```
