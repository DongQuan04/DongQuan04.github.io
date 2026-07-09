---
title: "Building the Release Version"
date: 2026-06-18
weight: 4
chapter: false
pre: " <b> 5.4.4 </b> "
---

#### Build an Android APK

1. Make sure that `ApiConfig.baseUrl` points to the correct production or staging `ApiUrl` (not a temporary testing endpoint).
2. Build the release APK by running:

```bash
flutter build apk --release
```

![Build APK release](/images/5-Workshop/5.4-flutter/build-apk.png)

3. The generated APK is located at:

```text
build/app/outputs/flutter-apk/app-release.apk
```

You can install this APK directly on a physical Android device for demonstration or testing.

{{% notice info %}}
To reduce the APK size by generating separate builds for each CPU architecture, add the `--split-per-abi` option.
{{% /notice %}}

#### Build for iOS (Optional, Requires macOS)

```bash
flutter build ios --release
```

After the build completes, open the project in Xcode (`ios/Runner.xcworkspace`) to archive the application and distribute it through TestFlight or install it directly on a physical device.

#### Install and Test on a Physical Device

1. Install the generated APK (or the iOS build) on a physical smartphone.
2. Repeat the demonstration workflow from **Sections 5.4.2** and **5.4.3**:

    - Register a merchant
    - Add menu items
    - Scan the table QR code as a customer
    - Place an order
    - Complete the payment

This verifies that the application works reliably on a real device, not just in the emulator.

![Demo on a physical device](/images/5-Workshop/5.4-flutter/real-device-demo.png)

You have successfully built and tested the Flutter application on a physical device with full connectivity to the AWS backend. In the next chapter (**5.5**), you will perform a complete end-to-end validation of the entire system.