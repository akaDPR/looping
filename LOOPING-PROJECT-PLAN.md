# LOOPING — Project Scope & Development Plan

**Version:** 1.0
**Date:** 15 March 2026
**Platform:** Flutter (iOS + Android)
**Budget:** INR 2,50,000 (Two Lakh Fifty Thousand)
**Timeline:** 14 Weeks

---

## TABLE OF CONTENTS

1. [Project Overview](#1-project-overview)
2. [Feature List — Included vs Excluded](#2-feature-list)
3. [Detailed Feature Specifications](#3-detailed-feature-specifications)
4. [Screen-by-Screen Breakdown](#4-screen-by-screen-breakdown)
5. [Technical Architecture](#5-technical-architecture)
6. [Development Phases & Milestones](#6-development-phases--milestones)
7. [Budget Breakdown](#7-budget-breakdown)
8. [Assumptions & Dependencies](#8-assumptions--dependencies)
9. [Post-Launch Support](#9-post-launch-support)
10. [Sign-Off](#10-sign-off)

---

## 1. PROJECT OVERVIEW

**Looping** is a proximity-based social networking app that allows users to discover, connect, and communicate with people nearby using Bluetooth Low Energy (BLE) and location services.

**Core Value Proposition:** Find and connect with verified, real people in your physical vicinity — at events, campuses, co-working spaces, and public places.

---

## 2. FEATURE LIST

### INCLUDED IN SCOPE

| # | Feature | Priority |
|---|---------|----------|
| F01 | User Registration (5-step flow) | Must Have |
| F02 | Login (Phone OTP + Email) | Must Have |
| F03 | User Profile Management | Must Have |
| F04 | Nearby Discovery (BLE + Location) | Must Have |
| F05 | Radar/Scan UI with live user bubbles | Must Have |
| F06 | Connection Request (Send / Accept / Reject) | Must Have |
| F07 | Real-time 1:1 Chat | Must Have |
| F08 | Voice Calling (1:1) | Must Have |
| F09 | Video Calling (1:1 with PiP) | Must Have |
| F10 | Push Notifications | Must Have |
| F11 | Screenshot & Screen Recording Protection | Must Have |
| F12 | Privacy Controls (Field Visibility Toggles) | Must Have |
| F13 | DigiLocker Integration (Document Verification) | Should Have |
| F14 | Social Account Linking (Google, Instagram, Facebook) | Should Have |
| F15 | Connection/Contact List | Must Have |
| F16 | Call History Log | Should Have |
| F17 | Block / Report User | Must Have |
| F18 | App Settings (Notifications, Privacy, Account) | Must Have |
| F19 | Delete Account | Must Have |
| F20 | App Store & Play Store Deployment | Must Have |

### EXCLUDED FROM SCOPE (Not included in ₹2.5L)

| # | Feature | Reason |
|---|---------|--------|
| X01 | Reels / Short Video Posting | Client specified exclusion |
| X02 | Group Chat | Phase 2 feature |
| X03 | Stories / Status Updates | Phase 2 feature |
| X04 | Admin Web Dashboard | Phase 2 feature |
| X05 | Payment / Subscription System | Phase 2 feature |
| X06 | Multi-language / Localization | Phase 2 feature |
| X07 | AI-based Matching / Recommendations | Phase 2 feature |
| X08 | File/Document Sharing in Chat | Phase 2 feature |
| X09 | Location Sharing in Chat | Phase 2 feature |
| X10 | Web App Version | Phase 2 feature |

---

## 3. DETAILED FEATURE SPECIFICATIONS

### F01 — User Registration (5-Step Flow)

**Step 1: Profile Setup**
- Full name (required)
- Relation (S/o, W/o, D/o)
- Date of birth (date picker)
- Occupation (text input)
- Address (text input)
- Profile photo upload (camera + gallery)
- Avatar with initials as fallback

**Step 2: Contact & Identity**
- Mobile number with OTP verification (Firebase Auth)
- Email address with verification link
- Identity document type selector (Aadhaar / National ID / Passport)
- Masked display of ID number (only last 4 digits visible)
- NOTE: Raw Aadhaar number is NOT stored (Aadhaar Act compliance)

**Step 3: Purpose & Education**
- Purpose selection chips: Friendly / Business / Networking / Other
- School name (text input)
- College name (text input)
- Current studies / course (text input)

**Step 4: Social Accounts & Documents**
- Link Instagram (OAuth)
- Link Facebook (OAuth)
- Link Google/Gmail (OAuth)
- Link LinkedIn (OAuth) — optional
- DigiLocker integration for document verification
- Document cards: Aadhaar, PAN, Office ID, College ID, Certificates
- Each document shows: Verified / Pending / Upload status

**Step 5: Privacy & Terms**
- Field visibility toggles for: Name, Phone, Email, Occupation, Education, Social Accounts
- Each toggle has: Label, sublabel, ON/OFF switch
- Terms & Conditions text box (scrollable)
- Checkbox: "I agree to Terms & Conditions and Privacy Policy"
- Screenshot restriction warning badge
- Submit button → Complete Registration

**Validation Rules:**
- Step 1: Name and DOB required
- Step 2: Phone OTP must be verified
- Step 3: At least one purpose selected
- Step 4: Optional (can skip)
- Step 5: Terms checkbox must be checked

---

### F02 — Login

- Email + Password login
- Phone number + OTP login
- "Forgot Password" flow (email reset link)
- "Create Account" link → Registration
- Auto-login if session exists (Firebase token refresh)
- Biometric login (fingerprint/Face ID) — device-native

---

### F03 — User Profile Management

- View own profile
- Edit all registration fields
- Change profile photo
- View verification status of documents
- Logout
- Delete account (with confirmation)

---

### F04 — Nearby Discovery (BLE + Location)

**How it works:**
1. App broadcasts a BLE advertisement containing a unique user token
2. App simultaneously scans for other Looping users' BLE signals
3. RSSI (signal strength) is used to estimate approximate distance
4. Firestore geo-queries provide fallback when BLE is unavailable

**Technical Details:**
- BLE scanning range: ~10-30 meters (varies by device)
- Scan interval: Every 3 seconds while discovery is active
- Battery optimization: Scan stops when app is backgrounded
- Location permission: Required for BLE on Android 12+
- Fallback: If BLE unavailable, use GPS-based proximity (50m radius)

**User Experience:**
- User taps "Find Nearby" on home screen
- Radar animation begins with sweep effect
- Nearby users appear as bubbles on radar
- Each bubble shows: Avatar, Name, Distance
- Tapping a bubble shows mini-profile
- User can send connection request from mini-profile

**Privacy:**
- User can turn discovery ON/OFF
- When OFF, user is invisible to nearby scans
- No location history is stored
- BLE tokens rotate every 24 hours

---

### F05 — Radar/Scan UI

- Animated radar with concentric rings
- Sweep animation (rotating gradient)
- Center dot showing "You" / "Your location"
- User bubbles positioned by distance (closer = nearer to center)
- Bubble shows: Colored avatar, name, distance label
- Selected bubble gets highlight ring
- Bottom navigation bar visible
- "Finding people nearby..." status indicator

---

### F06 — Connection Request System

**Send Request:**
- Tap user bubble → Mini profile card appears
- Card shows: Avatar, name, occupation, mutual connections count
- "Send Connection Request" button
- Button states: Default → Sending... → Sent!
- Toast/overlay: "Request Sent!"

**Receive Request:**
- Push notification: "[Name] wants to connect"
- In-app notification banner (top of screen)
- Request card with Accept / Decline buttons

**Accept:**
- Connected confirmation screen
- Shows both avatars linked
- "You and [Name] are now connected on Looping"
- "Open Chat" button

**Reject:**
- Silent rejection (requester sees "Pending" until timeout)
- Request expires after 48 hours if not accepted

**Connected State:**
- Both users appear in each other's Contacts list
- Chat is unlocked
- Voice/Video call is unlocked

---

### F07 — Real-time 1:1 Chat

**Message Types:**
- Text messages (with emoji support, system keyboard)
- Image messages (camera + gallery, compressed before upload)
- Timestamp on each message

**Chat Features:**
- Real-time delivery (Firestore real-time listeners)
- Message states: Sent (single tick) → Delivered (double tick) → Read (blue ticks)
- "Typing..." indicator
- Chat date dividers ("Today", "Yesterday", date)
- Connected badge at top of conversation
- Scroll to bottom on new message
- Pull to load older messages (paginated, 30 messages per batch)

**Chat Header:**
- Back button
- User avatar with online indicator (green dot)
- User name
- "Online" / "Last seen [time]" status
- Voice call button
- More options button (mute, block, report)

**Chat Input Bar:**
- Text input field with placeholder
- Send button (enabled only when text is not empty)
- Attachment button (image picker)

**Data:**
- Messages stored in Firestore subcollection per chat
- Chat list ordered by last message timestamp
- Unread count badge on chat tab

---

### F08 — Voice Calling (1:1)

**Incoming Call Screen:**
- Full screen white background
- Large caller avatar with pulse animation
- Caller name
- "Incoming Voice Call..." status text
- Accept button (green, phone icon)
- Decline button (red, rotated phone icon)
- Ringtone plays (system default or custom)
- Vibration pattern

**Active Call Screen:**
- Caller avatar (no pulse)
- Caller name
- "Connected" status
- Live timer counting up (MM:SS format)
- 6-button control grid:
  - Mute (toggleable, shows active state)
  - Speaker (toggleable)
  - Keypad (opens DTMF dial pad)
  - Video (upgrade to video call)
  - Hold (toggleable)
  - Info (show caller profile card)
- End call button (large, red, centered at bottom)

**Ended Call Screen:**
- Card centered on screen
- Phone icon in colored circle
- "Call Ended" title
- "[Name] • [duration]" subtitle
- "Call Back" button (dark/primary)
- "Send Message" button (secondary)

**Technical:**
- Powered by Zegocloud / Agora SDK
- Calls initiated via FCM data message (wakes the app)
- iOS: CallKit integration (native call UI when app is backgrounded/killed)
- Android: ConnectionService (native incoming call screen)
- Audio routing: earpiece (default), speaker, Bluetooth headset
- Call quality: Adaptive bitrate based on network

**Call States:**
- Idle → Ringing (outgoing) / Incoming → Connected → Ended
- Timeout: 60 seconds ring, then auto-end with "No Answer"
- Missed call: Push notification + entry in call history

---

### F09 — Video Calling (1:1 with PiP)

**Incoming Call Screen:**
- Dark background (#1a1a1a) with gradient layers
- Subtle grid pattern overlay
- Top/bottom gradient darkening
- Self-preview camera PiP (top-right corner, 96x132px, rounded)
- "End-to-end encrypted" badge (top center)
- Large caller avatar (112px) with pulse animation
- Caller name (white, with text shadow)
- "Incoming Video Call" status
- "Looping • Video" branding badge with green dot
- Accept button (green, video camera icon)
- Decline button (red, rotated phone icon)
- Both buttons have text labels underneath
- "Tap to answer or decline" hint text at bottom

**Active Call Screen:**
- Remote user's video fills the screen
- Dark gradient overlay at top and bottom edges
- Self-view PiP (top-right, draggable in future phase)
- PiP shows local camera feed with "You" label
- Header overlay: Caller name + live timer
- Bottom control bar (frosted glass, rounded):
  - Flip camera (front/back toggle)
  - Mute microphone (toggleable)
  - End call (large red button, centered)
  - Camera off (toggleable, shows avatar when off)
  - Effects/filters placeholder
- Tap screen to show/hide controls

**Ended Call Screen:**
- White background
- Centered card (same pattern as voice ended)
- Video camera icon in purple gradient circle
- "Video Call Ended" title
- "[Name] • [duration]" subtitle
- "Call Back" button (primary)
- "Send Message" button (secondary)

**Technical:**
- Same SDK as voice (Zegocloud/Agora)
- Camera permissions requested on first use
- Default: Front camera
- Video resolution: 720p (adaptive)
- Bandwidth: Falls back to audio-only on poor network
- Background: Video pauses, audio continues (PiP on supported devices)

---

### F10 — Push Notifications

| Trigger | Notification Text | Action |
|---------|-------------------|--------|
| New connection request | "[Name] wants to connect with you" | Open request |
| Connection accepted | "[Name] accepted your request!" | Open chat |
| New chat message | "[Name]: [message preview]" | Open chat |
| Incoming voice call | "[Name] is calling..." | Show call screen |
| Incoming video call | "[Name] is video calling..." | Show call screen |
| Missed call | "Missed call from [Name]" | Open call history |
| Screenshot attempt (if enabled) | "[Name] tried to screenshot" | Open chat |

**Technical:**
- Firebase Cloud Messaging (FCM)
- Data messages for calls (high priority, wakes app)
- Notification messages for chat/connection
- Token refresh handling
- Topic-based for future broadcast notifications

---

### F11 — Screenshot & Screen Recording Protection

**Settings Screen (Screenshot Guard):**
- Header: "Screenshot Guard" / "Privacy Settings"
- Hero section: Shield icon with green checkmark
- Title: "Content Protection"
- Description text
- 3 toggle rows:
  - Block Screenshots (ON by default)
  - Block Recording (ON by default)
  - Notify on Attempt (ON by default)
- Warning badge: "All protection features are currently active"
- "What's Protected" section:
  - Chat Messages — Active
  - User Profiles — Active
  - Shared Media — Active

**Technical Implementation:**
- Android: `FLAG_SECURE` on protected activities (prevents screenshots & screen recording)
- iOS: `UITextField` secure text entry overlay trick + notification observer for screen capture
- Notification to other user if screenshot attempt is detected (iOS only — Android blocks entirely)
- Protection applies to: Chat screen, Profile screen, Media viewer

---

### F12 — Privacy Controls

- Toggle visibility for each profile field:
  - Name → Visible to nearby users (ON/OFF)
  - Phone Number → Only after connecting (ON/OFF)
  - Email → Visible on profile (ON/OFF)
  - Occupation → Shown in discovery (ON/OFF)
  - Education → College & school info (ON/OFF)
  - Social Accounts → Instagram, Facebook (ON/OFF)
- Settings persist in Firestore user document
- Affects what nearby users see in radar bubbles and mini-profiles

---

### F13 — DigiLocker Integration

- "Link DigiLocker" button in registration Step 4
- OAuth flow: Opens DigiLocker login → user authorizes → returns verified docs
- Documents verified via DigiLocker API:
  - Aadhaar Card
  - PAN Card
  - Driving License (future)
- App stores only: `is_verified: true`, `verified_at: timestamp`, `masked_id: "XXXX 4321"`
- App does NOT store raw Aadhaar numbers (Aadhaar Act compliance)
- Verification badge shown on user profile

---

### F14 — Social Account Linking

| Platform | Method | What We Get |
|----------|--------|-------------|
| Google | Firebase Google Sign-In | Email, name, avatar |
| Instagram | Instagram Basic Display API | Username, profile pic |
| Facebook | Facebook Login SDK | Name, profile pic, email |
| LinkedIn | LinkedIn OAuth 2.0 | Name, headline (optional, future) |

- Each linked account shows as a card: Icon, Platform name, Handle/email, Connected checkmark
- User can unlink at any time
- Linked accounts shown on profile (if privacy toggle is ON)

---

### F15 — Connection/Contact List

- List of all accepted connections
- Each row: Avatar, Name, Last message preview, Timestamp
- Online indicator (green dot)
- Search/filter connections by name
- Sorted by most recent interaction
- Swipe actions: Mute, Block, Delete connection
- Empty state: "No connections yet. Start scanning!"

---

### F16 — Call History Log

- List of all voice and video calls
- Each row: Avatar, Name, Call type icon (voice/video), Direction (incoming/outgoing/missed), Duration, Timestamp
- Missed calls highlighted in red
- Tap to call back
- Grouped by date

---

### F17 — Block / Report User

**Block:**
- Available from: Chat header menu, Profile view, Connection list
- Confirmation dialog: "Block [Name]? They won't be able to contact you or see you nearby."
- Blocked user: Removed from contacts, chat hidden, invisible in discovery
- Unblock available in Settings → Blocked Users

**Report:**
- Report reasons: Harassment, Fake profile, Spam, Inappropriate content, Other
- Optional text description
- Report stored in Firestore `reports` collection
- Reported user flagged for manual review (future admin dashboard)

---

### F18 — App Settings

- **Account:** Edit profile, Change password, Linked accounts
- **Privacy:** Field visibility, Screenshot guard, Discovery ON/OFF
- **Notifications:** Enable/disable per type (messages, calls, requests)
- **Blocked Users:** List with unblock option
- **About:** Version number, Terms, Privacy Policy, Licenses
- **Help & Support:** Email/link
- **Delete Account**
- **Logout**

---

### F19 — Delete Account

- Confirmation: "Are you sure? This action cannot be undone."
- Second confirmation: Type "DELETE" to confirm
- On delete:
  - Firebase Auth account deleted
  - Firestore user document marked as deleted
  - Profile removed from all connections' lists
  - Chat history retained for other user but shows "[Deleted User]"
  - All personal data purged within 30 days (DPDPA compliance)

---

### F20 — App Store & Play Store Deployment

- App icon (1024x1024)
- Splash screen (Looping branding)
- 5 screenshots per platform (from showcase designs)
- App description (short + long)
- Privacy policy URL (hosted)
- Play Store: Target API level compliance, content rating questionnaire
- App Store: App Review guidelines compliance, export compliance

---

## 4. SCREEN-BY-SCREEN BREAKDOWN

| # | Screen | Type |
|---|--------|------|
| S01 | Splash Screen | Static |
| S02 | Login | Form |
| S03 | Registration Step 1 — Profile | Form |
| S04 | Registration Step 2 — Contact & ID | Form |
| S05 | Registration Step 3 — Purpose & Education | Form |
| S06 | Registration Step 4 — Social & Documents | Form + OAuth |
| S07 | Registration Step 5 — Privacy & Terms | Form |
| S08 | Home (Dashboard) | Interactive |
| S09 | Nearby Scan / Radar | Animated + BLE |
| S10 | User Mini-Profile (Bottom Sheet) | Modal |
| S11 | Connection Request Sent (Overlay) | Modal |
| S12 | Connection Accepted | Static |
| S13 | Chat List | List |
| S14 | Chat Conversation | Real-time |
| S15 | Voice Call — Incoming | Full-screen |
| S16 | Voice Call — Active | Full-screen |
| S17 | Voice Call — Ended | Full-screen |
| S18 | Video Call — Incoming | Full-screen (dark) |
| S19 | Video Call — Active | Full-screen (dark) |
| S20 | Video Call — Ended | Full-screen |
| S21 | Screenshot Guard Settings | Form |
| S22 | User Profile (Own) | Detail |
| S23 | User Profile (Other) | Detail |
| S24 | Contacts / Connection List | List |
| S25 | Call History | List |
| S26 | Settings | List |
| S27 | Blocked Users | List |
| S28 | Edit Profile | Form |
| S29 | Notifications List | List |

**Total: 29 screens**

---

## 5. TECHNICAL ARCHITECTURE

```
┌─────────────────────────────────────────────┐
│                 FLUTTER APP                  │
│  ┌─────────┐ ┌──────────┐ ┌──────────────┐  │
│  │  UI      │ │  State   │ │  Services    │  │
│  │  Screens │ │  Riverpod│ │  BLE/Calls   │  │
│  └────┬─────┘ └────┬─────┘ └──────┬───────┘  │
│       └─────────────┼──────────────┘          │
│                     │                         │
│              ┌──────┴───────┐                 │
│              │  Repository  │                 │
│              │    Layer     │                 │
│              └──────┬───────┘                 │
└─────────────────────┼─────────────────────────┘
                      │
        ┌─────────────┼─────────────┐
        │             │             │
  ┌─────┴─────┐ ┌─────┴─────┐ ┌────┴──────┐
  │ Firebase   │ │ Zegocloud │ │ DigiLocker│
  │ Auth       │ │ Voice SDK │ │ API       │
  │ Firestore  │ │ Video SDK │ │           │
  │ Storage    │ │           │ │           │
  │ FCM        │ │           │ │           │
  └────────────┘ └───────────┘ └───────────┘
```

**Firestore Collections:**

```
users/
  {userId}/
    profile: { name, dob, occupation, ... }
    privacy: { nameVisible, phoneVisible, ... }
    verification: { isAadhaarVerified, isPanVerified, ... }
    socialLinks: { instagram, facebook, google, ... }
    settings: { notificationsEnabled, discoveryEnabled, ... }

connections/
  {connectionId}/
    users: [userId1, userId2]
    status: "pending" | "accepted" | "rejected"
    requestedBy: userId
    createdAt: timestamp

chats/
  {chatId}/
    participants: [userId1, userId2]
    lastMessage: { text, timestamp, senderId }
    messages/ (subcollection)
      {messageId}/
        text: string
        senderId: userId
        timestamp: timestamp
        status: "sent" | "delivered" | "read"
        type: "text" | "image"

calls/
  {callId}/
    caller: userId
    callee: userId
    type: "voice" | "video"
    status: "ringing" | "connected" | "ended" | "missed"
    startTime: timestamp
    endTime: timestamp
    duration: number (seconds)

reports/
  {reportId}/
    reportedUser: userId
    reportedBy: userId
    reason: string
    description: string
    timestamp: timestamp
```

---

## 6. DEVELOPMENT PHASES & MILESTONES

### PHASE 1: Foundation (Week 1–3)
| Week | Tasks | Deliverable |
|------|-------|-------------|
| 1 | Project setup, architecture, Firebase config, Auth (OTP + Email) | Login working |
| 2 | Registration Steps 1-3, Profile photo upload | 3-step registration |
| 3 | Registration Steps 4-5, Social OAuth, Privacy toggles | Full registration flow |

**Milestone 1 Demo:** User can register (5 steps), login, view profile ✓

---

### PHASE 2: Discovery & Chat (Week 4–7)
| Week | Tasks | Deliverable |
|------|-------|-------------|
| 4 | Home screen, Bottom navigation, BLE setup | Home + nav shell |
| 5 | BLE scanning, Radar UI, Nearby user bubbles | Radar discovers users |
| 6 | Connection requests (send/accept/reject), Push notifications | Users can connect |
| 7 | Real-time chat, Message states, Typing indicator, Image messages | Full chat working |

**Milestone 2 Demo:** Complete flow — Discover → Connect → Chat ✓

---

### PHASE 3: Voice & Video Calls (Week 8–11)
| Week | Tasks | Deliverable |
|------|-------|-------------|
| 8 | Zegocloud SDK setup, Voice call initiation, FCM call signaling | Voice calls ring |
| 9 | Voice call UI (incoming/active/ended), Controls, CallKit/ConnectionService | Voice calls complete |
| 10 | Video call UI (incoming/active/ended), Camera, PiP | Video calls working |
| 11 | Call history, Missed call notifications, Edge cases, Network fallback | Calls polished |

**Milestone 3 Demo:** Full voice + video calling between users ✓

---

### PHASE 4: Polish & Launch (Week 12–14)
| Week | Tasks | Deliverable |
|------|-------|-------------|
| 12 | DigiLocker integration, Screenshot protection, Block/Report | Security features |
| 13 | Settings, Contact list, UI polish, Animation matching showcase | Feature complete |
| 14 | Device testing (5+ Android, 2+ iOS), Bug fixes, Store submissions | **APP LAUNCHED** |

**Milestone 4:** App live on Play Store + App Store ✓

---

## 7. BUDGET BREAKDOWN

### Development & Services

| Item | Cost (INR) |
|------|-----------|
| Apple Developer Account (1 year) | 8,700 |
| Google Play Developer Account (one-time) | 2,100 |
| Zegocloud / Agora (voice+video SDK, 6 months) | 15,000 |
| Firebase Blaze Plan (6 months buffer) | 10,000 |
| Domain for deep links + privacy policy hosting | 1,500 |
| Privacy Policy (lawyer-drafted, India-compliant) | 25,000 |
| App Store screenshots & assets design | 10,000 |
| Testing (BrowserStack / physical devices) | 5,000 |
| Miscellaneous (icons, fonts, certificates) | 5,000 |
| **Subtotal — Services & Tools** | **82,300** |

### Development

| Item | Cost (INR) |
|------|-----------|
| Development (14 weeks) | 1,47,700 |
| Code review + architecture | Included |
| Bug fixes during development | Included |
| **Subtotal — Development** | **1,47,700** |

### Contingency

| Item | Cost (INR) |
|------|-----------|
| Contingency buffer (scope adjustments, unforeseen) | 20,000 |

### TOTAL

| | INR |
|-|-----|
| **Grand Total** | **2,50,000** |

---

## 8. ASSUMPTIONS & DEPENDENCIES

### Assumptions
1. Client provides all branding assets (logo, colors, fonts) — already available from design system
2. Client provides content for Terms & Conditions and Privacy Policy text
3. Client handles DigiLocker API partnership application (developer provides integration)
4. One round of design revision per screen (major redesigns out of scope)
5. Testing on: Android 10+ (5 devices), iOS 15+ (2 devices)
6. App Store review may take 1-7 days (not included in 14-week timeline)
7. Voice/Video SDK free tier is sufficient for initial launch (<10,000 minutes/month)

### Dependencies
1. Firebase project created and billing enabled (Blaze plan)
2. Apple Developer Account active before Week 12
3. Google Play Developer Account active before Week 14
4. DigiLocker API access approved (apply 4-6 weeks before needed)
5. Social OAuth apps created (Instagram, Facebook, Google developer consoles)

### Legal Compliance
1. App will NOT store raw Aadhaar numbers (Aadhaar Act 2016 Section 29)
2. All personal data encrypted at rest (Firestore) and in transit (TLS 1.3)
3. DPDPA 2023 compliant: Consent collection, right to erasure, data minimization
4. Privacy Policy and Terms of Service must be reviewed by legal counsel
5. Grievance Officer details required in app (DPDPA mandate)

---

## 9. POST-LAUNCH SUPPORT

### Included (First 30 days after launch)
- Critical bug fixes
- App Store / Play Store rejection fixes
- Performance issues on supported devices

### Not Included (Quoted separately)
- New feature development
- Server/infrastructure maintenance beyond free tier
- Design changes or redesigns
- Admin dashboard
- Analytics integration
- Monthly maintenance retainer: Estimated ₹15,000-25,000/month

---

## 10. SIGN-OFF

### Agreed Scope
- 20 features as listed in Section 2 (F01–F20)
- 29 screens as listed in Section 4 (S01–S29)
- Flutter app for iOS and Android
- Firebase backend
- Voice and Video calling via Zegocloud/Agora
- 14-week delivery timeline
- ₹2,50,000 total budget

### Excluded (as agreed)
- 10 features as listed in Section 2 (X01–X10)
- Any feature not explicitly listed above
- Post-30-day support and maintenance
- Server costs beyond free tier limits

---

**Client Name:** ___________________________

**Client Signature:** ___________________________

**Date:** ___________________________

---

**Developer Name:** ___________________________

**Developer Signature:** ___________________________

**Date:** ___________________________

---

*This document serves as the agreed scope of work. Any changes to features, timeline, or budget must be documented in a separate Change Request and agreed by both parties.*
