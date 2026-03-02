# Building Wojakcoinwallet for iOS

This project uses [Capacitor](https://capacitorjs.com/) to run the same Next.js wallet UI inside a WebView on iOS.

**You need a Mac with Xcode** to build and run the iOS app. Building cannot be done on Linux/Windows.

## Prerequisites

- **macOS** with Xcode (from the App Store)
- **Xcode Command Line Tools**: `xcode-select --install`
- **CocoaPods**: `sudo gem install cocoapods` or `brew install cocoapods`
- Node.js 18+

## Setup

1. **Install dependencies**
   ```bash
   npm install --legacy-peer-deps
   ```

2. **Configure environment**
   Copy `.env.example` to `.env.local` and set:
   - **`NEXT_PUBLIC_ELECTRS_API_URL`** — Public Electrs URL (default: `https://api.wojakcoin.cash`).
   - **`NEXT_PUBLIC_PRICE_API_URL`** — (Optional) Base URL for price API.

3. **Build the web app and sync to iOS**
   ```bash
   npm run build
   npx cap sync ios
   ```
   Or in one step: `npm run build:ios`

4. **Install CocoaPods dependencies** (on the Mac)
   ```bash
   cd ios/App && pod install && cd ../..
   ```

5. **Open in Xcode and run**
   ```bash
   npm run open:ios
   ```
   Then in Xcode: select a simulator or device → Run (▶️). For a device you need an Apple Developer account and signing.

## Scripts

| Script | Description |
|--------|-------------|
| `npm run build:ios` | Runs `next build` then `cap sync ios` (copy `out/` into the iOS project). |
| `npm run open:ios` | Opens the iOS project in Xcode. |

## Build flow

1. `next build` produces a **static export** in `out/`.
2. `cap sync ios` copies `out/` into `ios/App/App/public`.
3. The app uses `NEXT_PUBLIC_ELECTRS_API_URL` (https://api.wojakcoin.cash) for blockchain data; CORS is enabled on the API for the app.

## Release / Archive

In Xcode:

1. Select **Any iOS Device (arm64)** (or a connected device).
2. **Product → Archive**.
3. After archiving, use **Distribute App** to export for App Store Connect or ad-hoc distribution.

## App icons

The Android project uses Wojak branding in `android/.../res/`. For iOS, set the app icon in Xcode:

- Open **ios/App/App/Assets.xcassets/AppIcon.appiconset** and add the required sizes, or replace with your `wojaklogo`-based assets.

## Notes

- **CocoaPods** was skipped on non-Mac (e.g. Linux CI). On the Mac, run `pod install` in `ios/App` before opening in Xcode.
- **Signing**: You need an Apple Developer account to run on a real device or ship to the App Store.
