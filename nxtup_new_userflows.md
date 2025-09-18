# Nxtup Social — New User Flows (Events · Community · Chats) — GitHub‑Safe Mermaid

## 0. Master App Flow (Landing-centric, pre/post-auth)
```mermaid
flowchart TD
  A[Landing Page] --> B{Authenticated?}
  B -- No --> L1[Landing - Public]
  B -- Yes --> L2[Landing - Logged in]

  L1 --> CTA1[CTA - Sign Up or Log In]
  L1 --> NAV1[Explore Community - readonly]
  L1 --> NAV2[Explore Events - readonly]
  L2 --> NAV3[Go to Community]
  L2 --> NAV4[Go to Events]
  L2 --> NAV5[Go to Chats]

  CTA1 --> AUTH[Auth - modal or page]
  AUTH --> AUTH2{Success?}
  AUTH2 -- Yes --> ONB0[Onboarding - modal]
  AUTH2 -- No --> L1

  ONB0 --> ONB_SKIP[All steps skippable]
  ONB0 --> ONB1[Step 1 - Location - auto or manual]
  ONB1 --> ONB2[Step 2 - Community preselect by location]
  ONB2 --> ONB3[Step 3 - Username choose or regenerate]
  ONB3 --> ONB4[Step 4 - Notifications optional]
  ONB4 --> L2
  ONB_SKIP --> L2

  L2 --> HUB{Where to?}
  HUB -- Community --> C0[Community Hub]
  HUB -- Events --> E0[Events]
  HUB -- Chats --> CH0[Chats]
```
---

## 1. Authentication and Onboarding (on Landing, modal)
```mermaid
flowchart TD
  A0[Auth trigger from Landing] --> A1{Mode?}
  A1 -- Sign Up --> SU[Sign Up - Email-Pw or Google]
  A1 -- Log In --> LI[Log In - Email-Pw or Google]
  SU --> A2{Success?}
  LI --> A2
  A2 -- No --> A3[Inline error - retry]
  A2 -- Yes --> OM0[Onboarding modal on Landing]

  OM0 --> S1[Location - allow GPS?]
  S1 --> S1Y[Yes - detect and save]
  S1 --> S1N[No - manual entry and save]
  S1Y --> S2[Community preselect]
  S1N --> S2

  S2 --> S2A[Auto select by location]
  S2A --> S2B[Option - change via picker]
  S2B --> S3[Username]

  S3 --> S3A[Enter custom username - check availability]
  S3 --> S3B[Use system suggested username]
  S3B --> S3C[Regenerate suggestion]
  S3A --> S3D{Available?}
  S3D -- No --> S3E[Show taken - suggest variants]
  S3D -- Yes --> S4[Notifications]

  S4 --> S5[Finish - close modal]
  S5 --> LND[Return to Landing - logged in]
```
---

## 2. Community (location-based, no user-created groups)
```mermaid
flowchart TD
  C0[Community Hub] --> C1[Selected Community]
  C0 --> C2[Community Picker - State to Districts]
  C2 --> C1

  C0 --> ALL[All - newest questions across communities]

  C1 --> QL[Questions list]
  QL --> QD[Question detail]
  C1 --> ASK[Ask a question]
  ASK --> AQ1[Enter question text]
  AQ1 --> AQ2[Add hashtag tags]
  AQ2 --> AQ2A[Select existing hashtag]
  AQ2 --> AQ2B[Create new hashtag - add to index]
  AQ2A --> AQ3[Post question - link to community]
  AQ2B --> AQ3
  AQ3 --> QL

  C1 --> HT[Hashtag filter]
  HT --> HT1[Filter by selected hashtag]
  C1 --> TR[Trending - top hashtags and activity]
  TR --> QL

  QD --> ACT{Actions}
  ACT -- Reply --> ANS[Add answer]
  ACT -- Share --> SH[Share]
  ACT -- Report --> RP[Report]
```
---

## 3. Events (reuse current event logic)
```mermaid
flowchart TD
  E0[Events] --> E1{Sub tab?}
  E1 -- Upcoming --> E2[Upcoming list with filters]
  E1 -- My Events --> E3[Registered or Hosting]
  E1 -- Past --> E4[Past events]

  E2 --> ED[Event detail]
  E3 --> ED
  E4 --> ED

  ED --> ED1[Banner, title, date time]
  ED --> ED2[Location map or online link]
  ED --> ED3[Description and host]
  ED --> ED4[Attendees preview and slots]
  ED --> ED5[CTA - RSVP or Register]

  ED5 --> PAY{Free or Paid?}
  PAY -- Free --> R1[Confirm RSVP]
  PAY -- Paid --> P1{Payment path}
  P1 -- External --> P2[Open browser and pay and return]
  P1 -- In app --> P3[Checkout and status]

  R1 --> RS{Success?}
  P2 --> RS
  P3 --> RS
  RS -- Yes --> R2[Add to My Events and reminders]
  RS -- No --> R3[Error or cancelled]

  ED --> CAP{Capacity full?}
  CAP -- Yes --> WL[Join waitlist and notify]
  ED --> APR{Approval required?}
  APR -- Yes --> A1[Request to join - pending]
  A1 --> A2{Approved?}
  A2 -- Yes --> R2
  A2 -- No --> A3[Notify denied]

  R2 --> EC[Auto create Event Chat]
```
---

## 4. Chats (request-based DM)
```mermaid
flowchart TD
  CH0[Chats] --> CH1[Conversation list and unreads]
  CH0 --> CHREQ[Chat requests]
  CH0 --> NEW[Start new chat]

  NEW --> SRCH[Search users]
  SRCH --> SEL[Select user]
  SEL --> REQ[Send chat request - optional intro]
  REQ --> CHREQ

  CHREQ --> RCV[Incoming request - view profile]
  RCV --> ACC[Accept]
  RCV --> REJ[Reject]
  RCV --> BLK[Block or Report]
  ACC --> TH[Open thread]
  REJ --> CHREQ
  BLK --> CHREQ

  TH --> MSG[Send text image file voice]
  MSG --> IND[Delivered and read indicators]
  TH --> OPT[Mute Leave Report Block]
```
---

## 5. Landing Page (states and navigation)
```mermaid
flowchart TD
  LP0[Landing - Public] --> H1[Hero - Find your local community]
  LP0 --> SEC1[How it works and previews]
  LP0 --> AUTHBTN[Sign Up or Log In]
  LP0 --> BROWSE1[Browse communities - readonly]
  LP0 --> BROWSE2[Browse events - readonly]
  AUTHBTN --> AUTHPAGE[Auth] 
  AUTHPAGE --> OM[Onboarding modal] 
  OM --> LP1[Landing - Logged in]

  LP1 --> PERS[Panels - Community feed, Trending, Upcoming events]
  LP1 --> QUICK[Quick actions - Ask, Browse, Events, Chats]
  LP1 --> HEADNAV[Top nav - Community, Events, Chats, Profile]
```
---

## 6. Profile and Settings (lightweight)
```mermaid
flowchart TD
  PR0[Profile and Account] --> PR1[View avatar display name username]
  PR0 --> PR2[Edit username check availability]
  PR0 --> PR3[Settings notifications privacy links]
  PR0 --> PR4[Help and Support]
  PR0 --> PR5[Logout]
```
