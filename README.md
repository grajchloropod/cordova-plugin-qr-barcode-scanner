# Not maintained

This plugin is not maintained anymore.

Please use the original plugin [phonegap/phonegap-plugin-barcodescanner](https://github.com/phonegap/phonegap-plugin-barcodescanner).

Or, better, try this plugin [bitpay/cordova-plugin-qrscanner](https://github.com/bitpay/cordova-plugin-qrscanner).

================================


# PhoneGap Plugin BarcodeScanner2
================================

Cross-platform BarcodeScanner for Cordova / PhoneGap.

This is a fork of [phonegap/phonegap-plugin-barcodescanner](https://github.com/phonegap/phonegap-plugin-barcodescanner) with the only difference being the design of the scaning interface.

## Changes in this fork (Android)

- Modern UPI-style scanner UI
  - Corner-only frame with rounded green bars (centered overlay)
  - Status text repositioned above the scan frame
  - Top bar with Back (left), Flip Camera and Torch (right)
- Theme color updates
  - Laser and corner bars use `#2ABA66`
- Icon updates
  - Density-specific icons added for crisp 24dp rendering
- Back button support
  - Back button added to the layout; click handling implemented in the Android AAR (`CaptureActivity`) to cancel and close the scanner
- Gradle maintenance
  - Replaced `jcenter()` with `mavenCentral()` in `src/android/barcodescanner.gradle`

Notes
- Visual/layout changes live in `src/android/barcodescanner-release-2.1.5/res/...` inside this plugin.
- Back button functionality requires a small code change in the AAR (`CaptureActivity`). The prebuilt AAR included in this repo contains that change.

## Customize it yourself

To customize the design layout for the scanning interface, edit this file: `src/android/barcodescanner-release-2.1.5/res/layout/capture.xml`, then run this commands:

    cd src/android/barcodescanner-release-2.1.5
    zip -r ../barcodescanner-release-2.1.5.aar *

The button icons are in the directory `src/android/barcodescanner-release-2.1.5/res/drawable`.

After you change an icon, or anything else in the folder `src/android/barcodescanner-release-2.1.5`, you need to run the previous commands as well.


### Rebuilding the Android AAR on Windows (PowerShell)

If you only changed resources (layouts, drawables, colors) in `src/android/barcodescanner-release-2.1.5`:

```
cd src/android/barcodescanner-release-2.1.5
Remove-Item ..\barcodescanner-release-2.1.5.aar -ErrorAction SilentlyContinue
Compress-Archive -Path * -DestinationPath ..\barcodescanner-release-2.1.5.zip -Force
Move-Item ..\barcodescanner-release-2.1.5.zip ..\barcodescanner-release-2.1.5.aar -Force
```

Then re-add the plugin to your app:

```
cordova plugin remove cordova-plugin-qr-barcode-scanner
cordova plugin add <path-or-repo-url>
cordova run android --device
```

If you changed Java code in the AAR (e.g., back button handling in `CaptureActivity`), build from AAR sources with JDK 8:

1. Set Java 8 in your shell
   - PowerShell:
     ```
     $env:JAVA_HOME="C:\\Program Files\\Eclipse Adoptium\\jdk-8.0.x-hotspot"
     $env:Path="$env:JAVA_HOME\\bin;$env:Path"
     java -version
     ```
2. From the AAR source project root (barcodescanner-lib-aar):
   ```
   .\gradlew.bat clean :barcodescanner:assembleRelease --no-daemon
   ```
3. Copy the output AAR to this plugin as `src/android/barcodescanner-release-2.1.5.aar` (or update `plugin.xml` to match your filename) and re-add the plugin.

Tip: If Gradle complains about `sdk.dir`, create/update `local.properties` in the AAR source root:

```
sdk.dir=C:/Users/<YOUR_USER>/AppData/Local/Android/Sdk
```



Follows the [Cordova Plugin spec](https://cordova.apache.org/docs/en/latest/plugin_ref/spec.html), so that it works with [Plugman](https://github.com/apache/cordova-plugman).

## Installation

Installation with cordova cli:

    cordova plugin add cordova-plugin-qr-barcode-scanner

This requires phonegap 7.1.0+ ( current stable v8.0.0 )

    phonegap plugin add cordova-plugin-qr-barcode-scanner

It is also possible to install via repo url directly ( unstable )

    phonegap plugin add cordova-plugin-qr-barcode-scanner

Optional variables:
This plugin requires the Android support library v4. The minimum version is `24.1.0`. Default value is `27.+`.  Check out the latest version [here](https://developer.android.com/topic/libraries/support-library/revisions.html).
```
phonegap plugin add cordova-plugin-qr-barcode-scanner --variable ANDROID_SUPPORT_V4_VERSION="27.1.1"
```
### Supported Platforms

- Android
- iOS
- Windows (Windows/Windows Phone 8.1 and Windows 10)
- Browser

Note: the Android source for this project includes an Android Library Project.
plugman currently doesn't support Library Project refs, so its been
prebuilt as a jar library. Any updates to the Library Project should be
committed with an updated jar.

Note: Windows 10 applications can not be build for `AnyCPU` architecture, which is default for Windows platform. If you want to build/run Windows 10 app, you should specify target architecture explicitly, for example (Cordova CLI):

```
cordova run windows -- --archs=x86
```

### PhoneGap Build Usage

Add the following to your config.xml:

```
<!-- add a version here, otherwise PGB will use whatever the latest version of the package on npm is -->
<plugin name="cordova-plugin-qr-barcode-scanner" />
```
On PhoneGap Build if you're using a version of cordova-android of 4 or less, ensure you're building with gradle:
```
<preference name="android-build-tool" value="gradle" />
```

## Using the plugin ##
The plugin creates the object `cordova.plugins.barcodeScanner` with the method `scan(success, fail)`.

The following barcode types are currently supported:

|  Barcode Type | Android | iOS | Windows  |
|---------------|:-------:|:---:|:--------:|
| QR_CODE       |    ✔    |  ✔  |     ✔    |
| DATA_MATRIX   |    ✔    |  ✔  |     ✔    |
| UPC_A         |    ✔    |  ✔  |     ✔    |
| UPC_E         |    ✔    |  ✔  |     ✔    |
| EAN_8         |    ✔    |  ✔  |     ✔    |
| EAN_13        |    ✔    |  ✔  |     ✔    |
| CODE_39       |    ✔    |  ✔  |     ✔    |
| CODE_93       |    ✔    |  ✔  |     ✔    |
| CODE_128      |    ✔    |  ✔  |     ✔    |
| CODABAR       |    ✔    |  ✖  |     ✔    |
| ITF           |    ✔    |  ✔  |     ✔    |
| RSS14         |    ✔    |  ✖  |     ✔    |
| PDF_417       |    ✔    |  ✔  |     ✔    |
| RSS_EXPANDED  |    ✔    |  ✖  |     ✖    |
| MSI           |    ✖    |  ✖  |     ✔    |
| AZTEC         |    ✔    |  ✔  |     ✔    |

`success` and `fail` are callback functions. Success is passed an object with data, type and cancelled properties. Data is the text representation of the barcode data, type is the type of barcode detected and cancelled is whether or not the user cancelled the scan.

A full example could be:
```js
   cordova.plugins.barcodeScanner.scan(
      function (result) {
          alert("We got a barcode\n" +
                "Result: " + result.text + "\n" +
                "Format: " + result.format + "\n" +
                "Cancelled: " + result.cancelled);
      },
      function (error) {
          alert("Scanning failed: " + error);
      },
      {
          preferFrontCamera : true, // iOS and Android
          showFlipCameraButton : true, // iOS and Android
          showTorchButton : true, // iOS and Android
          torchOn: true, // Android, launch with the torch switched on (if available)
          saveHistory: true, // Android, save scan history (default false)
          prompt : "Place a barcode inside the scan area", // Android
          resultDisplayDuration: 500, // Android, display scanned text for X ms. 0 suppresses it entirely, default 1500
          formats : "QR_CODE,PDF_417", // default: all but PDF_417 and RSS_EXPANDED
          orientation : "landscape", // Android only (portrait|landscape), default unset so it rotates with the device
          disableAnimations : true, // iOS
          disableSuccessBeep: false // iOS and Android
      }
   );
```

## Encoding a Barcode ##

The plugin creates the object `cordova.plugins.barcodeScanner` with the method `encode(type, data, success, fail)`.

Supported encoding types:

* TEXT_TYPE
* EMAIL_TYPE
* PHONE_TYPE
* SMS_TYPE

```
A full example could be:

   cordova.plugins.barcodeScanner.encode(cordova.plugins.barcodeScanner.Encode.TEXT_TYPE, "http://www.nytimes.com", function(success) {
            alert("encode success: " + success);
          }, function(fail) {
            alert("encoding failed: " + fail);
          }
        );
```

## iOS quirks ##

Since iOS 10 it's mandatory to add a `NSCameraUsageDescription` in the `Info.plist`.

`NSCameraUsageDescription` describes the reason that the app accesses the user's camera.
When the system prompts the user to allow access, this string is displayed as part of the dialog box. If you didn't provide the usage description, the app will crash before showing the dialog. Also, Apple will reject apps that access private data but don't provide an usage description.

To add this entry you can use the `edit-config` tag in the `config.xml` like this:

```
<edit-config target="NSCameraUsageDescription" file="*-Info.plist" mode="merge">
    <string>To scan barcodes</string>
</edit-config>
```

## Windows quirks ##

* Windows implementation currently doesn't support encode functionality.

* On Windows 10 desktop ensure that you have Windows Media Player and Media Feature pack installed.

## Thanks on Github ##

So many -- check out the original [iOS](https://github.com/phonegap/phonegap-plugins/tree/DEPRECATED/iOS/BarcodeScanner),  [Android](https://github.com/phonegap/phonegap-plugins/tree/DEPRECATED/Android/BarcodeScanner) and
[BlackBerry 10](https://github.com/blackberry/WebWorks-Community-APIs/tree/master/BB10-Cordova/BarcodeScanner) repos.

## Licence ##

The MIT License

Copyright (c) 2010 Matt Kane

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
