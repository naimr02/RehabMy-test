# RehabMy

A comprehensive rehabilitation management platform connecting patients and therapists for guided exercise tracking, progress monitoring, and real-time communication.

## Badges

![Min SDK](https://img.shields.io/badge/Min%20SDK-26-green)
![Target SDK](https://img.shields.io/badge/Target%20SDK-35-blue)
![Kotlin](https://img.shields.io/badge/Kotlin-2.0.0-purple)
![Compose](https://img.shields.io/badge/Jetpack%20Compose-Material%203-brightgreen)
![License](https://img.shields.io/badge/License-MIT-yellow)

## Key Features

### For Patients
- **Daily Exercise Dashboard** — View assigned exercises with completion status, repetitions, and pain-level tracking
- **Interactive Progress Visualization** — Circular progress indicator with animated completion percentage
- **Exercise Library** — Browse exercises categorized by body part (Hip, Knee, Ankle, Foot) with detailed instructions
- **Historical Progress Charts** — Weekly, monthly, and yearly completion-rate charts using ycharts
- **Date Navigation** — Browse past exercise assignments with intuitive date picker
- **Real-time Therapist Chat** — Direct messaging with assigned therapists

### For Therapists
- **Patient Management** — View all assigned patients with exercise completion summaries
- **Exercise Assignment** — Create and assign custom exercises with duration, frequency, and descriptions
- **AI-Powered Feedback** — Generate personalized exercise feedback using Google Gemini 1.5 Pro
- **Real-time Patient Chat** — Communicate with patients through integrated chat system
- **Multi-tab Interface** — Patients, Chat, and Manage Patients views

### Shared Features
- **Role-based Authentication** — Firebase Auth with Patient/Therapist role detection
- **Dark/Light Theme** — System-aware theme switching with persistent preference
- **Offline-first Architecture** — Firestore real-time listeners with local caching
- **Edge-to-edge UI** — Modern Material 3 design with dynamic color support

## Screenshots / Demo

| Patient Dashboard | Exercise Library | Progress Charts | Therapist Portal |
|:-----------------:|:----------------:|:---------------:|:----------------:|
| To be added | To be added | To be added | To be added |


## Tech Stack & Libraries

| Category | Technologies |
|----------|--------------|
| **Architecture** | MVVM, Repository Pattern, Hilt (DI), Clean Architecture principles |
| **UI / Layout** | Jetpack Compose (Material 3), Navigation Compose, Accompanist (Permissions, SwipeRefresh, SystemUIController) |
| **Networking / Database** | Firebase Auth, Firestore (real-time), Realtime Database, Firebase Storage, Firebase In-App Messaging |
| **AI / ML** | Google Generative AI (Gemini 1.5 Pro) |
| **Charting** | ycharts (Compose-native), MPAndroidChart |
| **Image Loading** | Coil Compose |
| **Async / Concurrency** | Kotlin Coroutines, StateFlow, Flow |
| **Testing** | JUnit, Espresso, Compose UI Test |
| **Build** | Gradle KTS (Kotlin DSL), Version Catalogs (libs.versions.toml) |

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                        Presentation Layer                    │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐   │
│  │   Patient    │  │  Therapist   │  │    Auth/Shared   │   │
│  │  Composables │  │  Composables │  │    Composables   │   │
│  └──────┬───────┘  └──────┬───────┘  └────────┬─────────┘   │
└─────────│─────────────────│────────────────────│─────────────┘
          │                 │                    │
          ▼                 ▼                    ▼
┌─────────────────────────────────────────────────────────────┐
│                      ViewModel Layer                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐   │
│  │  Exercise/   │  │ Therapist    │  │    Auth/Theme    │   │
│  │ Progress VM  │  │  ViewModels  │  │    ViewModels    │   │
│  └──────┬───────┘  └──────┬───────┘  └────────┬─────────┘   │
└─────────│─────────────────│────────────────────│─────────────┘
          │                 │                    │
          ▼                 ▼                    ▼
┌─────────────────────────────────────────────────────────────┐
│                     Repository / Service Layer               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐   │
│  │Firestore Repo│  │  Chat Repo   │  │  Gemini AI Svc   │   │
│  │Patient Svc   │  │Therapist Svc │  │                  │   │
│  └──────┬───────┘  └──────┬───────┘  └────────┬─────────┘   │
└─────────│─────────────────│────────────────────│─────────────┘
          │                 │                    │
          ▼                 ▼                    ▼
┌─────────────────────────────────────────────────────────────┐
│                        Data Layer                            │
│         Firebase Auth │ Firestore │ Realtime DB │ Storage    │
└─────────────────────────────────────────────────────────────┘
```

**Key Architectural Decisions:**
- **Hilt** for compile-time dependency injection (SingletonComponent for Firebase instances)
- **StateFlow** for reactive UI state management in ViewModels
- **Nested Navigation Graphs** for Patient/Therapist feature modules
- **Repository Pattern** abstracting Firestore/Realtime Database operations
- **Real-time Listeners** for live data synchronization across devices

## Getting Started

### Prerequisites
- **Android Studio** Ladybug (2024.2.1) or later
- **JDK 11** or higher
- **Android SDK** with API Level 35 (compileSdk) and API Level 26 (minSdk)
- **Firebase Project** with Authentication, Firestore, Realtime Database, and Storage enabled
- **Google AI API Key** (Gemini) for AI feedback generation

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/RehabMy.git
cd RehabMy

# Configure API keys (see Configuration section below)
cp local.properties.example local.properties
# Edit local.properties and add your Gemini API key

# Build debug APK
./gradlew assembleDebug

# Run on connected device/emulator
./gradlew installDebug
```

### Running Tests

```bash
# Unit tests
./gradlew test

# Instrumented tests (requires device/emulator)
./gradlew connectedAndroidTest
```

## Configuration / API Keys

### 1. Firebase Setup
1. Create a Firebase project at [Firebase Console](https://console.firebase.google.com)
2. Add Android app with package name `com.naimrlet.rehabmy_test`
3. Download `google-services.json` and place in `app/`
4. Enable Authentication (Email/Password), Firestore, Realtime Database, Storage
5. Configure Firestore security rules for user-based access

### 2. Local Properties
Create `local.properties` in project root:

```properties
# Gemini API Key for AI feedback generation
gemini.api.key=YOUR_GEMINI_API_KEY_HERE

# Optional: SDK/NDK paths
sdk.dir=/path/to/Android/Sdk
```

The API key is injected via `BuildConfig.GEMINI_API_KEY` at build time.

### 3. Firestore Data Structure
```
/users/{userId}
  - name: string
  - age: number
  - isTherapist: boolean
  - condition: string (patients only)
  - assignedPatient: array<string> (therapist UIDs)

/users/{patientId}/exercises/{exerciseId}
  - name, description: string
  - duration, repetitions: number
  - frequency: string
  - completed: boolean
  - painLevel: number
  - comments: string
  - assignedDate, dueDate, completedDate: Timestamp
  - assignedBy: string (therapist UID)

/library/exercises/{bodyPart}/{exerciseId}
  - name, description: string
  - bodyPart: enum (HIP, KNEE, ANKLE, FOOT)

/chats/{chatId}/messages/{messageId}
  - senderId, text, timestamp
```

## Project Structure

```
app/src/main/java/com/naimrlet/rehabmy_test/
├── auth/                    # Authentication (login, signup, user type selection)
│   ├── login/               # LoginScreen, LoginViewModel
│   ├── signup/              # SignupScreen, SignupViewModel
│   └── usertype/            # UserTypeSelectionScreen
├── patient/
│   ├── dashboard/           # PatientDashboardScreen, DashboardHomeScreen
│   │   └── home/            # ExerciseViewModel, ExerciseDetailDialog
│   ├── library/             # LibraryScreen, LibraryViewModel, ExerciseDetailScreen
│   ├── progress/            # AboutScreen (Progress), ProgressViewModel
│   └── therapist/           # TherapistScreen (patient view)
├── therapist/
│   ├── chat/                # TherapistChatScreen, PatientChatScreen, ViewModels
│   ├── service/             # TherapistPatientService
│   ├── ExerciseManagementScreen, PatientDetailsScreen, AssignExerciseDialog
│   └── ExerciseItem, ExerciseInfo
├── chat/                    # Shared chat components (MessageInput, MessageComponents)
├── model/                   # Exercise, LibraryExercise, BodyPart, ChatMessage
├── navigation/              # AppNavigation, DashboardDestination, TherapistChatGraph
├── di/                      # FirebaseModule (Hilt)
├── ui/theme/                # Material3 Theme, Color, Type, DarkModeViewModel
├── RehabMyApplication.kt    # Hilt Application class
└── MainActivity.kt          # Entry point, Edge-to-edge setup
```

## License

```
MIT License

Copyright (c) 2026 Naim bin Abdul Rahman

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
---

*Built with modern Android development practices — Jetpack Compose, Firebase, Hilt, and Material 3.*
