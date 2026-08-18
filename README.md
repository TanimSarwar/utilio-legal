<div align="center">
  <img src="./assets/icon.png" alt="Utilio Logo" width="120" height="120" style="border-radius: 28px;" />
  
  # Utilio
  
  <p><strong>A modern, production-ready cross-platform super app packed with 40+ daily utilities, financial tools, wellness trackers, and smart widgets.</strong></p>

  <p>
    <img src="https://img.shields.io/badge/Platform-Android%20%7C%20iOS%20%7C%20Web-6366F1?style=flat-square" alt="Platforms" />
    <img src="https://img.shields.io/badge/Expo-SDK%2052-000020?style=flat-square&logo=expo" alt="Expo SDK" />
    <img src="https://img.shields.io/badge/React%20Native-0.76-61DAFB?style=flat-square&logo=react" alt="React Native" />
    <img src="https://img.shields.io/badge/TypeScript-5.3-3178C6?style=flat-square&logo=typescript" alt="TypeScript" />
  </p>
</div>

---

## Tech Stack

- **Framework:** React Native + Expo (SDK 52, Hermes Engine)
- **Web:** React Native Web
- **Navigation:** Expo Router (file-based routing)
- **State Management:** Zustand with AsyncStorage persistence
- **Animations:** React Native Reanimated + Animated API
- **Icons:** Solar Icons (`@solar-icons/react-native`)
- **Fonts:** Poppins (Google Fonts via `@expo-google-fonts/poppins`)
- **Auth & Cloud Sync:** Google OAuth (via `expo-auth-session`) + Google Drive AppData Sync
- **Ads:** Google AdMob (`react-native-google-mobile-ads`)

---

## Getting Started

### Prerequisites

- Node.js 18+
- npm or yarn
- Expo CLI: `npm install -g expo-cli`
- EAS CLI (for builds): `npm install -g eas-cli`

### Setup

```bash
# Install dependencies
npm install --legacy-peer-deps

# Copy environment variables
cp .env.example .env

# Start development server
npx expo start
```

### Running on Devices

```bash
# Android native run / build
npx expo run:android

# iOS native run / Simulator
npx expo run:ios

# Web browser
npx expo start --web
```

---

## Google Authentication & Cloud Sync Setup

Utilio supports Google Sign-In to authenticate users and securely sync user favorites/picks with Google Drive AppData folder.

### Google Cloud Console Configuration

1. **OAuth Consent Screen:**
   - **App Name:** `Utilio`
   - **User Support Email:** Developer/support contact email.
   - **Test Users:** While in "Testing" status, ensure test email addresses are added under **Audience > Test users**.
   - **Scopes (Data access):** `openid`, `.../auth/userinfo.email`, `.../auth/userinfo.profile`, `.../auth/drive.appdata`, `.../auth/user.birthday.read`.

2. **Android OAuth Client ID:**
   - **Package Name:** `com.utilio.app`
   - **SHA-1 Certificate Fingerprints:**
     - For EAS builds: EAS Android Keystore SHA-1 (found via `eas credentials` or expo.dev build page).
     - For local debug builds: Local `debug.keystore` SHA-1.
   - **Custom URI Scheme:** In your Android OAuth Client ID settings in Google Cloud Console, ensure **"Enable custom URI scheme"** is checked.

3. **Web OAuth Client ID:**
   - Used for token exchange and Web / Expo AuthSession support.
   - **Authorized Redirect URIs:**
     - `http://localhost:8081`
     - `https://auth.expo.io/@tanimsarwar1/utilio`
     - `https://auth.expo.io/@tanimsarwar/utilio`

---

## AdMob Integration

Google Mobile Ads is fully integrated via `react-native-google-mobile-ads` with strict spam-protection and battery-friendly preloading.

### Architecture & Components

- **`adManager`** (`lib/ads.ts`): Central singleton managing background interstitial preloading, event lifecycles, and cooldowns.
- **`useInterstitialAd`** (`hooks/useInterstitialAd.ts`): React hook wrapping `adManager`.
- **`<AdBanner />`** (`components/ads/AdBanner.tsx`): Adaptive banner ads with fallback safety, automatic collapse on error, and category exclusions.
- **`useAdsStore`** (`store/ads.ts`): Zustand store tracking timestamps, cooldowns, and focus mode status.

### Frequency & Safeguard Rules

- **Interstitial Cooldown:** Enforces a minimum **3-minute interval** between full-screen ads.
- **Focus Mode:** Mutes all interstitial ads for **30 minutes** during productivity/study sessions.
- **Category Exclusions:** Ads are automatically disabled on wellness/mindfulness features (e.g., meditation, sleep tracker).
- **Banner Placements:** Integrated on Home feed, Category tabs, and top utility screens (Calculator, Unit Converter, Currency Converter, Age Calculator, etc.).

### Development vs Production vs Test Mode

- **Development (`__DEV__`)**: Automatically uses Google's official Test Ad Unit IDs.
- **Release Builds**: Uses your production IDs configured in `.env` / `constants/ads.ts`.
- **Testing on Release/Preview APKs**: Set `EXPO_PUBLIC_USE_TEST_ADS=true` in `.env` if you want to verify ads immediately on a release APK before AdMob live inventory approval.

---

## Project Structure

```
app/
  _layout.tsx              # Root layout (fonts, splash, onboarding check, AdMob init)
  onboarding.tsx           # 3-page swipeable intro
  settings.tsx             # Theme, accent color, notifications
  (tabs)/
    _layout.tsx            # Bottom curved dip tab navigator, theme switcher, Google auth modal
    index.tsx              # Home tab (weather, market indices, sports, quotes, recents, ad banner)
    my-pick.tsx            # Custom pinned favorites collection
    tools/                 # 10 tool screens (Unit converter, QR generator, Barcode, etc.)
    finance/               # 8 finance screens (Stocks, Budget, Currency converter, Calculator, etc.)
    wellness/              # 7 wellness screens (Water tracker, BMI, Breathing, Sounds, etc.)
    utilities/             # 15 utility screens (Prayer times, Weather, Notes, Pomodoro, etc.)
components/                # UI design system & modular widgets
constants/                 # Theme palettes, features registry, ad IDs
lib/                       # APIs (weather, sports, market), ads manager, Google sync, haptics
store/                     # Zustand stores (theme, auth, ads, favorites, settings)
```

---

## Building for Production

```bash
# Preview Android APK (for direct device installation)
eas build --platform android --profile preview

# Production Android App Bundle (.aab for Google Play Store)
eas build --platform android --profile production

# Production iOS Build
eas build --platform ios --profile production
```

---

## License

Private project.
