# Alogame KYC SDK — Android

Native identity-verification (XMDT) SDK for Android games — no WebView. Written in Kotlin, callable from Java.

Full docs: https://docs.alogame.vn/xmdt/android

> [!NOTE]
> Everything below describes the **dev** environment. Production has not been provisioned yet — do not point a live build at it.

## Install

```kotlin title="settings.gradle.kts"
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven { url = uri("https://jitpack.io") }
    }
}
```

```kotlin title="app/build.gradle.kts"
dependencies {
    implementation("com.github.alo-game:alogame-kyc-sdk-android:<version>")
}
```

Browse available versions at [jitpack.io/#alo-game/alogame-kyc-sdk-android](https://jitpack.io/#alo-game/alogame-kyc-sdk-android). Use the `com.github.alo-game:alogame-kyc-sdk-android` coordinate — not `vn.alogame:kyc-sdk`, which is not resolvable on JitPack even though it appears in this repo's own `build.gradle`.

The published artifact has zero third-party runtime dependencies.

## Requirements

- `compileSdk 36` — required, not a default choice; the SDK's own screen needs APIs that don't compile below it. This does not raise your app's own `targetSdk`.

## Quick start

```kotlin
import vn.alogame.kycsdk.AlogameKycSdk
import vn.alogame.kycsdk.AlogameKycConfig
import vn.alogame.kycsdk.AlogameKycEnv

AlogameKycSdk.init(context, AlogameKycConfig(env = AlogameKycEnv.DEV))
AlogameKycSdk.setGameRole(uid, sessionToken)
AlogameKycSdk.show(activity, listener)
```

`listener` implements `AlogameKycListener` — `onResult(result)` fires exactly once, after the screen closes, with `Success` / `Failed(reason, message)` / `Cancelled`. `Success` is a UI signal, not proof of verification — see the docs for the server-side check to pair it with.

## What this SDK does

Collects full name, date of birth, phone number (OTP-verified), and consent, then talks directly to Alogame's XMDT server — none of that data ever transits your game server. Your server's only involvement is minting a short-lived session token and handing it to the client; see the docs for that one call and for how to independently confirm a player's real verification status.
