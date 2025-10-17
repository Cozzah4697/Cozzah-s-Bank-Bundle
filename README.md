# Cozzah's Bank System - Complete Documentation

See [Development Notes](DEVELOPMENT.md) for information about the codebase.

## Table of Contents
- [Banker E2 Main](#banker-e2-main)
- [Bank Membership E2](#bank-membership-e2)
- [Bank Security E2](#bank-security-e2)
- [Bank Depo E2](#bank-depo-e2)
- [Printer EGP Display E2](#printer-egp-display-e2)
- [Bank Prop Grayscale Cycling E2](#bank-prop-grayscale-cycling-e2)
- [How to Use the Bank (Customer Guide)](#how-to-use-the-bank)

---

## Banker E2 Main

### Overview
The Banker E2 Main is the core chip that manages all customer transactions at your bank. It automatically tracks who deposits printers/racks, calculates taxes based on membership tiers, displays collection information on a HUD, and provides powerful admin tools for managing your banking operation.

### Automatic Features

#### Transaction Management
**Automatic Tax Collection:** When you look at a customer's printer rack or bitminer tower and collect money, the E2 automatically:
- Detects whose rack you're collecting from
- Calculates their tax rate based on membership tier (synced from the Membership E2)
- Deducts the tax amount
- Pays the customer their profit
- Tracks total tax collected per rack

#### Real-Time HUD Display
While looking at a customer's rack, you see:
- Customer name and rack/tower type
- Their membership tier (Copper/Silver/Gold/Diamond/Ruby) or Guard status
- Total tax collected from that specific rack
- Color-coded tier display (Diamond=cyan, Gold=yellow, Silver=gray, Copper=brown, Guards=blue)

### Tax Rates by Tier
| Tier | Tax Rate | Insurance |
|------|----------|-----------|
| Copper (Default) | 35% | 0% |
| Silver | 25% | 50% |
| Gold | 20% | 75% |
| Diamond | 10% | 100% |
| Ruby | 1% | 100% |
| Security Guards | 0% | N/A |

### Smart Rack Tracking
- Automatically remembers which customer owns each rack
- Tracks individual tax totals for every rack in your bank
- Smart batching: Groups collections within 5 seconds to reduce spam
- Shows total profit, tax amount, percentage, and rack count in single message

### Insurance System

The E2 tracks all customer printers and racks for insurance purposes.

#### Item Values
- **Racks:**
  - Standard Rack: $25,000
  - Pro Rack: $100,000
- **Printers:**
  - Basic: $30,000
  - Advanced: $60,000
  - Tier 5: $75,000
  - VIP: $200,000
  - VIP+: $275,000
  - Epic: $375,000
  - Legendary: $450,000

#### Insurance Payout Rates
Insurance is paid based on membership tier:
- **Copper:** 0% (no insurance)
- **Silver:** 50% of total item value
- **Gold:** 75% of total item value
- **Diamond:** 100% of total item value (full coverage)
- **Ruby:** 100% of total item value

#### How Insurance Works
1. Owner uses `*raided` command to notify all customers
2. Owner uses `*insurance` command to pay out all customers
3. E2 calculates each customer's total printer and rack values
4. Multiplies by their insurance rate
5. Pays automatically to each unique customer
6. Shows detailed breakdown in console
7. Offline customers are tracked and paid when they reconnect

### Admin Commands
> **Note:** All admin commands are prefixed with `*`

#### Help & Information
- `*help` - Displays list of all admin commands

#### Customer Management
- `*customers` - Shows complete list of all active customers with their membership tiers and tax totals collected
- `*profit` - Displays total tax profit summary including:
  - Total tax collected across all racks
  - Number of racks
  - Average tax per rack
  - Average profit per rack

#### Financial Management
- `*wire [player] [amount]` - Sends money to a specified player (minimum $1,000)
  - Example: `*wire John 50000`
  - Both you and the recipient get confirmation messages
  - Useful for paying insurance manually or refunds

#### Rack Management
- `*owner [player]` - Manually assigns a player as the owner of the rack you're currently looking at
  - Example: Look at a rack, then type `*owner John`
  - Useful if ownership detection fails or you need to reassign a rack
  - Shows confirmation with old and new owner names

#### Bank Closing System
- `*close [minutes] [notification_count]` - Initiates bank closing procedure with automated customer notifications
  - Example: `*close 30 6` (closes in 30 minutes with 6 notifications)
  - Automatically calculates notification intervals
  - Sends timed warnings to all customers via chat
  - Countdown format shows time remaining (e.g., "25 mins 30 secs")
  - Final notification authorizes you to destroy remaining printers
  - Customers get progressive warnings like "15 mins", "10 mins", "5 mins"

#### Raid & Insurance Commands
- `*raided` - Notifies all customers that the bank was raided
  - Sends alert message to all customers
  - Triggers insurance notification
  - Prepares customers for insurance payout

- `*insurance` - Manually pays insurance to ALL customers
  - Processes all racks and calculates total values
  - Pays each unique customer once
  - Shows detailed breakdown of payouts
  - Handles offline players automatically

- `*checkinsurance [player]` - Shows detailed insurance breakdown for specific player
  - Lists all printers with counts and values (color-coded by tier)
  - Lists all racks with values
  - Shows total item value
  - Shows insurance rate based on tier
  - Calculates exact payout amount

- `*setprinter [player] [type] [count]` - Manually set printer count for customer
  - Types: Basic, Advanced, Tier 5, VIP, VIP+, Epic, Legendary
  - Used for manual insurance tracking adjustments
  - Example: `*setprinter John VIP+ 5`

### Integration with Other E2s

#### Works With Membership E2
- Automatically receives updated membership data
- Uses tier information to calculate correct tax rates
- Displays tier names and colors in customer list
- Updates every 60 seconds automatically

#### Works With Security E2
- Receives guard roster automatically
- Guards get 0% tax rate on all collections
- Guard status shown in blue on HUD
- Updates on guard add/remove

#### Works With Deposit E2
- Receives rack ownership data from deposit station
- Receives printer count data for insurance tracking
- Automatic data transmission within 200 units

### Setup Requirements
- Wire to an EGP HUD (for collection display)
- Must have the Membership E2 and Security E2 placed nearby for full functionality
- Optional: Deposit E2 for automatic rack detection
- No additional configuration needed - works automatically once placed

#### HUD Position Configuration
Edit these variables at the top of the E2 code:
- `HUDX` - Horizontal position (default: 7.8, range: 0-100)
- `HUDY` - Vertical position (default: 73, range: 0-100)

### Customer Experience
When customers deposit at your bank:
1. You look at their rack and begin collecting
2. HUD shows their info and tier
3. E2 automatically calculates their tax
4. Customer receives their profit with a breakdown message
5. You keep the tax amount
6. System tracks the total for reporting

### Key Features
- **Detailed Reporting:** Full financial breakdowns on demand
- **Professional Messages:** Color-coded, branded chat notifications
- **Timer System:** 10-minute collection reminder with cash sound effect
- **Distance Tracking:** HUD clears when you move away from racks
- **Smart Batching:** Multiple collections within 5 seconds = single payment message
- **Insurance Tracking:** Automatic printer/rack value tracking for insurance

### Technical Notes
- Updates every 0.1 seconds for smooth HUD display
- Plays cash sound every 10 minutes as collection reminder
- Automatically syncs with other E2s via remote events
- No manual data entry required - everything is automatic
- Prevents processing membership payments as rack collections

---

## Bank Membership E2

### Overview
The Bank Membership E2 manages all customer membership tiers and upgrades at your bank. It provides an interactive touchscreen where customers can purchase permanent membership passes (Silver, Gold, Diamond, or Ruby) that reduce their tax rates and provide insurance benefits. All membership data is automatically saved to file and synced with the Banker E2 Main chip for seamless tax calculations.

### Membership Tiers & Pricing

| Tier | Tax Rate | Insurance | Cost | Notes |
|------|----------|-----------|------|-------|
| **Copper** (Default) | 35% | 0% | Free | Everyone starts here |
| **Silver** | 25% | 50% | $1,000,000 | 10% less tax than Copper |
| **Gold** | 20% | 75% | $2,000,000 | 15% less tax than Copper |
| **Diamond** | 10% | 100% | $5,000,000 | Best tax rate available |
| **Ruby** | 1% | 100% | Admin-only | VIP/Friend tier |

> All memberships are **one-time permanent passes** that never expire

### Dual Membership System

The E2 supports both **permanent** and **temporary** memberships:

#### Permanent Memberships
- Purchased by customers using in-game money
- Never expire
- Survive server restarts
- Stored in file: `banker_memberdata.txt`
- Can be granted by admin for free using `*give` command

#### Temporary Memberships
- Admin-granted only
- Used for promotional trials, VIP weekends, or testing
- Separate from permanent tier
- Players can have both permanent AND temporary tier
- **Effective tier** = whichever is higher
- Can be upgraded to permanent by purchasing
- Cleared manually with admin commands

### How It Works

#### Customer Experience

**Starting the Process:**
- Customer approaches the screen and clicks the "START" button
- Background slides away with smooth animations
- Membership tier cards slide into view
- Each tier displays tax rate, insurance, and cost

**Viewing Tiers:**
- Customer sees all membership options
- Color-coded cards (Copper=brown, Silver=gray, Gold=yellow, Diamond=cyan, Ruby=red)
- Clear pricing and benefit information
- Visual icons representing each tier (bars for lower tiers, diamond for top tiers)
- "PERM PASS" label on purchasable tiers

**Purchasing a Tier:**
- Customer clicks on desired membership button
- **Already Own Tier:** Gets message they already have same/higher tier
- **Insufficient Funds:** Gets retry cooldown timer message
- **Successful Purchase:**
  - Money request popup appears (5-second timeout)
  - Customer accepts payment
  - Membership instantly upgraded
  - Congratulations message sent
  - Cash register sound plays
  - File automatically saved

**Anti-Downgrade Protection:**
- System prevents purchasing lower tiers
- If Gold member clicks Silver, they're told they already have better
- Protects customers from wasting money

### Auto-Save System
- **File Storage:** All membership data saved to "banker_memberdata.txt"
- **Persistent Data:** Memberships stay after server restarts
- **Auto-Load:** Loads previous data when E2 is spawned
- **Real-Time Sync:** Updates file immediately after each purchase
- **JSON Format:** Reliable, human-readable format

### Admin Commands
> **Note:** All admin commands are prefixed with `*`

#### Membership Management

**Permanent Tier Commands:**
- `*give [player] [tier_number]` - Manually grants **permanent** membership tier to a player for free
  - Tier numbers: `1`=Silver, `2`=Gold, `3`=Diamond, `4`=Ruby
  - Example: `*give John 3` (gives Diamond to John)
  - Prevents downgrades (can't give Silver to Gold member)
  - Sends confirmation to both admin and player
  
**Useful for:**
- Promotional giveaways
- Fixing payment issues
- Rewarding loyal customers
- Staff benefits

**Temporary Tier Commands:**
- `*granttemp [player] [tier_number]` - Grants **temporary** membership tier
  - Tier numbers: `1`=Silver, `2`=Gold, `3`=Diamond
  - Example: `*granttemp Sarah 2` (gives temporary Gold to Sarah)
  - Separate from permanent tier - player can have both
  - Player's effective tier = highest of permanent OR temporary
  - Useful for promotional trials, VIP weekends, testing

- `*cleartemp` - Clears ALL temporary memberships at once
  - Removes all temp tiers from all players
  - Permanent tiers are unaffected
  - Useful for ending promotional periods

- `*listtemp` - Shows list of all active temporary memberships
  - Displays player names and their temporary tiers
  - Shows color-coded tier names
  - Helps track promotional memberships

- `*membership [player]` - Shows detailed tier breakdown for specific player
  - Displays permanent tier
  - Displays temporary tier
  - Shows effective tier (whichever is higher)
  - Example: Player has Gold permanent + Diamond temp = Diamond effective

**Usage Examples:**
```
*give John 1           → Grants permanent Silver to John
*give Sarah 2          → Grants permanent Gold to Sarah
*give Mike 3           → Grants permanent Diamond to Mike
*granttemp Alex 2      → Grants temporary Gold to Alex
*listtemp              → Shows all temporary memberships
*membership John       → Shows John's permanent, temp, and effective tiers
*cleartemp             → Removes all temporary memberships
```

### Key Features

#### Smart Payment Handling
- **Money Request System:** Uses in-game payment popup (5-second timeout)
- **Exact Amounts:** Verifies correct payment amount received ($1M, $2M, or $5M)
- **Auto-Refund:** If wrong amount paid, automatically refunds customer
- **Spam Protection:** 1-second cooldown between button clicks
- **Insufficient Funds Protection:** Shows time until customer can retry

#### User Experience
- **Distance Monitoring:** Auto-resets if customer walks 100+ units away
- **One User at a Time:** Locks screen to current user until they finish
- **Visual Feedback:** Sound effects for all interactions
- **Clear Messaging:** Color-coded chat notifications
- **Professional Branding:** Bank logo and clean interface
- **Animated Transitions:** Smooth 1-4 second screen transitions

#### Data Integrity
- **No Downgrades:** System prevents accidental tier downgrades
- **Duplicate Prevention:** Won't charge for tier already owned
- **Steam ID Tracking:** Uses Steam ID for reliable player identification
- **Offline Support:** Tracks memberships even when players offline

### Integration with Other E2s

#### Auto-Sync with Banker E2 Main
- Membership data automatically sent to Main E2 on request
- Main E2 uses data to calculate correct tax rates
- No manual updates required
- Instant communication via remote events
- 60-second auto-refresh system

#### Works With Security E2
- Security E2 can check customer tiers for door access
- Membership data shared automatically
- Used for customer identification in zones

#### Works With Deposit E2
- Deposit E2 displays customer tier on entry
- Shows tier benefits and insurance rates
- Real-time tier synchronization

### Setup Requirements

#### Required Components
- **EGP Screen:** Wire to an EGP screen for user interface
- **File System:** Ensure E2 has file write permissions
- **Banker E2 Main:** Must be placed within 200 units for auto-sync

#### First-Time Setup
1. Spawn the E2
2. Wire to EGP screen
3. E2 automatically creates data file
4. Screen displays membership options
5. Ready for customers immediately

### File Management
- **Auto-Created:** File created on first save
- **Location:** "banker_memberdata.txt" in E2 file system
- **Format:** JSON encoded for reliability
- **Backup:** Consider backing up file periodically

### Screen Layout

#### Home Screen
- Bank branding at top
- "GET YOUR MEMBERSHIP HERE" headline
- Large circular "START" button
- Clean, professional appearance

#### Membership Selection Screen
Membership cards displayed side-by-side, each card shows:
- Tier name with color coding
- Tax rate and insurance percentage
- Cost (or "DEFAULT" for Copper)
- Interactive purchase button
- Tier icon/symbol

### Technical Details

#### Data Storage
- Stores Steam ID → Tier Number mapping
- Example: `"STEAM_0:1:12345" → 3` (Diamond)
- Efficient lookup for tax calculations
- Minimal memory footprint
- Separate storage for permanent and temporary tiers

#### Payment Verification
- Checks exact payment amounts: $1M, $2M, or $5M
- Any other amount triggers refund
- 5-second payment window
- Handles payment cancellation gracefully

#### Error Handling
- Invalid player names handled gracefully
- File load failures create new empty table
- Payment failures notify customer
- All errors logged to console via print statements

### Customer Instructions

#### For First-Time Buyers
1. Approach the membership screen
2. Click the "START" button
3. Review the membership tiers and benefits
4. Click on the tier you want to purchase
5. Accept the payment request popup
6. Receive membership upgrade immediately

#### For Existing Members
- Screen will prevent purchasing same/lower tier
- Can only upgrade to higher tiers
- Memberships are permanent (never expire)
- Can have temporary tier alongside permanent tier

### Special Features

#### Sound Effects
- Cash register sound on successful purchase
- Button clicks for all interactions
- Door sounds for screen transitions
- Error beep for failed purchases

### Best Practices

#### For Bank Owners
- Place screen near other bank EGP screens
- Keep it well-lit and accessible
- Use `*give` command for promotional events
- Use `*granttemp` for trial memberships
- Back up membership file regularly
- Use `*listtemp` to track promotional memberships

#### Pricing Recommendations
- Current prices balanced for typical server economy
- Diamond tier = significant investment for serious customers
- Silver tier = accessible entry point
- Gold tier = middle ground for established players
- Ruby tier = special friends/VIPs only

### Troubleshooting

**If Memberships Don't Sync:**
- Check Banker E2 Main is within 200 units
- Verify both E2s belong to same owner
- Check console for "E2 Chip initialized" message

**If Payments Fail:**
- Customer needs to pay exact amount
- Must accept payment within 5 seconds
- Check customer has sufficient funds

**If File Won't Save:**
- Verify E2 has write permissions
- Console will show save confirmation
- Check file wasn't manually deleted

---

## Bank Security E2

### Overview
The Bank Security E2 is a comprehensive security system that manages guards, guests, door access, alarm systems, and raid detection for your bank. It handles guard payroll, door assignments, panic buttons, and multiple security zones with intelligent threat detection. Features an interactive guard application screen and integrates with visual alarm systems.

### Guard System

#### Guard Hiring & Management

**Application Screen:**
- Displays on EGP showing 4 guard slots with names
- Empty slots show "EMPTY" in white
- Filled slots show guard names in gold
- Offline guards show "OFFLINE: [SteamID]"
- Shows salary rate and "SECURE PRINTING AT 0% TAX" benefit
- "APPLY NOW" button for interested players
- 60-second cooldown per player to prevent spam

**Job Requirements:**
- Only Security Guards and Mercenaries can apply/be hired
- Automatic job validation on hire
- Auto-removal if guard changes to unauthorized job

**Automatic Payroll:**
- Guards paid every 10 minutes (600-second timer)
- Default salary: $900,000/hour ($150,000 per 10-minute payment)
- Customizable salary via admin command (minimum $50,000/hour)
- Payment messages sent to all active guards
- Shows time until next payment
- Severance pay given if removed while still authorized job

#### Guard Benefits
- **0% Tax Rate:** Guards pay no tax when depositing at the bank
- **Full Door Access:** Can open main security doors (Door1-3) and assigned guard doors
- **Salary Notifications:** Get paid with time-until-next-payment message
- **Job Protection:** Auto-removed if they change to unauthorized job
- **Security Alerts:** Receive all alarm and intrusion notifications

### Banker System

The E2 also supports a special "Banker" role:

- **Automatic detection** - Detects players with "Banker" job
- **Same benefits as guards** - 0% tax, paid every 10 minutes
- **Auto-hire/fire** - Automatically added when taking Banker job, removed when changing jobs
- **Security notifications** - Receives all security alerts
- **Salary** - Same rate as guards (default $900k/hour)

### Guest System

#### Guest Management
- **Permanent Access:** Owner can add guests for secure area access
- **Timed Passes:** Option to set expiration time in minutes
  - Example: `!add Sarah 30` (30-minute guest pass)
- **Auto-Expiration:** Guests automatically removed when time expires
- **Notifications:** Both owner and guest notified on add/remove/expiration

#### Guest Permissions
- Can open main security doors (Door1-3)
- Can access guest-authorized areas (Min1/Max1, excluding customer-only zones)
- Cannot open customer printer doors (unless specifically assigned)
- No salary payments

### Door System

#### Main Security Doors (Door1-3)
- **Access:** Owner, Guards, Guests, and Banker only
- **Fading Doors:** Auto-open for authorized users
- **Sound Effects:** Opening and closing sounds
- **Protection:** Unauthorized players get denial message
- **Auto-close:** Standard fading door behavior

#### Customer Printer Doors (Door4-13)
- **Individual Assignment:** Each door can be assigned to specific customer
- **Guard Doors:** Specific doors can be designated for all guards
- **Keypad Access:** Customers enter code for 5-minute access window
- **Color Coding:**
  - **Red (default):** No customer nearby, locked
  - **Green:** Assigned customer within 150 units, ready to open
- **Auto-Close:** Closes after 10 seconds if owner present in Min1/Max1 area
- **Raid Toggle:** Acts as toggle door during raids when owner not in area
- **Customer Code Resend:** `!code` command resends access code to customer

#### Door Assignment Features
- Customers only access their assigned door
- Guards can access all "guard doors"
- Owner has universal access to all doors
- Offline customers tracked by Steam ID
- Guests can access all customer doors

### Security Zones

#### Zone Detection System
The E2 monitors multiple zones with intelligent player detection:

**Customer Zone - Bottom Floor (Min1/Max1 - Lower Half)**
- **Allowed:** Owner, Guards, Guests, Banker, Assigned Customers (with keypad access)
- **Customer Logic:** Only customers with door assignments allowed
- **Keypad Timer:** 5-minute access window after code entry
- **Warning:** "Allowed player inside" message when customer present (to owner and guards)
- **Floor Detection:** Uses Y-coordinate to determine floor level

**Upper Zone - Top Floor (Min1/Max1 - Upper Half)**
- **Allowed:** Owner, Guards, Guests, Banker only
- **No Customers:** Customers not permitted on upper floor
- **Strict Access:** Triggers alarm immediately if customer enters

**Secure Area (Min2/Max2)**
- **Allowed:** Owner, Guards, Guests, Banker only
- **High Security:** No customer access ever
- **Critical Zone:** Often used for vault/safe areas
- **Instant Alarm:** Unauthorized entry triggers immediate alarm

#### Floor Mode Configuration
Edit the `FloorMode` variable in the E2 code:
- **Mode 1:** Single floor (no floor differentiation) - Default
- **Mode 2:** Bottom floor allows customers
- **Mode 3:** Top floor allows customers

#### Threat Detection
- **Lockpick Detection:** Instant alarm if anyone has lockpick/cracker equipped
- **Unauthorized Entry:** Alarm triggers for non-permitted players
- **Real-Time Monitoring:** Checks every second
- **Smart Floor Detection:** Uses Y-coordinate to determine floor level
- **Owner Bypass:** Owner always has access everywhere

### Alarm System

#### Automatic Alarm Features
- **Sound:** Plays "city_firebell_loop1.wav" on loop (configurable)
- **Visual Integration:** Syncs with Bank Prop Grayscale Cycling E2
  - Props turn red and flash during alarm
  - Returns to normal when alarm stops
- **Notifications:** Messages sent to owner, all guards, and banker
- **Auto-Start:** Triggers on unauthorized player detection or lockpick detection
- **Auto-Stop:** Silences when threat cleared
- **AlarmActive Output:** Sends signal to other E2s for synchronization

### Panic Button System

#### Manual Emergency Activation
- **Physical button** must be pressed for 3 seconds
- **Sound Feedback:** Incremental button clicks during press (increasing volume)
- **Activation Sound:** Plays "bot/i_could_use_some_help_over_here.wav" voice line
- **Emergency 911 Broadcast:** Sends to chat: "PANIC ALARM TRIGGERED AT [BANKNAME], WE ARE BEING RAIDED [LOCATION]"
- **Notifications:** Alerts owner and all guards via chat
- **Manual Reset:** `*resetpanic` command stops alarm and closes toggle doors
- **Counter Tracking:** Tracks button press progress (requires 3 presses)
- **Toggle Mode:** Panic triggers raid mode (doors stay open)

### Raid HUD Display System

Separate EGP screen (EGPRaid) shows real-time security status:

**Three Status States:**
- **SAFE (Green):**
  - No threats detected
  - All clear
  - Normal operations
  - Background: Green box

- **ALLOWED (Orange):**
  - Authorized customer in restricted area
  - Customer has valid keypad access
  - Monitoring but not alarmed
  - Background: Orange box

- **RAID (Red/Flashing):**
  - Unauthorized player detected
  - Alarm active
  - Emergency status
  - Background: Flashing red box
  - Large "RAID" text

**Visual Feedback:**
- Box color changes with status
- Large text display visible from distance
- Real-time updates every check cycle

### Configuration Variables

Edit these at the top of the E2 code:

- **Code:** Keypad access code for customers (default: `1234`)
- **Salary:** Guard/Banker pay rate per hour (default: `900000`, minimum: `50000`)
- **Sound:** Alarm sound file (default: `"ambient/alarms/city_firebell_loop1.wav"`)
- **Material:** Zone marker material (default: `"lights/white"`)
- **Location:** Descriptive text for panic broadcast (default: `"across from export Manager"`)
- **FloorMode:** Floor detection mode (default: `1`)

### Admin Commands
> **Note:** All admin commands are prefixed with `*`

#### Help & Information
- `*help` - Shows all admin commands
- `*guards` - Lists all current guards by name with online/offline status
- `*doors` - Shows all door assignments with player names

#### Guard Management
- `*add [player]` - Hires player as guard (must be Security Guard/Mercenary job)
  - Example: `*add John`
  - Plays celebration sound
  - Updates guard screen immediately
  - Sends congratulations message with salary info
  - Both admin and player get confirmation

- `*remove [player]` - Fires guard from position
  - Example: `*remove John`
  - Gives severance pay (1 hour salary) if still authorized job
  - Updates guard screen immediately
  - Sends termination message
  - Removes from guard roster

- `*set [amount]` - Changes guard salary (minimum $50,000/hour)
  - Example: `*set 1200000` (sets to $1.2M/hour)
  - If raise: Notifies all guards of pay increase
  - If cut: Notifies all guards of pay decrease (with apology message)
  - Updates guard application screen with new rate

- `*update` - Manually refreshes guard screen display
  - Forces screen update
  - Useful if screen gets desynced

#### Door Management
- `*adddoor [player]` - Assigns player to door you're looking at
  - Example: Look at door, type `*adddoor John`
  - Sends keypad code to assigned player
  - Both owner and player get confirmation
  - Door turns green when player nearby

- `*deldoor` - Removes assignment from door you're looking at
  - Example: Look at door, type `*deldoor`
  - Player notified of removal
  - Door returns to unassigned state (red)

- `*addguarddoor` - Makes door accessible to all guards
  - Example: Look at door, type `*addguarddoor`
  - All current and future guards can open it
  - Useful for shared security doors

- `*delguarddoor` - Removes guard access from door
  - Example: Look at door, type `*delguarddoor`
  - Returns door to owner-only or assignment-only

#### System Management
- `*resetpanic` - Resets panic button and closes all toggle doors
  - Stops raid mode
  - Closes any stuck-open doors
  - Resets panic counter to 0
  - Useful for ending false alarms

### Player Commands
> **Note:** All player commands are prefixed with `!`

#### Help & Information
- `!help` - Shows all player commands

#### Guest Management (Owner only)
- `!add [player]` - Adds player as guest (permanent until removed)
  - Example: `!add Sarah`
  - Guest gets full secure area access

- `!add [player] [minutes]` - Adds guest with time limit
  - Example: `!add Sarah 30` (30-minute guest pass)
  - Auto-expires and notifies both parties
  - Useful for temporary visitors

- `!remove [player]` - Removes guest access
  - Example: `!remove Sarah`
  - Both owner and guest notified

- `!guests` - Shows list of current guests with time remaining
  - Displays all active guests
  - Shows expiration times for timed guests

#### Customer Commands
- `!code` - Resends keypad access code to customer
  - Only works if customer has assigned door
  - Shows code in chat
  - Reminds customer to press 'E' to open doors

### Setup Requirements

#### Required Wiring
- **EGP:** Main guard application/roster screen (wirelink)
- **EGPRaid:** Separate raid status display screen (wirelink)
- **Door1-3:** Main security fading doors (wirelink)
- **Door4-13:** Customer printer fading doors (wirelink, up to 10 doors)
- **PanicButton:** Physical button (wirelink)
- **Min1/Max1:** Customer zone boundary markers (entities)
- **Min2/Max2:** Secure zone boundary markers (entities)
- **Keypad:** Keypad input for customer access (number)

#### Entity Setup
Place colored markers at zone corners:
- E2 automatically colors them on spawn:
  - **Min1/Max1:** Yellow (Customer zone)
  - **Max1:** Red (Upper boundary)
  - **Min2/Max2:** Orange (Secure zone)
- Markers use "lights/white" material for visibility
- Place markers outside player view (underground/in walls) for clean appearance

### Integration with Other E2s

#### Sends Data To
**Banker E2 Main:**
- Automatically sends guard roster
- Sends banker information
- Main E2 uses this for 0% tax rate
- Updates on guard add/remove/banker change
- Syncs via remote events

**Bank Prop Grayscale Cycling E2:**
- Sends AlarmActive status
- Syncs alarm state with visual props
- Props flash red during alarms

#### Receives Data From
**Bank Membership E2:**
- Can check customer tiers for enhanced security
- Used for customer identification in zones

#### Works Independently For
- Door access control
- Zone monitoring
- Guard and banker payroll
- Guest management
- Panic button system

### Key Features

#### Smart Detection
- **Floor-Based Logic:** Automatically determines which floor players are on
- **Find System Optimization:** Uses single find operation per tick for performance
- **Distance Checks:** 150-unit proximity for door color changes
- **Owner Bypass:** Owner always has access everywhere
- **Steam ID Tracking:** Remembers offline customers

#### Security Features
- **Lockpick Auto-Detect:** Instant alarm on detection
- **Multi-Zone Coverage:** Three separate security zones
- **Offline Tracking:** Remembers customers who disconnect
- **Real-time Monitoring:** Checks every second
- **Keypad Timer:** 5-minute access windows with countdown

#### User Experience
- **Sound Feedback:** Audio cues for all door actions
- **Visual Status:** Color-coded doors and raid HUD
- **Clear Messaging:** Professional chat notifications
- **Application UI:** Interactive hiring screen
- **Auto-coloring:** Zone markers colored automatically

### Guard Payment Example
With default $900,000/hour salary:
- Payment every 10 minutes
- Each payment: $150,000
- Guards notified: "Thank you for protecting [BANKNAME], you have been paid $150K. Next payment in 10 mins"

### Best Practices

#### For Bank Owners
- Set salary competitively to attract good guards
- Regularly check chat for job applications
- Assign doors to customers as they deposit printers
- Use guest system for trusted friends/VIPs
- Configure FloorMode based on your bank layout
- Set Location variable to your bank's actual location

#### Zone Setup Tips
- Make customer zone (Min1/Max1) large enough for comfortable movement
- Keep secure zone (Min2/Max2) small and critical
- Ensure floor boundary (Max1 Y-position) is clearly mid-level between floors
- Place markers outside player view (underground/in walls) for aesthetics
- Test zones thoroughly before opening to customers

### Troubleshooting

**If Doors Not Responding:**
- Verify wirelinks are connected properly
- Check door has fading door tool applied
- Ensure door entity is valid
- Test with owner first before assigning to customers

**If Alarm Won't Stop:**
- Check all zones for unauthorized players
- Look for players with lockpicks equipped
- Use `*resetpanic` to force reset
- Check if anyone is in Min2/Max2 area

**If Guards Not Getting Paid:**
- Verify guards are online
- Check they still have authorized job
- Ensure salary is set above minimum ($50k)
- Check console for payment confirmations

**If Data Not Syncing:**
- Verify Banker E2 Main is within 200 units
- Check both E2s owned by same player
- Look for "E2 Chip initialized" message in console
- Restart both E2s if needed

**If Customers Can't Access Doors:**
- Verify door assignment with `*doors` command
- Check customer has keypad code (they can use `!code`)
- Ensure keypad timer hasn't expired (5 minutes)
- Verify customer is within 150 units of door

---

## Bank Depo E2

### Overview
The Bank Depo E2 provides an interactive deposit station where customers can safely store their printers and bitminer towers at your bank. It features an animated touchscreen interface with a secure fading door system, automatic printer/rack detection, and real-time inventory display during deposit.

### Main Features

#### Automatic Detection Systems
- **Depo Area Detection (DepoMin/DepoMax zone):**
  - Scans for all printer racks (standard and pro)
  - Counts individual printers by type
  - Assigns ownership to depositing player via Steam ID
  - Detects empty racks vs racks with printers
  - Real-time count display: "Detected X racks and Y printers"
  
- **Base Area Tracking (BaseMin/BaseMax zone):**
  - Continuous monitoring of banker's storage area
  - Tracks total printer count by type
  - Tracks total rack count by type
  - Updates text screen output for external displays
  
- **Data Transmission:**
  - Automatically sends rack ownership data to Banker Main E2
  - Sends printer count data for insurance calculations
  - Finds Main E2 within 200-unit radius
  - Clears local data after successful transmission

#### Printer Classification
Automatically identifies printer types:
- Basic, Advanced, Tier 5, VIP, VIP+, Epic, Legendary
- Maps class names to insurance categories
- Counts duplicates per player
- Color-codes display by printer tier

#### Rack Classification
- **Standard Rack:** Basic printer_rack entities ($25k insurance value)
- **Pro Rack:** printer_rack_pro or racks with "pro"/"advanced" in class name ($100k insurance value)

### Customer Flow

#### Step-by-Step Process
1. **Approach Screen:** Customer walks up to deposit station
2. **Click START:** Customer clicks rotating handle to begin
3. **View Instructions:** Sees tier benefits and deposit instructions
4. **Confirm Understanding:** Clicks "I UNDERSTAND" button
5. **Begin Deposit:** Clicks "DEPOSIT" button
6. **Doors Activate:** 
   - Front fading door opens automatically
   - Back security door closes
   - Owner receives notification
7. **Pass Equipment:** Customer moves printers/racks through opening
8. **Detection Phase:** E2 detects and counts all items (1-second delay)
9. **Live Display:** Screen updates with printer/rack counts every 0.5 seconds
10. **Finish:** Customer clicks "FINISH AND EXIT"
11. **Auto-Exit:** Screen resets if customer walks >250 units away

### Screen States

#### Home Screen
- Bank branding at top
- Large rotating handle in center with "START" text
- Smooth rotation animation
- Clean, welcoming appearance

#### Instruction Screen
Shows detailed deposit information:
- **Deposit Process:** Warning about empty racks being destroyed
- **Collection Explanation:** How automatic payments work
- **Access Details:** Keypad usage, door colors, 5-minute timer
- **Storage Rules:** Rack placement guidelines
- **Team Slots:** Information about larger storage options
- **Membership Card:** Displays customer's current tier with benefits
- **"I UNDERSTAND" Button:** Confirmation to proceed

#### Deposit Active Screen
Real-time deposit interface:
- Bank branding at top
- **Animated arrow** pointing to deposit area (0.8-second pulsing)
- Large instruction text
- **"PRINTERS FOUND" Section:**
  - Header with underline
  - Rack counts by type (Standard/Pro) up to 2 types displayed
  - Printer counts by type with color coding
  - Real-time updates as items detected
  - "No printers or racks detected" message if empty
  - Maximum 20 printer types displayed
- **"FINISH AND EXIT" Button:** Large, pulsing button
- Live inventory updates every 0.5 seconds

### Door Control

#### Automatic Door Management
- **Front Door (Door):** Opens when deposit starts
- **Security Door (DoorSeurity):** Closes when deposit starts
- **Sound Effects:** Plays "czero/doorstop1.wav" on door operation
- **Owner Notification:** "Player is depositing Printers" message
- **Auto-reset:** Both doors return to normal state when customer exits

### Membership Integration

#### Tier Display
- Shows customer's current membership tier on entry
- Displays tier color (Copper=brown, Silver=gray, Gold=yellow, Diamond=cyan, Ruby=red)
- Shows tax rate and insurance percentage
- Dynamic tier icons (bar for lower tiers, diamond for top tiers)
- Real-time sync with Membership E2

#### Tier Benefits Shown
- Current tax rate
- Current insurance coverage
- Visual membership card
- Upgrade prompts if applicable

### Animation Features

#### Smooth Transitions
- 1-4 second transitions between screens
- Door slide animations
- Handle rotation animation
- Arrow pulsing (0.8 seconds)
- Auto-cleanup removes off-screen elements after 10 seconds

#### Visual Feedback
- Button click sounds
- Door sounds
- Screen transitions
- Color changes
- Pulsing "FINISH AND EXIT" button

### Technical Details

#### Detection Timing
- Initial detection: 1 second after deposit button press
- Display update: Every 0.5 seconds
- Distance check: 250 units for auto-exit
- Data transmission: After customer exits

#### Zone Requirements
- **DepoMin/DepoMax:** Defines deposit detection area
- **BaseMin/BaseMax:** Defines base storage tracking area
- Both zones must be configured with entity markers

#### Data Format
Sends to Banker Main E2:
- Rack ownership table (Steam ID → Rack data)
- Printer count table (Steam ID → Printer types/counts)
- Transmitted within 200-unit radius

### Integration with Other E2s

#### Works With Membership E2
- Receives membership data automatically
- Displays tier information on screen
- Shows real-time tier benefits
- Updates every 60 seconds

#### Works With Banker E2 Main
- Sends rack ownership data
- Sends printer count data for insurance
- Automatic transmission on customer exit
- Clears local data after successful send

#### Works With Security E2
- Closes security door during deposit
- Coordinates with door access system
- Owner receives deposit notifications

### Setup Requirements

#### Required Wiring
- **EGP:** Main touchscreen display (wirelink)
- **Door:** Front fading door (wirelink)
- **DoorSeurity:** Back security door (wirelink)
- **Button:** User interaction input (number)
- **DepoMin/DepoMax:** Deposit area boundaries (entities)
- **BaseMin/BaseMax:** Base storage area boundaries (entities)

#### Required Outputs
- **TextScreen:** String output for external displays

#### Zone Setup
1. Place DepoMin/DepoMax markers to define deposit detection area
2. Place BaseMin/BaseMax markers to define storage tracking area
3. Ensure zones don't overlap
4. Test detection before opening to customers

### Best Practices

#### For Bank Owners
- Keep deposit area well-lit and accessible
- Place screen at comfortable viewing height
- Ensure sufficient space in deposit zone
- Test detection with various printer types
- Check data transmission to Main E2 regularly
- Monitor base area for accurate tracking

#### For Customers
- Always deposit racks WITH printers inside
- Wait for detection confirmation
- Don't leave until seeing printer counts
- Click "FINISH AND EXIT" when done
- Contact owner if detection seems incorrect

### Troubleshooting

**If Printers Not Detected:**
- Check printer is fully inside DepoMin/DepoMax zone
- Verify DepoMin and DepoMax entities are valid
- Ensure 1-second detection delay has passed
- Check printer class name is recognized

**If Screen Won't Reset:**
- Customer may be too close (within 250 units)
- Try using "FINISH AND EXIT" button
- Owner can restart E2 if stuck
- Check distance monitoring is working

**If Data Not Sending to Main E2:**
- Verify Banker E2 Main is within 200 units
- Check Main E2 is named correctly: "Cozzah's Banker E2 Main"
- Look for transmission confirmation in console
- Restart both E2s if needed

**If Door Won't Open:**
- Check wirelinks are connected
- Verify fading door tool is applied
- Test door manually first
- Check E2 console for errors

---

## Printer EGP Display E2

### Overview
The Printer EGP Display E2 is a decorative/functional display system that shows an animated printer rack with real-time stats, collection animations, and special features for law enforcement payments and bank advertising.

### Main Features

#### Visual Display (Decorative)
Creates an animated printer rack display showing:
- **Storage Capacity Bar:** 0-$300k range with visual progress
- **Temperature Gauge:** With color changes based on heat
- **Profits Counter:** Running total display
- **Cooler Rating:** Efficiency display
- **Money Handle Animation:** 7-step collection sequence
- **Loading Spinner:** Animated loading indicator
- **Total Bank Profit:** Lifetime earnings counter

#### Simulated Automation
- **Printing Simulation:** Adds $700-720 every 3 seconds
- **Temperature System:** 
  - Randomly rises during printing
  - Cools when reaching 100°C
  - Color changes based on temperature
- **Storage System:**
  - Capacity bar (0-$300k)
  - Auto-collection trigger at $250k-300k
  - Visual fill animation
- **Collection Animation:** 7-step sequence when collecting

### RGB Color Cycling System

#### Dynamic Prop Coloring
- Cycles through RGB color spectrum
- Applies to 8 entities (E1-E8)
- Smooth hue transitions using HSV color space
- Base hue shifts with sin wave: 260 + sin(time × 5) × 180
- Updates every 0.1 seconds
- Creates flowing rainbow effect

#### Prop Support
Wire up to 8 props for color cycling:
- E1, E2, E3, E4, E5, E6, E7, E8
- All props get same synchronized color
- Only affects valid, owned entities

### Law Enforcement Payment System

#### Cop Payment Feature
- **Command:** `*cops` - Pays all nearby law enforcement
- **Amount:** $100,000 per officer
- **Radius:** 1000 units
- **Job Detection:** Automatically identifies:
  - Mayor
  - Police Officer / Police Chief
  - SWAT / SWAT Medic / SWAT Sniper
  - Secret Service
  - CUSTOM_POLICE
- **Usage:** Reward cops who help defend during raids
- **Notifications:** All paid officers receive confirmation

### Bank Advertisement System

#### Automatic Advertising
- **Message Count:** 33 unique bank advertisements
- **Frequency:** Every 5 minutes (configurable via MinAdvert variable)
- **Format:** Branded messages with bank name
- **Type:** GPT-generated banking taglines
- **Examples:**
  - "Secure your cash with us, or someone else will!"
  - "Don't wait until you're raided. Deposit now!"
  - "Trust issues? Perfect. That's why [BANK] was built like a bunker!"
  - "Because every dollar deserves a panic room!"

#### Advertisement Messages
All 33 messages include humor and banking themes:
- Security-focused taglines
- Anti-raid messaging
- Customer protection themes
- Professional banking language
- Playful and engaging tone

### Prop Mass & Collision System

#### Physical Properties
- **Mass:** 3000 per prop
- **Collision Group:** 20
- **Supports:** 12 props (Prop01-Prop12)
- **Command:** `*setstate` - Refreshes all prop settings
- **Purpose:** Prevents props from moving during interactions

### Admin Commands

#### Available Commands
- `*cops` - Pay $100k to all nearby law enforcement
  - Scans 1000-unit radius
  - Identifies valid law enforcement jobs
  - Pays each officer $100k
  - Sends confirmation to all recipients
  
- `*setstate` - Refresh prop mass/collision settings
  - Applies 3000 mass to all 12 props
  - Sets collision group to 20
  - Useful after prop respawn or duplication

### Setup Requirements

#### Required Wiring
- **EGP:** Main display screen (wirelink)
- **User:** User entity for interaction (entity)
- **Prop01-Prop13:** Physical printer props (entities, 13 total)
- **E1-E8:** Props for RGB color cycling (entities, 8 total)

#### Outputs
- **Profit:** Number output showing simulated profits

### Technical Details

#### Update Rates
- **Color Cycling:** Every 0.1 seconds (10 FPS)
- **Printing Simulation:** Every 3 seconds
- **Advertisement:** Every 5 minutes (300 seconds)
- **Visual Updates:** Real-time

#### Configuration Variables
- **MinAdvert:** Advertisement interval in minutes (default: 5)
- **FPS:** Animation frame rate (default: 30)
- Easily adjustable at top of E2 code

### Integration

#### Standalone Operation
- Works independently
- No required integration with other E2s
- Can be used purely decoratively
- Cop payment feature works anywhere

#### Optional Integration
- Can coordinate with Security E2 for raid detection
- Advertisement messages reference bank name from config
- Color cycling can match bank theme

### Best Practices

#### For Bank Owners
- Place display prominently for visibility
- Use cop payment during/after raids
- Adjust advertisement frequency to preference
- Wire all 12 props for best visual effect
- Test `*setstate` command after setup

#### Prop Setup
- Use printer/rack props for authenticity
- Arrange props artistically
- Ensure props are owned by you
- Apply mass settings before opening to customers

### Troubleshooting

**If Colors Not Changing:**
- Verify E1-E8 entities are wired
- Check you own all entities
- Ensure entities are valid props
- Look for console errors

**If Cop Payment Fails:**
- Check cops are within 1000 units
- Verify you have sufficient funds
- Ensure cop jobs match detection array
- Test with valid police job

**If Props Keep Moving:**
- Use `*setstate` command
- Check mass applied properly (3000)
- Verify collision group set to 20
- Ensure props aren't constrained

**If Advertisements Not Sending:**
- Check MinAdvert variable setting
- Verify timer is running
- Look for console output
- Ensure chat isn't blocked

---

## Bank Prop Grayscale Cycling E2

### Overview
The Bank Prop Grayscale Cycling E2 is a visual enhancement system that creates dynamic lighting effects for your bank's props. It provides smooth color-cycling animations during normal operation and switches to dramatic red flashing during alarm situations. This E2 works in conjunction with the Security E2 to provide visual feedback of your bank's security status.

### How It Works

#### Automatic Prop Detection
On spawn, the E2 automatically:
- **Scans 2000-unit radius** for compatible props
- **Owner Filter:** Only affects props owned by the E2's owner
- **Material Assignment:** Sets all props to "lights/white" material
- **One-time Scan:** Only scans once on spawn for performance

#### Supported Models
- **Hunter Plates:** plate6, plate8, plate3, plate175, plate4, plate025, plate5, plate2
- **Fences:** props_c17/fence01a
- All sizes of hunter plates supported

### Normal Mode - Grayscale Cycling

#### Visual Effect
When no alarm is active:
- **Smooth Animation:** Props cycle through grayscale colors
- **Color Range:** Black (0) to light gray (200)
- **Wave Effect:** Uses sine wave calculation: `100 + sin(time × 5) × 100`
- **Update Rate:** Refreshes every 0.1 seconds (10 times per second)
- **Visual Effect:** Creates a subtle "breathing" or "pulsing" effect
- **Alpha:** Full opacity (255)

#### Professional Aesthetic
- Subtle and modern
- Won't clash with other colored elements
- Works well in dark or light environments
- Adds life to static builds
- Not distracting to customers

### Alarm Mode - Red Flashing

#### Visual Effect
When alarm is triggered (AlarmActive = 1):
- **Color:** Bright red (255, 0, 0)
- **Pattern:** Alternates between visible and invisible
- **Update Rate:** Every 1 second
- **Flash Sequence:**
  - 1 second: Full red (alpha 255)
  - 1 second: Invisible (alpha 0) OR white (for fences)
- **Effect:** Creates urgent, unmistakable alert

#### Fence Exception
Fence props (`props_c17/fence01a`) have special behavior:
- **Normal:** Participate in grayscale cycling
- **Alarm:** Flash between red and white (always visible)
- **Purpose:** Ensure fences remain visible for safety

### Material Override System

#### Privacy Screen Feature
When Button input is active:
- **Button = 1 (ON):** Changes Prop to `cssconvertedmats/nuke_metalgrate_01`
- **Button = 0 (OFF):** Changes Prop to `cssconvertedmats/train_largemetal_door_01`
- **Use Case:** Create privacy screens that can be toggled
- **Requirements:** Wire a button and a specific prop entity

#### Material Exclusions
Props with certain materials are automatically excluded:
- **Lightbar Material:** `cssconvertedmats/lightbareon01`
- **Purpose:** Prevents interference with special lighting props
- **Behavior:** These props keep their original material and color

### Setup Requirements

#### Required Wiring
- **AlarmActive:** Wire to Security E2's "AlarmActive" output (number)
  - `0` = Normal mode (grayscale cycling)
  - `1` = Alarm mode (red flashing)
  
#### Optional Wiring
- **Button:** Button input for manual material override (number)
- **Prop:** Specific entity for privacy screen material switching (entity)

#### Prop Setup
1. Build your bank structure with compatible props
2. Spawn this E2 within 2000 units
3. E2 automatically finds and configures all your props
4. Props begin cycling immediately
5. No manual configuration needed

### Integration with Other E2s

#### Required Integration
- **Security E2:** Must wire AlarmActive output
  - Security E2 triggers alarm when threats detected
  - This E2 provides visual feedback
  - Automatic synchronization

#### Works Independently For
- Grayscale cycling animation (runs without Security E2)
- Prop detection and setup
- Material management
- Privacy screen feature

### Performance Optimization

#### Efficient Operation
- **Single Scan:** Only scans for props once on spawn
- **Owner Filter:** Only affects your props (no neighbor conflicts)
- **Material Check:** Skips props with exempt materials
- **Optimized Updates:** Different rates for normal vs alarm mode
- **No FindInSphere Spam:** Uses persistent array after initial scan

### Visual Examples

#### Normal Operation Sequence
- Start: Dark gray (RGB 0, 0, 0)
- Rise: Medium gray (RGB 100, 100, 100)
- Peak: Light gray (RGB 200, 200, 200)
- Fall: Medium gray (RGB 100, 100, 100)
- Return: Dark gray (RGB 0, 0, 0)
- **Duration:** Full cycle takes ~1.26 seconds
- **Effect:** Smooth, professional breathing animation

#### Alarm Operation Sequence
- Flash On: Bright red (RGB 255, 0, 0) at full opacity
- Flash Off: Same red at zero opacity (invisible)
- **Duration:** 1 second per state
- **Effect:** Urgent, impossible to miss
- **Fence Special:** Flashes between red and white (always visible)

### Technical Details

#### Update Rates
- **Normal Cycling:** Every 0.1 seconds (10 FPS)
- **Alarm Mode:** Every 1 second (1 FPS)
- **Scan Range:** 2000 units (one-time on spawn)

#### Color Calculation
Normal mode uses: `GrayValue = 100 + sin(curtime() × 5) × 100`
- Base: 100 (middle gray)
- Amplitude: 100 (range of oscillation)
- Frequency: 5 (speed of cycle)
- Result: Oscillates between 0 and 200

#### Material Persistence
- E2 checks material before applying colors
- Prevents interference with props that change materials
- Resets to proper material on E2 respawn
- Lightbar materials preserved

### Best Practices

#### Prop Selection
- Use hunter plates for walls, floors, decorative elements
- Use fences for barriers or doors
- Ensure props are within 2000 units of E2
- Own all props you want affected

#### Color Considerations
- Grayscale cycling is subtle and professional
- Won't clash with other colored elements
- Red alarm is universally recognized
- Works well in dark or light environments
- Complements most bank themes

#### Placement
- Place E2 centrally for maximum coverage
- Consider bank layout when placing
- Test coverage by spawning props at edges
- Ensure all important props are within range

### Troubleshooting

**If Props Don't Change Color:**
- Check prop is within 2000 units of E2
- Verify you own the prop
- Ensure prop isn't using excluded material (lightbareon01)
- Check if prop model is in supported list
- Respawn E2 to rescan

**If Cycling Seems Choppy:**
- Normal - updates every 0.1 seconds
- Should appear smooth to human eye
- Server lag may affect appearance
- Consider reducing total prop count if severe
- Check server performance

**If Alarm Doesn't Flash:**
- Verify AlarmActive wire is connected
- Check Security E2 is triggering alarm
- Test by manually setting AlarmActive to 1
- Check console for errors
- Ensure props are in Props array

**If Some Props Excluded:**
- Props with special materials (lightbareon01) are intentionally skipped
- Check if prop has excluded material
- Re-material props if you want them included
- E2 will pick them up on next spawn
- Use supported model types

**If Privacy Screen Not Working:**
- Check Button wire is connected
- Verify Prop entity is wired
- Ensure Prop is valid and owned
- Test button input manually
- Check material names are correct

---

## How to Use the Bank

### Customer Guide

#### Getting Started
Deposit your printers at the deposit station. The bank owner will be notified automatically and assign you a private storage slot with a unique customer access code.

#### Automatic Payments
Once assigned, you'll receive automatic payments from your printers after tax deduction. Payments are sent directly to you - no collection needed!

#### Reducing Tax
Purchase a permanent membership tier upgrade from the membership screen in the lobby:

| Tier | Cost | Tax Rate | Insurance |
|------|------|----------|-----------|
| **Copper** | Free | 35% | 0% |
| **Silver** | $1,000,000 | 25% | 50% |
| **Gold** | $2,000,000 | 20% | 75% |
| **Diamond** | $5,000,000 | 10% | 100% |

> **Note:** Memberships are permanent - you keep your tier for life across all server sessions.

#### Accessing Your Storage
1. Go to the back entrance (to the right of main doors)
2. Enter your customer code into the wire keypad (labeled by text screen)
3. The first fading door will open automatically
4. Press 'E' to open your assigned storage doors (they glow green when you're nearby)
5. Doors stay open for 10 seconds - you have 5 minutes of total access time
6. **Forgot your code?** Type `!code` in chat to have it resent

#### Storage Rules
- You may store shipments and printers in your slot as long as space is available
- Keep your printers accessible so the banker can collect from them
- Larger team-sized storage slots may be available - contact the banker to inquire about upgrades

#### Insurance Protection
- Insurance protects your equipment if the bank is raided
- Coverage percentage depends on your membership tier
- Copper tier has NO insurance (0%)
- Diamond tier has FULL insurance (100%)
- Owner pays insurance claims after raids

### ⚠️ WARNING
**Always deposit racks WITH printers inside, or they will be destroyed! Empty racks are not accepted.**

---

## Credits
Created by Cozzah

## Support
For issues, questions, or feature requests, please contact the developer.

---

*Last Updated: 18/10/24*
