# Android Interview Revision

## Index

1.  [Android Basics -- Activity, Fragment, Intent,
    Lifecycle](#1-android-basics)
2.  [Android Components -- Service, BroadcastReceiver,
    ContentProvider](#2-android-components)
3.  [UI Development -- XML, Views, RecyclerView, Adapter,
    ViewBinding](#3-ui-development)
4.  [Navigation -- Fragment Navigation, Back Stack, Navigation
    Component](#4-navigation)
5.  [Architecture -- ViewModel, LiveData, StateFlow, Repository,
    MVVM](#5-architecture)
6.  [Kotlin for Android -- Core Kotlin concepts](#6-kotlin-for-android)
7.  [Coroutines and Flow -- Suspend, Flow,
    Channels](#7-coroutines-and-flow)
8.  [Networking -- Retrofit, OkHttp, API integration,
    JSON](#8-networking)
9.  [Dependency Injection -- Hilt, Dagger](#9-dependency-injection)
10. [Background Tasks -- WorkManager, Threads,
    Handler](#10-background-tasks)
11. [Clean Architecture and Design -- SOLID,
    patterns](#11-clean-architecture-and-design)
12. [Jetpack Compose -- UI, state, recomposition](#12-jetpack-compose)
13. [Performance -- Memory leaks, ANR, optimization](#13-performance)
14. [Security and Storage -- DataStore, SharedPreferences, secure
    storage](#14-security-and-storage)
15. [Advanced Android -- Notifications, permissions, PendingIntent, deep
    links](#15-advanced-android)
16. [Testing -- Unit tests, UI tests, mocking](#16-testing)
17. [Modularization and Gradle -- Multi-module apps, build
    setup](#17-modularization-and-gradle)
18. [Build and Release -- R8, flavors, CI/CD](#18-build-and-release)
19. [System Design -- Scalability, offline-first, app
    design](#19-system-design)
20. [Interview Problem Solving -- Coding and real-world
    scenarios](#20-interview-problem-solving)

------------------------------------------------------------------------

## 1. Android Basics

**Revision focus:** Understand the main Android building blocks and
their lifecycles.

### Activity

**Q: What is an Activity?**\
A: An Activity represents a screen that a user can interact with. It
manages the UI and user interactions for that screen.

**Q: Explain the Activity lifecycle.**\
A: `onCreate()` initializes the screen, `onStart()` makes it visible,
and `onResume()` makes it interactive. When leaving, it may call
`onPause()`, `onStop()`, and finally `onDestroy()`.

**Q: What is the difference between onPause() and onStop()?**\
A: `onPause()` means the Activity is losing focus but may still be
partly visible. `onStop()` means it is no longer visible.

**Q: When is onRestart() called?**\
A: It is called when a stopped Activity is coming back to the
foreground, before `onStart()`.

**Q: Is onDestroy() always called?**\
A: No. Android can kill the process without calling it, so important
state should not depend on `onDestroy()`.

### Fragment

**Q: What is a Fragment?**\
A: A Fragment is a reusable part of a screen hosted inside an Activity
or another Fragment.

**Q: Why use Fragments?**\
A: They help split a screen into smaller reusable parts and support
flexible layouts and navigation.

**Q: Does a Fragment have its own lifecycle?**\
A: Yes. It has a lifecycle linked to its host, plus a separate view
lifecycle for its UI.

**Q: What is the difference between onCreate() and onCreateView()?**\
A: `onCreate()` initializes Fragment-level data. `onCreateView()`
creates the Fragment's view hierarchy.

**Q: Why should we use viewLifecycleOwner?**\
A: The Fragment view can be destroyed while the Fragment remains alive.
`viewLifecycleOwner` makes observers follow the view lifecycle and helps
prevent UI-related leaks.

### Intent

**Q: What is an Intent?**\
A: An Intent is a messaging object used to request an action from
another Android component, such as opening an Activity or starting a
Service.

**Q: Explicit vs implicit Intent?**\
A: An explicit Intent names the target component. An implicit Intent
describes an action and lets Android find a suitable component.

**Q: What is a Bundle?**\
A: A Bundle is a key-value container commonly used to pass small amounts
of data between Android components.

**Q: Should large objects be passed through Intent extras?**\
A: No. Pass an ID or small data and load the full object from a
repository or database.

------------------------------------------------------------------------

## 2. Android Components

**Revision focus:** Know what each component does and when to use it.

### Service

**Q: What is a Service?**\
A: A Service is an Android component for work that can continue without
a user interface. A Service does not automatically run on a background
thread.

**Q: What are the main types of Service?**\
A: Started Service performs work started by a component. Bound Service
lets components interact with it. A Foreground Service performs
user-noticeable work and must show a notification.

**Q: Does a Service run on the main thread?**\
A: By default, Service callbacks run on the app's main thread. Move
long-running work to an appropriate background mechanism.

**Q: Service vs WorkManager?**\
A: Use a Service for work that needs to run now or remain active while
appropriate. Use WorkManager for deferrable, guaranteed,
constraint-based work.

### BroadcastReceiver

**Q: What is a BroadcastReceiver?**\
A: It receives broadcast messages from Android or other apps, such as
battery or connectivity-related events.

**Q: Static vs dynamic receiver?**\
A: A static receiver is declared in the manifest, subject to Android
restrictions. A dynamic receiver is registered in code and should be
unregistered when no longer needed.

**Q: What is onReceive()?**\
A: It is the callback invoked when a matching broadcast is received. It
should finish quickly; long work should be delegated.

**Q: What is an ordered broadcast?**\
A: It is delivered to receivers in order, allowing a receiver to pass
results or abort delivery where permitted.

### ContentProvider

**Q: What is a ContentProvider?**\
A: It manages and shares structured app data with other apps through
content URIs.

**Q: What is ContentResolver?**\
A: It is the client API used to access data exposed by a
ContentProvider.

**Q: What is a content URI?**\
A: It uniquely identifies data exposed by a provider, commonly in a form
such as `content://authority/path`.

------------------------------------------------------------------------

## 3. UI Development

**Revision focus:** Build efficient screens and understand how Android
renders lists and views.

### XML and Views

**Q: What is a View?**\
A: A View is the basic UI element, such as a Button, TextView, or
EditText.

**Q: View vs ViewGroup?**\
A: A View is a UI element. A ViewGroup is a container that holds and
arranges other Views.

**Q: LinearLayout vs ConstraintLayout?**\
A: LinearLayout arranges children in a row or column. ConstraintLayout
positions views using relationships and is useful for complex responsive
layouts.

**Q: What is a custom View?**\
A: A custom View is a UI component created by extending View or another
View class to provide custom drawing or behavior.

### RecyclerView and Adapter

**Q: What is RecyclerView?**\
A: RecyclerView efficiently displays large or changing lists by reusing
item views instead of creating a new view for every item.

**Q: What is an Adapter?**\
A: An Adapter connects the data source to RecyclerView and creates and
binds item views.

**Q: What is ViewHolder?**\
A: ViewHolder stores references to item views so RecyclerView can reuse
them efficiently.

**Q: What is DiffUtil?**\
A: DiffUtil calculates the difference between old and new lists and
updates only the changed items.

**Q: ListView vs RecyclerView?**\
A: RecyclerView offers a more flexible layout system, ViewHolder
pattern, item animations, and better list-update tools.

### ViewBinding

**Q: What is ViewBinding?**\
A: ViewBinding generates type-safe binding classes for XML layouts and
reduces manual view lookups.

**Q: ViewBinding vs findViewById()?**\
A: ViewBinding provides direct, type-safe references and reduces casting
and null-related mistakes.

**Q: ViewBinding vs DataBinding?**\
A: ViewBinding provides view references. DataBinding also supports
binding expressions and observable data in XML.

------------------------------------------------------------------------

## 4. Navigation

**Revision focus:** Understand screen transitions, back behavior, and
navigation state.

**Q: What is Fragment navigation?**\
A: It is the process of moving between Fragments, commonly using
FragmentManager or the Navigation Component.

**Q: What is the Fragment back stack?**\
A: It stores previous Fragment transactions so the user can return to
earlier screens using Back.

**Q: What is the difference between add() and replace()?**\
A: `add()` places a Fragment into a container without removing existing
Fragments. `replace()` removes the Fragments in that container and adds
the new one.

**Q: What does addToBackStack() do?**\
A: It saves a Fragment transaction on the back stack. Pressing Back
reverses that transaction instead of immediately leaving the Activity.

**Q: What is the Navigation Component?**\
A: It is a Jetpack library that manages app navigation using navigation
graphs, destinations, actions, and a NavController.

**Q: What is Safe Args?**\
A: Safe Args is a Gradle plugin that generates type-safe classes for
passing arguments between Navigation destinations.

**Q: What is a deep link?**\
A: A deep link opens a specific destination in an app from a URL or
another external entry point.

------------------------------------------------------------------------

## 5. Architecture

**Revision focus:** Separate UI, business logic, and data access for
maintainable apps.

### MVVM

**Q: What is MVVM?**\
A: MVVM separates the UI (View), UI state and logic (ViewModel), and
data operations (Model or data layer).

**Q: What is ViewModel?**\
A: ViewModel stores and manages UI-related data. It survives
configuration changes such as screen rotation.

**Q: Does ViewModel survive process death?**\
A: No. ViewModel survives configuration changes, not process death. Use
SavedStateHandle or persistent storage for state that must be restored.

**Q: Should ViewModel hold an Activity or View reference?**\
A: No. It can outlive the UI, so holding Activity or View references can
cause memory leaks.

### LiveData and StateFlow

**Q: What is LiveData?**\
A: LiveData is a lifecycle-aware observable data holder commonly used to
update UI when data changes.

**Q: What is StateFlow?**\
A: StateFlow is a hot Kotlin Flow that always holds a current value and
emits updates to collectors.

**Q: LiveData vs StateFlow?**\
A: LiveData is lifecycle-aware by design. StateFlow is coroutine-based
and works across layers; UI collection should use lifecycle-aware
collection such as `repeatOnLifecycle`.

**Q: StateFlow vs SharedFlow?**\
A: StateFlow represents the latest state and requires an initial value.
SharedFlow is more flexible for broadcasting events or values to
multiple collectors.

### Repository

**Q: What is the Repository pattern?**\
A: A Repository provides a clean API for data access and hides whether
data comes from a network, database, or cache.

**Q: Why use a Repository?**\
A: It separates data sources from the ViewModel, makes testing easier,
and centralizes data-related decisions.

**Q: What is a single source of truth?**\
A: It means one defined source owns the current data, and other layers
observe or update it through that source.

------------------------------------------------------------------------

## 6. Kotlin for Android

**Revision focus:** Be comfortable explaining Kotlin features used in
production code.

**Q: val vs var?**\
A: `val` is read-only after initialization. `var` can be reassigned.

**Q: lateinit vs lazy?**\
A: `lateinit` initializes a non-null `var` later. `lazy` initializes a
`val` the first time it is accessed.

**Q: What is a data class?**\
A: A data class is designed to hold data and automatically provides
methods such as `equals()`, `hashCode()`, `toString()`, and `copy()`.

**Q: What is a sealed class?**\
A: A sealed class restricts its direct subclasses to a known set, making
it useful for representing states or results.

**Q: What is an extension function?**\
A: It adds callable functionality to an existing type without modifying
or inheriting from that type.

**Q: What is a higher-order function?**\
A: A function that accepts another function as a parameter or returns a
function.

**Q: What is a lambda?**\
A: A lambda is an anonymous function that can be stored in a variable or
passed as an argument.

**Q: What is a nullable type?**\
A: A nullable type can hold `null`, written with `?`, such as `String?`.

**Q: What are safe call and Elvis operators?**\
A: `?.` calls a property or function only when the value is not null.
`?:` provides a fallback value when the expression is null.

**Q: What is an inline function?**\
A: An inline function asks the compiler to place the function body at
the call site, which can reduce lambda overhead in suitable cases.

**Q: What is reified in Kotlin?**\
A: `reified` keeps a generic type available at runtime inside an inline
function.

**Q: == vs ===?**\
A: `==` checks structural equality using `equals()`. `===` checks
whether two references point to the same object.

------------------------------------------------------------------------

## 7. Coroutines and Flow

**Revision focus:** Explain asynchronous work, cancellation, and stream
handling.

**Q: What is a Coroutine?**\
A: A coroutine is a lightweight way to perform asynchronous work without
blocking a thread.

**Q: What is suspend?**\
A: A `suspend` function can pause and resume without blocking the
thread. It must be called from a coroutine or another suspend function.

**Q: launch vs async?**\
A: `launch` starts work and returns a Job. `async` returns a Deferred
that provides a result through `await()`.

**Q: What is a CoroutineScope?**\
A: It defines the lifecycle and cancellation boundary for coroutines.

**Q: What is Dispatchers.Main, IO, and Default?**\
A: Main is for UI work, IO is for blocking input/output operations, and
Default is for CPU-intensive work.

**Q: What is structured concurrency?**\
A: It keeps child coroutines within a defined scope so cancellation and
errors can be managed together.

**Q: What is a Flow?**\
A: Flow is a cold asynchronous stream that emits values when it is
collected.

**Q: Flow vs StateFlow?**\
A: Flow is usually cold and starts work for each collector. StateFlow is
hot and always keeps the latest state.

**Q: What is SharedFlow?**\
A: SharedFlow is a hot stream that can broadcast values to multiple
collectors.

**Q: What is flowOn()?**\
A: `flowOn()` changes the coroutine context used by upstream Flow
operations.

**Q: What is collectLatest()?**\
A: It cancels the previous collector action when a new value arrives and
starts handling the latest value.

**Q: How do you handle coroutine errors?**\
A: Use appropriate `try/catch`, `CoroutineExceptionHandler` where
suitable, and structured concurrency. Handle cancellation separately and
do not swallow `CancellationException`.

------------------------------------------------------------------------

## 8. Networking

**Revision focus:** Know the API request lifecycle, error handling, and
data mapping.

**Q: What is Retrofit?**\
A: Retrofit is a type-safe HTTP client for Android and Kotlin that
converts API interfaces into network calls.

**Q: What is OkHttp?**\
A: OkHttp is an HTTP client that handles network connections, requests,
responses, interceptors, and caching.

**Q: Retrofit vs OkHttp?**\
A: Retrofit provides a convenient API interface layer. OkHttp performs
the underlying HTTP communication.

**Q: What is an Interceptor?**\
A: An Interceptor can inspect or modify requests and responses, commonly
for authentication headers, logging, or common error handling.

**Q: What is JSON serialization?**\
A: It converts objects into JSON for sending or receiving data.
Deserialization converts JSON into objects.

**Q: What are common HTTP status codes?**\
A: 200 means success, 201 means created, 400 means bad request, 401
means unauthenticated, 403 means forbidden, 404 means not found, and 500
means server error.

**Q: How do you handle API errors?**\
A: Check HTTP status codes, parse error responses safely, map them to
app-level errors, and show a useful message in the UI.

**Q: How do you handle token expiry?**\
A: Detect authentication failure, refresh the token safely when
supported, retry the request once, and send the user to login if refresh
fails.

**Q: What is API pagination?**\
A: Pagination loads data in smaller pages instead of downloading the
full dataset at once.

**Q: What is caching?**\
A: Caching stores previously fetched data so it can be reused, reducing
network calls and improving response time.

------------------------------------------------------------------------

## 9. Dependency Injection

**Revision focus:** Explain object creation, dependency management, and
testability.

**Q: What is Dependency Injection?**\
A: Dependency Injection provides an object with the dependencies it
needs instead of making it create them itself.

**Q: Why use DI?**\
A: It reduces tight coupling, improves testability, and makes object
creation easier to manage.

**Q: What is Dagger?**\
A: Dagger is a compile-time dependency injection framework that
generates code to provide dependencies.

**Q: What is Hilt?**\
A: Hilt is built on Dagger and provides Android-specific components and
standard dependency scopes.

**Q: Hilt vs Dagger?**\
A: Hilt simplifies Dagger setup for Android apps. Dagger gives more
manual control but needs more configuration.

**Q: What is a Singleton scope?**\
A: It means one instance is shared within the lifetime of the component
or scope where it is provided.

**Q: What is constructor injection?**\
A: Dependencies are passed through a class constructor. It is usually
the preferred way because requirements are clear and easy to test.

------------------------------------------------------------------------

## 10. Background Tasks

**Revision focus:** Choose the correct tool for work outside the
immediate UI flow.

**Q: What is WorkManager?**\
A: WorkManager schedules deferrable background work that should run
reliably, even if the app exits or the device restarts.

**Q: When should you use WorkManager?**\
A: Use it for tasks such as syncing data or uploading logs when the work
can be delayed and should satisfy constraints.

**Q: What are WorkManager constraints?**\
A: Constraints are conditions such as network availability, charging, or
battery state that must be met before work runs.

**Q: OneTimeWorkRequest vs PeriodicWorkRequest?**\
A: OneTimeWorkRequest schedules work once. PeriodicWorkRequest schedules
recurring work subject to system timing limits.

**Q: What is a Thread?**\
A: A Thread is an execution path that allows work to run separately from
other threads.

**Q: What is a Handler?**\
A: A Handler posts tasks or messages to a particular thread's
MessageQueue, commonly the main thread.

**Q: Handler vs Coroutine?**\
A: Handler works with a thread's message queue. Coroutines provide
structured asynchronous programming with cancellation and easier
composition.

**Q: Can we update UI from a background thread?**\
A: No. Android UI operations must run on the main thread. Switch to the
main dispatcher or post to the main thread.

------------------------------------------------------------------------

## 11. Clean Architecture and Design

**Revision focus:** Explain how you keep large applications
maintainable.

**Q: What is Clean Architecture?**\
A: It separates an app into layers such as presentation, domain, and
data, with dependencies pointing toward business rules.

**Q: What are the common layers?**\
A: Presentation handles UI, Domain contains business rules and use
cases, and Data handles repositories and data sources.

**Q: What is a Use Case?**\
A: A Use Case represents one specific business action, such as
submitting a payment or loading a user profile.

**Q: What is SOLID?**\
A: SOLID is a set of five design principles that help make software
easier to maintain, extend, and test.

**Q: Explain Single Responsibility Principle.**\
A: A class should have one main reason to change and should focus on one
responsibility.

**Q: Explain Open/Closed Principle.**\
A: Software entities should be open for extension but closed for
modification.

**Q: Explain Liskov Substitution Principle.**\
A: A subtype should be usable wherever its parent type is expected
without breaking correct behavior.

**Q: Explain Interface Segregation Principle.**\
A: Prefer small, focused interfaces instead of forcing classes to
implement methods they do not need.

**Q: Explain Dependency Inversion Principle.**\
A: High-level business logic should depend on abstractions, not directly
on low-level implementation details.

**Q: What is the Singleton pattern?**\
A: It provides one shared instance within a defined scope. Use it
carefully because global shared state can make testing harder.

**Q: What is the Factory pattern?**\
A: It centralizes object creation and hides the details of which
concrete class is created.

**Q: What is the Observer pattern?**\
A: It allows subscribers to receive updates when a source changes, as
seen in LiveData and Flow-based designs.

------------------------------------------------------------------------

## 12. Jetpack Compose

**Revision focus:** Understand modern declarative UI and state handling.

**Q: What is Jetpack Compose?**\
A: Jetpack Compose is Android's modern declarative UI toolkit. You
describe the UI based on state instead of manually updating Views.

**Q: XML Views vs Compose?**\
A: XML uses layouts and imperative View updates. Compose uses Kotlin
functions and declarative state-driven UI.

**Q: What is a Composable function?**\
A: A function marked with `@Composable` that describes part of the UI.

**Q: What is recomposition?**\
A: Recomposition is when Compose reruns composable functions whose
observed state has changed to update the UI.

**Q: What is remember?**\
A: `remember` keeps a value across recompositions while the composable
remains in the composition.

**Q: What is rememberSaveable?**\
A: It can preserve supported UI state across activity recreation and
system-initiated process recreation using saved state.

**Q: What is state hoisting?**\
A: It moves state to a parent composable so state ownership is clear and
child composables can remain reusable.

**Q: What is recomposition skipping?**\
A: Compose can skip recomposing eligible composables when their inputs
have not changed.

**Q: What is LaunchedEffect?**\
A: It starts a coroutine tied to a composable's lifecycle and restarts
it when its keys change.

**Q: What is LazyColumn?**\
A: LazyColumn displays a vertically scrolling list and composes items as
needed.

**Q: How do you improve Compose performance?**\
A: Keep state close to where it is used, provide stable keys for list
items, avoid expensive work during composition, and profile before
optimizing.

------------------------------------------------------------------------

## 13. Performance

**Revision focus:** Diagnose real production issues instead of guessing.

**Q: What is a memory leak?**\
A: A memory leak happens when an object is no longer needed but is still
referenced, preventing garbage collection.

**Q: Common causes of memory leaks in Android?**\
A: Holding Activity or View references in long-lived objects, not
unregistering listeners, and keeping callbacks or contexts longer than
needed.

**Q: How do you detect memory leaks?**\
A: Use Android Studio Memory Profiler and tools such as LeakCanary to
identify retained objects and reference paths.

**Q: What is ANR?**\
A: ANR means Application Not Responding. It usually happens when the
main thread is blocked for too long.

**Q: Common causes of ANR?**\
A: Long database or network work on the main thread, heavy calculations,
lock contention, or slow initialization.

**Q: How do you prevent ANR?**\
A: Keep the main thread responsive, move blocking work to background
dispatchers, and avoid long locks or heavy operations during startup.

**Q: What is overdraw?**\
A: Overdraw occurs when the same screen pixel is drawn multiple times in
one frame, wasting rendering work.

**Q: How do you improve app startup time?**\
A: Delay nonessential initialization, avoid heavy work in Application
startup, use lazy initialization, and measure startup with profiling
tools.

**Q: How do you improve RecyclerView performance?**\
A: Use efficient item layouts, DiffUtil/ListAdapter, stable item
identity where appropriate, and avoid unnecessary rebinding.

**Q: What tools help with performance analysis?**\
A: Android Studio Profiler, CPU and Memory Profiler, Layout Inspector,
Perfetto, and Macrobenchmark.

------------------------------------------------------------------------

## 14. Security and Storage

**Revision focus:** Store data safely and protect user information.

**Q: SharedPreferences vs DataStore?**\
A: SharedPreferences is a simple key-value storage API. DataStore is a
modern asynchronous storage solution that uses Flow and supports
Preferences or typed data.

**Q: What is Preferences DataStore?**\
A: It stores key-value data asynchronously without a predefined schema.

**Q: What is Room?**\
A: Room is a persistence library over SQLite that provides entities,
DAOs, and compile-time query checking.

**Q: Where should authentication tokens be stored?**\
A: Store sensitive tokens using a secure design, such as encrypted
storage backed by Android Keystore. Avoid plain text storage.

**Q: What is Android Keystore?**\
A: Android Keystore protects cryptographic keys and can keep key
material outside the app process.

**Q: What is encryption at rest?**\
A: It means data is encrypted while stored on the device, reducing
exposure if storage is accessed without authorization.

**Q: How should API keys be protected?**\
A: Do not assume a key embedded in an APK is secret. Restrict keys on
the server/provider side and keep sensitive secrets on a trusted
backend.

**Q: What is certificate pinning?**\
A: It restricts which server certificates or public keys an app trusts,
providing extra protection against some man-in-the-middle attacks. It
needs a safe rotation strategy.

**Q: What is ProGuard/R8?**\
A: R8 shrinks, optimizes, and obfuscates app code for release builds. It
is not a replacement for secure storage or server-side security.

------------------------------------------------------------------------

## 15. Advanced Android

**Revision focus:** Understand platform behavior and secure component
interaction.

**Q: What is a Notification?**\
A: A notification displays information or actions to the user outside
the app's normal UI.

**Q: What is a Notification Channel?**\
A: On Android 8.0 and above, notifications belong to channels that users
can manage individually.

**Q: What is PendingIntent?**\
A: A PendingIntent lets another app or the Android system perform a
predefined action on your app's behalf later.

**Q: PendingIntent vs Intent?**\
A: An Intent describes an action. A PendingIntent grants another process
permission to execute that action later.

**Q: What is the difference between FLAG_IMMUTABLE and FLAG_MUTABLE?**\
A: Immutable prevents the receiver from changing the PendingIntent's
contents. Mutable allows permitted changes and should be used only when
required.

**Q: What are runtime permissions?**\
A: These are dangerous permissions that users grant while the app is
running, when the feature needs them.

**Q: What is a deep link?**\
A: A deep link opens a specific app destination using a URI or web link.

**Q: Deep link vs App Link?**\
A: A deep link can use a custom URI scheme or web URL. An Android App
Link is a verified HTTP/HTTPS link associated with a website and app.

**Q: What is a foreground service notification?**\
A: It is the required visible notification that informs users about
ongoing foreground-service work.

------------------------------------------------------------------------

## 16. Testing

**Revision focus:** Show that you can build reliable code and verify
behavior.

**Q: What is Unit Testing?**\
A: Unit testing checks a small piece of code, such as a function or
ViewModel, in isolation.

**Q: What is UI Testing?**\
A: UI testing checks app behavior through the user interface, such as
tapping buttons and verifying displayed results.

**Q: Unit test vs Instrumentation test?**\
A: Unit tests usually run on the local JVM. Instrumentation tests run on
an Android device or emulator.

**Q: What is JUnit?**\
A: JUnit is a testing framework used to write and run tests on the JVM.

**Q: What is MockK or Mockito?**\
A: They are mocking libraries used to create test doubles for
dependencies.

**Q: What is a fake vs mock?**\
A: A fake is a working lightweight implementation. A mock is an object
configured to verify interactions or return specified results.

**Q: How do you test a ViewModel?**\
A: Provide fake or mocked dependencies, trigger ViewModel actions, and
assert the emitted UI state.

**Q: How do you test Coroutines and Flow?**\
A: Use coroutine test utilities such as `runTest` and a test dispatcher,
then collect and assert Flow emissions.

**Q: What should be tested in a Repository?**\
A: Test data mapping, source selection, caching behavior, error
handling, and interactions with data sources.

------------------------------------------------------------------------

## 17. Modularization and Gradle

**Revision focus:** Explain how to structure and maintain a growing
Android codebase.

**Q: What is modularization?**\
A: Modularization splits an app into smaller Gradle modules with clear
responsibilities and dependencies.

**Q: Why use multiple modules?**\
A: It improves separation of concerns, ownership, reuse, testability,
and can improve build performance when structured well.

**Q: What is an Android app module?**\
A: The application module builds the installable app and brings together
the modules and resources needed for the application.

**Q: What is a library module?**\
A: A library module contains reusable code or resources that can be
included by other modules.

**Q: What is a feature module?**\
A: A feature module contains a specific app feature, such as login,
payments, or profile.

**Q: What is Gradle?**\
A: Gradle is the build automation system used to compile code, manage
dependencies, and package Android apps.

**Q: What is the difference between implementation and api
dependencies?**\
A: `implementation` keeps a dependency internal to the module's
consumers. `api` exposes it to consumers and can increase compile-time
coupling.

**Q: What is a Gradle build variant?**\
A: A build variant is a specific app build created from build types and
product flavors.

------------------------------------------------------------------------

## 18. Build and Release

**Revision focus:** Understand how an app is configured, optimized,
signed, and delivered.

**Q: Debug vs Release build?**\
A: Debug builds are intended for development and debugging. Release
builds are optimized and prepared for distribution.

**Q: What are product flavors?**\
A: Product flavors create different app variants, such as development,
staging, and production, with different settings or resources.

**Q: What are build types?**\
A: Build types define build behavior, commonly `debug` and `release`,
including settings such as minification and signing.

**Q: What is R8?**\
A: R8 reduces app size by shrinking and optimizing code and can
obfuscate names to make reverse engineering harder.

**Q: What are ProGuard rules?**\
A: Keep rules tell the shrinker which classes or members must not be
removed or renamed, often for reflection or library compatibility.

**Q: What is APK vs AAB?**\
A: APK is an installable Android package. AAB is a publishing format
from which Google Play generates optimized APKs for devices.

**Q: What is app signing?**\
A: App signing verifies the app's identity and allows Android and app
stores to validate updates from the same publisher.

**Q: What is CI/CD?**\
A: CI/CD automates steps such as building, testing, signing, and
distributing app versions.

**Q: What should a mobile CI pipeline include?**\
A: Code checks, unit tests, build variants, secure signing, artifact
storage, and controlled deployment to testing or production.

------------------------------------------------------------------------

## 19. System Design

**Revision focus:** Be ready to explain design decisions for large,
real-world apps.

**Q: How would you design a scalable Android app?**\
A: Separate features and layers, define clear interfaces, use
repositories, inject dependencies, and keep UI state predictable and
testable.

**Q: What is offline-first architecture?**\
A: The app reads from local storage first and synchronizes with the
network when available, so core screens can work offline.

**Q: How would you handle data synchronization?**\
A: Store local changes, track pending operations, retry safely, resolve
conflicts, and synchronize when network and system constraints allow.

**Q: What is pagination and why is it useful?**\
A: It loads data in smaller chunks, reducing memory use, network
traffic, and initial loading time.

**Q: How would you handle a slow API?**\
A: Show loading and cached content where possible, set suitable
timeouts, handle retries carefully, and provide clear error and retry
states.

**Q: How would you design authentication?**\
A: Use secure token handling, centralized authentication logic, safe
refresh handling, and server-side authorization checks.

**Q: How do you handle duplicate API requests?**\
A: Centralize request ownership, share in-flight results where suitable,
and prevent repeated UI triggers.

**Q: How do you handle conflicting local and server data?**\
A: Define a clear conflict policy, such as server version, timestamps,
or user choice, based on the business requirement.

**Q: How do you make an app maintainable for a large team?**\
A: Define coding standards, module ownership, architecture boundaries,
code reviews, automated tests, and shared documentation.

**Q: How do you choose between a quick fix and a refactor?**\
A: Consider user impact, production risk, time, future maintenance cost,
and test coverage. Make the smallest safe change unless a larger
redesign is justified.

------------------------------------------------------------------------

## 20. Interview Problem Solving

**Revision focus:** Explain your approach clearly, not just the final
code.

**Q: How do you approach a coding problem?**\
A: Clarify the input and expected output, discuss edge cases, choose a
suitable approach, explain complexity, then write and test the code.

**Q: What is time complexity?**\
A: Time complexity describes how the running time grows as the input
size increases.

**Q: What is space complexity?**\
A: Space complexity describes how the extra memory requirement grows
with input size.

**Q: Array vs List?**\
A: An array has a fixed size. A Kotlin List is a collection interface; a
MutableList supports adding and removing elements.

**Q: List vs Set?**\
A: A List preserves element order and can contain duplicates. A Set
stores unique elements.

**Q: What is a HashMap?**\
A: A HashMap stores key-value pairs and typically provides average
constant-time lookup by key.

**Q: What is a race condition?**\
A: A race condition happens when the result depends on the unpredictable
timing of concurrent operations.

**Q: What is thread safety?**\
A: Thread safety means code behaves correctly when accessed by multiple
threads at the same time.

**Q: How do you debug a production crash?**\
A: Check crash reports and stack traces, identify affected versions and
devices, reproduce the issue, find the root cause, fix it, and add
regression coverage.

**Q: How do you explain a technical decision in an interview?**\
A: Explain the problem, options considered, chosen solution, trade-offs,
result, and what you would improve.

**Q: How do you answer a project-based question?**\
A: Use STAR: Situation, Task, Action, and Result. Focus on your own
contribution and measurable impact.
