# 🏠 StudentNest – Student Accommodation Finder

**CSE201 Mobile Application Development Assignment**
Botswana School of Business Sciences | Year 2 | Semester 2, 2026

---

## 📋 Project Overview

StudentNest is an Android application that helps tertiary students in Gaborone find affordable and safe accommodation. The app allows students to browse listings, filter by price/location/date, chat with landlords, pay deposits, and reserve rooms.

---

## ⚙️ Technical Requirements

| Requirement | Value |
|---|---|
| Language | Kotlin |
| IDE | Android Studio Hedgehog 7+ |
| Min SDK | 26 (Android 8.0 Oreo) |
| Target SDK | 34 |
| Gradle | 7.4.2 |
| Database | Room (local/offline) |
| Emulator | Nexus 5 API 23+ |

---

## 🚀 Setup Instructions

### Step 1 – Open Project
1. Extract the zip file
2. Open Android Studio → **File → Open** → select the `StudentNest` folder
3. Wait for Gradle sync to complete (first time may take 3–5 minutes)

### Step 2 – Sync Dependencies
- If prompted, click **Sync Now**
- All dependencies will auto-download via Gradle

### Step 3 – Run the App
1. Open **AVD Manager** → create a **Nexus 5** emulator with **API 26+**
2. Click the green ▶ Run button
3. On first launch, the database seeds automatically with **50 listings** and **50 students**

### Step 4 – Test Login
Use these demo credentials:
- **Email:** `thabo.molefe@studentnest.com`
- **Password:** `student123`

Or register a new account from the Register screen.

---

## 🗂️ Project Structure

```
app/src/main/java/com/studentnest/
├── data/
│   ├── dao/                  # Room DAO interfaces
│   │   ├── UserDao.kt
│   │   ├── ListingDao.kt
│   │   ├── ReservationDao.kt
│   │   ├── UserPreferencesDao.kt
│   │   └── ChatMessageDao.kt
│   ├── database/
│   │   └── AppDatabase.kt    # Room DB + seed data
│   ├── entities/             # Room entities (tables)
│   │   ├── User.kt
│   │   ├── Listing.kt
│   │   ├── Reservation.kt
│   │   ├── UserPreferences.kt
│   │   └── ChatMessage.kt
│   └── repository/
│       └── AppRepository.kt  # Single source of truth
│
├── ui/
│   ├── auth/
│   │   ├── SplashActivity.kt
│   │   ├── AuthActivity.kt
│   │   ├── LoginFragment.kt
│   │   ├── RegisterFragment.kt
│   │   └── AuthViewModel.kt
│   ├── listings/
│   │   ├── ListingsFragment.kt   # Home – browse all
│   │   ├── ListingsViewModel.kt
│   │   ├── ListingAdapter.kt
│   │   └── ListingDetailActivity.kt
│   ├── filters/
│   │   └── FilterBottomSheet.kt
│   ├── reservation/
│   │   ├── ReservationActivity.kt  # Payment screen
│   │   ├── ReservationViewModel.kt
│   │   ├── ReceiptActivity.kt
│   │   ├── ReservationsFragment.kt # My bookings
│   │   └── ReservationAdapter.kt
│   ├── chat/
│   │   ├── ChatActivity.kt
│   │   ├── ChatViewModel.kt
│   │   ├── ChatAdapter.kt
│   │   └── ChatListFragment.kt
│   ├── ProfileFragment.kt
│   └── MainActivity.kt
│
└── utils/
    ├── SessionManager.kt     # SharedPreferences session
    ├── NotificationHelper.kt # Local notifications
    ├── NotificationReceiver.kt
    └── Extensions.kt         # Kotlin extension functions
```

---

## 🎯 Features Implemented

### A. User Management (20 marks)
- ✅ Student registration with validation
- ✅ Login with email + password
- ✅ Session management via SharedPreferences
- ✅ 50 pre-seeded student accounts
- ✅ Role-based: STUDENT / PROVIDER

### B. Listings (25 marks)
- ✅ 50 listings seeded on first launch
- ✅ Title, price (BWP), location (Gaborone areas), type
- ✅ Amenities, availability date, deposit amount
- ✅ House image per listing
- ✅ Distance from UB campus

### C. Smart Filtering & Alerts (20 marks)
- ✅ Filter by price range (RangeSlider)
- ✅ Filter by location (Spinner)
- ✅ Filter by room type (Spinner)
- ✅ Filter by availability date (DatePickerDialog)
- ✅ Local notifications when listings match saved preferences
- ✅ User can save alert preferences from Profile screen

### D. Deposit & Reservation (15 marks)
- ✅ Simulated card payment (Card / Mobile Money)
- ✅ Receipt with unique reference number (e.g. SN-AB3C123456)
- ✅ Listing status changes to RESERVED after payment
- ✅ Other users cannot reserve a reserved room

### E. Extension Feature – Chat (20 marks)
- ✅ Chat between student and landlord per listing
- ✅ Sent/received message bubbles with timestamps
- ✅ Messages persisted in Room Database
- ✅ Unread message count tracking

---

## 🗃️ Database Schema

### `users` table
| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PK | Auto-increment |
| studentId | TEXT | e.g. STU001 |
| fullName | TEXT | |
| email | TEXT | Unique |
| password | TEXT | Plain text (demo only) |
| phone | TEXT | |
| role | TEXT | STUDENT / PROVIDER |
| createdAt | LONG | Timestamp |

### `listings` table
| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PK | Auto-increment |
| title | TEXT | |
| description | TEXT | |
| price | REAL | BWP/month |
| depositAmount | REAL | BWP |
| location | TEXT | Gaborone area |
| address | TEXT | Full address |
| type | TEXT | Room type |
| amenities | TEXT | Comma-separated |
| availabilityDate | TEXT | dd/MM/yyyy |
| imageUrl | TEXT | Drawable name |
| status | TEXT | AVAILABLE / RESERVED |
| providerId | INTEGER | FK → users.id |
| distanceFromUb | REAL | km |

### `reservations` table
| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PK | Auto-increment |
| listingId | INTEGER | FK → listings.id |
| studentId | INTEGER | FK → users.id |
| referenceNumber | TEXT | e.g. SN-XY9Z123456 |
| depositPaid | REAL | BWP |
| paymentMethod | TEXT | Card / Mobile Money |
| cardLastFour | TEXT | Last 4 digits |
| status | TEXT | CONFIRMED / CANCELLED |
| reservedAt | LONG | Timestamp |

### `user_preferences` table
| Column | Type | Notes |
|--------|------|-------|
| userId | INTEGER PK | FK → users.id |
| minPrice | REAL | |
| maxPrice | REAL | |
| preferredLocation | TEXT | |
| preferredType | TEXT | |
| notificationsEnabled | BOOLEAN | |

### `chat_messages` table
| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PK | Auto-increment |
| listingId | INTEGER | Which listing chat |
| senderId | INTEGER | FK → users.id |
| receiverId | INTEGER | FK → users.id |
| message | TEXT | |
| isRead | BOOLEAN | |
| sentAt | LONG | Timestamp |

---

## 📱 Screens (Activities / Fragments)

1. **SplashActivity** – App logo + auto-navigate
2. **AuthActivity** → LoginFragment / RegisterFragment
3. **MainActivity** → bottom navigation host
   - **ListingsFragment** – Browse & filter all listings
   - **ReservationsFragment** – My reservations history
   - **ChatListFragment** – Message inbox
   - **ProfileFragment** – Preferences & alert settings
4. **ListingDetailActivity** – Full detail + reserve/chat buttons
5. **ReservationActivity** – Payment form (card / mobile money)
6. **ReceiptActivity** – Confirmation receipt with reference number
7. **ChatActivity** – Real-time chat with landlord

---

## 🧪 Test Credentials

| Role | Email | Password |
|------|-------|----------|
| Student | thabo.molefe@studentnest.com | student123 |
| Student | mpho.seretse@studentnest.com | student123 |
| Landlord | landlord@studentnest.com | provider123 |

---

## 📦 Deliverables Checklist

- ☐ Zipped Android Studio project (this file)
- ☐ APK file (build → Generate Signed APK or debug APK)
- ☐ 3–6 page report (design, schema, screenshots) – PowerPoint
- ☐ Demo video/screencast (max 10 minutes)
- ☐ GitHub repository link (optional)

---

## 🔧 Building the APK

1. In Android Studio: **Build → Build Bundle(s) / APK(s) → Build APK(s)**
2. Find the APK at: `app/build/outputs/apk/debug/app-debug.apk`

---

*StudentNest – Built with ❤️ using Kotlin + Room Database*
