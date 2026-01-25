---
name: performance
description: Expert guidance for optimizing SwiftUI performance, memory management, and profiling iOS applications. Use when optimizing view rendering, managing memory, or improving application responsiveness.
---

# Performance Optimization Skill

## Overview
Use this skill to optimize SwiftUI applications for responsive performance, efficient memory usage, and smooth user interactions. This skill helps you profile applications, identify bottlenecks, and apply performance optimization patterns.

## Workflow Decision Tree

### 1) Review existing performance
- Measure actual performance with Instruments (not perception)
- Check for unnecessary view redraws and state updates
- Verify async/background operations for heavy work
- Check for memory leaks and retain cycles
- Assess collection view rendering efficiency
- Check image loading and caching strategies
- Verify network request optimization

### 2) Improve existing performance
- Use LazyVStack/LazyHStack for long lists
- Implement pagination for large datasets
- Add image caching with async loading
- Debounce/throttle frequent updates
- Move heavy computations to background
- Extract subviews to reduce view body complexity
- Cache expensive computations
- Use Equatable conformance for view updates

### 3) Implement new performance-critical feature
- Profile early with Instruments
- Use LazyVStack for collections from the start
- Implement pagination and lazy loading
- Cache network responses appropriately
- Use background queues for heavy work
- Design for responsive main thread
- Test on real devices with profiling
- Monitor memory usage throughout development

## Core Guidelines

### Minimize View Updates
- Use proper state management to avoid unnecessary redraws
- Leverage `@State`, `@Binding`, and `@ObservedObject` correctly
- Use `equatable` conformance to control view updates
- Avoid expensive computations in view body

### Optimize SwiftUI View Body
- Keep view body fast (< 16ms for 60fps)
- Extract subviews to break up complex hierarchies
- Use `@ViewBuilder` for conditional views
- Avoid creating new objects in body

### Use Lazy Loading for Collections
- Use `LazyVStack`/`LazyHStack` for long lists
- Implement pagination for large datasets
- Load images asynchronously
- Cache expensive computations

### Manage Memory Effectively
- Avoid retain cycles with `[weak self]` or `[unowned self]`
- Release large objects when no longer needed
- Use value types (structs) where appropriate
- Monitor memory usage with Instruments

### Profile Before Optimizing
- Use Instruments to identify bottlenecks
- Measure actual performance, not perceived
- Focus on user-impacting performance issues
- Test on real devices, not just simulator

### Optimize Network and I/O
- Cache network responses appropriately
- Use background queues for heavy work
- Implement request deduplication
- Use pagination and incremental loading

## Tradeoffs

### Lazy Loading vs Eager Loading
**Lazy Loading:**
- Better for long lists
- Lower initial memory
- Slight lag when scrolling
- Complex to implement correctly

**Eager Loading:**
- Simple to implement
- Predictable behavior
- High memory usage
- Slow initial load

### Value Types vs Reference Types
**Value Types (Struct):**
- Better performance (stack allocation)
- Thread-safe by default
- No reference counting
- Can lead to excessive copying

**Reference Types (Class):**
- Identity semantics
- Better for shared mutable state
- Reference counting overhead
- Risk of retain cycles

### Caching vs Real-Time Computation
**Caching:**
- Faster repeated access
- Uses more memory
- Risk of stale data
- Complexity in invalidation

**Real-Time:**
- Always fresh data
- Lower memory usage
- Higher CPU usage
- Slower access

## Output Template

```swift
import SwiftUI

// MARK: - Efficient List with Lazy Loading
struct EfficientUserListView: View {
    @StateObject private var viewModel = UserListViewModel()
    
    var body: some View {
        ScrollView {
            LazyVStack(spacing: 12) {
                ForEach(viewModel.users) { user in
                    UserRowView(user: user)
                        .onAppear {
                            // Load more when near end
                            if viewModel.shouldLoadMore(for: user) {
                                Task {
                                    await viewModel.loadMore()
                                }
                            }
                        }
                }
                
                if viewModel.isLoading {
                    ProgressView()
                        .padding()
                }
            }
            .padding()
        }
        .task {
            await viewModel.loadInitial()
        }
    }
}

// MARK: - Pagination View Model
@MainActor
class UserListViewModel: ObservableObject {
    @Published var users: [User] = []
    @Published var isLoading = false
    
    private var currentPage = 0
    private var hasMorePages = true
    private let pageSize = 20
    
    func loadInitial() async {
        guard users.isEmpty else { return }
        await loadMore()
    }
    
    func loadMore() async {
        guard !isLoading && hasMorePages else { return }
        
        isLoading = true
        defer { isLoading = false }
        
        do {
            let newUsers = try await fetchUsers(page: currentPage, size: pageSize)
            
            if newUsers.count < pageSize {
                hasMorePages = false
            }
            
            users.append(contentsOf: newUsers)
            currentPage += 1
        } catch {
            print("Failed to load users: \(error)")
        }
    }
    
    func shouldLoadMore(for user: User) -> Bool {
        guard let lastUser = users.last else { return false }
        return user.id == lastUser.id && hasMorePages
    }
    
    private func fetchUsers(page: Int, size: Int) async throws -> [User] {
        // Network call
        return []
    }
}

// MARK: - Optimized Row View with Equatable
struct UserRowView: View, Equatable {
    let user: User
    
    var body: some View {
        HStack(spacing: 12) {
            AsyncImage(url: user.avatarURL) { image in
                image
                    .resizable()
                    .aspectRatio(contentMode: .fill)
            } placeholder: {
                Color.gray
            }
            .frame(width: 50, height: 50)
            .clipShape(Circle())
            
            VStack(alignment: .leading, spacing: 4) {
                Text(user.name)
                    .font(.headline)
                Text(user.email)
                    .font(.subheadline)
                    .foregroundStyle(.secondary)
            }
            
            Spacer()
        }
        .padding()
        .background(Color(.systemBackground))
        .clipShape(RoundedRectangle(cornerRadius: 12))
    }
    
    // Control when view updates
    static func == (lhs: UserRowView, rhs: UserRowView) -> Bool {
        lhs.user.id == rhs.user.id &&
        lhs.user.name == rhs.user.name &&
        lhs.user.email == rhs.user.email
    }
}

// MARK: - Image Cache
actor ImageCache {
    private var cache: [URL: UIImage] = [:]
    private let maxCacheSize = 100
    
    func image(for url: URL) -> UIImage? {
        cache[url]
    }
    
    func store(_ image: UIImage, for url: URL) {
        if cache.count >= maxCacheSize {
            // Remove oldest entry (simplified)
            cache.removeValue(forKey: cache.keys.first!)
        }
        cache[url] = image
    }
    
    func clear() {
        cache.removeAll()
    }
}

// MARK: - Cached Async Image View
struct CachedAsyncImage: View {
    let url: URL
    @State private var image: UIImage?
    @State private var isLoading = true
    
    static let cache = ImageCache()
    
    var body: some View {
        Group {
            if let image = image {
                Image(uiImage: image)
                    .resizable()
            } else if isLoading {
                ProgressView()
            } else {
                Image(systemName: "photo")
                    .foregroundStyle(.secondary)
            }
        }
        .task {
            await loadImage()
        }
    }
    
    private func loadImage() async {
        // Check cache first
        if let cached = await Self.cache.image(for: url) {
            image = cached
            isLoading = false
            return
        }
        
        // Download image
        do {
            let (data, _) = try await URLSession.shared.data(from: url)
            if let downloadedImage = UIImage(data: data) {
                await Self.cache.store(downloadedImage, for: url)
                image = downloadedImage
            }
        } catch {
            print("Failed to load image: \(error)")
        }
        
        isLoading = false
    }
}

// MARK: - Computed Property Optimization
struct UserProfileView: View {
    let user: User
    
    // Computed once, not on every render
    private var formattedDate: String {
        let formatter = DateFormatter()
        formatter.dateStyle = .medium
        return formatter.string(from: user.joinDate)
    }
    
    var body: some View {
        VStack {
            Text(user.name)
            Text("Joined: \(formattedDate)")
        }
    }
}

// MARK: - Background Processing
class DataProcessor {
    func processLargeDataset(_ data: [Item]) async -> [Result] {
        await withTaskGroup(of: Result.self) { group in
            for item in data {
                group.addTask {
                    // Process on background queue
                    await self.process(item)
                }
            }
            
            var results: [Result] = []
            for await result in group {
                results.append(result)
            }
            return results
        }
    }
    
    private func process(_ item: Item) async -> Result {
        // Heavy computation
        return Result()
    }
}

// MARK: - Debounced Search
struct SearchView: View {
    @State private var searchText = ""
    @State private var results: [Item] = []
    @State private var searchTask: Task<Void, Never>?
    
    var body: some View {
        VStack {
            TextField("Search", text: $searchText)
                .onChange(of: searchText) { newValue in
                    performSearch(query: newValue)
                }
            
            List(results) { item in
                Text(item.name)
            }
        }
    }
    
    private func performSearch(query: String) {
        // Cancel previous search
        searchTask?.cancel()
        
        // Debounce: wait 300ms before searching
        searchTask = Task {
            try? await Task.sleep(nanoseconds: 300_000_000)
            
            guard !Task.isCancelled else { return }
            
            // Perform search
            let newResults = await search(query: query)
            
            guard !Task.isCancelled else { return }
            
            results = newResults
        }
    }
    
    private func search(query: String) async -> [Item] {
        // Actual search implementation
        return []
    }
}

// MARK: - Request Deduplication
actor RequestCache<Key: Hashable, Value> {
    private var cache: [Key: Value] = [:]
    private var inFlightRequests: [Key: Task<Value, Error>] = [:]
    
    func value(
        for key: Key,
        fetch: @escaping () async throws -> Value
    ) async throws -> Value {
        // Return cached value if available
        if let cached = cache[key] {
            return cached
        }
        
        // Return in-flight request if exists
        if let existing = inFlightRequests[key] {
            return try await existing.value
        }
        
        // Create new request
        let task = Task<Value, Error> {
            try await fetch()
        }
        inFlightRequests[key] = task
        
        do {
            let value = try await task.value
            cache[key] = value
            inFlightRequests[key] = nil
            return value
        } catch {
            inFlightRequests[key] = nil
            throw error
        }
    }
}

// Usage
class UserService {
    private let requestCache = RequestCache<String, User>()
    
    func fetchUser(id: String) async throws -> User {
        try await requestCache.value(for: id) {
            try await self.performFetch(id: id)
        }
    }
    
    private func performFetch(id: String) async throws -> User {
        // Actual network call
        fatalError()
    }
}
```

## Performance Patterns

### Pattern 1: Memoization for Expensive Computations
```swift
class ViewModel: ObservableObject {
    @Published var input: String = ""
    
    private var cachedResult: (input: String, result: String)?
    
    var expensiveComputation: String {
        if let cached = cachedResult, cached.input == input {
            return cached.result
        }
        
        let result = performExpensiveComputation(input)
        cachedResult = (input, result)
        return result
    }
    
    private func performExpensiveComputation(_ input: String) -> String {
        // Heavy computation
        return input.uppercased()
    }
}
```

### Pattern 2: Prefetching Data
```swift
class ContentViewModel: ObservableObject {
    @Published var items: [Item] = []
    
    func loadContent() async {
        // Load visible items first
        let visibleItems = try? await fetchItems(page: 0)
        items = visibleItems ?? []
        
        // Prefetch next page in background
        Task.detached(priority: .low) {
            _ = try? await self.fetchItems(page: 1)
        }
    }
    
    private func fetchItems(page: Int) async throws -> [Item] {
        // Network call
        return []
    }
}
```

### Pattern 3: Virtualized List with GeometryReader
```swift
struct VirtualizedListView: View {
    let items: [Item]
    let itemHeight: CGFloat = 60
    @State private var visibleRange: Range<Int> = 0..<20
    
    var body: some View {
        GeometryReader { geometry in
            ScrollView {
                LazyVStack(spacing: 0) {
                    ForEach(items.indices, id: \.self) { index in
                        if visibleRange.contains(index) {
                            ItemView(item: items[index])
                                .frame(height: itemHeight)
                        } else {
                            Color.clear
                                .frame(height: itemHeight)
                        }
                    }
                }
                .background(
                    GeometryReader { scrollGeometry in
                        Color.clear.preference(
                            key: ScrollOffsetKey.self,
                            value: scrollGeometry.frame(in: .named("scroll")).minY
                        )
                    }
                )
            }
            .coordinateSpace(name: "scroll")
            .onPreferenceChange(ScrollOffsetKey.self) { offset in
                updateVisibleRange(offset: offset, height: geometry.size.height)
            }
        }
    }
    
    private func updateVisibleRange(offset: CGFloat, height: CGFloat) {
        let start = max(0, Int(-offset / itemHeight) - 5)
        let end = min(items.count, start + Int(height / itemHeight) + 10)
        visibleRange = start..<end
    }
}

struct ScrollOffsetKey: PreferenceKey {
    static var defaultValue: CGFloat = 0
    static func reduce(value: inout CGFloat, nextValue: () -> CGFloat) {
        value = nextValue()
    }
}
```

### Pattern 4: Throttling Updates
```swift
@MainActor
class ThrottledViewModel: ObservableObject {
    @Published var displayValue: Double = 0
    
    private var actualValue: Double = 0
    private var updateTask: Task<Void, Never>?
    
    func updateValue(_ value: Double) {
        actualValue = value
        
        // Only update display every 100ms
        if updateTask == nil {
            updateTask = Task {
                defer { updateTask = nil }
                
                try? await Task.sleep(nanoseconds: 100_000_000)
                displayValue = actualValue
            }
        }
    }
}
```

### Pattern 5: Memory-Efficient Data Models
```swift
// Inefficient: Storing computed values
struct InefficientUser {
    let firstName: String
    let lastName: String
    let fullName: String  // Redundant!
}

// Efficient: Compute on demand
struct EfficientUser {
    let firstName: String
    let lastName: String
    
    var fullName: String {
        "\(firstName) \(lastName)"
    }
}

// Even better: Use lazy for expensive computations
class User {
    let data: Data
    
    lazy var parsedData: ParsedData = {
        // Expensive parsing
        parseData(data)
    }()
    
    private func parseData(_ data: Data) -> ParsedData {
        // Heavy computation
        fatalError()
    }
}
```

## Profiling with Instruments

```swift
// Use signposts for custom profiling
import os.signpost

let log = OSLog(subsystem: "com.example.app", category: "Performance")

func performExpensiveOperation() {
    os_signpost(.begin, log: log, name: "Expensive Operation")
    
    // Do work
    
    os_signpost(.end, log: log, name: "Expensive Operation")
}

// Measure specific code blocks
func measurePerformance() {
    let start = CFAbsoluteTimeGetCurrent()
    
    // Code to measure
    
    let elapsed = CFAbsoluteTimeGetCurrent() - start
    print("Operation took \(elapsed) seconds")
}
```

## Anti-Patterns to Avoid

### ❌ Don't Create Objects in View Body
```swift
// Bad: Creates new formatter every render
var body: some View {
    let formatter = DateFormatter()
    formatter.dateStyle = .medium
    return Text(formatter.string(from: date))
}

// Good: Create formatter once
private let formatter: DateFormatter = {
    let f = DateFormatter()
    f.dateStyle = .medium
    return f
}()

var body: some View {
    Text(formatter.string(from: date))
}
```

### ❌ Don't Use `@State` for Large Objects
```swift
// Bad: Large object in @State
struct MyView: View {
    @State private var largeDataset: [VeryLargeObject] = []
    
    var body: some View {
        List(largeDataset) { item in
            // ...
        }
    }
}

// Good: Use @StateObject for complex data
@MainActor
class DataViewModel: ObservableObject {
    @Published var dataset: [VeryLargeObject] = []
}

struct MyView: View {
    @StateObject private var viewModel = DataViewModel()
    
    var body: some View {
        List(viewModel.dataset) { item in
            // ...
        }
    }
}
```

### ❌ Don't Ignore Retain Cycles
```swift
// Bad: Retain cycle
class ViewModel: ObservableObject {
    var onComplete: (() -> Void)?
    
    func setup() {
        onComplete = {
            self.cleanup()  // Retain cycle!
        }
    }
}

// Good: Weak capture
class ViewModel: ObservableObject {
    var onComplete: (() -> Void)?
    
    func setup() {
        onComplete = { [weak self] in
            self?.cleanup()
        }
    }
}
```

### ❌ Don't Block the Main Thread
```swift
// Bad: Heavy work on main thread
func loadData() {
    let data = expensiveComputation()  // Blocks UI!
    self.data = data
}

// Good: Use background queue
func loadData() async {
    let data = await Task.detached {
        expensiveComputation()
    }.value
    
    await MainActor.run {
        self.data = data
    }
}
```
