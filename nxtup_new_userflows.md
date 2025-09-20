# Nxtup Social — Updated User Flow (Events, Community, Chats)

Paste this into any Mermaid-compatible viewer (Mermaid Live, VS Code Mermaid extension, Notion, Obsidian) or render via CI to export SVG/PNG.

---

## 1. Landing Page and Onboarding

```mermaid
flowchart TD
  A[App Launch - Landing Page]

  %% Auth Entry
  A --> B{Authenticated?}
  B -- No --> C[Popup Login or Sign Up]
  B -- Yes --> L[Landing Page Logged In]

  %% Login and Signup
  C --> C1[Login - Email and Password or Google]
  C --> C2[Sign Up - Email and Password or Google]
  C --> C3[Forgot Password - Reset Flow]

  %% After Signup - Onboarding popup
  C2 --> O1[Onboarding Popup - Skippable Steps]
  O1 --> O2[Step 1 Locality Detection - GPS or Manual]
  O2 --> O3[Step 2 Profile Setup - Username Custom or Suggested]
  O3 --> O4[Step 3 Interests and Hashtags Selection]
  O4 --> L

  %% Login success
  C1 --> L

  %% Landing Page logged in
  L --> NAV[Navigation Tabs - Community, Events, Chats, Profile or Settings]
```

---

## 2. Community Feed

```mermaid
flowchart TD
  L --> CF[Community Feed]

  %% Feed Logic
  CF --> CF1[Default - Posts in Current Location]
  CF1 --> CF2{Posts Available?}
  CF2 -- Yes --> CF3[Infinite Scroll]
  CF2 -- No --> CF4[Fetch Nearby Locations]

  %% Global Feed
  CF --> GF[Global Feed Tab - All Locations]

  %% Posting
  CF --> P1[Create Post - Must Include Hashtag]
  P1 --> P2[Choose Location - Current or Other]
  P2 --> P3[Post Submitted - Appears in Feed]

  %% Hashtags
  CF --> H1[Filter or Search by Hashtag]
  CF --> H2[Trending Topics by Hashtag]

  %% Interactions
  CF --> I1[Like, Comment, Share]
  CF --> I2[Report, Hide, Block]
```

---

## 3. Events

```mermaid
flowchart TD
  NAV --> EV[Events Tab]
  EV --> E1{Sub Tab?}
  E1 -- Upcoming --> E2[Upcoming Events List]
  E1 -- My Events --> E3[My Registered or Hosting]
  E1 -- Past Events --> E4[Past Events]

  %% Event Detail
  E2 --> ED[Event Detail]
  E3 --> ED
  E4 --> ED

  ED --> ED1[Banner, Title, Date and Time, Host Info]
  ED --> ED2[Location Map or Online Link]
  ED --> ED3[Description and Attendees Preview]
  ED --> ED4[CTA RSVP or Register]

  %% RSVP or Register
  ED4 --> R1{Free or Paid?}
  R1 -- Free --> R2[Confirm RSVP]
  R1 -- Paid --> R3[Payment Flow]

  R2 --> RS[Success - Add to My Events, Reminder, Event Chat]
  R3 --> RS
  RS --> CH1[Auto Create Event Chat]
```

---

## 4. Chats

```mermaid
flowchart TD
  NAV --> CH[Chats Tab]
  CH --> CHL[Chat List and Requests]

  %% Requests
  CHL --> RQ1[Send Chat Request - optional message]
  RQ1 --> RQ2[Receiver Accept or Reject]
  RQ2 -- Accept --> CT[Open Chat Thread]
  RQ2 -- Reject --> RJ[Request Declined]

  %% Chat Thread
  CT --> M1[Send Message - Text, Media, Voice]
  CT --> M2[Delivered and Read Indicators]
  CT --> M3[Thread Actions - Mute, Leave, Report, Block]
```

---

## 5. Profile and Settings

```mermaid
flowchart TD
  NAV --> PR[Profile and Settings]
  PR --> P1[View and Edit Profile - Avatar, Bio, Interests, Username]
  PR --> P2[Settings - Account, Notifications, Privacy]
  PR --> P3[Help and Support]
  PR --> P4[Logout - Confirm and Clear Session - Back to Landing Page]
```
