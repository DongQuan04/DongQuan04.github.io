---
title: "Preparing the Flutter App"
date: 2026-06-17
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

#### Install the Dependencies

1. Open a terminal in the root directory of the Flutter project and run:

```bash
flutter pub get
```

![flutter pub get](/images/5-Workshop/5.4-flutter/pub-get.png)

2. Verify that the following key packages are listed in `pubspec.yaml`:

+ `provider` — State management (`ModeProvider`, `MerchantProvider`, `CustomerProvider`).
+ `http` — Sends REST API requests to the backend.
+ `mobile_scanner` — Scans QR codes.
+ `shared_preferences` — Stores local data such as the user ID, selected mode, and merchant information.
+ `intl` — Formats currency and date/time using the `vi_VN` locale.
+ `google_fonts` — Provides typography using **Baloo 2** and **Inter**.

#### Configure the Backend Connection

1. Open the `lib/shared/design.dart` file and locate the `ApiConfig` class.
2. Replace the `baseUrl` value with the `ApiUrl` obtained from the `sam deploy` output in **Section 5.3.1**:

```dart
class ApiConfig {
  static const baseUrl = 'https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/Prod';

  static const menuEndpoint     = '/menu';
  static const invoicesEndpoint = '/invoices';
  static const ordersEndpoint   = '/orders';
  static const paymentsEndpoint = '/payments';
}
```

![ApiConfig configuration](/images/5-Workshop/5.4-flutter/api-config.png)

{{% notice warning %}}
If you forget to update `baseUrl`, the application will not be able to communicate with the backend APIs and will display errors when loading menus or submitting orders. If you encounter connection issues, verify this configuration first.
{{% /notice %}}

#### Run the Application

1. Connect an Android or iOS device, or start an emulator, and verify that it is detected:

```bash
flutter devices
```

2. Run the application:

```bash
flutter run
```

![Flutter run](/images/5-Workshop/5.4-flutter/flutter-run.png)

3. The application should launch to the **ModeSelectScreen**, where you can choose either the **Merchant** or **Customer** role.

![Mode selection screen](/images/5-Workshop/5.4-flutter/mode-select.png)

In the next section (**5.4.2**), you will act as a **Merchant** to create a store and add menu items before testing the customer workflow.