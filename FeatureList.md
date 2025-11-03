# Cozzah's Bank System - Complete Feature List

## Overview
Cozzah's Bank System is a comprehensive Garry's Mod DarkRP banking solution featuring tiered memberships, automated tax collection, raid detection with visual/audio alarms, insurance payouts, deposit tracking, and guard management systems.

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

#### Panic Button System:
- **3-second press activation** with incremental button sound feedback
- **Emergency 911 broadcast:** "PANIC ALARM TRIGGERED AT [BANKNAME], WE ARE BEING RAIDED [LOCATION]"
- **Voice line:** Plays "bot/i_could_use_some_help_over_here.wav"
- **Notifications:** Alerts owner and all guards via chat
- **Manual reset:** `*resetpanic` command stops alarm and closes toggle doors
- **Counter tracking:** Shows button press progress

#### Raid HUD Display System:
- **Separate EGP screen (EGPRaid)** shows real-time security status
- **Three status states:**
  - **SAFE (Green):** No threats detected, all clear
  - **ALLOWED (Orange):** Authorized customer in restricted area (with keypad access)
  - **RAID (Red/Flashing):** Unauthorized player detected, alarm active
- **Visual feedback:** Box color changes with status (green â†’ orange â†’ red/flashing)
- **Large text display:** Status clearly visible from distance

#### Alarm System:
- **Sound:** Plays "city_firebell_loop1.wav" on loop
- **Visual integration:** Syncs with Bank Prop Grayscale Cycling E2 (props turn red and flash)
- **Auto-start:** Triggers on unauthorized player detection or lockpick detection
- **Auto-stop:** Silences when threat cleared
- **Notifications:** Messages sent to owner and all guards
- **AlarmActive output:** Sends signal to other E2s for synchronization

#### Configuration Variables (editable in code):
- **Code:** Keypad access code (default: 1234)
- **Salary:** Guard pay rate per hour (default: $900,000/hour, minimum $50,000/hour)
- **Sound:** Alarm sound file (default: "ambient/alarms/city_firebell_loop1.wav")
- **Material:** Zone marker material (default: "lights/white")
- **Location:** Descriptive text for panic broadcast location (default: "across from export Manager")
- **FloorMode:** Floor detection setting (default: 1)
  - Mode 1: Single floor (no floor differentiation)
  - Mode 2: Bottom floor allows customers
  - Mode 3: Top floor allows customers

#### Setup Requirements:
- **Required Wiring:**
  - EGP: Main guard application/roster screen
  - EGPRaid: Separate raid status display screen
  - Door1-3: Main security fading doors (wirelink)
  - Door4-13: Customer printer fading doors (wirelink, up to 10 doors)
  - PanicButton: Physical button (wirelink)
  - Min1/Max1: Customer zone boundary markers (entities)
  - Min2/Max2: Secure zone boundary markers (entities)
  - Keypad: Keypad input (number)

- **Entity Setup:**
  - Place colored markers at zone corners
  - E2 automatically colors them on spawn:
    - Min1/Max1 (Customer zone): Yellow
    - Max1 (Upper boundary): Red
    - Min2/Max2 (Secure zone): Orange
  - Markers use "lights/white" material for visibility

#### Banker System:
- **Automatic banker detection** (job-based: "Banker" job required)
- **Banker benefits:** 0% tax like guards, paid every 10 minutes
- **Auto-hire:** Detects when player takes banker job
- **Auto-fire:** Removes banker when job changes
- **Salary:** Same rate as guards ($900k/hour default)
- **Notifications:** Receives all security alerts like guards

#### Additional Features:
- Remote table sharing with other E2s
- Door auto-close system (10 seconds)
- Raid mode toggle doors
- Configurable alarm sound and materials
- Automatic guard payment notifications (every 10 minutes)
- Door open/close sound effects
- 60-second application cooldown per player

---

### 2. Banker Main E2 (`cozzah's_banker_e2_main_V1.2_pending_payouts.txt`)

#### Main Features:
- Automatic printer/rack collection system
- Smart payout batching (groups collections within 5 seconds to reduce spam)
- Per-rack tax tracking and statistics
- Automatic rack owner assignment with optional manual owner change command
- Customer notification system
- HUD position customisation (HUDX/HUDY variables)
- 10-minute collection reminder timer with cash sound effect
- Distance tracking (HUD clears when moving away from racks)
- Updates every 0.1 seconds for smooth HUD display

#### Collection System:
- Real-time collection display HUD showing:
  - Customer name & rack type (RACK/TOWER)
  - Membership tier with colour coding (Diamond=cyan, Gold=yellow, Silver=gray, Copper=brown, Guards=blue)
  - Total tax collected per rack
  - Collection amount preview
- Automatic tax calculation based on membership tier
- Guard exemption (0% tax for guards)
- Smart batching: Single payment message for multiple rack collections within 5 seconds
- Payment breakdown shows: total profit, tax amount, percentage, rack count
- Color-coded, branded chat notifications
- Tracks individual tax totals for every rack in bank

#### Tax Rates by Tier:
| Tier | Tax Rate | Insurance | Cost |
|------|----------|-----------|------|
| Copper (Default) | 35% | 0% | Free |
| Silver | 25% | 50% | $1,000,000 |
| Gold | 20% | 75% | $2,000,000 |
| Diamond | 10% | 100% | $5,000,000 |
| Ruby (Admin Only) | 0% | 100% | Free (Admin Granted) |
| Security Guards | 0% | N/A | Job-based |

#### Administration:
- Bank closing notification system with countdown
- Customer list with tier display and tax totals
- Total profit summary (all racks combined)
- Manual wire transfer system (minimum $1,000)
- Raid notification system (alerts all customers for insurance trigger)
- Rack owner override system

#### Insurance System:
- **Automatic insurance calculation** based on membership tier
- **Item value tracking:**
  - Standard Rack: $25,000
  - Pro Rack: $100,000
  - Basic Printer: $30,000
  - Advanced Printer: $60,000
  - Tier 5 Printer: $75,000
  - VIP Printer: $200,000
  - VIP+ Printer: $275,000
  - Epic Printer: $375,000
  - Legendary Printer: $450,000
- **Insurance payout rates by tier:**
  - Copper: 0% (no insurance)
  - Silver: 50% of item value
  - Gold: 75% of item value
  - Diamond: 100% of item value (full coverage)
  - Ruby: 100% of item value
  - Guards: Not applicable
- **Payout process:**
  - Calculates total value of all printers and racks per customer
  - Multiplies by customer's insurance rate
  - Pays out automatically to all affected customers
  - Processes each unique player once (prevents duplicates)
  - Shows detailed breakdown of printer types, counts, and values
  - Offline customers tracked and paid when they reconnect

#### Chat Commands (`*` prefix):
- `*help` - Show admin commands with full command list
- `*close [minutes] [notification_count]` - Send closing warnings with automated countdown
  - Example: `*close 30 6` (30 minutes with 6 notifications)
  - Auto-calculates notification intervals
  - Progressive warnings ("15 mins", "10 mins", "5 mins")
  - Final notification authorizes printer destruction
- `*customers` - List all customers with tiers and tax totals
- `*profit` - Show total tax profit summary (total, rack count, average per rack)
- `*wire [player] [amount]` - Send money to player (both parties get confirmation)
- `*owner [player]` - Assign player to aimed rack (shows old/new owner)
- `*raided` - Notify all customers of raid (triggers insurance notification)
- `*insurance` - Manually pays insurance to ALL customers (processes all racks)
- `*checkinsurance [player]` - Shows detailed insurance breakdown for specific player:
  - Lists all printers with counts and values (color-coded by tier)
  - Lists all racks with values
  - Shows total item value
  - Shows insurance rate based on tier
  - Calculates exact payout amount
- `*setprinter [player] [type] [count]` - Manually set printer count for customer
  - Types: Basic, Advanced, Tier 5, VIP, VIP+, Epic, Legendary
  - Used for manual insurance tracking adjustments

#### Integration Features:
- Remote table synchronisation with Security & Membership E2s
- Automatic customer tier recognition from Membership E2
- Guard roster sync from Security E2 for 0% tax
- Tax total accumulation per rack
- Automatic syncs via remote events - no manual data entry

---

### 3. Membership E2 (`cozzah's_bank_membership_e2.txt`)

#### Main Features:
- **Dual membership system:** Permanent and Temporary passes
- **Permanent:** One-time purchase that never expires (survives server restarts)
- **Temporary:** Half-price session passes (expires on disconnect)
- File-based persistence (`banker_memberdata.txt` in JSON format)
- 5 Membership tiers: Copper (free), Silver, Gold, Diamond, Ruby
- Interactive EGP purchase interface with animated UI transitions
- Remote table broadcasting to other E2s (Main, Security, Deposit)
- Tier upgrade system (no downgrades allowed)
- 60-second auto-refresh system
- Automatic E2 chip registration

#### Membership Tiers:
| Tier | Tax | Insurance | Permanent Cost | Temporary Cost |
|------|-----|-----------|----------------|----------------|
| **Copper** (Default) | 35% | 0% | FREE | N/A |
| **Silver** | 25% | 50% | $1M | $500K |
| **Gold** | 20% | 75% | $2M | $1M |
| **Diamond** | 10% | 100% | $5M | $2.5M |
| **Ruby** | 0% | 100% | Admin-only | Admin-only |

#### Temporary Membership System:
- **Player-purchasable** temporary passes at 50% of permanent price
- **Separate temp buttons** on UI below permanent membership buttons
- **Effective tier calculation:** Uses higher of permanent OR temporary
- **Session-based:** Temp tier expires when player disconnects
- **Full benefits:** Same tax rate and insurance as permanent tier
- **Upgrade path:** Can purchase permanent at any time to keep tier forever
- **Use cases:** Trial runs, cost-effective short-term printing, testing bank benefits

#### Screen Interface:
- **Home Screen:** Bank branding, "GET YOUR MEMBERSHIP HERE" headline, large "START" button with animations
- **Membership Selection Screen:** Shows membership cards side-by-side with:
  - Tier name with color coding (Copper=brown, Silver=gray, Gold=yellow, Diamond=cyan)
  - Tax rate and insurance percentage
  - Two purchase buttons per tier:
    - **"PERM [cost]"** - Permanent membership button (top)
    - **"TEMP [cost]"** - Temporary membership button (bottom)
  - Tier icon/symbol (bar for Silver/Gold, diamond for Diamond tier)
- **Anti-downgrade protection:** Shows message if player already has same/higher tier
- **Distance monitoring:** Auto-resets if customer walks 100+ units away
- **One user at a time:** Locks screen to current user

#### Payment System:
- 5-second payment window with popup
- Exact payment verification (accepts both temp and perm pricing)
- Automatic refund on invalid payments
- Payment cancellation handled gracefully
- Already-purchased tier prevention
- Cash register sound on successful purchase
- Spam protection (1-second cooldown between button clicks)

#### Chat Commands (`*` prefix):
- `*help` - Show admin help with full command list
- `*ruby [player]` - Grant Ruby tier (admin-only VIP tier)
- `*remove [player]` - Remove player's permanent membership (full refund issued)
- `*members` - List all members with both permanent and temporary tiers
- `*membership [player]` - Show detailed tier breakdown for specific player:
  - Permanent tier
  - Temporary tier  
  - Effective tier (whichever is higher)

#### File Management:
- Auto-created on first save in "banker_memberdata.txt"
- Stores Steam ID â†’ Tier Number mapping (e.g., "STEAM_0:1:12345" â†’ 3)
- Efficient lookup for tax calculations
- Automatic save on membership changes
- Load on E2 spawn/reset
- Minimal memory footprint
- JSON format for reliability

#### Sound Effects:
- Cash register sound on successful purchase
- Button clicks for all interactions
- Door sounds for screen transitions
- Error beep for failed purchases

#### Data Integrity Features:
- No downgrades (prevents accidental tier downgrades)
- Duplicate prevention (won't charge for tier already owned)
- Steam ID tracking for reliable player identification
- Offline support (tracks memberships even when players offline)

---

## Support Files & Libraries

### Configuration File (`membership_configuration.txt`)

#### Purpose:
Centralized configuration system that defines all membership tier properties and provides helper functions used across all bank E2s.

#### Key Features:
- Single source of truth for all tier properties (tax, insurance, costs, colors)
- Bank name configuration with 13-character auto-truncation  
- Dual membership tier support (permanent + temporary)
- Effective tier calculation (uses higher of perm/temp)
- Legacy data format compatibility
- Consistent color palette across all UIs

#### Helper Functions Library:
- `getTierConfig()` - Complete tier configuration table
- `getTierName(Tier)` / `getTierColor(Tier)` - Name and hex color
- `getTierColorR/G/B(Tier)` - RGB components (0-255)
- `getTierTax(Tier)` / `getTierInsurance(Tier)` - Tax and insurance rates
- `getTierCost(Tier)` / `getTierCostTemp(Tier)` - Permanent and temporary pricing
- `getEffectiveTier()` / `getPermTier()` / `getTempTier()` - Player tier lookups
- `limitBankNameLength(Name, MaxLength)` - Name truncation with ellipsis

---

### Utility Functions (`functions.txt`)

#### Purpose:
Shared utility library providing common operations for messaging, formatting, distance checks, and player detection.

#### Core Utilities:
- **Messaging:** `entity:msg(Text)` - Send validated chat messages
- **Player Lookup:** `string:findPlayer()` - Find by name or Steam ID
- **Number Formatting:** 
  - `formatNumber(N)` - 2500000 → "2.5M"
  - `formatNumberComma(N)` - 1000000 → "1,000,000"
  - `formatTimeInSeconds(N)` - 90 → "1 min 30 secs"
- **String Operations:** 
  - `limitNameLength()` - Truncate with ellipsis
  - `capitalizeFirst()` - Capitalize first letter
  - `string:startsWith()` - Prefix checking
- **Distance Monitoring:**
  - `distanceCheck(Player, Max, Next)` - Auto-execute on distance exceeded
  - `distanceFromEntity()` - Monitor specific entity proximity
  - `playerInRange()` - Trigger functions on player proximity changes
- **Entity Detection:**
  - `findE2ByName(Name, Radius)` - Locate E2 chips by name
  - `array:findEntity(E)` - Find entity in array

---

### UI Library (`ui.txt`)

#### Purpose:
Core EGP rendering system with element management, cursor detection, and the standard bank home screen template.

#### Key Features:
- Auto-ID element tracking system (`table:get()`)
- Drawing primitives: text, circles, boxes, lines
- Cursor interaction and click detection
- Offscreen element cleanup (prevents memory leaks)
- Standardized bank branding screen (`bankHomeScreen()`)

#### Bank Home Screen Template:
Creates consistent branding across all screens with:
- Gray top section (89, 89, 89) / Dark bottom (42, 42, 42)
- Bank name display at top
- Membership tier cards for Silver, Gold, Diamond:
  - Color-coded icons and text
  - Tax/insurance rates display
  - Dual purchase buttons: "PERM [cost]" and "TEMP [cost]"
  - Visual tier indicators (bars for tiers, diamond shape for Diamond)
- 512x512 pixel standard with top-left origin

---

### Security UI Library (`ui_security.txt`)

#### Purpose:
Specialized UI for Security E2 with guard roster display and application screen.

#### Features:
- **Guard roster screen** showing 4 guard slots:
  - Guard names (10 char limit)
  - Online status (gold text) / Offline status (shows Steam ID)
  - Empty slots shown in white
- **Application screen** displaying:
  - Salary as "$X/10Min"
  - "0% TAX" benefit highlight
  - Large "APPLY NOW" button
- **Dual-screen support:** Main roster (EGP) + Raid status (EGPRaid)
- Auto-detects online/offline guards via Steam ID lookup
- Decorative bank icons (gold bars, diamond)

---

### Animation Library (`animations.txt`)

#### Purpose:
Frame-based animation system for smooth UI element movement and rotation at 30 FPS.

#### Core Functions:
- `moveTo(Elements, Name, Dur, Target)` - Slide element to position
- `rotateTo(Name, Dur, Angle)` - Rotate element smoothly
- `startAnimation()` / `stopAnimation()` - Animation control
- Independent timers per element
- Auto-cleanup prevents animation conflicts

#### Used For:
- Deposit handle rotation (360° spin)
- Button pulse effects
- Screen transition slides  
- Tier card reveals
- Arrow bounce animations
- Loading spinners

---

### 4. Deposit E2 (`cozzah's_bank_depo_e2.txt`)

#### Main Features:
- Interactive deposit station with animated touchscreen interface
- Secure fading door system for equipment transfer
- Automatic door control during deposit (opens front door, closes back security door)
- **Automatic rack and printer detection** in deposit area (DepoMin/DepoMax zone)
- **Base area printer/rack tracking** (BaseMin/BaseMax zone)
- Membership tier display on entry with real-time sync
- Distance-based auto-exit (250 units)
- Comprehensive instruction screen
- Auto-reset on user exit
- **Live printer count display** on screen during deposit

#### Detection Systems:
- **Depo Area Detection (DepoMin/DepoMax):**
  - Scans for all printer racks (standard and pro)
  - Counts individual printers by type (Basic, Advanced, Tier 5, VIP, VIP+, Epic, Legendary)
  - Assigns ownership to depositing player via Steam ID
  - Detects empty racks vs racks with printers
  - Real-time count display: "Detected X racks and Y printers"
  
- **Base Area Tracking (BaseMin/BaseMax):**
  - Continuous monitoring of banker's storage area
  - Tracks total printer count by type
  - Tracks total rack count by type
  - Updates text screen output for external displays
  
- **Data Transmission to Main E2:**
  - Automatically sends rack ownership data to Banker Main E2
  - Sends printer count data for insurance calculations
  - Finds Main E2 within 200-unit radius
  - Clears local data after successful transmission

#### Screen States:
- **Home Screen:** Bank branding, large handle in center with "START" text, rotating handle animation
- **Deposit Active Screen:** 
  - Bank branding at top
  - Animated arrow pointing to deposit area
  - Large instruction text
  - **"PRINTERS FOUND" section** showing:
    - Rack counts by type (Standard/Pro)
    - Printer counts by type with color coding
    - Real-time updates as items are detected
    - "No printers or racks detected" message if empty
  - "FINISH AND EXIT" button with pulsing animation
  
#### Instruction Screen Includes:
- **Deposit process:** Rack requirement warning (âš ï¸ empty racks will be destroyed)
- **Collection explanation:** Automatic payments after tax deduction
- **Customer access details:** Keypad usage, door colours (green when nearby), 5-minute timer
- **Storage rules:** Rack placement guidelines
- **Team-sized slot information**
- **Membership tier card display:** Shows current tier with color coding and benefits
- **"I UNDERSTAND" confirmation button**

#### Customer Flow:
1. Customer clicks rotating handle to begin
2. Views tier benefits and deposit instructions
3. Clicks "I UNDERSTAND" confirmation
4. Clicks "DEPOSIT" button
5. Front fading door opens automatically, back security door closes
6. Customer passes printers/racks through
7. **E2 detects and counts all items** in deposit area (1-second delay)
8. **Live count displayed on screen** (updates every 0.5 seconds)
9. Bank owner receives notification with detection summary
10. Customer clicks "FINISH AND EXIT" when done
11. Auto-close if customer walks >250 units away

#### Rack Classification:
- **Standard Rack:** Basic printer_rack entities
- **Pro Rack:** printer_rack_pro or racks with "pro"/"advanced" in class name
- Insurance values: Standard $25k, Pro $100k

#### Printer Classification:
Automatically identifies printer types for insurance tracking:
- Basic, Advanced, Tier 5, VIP, VIP+, Epic, Legendary
- Maps class names to insurance categories
- Counts duplicates per player

#### Animation Features:
- 1-4 second transitions for smooth experience
- Dynamic tier icon display (bar/diamond based on tier)
- Auto-cleanup removes off-screen elements after 10 seconds
- Arrow animation with 0.8-second pulsing movement
- Various door and button sounds for immersion

#### Integration:
- Remote membership data synchronisation
- Displays real-time tier information
- Visual membership card display with tier colours
- Sends rack/printer data to Main E2 for tax calculations
- Owner notification system

---

### 5. Printer EGP Display E2 (`printer_egp_auto_cop_pay_set_mass_combo.txt`)

#### Main Features:
- Animated printer rack display with 7-step collection animation
- Command-payment system for law enforcement ($100k payout)
- Prop mass & collision configuration (3000 mass, collision groups 20)
- RGB colour cycling system (8 props)
- Bank advertisement system (33 unique GPT-generated messages)
- Supports 12 props via inputs (Prop01-Prop12)

#### Visual Features (Decorative Display):
- **Real-time stats display:**
  - Storage capacity bar (0-$300k)
  - Temperature gauge with colour changes
  - Profits counter
  - Cooler rating
- **Animated sequences:**
  - 7-step collection animation
  - Money handle animation
  - Loading animation with spinner
  - Total bank profit counter

#### Automation:
- Simulated printing (adds $700-720 every 3 seconds)
- Temperature simulation (rises randomly, cools at 100Â°C)
- Auto-collection trigger at $250k-300k threshold

#### Law Enforcement System:
- `*cops` command: Pay $100k to all nearby law enforcement within 1000-unit radius
- Job detection array includes: Mayor, Police, SWAT, Secret Service, Custom Police
- Radius-based proximity detection

#### Advertisement System:
- 33 unique bank advertisement messages
- Auto-sends every 5 minutes (configurable via MinAdvert variable)
- GPT-generated banking taglines
- Automated messaging system

#### Additional Commands:
- `*setstate` - Refresh prop mass/collision settings (applies to all 12 props)

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
- **Normal Mode:** Smooth grayscale cycling (0-200 brightness value)
- **Alarm Mode:** Red strobe effect (255,0,0 flashing)
- **Fence Exception:** Red and White strobe at 255 alpha during alarm

#### Material System:
- **Button ON:** `cssconvertedmats/nuke_metalgrate_01` (privacy screen engaged)
- **Button OFF:** `cssconvertedmats/train_largemetal_door_01` (privacy screen disengaged)
- **All other props:** `lights/white` material
- **Lightbar Preservation:** Does not override `cssconvertedmats/lightbareon01`

#### Integration:
- Works with Bank Security E2 alarm system
- Automatically detects alarm state and switches to red strobe
- Returns to normal grayscale cycling when alarm cleared

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
- **Security â†” Main:** Shares guard list for 0% tax
- **Membership â†’ Main:** Shares tier data for tax calculation
- **Membership â†’ Security:** Shares tier data for customer identification
- **Membership â†’ Deposit:** Shares tier data for display
- **Deposit â†’ Main:** Sends rack ownership and printer count data
- **Security â†’ Grayscale:** Sends alarm state for visual effects

---

## Special Features Summary

### Security:
13 door inputs, multi-zone detection, panic system, floor detection, customer codes, banker system, raid HUD display

### Economy:
Tiered taxation, batch payouts, insurance system, guard salaries, rack tracking, automatic detection

### User Experience:
Animated UIs, help screens, chat commands, visual feedback, timer displays, live printer counting

### Administration:
25+ chat commands, manual overrides, customer management, profit tracking, closing system, insurance claims

### Automation:
Auto-collection, batch messaging, distance checks, door timers, alarm triggers, prop detection, data transmission

### Persistence:
File-based membership storage, remote E2 synchronisation, offline player tracking

---

## Credits
Created by Cozzah

## Support
For issues, questions, or feature requests, please contact the developer.

---

*Last Updated: November 2025*
