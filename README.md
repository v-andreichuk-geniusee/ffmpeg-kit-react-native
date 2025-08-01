# Stable Release (Forked Local Implementation)

This is a **stable, locally maintained fork** with fixes and restored functionality for both **iOS** and **Android** platforms. The original upstream author removed the distributed binaries, so this fork fills that gap with working artifacts and local improvements.

Original docs here: https://github.com/arthenica/ffmpeg-kit

## 🔧 Highlights
- Forked from the original repository to restore and maintain functionality after upstream binaries were removed.
- Applied local fixes to ensure stability and compatibility on both iOS and Android.
- Integrated and pinned the forked `ffmpeg-kit-react-native` at `v1.0.0` to preserve local fixes and compatibility.
- Self-contained release—includes rebuilt binaries (see assets) where applicable.

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

## 📲 Platform-Specific Setup

After installing the package, follow the steps below to configure your project for iOS and Android.

---

### 🍏 iOS Setup

1. Navigate to the iOS project directory:
   ```bash
   cd ios && pos install
   ```

That’s it! You’re good to build and run on iOS.

---

### 🤖 Android Setup

Follow these steps carefully to configure your Android build properly:

---

#### 1. Modify `android/build.gradle`

Add the following **after** the `buildscript { ... }` block:

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

At the **very top**, add:

```groovy
import java.net.URL;
```

Then, **inside the `android` block**, add:

```groovy
repositories {
    flatDir {
        dirs "$rootDir/libs"
    }
}
```

Inside the `dependencies` block, add:

```groovy
implementation(name: 'ffmpeg-kit-full-gpl', ext: 'aar')
implementation(name: 'smart-exception-common-0.2.1', ext: 'jar')
implementation(name: 'smart-exception-java-0.2.1', ext: 'jar')
```

---

#### 3. Add automatic AAR downloader

At the **very end of `android/app/build.gradle`**, add the following block to automatically download the `ffmpeg-kit-full-gpl.aar`:

```groovy
afterEvaluate {
    def aarUrl = 'https://github.com/v-andreichuk-geniusee/ffmpeg-kit-react-native/releases/download/v1.0.0/ffmpeg-kit-full-gpl.aar'
    def aarFile = file("${rootDir}/libs/ffmpeg-kit-full-gpl.aar")

    tasks.register("downloadFFmpegKitAar") {
        doLast {
            if (!aarFile.parentFile.exists()) {
                println "📁 Creating directory: ${aarFile.parentFile.absolutePath}"
                aarFile.parentFile.mkdirs()
            }
            if (!aarFile.exists()) {
                println "⏬ Downloading ffmpeg-kit-full-gpl.aar from $aarUrl..."
                new URL(aarUrl).withInputStream { i ->
                    aarFile.withOutputStream { it << i }
                }
                println "✅ AAR downloaded to ${aarFile.absolutePath}"
            } else {
                println "ℹ️ AAR already exists at ${aarFile.absolutePath}"
            }
        }
    }

    preBuild.dependsOn("downloadFFmpegKitAar")
}
```

---

#### 4. Add Smart Exception JARs

- Download the following files from this release's assets:
  - `smart-exception-common-0.2.1.jar`
  - `smart-exception-java-0.2.1.jar`

- Place them inside:
  ```
  android/libs/
  ```

> 🛠️ Create the `libs` folder manually if it doesn’t exist.

---

#### 5. Add `postinstall` script

To ensure everything is downloaded and patched after installation, add the following to your `package.json` scripts section:

```json
"scripts": {
  "postinstall": "cd android && ./gradlew :app:downloadFFmpegKitAar"
}
```

This ensures the AAR is downloaded automatically after running `yarn install` or `npm install`.

---

✅ Once all steps are complete, you can build the Android app with full ffmpeg-kit support.
