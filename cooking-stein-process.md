
Cooking with Android OS
=====

The "cooking stein/process" for Android development. This handbook is based on my personal experience and any responsibility on your own apps I can not grantee. Think about something wealth from this article and filter out what you need in your code base.

###### Software-Dev on Android OS is still difficult as we thing, however is better than we thought. We need pattern and safe tools and general process. Avoid complex instead by simple coding. Users care only on app's features, ***NOT YOUR CODE***.

Try these, you might ease your development on Android.

- MVC programming
 - [Activity](https://developer.android.com/reference/android/app/Activity.html) as controller
 - [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html) as view
 - Models with [POJO](https://en.wikipedia.org/wiki/Plain_Old_Java_Object) organized. Don't use more than 2 collections in your class.

- Data persistence
  - [SharePreference](https://developer.android.com/reference/android/content/SharedPreferences.html)
  - [Realm](https://realm.io/) replace
    - [Realm](https://realm.io/) is easy and [POJO](https://en.wikipedia.org/wiki/Plain_Old_Java_Object) based.
    - [Realm](https://realm.io/) is cross-platform which means that iOS App, Windows App and Android App have equal data-back-end.
    - [Realm](https://realm.io/) is general more fast than SQLite.
    - [Realm](https://realm.io/) is compatible with common JSON components like [gson](https://github.com/google/gson) or [jackson](https://github.com/FasterXML/jackson).
    - [Sample](https://realm.io/docs/java/latest/)
    - [Official site](https://realm.io/).

- Internet/REST/Image
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

- Async, sync, background.
  - Small job: Use [Loader API ](https://developer.android.com/guide/components/loaders.html) in [Activity](https://developer.android.com/reference/android/app/Activity.html)  or [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html) instead [AsyncTask](https://developer.android.com/reference/android/os/AsyncTask.html).
  - When using [AsyncTask](https://developer.android.com/reference/android/os/AsyncTask.html) please call [AsyncTaskCompat](https://developer.android.com/reference/android/support/v4/os/AsyncTaskCompat.html) to fire it.
    - Use [WeakReference](https://developer.android.com/reference/java/lang/ref/WeakReference.html) for [Activity](https://developer.android.com/reference/android/app/Activity.html)  or [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html).
  - Medium job: [IntentService](https://developer.android.com/reference/android/app/IntentService.html) with [ResultReceiver](https://chromium.googlesource.com/android_tools/+/master/sdk/extras/android/support/v4/src/java/android/support/v4/os/ResultReceiver.java).
  - Best choice is [RxAndroid](https://github.com/ReactiveX/RxAndroid).
    - [AsyncTask](https://developer.android.com/reference/android/os/AsyncTask.html) will break down if component lost lifecycle, i.e [Activity](https://developer.android.com/reference/android/app/Activity.html) has been killed by system.
    - Example:
      ![1-1](/media/death-of-async-task.png)
                [1-1] Equivalent code(java) between AsyncTask and RxAndroid


- Less coupling, more events.
   - Min Listeners between equal-level components.
        - Example: [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html)  between [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html)  by using [setTargetFragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html#setTargetFragment(android.support.v4.app.Fragment, int)).
    - Don't forget to use *instance-of* to check [getTargetFragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html#getTargetFragment()).
    - Try to avoid listeners between  [Activity](https://developer.android.com/reference/android/app/Activity.html)  and  [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html). There's [standard pattern](https://developer.android.com/training/basics/fragments/communicating.html) of Google how  [Activity](https://developer.android.com/reference/android/app/Activity.html) communicates with  [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html) each-other.
    - More than **three** listeners means there's a  coupling problem between components. The best way to solve it is by using [Event-Bus](https://github.com/greenrobot/EventBus).
    - [Example with listener: SettingHeadersFragment vs. SettingContentFragment](https://github.com/XinyueZ/preference-demo/tree/master/preference-fragment-comapt/app/src/main/java/com/demo/preference/app/fragments)
    - [With event-bus: de-coupling from fragment-animation and other activity](https://github.com/XinyueZ/animsample/blob/master/app/src/main/java/com/animsample/TwoSidesFramesActivity.java#L160).
    - Problems:
    ![1-2](/media/too-many-listeners.png)
                [1-2] Too many listeners, too many dependencies on others.
    ![1-3](/media/too-many-set-listeners.png)
                [1-3] Explosion-like coding setting listeners.
- De-coupling tools:
  - Use [Event-Bus](https://github.com/greenrobot/EventBus)
    - To establish communication between components like [Activity](https://developer.android.com/reference/android/app/Activity.html),  [Fragment](https://developer.android.com/reference/android/support/v4/app/Fragment.html) or [Service](https://developer.android.com/reference/android/app/Service.html) etc. It's the relationship between "same level" things or host and guest relation.
    ![1-4](/media/EventBus-Publish-Subscribe.png)
              [1-4] EventBus architecture
    - Save listeners(class fielders), save ```setXXXXListeners```, save ```listener!=null or xxx instanceOf``` before calling ```listener.onXXXX```, save more codes.
  - Use [RxAndroid](https://github.com/ReactiveX/RxAndroid)  
    - To connect background data and foreground receivers. It's the a subordinate relationship, from back to front, from bottom to top.
    - Make channel between data-stream, lost nothing.

- Compat library
  There are so many methods and classes defined in v4-v13 libraries that can help us.
    - Use [ViewCompat](https://developer.android.com/reference/android/support/v4/view/ViewCompat.html) to create [ViewPropertyAnimator](https://developer.android.com/reference/android/view/ViewPropertyAnimator.html).
    - Use [ViewStubCompat](https://android.googlesource.com/platform/frameworks/support/+/1949ae9aeaadf52ad7bd7bb74ca5419c67ea7f65/v7/appcompat/src/android/support/v7/internal/widget/ViewStubCompat.java).
    - Use [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html) instead ListView.
      - Use [CardView](https://developer.android.com/reference/android/support/v7/widget/CardView.html) as items.
      - Define [divide(A sample DividerDecoration)](https://android.googlesource.com/platform/frameworks/support/+/refs/heads/master/v7/preference/src/android/support/v7/preference/PreferenceFragmentCompat.java) like ListView but a little "complex".
- Robust
  - Inspect your code always!
    ![1-4](/media/code-inspect.png)
        [1-4] Code inspect menu in Android Studio
  - Avoid boxing and unboxing.
    - Integer, Double, Float, Boolean should not be parameter normally.
    - Use [Android v4 collections](https://developer.android.com/reference/android/support/v4/util/package-summary.html?hl=zh-cn).
  - Use [TextUtils](https://developer.android.com/reference/android/text/TextUtils.html) for strings-operations.
  - Use [Custom Tab](https://developer.chrome.com/multidevice/android/customtabs) instead [WebView](https://developer.android.com/reference/android/webkit/WebView.html)
    - Share runtime sessions with on device bundled browser.
    - Pre-load web-elements and content before links are opened.
    - However a compatible.
      - [Helper](https://github.com/XinyueZ/nasapic/blob/master/app/src/main/java/com/nasa/pic/customtab/CustomTabActivityHelper.java#L24) must be done for some devices that don't support "tab".
      - [Usage](https://github.com/XinyueZ/nasapic/blob/master/app/src/main/java/com/nasa/pic/app/activities/PhotoViewActivity.java#L144)
  - Limit PNGs for icons by using [VectorDrawableCompat](https://youtu.be/w45y_w4skKs?t=573) which is SVG based.
    - Use Android Studio Vector-Asset to create icons.
  - When app runs background, no-refresh on [View](https://developer.android.com/reference/android/view/View.html) although your objects might handle it with  [WeakReference](https://developer.android.com/reference/java/lang/ref/WeakReference.html).
  - Use v4, v7, v8, even "play-service" library or other "compat" libraries to
    - Start [Activity](https://developer.android.com/reference/android/app/Activity.html)
    - Start [AsyncTask](https://developer.android.com/reference/android/os/AsyncTask.html)
    - Start [Loader API ](https://developer.android.com/guide/components/loaders.html)
    - Start [ResultReceiver](https://github.com/futuresimple/android-support-v4/blob/master/src/java/android/support/v4/os/ResultReceiver.java).
    - etc.
  - No more [NineOldAndroid](https://github.com/JakeWharton/NineOldAndroids), it's time to begin even with Android 4.1 as basic or higher.
  - Use runtime-permission when you need. Don't allow permissions on splash or at app-enter-point. Try library [permission-dispatcher](https://github.com/hotchemi/PermissionsDispatcher).
  - Use [data-binding](https://www.google.de/?ion=1&espv=2#q=android%20databinding), not [Butter Knife](http://jakewharton.github.io/butterknife/), instead findViewById to get views.
  - In "callback"s should check if component "null" or not.
                 Activity activity = getActivity();
	               if(activity!=null) {
                   //Check in fragment for some listeners.
                 }

                 //Outside onViewCreated and associated called functions, all other methods must do.
                 View v = getView();
                 if(v != null) {

                 }
- Material(Updated)
  - [Android Summit 2015](https://www.youtube.com/playlist?list=PLWz5rJ2EKKc_Tt7q77qwyKRgytF1RzRx8)
  - [IO 2016 Android sessions](https://www.youtube.com/playlist?list=PLWz5rJ2EKKc8jQTUYvIfqA9lMvSGQWtte)
  - [Android Performance Patterns](https://www.youtube.com/playlist?list=PLOU2XLYxmsIKEOXh5TwZEv89aofHzNCiu)
