# Expo iOS Development Guide

This repository contains multiple projects. The primary mobile app under `app/` is a Flutter application (Dart) with a native iOS project in `app/ios`. Expo (React Native) cannot be integrated into a Flutter app. Therefore, Expo Dev Client cannot be used to run `app/ios` directly.

However, the repo also includes a React Native + Expo example under `sdks/react-native/example` that provides a fully working Expo-managed iOS development flow. This guide explains both paths:

- Path A — Flutter app (original iOS app at `app/ios`): how to run with Flutter/Xcode.
- Path B — Expo-managed React Native example (`sdks/react-native/example`): how to run with Expo tooling on iOS Simulator or device.

---

## Prerequisites

- macOS with Xcode 15+ and Command Line Tools
- CocoaPods: `sudo gem install cocoapods`
- Node.js 20+ and npm 10+ (or pnpm/yarn if you prefer)
- Expo CLI (via `npx`): no global install required
- Optional (for Path A): Flutter SDK installed and on PATH
- Apple Developer account for running on physical devices

---

## Path A: Run the Flutter iOS app (`app/ios`)

The iOS application under `app/` is built with Flutter. To run it on iOS:

1) Install dependencies

- Install Flutter: https://docs.flutter.dev/get-started/install
- From the repo root: `cd app`
- Get packages: `flutter pub get`

2) iOS setup

- Install pods: `cd ios && pod install`
- Open workspace: `open Runner.xcworkspace` (in Xcode)
- In Xcode, set a valid Development Team for signing if running on device.

3) Run

- From Xcode, select a Simulator or your device and press Run
- Or via CLI: `cd ..` back to `app/` and run `flutter run -d iOS`

Troubleshooting

- Clear derived data: `rm -rf ~/Library/Developer/Xcode/DerivedData`
- Reinstall pods: `cd app/ios && pod deintegrate && pod install`
- Reset Flutter caches: `flutter clean && flutter pub get`

---

## Path B: Run the Expo-managed RN example (`sdks/react-native/example`)

This example is an Expo-managed React Native app already configured with Expo SDK and a working iOS project for Dev Client builds.

1) Install dependencies

- From repo root: `cd sdks/react-native/example`
- Install: `npm ci` (or `npm install`)

2) First iOS build (installs Expo Dev Client)

- `npx expo run:ios`
  - Select a Simulator or connected device. Xcode will build and install a Dev Client.
  - If using a physical device, open the generated Xcode workspace and set your Development Team if prompted.

3) Subsequent development cycles

- Start Metro with Dev Client: `npx expo start --dev-client`
- Launch the app (Simulator or device). It will connect to Metro automatically.

4) Useful commands

- Open Dev Menu: Cmd+Ctrl+Z on Simulator, or shake device
- Reload JS: press `r` in the Metro terminal or use Dev Menu
- Type check: `tsc --noEmit` (if you have TypeScript globally) or configure a script
- Doctor: `npx expo doctor`

Troubleshooting

- Reset Metro cache: `npx expo start -c`
- Clear iOS build artifacts:
  - `rm -rf ios/Pods ios/Podfile.lock` then `cd ios && pod install`
  - Clear derived data: `rm -rf ~/Library/Developer/Xcode/DerivedData`
- If scheme not found after `run:ios`, open the workspace in Xcode and let it index once.

---

## Why Expo cannot drive the Flutter app

Expo Dev Client works for React Native (managed or bare) projects. The `app/` project here is Flutter, so its `ios` directory is generated and managed by Flutter tooling, not React Native. Converting a Flutter app to React Native/Expo is a non-trivial migration and out of scope for this integration task.

If you want to pursue a React Native + Expo version of the mobile app, a follow-up plan would include:

- Creating a new RN app scaffold with Expo
- Porting features and native integrations progressively
- Using the existing `sdks/react-native` package to interface with Omi hardware/services

---

## Summary

- Flutter app (`app/ios`): run with Flutter/Xcode (Path A)
- Expo iOS dev flow: use the Expo-managed RN example at `sdks/react-native/example` (Path B)

Both sets of instructions are maintained in this repo for clarity. If you prefer, we can create a dedicated Expo app in a follow-up PR and begin porting UI/logic from Flutter to React Native.
