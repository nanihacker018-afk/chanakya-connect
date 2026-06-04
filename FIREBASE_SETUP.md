# Firebase Setup Guide for Chanakya Connect

## 📋 Overview
This guide provides complete step-by-step instructions to configure Firebase for the Chanakya Connect Flutter project with Android package name `com.chanakyatech.chanakya_connect`.

---

## 🎯 What's Already Been Done

The following Firebase configurations have already been applied to your project:

✅ **Android Configuration Files:**
- `android/build.gradle` - Root Gradle with plugin management
- `android/app/build.gradle` - App-level Gradle with Firebase dependencies
- `android/app/src/main/AndroidManifest.xml` - Android manifest with permissions
- `android/settings.gradle` - Settings with Google Services plugin

✅ **Flutter Configuration Files:**
- `pubspec.yaml` - Updated with Firebase dependencies:
  - `firebase_core: ^2.24.0`
  - `firebase_auth: ^4.15.0`
  - `cloud_firestore: ^4.14.0`
  - `firebase_messaging: ^14.7.0`
- `lib/main.dart` - Firebase initialization code
- `lib/firebase_options.dart` - Firebase options template

---

## 🚀 Step-by-Step Setup Instructions

### Step 1: Create Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Add project"** or **"Create a new project"**
3. Enter project name: **Chanakya Connect** (or your preferred name)
4. Accept Google Analytics terms and click **"Create project"**
5. Wait for the project to initialize (this takes 1-2 minutes)

### Step 2: Register Android App with Firebase

1. In Firebase Console, click the **Android icon** to add an Android app
2. Fill in the app details:
   - **Android Package Name:** `com.chanakyatech.chanakya_connect`
   - **App Nickname:** Chanakya Connect (optional)
   - **SHA-1 Certificate Fingerprint:** (optional, but recommended)
3. Click **"Register app"**
4. **Download `google-services.json`** file
5. Click **"Next"** until you reach the final step
6. Click **"Continue to console"**

### Step 3: Place google-services.json File

**IMPORTANT:** The `google-services.json` file is critical for Firebase to work!

**File Path:** Place the downloaded `google-services.json` in:
```
chanakya-connect/android/app/google-services.json
```

**Exact Directory Structure:**
```
chanakya-connect/
├── android/
│   ├── app/
│   │   ├── google-services.json  ← PLACE YOUR FILE HERE
│   │   ├── build.gradle
│   │   ├── src/
│   │   └── ...
│   ├── build.gradle
│   └── settings.gradle
├── lib/
├── pubspec.yaml
└── ...
```

**Steps to place the file:**
1. Download `google-services.json` from Firebase Console
2. Copy the file
3. Navigate to `android/app/` folder in your project
4. Paste the file there
5. File should be named exactly: `google-services.json` (case-sensitive)

### Step 4: Update firebase_options.dart

Now you need to populate `lib/firebase_options.dart` with your Firebase credentials.

**Where to find your Firebase credentials:**

Option A - From `google-services.json`:
1. Open the `google-services.json` file you just placed
2. Find these values:
   - `"api_key"` → Your API key
   - `"client_id"` → Your app ID
   - `"project_id"` → Your project ID
   - `"storage_bucket"` → Your storage bucket
   - `"project_number"` → Your messaging sender ID (in format as project number)

Option B - From Firebase Console:
1. Go to Firebase Console → Your Project
2. Click **⚙️ Settings** (gear icon)
3. Click **"Project settings"**
4. Go to **"Your apps"** → **Android**
5. You'll see all the configuration values

**Update `lib/firebase_options.dart`:**

Replace the placeholder values in the `android` configuration:

```dart
static const FirebaseOptions android = FirebaseOptions(
  apiKey: 'YOUR_API_KEY_FROM_GOOGLE_SERVICES_JSON',
  appId: 'YOUR_APP_ID_FROM_GOOGLE_SERVICES_JSON',
  messagingSenderId: 'YOUR_PROJECT_NUMBER_FROM_GOOGLE_SERVICES_JSON',
  projectId: 'YOUR_PROJECT_ID_FROM_GOOGLE_SERVICES_JSON',
  storageBucket: 'YOUR_STORAGE_BUCKET_FROM_GOOGLE_SERVICES_JSON',
);
```

Example (replace with your actual values):
```dart
static const FirebaseOptions android = FirebaseOptions(
  apiKey: 'AIzaSyD1234567890abcdefghij',
  appId: '1:123456789:android:abcdef1234567890',
  messagingSenderId: '123456789',
  projectId: 'chanakya-connect-abc123',
  storageBucket: 'chanakya-connect-abc123.appspot.com',
);
```

### Step 5: Enable Firebase Services

#### A. Enable Authentication
1. In Firebase Console, go to **Authentication** (left sidebar)
2. Click **"Get started"** or **"Sign-in method"**
3. Click **"Add new provider"** and enable:
   - ✅ **Email/Password** (required for basic login)
   - ✅ **Google** (optional, for social login)
4. Save changes

#### B. Create Firestore Database
1. Go to **Firestore Database** (left sidebar)
2. Click **"Create database"**
3. Choose location (closest to your users - India is recommended for Chanakya school)
4. Start in **"Production mode"** (or test mode for development)
5. Click **"Enable"**

#### C. Enable Cloud Messaging (for notifications)
1. Go to **Cloud Messaging** tab
2. The Android API key and Sender ID should already be configured
3. Note down the **Sender ID** for backend notification setup

### Step 6: Install Dependencies

Run the following commands in your terminal:

```bash
# Navigate to project directory
cd chanakya-connect

# Get all Flutter dependencies
flutter pub get

# Optional: Clean before building
flutter clean
flutter pub get
```

### Step 7: Run the App

```bash
flutter run
```

**Expected Output:**
```
✓ Firebase initialized successfully
Firebase Initialized!
Chanakya Connect is ready to use.
```

If you see this message, **Firebase setup is complete!** ✅

---

## 🔍 Troubleshooting

### Error: "google-services.json not found"
- **Solution:** Ensure the file is placed at `android/app/google-services.json` (not in `android/` root directory)
- Check the filename is exactly `google-services.json` (lowercase, no spaces)
- Run `flutter clean` then `flutter pub get` then `flutter run`

### Error: "Package mismatch"
- **Problem:** Package name doesn't match between Firebase and app
- **Solution:** Verify `com.chanakyatech.chanakya_connect` is registered in Firebase Console
- Check in `android/app/build.gradle`: `applicationId = "com.chanakyatech.chanakya_connect"`
- Check in `android/app/src/main/AndroidManifest.xml`: `package="com.chanakyatech.chanakya_connect"`

### Error: "Failed to initialize Firebase"
- Verify Firebase project is created
- Confirm Android app is registered with correct package name
- Ensure all values in `firebase_options.dart` are correct
- Check that `google-services.json` is in the right location
- Try deleting `google-services.json` from Firebase Console and re-downloading

### Error: "Google Services plugin not found"
- Run `flutter clean`
- Delete `android/.gradle` and `android/build` directories
- Run `flutter pub get`
- Run `flutter run`

### App still showing placeholder UI after Firebase initialization
- This is normal! You need to implement your authentication and app screens next
- The current `lib/main.dart` just confirms Firebase is working

---

## ✅ Verification Checklist

After setup, verify everything is working:

- [ ] Firebase project created at console.firebase.google.com
- [ ] Android app registered with package name `com.chanakyatech.chanakya_connect`
- [ ] `google-services.json` downloaded and placed in `android/app/`
- [ ] `lib/firebase_options.dart` updated with your Firebase credentials
- [ ] `pubspec.yaml` has Firebase dependencies
- [ ] Run `flutter pub get` successfully
- [ ] Run `flutter run` shows "Firebase initialized successfully"
- [ ] Firebase Authentication enabled in Console
- [ ] Firestore Database created
- [ ] Cloud Messaging enabled

---

## 🎓 Next Steps

Once Firebase is working, implement:

1. **Authentication System**
   - Sign up screen
   - Login screen
   - Password reset
   - Role-based navigation (Student/Teacher/Admin)

2. **User Profiles**
   - Store user data in Firestore
   - Handle profile images

3. **Cloud Firestore Collections**
   - Create collections for: users, classes, homework, attendance, grades
   - Set up security rules

4. **Push Notifications**
   - Setup Firebase Cloud Messaging
   - Send notifications from backend

5. **Other Features**
   - Attendance tracking
   - Homework management
   - Grade management
   - Admin dashboard

---

## 📚 Useful Resources

- [Firebase Documentation](https://firebase.google.com/docs)
- [Firebase Authentication](https://firebase.google.com/docs/auth)
- [Cloud Firestore](https://firebase.google.com/docs/firestore)
- [Firebase Messaging](https://firebase.google.com/docs/messaging)
- [FlutterFire Setup Guide](https://firebase.flutter.dev/docs/overview)

---

## 🆘 Still Having Issues?

If you're stuck:

1. Check the [Firebase Error Messages](https://firebase.google.com/docs/auth/troubleshoot)
2. Run `flutter doctor -v` to check Flutter environment
3. Check Android Studio for SDK issues: Tools → SDK Manager
4. Review the logs in Firebase Console for errors
5. Make sure your device/emulator is connected to internet
6. Try on a physical device if emulator fails

**Good luck! 🚀**
