# Cozzah's Bank System - Complete Feature List

## Core E2 Chips

### 1. Bank Security E2 (`cozzah's_bank_security_v1.2.txt`)

#### Main Features:
- Multi-zone security system with customizable detection areas (Min1/Max1, Min2/Max2)
- Floor detection system (3 modes: single-floor, bottom-floor customers, top-floor customers)
- Customer door assignment system with individual door access
- Guard-accessible doors (shared access for all guards)
- Lockpick detection with automatic alarm triggering
- Panic button system with 3-second-press activation
- Timed customer access (5 minutes with keypad code entry)
- Automatic alarm with sound + visual indicators
- Door color coding (red=locked, green=customer nearby/authorized)
- Storage doors auto-close after 10 seconds if the owner is in Min1/Max1 area, else act as a toggle door for raids

#### Personnel Management:
- Guard system (max 4 guards, 0% tax, automatic salary every 10 minutes)
- Guest system (temporary or permanent access)
- Customer assignment system (per-door authorisation)
- Automatic job validation (Security Guard/Mercenary roles only)
- Auto-removal when changing from authorised jobs

#### Chat Commands - Admin (`*` prefix):
- `*help` - Show admin commands
- `*resetpanic` - Reset panic buttons and close toggle doors
- `*add [player]` - Add guard
- `*remove [player]` - Remove guard (gets paid severance pay unless changed job)
- `*guards` - List all guards
- `*set [amount]` - Set guard salary (minimum $50k/h)
- `*update` - Manually refresh guard screen
- `*adddoor [player]` - Assign door to player (look at door first)
- `*deldoor` - Remove door assignment (look at door)
- `*doors` - List all door assignments
- `*addguarddoor` - Make door accessible to all guards (look at door)
- `*delguarddoor` - Remove guard access from door (look at door)

#### Chat Commands - Player (`!` prefix):
- `!help` - Show player commands
- `!add [player]` - Add guest
- `!add [player] [minutes]` - Add timed guest
- `!remove [player]` - Remove guest
- `!guests` - List guests with time remaining
- `!code` - Resend keypad code (for assigned customers)

#### Additional Features:
- Remote table sharing with other E2s
- Door auto-close system (10 seconds)
- Raid mode toggle doors
- Visual EGP status display (SAFE/RAID/ALLOWED)
- Configurable alarm sound and materials
- Automatic guard payment notifications
- Door open/close sound effects

---

### 2. Banker Main E2 (`cozzah's_banker_e2_main_V1.2_pending_payouts.txt`)

#### Main Features:
- Automatic printer/rack collection system
- Smart payout batching (groups collections within 5 seconds to reduce spam)
- Per-rack tax tracking and statistics
- Automatic rack owner assignment with optional manual owner change command
- Customer notification system
- HUD position customisation (HUDX/HUDY variables)

#### Collection System:
Real-time collection display HUD showing:
- Customer name & rack type (RACK/TOWER)
- Membership tier with colour coding
- Total tax collected per rack
- Collection amount preview
- Automatic tax calculation based on membership tier
- Guard exemption (0% tax for guards)
- Smart batching: Single payment message for multiple rack collections
- Payment shows: total profit, tax amount, percentage, rack count

#### Administration:
- Bank closing notification system with countdown
- Customer list with tier display and tax totals
- Total profit summary (all racks combined)
- Manual wire transfer system
- Raid notification system (alerts all customers)
- Rack owner override system

#### Chat Commands (`*` prefix):
- `*help` - Show admin commands
- `*close [minutes] [notification_count]` - Send closing warnings
- `*customers` - List all customers with tiers and tax totals
- `*profit` - Show total tax profit summary
- `*wire [player] [amount]` - Send money to player (min $1,000)
- `*owner [player]` - Assign player to aimed rack
- `*raided` - Notify all customers of raid (insurance trigger)

#### Additional Features:
- Remote table synchronisation with Security & Membership E2s
- 10-minute collection reminder timer
- Automatic customer tier recognition
- Tax total accumulation per rack

---

### 3. Membership E2 (`cozzah's_bank_membership_e2.txt`)

#### Main Features:
- Permanent membership tier system (survives server restarts)
- File-based persistence (`banker_memberdata.txt`)
- 5 Membership tiers: Copper (free), Silver, Gold, Diamond, Ruby (ruby can only be given via chat command and has a 1% tax, used to give to good friends)
- Interactive EGP purchase interface
- Remote table broadcasting to other E2s
- Tier upgrade system (no downgrades allowed)

#### Membership Tiers:
| Tier | Tax | Insurance | Cost |
|------|-----|-----------|------|
| **Copper** (Default) | 35% | 0% | FREE |
| **Silver** | 25% | 50% | $1M |
| **Gold** | 20% | 75% | $2M |
| **Diamond** | 10% | 100% | $5M |
| **Ruby** | 1% | 100% | Admin-only |

#### Chat Commands (`*` prefix):
- `*give [player] [tier_number]` - Assign tier (1-4: Silver/Gold/Diamond/Ruby)

#### Additional Features:
- Visual tier cards with icons (Bar for lower tiers, diamond for top tier)
- Animated UI transitions
- Automatic refund on invalid payments
- One-time permanent purchase (no subscriptions)
- Already-purchased tier prevention
- Remote E2 initialisation system

---

### 4. Deposit E2 (`cozzah's_bank_depo_e2.txt`)

#### Main Features:
- Interactive deposit station with instructions
- Automatic door control during deposit
- Membership tier display on entry
- Distance-based auto-exit (250 units)
- Comprehensive instruction screen

#### Instruction Screen Includes:
- Deposit process (rack requirement warning)
- Collection explanation
- Customer access details (keypad, door colours, 5-minute timer)
- Storage rules
- Team-sized slot information

#### Additional Features:
- Animated door opening
- Visual membership card display with tier colours
- Auto-reset on user exit
- Remote membership data synchronisation
- "I UNDERSTAND" confirmation button
- Dynamic tier icon display (bar/diamond based on tier)

---

### 5. Printer EGP Display E2 (`printer_egp_auto_cop_pay_set_mass_combo.txt`)

#### Main Features:
- Animated printer rack display with collection animation
- Command-payment system for law enforcement in area if they help stop raid
- Prop mass & collision configuration
- RGB colour cycling system (8 props)
- Bank advertisement system using an array of adverts

#### Visual Features (Does nothing, just cool to look at):
- Real-time stats display (storage, temperature, profits, cooler rating)
- Animated collection sequence (7-step animation)
- Money handle animation
- Temperature gauge with colour changes
- Loading animation with spinner
- Total bank profit counter

#### Automation:
- Simulated printing (adds $700-720 every 3 seconds)
- Temperature simulation (rises randomly, cools at 100°C)
- Storage capacity bar (0-$300k)
- Auto-collection trigger at $250k-300k

#### Law Enforcement System:
- `*cops` command: Pay $100k to all nearby law enforcement
- Job detection array: Mayor, Police, SWAT, Secret Service, Custom Police
- 1000-unit radius detection

#### Advertisement System:
- 33 unique bank advertisement messages
- Auto-sends every 5 minutes (configurable: MinAdvert variable)
- GPT-generated banking taglines for fun

#### Additional Commands:
- `*setstate` - Refresh prop mass/collision settings (3000 mass, collision groups 20)
- Supports 12 props via inputs (Prop01-Prop12)

---

### 6. Prop Grayscale Cycling E2 (`bank_prop_grayscale_cycling.txt`)

#### Features:
- Grayscale colour cycling effect (black to light grey)
- Alarm override (red flashing when alarm active)
- Material switching based on button input for privacy screen
- Multi-model support (hunter plates, fence models)
- 2000-unit radius prop detection
- Special lightbar detection (preserves `cssconvertedmats/lightbareon01`)

#### Colour System:
- **Normal:** Smooth grayscale cycling (0-200 brightness value)
- **Alarm:** Red strobe effect (255,0,0 flashing)
- **Fence exception:** Red and White strobe at 255 alpha during alarm

#### Material System:
- **Button ON:** `cssconvertedmats/nuke_metalgrate_01`
- **Button OFF:** `cssconvertedmats/train_largemetal_door_01`
- **All other props:** `lights/white` material

---

## System Integration

### Remote Event Synchronisation:
- Membership data broadcasts to Main & Security E2s
- Guard data broadcasts to Main E2
- 60-second auto-refresh system
- Automatic E2 chip registration

### File Persistence:
- Membership data: `banker_memberdata.txt` (JSON format)
- Survives server restarts
- Automatic save on membership changes
- Load on E2 spawn/reset

### Key Interconnections:
- **Security ↔ Main:** Shares guard list for 0% tax
- **Membership ↔ Main:** Shares tier data for tax calculation
- **Membership ↔ Security:** Shares tier data for customer identification
- **Membership ↔ Deposit:** Shares tier data for display

---

## Special Features Summary

### Security:
13 door inputs, multi-zone detection, panic system, floor detection, customer codes

### Economy:
Tiered taxation, batch payouts, insurance system, guard salaries, rack tracking

### User Experience:
Animated UIs, help screens, chat commands, visual feedback, timer displays

### Administration:
20+ chat commands, manual overrides, customer management, profit tracking, closing system

### Automation:
Auto-collection, batch messaging, distance checks, door timers, alarm triggers

### Persistence:
File-based membership storage, remote E2 synchronisation
