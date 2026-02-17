# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ByteBank is a Flutter banking simulation app (FIAP Phase 3 Tech Challenge). Users can perform transfers, deposits, and withdrawals. It integrates with:
- **Firebase Auth** for user authentication
- **Cloud Firestore** for fetching and storing authenticated user transactions
- **Firebase Storage** for storing payment receipt images

## Common Commands

```bash
# Install dependencies
flutter pub get

# Run the app (debug mode)
flutter run

# Build for release
flutter build apk          # Android
flutter build ios          # iOS

# Run all tests
flutter test

# Run a single test file
flutter test test/path/to/test_file.dart

# Lint / analyze code
flutter analyze

# Format code
dart format .
```

## Architecture

This is a Flutter app with the following expected layers:

- **Authentication**: Firebase Auth integration
- **Data Layer**: Cloud Firestore for reading and writing user transactions
- **Storage Layer**: Firebase Storage for uploading and retrieving payment receipt images
- **State Management**: Provider — manages app state for authenticated user and transaction data
- **UI Layer**: Responsive Flutter widgets for banking operations (transfer, deposit, withdrawal)

> **IMPORTANT**: This project does NOT use any external REST API. All data (transactions, users) is stored and retrieved exclusively via **Cloud Firestore**. All file uploads use **Firebase Storage**. Do not introduce `http`, `dio`, or any HTTP client for backend communication.

### Firebase Setup
Firebase configuration files are required:
- `google-services.json` for Android (`android/app/`)
- `GoogleService-Info.plist` for iOS (`ios/Runner/`)

These are not committed to version control.

## Documentation Source

Always use the **context7 MCP** (already configured in `.mcp.json`) to look up up-to-date documentation for any library used in this project (Flutter, Firebase, Provider, FL Chart, etc.) before writing code. Example usage within Claude Code:

```
# Resolve library ID then query docs
mcp__context7__resolve-library-id  →  mcp__context7__query-docs
```

Preferred libraries for context7 lookups:
- `firebase_auth`, `cloud_firestore`, `firebase_storage` (FlutterFire)
- `provider`
- `fl_chart`
- `image_picker`, `cached_network_image`
