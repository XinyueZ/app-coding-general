
Coding on Android
=====

General coding style for Android development. This handbook is based on my personal experience and any responsibility on your own apps I can not grantee. Think about something wealth from this article and filter out what you need in your code.

- MVC programming
 - [Activity](https://developer.android.com/reference/android/app/Activity.html) as controller
 - [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html) as view
 - Models with [POJO](https://en.wikipedia.org/wiki/Plain_Old_Java_Object) organized.

- Data persistence
  - [SharePreference](https://developer.android.com/reference/android/content/SharedPreferences.html)
  - [Realm](https://realm.io/) replace
    - [Realm](https://realm.io/) is easy and [POJO](https://en.wikipedia.org/wiki/Plain_Old_Java_Object) based.
    - [Realm](https://realm.io/) is cross-platform which means that iOS App, Windows App and Android App have equal data-back-end.
    - [Realm](https://realm.io/) is general more fast than SQLite.
    - [Realm](https://realm.io/) is compatible with common JSON components like [gson](https://github.com/google/gson) or [jackson](https://github.com/FasterXML/jackson).
    - [Sample](https://realm.io/docs/java/latest/)
    - [Official site](https://realm.io/).

- Internet/REST
  - [gson](https://github.com/google/gson) more, less [jackson](https://github.com/FasterXML/jackson).
    - The [jackson](https://github.com/FasterXML/jackson) has too much more dependencies that overhead our build.gradle.
  - OK-Stack of [Square Open Source](http://square.github.io/#android) instead [Volley](https://developer.android.com/training/volley/index.html)
    -  [Volley](https://developer.android.com/training/volley/index.html) overheads coding when there's amount of requests to define.
    -  [Volley](https://developer.android.com/training/volley/index.html) brings too much more customized options like Image-loader, request-headers.
    - Example of [Volley](https://github.com/XinyueZ/minNews/tree/master/src/com/gmail/hasszhao/mininews/tasks).
    - [OK HTTP](http://square.github.io/okhttp/) as network-component.
      - Easier than standard HttpClient and HttpConnection.
      - [Simple and less coding](http://square.github.io/okhttp/).
    - [Retrofit2](http://square.github.io/retrofit/) as REST-component.
      - Very compatible with [Realm](https://realm.io/).
      - Very compatible with [gson](https://github.com/google/gson).
      - [Simple and less coding](http://square.github.io/retrofit/).
  - [Glide](https://www.google.de/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwjPstuAgfHMAhWIbRQKHXODDygQFggdMAA&url=https%3A%2F%2Fgithub.com%2Fbumptech%2Fglide&usg=AFQjCNHZ_1a6kVBhhXyyVxpVvJaRrbqnZQ&sig2=gFc6rzMJv0ppUEmz2fKvAA) as "Image-Loader" .

- Async-Task
  - Use [Loader API ](https://developer.android.com/guide/components/loaders.html) in [Activity](https://developer.android.com/reference/android/app/Activity.html)  or [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html) instead [AsyncTask](https://developer.android.com/reference/android/os/AsyncTask.html).
  - When using [AsyncTask](https://developer.android.com/reference/android/os/AsyncTask.html) please call [AsyncTaskCompat](https://developer.android.com/reference/android/support/v4/os/AsyncTaskCompat.html) to fire it.
  - Using [WeakReference](https://developer.android.com/reference/java/lang/ref/WeakReference.html) for [Activity](https://developer.android.com/reference/android/app/Activity.html)  or [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html).

- App robust
  - Less coupling, more events.
   - Listeners between equal-level components.
        - Example: [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html)  between [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html)  by using [setTargetFragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html#setTargetFragment(android.support.v4.app.Fragment, int)).
    - Don't forget to use *instance-of* to check [getTargetFragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html#getTargetFragment()).
    - Try to avoid listeners between  [Activity](https://developer.android.com/reference/android/app/Activity.html)  and  [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html). There's [standard pattern](https://developer.android.com/training/basics/fragments/communicating.html) of Google how  [Activity](https://developer.android.com/reference/android/app/Activity.html) communicates with  [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html) each-other.
  - More than **three** listeners means there's a  coupling problem between components. The best way to solve it is by using [Event-Bus](https://github.com/greenrobot/EventBus).
  - When app runs background, no-refresh on [View](https://developer.android.com/reference/android/view/View.html) although your objects might handle it with  [WeakReference](https://developer.android.com/reference/java/lang/ref/WeakReference.html).
  - Using v4, v7, v8, even "play-service" library or other "compat" libraries to
    - Start [Activity](https://developer.android.com/reference/android/app/Activity.html)
    - Start [AsyncTask](https://developer.android.com/reference/android/os/AsyncTask.html)
    - Start [Loader API ](https://developer.android.com/guide/components/loaders.html)
    - Start [ResultReceiver](https://github.com/futuresimple/android-support-v4/blob/master/src/java/android/support/v4/os/ResultReceiver.java).
    - etc.
  - No more [NineOldAndroid](https://github.com/JakeWharton/NineOldAndroids), it's time to begin even with Android 4.1 as basic or more.
