# Android + React Native CLI Setup (Ubuntu 24.04)

This guide provides a clean, simple, single-user setup for Android development with React Native CLI on Ubuntu 24.04.

---

## 1. Install Base Packages

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl git build-essential unzip zip qemu-kvm libvirt-daemon-system libvirt-clients virt-manager cpu-checker ca-certificates
sudo apt install -y adb
sudo apt install -y openjdk-17-jdk
```

---

## 2. Install nvm

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
source ~/.bashrc
nvm --version
```

---

## 3. Install Node (LTS)

```bash
nvm install --lts
nvm use --lts
nvm alias default lts/*
node -v
```

---

## 4. Install Android Studio

```bash
sudo snap install android-studio --classic
android-studio
```

Inside Android Studio, complete the first-run setup and install via SDK Manager:
- SDK Platform (API 33 / 34 / 35 — pick one)
- Android SDK Build-Tools
- Android SDK Platform-Tools
- Android SDK Command-line Tools
- Emulator
- System image (x86_64)

The SDK will be installed to `~/Android/Sdk` by default.

---

## 5. Add Android SDK Environment Variables (optional)

If you want the environment variables visible in terminals, append to `~/.bashrc`:

```bash
export ANDROID_SDK_ROOT="$HOME/Android/Sdk"
export ANDROID_HOME="$ANDROID_SDK_ROOT"
export PATH="$PATH:$ANDROID_SDK_ROOT/platform-tools:$ANDROID_SDK_ROOT/emulator"
```

Then reload:

```bash
source ~/.bashrc
echo $ANDROID_SDK_ROOT
```

> Note: Android Studio generally manages SDK paths automatically; setting these is optional but useful for some CLI tools.

---

## 6. Verify Tools

```bash
which adb
adb --version
```

---

## 7. Create Workspace

```bash
mkdir -p ~/works
cd ~/works
```

---

## 8. Create React Native Project (RN 0.82)

```bash
npx @react-native-community/cli@latest init TurboModuleExample --version 0.82
cd TurboModuleExample
```

---

## 9. Verify Android Setup

```bash
adb devices
npx react-native doctor
```

---

## 10. Run the Android App

Make sure the Android Emulator is running (Device Manager → Start AVD) or connect a physical device with USB debugging enabled.

```bash
npx react-native run-android
```

If Metro bundler does not start automatically, in another terminal run:

```bash
npx react-native start
```

---

## Troubleshooting Tips

- If build fails with `SDK location not found`:
  - Create `android/local.properties` in your project (project root) with:
    ```
    sdk.dir=/home/<your-username>/Android/Sdk
    ```
    Replace `<your-username>` with your username (e.g., `oora`).

- If `adb` is not found, ensure `$ANDROID_SDK_ROOT/platform-tools` is in your `PATH`, or install platform-tools via SDK Manager.

- If Gradle complains about Java, verify:
  ```bash
  java -version
  ```
  Should show Java 17.

- To clean Android build:
  ```bash
  cd android
  ./gradlew clean
  cd ..
  ```

---

## Final Notes

- This setup is single-user, standard, and avoids system-wide SDK changes.
- Keep Android Studio and SDK tools updated via SDK Manager.
- Use `nvm` to avoid system-wide Node installations and to easily switch Node versions.
