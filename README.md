LifecycleDispose
=====

`LifecycleDispose` dispose RxJava streams on lifecycle down event that corresponding to Activity/Fragment's lifecycle state when subscribe using [Android Architecture Components Lifecycle](https://developer.android.com/topic/libraries/architecture/lifecycle).

![Lifecycle State](https://developer.android.com/images/topic/libraries/architecture/lifecycle-states.svg)

## Usage
### disposeOnPause
Dispose when onPause is called.

```kotlin
Observable.interval(1, TimeUnit.SECONDS)
    .subscribe()
    .disposeOnPause(this) // `this` is FragmentActivity or Fragment
```

### disposeOnStop
Dispose when onStop is called.

```kotlin
Observable.interval(1, TimeUnit.SECONDS)
    .subscribe()
    .disposeOnStop(this) // `this` is FragmentActivity or Fragment
```

### disposeOnDestroy
Dispose when onDestroy is called.
If Fragment has view, dispose when onDestroyView is called.

```kotlin
Observable.interval(1, TimeUnit.SECONDS)
    .subscribe()
    .disposeOnDestroy(this) // `this` is FragmentActivity or Fragment
```

### disposeOnLifecycle
Dispose on lifecycle down event that corresponding to Activity/Fragment's lifecycle state. See next section.

```kotlin
Observable.interval(1, TimeUnit.SECONDS)
    .subscribe()
    .disposeOnLifecycle(this) // `this` is FragmentActivity or Fragment
```

## Lifecycle down event that corresponding to Activity/Fragment's lifecycle state
### Activity

**Table 1** Corresponding between Activity's lifecycle state and Lifecycle down event.

| Subscribe | Lifecycle.State | Lifecycle.Event | Dispose   |
| --------- | --------------- | --------------- | --------- |
| onCreate  | INITIALIZED     | ON_DESTROY      | onDestroy |
| onStart   | CREATED         | ON_STOP         | onStop    |
| onResume  | STARTED         | ON_PAUSE        | onPause   |
| onPause   | STARTED         | ON_DESTROY      | onDestroy |
| onStop    | CREATED         | ON_DESTROY      | onDestroy |
| onDestroy | DESTROYED       | ON_DESTROY      | onDestroy |


### Fragment

**Table 2** Corresponding between Fragment's lifecycle state and Lifecycle down event.

 | Subscribe     | ViewLifecycle.State   | Dispose       | Lifecycle.State | Dispose       |
 | ------------- | --------------------- | ------------- | --------------- | ------------- |
 | onAttach      | IllegalStateException | onDestroy     | INITIALIZED     | onDestroy     |
 | onCreate      | IllegalStateException | onDestroy     | INITIALIZED     | onDestroy     |
 | onCreateView  | IllegalStateException | onDestroyView | CREATED         | onDestroy     |
 | onViewCreated | INITIALIZED           | onDestroyView | not called      | not called    |
 | onStart       | CREATED               | onStop        | CREATED         | onStop        |
 | onResume      | STARTED               | onPause       | STARTED         | onPause       |
 | onPause       | STARTED               | onDestroyView | STARTED         | onDestroy     |
 | onStop        | CREATED               | onDestroyView | CREATED         | onDestroy     |
 | onDestroyView | DESTROYED             | onDestroyView | not called      | not called    |
 | onDestroy     | IllegalStateException | onDestroy     | DESTROYED       | onDestroy     |

## Gradle

[![](https://jitpack.io/v/wada811/LifecycleDispose.svg)](https://jitpack.io/#wada811/LifecycleDispose)

```groovy
repositories {
    maven { url "https://www.jitpack.io" }
}

dependencies {
    implementation 'com.github.wada811:LifecycleDispose:x.y.z'
}
```

## License

Copyright (C) 2019 wada811

Licensed under the Apache License, Version 2.0
