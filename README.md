# godot-appodeal-android-plugin
Appodeal Android SDK for Godot

API Compatible with [Godot Appodeal iOS module](https://github.com/PoqXert/godot-appodeal-ios-module) (exclude Android-only methods).

## Setup

If not already done, make sure you have enabled and successfully set up Android Custom Builds. Grab the``GodotAppodeal`` plugin binary and config from the releases page and put both into res://android/plugins. The plugin should now show up in the Android export settings, where you can enable it.

Add to ``res://android/build/build.gradle`` in ``android -> defaultConfig``:
```
multiDexEnabled true
```
By default, the plugin uses all networks and all ad types. You can use [Mediation Wizart](https://wiki.appodeal.com/en/android/2-6-5-android-sdk-integration-guide/mediation-wizard) and change dependencies in ``GodotAppodeal.gdap``.
For example:
```ini
[dependencies]

remote=["com.appodeal.ads.sdk:core:2.6.5",
"com.appodeal.ads.sdk.networks:a4g:2.6.5.2",
"com.appodeal.ads.sdk.networks:adcolony:2.6.5.4",
"com.appodeal.ads.sdk.networks:applovin:2.6.5.4",
"com.appodeal.ads.sdk.networks:appodealx:2.6.5.1",
"com.appodeal.ads.sdk.networks:chartboost:2.6.5.1",
"com.appodeal.ads.sdk.networks:facebook:2.6.5.3",
"com.appodeal.ads.sdk.networks:admob:2.6.5.2",
"com.appodeal.ads.sdk.networks:inmobi:2.6.5.1",
"com.appodeal.ads.sdk.networks:ironsource:2.6.5.3",
"com.appodeal.ads.sdk.networks:my_target:2.6.5.3",
"com.appodeal.ads.sdk.networks:ogury:2.6.5.3",
"com.appodeal.ads.sdk.networks:startapp:2.6.5.5",
"com.appodeal.ads.sdk.networks:tapjoy:2.6.5.2",
"com.appodeal.ads.sdk.networks:vast:2.6.5.1"]
```

You can add the optional permissions to AndroidManifest.xml (res://android/build) file under the manifest tag to improve ad targeting:
```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" /> <!--optional-->
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" /> <!--optional-->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" /> <!--optional-->
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" /> <!--optional-->
<uses-permission android:name="android.permission.VIBRATE" /><!--optional-->
```

### AdMob

If you use AdMob, add meta-data to AndroidManifest.xml in ``<application></application>``:
```xml
<!-- AdMob -->
        <meta-data
            android:name="com.google.android.gms.ads.APPLICATION_ID"
            android:value="YOUR_ADMOB_APP_ID"/>
```

## Usage

To use the ``GodotAppodeal`` API you first have to get the ``GodotAppodeal`` singleton:
```gdscript
var _appodeal

func _ready():
  if Engine.has_singleton("GodotAppodeal"):
    _appodeal = Engine.get_singeton("GodotAppodeal")
```
### Initialization

To initialization Appodeal SDK call ``initialize`` method:
```gdscript
func initialize(app_key: String, ad_types: int, consent: bool) -> void
```
For example:
```gdscript
_appodeal.initialize("YOU_ANDROID_APPODEAL_APP_KEY", 8, false)
```
### Ad Types

The adTypes parameter in the code is responsible for the ad formats you are going to implement into your app.
You can define enum for it:
```gdscript
enum AdType {
  INTERSTITIAL = 1,
  BANNER = 2,
  NATIVE = 4,
  REWARDED_VIDEO = 8,
  NON_SKIPPABLE_VIDEO = 16,
}
```
Ad types can be combined using "|" operator.

### Show Styles

The showStyles parameter use for show ad.
You can define enum for it:
```gdscript
enum ShowStyle {
  INTERSTITIAL = 1,
  BANNER_TOP = 2,
  BANNER_BOTTOM = 4,
  REWARDED_VIDEO = 8,
  NON_SKIPPABLE_VIDEO = 16,
}
```

### Methods

#### Initialization

```gdscript
# Initialization Appodeal SDK
func initialize(app_key: String, ad_types: int, consent: bool) -> void
```
```gdscript
# Checking initialization for ad type
func isInitializedForAdType(ad_type: int) -> bool
```

#### Display ad

```gdscript
# Display ad
func showAd(show_style: int) -> bool
```
```gdscript
# Display ad for specified placement
func showAdForPlacement(show_style: int, placement: String) -> bool
```
```gdscript
# Check ability to display ad
func canShow(show_style: int) -> bool
```
```gdscript
# Check ability to display ad for specified placement
func canShowForPlacement(ad_type: int, placement: String) -> bool
```
```gdscript
# Hide banner
func hideBanner()
```

#### Configure SDK

```gdscript
# Enable/Disable testing
func setTestingEnabled(enabled: bool) -> void
```
```gdscript
# Disable specified networks
func disableNetworks(networks: Array) -> void
```
```gdscript
# Disable specified networks for ad type
func disableNetworksForAdType(networks: Array, ad_type: int) -> void
```
```gdscript
# Disable specified network
func disableNetwork(network: String) -> void
```
```gdscript
# Disable specified network for ad type
func disableNetworkForAdType(network: String, ad_type: int) -> void
```
```gdscript
# Request Android M permissions (Android-only)
func requestAndroidMPermissions() -> void
```
```gdscript
# Disable location tracking (use before initialization).
# setLocationTracking(true) don't have effect on Android.
func setLocationTracking(enabled: bool) -> void
```
```gdscript
# Disable data collection for kids apps
func setChildDirectedTreatment(for_kids: bool) -> void
```
```gdscript
# Change GDPR consent status
func updateConsent(consent: bool) -> void
```
```gdscript
# Disable write external storage permission check (Android-only)
func disableWriteExternalStoragePermissionCheck() -> void
```
```gdscript
# Set logging
func setLogLevel(log_level: int) -> void
```
```gdscript
# Mute videos if call volume is muted (Android-only)
func muteVideosIfCallsMuted(mute: bool) -> void
```
```gdscript
# Send extra data
func setExtras(data: Dictionary) -> void
```
```gdscript
# Set segment filter
func setSegmentFilter(filter: Dictionary) -> void
```
```gdscript
# Enable/Disable smart banners
func setSmartBannersEnabled(enabled: bool) -> void
```
```gdscript
# Enable/Disable banner animation
func setBannerAnimationEnabled(enabled: bool) -> void
```
```gdscript
# Enable/Disable 728x90 banners
# Enable if 1, otherwise disable
func setPreferredBannerAdSize(size: int) -> void
```

#### Caching

```gdscript
# Enable/Disable autocache
func setAutocache(enabled: bool, ad_type: int) -> void
```
```gdscript
# Check autocache enabled
func isAutocacheEnabled(ad_type: int) -> bool
```
```gdscript
# Check cache
func isPrecacheAd(ad_type: int) -> bool
```
```gdscript
# Cache
func cacheAd(ad_type: int) -> void
```

#### User Settings

```gdscript
# Set user ID for S2S callbacks
func setUserId(user_id: String) -> void
```
```gdscript
# Set user age
func setUserAge(age: int) -> void
```
```gdscript
# Set user gender
func setUserGender(gender: int) -> void
```

#### Other

```gdscript
# Get predicted eCPM for ad type
func getPredictedEcpmForAdType(ad_type: int) -> float
```
```gdscript
# Get Reward info for placement
func getRewardForPlacement(placement: String) -> Dictionary
```
Reward Dictionary have ``currency`` and ``amount`` keys.
```gdscript
# Track in-app purchases
func trackInAppPurchase(amount: float, currency: String) -> void
```

## Signals

### Interstitial

```gdscript
# Emit when interstitial is loaded
signal interstitial_loaded(precached: bool)
```
```gdscript
# Emit when interstitial failed to load
signal interstitial_load_failed()
```
```gdscript
# Emit when interstitial is shown
signal interstitial_shown()
```
```gdscript
# Emit when interstitial show failed
signal interstitial_show_failed()
```
```gdscript
# Emit when interstitial is clicked
signal interstitial_clicked()
```
```gdscript
# Emit when interstitial is closed
signal interstitial_closed()
```
```gdscript
# Emit when interstitial is expired
signal interstitial_expired()
```

### Banner

```gdscript
# Emit when banner is loaded
signal banner_loaded(precached: bool)
```
```gdscript
# Emit when banner failed to load
signal banner_load_failed()
```
```gdscript
# Emit when banner is shown
signal banner_shown()
```
```gdscript
# Emit when banner show failed
signal banner_show_failed()
```
```gdscript
# Emit when banner is clicked
signal banner_clicked()
```
```gdscript
# Emit when banner is expired
signal banner_expired()
```

### Rewarded Video

```gdscript
# Emit when rewarded video is loaded
signal rewarded_video_loaded(precache: bool)
```
```gdscript
# Emit when rewarded video failed to load
signal rewarded_video_load_failed()
```
```gdscript
# Emit when rewarded video is shown
signal rewarded_video_shown()
```
```gdscript
# Emit when rewarded video show failed
signal rewarded_video_show_failed()
```
```gdscript
# Emit when rewarded video is viewed until the end
signal rewarded_video_finished(amount: float, currency: String)
```
```gdscript
# Emit when rewarded video is closed
signal rewarded_video_closed(finished: bool)
```
```gdscript
# Emit when rewarded video is expired
signal rewarded_video_expired()
```
```gdscript
# Emit when rewarded video is clicked
signal rewarded_video_clicked()
```

### Non-Skippable Video

```gdscript
# Emit when non-skippable video is loaded
signal non_skippable_video_loaded(precache: bool)
```
```gdscript
# Emit when non-skippable video failed to load
signal non_skippable_video_load_failed()
```
```gdscript
# Emit when non-skippable video is shown
signal non_skippable_video_shown()
```
```gdscript
# Emit when non-skippable video show failed
signal non_skippable_video_show_failed()
```
```gdscript
# Emit when non-skippable video is viewed until the end
signal non_skippable_video_finished()
```
```gdscript
# Emit when non-skippable video is closed
signal non_skippable_video_closed(finished: bool)
```
```gdscript
# Emit when non-skippable video is expired
signal non_skippable_video_expired()
```
