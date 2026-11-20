<div align="center">

<img src="app/src/main/res/drawable/app_logo_v1.png" alt="NOMADA logo" width="180"/>

# NOMADA · Android App

*Discover, create, share and explore: social notes tied to real places.*

![Kotlin](https://img.shields.io/badge/Kotlin-7F52FF?logo=kotlin&logoColor=white)
![Jetpack Compose](https://img.shields.io/badge/Jetpack_Compose-4285F4?logo=jetpackcompose&logoColor=white)
![Hilt](https://img.shields.io/badge/Hilt-DI-34A853)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?logo=firebase&logoColor=black)
![Google Maps](https://img.shields.io/badge/Google_Maps-4285F4?logo=googlemaps&logoColor=white)

</div>

> **Final Degree Project (TFG)** of the Higher Vocational Training Program in Cross-Platform
> Application Development (DAM), IES Francisco de Quevedo, Madrid, 2024‑2025.
> **Collaborative project** by a team of 5, organized with **Scrum**.
>
> This repository contains the **Android app**. The backend (REST API built with Spring Boot) lives in
> **[TFG_Server](https://github.com/j-novillo/TFG_Server)**.

---

## What is NOMADA?

NOMADA is a **social network of geolocated notes**. Users can leave notes pinned to a spot on the
map (a restaurant, a landscape, a monument, an event…), rate them, add photos, decide who can see
them and discover what other people have left around them.

The idea starts from a question: *what if people's memories and recommendations were tied to the
place where they happened?* The app combines an interactive map with social features (profiles,
followers, likes and saved notes) to answer it.

```
┌─────────────────────────┐   HTTPS + JWT   ┌──────────────────────────┐      ┌─────────┐
│  Android app            │ ──────────────► │  REST API (Spring Boot)  │ ───► │  MySQL  │
│  this repository        │ ◄────────────── │  TFG_Server              │      └─────────┘
└──────────┬──────────────┘      JSON       └──────────────────────────┘
           │
           ▼
   Firebase (Auth, Storage, Crashlytics, Messaging) · Google Maps / Places
```

## Features

**Account and security**
- Sign-up with real-time validation and **email verification** (handled by the server).
- Login with **JWT** and **automatic session renewal** (OkHttp interceptor and authenticator).
- **Fingerprint login** (Biometric API), with credentials stored encrypted in the
  Android Keystore (Security Crypto).

**Notes**
- Six note types, each with its own icon: *classic, event, historical, food, landscape and cultural*.
- Per-note privacy (**public, followers only or private**), 1‑to‑10 rating, automatic GPS
  geolocation and **photos** stored in Firebase Storage.

**Map**
- **Google Maps** with custom markers by note type and a special marker when several notes
  share the same location.
- Place search (Google Places), filters by type and light or dark map style.

**Social**
- Profiles with a picture, user search, follow and unfollow.
- Likes and saved notes, with sortable and filterable lists.
- **Dark / light mode** across the whole app.

## Architecture

**Clean Architecture + MVVM**, with the UI built entirely in **Jetpack Compose**:

```
app/src/main/java/com/example/client
├── ui/                 # Compose screens + ViewModels (one package per screen)
│   ├── splashScreen, startScreen, login, registerScreen
│   ├── noteMap/        Map, place search, list of notes at a location
│   ├── normalNoteScreen/, addNoteScreen/
│   ├── userScreen/     Own profile, visitor profile, user search
│   ├── navigation/     Navigation Compose with typed destinations
│   └── theme/          Material 3, light and dark theme
├── domain/
│   ├── model/          Domain models (Note, Event, User...)
│   └── usecases/       One use case per action (authentication, notes, social, map, images...)
├── data/
│   ├── remote/         Retrofit: services, data sources and security (tokens, interceptors)
│   ├── local/          Room (user and location) and DataStore
│   ├── firebase/       Firebase Auth
│   └── repositories/   Single source of truth for the domain
└── di/                 Hilt modules
```

- Screen state is exposed from the ViewModels with **StateFlow** and collected in Compose.
- Network and database calls are modeled as `Flow<NetworkResult<T>>`
  (Loading / Success / Error) and run on injected dispatchers.
- Two **build flavors**: `development` (local API) and `production` (deployed API).

## Tech stack

| Area | Technologies |
|---|---|
| Language and UI | Kotlin, Jetpack Compose, Material 3, Navigation Compose, SplashScreen API |
| Architecture | MVVM, Clean Architecture, Hilt, Coroutines + Flow |
| Networking | Retrofit, OkHttp (logging, interceptors and an authenticator to refresh the JWT), Gson |
| Local persistence | Room, DataStore |
| Maps | Google Maps Compose, Play Services Location, Google Places API, Accompanist Permissions |
| Firebase | Auth, Storage, Firestore, Crashlytics, Cloud Messaging |
| Security | Biometric API, AndroidX Security Crypto |
| Images | Coil |

## How to run it

Requirements: a recent Android Studio, **SDK 35** (minimum Android 8.0, API 26), a
**Google Maps / Places** key and a Firebase `google-services.json`.

1. Clone the repository and open it in Android Studio.
2. Set your Google Maps key in `AndroidManifest.xml` and your Places key in `app/build.gradle.kts`.
3. Choose the *build variant*:
   - `development`: points to a local server (adjust the IP in `build.gradle.kts`).
   - `production`: points to the deployed API.
4. Start the backend ([TFG_Server](https://github.com/j-novillo/TFG_Server)) if you use `development`.
5. Run the app.

## Team and methodology

Built by **Jorge Novillo**, **Adrián Saavedra**, **Marcos Sánchez**, **Mario Valiente** and
**Aitor García**, following **Scrum** (user-story backlog, sprints, planning, reviews and
retrospectives, managed in Taiga) and a **GitFlow** workflow with feature branches and
Pull Requests reviewed by the team (more than 40 PRs in this repository alone).

**[docs/METHODOLOGY.md](docs/METHODOLOGY.md)** explains how we organized ourselves and what each
person worked on.
