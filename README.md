# Android Interview Revision

## Lets Start

1. [Android Basics – Activity, Fragment, Intent, Lifecycle](#1-android-basics)
2. [Android Components – Service, BroadcastReceiver, ContentProvider](#2-android-components)
3. [UI Development – XML, Views, RecyclerView, Adapter, ViewBinding](#3-ui-development)
4. [Navigation – Fragment Navigation, Back Stack, Navigation Component](#4-navigation)
5. [Architecture – ViewModel, LiveData, StateFlow, Repository, MVVM](#5-architecture)
6. [Kotlin for Android – Core Kotlin concepts](#6-kotlin-for-android)
7. [Coroutines and Flow – Suspend, Flow, Channels](#7-coroutines-and-flow)
8. [Networking – Retrofit, OkHttp, API integration, JSON](#8-networking)
9. [Dependency Injection – Hilt, Dagger](#9-dependency-injection)
10. [Background Tasks – WorkManager, Threads, Handler](#10-background-tasks)
11. [Clean Architecture and Design – SOLID, patterns](#11-clean-architecture-and-design)
12. [Jetpack Compose – UI, state, recomposition](#12-jetpack-compose)
13. [Performance – Memory leaks, ANR, optimization](#13-performance)
14. [Security and Storage – DataStore, SharedPreferences, secure storage](#14-security-and-storage)
15. [Advanced Android – Notifications, permissions, PendingIntent, deep links](#15-advanced-android)
16. [Testing – Unit tests, UI tests, mocking](#16-testing)
17. [Modularization and Gradle – Multi-module apps, build setup](#17-modularization-and-gradle)
18. [Build and Release – R8, flavors, CI/CD](#18-build-and-release)
19. [System Design – Scalability, offline-first, app design](#19-system-design)
20. [Interview Problem Solving – Coding and real-world scenarios](#20-interview-problem-solving)

---

## 1. Android Basics

**Activity**

**Q: What is an Activity?**  
A: An Activity is usually one screen of an Android app. For example, LoginActivity shows the login screen and HomeActivity shows the home screen.

**Q: Explain the Activity lifecycle.**  
A: Android calls lifecycle methods as a screen opens, becomes active, goes to the background, or closes. Common order: `onCreate()` → `onStart()` → `onResume()`. When leaving: `onPause()` → `onStop()` and sometimes `onDestroy()`.

**Q: What is the difference between onPause() and onStop()?**  
A: `onPause()` means the screen is losing focus. `onStop()` means the screen is no longer visible. Example: a dialog-like screen may cause `onPause()` while the Activity is still partly visible.

**Q: When is onRestart() called?**  
A: When a stopped Activity is opened again. Example: open another app, then return to your app; the stopped Activity may receive `onRestart()`.

**Q: Is onDestroy() always called?**  
A: No. Android may kill the app process when it needs memory. Save important data before that; do not depend on `onDestroy()`.

**Fragment**

**Q: What is a Fragment?**  
A: A Fragment is a reusable part of a screen that lives inside an Activity. Example: a screen can have separate Fragments for Profile, Orders, and Settings.

**Q: Why use Fragments?**  
A: They help split a large screen into smaller parts and make navigation and reuse easier. Example: the same profile Fragment can be shown in different parts of an app.

**Q: Does a Fragment have its own lifecycle?**  
A: Yes. It has a lifecycle for the Fragment and another for its view. The view can be destroyed while the Fragment itself still exists.

**Q: What is the difference between onCreate() and onCreateView()?**  
A: Use `onCreate()` to prepare Fragment data. Use `onCreateView()` to create its UI layout.

**Q: Why use viewLifecycleOwner?**  
A: It makes UI observers stop when the Fragment's view is destroyed. Example: observe LiveData with `viewLifecycleOwner` so an old view is not updated after it is gone.

**Intent**

**Q: What is an Intent?**  
A: An Intent asks Android to perform an action. Example: open the Home screen, share text, or start a service.

**Q: Explicit vs implicit Intent?**  
A: Explicit Intent names the screen to open, such as `HomeActivity`. Implicit Intent describes an action, such as opening a website, and Android finds an app that can handle it.

**Q: What is a Bundle?**  
A: A Bundle stores small pieces of data as key-value pairs. Example: pass `userId = 101` from a list screen to a details screen.

**Q: Should we pass a large object through an Intent?**  
A: No. Pass its ID instead and load the full data from the database or API. This avoids transaction-size problems.

---

## 2. Android Components

**Service**

**Q: What is a Service?**  
A: A Service performs work without its own screen. Example: a music app can keep playing audio when the user opens another app. A Service does not automatically run on a background thread.

**Q: What are the main types of Service?**  
A: A Started Service is started to do work. A Bound Service lets another component communicate with it. A Foreground Service does user-noticeable work and shows a notification.

**Q: Does a Service run on the main thread?**  
A: Yes, its callbacks normally run on the main thread. Move long or blocking work to a background thread or another suitable API.

**Q: Service vs WorkManager?**  
A: Use a Service when work needs to run now or stay active, such as music playback. Use WorkManager for reliable work that can happen later, such as syncing pending data.

**BroadcastReceiver**

**Q: What is a BroadcastReceiver?**  
A: It listens for broadcast messages from Android or other apps. Example: your app can react to a supported system event such as device boot.

**Q: Static vs dynamic receiver?**  
A: A static receiver is declared in the Manifest, subject to Android restrictions. A dynamic receiver is registered in code and should be unregistered when no longer needed.

**Q: What is onReceive()?**  
A: It is called when the receiver gets a matching broadcast. Keep it short; start or schedule suitable background work for longer tasks.

**ContentProvider**

**Q: What is a ContentProvider?**  
A: It provides a standard way to share app data with other apps. Example: Android's Contacts provider lets an app read contacts when the user grants permission.

**Q: What is ContentResolver?**  
A: It is the API an app uses to access a ContentProvider. Example: use it to query contact information.

**Q: What is a content URI?**  
A: It is an address for data from a provider, often starting with `content://`. Example: a URI can point to a particular contact.

---

## 3. UI Development

**Views and XML**

**Q: What is a View?**  
A: A View is one UI element, such as a TextView, Button, or EditText.

**Q: View vs ViewGroup?**  
A: A View is an individual UI element. A ViewGroup holds and arranges other Views. Example: TextView is a View; LinearLayout is a ViewGroup.

**Q: LinearLayout vs ConstraintLayout?**  
A: LinearLayout places items in a row or column. ConstraintLayout positions items using constraints and is useful for complex screens.

**Q: What is a custom View?**  
A: It is a UI element you create for a special design or behavior. Example: a custom progress circle or signature pad.

**RecyclerView and Adapter**

**Q: What is RecyclerView?**  
A: RecyclerView shows a list efficiently by reusing item views. Example: showing hundreds of products without creating hundreds of views at once.

**Q: What does an Adapter do?**  
A: It connects the list data to RecyclerView. It creates item views and puts the correct data into each row.

**Q: What is ViewHolder?**  
A: It holds references to the views inside one list row. RecyclerView reuses it instead of finding the same views again and again.

**Q: What is DiffUtil?**  
A: It compares the old list with the new list and finds what changed. Example: if one product price changes, it can update that row instead of refreshing the whole list.

**Q: ListView vs RecyclerView?**  
A: RecyclerView is more flexible and has better support for different layouts, animations, and efficient list updates.

**ViewBinding**

**Q: What is ViewBinding?**  
A: It creates a binding class for an XML layout, so you can access views directly. Example: `binding.loginButton` instead of calling `findViewById()`.

**Q: ViewBinding vs DataBinding?**  
A: ViewBinding gives type-safe access to views. DataBinding also allows data and expressions to be connected directly in XML.

---

## 4. Navigation

**Q: What is Fragment navigation?**  
A: It means moving from one Fragment to another. Example: tap a product and move from ProductListFragment to ProductDetailsFragment.

**Q: What is the Fragment back stack?**  
A: It keeps previous Fragment transactions so the user can return to the previous screen by pressing Back.

**Q: What is the difference between add() and replace()?**  
A: `add()` puts a Fragment into the container without removing the existing one. `replace()` removes the current Fragment in that container and adds the new one.

**Q: What does addToBackStack() do?**  
A: It saves the transaction so Back can reverse it. Example: open Product Details from Product List; pressing Back returns to the list if the transaction was added to the back stack.

**Q: What is the Navigation Component?**  
A: It is a Jetpack library that manages app navigation using a navigation graph and NavController. It helps manage destinations and Back behavior.

**Q: What is Safe Args?**  
A: It generates type-safe classes for passing data between navigation destinations. Example: pass a product ID to the details Fragment without manually reading string keys.

**Q: What is a deep link?**  
A: It opens a specific screen from a link. Example: tapping an order link in an email opens that order inside the app.

---

## 5. Architecture

**MVVM**

**Q: What is MVVM?**  
A: MVVM separates the screen, its UI logic, and data handling. Example: Activity or Fragment displays data, ViewModel prepares it, and Repository loads it.

**Q: What is ViewModel?**  
A: ViewModel holds screen-related data and logic. It survives configuration changes such as screen rotation.

**Q: Does ViewModel survive process death?**  
A: No. It survives configuration changes, but not process death. Use SavedStateHandle or a database to restore important data.

**Q: Should ViewModel hold an Activity or View reference?**  
A: No. ViewModel can live longer than the screen. Holding an Activity or View can cause a memory leak.

**LiveData and StateFlow**

**Q: What is LiveData?**  
A: LiveData is an observable data holder that is aware of the Activity or Fragment lifecycle. Example: update the screen when the user list changes.

**Q: What is StateFlow?**  
A: StateFlow keeps the latest value and sends updates to collectors. Example: a ViewModel exposes `Loading`, `Success`, or `Error` screen state.

**Q: LiveData vs StateFlow?**  
A: LiveData has lifecycle awareness built in. StateFlow is part of Kotlin Coroutines and can be used in different layers; collect it in the UI with lifecycle-aware APIs.

**Q: StateFlow vs SharedFlow?**  
A: StateFlow is for current state and always has a value. SharedFlow can send events or values to multiple listeners.

**Repository**

**Q: What is the Repository pattern?**  
A: Repository gives the ViewModel one place to get or save data. It hides whether data comes from an API, database, or cache.

**Q: Why use a Repository?**  
A: It keeps data code out of the ViewModel and makes testing easier. Example: the ViewModel calls `getProducts()` without knowing how products are loaded.

**Q: What is a single source of truth?**  
A: It means one place owns the latest data. Example: the database is the main source for the product list, and the UI observes it.

---

## 6. Kotlin for Android

**Q: What is the difference between val and var?**  
A: `val` cannot be assigned a new value after initialization. `var` can. Example: use `val userId = 10` when the ID should not change.

**Q: lateinit vs lazy?**  
A: `lateinit` lets you set a `var` later. `lazy` creates a `val` only when it is first used. Example: use `lazy` for an object that is expensive to create.

**Q: What is a data class?**  
A: A data class is made to hold data. Kotlin generates useful methods like `toString()`, `equals()`, and `copy()`. Example: `data class User(val name: String, val age: Int)`.

**Q: What is a sealed class?**  
A: It represents a fixed set of possible types. Example: a screen result can be `Loading`, `Success(data)`, or `Error(message)`.

**Q: What is an extension function?**  
A: It lets you call a new function on an existing type without changing that type. Example: add `String.isValidEmail()`.

**Q: What is a higher-order function?**  
A: It takes another function as an input or returns a function. Example: `list.filter { it.age > 18 }` takes a condition function.

**Q: What is a lambda?**  
A: A lambda is a small function written without a function name. Example: `{ name -> println(name) }`.

**Q: What is a nullable type?**  
A: It is a type that can also contain `null`. Example: `String?` can contain a name or null.

**Q: What are safe call and Elvis operators?**  
A: `?.` calls something only if the value is not null. `?:` gives a default value. Example: `name?.length ?: 0`.

**Q: What is an inline function?**  
A: It asks the compiler to place the function body at the call site. It can reduce overhead when functions take lambdas.

**Q: What is reified?**  
A: It lets an inline generic function access its type at runtime. Example: a generic helper can check `value is T`.

**Q: == vs ===?**  
A: `==` checks whether values are equal. `===` checks whether both variables point to the exact same object.

---

## 7. Coroutines and Flow

**Q: What is a Coroutine?**  
A: A coroutine runs asynchronous work without blocking a thread. Example: load products from an API while the user can still scroll the screen.

**Q: What is suspend?**  
A: A `suspend` function can pause and continue later without blocking its thread. Example: a suspend API function waits for a response without freezing the UI.

**Q: launch vs async?**  
A: `launch` starts work when you do not need a returned result. `async` returns a result through `await()`. Example: use `async` to load two independent values in parallel.

**Q: What is CoroutineScope?**  
A: It controls how long coroutines live. Example: `viewModelScope` cancels its coroutines when the ViewModel is cleared.

**Q: What are Dispatchers.Main, IO, and Default?**  
A: Main is for UI work, IO is for network or file operations, and Default is for heavy calculations. Example: calculate a large report on Default, not Main.

**Q: What is structured concurrency?**  
A: Child coroutines belong to a parent scope, so cancellation and errors can be managed together. Example: when a screen's ViewModel is cleared, its child work is cancelled.

**Q: What is Flow?**  
A: Flow sends a series of values over time. Example: observe database changes and update the product list whenever it changes.

**Q: Flow vs StateFlow?**  
A: Flow normally starts when collected. StateFlow is already active and keeps the latest value for new collectors.

**Q: What is SharedFlow?**  
A: SharedFlow sends values to multiple collectors. Example: several parts of a screen can listen to the same app event.

**Q: What is flowOn()?**  
A: It changes the dispatcher used by the upstream Flow work. Example: run a database or mapping operation on an appropriate background dispatcher.

**Q: What is collectLatest()?**  
A: If a new value arrives, it cancels the previous collection action and handles the latest one. Example: search for a word while the user is typing; older searches can be cancelled.

**Q: How do you handle coroutine errors?**  
A: Use `try/catch` around work that can fail and handle cancellation correctly. Example: if an API call fails, send an Error state to the UI.

---

## 8. Networking

**Q: What is Retrofit?**  
A: Retrofit makes API calls using Kotlin or Java interfaces. Example: define `getUsers()` and Retrofit handles the HTTP request.

**Q: What is OkHttp?**  
A: OkHttp performs the actual HTTP communication. It handles connections, requests, responses, interceptors, and caching.

**Q: Retrofit vs OkHttp?**  
A: Retrofit makes API code easier to write. OkHttp handles the network work underneath.

**Q: What is an Interceptor?**  
A: It can change or inspect requests and responses. Example: add an access token to every API request.

**Q: What is JSON serialization?**  
A: Serialization converts an object into JSON. Deserialization converts JSON into an object. Example: convert a User object into request JSON.

**Q: What are common HTTP status codes?**  
A: 200 means success, 201 created, 400 bad request, 401 not logged in or invalid authentication, 403 not allowed, 404 not found, and 500 server error.

**Q: How do you handle API errors?**  
A: Check the response code, read the error body safely, and show a useful message. Example: show “Please log in again” for an expired session.

**Q: How do you handle token expiry?**  
A: If the API returns an authentication error, try refreshing the token if the backend supports it. Retry safely once; if refresh fails, ask the user to log in again.

**Q: What is pagination?**  
A: It loads data in small parts. Example: load 20 products first, then load the next 20 when the user reaches the bottom.

**Q: What is caching?**  
A: It saves data so the app can reuse it. Example: show recently loaded products immediately instead of waiting for the API every time.

---

## 9. Dependency Injection

**Q: What is Dependency Injection?**  
A: A class receives the objects it needs instead of creating them itself. Example: a ViewModel receives a UserRepository through its constructor.

**Q: Why use DI?**  
A: It reduces tight connections between classes and makes testing easier. In tests, you can pass a fake repository.

**Q: What is Dagger?**  
A: Dagger is a tool that generates code to create and provide objects.

**Q: What is Hilt?**  
A: Hilt is built on Dagger and makes dependency injection easier in Android apps.

**Q: Hilt vs Dagger?**  
A: Hilt provides ready-made Android setup and scopes. Dagger gives more manual control but needs more configuration.

**Q: What is a Singleton scope?**  
A: It keeps one instance for the lifetime of its DI component. Example: share one Retrofit or database instance across the app.

**Q: What is constructor injection?**  
A: Dependencies are passed through the class constructor. Example: `class UserViewModel(private val repo: UserRepository)`.

---

## 10. Background Tasks

**Q: What is WorkManager?**  
A: WorkManager runs reliable background work that can wait until the right conditions are met. Example: upload pending reports when internet becomes available.

**Q: When should you use WorkManager?**  
A: Use it when work must eventually finish but does not need to start immediately. Example: sync app data in the background.

**Q: What are WorkManager constraints?**  
A: They are conditions required before work runs. Example: run a large upload only when the device has an internet connection and is charging.

**Q: OneTimeWorkRequest vs PeriodicWorkRequest?**  
A: OneTimeWorkRequest runs a task once. PeriodicWorkRequest repeats it at an allowed interval.

**Q: What is a Thread?**  
A: A Thread is a separate path of execution. Example: run a heavy calculation away from the main UI thread.

**Q: What is a Handler?**  
A: A Handler sends tasks or messages to a thread's message queue. Example: post a small UI update to the main thread.

**Q: Handler vs Coroutine?**  
A: Handler works with a thread's message queue. Coroutines make asynchronous work easier to write, cancel, and manage.

**Q: Can we update UI from a background thread?**  
A: No. Update Android views on the main thread. Example: switch to `Dispatchers.Main` before changing a TextView.

---

## 11. Clean Architecture and Design

**Q: What is Clean Architecture?**  
A: It separates UI, business rules, and data work into clear layers. Example: the payment rules should not depend directly on an Android Activity.

**Q: What are the common layers?**  
A: Presentation shows the UI, Domain contains business rules, and Data handles APIs and databases.

**Q: What is a Use Case?**  
A: A Use Case performs one business action. Example: `SubmitPaymentUseCase` validates and submits a payment.

**Q: What is SOLID?**  
A: SOLID is a group of five rules that help keep code easier to change and test.

**Q: What is Single Responsibility Principle?**  
A: A class should have one main job. Example: keep API calls in a repository, not inside an Activity that also handles UI.

**Q: What is Open/Closed Principle?**  
A: Add new behavior without changing stable existing code too much. Example: add a new payment method through an interface implementation.

**Q: What is Liskov Substitution Principle?**  
A: A child class should work wherever its parent type is expected. A child should not break the behavior callers depend on.

**Q: What is Interface Segregation Principle?**  
A: Keep interfaces small and focused. Example: do not force a read-only screen to implement update and delete methods.

**Q: What is Dependency Inversion Principle?**  
A: High-level business code should depend on interfaces, not concrete classes. Example: a payment use case depends on `PaymentRepository`, not a specific Firebase class.

**Q: What is Singleton pattern?**  
A: It provides one shared instance within a defined scope. Example: one shared database instance. Avoid using global singletons for everything.

**Q: What is Factory pattern?**  
A: It creates objects in one place. Example: a factory chooses the correct payment processor based on payment type.

**Q: What is Observer pattern?**  
A: One object sends updates to other objects listening to it. Example: the UI observes StateFlow and refreshes when screen state changes.

---

## 12. Jetpack Compose

**Q: What is Jetpack Compose?**  
A: Compose is Android's modern UI toolkit. You write UI using Kotlin functions instead of XML layouts.

**Q: XML Views vs Compose?**  
A: XML describes layouts and Views are updated with code. Compose describes the UI based on state. Example: change a state value and Compose updates the screen.

**Q: What is a Composable function?**  
A: It is a Kotlin function marked with `@Composable` that describes part of the UI. Example: `@Composable fun LoginButton()`.

**Q: What is recomposition?**  
A: Compose runs the needed UI functions again when the state they read changes. Example: changing a counter from 1 to 2 updates the displayed number.

**Q: What is remember?**  
A: It keeps a value during recomposition while that composable stays in the composition. Example: remember the current text field value.

**Q: What is rememberSaveable?**  
A: It can also restore supported UI state after activity recreation. Example: keep a typed search query after screen rotation.

**Q: What is state hoisting?**  
A: Move state to a parent and pass the value and update function to a child. Example: the parent owns a checkbox value and the checkbox reports changes.

**Q: What is LaunchedEffect?**  
A: It runs a coroutine when a composable enters the screen and can restart when its keys change. Example: load initial screen data when the user ID changes.

**Q: What is LazyColumn?**  
A: It shows a scrolling list and creates items as needed. Example: display a long list of orders efficiently.

**Q: How do you improve Compose performance?**  
A: Keep state near the UI that needs it, use stable keys in lists, and avoid heavy calculations inside composable functions. Measure before optimizing.

---

## 13. Performance

**Q: What is a memory leak?**  
A: An object stays in memory even though the app no longer needs it. Example: a singleton keeps a reference to an Activity after the user leaves that screen.

**Q: What commonly causes memory leaks?**  
A: Long-lived references to Activities or Views, unremoved listeners, and callbacks that are never cleared.

**Q: How do you find memory leaks?**  
A: Use Android Studio Memory Profiler or LeakCanary. They help show which objects are still being held in memory.

**Q: What is ANR?**  
A: ANR means Application Not Responding. It happens when the main thread is blocked for too long.

**Q: What causes ANR?**  
A: Heavy work on the main thread, slow database calls, long network operations, or waiting on locks.

**Q: How do you prevent ANR?**  
A: Keep the main thread free for UI work. Move slow operations to background dispatchers and avoid long blocking calls.

**Q: What is overdraw?**  
A: It means the same pixel is drawn multiple times in one frame. Too much overdraw can make the UI slower.

**Q: How do you improve app startup time?**  
A: Do only important work at startup and delay the rest. Measure startup time before and after changes.

**Q: How do you improve RecyclerView performance?**  
A: Use efficient row layouts, DiffUtil, and avoid unnecessary updates. Example: update only the changed product row instead of refreshing the full list.

**Q: Which tools help check performance?**  
A: Android Studio Profiler, Layout Inspector, Perfetto, and Macrobenchmark help find memory, CPU, UI, and startup issues.

---

## 14. Security and Storage

**Q: SharedPreferences vs DataStore?**  
A: SharedPreferences stores simple key-value data. DataStore is a newer asynchronous option that works well with Coroutines and Flow.

**Q: What is Preferences DataStore?**  
A: It stores key-value settings. Example: save the selected theme or language.

**Q: What is Room?**  
A: Room is a library for storing structured data in a local SQLite database. Example: save products so they can be shown offline.

**Q: Where should authentication tokens be stored?**  
A: Do not store tokens as plain text. Use a secure storage design, with encryption keys protected by Android Keystore where appropriate.

**Q: What is Android Keystore?**  
A: It securely manages cryptographic keys. Example: an app can use a key from Keystore to encrypt or decrypt sensitive data.

**Q: What is encryption at rest?**  
A: It means data is encrypted while saved on the device. If someone gets access to the storage, the data is harder to read.

**Q: Can we safely hide an API secret inside an APK?**  
A: No. APK files can be inspected. Keep real secrets on a backend and restrict public API keys where possible.

**Q: What is certificate pinning?**  
A: The app checks that the server certificate or public key matches an expected value. It adds protection but needs a plan for certificate changes.

**Q: Is R8 a security tool?**  
A: R8 can make code harder to read by shrinking and obfuscating it, but it does not protect secrets. Never rely on obfuscation alone for security.

---

## 15. Advanced Android

**Q: What is a Notification?**  
A: A notification shows information outside the app screen. Example: a shopping app shows “Your order has been shipped.”

**Q: What is a Notification Channel?**  
A: It groups notifications so users can control them. Example: users can turn off promotional notifications but keep order updates.

**Q: What is PendingIntent?**  
A: It lets Android or another app perform an action in your app later. Example: tapping a notification opens a particular order screen.

**Q: PendingIntent vs Intent?**  
A: Intent describes an action now. PendingIntent gives permission to perform that action later.

**Q: FLAG_IMMUTABLE vs FLAG_MUTABLE?**  
A: Immutable means the PendingIntent cannot be changed by the receiver. Mutable allows changes when a feature needs them; use it only when required.

**Q: What are runtime permissions?**  
A: These are permissions the user must approve while using the app. Example: ask for camera permission when the user taps “Take Photo”.

**Q: What is a deep link?**  
A: It opens a particular screen from a link. Example: a link to an invoice opens that invoice screen in the app.

**Q: Deep link vs Android App Link?**  
A: A deep link can use a custom app URL or a web URL. An Android App Link is a verified website link connected to the app.

**Q: Why does a Foreground Service show a notification?**  
A: It makes ongoing user-visible work clear. Example: show a notification while navigation or audio playback is active.

---

## 16. Testing

**Q: What is Unit Testing?**  
A: It tests one small part of the code. Example: check that a function calculates a discount correctly.

**Q: What is UI Testing?**  
A: It tests what the user sees and does. Example: tap Login and check that the Home screen opens.

**Q: Unit test vs Instrumentation test?**  
A: A unit test usually runs on the local JVM. An instrumentation test runs on an Android device or emulator.

**Q: What is JUnit?**  
A: JUnit is a tool for writing and running tests. Example: use it to check the output of a Kotlin function.

**Q: What are Mockito and MockK?**  
A: They help create fake or controlled dependencies in tests. Example: mock a repository so a ViewModel test does not call a real API.

**Q: Fake vs Mock?**  
A: A fake is a simple working replacement, such as an in-memory repository. A mock is usually used to check calls or return prepared results.

**Q: How do you test a ViewModel?**  
A: Give it a fake repository, call its function, and check the UI state it produces.

**Q: How do you test Coroutines and Flow?**  
A: Use `runTest` and test dispatchers. Collect the Flow and check the values it emits.

**Q: What should you test in a Repository?**  
A: Check successful data loading, errors, caching, and whether it chooses the correct data source.

---

## 17. Modularization and Gradle

**Q: What is modularization?**  
A: It splits a large app into smaller modules. Example: separate Login, Payment, Profile, and core networking modules.

**Q: Why use multiple modules?**  
A: It keeps code organized, allows teams to work separately, and can improve build times when modules are designed well.

**Q: What is an app module?**  
A: It builds the final Android application that users install.

**Q: What is a library module?**  
A: It contains reusable code or resources that other modules can use. Example: a common UI component library.

**Q: What is a feature module?**  
A: It contains one app feature. Example: all code related to payments can live in a payment feature module.

**Q: What is Gradle?**  
A: Gradle builds the app, manages dependencies, and runs build tasks.

**Q: implementation vs api dependency?**  
A: `implementation` keeps a dependency inside that module. `api` also exposes it to modules that depend on this module.

**Q: What is a build variant?**  
A: It is a particular version of the app created using build types and product flavors. Example: `devDebug` or `prodRelease`.

---

## 18. Build and Release

**Q: Debug vs Release build?**  
A: Debug is used during development. Release is optimized and prepared for users.

**Q: What are product flavors?**  
A: They create different app versions from the same code. Example: Dev, UAT, and Production can use different API URLs and app names.

**Q: What are build types?**  
A: They define how the app is built, commonly Debug and Release. They can have different settings such as logging and code shrinking.

**Q: What is R8?**  
A: R8 removes unused code, optimizes the app, and can change class names to reduce app size and make code harder to read.

**Q: What are ProGuard rules?**  
A: They tell the shrinker which classes or methods must be kept. Example: add keep rules if a library uses reflection.

**Q: APK vs AAB?**  
A: APK is an installable app file. AAB is the publishing file uploaded to Google Play, which creates suitable APKs for devices.

**Q: What is app signing?**  
A: It proves the app's identity. Android uses the signing certificate to check that updates come from the same app publisher.

**Q: What is CI/CD?**  
A: It automates building, testing, and delivering the app. Example: every approved code change can trigger a build and run tests.

**Q: What should an Android CI pipeline include?**  
A: Run code checks and tests, build required flavors, sign release builds securely, and upload the output for testing or release.

---

## 19. System Design

**Q: How would you design a scalable Android app?**  
A: Split the app into features and layers, use clear interfaces, and keep UI, business logic, and data code separate. This makes changes easier as the app grows.

**Q: What is offline-first architecture?**  
A: The app can show saved local data even without internet. Example: show the last loaded product list and sync updates when the network returns.

**Q: How would you sync local and server data?**  
A: Save pending changes locally, retry when the network is available, and decide how to handle conflicts. Example: sync an offline form when the user gets internet.

**Q: Why use pagination?**  
A: It loads a small amount of data at a time. Example: load 20 records per request instead of downloading 5,000 records together.

**Q: How would you handle a slow API?**  
A: Show a loading state, use cached data if available, set timeouts, and give the user a retry option.

**Q: How would you design authentication?**  
A: Keep token handling in one place, store tokens securely, refresh them safely, and let the server check permissions.

**Q: How do you avoid duplicate API calls?**  
A: Keep request handling in one layer and avoid triggering the same request repeatedly. Example: disable the Submit button while a payment request is running.

**Q: How do you handle local and server data conflicts?**  
A: Decide a clear rule with the product team. Example: the server version may win, or the app may ask the user which version to keep.

**Q: How do you keep a large app maintainable?**  
A: Use clear module ownership, coding rules, code reviews, tests, and documentation so developers can work without breaking other features.

**Q: How do you decide between a quick fix and a refactor?**  
A: Check the risk, time, user impact, and future maintenance cost. For a production issue, first make a safe fix; refactor when there is a clear benefit.

---

## 20. Interview Problem Solving

**Q: How do you approach a coding question?**  
A: First understand the input and output. Then explain your approach, check edge cases, write the code, and test it with a small example.

**Q: What is time complexity?**  
A: It explains how the time taken grows when input size grows. Example: checking every item in a list is usually O(n).

**Q: What is space complexity?**  
A: It explains how much extra memory an algorithm needs. Example: creating a second list of n items uses O(n) extra space.

**Q: Array vs List?**  
A: An array has a fixed size. A Kotlin List is a read-only collection interface, while MutableList lets you add or remove items.

**Q: List vs Set?**  
A: A List keeps order and can contain duplicates. A Set stores unique values. Example: use a Set to keep unique user IDs.

**Q: What is a HashMap?**  
A: It stores key-value pairs. Example: store a user ID as the key and the user's name as the value for quick lookup.

**Q: What is a race condition?**  
A: It happens when the result depends on which concurrent task finishes first. Example: two requests update the same account balance at the same time.

**Q: What is thread safety?**  
A: Code is thread-safe when it works correctly even if multiple threads use it at the same time.

**Q: How do you debug a production crash?**  
A: Check the crash report and stack trace, find which app version and device are affected, reproduce it, fix the cause, and add a test.

**Q: How do you explain a technical decision?**  
A: Explain the problem, options, reason for your choice, result, and any trade-off. Give a real project example.

**Q: How do you answer project-based questions?**  
A: Use STAR: Situation, Task, Action, Result. Explain what the problem was, what you had to do, what you personally changed, and what improved.
