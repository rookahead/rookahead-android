# Rookahead for Android

The Play Store app for [Rookahead](https://rookahead.com). It's a Trusted Web Activity: a thin Android
wrapper that opens rookahead.com full screen in Chrome's engine. The app itself lives in
[rookahead/rookahead](https://github.com/rookahead/rookahead), so a change there reaches the Android app at
once with no new release. A new release is only needed to change the wrapper (name, icon, colours,
Android version).

Generated with [Bubblewrap](https://github.com/GoogleChromeLabs/bubblewrap) 1.27 from the site's web
manifest. Package `com.rookahead.app`, target SDK 36, min SDK 24 (Android 7.0).

## Building

Needs JDK 17 and the Android SDK (`~/.bubblewrap/config.json` points Bubblewrap at both).

```sh
export BUBBLEWRAP_KEYSTORE_PASSWORD=…  BUBBLEWRAP_KEY_PASSWORD=…
yes n | bubblewrap build --skipPwaValidation
```

That writes `app-release-bundle.aab` (upload this to Play) and `app-release-signed.apk` (install it on a
phone to test). Before each new release, raise `appVersionCode` and `appVersionName` in
`twa-manifest.json` and run `bubblewrap update`.

## The signing key

The upload key is **not** in this repo. It lives in `../rookahead-android-keys/` (`upload.keystore` and
`passwords.txt`) and must be backed up somewhere safe: every update has to be signed with it. With Play
App Signing, Google holds the key that signs what users install; if the upload key is lost, Play support
can reset it, but that takes days.

Upload key SHA-256:

    EE:14:A3:9E:94:A3:BA:28:DA:65:08:B6:26:65:2F:F0:C6:B2:82:B0:D7:C7:A8:70:3C:46:05:0E:B5:D0:7A:0E

## Digital Asset Links

Android only opens the site without a browser bar when rookahead.com vouches for the app, in
`https://rookahead.com/.well-known/assetlinks.json` (in the main repo, `public/.well-known/`). It lists
the SHA-256 of every key that signs the app: the upload key above (for APKs installed by hand), and
Google Play's **app signing key** (what Play installs; Play Console → Test and release → App integrity):

    7E:E7:F7:24:6C:8D:92:2C:68:CE:FC:C6:02:68:0C:9C:12:2A:9C:22:1A:D7:1D:B3:50:DC:0F:72:5B:A9:6E:80

If either is missing from assetlinks.json, that version of the app opens with a browser bar.

## Store listing

`store/listing.md` has the listing text, content rating and data safety answers; the icon, feature graphic
and screenshots are beside it.
