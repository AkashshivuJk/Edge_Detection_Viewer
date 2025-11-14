# Edge Detection Viewer

A minimal Android + Native C++ project with real-time edge detection capabilities and a TypeScript web viewer. This project demonstrates JNI integration, OpenCV usage, and modern Android development practices.

## Table of Contents

- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Building the Android App](#building-the-android-app)
- [Running the Web Viewer](#running-the-web-viewer)
- [Project Details](#project-details)

## Project Structure

```
EdgeDetectionViewer/
├── README.md                          # Project documentation
├── build.gradle                       # Root-level Gradle configuration
├── gradle.properties                  # Gradle properties
├── settings.gradle                    # Gradle settings
├── package.json                       # NPM configuration for web viewer
│
├── app/                               # Android application module
│   ├── build.gradle                   # App-level Gradle configuration
│   ├── proguard-rules.pro             # ProGuard configuration for release builds
│   └── src/
│       ├── main/
│       │   ├── AndroidManifest.xml    # Android app manifest
│       │   ├── cpp/                   # Native C++ source code
│       │   │   ├── CMakeLists.txt     # CMake configuration for native build
│       │   │   └── native-lib.cpp     # JNI implementation with edge detection
│       │   ├── java/
│       │   │   └── com/flamapp/edgedetection/
│       │   │       └── MainActivity.kt # Main Android activity
│       │   └── res/                   # Android resources
│       │       ├── layout/
│       │       │   └── activity_main.xml  # Main activity layout
│       │       └── values/
│       │           └── strings.xml    # String resources
│       └── test/
│           └── ExampleUnitTest.kt     # Unit tests
│
├── build/                             # Build output directory
│   └── reports/
│       └── problems/
│           └── problems-report.html   # Build report
│
├── web/                               # Web viewer application
│   ├── index.html                     # Web viewer UI
│   ├── tsconfig.json                  # TypeScript configuration
│   └── viewer.ts                      # TypeScript viewer implementation
│
└── opencv/                            # OpenCV Android SDK (optional)
    └── README.md                      # OpenCV README
```

## Prerequisites

### For Android Development:
- **Android Studio** (latest version recommended)
- **Android SDK** 34 or higher
- **Android NDK** (required for native C++ compilation)
- **CMake** 3.18.1 or higher
- **Java/Kotlin** support enabled in Android Studio
- **Gradle** (bundled with Android Studio)

### For Web Viewer:
- **Node.js** and **npm** (for TypeScript compilation)
- **TypeScript** 5.0 or higher

### Hardware:
- Android device or emulator (API level 24+) for app testing
- Modern web browser for web viewer

## Installation & Setup

### Step 1: Clone or Extract the Project

```bash
# Navigate to your projects directory
cd /path/to/projects

# Clone the repository or extract the exported assets
# (If already extracted, skip to step 2)
```

### Step 2: Set Up Android Development Environment

1. **Open the project in Android Studio:**
   - Launch Android Studio
   - Select "File" → "Open"
   - Navigate to the `EdgeDetectionViewer` directory and open it

2. **Configure SDK Manager:**
   - Go to "Android Studio" → "Settings" → "Languages & Frameworks" → "Android SDK"
   - Ensure you have installed:
     - SDK Platform 34 or higher
     - Android SDK Tools
     - Android NDK (latest)
     - CMake 3.18.1 or higher
   - Apply changes and wait for installation to complete

3. **Install Gradle Dependencies:**
   - Android Studio will automatically download required Gradle dependencies
   - If not automatic, run:
     ```bash
     cd EdgeDetectionViewer
     ./gradlew build
     ```

### Step 3: (Optional) Set Up OpenCV

If you want to use OpenCV for advanced edge detection:

1. **Download OpenCV Android SDK:**
   - Visit [OpenCV Releases](https://opencv.org/releases/)
   - Download the Android SDK (e.g., version 4.8.0 or latest)

2. **Extract to Project:**
   ```bash
   # Extract the downloaded OpenCV Android SDK
   unzip opencv-android-sdk.zip
   
   # Move to project root as 'opencv' directory
   mv opencv-android-sdk EdgeDetectionViewer/opencv
   ```

3. **Configure Build (if using OpenCV):**
   - Add OpenCV module to `settings.gradle`:
     ```gradle
     include ':opencv'
     project(':opencv').projectDir = new File(rootDir, 'opencv/opencv-android')
     ```
   - Update `app/build.gradle` dependencies:
     ```gradle
     dependencies {
         implementation project(':opencv')
     }
     ```

4. **Copy Native Libraries:**
   ```bash
   # If cmake compilation includes OpenCV, native libs are handled automatically
   # Otherwise, manually copy prebuilt libraries:
   cp -r opencv/native/libs/* app/src/main/jniLibs/
   ```

## Building the Android App

### Option 1: Build via Android Studio (Recommended for Development)

1. Open the project in Android Studio
2. Connect your Android device or start an emulator (API 24+)
3. Click the **"Run"** button (green play icon) in the toolbar
4. Select your target device and click OK
5. Android Studio will build, compile native code, and deploy the APK

### Option 2: Build via Command Line

```bash
# Navigate to project root
cd EdgeDetectionViewer

# Build debug APK
./gradlew assembleDebug

# Build release APK (optimized)
./gradlew assembleRelease

# Build and immediately install on connected device
./gradlew installDebug

# Run on connected device
./gradlew runDebug
```

### Build Artifacts

- **Debug APK:** `app/build/outputs/apk/debug/app-debug.apk`
- **Release APK:** `app/build/outputs/apk/release/app-release.apk`

### Build Configuration

The app is configured with:
- **Compile SDK:** 34
- **Min SDK:** 24 (Android 7.0+)
- **Target SDK:** 34 (Android 14)
- **Architecture:** arm64-v8a, armeabi-v7a
- **Language:** Kotlin with JNI integration

## Running the Web Viewer

The web viewer displays edge detection frames in a browser interface.

### Step 1: Install Dependencies

```bash
cd EdgeDetectionViewer/web
npm install
```

### Step 2: Compile TypeScript

```bash
# Compile TypeScript to JavaScript
npm run build

# Or manually compile
npx tsc viewer.ts
```

### Step 3: Start a Local Server

The web viewer requires a local HTTP server (CORS and fetch API restrictions).

**Option A: Using Python 3 (Recommended)**
```bash
cd EdgeDetectionViewer/web
python3 -m http.server 8000
```

**Option B: Using Node.js (http-server)**
```bash
cd EdgeDetectionViewer/web
npm install -g http-server
http-server -p 8000
```

**Option C: Using VS Code Live Server**
- Install the "Live Server" extension in VS Code
- Right-click on `index.html` and select "Open with Live Server"

### Step 4: Access the Web Viewer

Open your browser and navigate to:
```
http://localhost:8000/index.html
```

The viewer should display the edge detection interface (requires Android app to be sending data).

## Project Details

### Architecture

- **Android App (Kotlin):** Main application entry point, UI rendering, and device interaction
- **Native Layer (C++):** High-performance edge detection using OpenCV
- **JNI Bridge:** Java-to-C++ communication for processing
- **Web Viewer (TypeScript):** Real-time visualization and control interface

### Key Files

- `MainActivity.kt` - Android entry point and UI initialization
- `native-lib.cpp` - C++ edge detection implementation with Canny algorithm
- `CMakeLists.txt` - Build configuration for native libraries
- `viewer.ts` - TypeScript web viewer logic
- `activity_main.xml` - Android layout resources

### Native Build System

This project uses **CMake** for compiling native C++ code:
- CMake version: 3.18.1+
- Supported ABIs: arm64-v8a, armeabi-v7a
- Configuration: `app/src/main/cpp/CMakeLists.txt`

### Features

✅ Real-time edge detection using Canny algorithm  
✅ JNI integration for native C++ performance  
✅ OpenCV integration (optional)  
✅ Modern Android (API 24-34) compatibility  
✅ Web-based viewer for live preview  
✅ Multi-architecture support (32-bit and 64-bit ARM)

## Troubleshooting

### Common Issues

**"NDK not installed"**
- Go to Android Studio Settings → Languages & Frameworks → Android SDK
- Install the latest NDK

**"CMake not found"**
- Install CMake from SDK Manager
- Verify CMake version: `cmake --version`

**"OpenCV not found"**
- Ensure OpenCV is extracted to `EdgeDetectionViewer/opencv/`
- Check that the module is correctly referenced in `settings.gradle`

**Web viewer not loading**
- Verify local server is running on port 8000
- Check browser console for CORS or fetch errors
- Ensure Android app is sending data to the server

**Build fails with native code errors**
- Clean and rebuild: `./gradlew clean build`
- Invalidate Android Studio cache: "File" → "Invalidate Caches"
- Verify NDK and CMake versions match project requirements

## License

This project is provided as-is for educational and development purposes.

## Support

For issues or questions:
1. Check the Android Studio build output for detailed error messages
2. Review the CMake configuration in `CMakeLists.txt`
3. Verify all prerequisites are installed and updated
4. Consult [OpenCV Documentation](https://docs.opencv.org/) for edge detection details
