---
name: navigation
description: Expert guidance for implementing modern navigation patterns in SwiftUI with NavigationStack, deep linking, and state management. Use when building navigation flows, managing state restoration, or implementing deep links.
---

# Navigation Skill

## Overview
Use this skill to implement modern SwiftUI navigation using NavigationStack, handle deep links properly, and manage complex navigation flows. This skill helps you build intuitive user experiences with proper state management and navigation patterns.

## Workflow Decision Tree

### 1) Review existing navigation code
- Verify NavigationStack is used instead of deprecated NavigationView
- Check that navigation state is centralized in coordinators or route enums
- Verify deep link handling with URL parsing and validation
- Check for proper state restoration
- Verify sheets and full-screen covers are managed correctly
- Check for excessive navigation state variables

### 2) Improve existing navigation code
- Replace NavigationView with NavigationStack
- Replace scattered navigation logic with centralized coordinator
- Implement type-safe navigation with Codable routes
- Add deep link support if missing
- Simplify sheet management with enums instead of multiple @State bools
- Implement state restoration for navigation paths

### 3) Implement new navigation feature
- Use NavigationStack for linear navigation flows
- Create navigation coordinator for centralized routing
- Define routes as Hashable enums for type safety
- Implement deep link parsing for URL schemes
- Use proper sheet/fullScreenCover management
- Handle navigation state restoration
- Test navigation flows and back navigation

## Core Guidelines

### Use NavigationStack for Modern Apps (iOS 16+)
- Prefer `NavigationStack` over deprecated `NavigationView`
- Use value-based navigation with `navigationDestination(for:)`
- Leverage `NavigationPath` for dynamic navigation stacks
- Use type-safe navigation with codable routes

### Centralize Navigation Logic
- Create a navigation coordinator or router
- Use enums to represent navigation destinations
- Keep navigation state in a single source of truth
- Avoid scattering navigation logic throughout views

### Handle Deep Links Properly
- Parse URLs into navigation routes
- Validate deep link parameters
- Handle navigation state restoration
- Support universal links and custom URL schemes

### Manage Navigation State
- Store navigation paths for state restoration
- Handle back navigation correctly
- Support programmatic navigation
- Clear navigation stack when appropriate (e.g., logout)

### Handle Sheet and FullScreenCover Properly
- Use `@State` for simple sheet presentation
- Use view model properties for complex state
- Support dismissal from within sheets
- Handle multiple presentation layers correctly

### Test Navigation Flows
- Test deep link parsing
- Verify navigation stack behavior
- Test back navigation
- Validate state restoration

## Tradeoffs

### NavigationStack vs NavigationSplitView
**NavigationStack:**
- Linear navigation flow
- Mobile-first design
- Single column on iPhone
- Simple hierarchical navigation

**NavigationSplitView:**
- Multi-column layout
- iPad/Mac optimized
- Sidebar + detail pattern
- More complex state management

### Coordinator Pattern vs Direct Navigation
**Coordinator:**
- Centralized navigation logic
- Better testability
- More boilerplate
- Clearer navigation flow

**Direct Navigation:**
- Less boilerplate
- Simpler for small apps
- Navigation mixed with views
- Harder to test

## Output Template

```swift
// MARK: - Navigation Route (Type-Safe)
enum Route: Hashable, Codable {
    case userList
    case userDetail(userId: String)
    case userEdit(userId: String)
    case settings
    case profile(username: String)
}

// MARK: - Navigation Coordinator
@MainActor
class NavigationCoordinator: ObservableObject {
    @Published var path = NavigationPath()
    
    func navigate(to route: Route) {
        path.append(route)
    }
    
    func navigateToRoot() {
        path.removeLast(path.count)
    }
    
    func navigateBack() {
        if !path.isEmpty {
            path.removeLast()
        }
    }
    
    func handleDeepLink(_ url: URL) {
        guard let route = parseDeepLink(url) else { return }
        
        // Clear existing navigation
        path.removeLast(path.count)
        
        // Navigate to deep link destination
        navigate(to: route)
    }
    
    private func parseDeepLink(_ url: URL) -> Route? {
        // myapp://user/123
        guard url.scheme == "myapp" else { return nil }
        
        let components = url.pathComponents.filter { $0 != "/" }
        
        switch components.first {
        case "user":
            if let userId = components.dropFirst().first {
                return .userDetail(userId: userId)
            }
        case "profile":
            if let username = components.dropFirst().first {
                return .profile(username: username)
            }
        case "settings":
            return .settings
        default:
            break
        }
        
        return nil
    }
}

// MARK: - Root Navigation View
struct RootNavigationView: View {
    @StateObject private var coordinator = NavigationCoordinator()
    
    var body: some View {
        NavigationStack(path: $coordinator.path) {
            HomeView()
                .navigationDestination(for: Route.self) { route in
                    destinationView(for: route)
                }
        }
        .environmentObject(coordinator)
        .onOpenURL { url in
            coordinator.handleDeepLink(url)
        }
    }
    
    @ViewBuilder
    private func destinationView(for route: Route) -> some View {
        switch route {
        case .userList:
            UserListView()
        case .userDetail(let userId):
            UserDetailView(userId: userId)
        case .userEdit(let userId):
            UserEditView(userId: userId)
        case .settings:
            SettingsView()
        case .profile(let username):
            ProfileView(username: username)
        }
    }
}

// MARK: - Home View with Navigation
struct HomeView: View {
    @EnvironmentObject private var coordinator: NavigationCoordinator
    
    var body: some View {
        List {
            NavigationLink(value: Route.userList) {
                Label("Users", systemImage: "person.2")
            }
            
            NavigationLink(value: Route.settings) {
                Label("Settings", systemImage: "gear")
            }
        }
        .navigationTitle("Home")
        .toolbar {
            ToolbarItem(placement: .primaryAction) {
                Button("Profile") {
                    coordinator.navigate(to: .profile(username: "current"))
                }
            }
        }
    }
}

// MARK: - Detail View with Navigation
struct UserDetailView: View {
    let userId: String
    @EnvironmentObject private var coordinator: NavigationCoordinator
    @State private var user: User?
    
    var body: some View {
        VStack {
            if let user = user {
                UserInfoView(user: user)
                
                Button("Edit User") {
                    coordinator.navigate(to: .userEdit(userId: userId))
                }
            } else {
                ProgressView()
            }
        }
        .navigationTitle("User Details")
        .task {
            user = await fetchUser(userId)
        }
    }
    
    private func fetchUser(_ id: String) async -> User? {
        // Fetch user from service
        return nil
    }
}

// MARK: - Sheet Presentation
struct HomeViewWithSheet: View {
    @State private var showingSettings = false
    @State private var selectedUser: User?
    
    var body: some View {
        List {
            Button("Show Settings") {
                showingSettings = true
            }
        }
        .sheet(isPresented: $showingSettings) {
            NavigationStack {
                SettingsView()
                    .toolbar {
                        ToolbarItem(placement: .cancellationAction) {
                            Button("Done") {
                                showingSettings = false
                            }
                        }
                    }
            }
        }
        .sheet(item: $selectedUser) { user in
            UserDetailSheet(user: user)
        }
    }
}

// MARK: - Dismissible Sheet
struct UserDetailSheet: View {
    let user: User
    @Environment(\.dismiss) private var dismiss
    
    var body: some View {
        NavigationStack {
            UserDetailView(userId: user.id)
                .toolbar {
                    ToolbarItem(placement: .cancellationAction) {
                        Button("Close") {
                            dismiss()
                        }
                    }
                }
        }
    }
}
```

## Advanced Patterns

### Pattern 1: Codable Navigation Path for State Restoration
```swift
@MainActor
class NavigationCoordinator: ObservableObject {
    @Published var path = NavigationPath()
    
    private let savePath = "navigation_path"
    
    init() {
        restorePath()
    }
    
    func savePath() {
        guard let representation = path.codable else { return }
        
        do {
            let data = try JSONEncoder().encode(representation)
            UserDefaults.standard.set(data, forKey: savePath)
        } catch {
            print("Failed to save navigation path: \(error)")
        }
    }
    
    func restorePath() {
        guard let data = UserDefaults.standard.data(forKey: savePath) else { return }
        
        do {
            let representation = try JSONDecoder().decode(
                NavigationPath.CodableRepresentation.self,
                from: data
            )
            path = NavigationPath(representation)
        } catch {
            print("Failed to restore navigation path: \(error)")
        }
    }
}

// Auto-save on changes
struct RootView: View {
    @StateObject private var coordinator = NavigationCoordinator()
    
    var body: some View {
        NavigationStack(path: $coordinator.path) {
            HomeView()
        }
        .onChange(of: coordinator.path) { _ in
            coordinator.savePath()
        }
    }
}
```

### Pattern 2: Tab-Based Navigation with Coordinators
```swift
enum Tab {
    case home, search, profile
}

struct TabCoordinator: View {
    @State private var selectedTab: Tab = .home
    @StateObject private var homeCoordinator = NavigationCoordinator()
    @StateObject private var searchCoordinator = NavigationCoordinator()
    @StateObject private var profileCoordinator = NavigationCoordinator()
    
    var body: some View {
        TabView(selection: $selectedTab) {
            NavigationStack(path: $homeCoordinator.path) {
                HomeView()
                    .navigationDestination(for: Route.self) { route in
                        destinationView(for: route)
                    }
            }
            .environmentObject(homeCoordinator)
            .tabItem { Label("Home", systemImage: "house") }
            .tag(Tab.home)
            
            NavigationStack(path: $searchCoordinator.path) {
                SearchView()
                    .navigationDestination(for: Route.self) { route in
                        destinationView(for: route)
                    }
            }
            .environmentObject(searchCoordinator)
            .tabItem { Label("Search", systemImage: "magnifyingglass") }
            .tag(Tab.search)
            
            NavigationStack(path: $profileCoordinator.path) {
                ProfileView()
                    .navigationDestination(for: Route.self) { route in
                        destinationView(for: route)
                    }
            }
            .environmentObject(profileCoordinator)
            .tabItem { Label("Profile", systemImage: "person") }
            .tag(Tab.profile)
        }
    }
    
    @ViewBuilder
    private func destinationView(for route: Route) -> some View {
        // Shared destination logic
        EmptyView()
    }
}
```

### Pattern 3: NavigationSplitView for iPad
```swift
struct SplitNavigationView: View {
    @State private var selectedRoute: Route?
    @State private var columnVisibility = NavigationSplitViewVisibility.all
    
    var body: some View {
        NavigationSplitView(columnVisibility: $columnVisibility) {
            // Sidebar
            List(routes, selection: $selectedRoute) { route in
                NavigationLink(value: route) {
                    routeLabel(for: route)
                }
            }
            .navigationTitle("Menu")
        } detail: {
            // Detail
            if let route = selectedRoute {
                destinationView(for: route)
            } else {
                ContentUnavailableView(
                    "Select an Item",
                    systemImage: "sidebar.left",
                    description: Text("Choose an item from the sidebar")
                )
            }
        }
    }
    
    private var routes: [Route] {
        [.userList, .settings, .profile(username: "current")]
    }
    
    @ViewBuilder
    private func routeLabel(for route: Route) -> some View {
        switch route {
        case .userList:
            Label("Users", systemImage: "person.2")
        case .settings:
            Label("Settings", systemImage: "gear")
        case .profile:
            Label("Profile", systemImage: "person.circle")
        default:
            EmptyView()
        }
    }
    
    @ViewBuilder
    private func destinationView(for route: Route) -> some View {
        EmptyView()
    }
}
```

### Pattern 4: Alert and Confirmation Dialog
```swift
struct UserActionsView: View {
    @State private var showingDeleteAlert = false
    @State private var showingActionSheet = false
    
    var body: some View {
        VStack {
            Button("Delete", role: .destructive) {
                showingDeleteAlert = true
            }
            
            Button("More Actions") {
                showingActionSheet = true
            }
        }
        .alert("Delete User", isPresented: $showingDeleteAlert) {
            Button("Delete", role: .destructive) {
                deleteUser()
            }
            Button("Cancel", role: .cancel) { }
        } message: {
            Text("Are you sure you want to delete this user?")
        }
        .confirmationDialog("Choose Action", isPresented: $showingActionSheet) {
            Button("Edit") { editUser() }
            Button("Share") { shareUser() }
            Button("Cancel", role: .cancel) { }
        }
    }
    
    private func deleteUser() { }
    private func editUser() { }
    private func shareUser() { }
}
```

### Pattern 5: Programmatic Navigation with ID
```swift
struct ScrollToView: View {
    @State private var selectedId: Int?
    
    var body: some View {
        VStack {
            Button("Jump to Item 50") {
                selectedId = 50
            }
            
            ScrollViewReader { proxy in
                ScrollView {
                    ForEach(0..<100) { i in
                        Text("Item \(i)")
                            .id(i)
                            .padding()
                    }
                }
                .onChange(of: selectedId) { newValue in
                    guard let id = newValue else { return }
                    withAnimation {
                        proxy.scrollTo(id, anchor: .center)
                    }
                }
            }
        }
    }
}
```

## Anti-Patterns to Avoid

### ❌ Don't Use NavigationLink with Empty Destination
```swift
// Bad: Empty destination
NavigationLink("") {
    EmptyView()
}
.onTapGesture {
    // Navigate programmatically
}

// Good: Use Button for programmatic navigation
Button("Navigate") {
    coordinator.navigate(to: .detail)
}
```

### ❌ Don't Nest NavigationStacks
```swift
// Bad: Nested NavigationStacks
NavigationStack {
    DetailView()
}

struct DetailView: View {
    var body: some View {
        NavigationStack {  // Don't do this!
            Text("Detail")
        }
    }
}

// Good: Single NavigationStack at root
NavigationStack {
    DetailView()
}

struct DetailView: View {
    var body: some View {
        Text("Detail")
    }
}
```

### ❌ Don't Mix Navigation Patterns
```swift
// Bad: Mixing NavigationStack and NavigationView
NavigationView {  // Deprecated
    NavigationStack {  // Don't mix!
        Content()
    }
}

// Good: Use NavigationStack only
NavigationStack {
    Content()
}
```

### ❌ Don't Overuse Sheet Presentation
```swift
// Bad: Too many state variables for sheets
@State private var showSheet1 = false
@State private var showSheet2 = false
@State private var showSheet3 = false
// ... 10+ more sheets

// Good: Use enum for sheet state
enum SheetType: Identifiable {
    case settings, profile, edit
    
    var id: Self { self }
}

@State private var activeSheet: SheetType?

// Then:
.sheet(item: $activeSheet) { type in
    sheetView(for: type)
}
```
