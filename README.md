# Stable Release (Forked Local Implementation)

This is a **stable, locally maintained fork** with fixes and restored functionality for both **iOS** and **Android** platforms. The original upstream author removed the distributed binaries, so this fork fills that gap with working artifacts and local improvements.

📚 Original documentation: https://github.com/arthenica/ffmpeg-kit

## 🔧 Highlights

- Forked from the original repository to restore and maintain functionality after upstream binaries were removed.
- Applied local fixes to ensure stability and compatibility on both iOS and Android.
- Integrated and pinned the forked `ffmpeg-kit-react-native` at `v1.0.0` to preserve local fixes and compatibility.
- Self-contained release—includes rebuilt binaries (see assets) where applicable.

---

## 🚀 Installation

To install the forked version of `ffmpeg-kit-react-native`, use one of the following:

```bash
# Using Yarn
yarn add "git+https://github.com/v-andreichuk-geniusee/ffmpeg-kit-react-native.git#v1.0.0"

# Or using npm
npm install "git+https://github.com/v-andreichuk-geniusee/ffmpeg-kit-react-native.git#v1.0.0"
```

Or add it manually to your `package.json`:

```json
"ffmpeg-kit-react-native": "git+https://github.com/v-andreichuk-geniusee/ffmpeg-kit-react-native.git#v1.0.0"
```

Then install dependencies:

```bash
yarn install
# or
npm install
```

✅ **Note:** Locking to `#v1.0.0` ensures your project uses the exact stable commit with local fixes from this fork.

---

## 📲 Platform-Specific Setup

After installing the package, follow the steps below to configure your project for **iOS** and **Android** platforms.

---

### 🍏 iOS Setup

1. Navigate to the iOS directory and install CocoaPods:

```bash
cd ios && pod install
```

That's it! You’re good to build and run on iOS.

---

### 🤖 Android Setup

Follow these steps carefully to configure your Android build properly:

---

#### 1. Modify `android/build.gradle`

Remove this line **if present**:

```groovy
buildscript {
    ext {
        // <<Remove the line below>>
        ffmpegKitPackage = "<package name>"
    }
    ...
}
```

Then, **after the `buildscript { ... }` block**, add:

```groovy
allprojects {
    repositories {
        google()
        mavenCentral()
        flatDir {
            dirs "$rootDir/libs"
        }
    }
}
```

---

#### 2. Edit `android/app/build.gradle`

Inside the `android` block, add:

```groovy
repositories {
    flatDir {
        dirs "$rootDir/libs"
    }
}
```

Inside the `dependencies` block, remove (if present):

```groovy
implementation 'com.arthenica:ffmpeg-kit-full-gpl:6.0-2.LTS'
```

And add:

```groovy
implementation(name: 'ffmpeg-kit-full-gpl', ext: 'aar')
implementation(name: 'smart-exception-common-0.2.1', ext: 'jar')
implementation(name: 'smart-exception-java-0.2.1', ext: 'jar')
```

---

#### 3. Download Required Binaries

Download the following files from this release’s assets:

- `ffmpeg-kit-full-gpl.aar`
- `smart-exception-common-0.2.1.jar`
- `smart-exception-java-0.2.1.jar`

Place them in:

```
android/libs/
```

> 🛠️ Create the `libs` folder manually if it doesn’t exist.

---

✅ Once all steps are complete, you can build and run the Android app with full `ffmpeg-kit` support.
