# تعليمات البناء للمطورين
## Smart Access App - Build Instructions

### نظرة عامة
هذا الدليل مخصص للمطورين الذين يريدون بناء التطبيق من الكود المصدري أو المساهمة في تطويره.

---

## متطلبات التطوير

### البرامج المطلوبة
- **Flutter SDK**: 3.16.9 أو أحدث
- **Dart SDK**: 3.2.0 أو أحدث
- **Android Studio**: 2023.1 أو أحدث
- **Xcode**: 15.0 أو أحدث (لتطوير iOS على macOS)
- **Git**: لإدارة الإصدارات

### أدوات إضافية
- **VS Code** أو **Android Studio** مع إضافات Flutter
- **Chrome** للتطوير على الويب
- **Android Emulator** أو جهاز Android فعلي
- **iOS Simulator** أو جهاز iOS فعلي (macOS فقط)

---

## إعداد بيئة التطوير

### 1. تثبيت Flutter
```bash
# تحقق من تثبيت Flutter
flutter doctor -v

# إذا لم يكن مثبتاً، اتبع التعليمات في:
# https://flutter.dev/docs/get-started/install
```

### 2. إعداد المحرر

#### VS Code
```bash
# تثبيت إضافات Flutter و Dart
code --install-extension Dart-Code.flutter
code --install-extension Dart-Code.dart-code
```

#### Android Studio
1. افتح Android Studio
2. اذهب إلى **File** > **Settings** > **Plugins**
3. ابحث عن "Flutter" وثبته
4. أعد تشغيل Android Studio

### 3. إعداد الأجهزة

#### Android
```bash
# تحقق من الأجهزة المتصلة
flutter devices

# إنشاء محاكي Android (إذا لم يكن موجوداً)
flutter emulators --create --name test_emulator

# تشغيل المحاكي
flutter emulators --launch test_emulator
```

#### iOS (macOS فقط)
```bash
# تحقق من محاكيات iOS المتاحة
xcrun simctl list devices

# تشغيل محاكي iOS
open -a Simulator
```

---

## بناء المشروع

### 1. استنساخ المشروع
```bash
git clone <repository-url>
cd smart_access_app
```

### 2. تثبيت التبعيات
```bash
# تثبيت حزم Flutter
flutter pub get

# تحديث التبعيات (إذا لزم الأمر)
flutter pub upgrade

# تنظيف المشروع
flutter clean
flutter pub get
```

### 3. إنشاء الملفات المطلوبة

#### ملفات التكوين
```bash
# إنشاء ملفات التكوين للمنصات المختلفة
flutter create --platforms=android,ios .
```

#### مفاتيح API (إذا لزم الأمر)
قم بإنشاء ملف `lib/config/api_keys.dart`:
```dart
class ApiKeys {
  static const String openaiApiKey = 'your-openai-api-key';
  static const String googleMapsApiKey = 'your-google-maps-api-key';
  // أضف مفاتيح API الأخرى هنا
}
```

### 4. التحقق من المشروع
```bash
# تحليل الكود للتأكد من عدم وجود أخطاء
flutter analyze

# تشغيل الاختبارات
flutter test

# تحقق من التنسيق
dart format --set-exit-if-changed .
```

---

## أوضاع البناء

### وضع التطوير (Debug)
```bash
# تشغيل في وضع التطوير
flutter run

# تشغيل مع إعادة التحميل السريع
flutter run --hot-reload

# تشغيل مع إعادة التشغيل السريع
flutter run --hot-restart
```

### وضع الملف الشخصي (Profile)
```bash
# بناء في وضع الملف الشخصي (لقياس الأداء)
flutter run --profile

# بناء APK في وضع الملف الشخصي
flutter build apk --profile
```

### وضع الإنتاج (Release)
```bash
# تشغيل في وضع الإنتاج
flutter run --release

# بناء APK للإنتاج
flutter build apk --release

# بناء App Bundle للإنتاج
flutter build appbundle --release
```

---

## بناء للمنصات المختلفة

### Android

#### APK عادي
```bash
# بناء APK شامل
flutter build apk --release

# بناء APK مقسم حسب البنية (أصغر حجماً)
flutter build apk --split-per-abi --release

# بناء APK لبنية محددة
flutter build apk --target-platform android-arm64 --release
```

#### Android App Bundle (للنشر في Google Play)
```bash
# بناء App Bundle
flutter build appbundle --release

# بناء مع تحسينات إضافية
flutter build appbundle --release --obfuscate --split-debug-info=build/debug-info
```

### iOS (macOS فقط)

#### بناء للمحاكي
```bash
flutter build ios --simulator --release
```

#### بناء للجهاز
```bash
# بناء للجهاز
flutter build ios --release

# بناء IPA للتوزيع
flutter build ipa --release
```

### الويب
```bash
# بناء للويب
flutter build web --release

# بناء مع تحسينات إضافية
flutter build web --release --web-renderer canvaskit
```

### سطح المكتب

#### Windows
```bash
flutter build windows --release
```

#### macOS
```bash
flutter build macos --release
```

#### Linux
```bash
flutter build linux --release
```

---

## تحسين البناء

### تقليل حجم التطبيق
```bash
# استخدام R8/ProGuard لتقليل الحجم
flutter build apk --release --shrink

# تفعيل تقسيم الكود
flutter build appbundle --release --split-debug-info=build/debug-info
```

### تحسين الأداء
```bash
# بناء مع تحسينات الأداء
flutter build apk --release --tree-shake-icons

# تفعيل التحسينات المتقدمة
flutter build appbundle --release --obfuscate --split-debug-info=build/debug-info
```

---

## الاختبار

### اختبارات الوحدة
```bash
# تشغيل جميع الاختبارات
flutter test

# تشغيل اختبارات محددة
flutter test test/services/speech_service_test.dart

# تشغيل الاختبارات مع تقرير التغطية
flutter test --coverage
```

### اختبارات التكامل
```bash
# تشغيل اختبارات التكامل
flutter drive --target=test_driver/app.dart
```

### اختبارات الواجهة
```bash
# تشغيل اختبارات الواجهة
flutter test integration_test/
```

---

## التوقيع والنشر

### Android

#### إنشاء مفتاح التوقيع
```bash
# إنشاء keystore جديد
keytool -genkey -v -keystore ~/upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload

# إنشاء ملف key.properties
echo "storePassword=your-store-password" > android/key.properties
echo "keyPassword=your-key-password" >> android/key.properties
echo "keyAlias=upload" >> android/key.properties
echo "storeFile=~/upload-keystore.jks" >> android/key.properties
```

#### تحديث build.gradle
أضف إلى `android/app/build.gradle`:
```gradle
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

android {
    signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
            storePassword keystoreProperties['storePassword']
        }
    }
    buildTypes {
        release {
            signingConfig signingConfigs.release
        }
    }
}
```

#### بناء APK موقع
```bash
flutter build apk --release
```

### iOS

#### إعداد التوقيع في Xcode
1. افتح `ios/Runner.xcworkspace` في Xcode
2. اختر فريق التطوير في **Signing & Capabilities**
3. تأكد من إعداد Bundle Identifier صحيح

#### بناء للتوزيع
```bash
# بناء للأرشيف
flutter build ios --release

# أو بناء IPA مباشرة
flutter build ipa --release
```

---

## أتمتة البناء

### GitHub Actions
إنشاء ملف `.github/workflows/build.yml`:
```yaml
name: Build and Test

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - uses: subosito/flutter-action@v2
      with:
        flutter-version: '3.16.9'
    - run: flutter pub get
    - run: flutter analyze
    - run: flutter test

  build-android:
    needs: test
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - uses: subosito/flutter-action@v2
      with:
        flutter-version: '3.16.9'
    - run: flutter pub get
    - run: flutter build apk --release
    - uses: actions/upload-artifact@v3
      with:
        name: android-apk
        path: build/app/outputs/flutter-apk/app-release.apk
```

### Scripts محلية
إنشاء ملف `scripts/build.sh`:
```bash
#!/bin/bash

echo "Building Smart Access App..."

# تنظيف المشروع
flutter clean
flutter pub get

# تحليل الكود
flutter analyze

# تشغيل الاختبارات
flutter test

# بناء للمنصات المختلفة
echo "Building for Android..."
flutter build apk --release

echo "Building App Bundle..."
flutter build appbundle --release

if [[ "$OSTYPE" == "darwin"* ]]; then
    echo "Building for iOS..."
    flutter build ios --release
fi

echo "Build completed successfully!"
```

---

## استكشاف أخطاء البناء

### مشاكل شائعة

#### "Gradle build failed"
```bash
# تنظيف Gradle cache
cd android
./gradlew clean
cd ..
flutter clean
flutter pub get
```

#### "CocoaPods not installed" (iOS)
```bash
# تثبيت CocoaPods
sudo gem install cocoapods

# تحديث pods
cd ios
pod install
cd ..
```

#### "Dart analysis issues"
```bash
# إصلاح مشاكل التنسيق
dart format .

# إصلاح مشاكل التحليل
dart fix --apply
```

#### مشاكل التبعيات
```bash
# تحديث pubspec.lock
flutter pub deps
flutter pub upgrade

# حل تضارب التبعيات
flutter pub dependency_override
```

---

## أدوات التطوير المفيدة

### Flutter Inspector
```bash
# تشغيل مع Flutter Inspector
flutter run --debug
# ثم افتح DevTools في المتصفح
```

### تحليل الأداء
```bash
# تشغيل مع تحليل الأداء
flutter run --profile --trace-startup
```

### تحليل حجم التطبيق
```bash
# تحليل حجم APK
flutter build apk --analyze-size

# تحليل حجم App Bundle
flutter build appbundle --analyze-size
```

---

## المساهمة في المشروع

### إعداد بيئة المساهمة
1. Fork المشروع على GitHub
2. استنسخ fork الخاص بك
3. أنشئ branch جديد للميزة أو الإصلاح
4. اتبع معايير الكود المحددة
5. اكتب اختبارات للكود الجديد
6. أرسل Pull Request

### معايير الكود
- استخدم `dart format` لتنسيق الكود
- اتبع [Effective Dart](https://dart.dev/guides/language/effective-dart)
- اكتب تعليقات واضحة باللغة العربية
- أضف اختبارات للوظائف الجديدة

---

## الدعم الفني للمطورين

للحصول على مساعدة في التطوير:
- **GitHub Issues**: لتقارير الأخطاء والطلبات
- **Discord**: للنقاشات المباشرة
- **البريد الإلكتروني**: dev-support@smartaccess.app

---

**نتمنى لك تطويراً ممتعاً ومثمراً!**

