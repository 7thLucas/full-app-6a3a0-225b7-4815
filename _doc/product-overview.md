# Product Overview — PayFlow

## What It Is
A minimal freelancer payment and billing tool. Freelancers create a payment request (amount + description), share or trigger a Razorpay UPI checkout, and see an instant success/failure result — with every transaction saved locally. No backend required in the MVP. No wallet, no marketplace, no super-app logic.

## What It Is NOT
- Not a wallet or balance system
- Not a marketplace or escrow platform
- Not a multi-gateway routing engine
- No fake cashback, fake balance, or reward mechanics
- Strictly: create request → Razorpay UPI checkout → result → local save

## The Problem It Solves
Freelancers waste time and lose professionalism collecting payments:
- No simple tool to generate a payment request on the spot
- Clients pay via UPI but freelancers have no record except bank SMS
- Chasing payments means manual follow-up with no status visibility
- Professional invoicing tools are overkill for quick one-off project payments

## The Solution
Three screens and one button. Freelancer enters amount + service description → taps Pay → Razorpay handles the UPI checkout → result screen confirms success or failure with Payment ID, and the transaction is saved locally for future reference. Operational in under a minute.

## User / Persona
**Freelancer**: a solo professional (designer, developer, writer, consultant) who does project-based or gig work and needs to collect client payments quickly via UPI — without signing up for a full invoicing suite or banking portal.
- Primary need: collect payment fast, look professional doing it
- Secondary need: have a local record of what was paid, when, for what

## Screens & User Flow
**Home Screen → Payment Screen → Result Screen**

### Home Screen
- Clean freelancer dashboard
- Two primary actions: "Create Payment Request" and "Quick UPI Payment"
- Recent transactions list (local storage): amount, description (service name), status
- Minimal, card-based layout

### Payment Screen
- Amount input (required)
- Description / service name input (optional)
- "Pay / Generate Payment" button
- On tap: opens Razorpay Checkout
- Amount converted to paise (amount × 100) before passing to Razorpay
- Prefill contact and email optional
- Uses Razorpay Key ID (placeholder in MVP)

### Result Screen
- Payment status: Success (green) / Failed (red)
- If success: display Payment ID, amount, description
- Simple centered layout, no extra features
- On success: transaction saved locally before navigation

## Razorpay Integration
- Package: `razorpay_flutter` (Flutter) / Razorpay Web Checkout SDK (web build)
- Events handled: `PAYMENT_SUCCESS`, `PAYMENT_ERROR`
- On success: save transaction to local storage → navigate to ResultScreen
- On failure: navigate to ResultScreen with failed status
- Key ID: placeholder in MVP (real key injected via env variable)

## Local Data Storage
Each transaction record stores:
- `amount` (numeric)
- `description` (service name string)
- `paymentId` (Razorpay Payment ID, from success event)
- `status` ("success" | "failed")
- `timestamp`

No backend required in MVP. No sync, no cloud, no auth.

## Architecture (Flutter / Web-equivalent)
```
lib/main.dart
screens/
  home_screen.dart
  payment_screen.dart
  result_screen.dart
services/
  razorpay_service.dart
models/
  transaction_model.dart
widgets/
  transaction_tile.dart
```

## Brand
- **Name**: PayFlow
- **Tone**: Fast, clean, no-nonsense — built for the person who works alone and moves quickly
- **Visual direction**: Minimal fintech; white surfaces, card-based, indigo primary (#4F46E5), green success (#22C55E), red failure (#EF4444), slate text
- **UI principle**: No heavy animations, fast performance, mobile-first

## Scope (MVP — strict)
- Home screen with recent transactions from localStorage
- Payment screen with amount + description inputs → Razorpay checkout
- Result screen with success/failure state + Payment ID on success
- Local transaction persistence (localStorage / shared_preferences)
- Error-free runnable app, production-ready file structure

## Scalability (Future Phases)
Architecture must support future upgrades without refactoring:
1. Invoice PDF generation
2. Client management system
3. Cloud sync backend
4. Payment analytics dashboard

## Key Reference Numbers (for North Star projection)
- Active freelancer sends (assumption chip): 10 payment requests/week
- Average project payment (assumption chip): ₹5,000 per request
- Time saved per payment vs. manual bank coordination: ~15 min
- Freelancer's effective hourly rate (assumption chip): ₹500/hr
