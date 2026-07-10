# KinCare

Enterprise-grade Flutter application for child and medication management built with Clean Architecture, MVVM, GetX, and GraphQL.

## Architecture

```
lib/
├── app/                    # Application configuration
│   ├── bindings/           # GetX dependency injection bindings
│   ├── constants/          # App-wide string, dimension, and key constants
│   ├── routes/             # Named routes and page configuration
│   ├── services/           # Logger and theme services
│   └── theme/              # Material 3 theme (light/dark), colors, typography
├── core/                   # Shared infrastructure
│   ├── accessibility/      # Semantic helpers, responsive utilities
│   ├── api/                # GraphQL client, service, queries/mutations
│   ├── errors/             # AppException hierarchy, Result<T> wrapper
│   ├── network/            # Connectivity abstraction
│   ├── storage/            # Hive local storage abstraction
│   └── widgets/            # Reusable widget library
├── data/                   # Data layer
│   ├── datasource/
│   │   ├── local/          # Hive cache datasources
│   │   └── remote/         # GraphQL datasources
│   ├── models/             # Data models with JSON serialization
│   └── repositories/       # Repository implementations (offline-first)
├── domain/                 # Business logic layer
│   ├── entities/           # Pure domain entities
│   ├── repositories/       # Repository contracts (interfaces)
│   └── usecases/           # Single-responsibility use cases
└── presentation/           # UI layer
    ├── controllers/        # GetX controllers (MVVM ViewModels)
    ├── modules/            # Feature screens
    │   ├── auth/
    │   ├── dashboard/
    │   ├── children/
    │   ├── medication/
    │   ├── profile/
    │   ├── help/
    │   └── about/
    └── widgets/            # Shared presentation widgets (drawer)
```

## Design Principles

- **SOLID** — Single-responsibility use cases, interface segregation via repository contracts
- **Clean Architecture** — Domain layer has zero dependencies on data/presentation
- **MVVM** — Controllers serve as ViewModels; UI is declarative and reactive
- **Repository Pattern** — All data access through repository abstractions
- **Offline-First** — Hive cache with network-aware fallback
- **Dependency Injection** — GetX bindings per module, lazy initialization

## Data Flow

```
UI (Screen) → Controller → UseCase → Repository (interface)
                                          ↓
                              RepositoryImpl (data layer)
                              ├── RemoteDatasource (GraphQL)
                              └── LocalDatasource (Hive)
```

## GraphQL Integration

- **API**: GraphQLZero (mock API)
- **Mapping**: Posts → Children, Todos → Medications, Users → Profiles
- **Features**: Query + mutation support, retry (2 attempts), timeout (30s), in-memory cache, error extraction

## State Management (GetX)

- Reactive observables (`.obs`) for UI state
- `GetView<Controller>` for declarative screen binding
- `Obx(() => ...)` for fine-grained rebuilds
- Module-scoped bindings with `Get.lazyPut`

## Modules

| Module | Features |
|--------|----------|
| **Auth** | Mock login, form validation, session persistence, remember-me |
| **Dashboard** | Summary cards, quick actions, recent activities, pull-to-refresh |
| **Children** | List with search/sort/filter, detail view, add/edit, responsive grid |
| **Medications** | CRUD, search/filter, confirmation dialogs, offline cache |
| **Profile** | View/edit profile, avatar, GraphQL + cache |
| **Help** | Expandable FAQs, support contact |
| **About** | Version info, license page |

## Accessibility

Every screen implements:

- **Semantics**: labels, hints, roles on all interactive elements
- **Screen readers**: TalkBack / VoiceOver compatible
- **Touch targets**: Minimum 48x48dp on all tappable elements
- **Focus management**: Logical tab order, keyboard navigation
- **Text scaling**: Clamped between 0.8x–2.0x
- **Responsive**: Phone/tablet/desktop, portrait/landscape
- **Theme**: Light + dark mode with dynamic switching

## Reusable Widget Library

`PrimaryButton` · `SecondaryButton` · `CustomTextField` · `AppSearchBar` · `AppCard` · `EmptyView` · `ErrorView` · `LoadingView` · `SkeletonLoading` · `ConfirmDialog`

## Setup

### Prerequisites

- Flutter 3.x (tested on 3.41.7)
- Dart 3.x

### Install and Run

```bash
flutter pub get
flutter run
```

### Build

```bash
flutter build apk --release    # Android
flutter build ios --release     # iOS
```

### Generate Mocks (for testing)

```bash
dart run build_runner build
```

### Run Tests

```bash
flutter test
```

### Static Analysis

```bash
flutter analyze
```

## Demo Credentials

```
Email:    admin@kincare.com
Password: password
```

## Error Handling

All errors flow through a sealed `Result<T>` type:

```dart
sealed class Result<T> {
  const factory Result.success(T data) = Success<T>;
  const factory Result.failure(AppException exception) = Failure<T>;
}
```

Exception hierarchy: `NetworkException` · `TimeoutException` · `GraphQLException` · `ParsingException` · `AuthException` · `CacheException` · `UnexpectedException`

## Testing

- **Unit tests**: Result wrapper, data models, use cases with mocked repositories
- **Widget tests**: PrimaryButton, SecondaryButton, CustomTextField, EmptyView, ErrorView, ConfirmDialog
- **51 tests total**, all passing

## Tech Stack

| Category | Technology |
|----------|-----------|
| Framework | Flutter 3.x / Dart 3.x |
| Design System | Material 3 |
| State Management | GetX |
| API | GraphQL (graphql_flutter) |
| Local Storage | Hive |
| Testing | flutter_test, mockito |
| Connectivity | connectivity_plus |
| Loading | shimmer |

## License

MIT
