# Cozzah's Bank System - Complete Documentation

See [Development Notes](DEVELOPMENT.md) for information about the codebase.

## Table of Contents
- [Banker E2 Main](#banker-e2-main)
- [Bank Depo E2](#bank-depo-e2)
- [Bank Membership E2](#bank-membership-e2)
- [Bank Security E2](#bank-security-e2)
- [Bank Prop Grayscale Cycling E2](#bank-prop-grayscale-cycling-e2)
- [How to Use the Bank (Customer Guide)](#how-to-use-the-bank)

---

## Banker E2 Main

Single-file data: Editable variables are placed at the top of the E2 file when only used within that chip
Shared data: Common values used across multiple E2s have been moved to a centralized config file

### Technical Details

#### Update Rates
- **Cycling Mode:** Every 0.1 seconds (10 FPS)
- **Alarm Mode:** Every 1 second
- **Scan Range:** 2000 units (one-time on spawn)

#### Material Persistence
- E2 checks material before applying colors
- Prevents interference with props that change materials
- Resets to proper material on E2 respawn

### Visual Examples

#### Normal Operation
- Start: Dark gray (almost black)
- Peak: Light gray (clearly visible)
- Return: Back to dark gray
- Continuous smooth cycle
- Professional, modern aesthetic

#### During Alarm
- Flash bright red (1 second)
- Disappear/turn white (1 second)
- Flash bright red again
- Impossible to miss
- Clear emergency indicator

### Best Practices

#### Color Considerations
- Grayscale cycling is subtle and professional
- Won't clash with other colored elements
- Red alarm is universally recognized
- Works well in dark or light environments

### Troubleshooting

**If Props Don't Change Color:**
- Check prop is within 2000 units of E2
- Verify you own the prop
- Ensure prop isn't using excluded material
- Check if prop model is in supported list

**If Cycling Seems Choppy:**
- Normal - updates every 0.1 seconds
- Should appear smooth to human eye
- Server lag may affect appearance
- Consider reducing prop count if severe

**If Alarm Doesn't Flash:**
- Verify AlarmActive wire is connected
- Check Security E2 is working
- Test by manually setting AlarmActive to 1
- Check console for errors

**If Some Props Excluded:**
- Props with special materials are intentionally skipped
- Check if prop has "lightbareon01" material
- Re-material props if you want them included
- E2 will pick them up on next spawn

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

### ⚠️ WARNING
**Always deposit racks WITH printers inside, or they will be destroyed! Empty racks are not accepted.**

---

## Credits
Created by Cozzah

## Support
For issues, questions, or feature requests, please contact the developer.

---

*Last Updated: 2024* Overview
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
- Their membership tier (Copper/Silver/Gold/Diamond) or Guard status
- Total tax collected from that specific rack
- Color-coded tier display (Diamond=cyan, Gold=yellow, Silver=gray, Copper=brown, Guards=blue)

### Tax Rates by Tier
| Tier | Tax Rate | Insurance |
|------|----------|-----------|
| Copper (Default) | 35% | 0% |
| Silver | 25% | 50% |
| Gold | 20% | 75% |
| Diamond | 10% | 100% |
| Security Guards | 0% | N/A |

### Smart Rack Tracking
- Automatically remembers which customer owns each rack
- Tracks individual tax totals for every rack in your bank

### Admin Commands
> **Note:** All admin commands are prefixed with `*`

#### Help & Information
- `*help` - Displays list of all admin commands
- `*customers` - Shows complete list of all active customers with their membership tiers and tax totals collected
- `*profit` - Displays total tax profit summary including total collected, number of racks, and average per rack

#### Financial Management
- `*wire [player] [amount]` - Sends money to a specified player (minimum 1000)
  - Example: `*wire John 50000`
  - Both you and the recipient get confirmation messages
  - Can be used if needing to pay insurance if raided

#### Rack Management
- `*owner [player]` - Manually assigns a player as the owner of the rack you're currently looking at
  - Example: Look at a rack, then type `*owner John`
  - Useful if ownership detection fails or you need to reassign a rack
  - Shows confirmation with old and new owner names

#### Bank Closing System
- `*close [minutes] [notification_amount]` - Initiates bank closing procedure with automated customer notifications
  - Example: `*close 30 6` (closes in 30 minutes with 6 notifications)
  - Automatically calculates notification intervals
  - Sends timed warnings to all customers via chat
  - Countdown format shows time remaining in a readable format (e.g., "25 mins 30 secs")
  - Final notification authorizes you to destroy remaining printers
  - Customers get progressive warnings like "15 mins", "10 mins", "5 mins", etc.

### Integration with Other E2s

#### Works With Membership E2
- Automatically receives updated membership data
- Uses tier information to calculate correct tax rates
- Displays tier names and colors in customer list

#### Works With Security E2
- Receives guard roster automatically
- Guards get 0% tax rate on all collections
- Guard status shown in blue on HUD

### Setup Requirements
- Wire to a EGP HUD (for collection display)
- Must have the Membership E2 and Security E2 placed nearby for full functionality
- No additional configuration needed - works automatically once placed

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
- **Timer System:** 10-minute collection reminder with sound effect
- **Distance Tracking:** HUD clears when you move away from racks

### Technical Notes
- Updates every 0.1 seconds for smooth HUD display
- Plays cash sound every 10 minutes as collection reminder
- Automatically syncs with other E2s via remote events
- No manual data entry required - everything is automatic

---

## Bank Depo E2

### Overview
The Bank Depo E2 provides an interactive deposit station where customers can safely store their printers and bitminer towers at your bank. It features an animated touchscreen interface with a secure fading door system that allows customers to pass their equipment through for storage.

### How It Works

#### Customer Experience

**Starting the Deposit:**
- Customer approaches the screen and clicks the handle in the center
- Screen animates with a 180-degree rotation
- Background elements slide off screen with sound effects
- Main deposit interface appears

**Deposit Button:**
- Customer clicks the "DEPOSIT" button
- Activates the deposit screen showing an animated pointing arrow
- Opens the fading door (Door output = 1)
- Arrow pulses continuously to indicate deposit area
- Screen displays: "DEPOSIT YOUR PRINTERS AND RACK HERE"

**Depositing Equipment:**
- Customer passes their printers/towers through the opened fading door
- Owner receives automatic notification to store the equipment in appropriate slots
- Customer can take their time - door remains open while screen is active

**Finishing Up:**
- Customer clicks "FINISH AND EXIT" button
- Door automatically closes (Door output = 0)
- Screen resets to home state with handle
- Sound effects play to confirm exit
- Next customer can now use the screen

### Key Features

#### Smart User Management
- **One User at a Time:** Once someone starts depositing, the screen locks to that player until they exit
- **Auto-Reset:** If customer walks more than 250 units away from screen, it automatically resets
  - Prevents screen from being left open indefinitely
  - Allows next customer to use it immediately
  - Door closes automatically for security

#### Visual Feedback
- **Animated Transitions:** Smooth sliding and rotating animations guide the user
- **Sound Effects:** Audio cues for every interaction
- **Clear Instructions:** Large, easy-to-read text tells customers exactly what to do
- **Pulsing Arrow:** Animated directional indicator shows where to deposit equipment

#### Security Features
- **Distance Monitoring:** Constantly checks if customer is nearby
- **Automatic Door Control:** Door only opens during active deposit session
- **Clean Reset:** Screen fully resets between customers to prevent confusion
- **Owner Notifications:** You get messaged when customers deposit equipment

### Setup Requirements

#### Required Wiring
- **EGP Screen:** Wire to an EGP screen for the user interface
- **Door Output:** Wire the "Door" output to a fading door button/controller
  - Set to NOT "Toggle" mode on your fading door
  - Door should be positioned where customers can pass printers through
  - Recommended: Make it a wall opening rather than a walkable door

#### Integration
- Works independently
- No connection to other E2s required
- Self-contained deposit system
- Can have multiple deposit stations at different locations

### Owner Workflow
1. Customer clicks deposit and passes equipment through
2. You receive notification message
3. You retrieve equipment from deposit area
4. You place equipment in assigned storage slot
5. Customer clicks exit when done

### Screen States

#### Home Screen (Default)
- Bank branding at top
- Large handle in center with "START" text

#### Deposit Active Screen
- Bank branding remains visible
- Animated arrow pointing to deposit area
- Large instruction text
- "FINISH AND EXIT" button prominently displayed
- Pulsing animation to draw attention

### Technical Details
- **Animation Speed:** 1-4 second transitions for smooth experience
- **Auto-Cleanup:** Removes off-screen elements after 10 seconds to optimize performance
- **Sound Library:** Uses various door and button sounds for immersion

### Customer Instructions
1. Click the rotating handle to begin
2. Click the "DEPOSIT" button
3. Pass your printers/towers through the opened slot
4. Wait for bank owner to confirm storage
5. Click "FINISH AND EXIT" when done
6. Don't walk away - screen will auto-close if you go too far

### Best Practices
- Check the deposit area frequently when customers are active
- Respond quickly to deposit notifications
- Test the 250-unit auto-close distance to ensure it works with your layout

---

## Bank Membership E2

### Overview
The Bank Membership E2 manages all customer membership tiers and upgrades at your bank. It provides an interactive touchscreen where customers can purchase permanent membership passes (Silver, Gold, or Diamond) that reduce their tax rates and provide insurance benefits. All membership data is automatically saved to file and synced with the Banker E2 Main chip for seamless tax calculations.

### Membership Tiers & Pricing

| Tier | Tax Rate | Insurance | Cost | Notes |
|------|----------|-----------|------|-------|
| **Copper** (Default) | 35% | 0% | Free | Everyone starts here |
| **Silver** | 25% | 50% | $1,000,000 | 10% less tax than Copper |
| **Gold** | 20% | 75% | $2,000,000 | 15% less tax than Copper |
| **Diamond** | 10% | 100% | $5,000,000 | Best tax rate available |

> All memberships are **one-time permanent passes** that never expire

### How It Works

#### Customer Experience

**Starting the Process:**
- Customer approaches the screen and clicks the "START" button
- Background slides away with animations
- Membership tier cards slide into view
- Each tier displays tax rate, insurance, and cost

**Viewing Tiers:**
- Customer sees all four membership options
- Color-coded cards (Copper=brown, Silver=gray, Gold=yellow, Diamond=cyan)
- Clear pricing and benefit information
- Visual icons representing each tier
- "PERM PASS" label on purchasable tiers

**Purchasing a Tier:**
- Customer clicks on desired membership button
- **Already Own Tier:** Gets message they already have same/higher tier
- **Insufficient Funds:** Gets retry cooldown timer message
- **Successful Purchase:**
  - Money request popup appears
  - Customer accepts payment
  - Membership instantly upgraded (Updated to main E2 every 10 mins)
  - Congratulations message sent
  - Cash register sound plays

**Anti-Downgrade Protection:**
- System prevents purchasing lower tiers
- If Gold member clicks Silver, they're told they already have better
- Protects customers from wasting money

### Auto-Save System
- **File Storage:** All membership data saved to "banker_memberdata.txt"
- **Persistent Data:** Memberships stay after server restarts
- **Auto-Load:** Loads previous data when E2 is spawned
- **Real-Time Sync:** Updates file immediately after each purchase

### Admin Commands
> **Note:** All admin commands are prefixed with `*`

#### Membership Management
- `*give [player] [tier_number]` - Manually grants membership tier to a player for free
  - Tier numbers: `1`=Silver, `2`=Gold, `3`=Diamond
  - Example: `*give John 3` (gives Diamond to John)
  - Prevents downgrades (can't give Silver to Gold member)
  - Sends confirmation to both admin and player
  
**Useful for:**
- Promotional giveaways
- Fixing payment issues
- Rewarding loyal customers
- Staff benefits

**Usage Examples:**
```
*give John 1     → Grants Silver membership to John
*give Sarah 2    → Grants Gold membership to Sarah
*give Mike 3     → Grants Diamond membership to Mike
```

### Key Features

#### Smart Payment Handling
- **Money Request System:** Uses in-game payment popup (5-second timeout)
- **Exact Amounts:** Verifies correct payment amount received
- **Auto-Refund:** If wrong amount paid, automatically refunds customer
- **Spam Protection:** 1-second cooldown between button clicks
- **Insufficient Funds Protection:** Shows time until customer can retry

#### User Experience
- **Distance Monitoring:** Auto-resets if customer walks 100+ units away
- **One User at a Time:** Locks screen to current user until they finish
- **Visual Feedback:** Sound effects for all interactions
- **Clear Messaging:** Color-coded chat notifications
- **Professional Branding:** Bank logo and clean interface

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

#### Works With Security E2 (Indirectly)
- Guards always get 0% tax regardless of membership
- Guard status overrides membership benefits
- Managed by Security E2, not Membership E2

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
Four membership cards displayed side-by-side, each card shows:
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
6. Receive membership upgrade within 10 mins

#### For Existing Members
- Screen will prevent purchasing same/lower tier
- Can only upgrade to higher tiers
- Memberships are permanent (never expire)

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
- Back up membership file regularly

#### Pricing Recommendations
- Current prices balanced for typical server economy
- Diamond tier = significant investment for serious customers
- Silver tier = accessible entry point
- Gold tier = middle ground for established players

### Troubleshooting

**If Memberships Don't Sync:**
- Check Banker E2 Main is within 200 units
- Verify both E2s belong to same owner
- Check console for "E2 Chip initialized" message

**If Payments Fail:**
- Customer needs to pay exact amount
- Must accept payment within 5 seconds

**If File Won't Save:**
- Verify E2 has write permissions
- Console will show save confirmation

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

**Job Requirements:**
- Only Security Guards and Mercenaries can apply/be hired

**Automatic Payroll:**
- Guards paid every 10 minutes (configurable timer)
- Default salary: $900,000/hour ($150,000 per 10-minute payment)
- Customizable salary via admin command
- Payment messages sent to all active guards

#### Guard Benefits
- **0% Tax Rate:** Guards pay no tax when depositing at the bank
- **Full Door Access:** Can open main security doors (Door1-3) and assigned guard doors
- **Salary Notifications:** Get paid with time-until-next-payment message
- **Job Protection:** Auto-removed if they change to unauthorized job

### Guest System

#### Guest Management
- **Temporary Access:** Owner can add guests for secure area access
- **Timed Passes:** Option to set expiration time in minutes
- **Auto-Expiration:** Guests automatically removed when time expires
- **Notifications:** Both owner and guest notified on add/remove

#### Guest Permissions
- Can open main security doors (Door1-3)
- Can access guest-authorized areas
- Cannot open customer printer doors
- No salary payments

### Door System

#### Main Security Doors (Door1-3)
- **Access:** Owner, Guards, and Guests only
- **Fading Doors:** Auto-open for 4 seconds then close
- **Sound Effects:** Opening and closing sounds
- **Protection:** Unauthorized players get denial message

#### Customer Printer Doors (Door4-13)
- **Individual Assignment:** Each door can be assigned to specific customer
- **Guard Doors:** Specific doors can be designated for all guards
- **Color Coding:**
  - Red (default): No customer nearby
  - Green: Assigned customer within 150 units
- **Auto-Close:** Closes after 10 seconds if owner present in area
- **Raid Toggle:** Can be force-toggled open during emergencies

#### Door Assignment Features
- Customers only access their assigned door
- Guards can access all "guard doors"
- Owner has universal access
- Offline customers tracked by Steam ID

### Security Zones

#### Zone Detection System
The E2 monitors multiple zones with intelligent player detection:

**Bottom Floor (MinCustomer/Max1 - Lower Half)**
- **Allowed:** Owner, Guards, Guests, Assigned Customers (with keypad access)
- **Customer Logic:** Only customers with door assignments allowed
- **Keypad Timer:** 5-minute access window after code entry
- **Warning:** "Allowed player inside" when customer present shown on HUD and via chat message

**Top Floor (MinCustomer/Max1 - Upper Half)**
- **Allowed:** Owner, Guards, Guests only
- **No Customers:** Customers not permitted on upper floor
- **Strict Access:** Triggers alarm immediately

**Secure Area (Min2/Max2)**
- **Allowed:** Owner, Guards, Guests only
- **High Security:** No customer access ever
- **Critical Zone:** Often used for vault/safe areas

#### Threat Detection
- **Lockpick Detection:** Instant alarm if anyone has lockpick/cracker equipped
- **Unauthorized Entry:** Alarm triggers for non-permitted players
- **Real-Time Monitoring:** Checks every second
- **Smart Floor Detection:** Uses Y-coordinate to determine floor level

### Alarm System

#### Automatic Alarm Features
- **Sound:** Plays "city_firebell_loop1.wav" on loop
- **Visual Integration:** Works with "bank prop grayscale cycling" E2
  - Props turn red and flash during alarm
  - Returns to normal when alarm stops
- **Notifications:** Messages sent to owner and all guards
- **Auto-Start:** Triggers on unauthorized player detection
- **Auto-Stop:** Silences when threat cleared

#### Raid HUD Display
Separate EGP screen (EGPRaid) shows security status:
- **SAFE (Green):** No threats detected
- **ALLOWED (Orange):** Authorized customer in restricted area
- **RAID (Red/Flashing):** Unauthorized player detected

### Panic Button System
- **Manual Trigger:** Physical button can be pressed for 3 seconds
- **Emergency Broadcast:** Sends 911 call
- **Message Format:** "PANIC ALARM TRIGGERED AT [OWNER]'S BANK, WE ARE BEING RAIDED [LOCATION]"
- **Sound Effect:** "I could use some help over here" voice line
- **Reset Command:** Admin can reset panic system

### Admin Commands
> **Note:** All admin commands are prefixed with `*`

#### Help & Information
- `*help` - Shows all admin commands
- `*guards` - Lists all current guards by name
- `*doors` - Shows all door assignments with player names

#### Guard Management
- `*add [player]` - Hires player as guard (must be Security Guard/Mercenary job)
  - Example: `*add John`
  - Plays celebration sound
  - Updates guard screen immediately
  - Sends congratulations message with salary info

- `*remove [player]` - Fires guard from position
  - Example: `*remove John`
  - Gives severance pay (1 hour salary) if still authorized job
  - Updates guard screen immediately
  - Sends termination message

- `*set [amount]` - Changes guard salary (minimum $50,000/hour)
  - Example: `*set 1200000` (sets to $1.2M/hour)
  - Notifies all guards of pay raise or cut
  - Updates guard application screen

#### Door Management
- `*adddoor [player]` - Assigns player to door you're looking at
  - Example: Look at door, type `*adddoor John`
  - Sends keypad code to assigned player
  - Both owner and player get confirmation

- `*deldoor` - Removes assignment from door you're looking at
  - Example: Look at door, type `*deldoor`
  - Player notified of removal
  - Door returns to unassigned state

- `*addguarddoor` - Makes door accessible to all guards
  - Example: Look at door, type `*addguarddoor`
  - All current and future guards can open it

- `*delguarddoor` - Removes guard access from door
  - Example: Look at door, type `*delguarddoor`
  - Returns door to owner-only or assignment-only

#### System Management
- `*update` - Manually refreshes guard screen display
- `*resetpanic` - Resets panic button and closes all toggle doors
  - Stops raid mode
  - Closes any stuck-open doors
  - Resets panic counter

### Player Commands
> **Note:** All player commands are prefixed with `!`

#### Help & Information
- `!help` - Shows all player commands

#### Guest Management
- `!add [player]` - Adds player as guest (permanent until removed)
  - Example: `!add Sarah`

- `!add [player] [minutes]` - Adds guest with time limit
  - Example: `!add Sarah 30` (30-minute guest pass)
  - Auto-expires and notifies both parties

- `!remove [player]` - Removes guest access
  - Example: `!remove Sarah`

- `!guests` - Shows list of current guests with time remaining

### Guard Application System

#### Customer-Facing Features
Screen shows current guard roster (4 slots maximum):
- "APPLY NOW" button for interested players
- Application requirements displayed:
  - Salary amount shown per 10 minutes
  - "SECURE PRINTING AT 0% TAX" benefit
  - Must be Security Guard or Mercenary job

#### Application Process
1. Player clicks "APPLY NOW" button
2. System checks if they're an authorized job
3. Owner receives notification with player name
4. Owner uses `*add` command to hire them
5. 60-second cooldown prevents button spam per player

### Setup Requirements

#### Required Wiring
- **EGP:** Main guard application/roster screen
- **EGPRaid:** Separate raid status display screen
- **Door1-3:** Main security fading doors (wirelink)
- **Door4-13:** Customer printer fading doors (wirelink, up to 10 doors)
- **PanicButton:** Physical button (wirelink)
- **MinCustomer/MaxCustomer:** Zone boundary markers (entities)
- **Max1:** Upper boundary for customer zone (entity)
- **Min2/Max2:** Secure zone boundaries (entities)
- **Keypad:** Keypad input (number)

#### Entity Setup
Place colored markers at zone corners:
- **MinCustomer/MaxCustomer:** Yellow
- **Max1:** Red
- **Min2/Max2:** Orange
- E2 automatically colors them on spawn
- Markers use "lights/white" material for visibility

#### Configuration Variables (in code)
- **Code:** Keypad code (default: 1234)
- **Salary:** Guard pay per hour (default: 900000)
- **Sound:** Alarm sound file
- **Location:** Text description for panic broadcasts

### Integration with Other E2s

#### Sends Data To
**Banker E2 Main:**
- Automatically sends guard roster
- Main E2 uses this for 0% tax rate
- Updates on guard add/remove
- Syncs via remote events

#### Receives Data From
**Bank Prop Grayscale Cycling E2:**
- Sends AlarmActive status
- Syncs alarm state with visual props
- Props flash red during alarms

#### Works Independently For
- Door access control
- Zone monitoring
- Guard payroll
- Guest management

### Key Features

#### Smart Detection
- **Floor-Based Logic:** Automatically determines which floor players are on
- **Find System Optimization:** Uses single find operation per tick
- **Distance Checks:** 150-unit proximity for door color changes
- **Owner Bypass:** Owner always has access everywhere

#### Security Features
- **Lockpick Auto-Detect:** Instant alarm on detection
- **Multi-Zone Coverage:** Three separate security zones
- **Offline Tracking:** Remembers customers who disconnect

#### User Experience
- **Sound Feedback:** Audio cues for all door actions
- **Visual Status:** Color-coded doors and raid HUD
- **Clear Messaging:** Professional chat notifications
- **Application UI:** Interactive hiring screen

### Guard Payment Example
With default $900,000/hour salary:
- Payment every 10 minutes
- Each payment: $150,000
- Guards notified: "Thank you for protecting COZZAH'S BANK, you have been paid $150K. Next payment in 10 mins"

### Best Practices

#### For Bank Owners
- Set salary competitively to attract good guards
- Regularly check chat for job applications
- Assign doors to customers as they deposit printers
- Use guest system for trusted friends/VIPs

#### Zone Setup Tips
- Make customer zone large enough for comfortable movement
- Keep secure zone (Min2/Max2) small and critical
- Ensure floor boundary is clearly mid-level between floors
- Place markers outside player view (underground/in walls)

### Troubleshooting

**If Doors Not Responding:**
- Verify wirelinks are connected
- Check door has fading door tool applied

**If Alarm Won't Stop:**
- Check all zones for players
- Look for players with lockpicks equipped
- Use `*resetpanic` to force reset

**If Data Not Syncing:**
- Verify Banker E2 Main is within 200 units
- Check both E2s owned by same player
- Look for "E2 Chip initialized" message in console

---

## Bank Prop Grayscale Cycling E2

### Overview
The Bank Prop Grayscale Cycling E2 is a visual enhancement system that creates dynamic lighting effects for your bank's props. It provides smooth color-cycling animations during normal operation and switches to dramatic red flashing during alarm situations. This E2 works in conjunction with the Security E2 to provide visual feedback of your bank's security status.

### How It Works

#### Automatic Prop Detection
- **On Spawn:** Scans 2000-unit radius for compatible props
- **Owner Filter:** Only affects props owned by the E2's owner
- **Material Assignment:** Automatically sets all props to "lights/white" material

**Supported Models:**
- `hunter/plates` (various sizes: plate6, plate8, plate3, plate175, plate4, plate025, plate5, plate2)
- `props_c17/fence01a`

### Normal Mode - Grayscale Cycling
When no alarm is active:
- **Smooth Animation:** Props cycle through grayscale colors
- **Color Range:** Black (0) to light gray (200)
- **Wave Effect:** Uses sine wave calculation for smooth transitions
- **Update Rate:** Refreshes every 0.1 seconds (10 times per second)
- **Visual Effect:** Creates a subtle "breathing" effect

### Alarm Mode - Red Flashing
When alarm is triggered:
- **Color:** Bright red (255, 0, 0)
- **Pattern:** Alternates between full opacity and transparent
- **Effect:** Creates urgent flashing alert
- **Speed:** Updates every 1 second
- **Special Case:** Fence props stay visible (white) instead of going transparent
- **Integration:** Automatically syncs with Security E2's AlarmActive output

### Setup Requirements

#### Required Wiring
- **AlarmActive:** Wire to Security E2's "AlarmActive" output (number)
  - `0` = Normal mode (grayscale cycling)
  - `1` = Alarm mode (red flashing)
- **Button:** Optional button input for manual material override
- **Prop:** Optional entity input for special prop material control

#### Prop Setup
1. Build your bank structure with compatible props
2. Spawn this E2
3. E2 automatically finds and configures all your props
4. Props begin cycling immediately

#### Recommended Prop Layout
- Use plates for walls, floors, and decorative elements
- Use fences for barriers or doors
- Ensure props are within 2000 units of E2

### Visual Behavior

#### Grayscale Cycling Details
- **Darkest Point:** RGB(0, 0, 0) - Pure black
- **Lightest Point:** RGB(200, 200, 200) - Light gray
- **Effect:** Creates subtle, professional ambient lighting
- **Purpose:** Adds life to static builds without being distracting

#### Alarm Flashing Details
- **Flash On:** RGB(255, 0, 0) - Bright red, full opacity
- **Flash Off:** RGB(255, 0, 0) - Bright red, zero opacity (invisible)
- **Exception:** Fences flash between red and white (always visible)
- **Interval:** 1-second toggle (1 second on, 1 second off)
- **Purpose:** Unmistakable visual alarm indicator

### Integration with Other E2s

#### Required Integration
- **Security E2:** Must be wired to AlarmActive output
- Security E2 triggers alarm when threats detected
- This E2 provides visual feedback
- Automatic synchronization

#### Works Independently For
- Grayscale cycling animation (runs without Security E2)
- Prop detection and setup
- Material management

### Special Features

#### Material Override System
When "Button" input is active and "Prop" is valid:
- **Button = 1:** Changes prop material to "cssconvertedmats/nuke_metalgrate_01"
- **Button = 0:** Changes prop material to "cssconvertedmats/train_largemetal_door_01"
- **Use Case:** Allows you to turn a prop into a privacy screen

#### Prop Exclusions
- **Light Barrier Props:** Props with "cssconvertedmats/lightbareon01" material are skipped
- Prevents interference with other base props
- Useful if you have mixed prop setups

### Performance Optimization
- **Single Scan:** Only scans for props once on spawn
- **Owner Filter:** Only affects your props (won't conflict with nearby builds)
- **Material Check:** Skips props already using exempt materials
- **Efficient Updates:** Uses optimized color setting methods

###
