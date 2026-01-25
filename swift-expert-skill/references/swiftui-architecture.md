
# SwiftUI Architecture & State Management Skill

## Overview
Use this skill to build, review, or improve SwiftUI architecture following best practices for state management, data flow, view composition, and testability. This skill helps you choose the right property wrappers, structure apps effectively, and maintain clean separation of concerns.

## Workflow Decision Tree

### 1) Review existing SwiftUI architecture
- Verify correct property wrapper usage (@State, @Binding, @StateObject, @ObservedObject)
- Check unidirectional data flow (data down, events up)
- Verify views are simple and declarative
- Check side effects are properly managed (.task, .onChange)
- Verify dependency injection is used for testability
- Check for massive view models or god objects

### 2) Improve existing SwiftUI architecture
- Replace incorrect property wrapper usage
- Extract complex view logic into view models
- Split large views into smaller, reusable components
- Move side effects out of view bodies
- Add protocol-based dependency injection for testing
- Consider migrating to @Observable for iOS 17+

### 3) Implement new SwiftUI feature
- Choose appropriate property wrappers based on ownership
- Design unidirectional data flow
- Keep views declarative (no logic in body)
- Use .task for async work, .onChange for reactions
- Inject dependencies through initializers
- Create small, focused view models by feature

## Core Guidelines

### Choose the Right Property Wrapper
- `@State`: For simple, view-local state (primitives, simple structs)
- `@Binding`: To pass write access to state owned by a parent
- `@StateObject`: To create and own a reference type (ObservableObject)
- `@ObservedObject`: To observe a reference type owned elsewhere
- `@EnvironmentObject`: For app-wide shared state
- `@Environment`: For system values or custom environment keys
- For iOS 17+: Use `@Observable` instead of `ObservableObject`

### Follow Unidirectional Data Flow
- Data flows down through view hierarchy
- Events/actions flow up through callbacks or bindings
- Never manipulate parent state directly from children
- Use explicit bindings or callbacks for child-to-parent communication

### Keep Views Simple
- Views should be declarative, not imperative
- Extract complex logic into view models or separate functions
- Split large views into smaller, reusable components
- Views should render state, not manage it
- Never perform side effects in view body

### Manage Side Effects Properly
- Use `.task` modifier for async work tied to view lifecycle
- Use `.onChange` for reacting to state changes
- Use `.onAppear` sparingly (prefer `.task`)
- Never perform side effects in view body

### Structure for Testability
- Inject dependencies through initializers
- Use protocols for service abstractions
- Keep business logic separate from views
- Make view models testable with protocol-based dependencies

### Use ViewModifiers for Reusable Behavior
- Extract common styling into custom ViewModifiers
- Create reusable view configurations
- Avoid duplicating appearance code

## Tradeoffs

### @State vs @StateObject
**@State:**
- Simple value types
- View-local state
- Automatically managed lifecycle
- No reference semantics needed

**@StateObject:**
- Complex business logic
- Need reference semantics
- Sharing state with child views
- Lifecycle tied to view

### ObservableObject vs Observation Framework (@Observable)
**ObservableObject (iOS 13+):**
- Widely compatible
- Requires `@Published` for each property
- More boilerplate

**@Observable (iOS 17+):**
- Less boilerplate
- Better performance
- Automatic tracking
- Requires newer OS versions

### Feature-Based vs Layer-Based Architecture
**Feature-Based:**
- Better for large apps
- Clear feature boundaries
- Easier to test features in isolation

**Layer-Based:**
- Better for small apps
- Traditional separation (Views, ViewModels, Models)
- Simpler to understand initially

## Output Template

```swift
// MARK: - Model (Immutable Data)
struct User: Identifiable, Codable {
    let id: UUID
    var name: String
    var email: String
}

// MARK: - Service Protocol (for DI)
protocol UserServiceProtocol {
    func fetchUsers() async throws -> [User]
    func updateUser(_ user: User) async throws
}

// MARK: - View Model (Business Logic)
@MainActor
class UserListViewModel: ObservableObject {
    // Published state
    @Published var users: [User] = []
    @Published var isLoading = false
    @Published var errorMessage: String?
    
    // Dependencies
    private let userService: UserServiceProtocol
    
    // Dependency injection
    init(userService: UserServiceProtocol) {
        self.userService = userService
    }
    
    // Actions
    func loadUsers() async {
        isLoading = true
        errorMessage = nil
        
        do {
            users = try await userService.fetchUsers()
        } catch {
            errorMessage = "Failed to load users: \(error.localizedDescription)"
        }
        
        isLoading = false
    }
    
    func updateUser(_ user: User) async {
        do {
            try await userService.updateUser(user)
            if let index = users.firstIndex(where: { $0.id == user.id }) {
                users[index] = user
            }
        } catch {
            errorMessage = "Failed to update user: \(error.localizedDescription)"
        }
    }
}

// MARK: - Main View
struct UserListView: View {
    @StateObject private var viewModel: UserListViewModel
    
    init(userService: UserServiceProtocol = UserService()) {
        _viewModel = StateObject(wrappedValue: UserListViewModel(userService: userService))
    }
    
    var body: some View {
        NavigationStack {
            contentView
                .navigationTitle("Users")
                .task {
                    await viewModel.loadUsers()
                }
        }
    }
    
    @ViewBuilder
    private var contentView: some View {
        if viewModel.isLoading {
            ProgressView()
        } else if let errorMessage = viewModel.errorMessage {
            ErrorView(message: errorMessage) {
                Task { await viewModel.loadUsers() }
            }
        } else {
            userListContent
        }
    }
    
    private var userListContent: some View {
        List(viewModel.users) { user in
            NavigationLink(value: user) {
                UserRowView(user: user)
            }
        }
        .navigationDestination(for: User.self) { user in
            UserDetailView(
                user: user,
                onUpdate: { updated in
                    Task { await viewModel.updateUser(updated) }
                }
            )
        }
    }
}

// MARK: - Child View with Binding
struct UserDetailView: View {
    @State private var editedUser: User
    let onUpdate: (User) -> Void
    
    init(user: User, onUpdate: @escaping (User) -> Void) {
        _editedUser = State(initialValue: user)
        self.onUpdate = onUpdate
    }
    
    var body: some View {
        Form {
            TextField("Name", text: $editedUser.name)
            TextField("Email", text: $editedUser.email)
            
            Button("Save") {
                onUpdate(editedUser)
            }
        }
        .navigationTitle("Edit User")
    }
}

// MARK: - Reusable Component
struct UserRowView: View {
    let user: User
    
    var body: some View {
        VStack(alignment: .leading) {
            Text(user.name)
                .font(.headline)
            Text(user.email)
                .font(.subheadline)
                .foregroundStyle(.secondary)
        }
    }
}

// MARK: - Error View Component
struct ErrorView: View {
    let message: String
    let retry: () -> Void
    
    var body: some View {
        ContentUnavailableView {
            Label("Error", systemImage: "exclamationmark.triangle")
        } description: {
            Text(message)
        } actions: {
            Button("Retry", action: retry)
        }
    }
}
```

## Architecture Patterns

### Pattern 1: Environment-Based Dependency Injection
```swift
// Define environment key
private struct UserServiceKey: EnvironmentKey {
    static let defaultValue: UserServiceProtocol = UserService()
}

extension EnvironmentValues {
    var userService: UserServiceProtocol {
        get { self[UserServiceKey.self] }
        set { self[UserServiceKey.self] = newValue }
    }
}

// Use in views
struct ContentView: View {
    @Environment(\.userService) private var userService
    
    var body: some View {
        Text("Hello")
            .task {
                let users = try? await userService.fetchUsers()
            }
    }
}

// Inject in app
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
                .environment(\.userService, UserService())
        }
    }
}
```

### Pattern 2: Observable Macro (iOS 17+)
```swift
import Observation

@Observable
class UserListViewModel {
    var users: [User] = []
    var isLoading = false
    var errorMessage: String?
    
    private let userService: UserServiceProtocol
    
    init(userService: UserServiceProtocol) {
        self.userService = userService
    }
    
    func loadUsers() async {
        isLoading = true
        defer { isLoading = false }
        
        do {
            users = try await userService.fetchUsers()
        } catch {
            errorMessage = error.localizedDescription
        }
    }
}

// Use without @StateObject/@ObservedObject
struct UserListView: View {
    let viewModel: UserListViewModel
    
    var body: some View {
        List(viewModel.users) { user in
            Text(user.name)
        }
    }
}
```

### Pattern 3: ViewModifier for Common Styling
```swift
struct CardStyle: ViewModifier {
    func body(content: Content) -> some View {
        content
            .padding()
            .background(Color(.systemBackground))
            .clipShape(RoundedRectangle(cornerRadius: 12))
            .shadow(color: .black.opacity(0.1), radius: 5, y: 2)
    }
}

extension View {
    func cardStyle() -> some View {
        modifier(CardStyle())
    }
}

// Usage
Text("Hello")
    .cardStyle()
```

### Pattern 4: Container Views
```swift
struct AsyncContentView<Content: View>: View {
    let isLoading: Bool
    let error: Error?
    @ViewBuilder let content: () -> Content
    let retry: () -> Void
    
    var body: some View {
        Group {
            if isLoading {
                ProgressView()
            } else if let error = error {
                ErrorView(message: error.localizedDescription, retry: retry)
            } else {
                content()
            }
        }
    }
}

// Usage
AsyncContentView(
    isLoading: viewModel.isLoading,
    error: viewModel.error,
    content: { UserList(users: viewModel.users) },
    retry: { Task { await viewModel.load() } }
)
```

### Pattern 5: Preference Keys for Child-to-Parent Communication
```swift
struct ScrollOffsetPreferenceKey: PreferenceKey {
    static var defaultValue: CGFloat = 0
    static func reduce(value: inout CGFloat, nextValue: () -> CGFloat) {
        value = nextValue()
    }
}

struct ContentView: View {
    @State private var scrollOffset: CGFloat = 0
    
    var body: some View {
        ScrollView {
            VStack {
                ForEach(0..<50) { i in
                    Text("Item \(i)")
                }
            }
            .background(
                GeometryReader { geometry in
                    Color.clear.preference(
                        key: ScrollOffsetPreferenceKey.self,
                        value: geometry.frame(in: .named("scroll")).minY
                    )
                }
            )
        }
        .coordinateSpace(name: "scroll")
        .onPreferenceChange(ScrollOffsetPreferenceKey.self) { value in
            scrollOffset = value
        }
    }
}
```

## Anti-Patterns to Avoid

### ❌ Don't Mutate State in View Body
```swift
// Bad: Side effects in body
var body: some View {
    let _ = self.count += 1  // Don't do this!
    Text("\(count)")
}

// Good: Use proper state management
var body: some View {
    Text("\(count)")
        .onAppear {
            count += 1
        }
}
```

### ❌ Don't Use @StateObject for Injected Dependencies
```swift
// Bad: Creating new instance every time
struct MyView: View {
    @StateObject private var viewModel = MyViewModel()
    
    init(dependency: Dependency) {
        // Can't inject here properly
    }
}

// Good: Inject through initializer
struct MyView: View {
    @StateObject private var viewModel: MyViewModel
    
    init(dependency: Dependency) {
        _viewModel = StateObject(wrappedValue: MyViewModel(dependency: dependency))
    }
}
```

### ❌ Don't Overuse EnvironmentObject
```swift
// Bad: Everything in environment
@EnvironmentObject var networkManager: NetworkManager
@EnvironmentObject var authManager: AuthManager
@EnvironmentObject var dataManager: DataManager
// ... too many dependencies

// Good: Inject only truly global state
@StateObject private var viewModel: MyViewModel
// Inject specific dependencies through init
```

### ❌ Don't Create Massive View Models
```swift
// Bad: God object view model
class AppViewModel: ObservableObject {
    @Published var users: [User] = []
    @Published var posts: [Post] = []
    @Published var settings: Settings = .default
    // ... hundreds of properties
}

// Good: Split by feature
class UserViewModel: ObservableObject { }
class PostViewModel: ObservableObject { }
class SettingsViewModel: ObservableObject { }
```
