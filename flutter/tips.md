### Flutter tips

**startup time measuring :**

```text
flutter run --trace-startup --profile
```

**show performance overlay**

```dart
void main() {
  return runApp(
    GetMaterialApp(showPerformanceOverlay: true),
  );
}
```

**Launcher icon set in flutter**

`Note: icons size 512x512 or specify size becacuse image is scale`

```yaml
flutter_launcher_icons: ^0.13.1
```

```yaml
flutter_launcher_icons:
  android: "launcher_icon"
  image_path: "assets/logo/logo.png"
```

```text
flutter pub get
```

```text
flutter pub run flutter_launcher_icons 
```

**Splash screen icon set in flutter**

```yaml
flutter_native_splash: ^2.3.1
```

```yaml
flutter_native_splash:
  color: "#FFFFFF"
  image: "assets/logo/launcher_icon.png"
```

```text
flutter pub get
```

```text
dart run flutter_native_splash:create
```

**File picker**

```yaml
file_picker: ^5.3.3
```

**Datetime formatter**

```yaml
intl: ^0.18.1
```

```dart
import 'package:intl/intl.dart';

DateTime now = DateTime.now();
String formattedDate = DateFormat('yyyy-MM-dd – kk:mm').format(now);
```

**Flutter rename**

```text
flutter pub add rename
```

```text
flutter pub global activate-    rename
```

```text
flutter pub global run rename --bundleId com.onatcipli.networkUpp
flutter pub global run rename --appname "Network Upp"
```

or

```text
flutter pub global run rename --appname (-a) yourappname --target (-t) [android, ios, web, windows, macOS, linux]          
```

**Flutter share text**

```yaml
flutter_share: ^2.0.0
```

```dart
// share app
static Future<void> share() async {
    await FlutterShare.share(
      title: 'Share New Experiance',
      text: 'Exciting News! Our App is now live.',
      linkUrl: Constant.appLink,
      chooserTitle: 'Have a nice experiance',
    );
}
```

**Flutter inapp rating**

```yaml
in_app_review: ^2.0.8
```

```dart
static Future<void> review() async {
    final InAppReview inAppReview = InAppReview.instance;

    if (await inAppReview.isAvailable()) {
      inAppReview.requestReview();
    }
  }
```

**Flutter html preview**

```yaml
flutter_html: ^3.0.0-beta.2
```

```dart
const String htmlData = '''
<h1>Heading 001</h1>
<h2>Heading 002</h2>
<p>Paragraph 001</p>
'''
Html(data: htmlData)
```

**Flutter URL launcher**

```yaml
url_launcher: ^6.2.1
```

```dart
static Future<void> launch(String url) async {
    final Uri uri = Uri.parse(url);
    if (!await launchUrl(uri)) {
      throw Exception('Could not launch $uri');
    }
}
launch('http://www.github.com');
```

**SVG Image in flutter**

```yaml
flutter_svg: ^2.0.9
```

```dart
SvgPicture.asset(
    "assets/svgs/whatsapp.svg",
    colorFilter: ColorFilter.mode(
        Theme.of(context).cardColor,
        BlendMode.srcIn,
    ),
)
```