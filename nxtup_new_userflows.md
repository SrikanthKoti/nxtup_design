# Nxtup Social — New User Flows (Events · Community · Chats)

## 0. Master App Flow (Landing-centric, pre/post-auth)
```mermaid
flowchart TD
  A[Landing Page] --> B{Authenticated?}
  B -- No --> L1[Landing: Public View]
  B -- Yes --> L2[Landing: Logged-in View]

  %% Primary CTAs on Landing (both views)
  L1 --> CTA1[CTA: Sign Up / Log In]
  L1 --> NAV1[Explore: Community (readonly)]
  L1 --> NAV2[Explore: Events (readonly)]
  L2 --> NAV3[Go to Community]
  L2 --> NAV4[Go to Events]
  L2 --> NAV5[Go to Chats]

  %% Auth -> returns to Landing
  CTA1 --> AUTH[Auth Modal / Page]
  AUTH --> AUTH2{Success?}
  AUTH2 -- Yes --> ONB0[Onboarding Modal]
  AUTH2 -- No --> L1

  %% Onboarding always on Landing (modal)
  ONB0 --> ONB_skip[All steps skippable]
  ONB0 --> ONB1[Step 1: Location → Auto-detect or Manual]
  ONB1 --> ONB2[Step 2: Community Preselect by Location]
  ONB2 --> ONB3[Step 3: Username (choose or regenerate suggestion)]
  ONB3 --> ONB4[Step 4: Notifications (optional)]
  ONB4 --> L2
  ONB_skip --> L2

  %% From logged-in Landing, user navigates
  L2 --> HUB{Where to?}
  HUB -- Community --> C0[Community Hub]
  HUB -- Events --> E0[Events]
  HUB -- Chats --> CH0[Chats]
```

---

## 1. Authentication & Onboarding (on Landing, modal)
```mermaid
flowchart TD
  A0[Auth Trigger from Landing] --> A1{Mode?}
  A1 -- Sign Up --> SU[Sign Up: Email/Password or Google]
  A1 -- Log In --> LI[Log In: Email/Password or Google]
  SU --> A2{Success?}
  LI --> A2
  A2 -- No --> A3[Inline Error → Retry]
  A2 -- Yes --> OM0[Onboarding Modal on Landing]

  %% Onboarding steps (skippable)
  OM0 --> S1[Location: Allow GPS?]
  S1 --> S1Y[Yes → Detect & Save] --> S2
  S1 --> S1N[No → Manual Entry → Save] --> S2

  S2[Community Preselect] --> S2a[Auto-select based on location]
  S2a --> S2b[Option: Change Community via picker]
  S2b --> S3

  S3[Username] --> S3a[Enter custom username → Check availability]
  S3 --> S3b[Use system-suggested random username]
  S3b --> S3c[Regenerate suggestion (optional)]
  S3a --> S3d{Available?}
  S3d -- No --> S3e[Show taken → Suggest variants]
  S3d -- Yes --> S4

  S4[Notifications (optional)] --> S5[Finish → Close Modal]
  S5 --> LND[Return to Landing (logged-in state)]
```

---

## 2. Community (Location-based, no user-created groups)
```mermaid
flowchart TD
  C0[Community Hub] --> C1[Selected Community: e.g., Vishakhapatnam District]
  C0 --> C2[Community Picker: Browse by State → Districts]
  C2 --> C1

  %% All section
  C0 --> ALL[All (cross-community): newest questions]

  %% Community Content
  C1 --> QL[Questions List (in this community)]
  QL --> QD[Open Question Detail]
  C1 --> ASK[Ask a Question]
  ASK --> AQ1[Enter Question Text]
  AQ1 --> AQ2[Add '#' tag(s)]
  AQ2 --> AQ2a[Select existing '#' from community]
  AQ2 --> AQ2b[Create new '#' → added to community index]
  AQ2a --> AQ3[Post Question → Link to community]
  AQ2b --> AQ3
  AQ3 --> QL

  %% Hashtag filtering + trending
  C1 --> HT[Hashtag Filter]
  HT --> HT1[Filter Questions by selected '#']
  C1 --> TR[Trending (top '#'/activity)]
  TR --> QL

  %% Cross-navigation
  QD --> ACT{Actions}
  ACT -- Reply/Answer --> ANS[Add Answer]
  ACT -- Share --> SH[System Share Sheet]
  ACT -- Report --> RP[Report → Submit]
```

---

## 3. Events (re-using existing event logic)
```mermaid
flowchart TD
  E0[Events] --> E1{Sub-Tab?}
  E1 -- Upcoming --> E2[Upcoming List + Filters (date/price/distance/online)]
  E1 -- My Events --> E3[Registered / Hosting]
  E1 -- Past --> E4[Past Events]

  %% Event Detail & RSVP
  E2 -->|Tap Card| ED[Event Detail]
  E3 -->|Tap Card| ED
  E4 -->|Tap Card| ED

  ED --> ED1[Banner, Title, Date/Time]
  ED --> ED2[Location/Map or Online Link]
  ED --> ED3[Description + Host Profile Link]
  ED --> ED4[Attendees Preview + Slots]
  ED --> ED5[CTA: RSVP / Register]

  ED5 --> PAY{Free or Paid?}
  PAY -- Free --> R1[Confirm RSVP]
  PAY -- Paid --> P1{Payment Path}
  P1 -- External --> P2[Open Browser → Pay → Return]
  P1 -- In-app --> P3[Checkout → Status]

  R1 --> RS{Success?}
  P2 --> RS
  P3 --> RS
  RS -- Yes --> R2[Add to My Events + Enable Reminders + Toast]
  RS -- No --> R3[Error/Cancelled → Stay on Detail]

  %% Capacity / Approval / Chat
  ED --> CAP{Capacity Full?}
  CAP -- Yes --> WL[Join Waitlist → Notify on open]
  ED --> APR{Approval Required?}
  APR -- Yes --> A1[Request to Join → Pending → Host Decision]
  A1 --> A2{Approved?}
  A2 -- Yes --> R2
  A2 -- No --> A3[Notify Denied]

  R2 --> EC[Auto-create Event Chat (host + attendees)]
```

---

## 4. Chats (request-based DM)
```mermaid
flowchart TD
  CH0[Chats] --> CH1[Conversation List + Unreads]
  CH0 --> CHREQ[Chat Requests]
  CH0 --> NEW[Start New Chat]

  %% New Chat
  NEW --> SRCH[Search Users]
  SRCH --> SEL[Select User]
  SEL --> REQ[Send Chat Request (optional intro message)]
  REQ --> CHREQ

  %% Requests
  CHREQ --> RCV[Incoming Request → View profile preview]
  RCV --> ACC[Accept] --> TH[Open Thread]
  RCV --> REJ[Reject] --> CHREQ
  RCV --> BLK[Block/Report] --> CHREQ

  %% Thread
  TH --> MSG[Send Message (text, image/file/voice)]
  MSG --> IND[Delivered/Read Indicators]
  TH --> OPT[Thread Options: Mute, Leave, Report/Block]
```

---

## 5. Landing Page (states & navigation)
```mermaid
flowchart TD
  LP0[Landing: Public] --> H1[Hero: “Find your local community” + CTA]
  LP0 --> SEC1[Sections: How it works, Communities, Events Preview]
  LP0 --> AUTHBTN[Sign Up / Log In]
  LP0 --> BROWSE1[Browse Communities (readonly)]
  LP0 --> BROWSE2[Browse Events (readonly)]
  AUTHBTN --> AUTHPAGE[Auth Page/Modal] --> OM[Onboarding Modal] --> LP1

  LP1[Landing: Logged-in] --> PERS[Personalized panels: Selected Community feed, Trending #, Upcoming Events]
  LP1 --> QUICK[Quick actions: Ask Question, Browse Communities, Go to Events, Open Chats]
  LP1 --> HEADNAV[Top Nav: Community | Events | Chats | Profile/Settings]
```

---

## 6. Profile & Settings (lightweight)
```mermaid
flowchart TD
  PR0[Profile/Account] --> PR1[View: avatar, display name, username]
  PR0 --> PR2[Edit: username (check availability), display name, bio]
  PR0 --> PR3[Settings: Notifications, Privacy, Account Links]
  PR0 --> PR4[Help & Support]
  PR0 --> PR5[Logout]
```
