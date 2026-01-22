# ArqelUi

A modern, feature-complete key system UI for Roblox executors. Supports custom validation, Junkie SDK integration, persistent key storage, and keyless mode.

## Installation

```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobru1/expert-octo-doodle/refs/heads/main/ArqelUi.luau"))()
```

## Quick Start

```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobru1/expert-octo-doodle/refs/heads/main/ArqelUi.luau"))()

-- Configure basic settings
Arqel.Appearance.Title = "My Script"
Arqel.Links.GetKey = "https://your-key-link.com"
Arqel.Links.Discord = "https://discord.gg/invite"

-- Define key validation logic
Arqel.Callbacks.OnVerify = function(key)
    return key == "YOUR_VALID_KEY"
end

-- Define what happens after successful validation
Arqel.Callbacks.OnSuccess = function()
    loadstring(game:HttpGet("YOUR_SCRIPT_URL"))()
end

-- Launch the key system
Arqel:Launch()
```

---

## Usage Examples

### Standard Key System

The most common implementation with custom key validation.

```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobru1/expert-octo-doodle/refs/heads/main/ArqelUi.luau"))()

Arqel.Appearance.Title = "Premium Script"
Arqel.Appearance.Subtitle = "Enter your license key"
Arqel.Links.GetKey = "https://pastebin.com/your-key"
Arqel.Links.Discord = "https://discord.gg/server"

-- Simple validation
Arqel.Callbacks.OnVerify = function(key)
    if key == "PREMIUM_2024" then
        return true
    end
    return false
end

Arqel.Callbacks.OnSuccess = function()
    print("Key validated successfully!")
    -- Load your main script
end

Arqel:Launch()
```

### Keyless Mode

Allows immediate access without requiring a key. Useful for free scripts or testing.

```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobru1/expert-octo-doodle/refs/heads/main/ArqelUi.luau"))()

Arqel.Appearance.Title = "Free Script"
Arqel.Options.Keyless = true

Arqel.Callbacks.OnSuccess = function()
    print("Script loaded without key!")
    loadstring(game:HttpGet("YOUR_SCRIPT_URL"))()
end

Arqel:Launch()
```

### Junkie SDK Integration

Integrates with the Junkie key system for automatic validation and HWID management.

```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobru1/expert-octo-doodle/refs/heads/main/ArqelUi.luau"))()

Arqel.Appearance.Title = "My Script"
Arqel.Links.Discord = "https://discord.gg/server"

-- Launch with Junkie configuration
Arqel:LaunchJunkie({
    Service = "YOUR_SERVICE_NAME",    -- Your Junkie service name
    Identifier = "YOUR_IDENTIFIER",    -- Your unique identifier
    Provider = "YOUR_PROVIDER_NAME"    -- Provider name
})

-- Keys are automatically validated through Junkie
```

### HTTP API Validation

Validate keys against your own API endpoint.

```lua
local HttpService = game:GetService("HttpService")

Arqel.Callbacks.OnVerify = function(key)
    local success, response = pcall(function()
        return game:HttpGet("https://api.yoursite.com/validate?key=" .. key)
    end)
    
    if success then
        local data = HttpService:JSONDecode(response)
        return {
            valid = data.valid,
            error = data.error or "UNKNOWN",
            message = data.message or "Invalid key"
        }
    end
    
    return false
end
```

---

## Configuration Reference

### Appearance

Customizes the visual appearance of the key system UI.

```lua
Arqel.Appearance = {
    Title = "Arqel",                              -- Main title displayed in header
    Subtitle = "Enter your key to continue",      -- Subtitle shown in status box
    Icon = "rbxassetid://95721401302279",        -- Icon displayed in header
    IconSize = UDim2.new(0, 30, 0, 30)           -- Size of the header icon
}
```

**Properties:**
- `Title` - The main title shown at the top of the window
- `Subtitle` - Secondary text shown in the status area
- `Icon` - Roblox asset ID for the icon (use `rbxassetid://ID` format)
- `IconSize` - UDim2 value controlling icon dimensions

### Links

Defines external links for key acquisition and community support.

```lua
Arqel.Links = {
    GetKey = "",    -- URL copied when user clicks "Acquire License"
    Discord = ""    -- Discord invite copied when user clicks Discord button
}
```

**Properties:**
- `GetKey` - Link to your key distribution page (Pastebin, website, etc.)
- `Discord` - Discord server invite link for support

### Storage

Controls persistent key storage on the user's system.

```lua
Arqel.Storage = {
    FileName = "Arqel_Key",    -- Name of file where key is saved
    Remember = true,            -- Whether to save keys to file
    AutoLoad = true             -- Whether to auto-validate saved keys on launch
}
```

**Properties:**
- `FileName` - Base name for the saved key file (automatically gets `.txt` extension)
- `Remember` - If `true`, saves valid keys to the user's executor workspace
- `AutoLoad` - If `true`, automatically loads and validates saved keys on launch

**Note:** Requires executor support for `writefile`, `readfile`, and `isfile` functions.

### Options

General behavior settings for the key system.

```lua
Arqel.Options = {
    Keyless = nil,      -- Keyless mode: nil (auto), true (force), false (disable)
    Blur = true,        -- Background blur effect when UI is open
    Draggable = true    -- Allow dragging the window by the header
}
```

**Properties:**
- `Keyless` - Controls keyless mode behavior:
  - `nil` - Automatically detects if keyless is available (Junkie mode only)
  - `true` - Forces keyless mode (no key required)
  - `false` - Disables keyless mode
- `Blur` - Adds a blur effect to the game behind the UI
- `Draggable` - Enables window dragging functionality

### Theme

Customizes all UI colors to match your branding.

```lua
Arqel.Theme = {
    Accent = Color3.fromRGB(139, 0, 0),          -- Primary accent color
    AccentHover = Color3.fromRGB(170, 20, 20),   -- Accent color on hover
    Background = Color3.fromRGB(15, 15, 15),     -- Main background
    Header = Color3.fromRGB(20, 20, 20),         -- Header background
    Input = Color3.fromRGB(25, 25, 25),          -- Input field background
    Text = Color3.fromRGB(255, 255, 255),        -- Primary text color
    TextDim = Color3.fromRGB(120, 120, 120),     -- Secondary text color
    Success = Color3.fromRGB(50, 205, 110),      -- Success state color
    Error = Color3.fromRGB(245, 70, 90),         -- Error state color
    Warning = Color3.fromRGB(255, 180, 50),      -- Warning state color
    StatusIdle = Color3.fromRGB(180, 80, 80),    -- Idle status color
    Discord = Color3.fromRGB(88, 101, 242),      -- Discord button color
    DiscordHover = Color3.fromRGB(114, 137, 218),-- Discord hover color
    Divider = Color3.fromRGB(45, 45, 70)         -- Divider line color
}
```

All colors use Roblox's `Color3.fromRGB()` format with values from 0-255.

### Callbacks

Functions called during key system lifecycle events.

```lua
Arqel.Callbacks = {
    OnVerify = nil,    -- Function to validate keys
    OnSuccess = nil,   -- Called after successful validation
    OnFail = nil,      -- Called when validation fails
    OnClose = nil      -- Called when UI is closed
}
```

#### OnVerify Function

Validates user-provided keys. Can return a boolean or a detailed table.

**Simple boolean return:**
```lua
Arqel.Callbacks.OnVerify = function(key)
    if key == "VALID_KEY" then
        return true
    end
    return false
end
```

**Detailed table return:**
```lua
Arqel.Callbacks.OnVerify = function(key)
    return {
        valid = false,                  -- Boolean: is the key valid?
        error = "KEY_EXPIRED",          -- Error code (see error codes section)
        message = "Your key has expired" -- Human-readable error message
    }
end
```

**Parameters:**
- `key` - The key string entered by the user (whitespace is automatically trimmed)

**Return values:**
- `true` - Key is valid, proceed to success
- `false` - Key is invalid
- `table` - Detailed response with `valid`, `error`, and `message` fields

#### OnSuccess Function

Called after successful key validation, before the UI closes.

```lua
Arqel.Callbacks.OnSuccess = function()
    print("Access granted!")
    loadstring(game:HttpGet("YOUR_SCRIPT_URL"))()
end
```

#### OnFail Function

Called when key validation fails. Receives the error message as a parameter.

```lua
Arqel.Callbacks.OnFail = function(errorMsg)
    print("Validation failed:", errorMsg)
end
```

**Parameters:**
- `errorMsg` - String containing the error message shown to the user

#### OnClose Function

Called when the user closes the UI without completing validation.

```lua
Arqel.Callbacks.OnClose = function()
    print("User closed the key system")
end
```

---

## Error Codes

When returning a table from `OnVerify`, you can specify these error codes for better user feedback:

| Error Code | Description | User Feedback |
|------------|-------------|---------------|
| `KEY_INVALID` | Key not found in system | "Key not found in system" |
| `KEY_EXPIRED` | License has expired | "License has expired" |
| `HWID_BANNED` | Hardware ID is banned | "Hardware banned" (auto-kicks) |
| `KEY_INVALIDATED` | Key was manually revoked | "License was revoked" |
| `ALREADY_USED` | One-time key already used | "One-time key already used" |
| `HWID_MISMATCH` | HWID limit reached | "HWID limit reached" |
| `SERVICE_NOT_FOUND` | Service doesn't exist | "Service not found" |
| `SERVICE_MISMATCH` | Wrong service for key | "Wrong service" |
| `PREMIUM_REQUIRED` | Premium access needed | "Premium required" |
| `ERROR` | Network or server error | "Network error" |

**Example usage:**
```lua
Arqel.Callbacks.OnVerify = function(key)
    local validUntil = "2024-12-31"
    local currentDate = os.date("%Y-%m-%d")
    
    if currentDate > validUntil then
        return {
            valid = false,
            error = "KEY_EXPIRED",
            message = "Your license expired on " .. validUntil
        }
    end
    
    return {valid = true}
end
```

---

## Notification System

Display temporary notifications to users.

```lua
Arqel:Notify(title, message, duration, iconType)
```

**Parameters:**
- `title` - Notification title (string)
- `message` - Notification message (string)
- `duration` - Display duration in seconds (number, default: 5)
- `iconType` - Icon style (string, see below)

**Icon Types:**
- `"info"` - General information (shield icon, accent color)
- `"success"` - Success messages (checkmark, green)
- `"error"` - Error messages (alert icon, red)
- `"warning"` - Warnings (alert icon, orange)
- `"shield"` - Security-related (shield icon, accent)
- `"key"` - Key-related (key icon, accent)
- `"copy"` - Copy confirmations (copy icon, green)
- `"discord"` - Discord-related (discord icon, blurple)
- `"close"` - Close/exit messages (X icon, red)

**Examples:**
```lua
Arqel:Notify("Welcome", "Key system loaded!", 3, "info")
Arqel:Notify("Success", "Key validated successfully!", 2, "success")
Arqel:Notify("Error", "Invalid license key", 4, "error")
Arqel:Notify("Warning", "Key expires in 3 days", 5, "warning")
```

---

## Utility Functions

### GetSavedKey()

Retrieves the saved key from storage (if available).

```lua
local savedKey = Arqel:GetSavedKey()

if savedKey then
    print("Found saved key:", savedKey)
else
    print("No saved key found")
end
```

**Returns:** `string` or `nil`

### ClearSavedKey()

Deletes the saved key from storage.

```lua
local success = Arqel:ClearSavedKey()

if success then
    print("Saved key cleared")
end
```

**Returns:** `boolean` - Success status

---

## Global Variables

ArqelUi sets and uses these global variables for state management:

### getgenv().SCRIPT_KEY

Contains the validated key string after successful validation.

```lua
Arqel.Callbacks.OnSuccess = function()
    print("Validated key:", getgenv().SCRIPT_KEY)
end
```

**Values:**
- `string` - The validated key
- `"KEYLESS"` - Set in keyless mode
- `"_CLOSED_"` - Set when UI is closed without validation
- `nil` - No key validated yet

### getgenv().ArqelLoaded

Indicates whether the ArqelUi system is currently active.

```lua
if getgenv().ArqelLoaded then
    print("Key system is already running")
end
```

**Values:**
- `true` - UI is currently loaded
- `false` - UI is not loaded

### getgenv().ArqelClosed

Indicates whether the user closed the UI without validation.

```lua
if getgenv().ArqelClosed then
    print("User closed the key system")
end
```

**Values:**
- `true` - UI was closed by user
- `false` - UI was not closed

---

## Methods

### Launch()

Launches the standard key system UI with custom validation.

```lua
Arqel:Launch()
```

**Behavior:**
1. Checks if a valid key already exists in `getgenv().SCRIPT_KEY`
2. If `AutoLoad` is enabled, attempts to load and validate saved key
3. If `Keyless` is `true`, shows keyless UI
4. Otherwise, shows standard key input UI
5. Waits for validation or closure
6. Sets `getgenv().SCRIPT_KEY` to the result

**Requirements:**
- `Callbacks.OnVerify` must be defined for validation

### LaunchJunkie(config)

Launches the key system with Junkie SDK integration.

```lua
Arqel:LaunchJunkie({
    Service = "YOUR_SERVICE_NAME",
    Identifier = "YOUR_IDENTIFIER", 
    Provider = "YOUR_PROVIDER"
})
```

**Parameters:**
- `config` - Table containing Junkie configuration:
  - `Service` (required) - Your Junkie service name
  - `Identifier` (required) - Your unique identifier
  - `Provider` (required) - Provider name

**Behavior:**
1. Loads the Junkie SDK from `https://jnkie.com/sdk/library.lua`
2. Configures Junkie with your service details
3. Automatically sets up validation through Junkie's API
4. Handles HWID checking, key expiration, and other Junkie features
5. Auto-detects keyless availability if `Options.Keyless` is `nil`

**Note:** Junkie integration handles validation automatically. You don't need to set `Callbacks.OnVerify`.

---

## Advanced Examples

### Multiple Key Tiers

```lua
local KEYS = {
    ["BASIC_KEY"] = "basic",
    ["PREMIUM_KEY"] = "premium",
    ["LIFETIME_KEY"] = "lifetime"
}

Arqel.Callbacks.OnVerify = function(key)
    local tier = KEYS[key]
    
    if tier then
        getgenv().USER_TIER = tier
        return true
    end
    
    return {
        valid = false,
        error = "KEY_INVALID",
        message = "Key not recognized"
    }
end

Arqel.Callbacks.OnSuccess = function()
    local tier = getgenv().USER_TIER
    
    if tier == "basic" then
        loadstring(game:HttpGet("BASIC_SCRIPT_URL"))()
    elseif tier == "premium" then
        loadstring(game:HttpGet("PREMIUM_SCRIPT_URL"))()
    elseif tier == "lifetime" then
        loadstring(game:HttpGet("LIFETIME_SCRIPT_URL"))()
    end
end
```

### Time-Based Keys

```lua
Arqel.Callbacks.OnVerify = function(key)
    -- Format: KEY_YYYYMMDD
    local keyDate = key:match("KEY_(%d%d%d%d%d%d%d%d)")
    
    if not keyDate then
        return {valid = false, error = "KEY_INVALID"}
    end
    
    local year = tonumber(keyDate:sub(1, 4))
    local month = tonumber(keyDate:sub(5, 6))
    local day = tonumber(keyDate:sub(7, 8))
    
    local keyTime = os.time({year = year, month = month, day = day})
    local currentTime = os.time()
    local daysPassed = math.floor((currentTime - keyTime) / 86400)
    
    if daysPassed > 30 then
        return {
            valid = false,
            error = "KEY_EXPIRED",
            message = "Key expired " .. daysPassed .. " days ago"
        }
    end
    
    return {valid = true}
end
```

### Webhook Logging

```lua
local HttpService = game:GetService("HttpService")

local function sendWebhook(title, description, color)
    local webhook = "YOUR_DISCORD_WEBHOOK_URL"
    
    local data = {
        embeds = {{
            title = title,
            description = description,
            color = color,
            timestamp = os.date("!%Y-%m-%dT%H:%M:%S")
        }}
    }
    
    pcall(function()
        HttpService:PostAsync(webhook, HttpService:JSONEncode(data))
    end)
end

Arqel.Callbacks.OnSuccess = function()
    local player = game.Players.LocalPlayer
    sendWebhook(
        "✅ Successful Login",
        "User: " .. player.Name .. "\nKey: " .. getgenv().SCRIPT_KEY,
        3066993 -- Green color
    )
end

Arqel.Callbacks.OnFail = function(errorMsg)
    local player = game.Players.LocalPlayer
    sendWebhook(
        "❌ Failed Login",
        "User: " .. player.Name .. "\nError: " .. errorMsg,
        15158332 -- Red color
    )
end
```

### Custom Theme Example

```lua
-- Dark purple theme
Arqel.Theme.Accent = Color3.fromRGB(138, 43, 226)
Arqel.Theme.AccentHover = Color3.fromRGB(159, 95, 226)
Arqel.Theme.Background = Color3.fromRGB(10, 10, 15)
Arqel.Theme.Header = Color3.fromRGB(15, 15, 25)
Arqel.Theme.Input = Color3.fromRGB(20, 20, 30)

-- Light theme
Arqel.Theme.Background = Color3.fromRGB(240, 240, 245)
Arqel.Theme.Header = Color3.fromRGB(255, 255, 255)
Arqel.Theme.Input = Color3.fromRGB(250, 250, 255)
Arqel.Theme.Text = Color3.fromRGB(20, 20, 20)
Arqel.Theme.TextDim = Color3.fromRGB(100, 100, 100)
```

---

## Troubleshooting

### Key Not Saving

**Issue:** Keys aren't being saved between sessions.

**Solutions:**
- Ensure your executor supports file functions (`writefile`, `readfile`, `isfile`)
- Check that `Arqel.Storage.Remember` is set to `true`
- Verify file permissions in your executor

### Validation Not Working

**Issue:** `OnVerify` callback isn't being called.

**Solutions:**
- Ensure `Callbacks.OnVerify` is defined before calling `Launch()`
- Check that your validation function returns a proper value (boolean or table)
- Test your validation logic independently

### UI Not Appearing

**Issue:** Key system UI doesn't show up.

**Solutions:**
- Check if `getgenv().ArqelLoaded` is already `true` (UI already running)
- Verify the script loaded without errors
- Check your executor's CoreGui compatibility

### Junkie Integration Failing

**Issue:** Junkie mode shows errors.

**Solutions:**
- Verify your Service, Identifier, and Provider values are correct
- Check network connectivity
- Ensure Junkie servers are operational

---

## Credits

Inspired by [Sentinel Key System](https://github.com/AsuraXowner/Sentinel-Open-Source/blob/main/SentinelKeySystem) created by [gamerfoxy](https://github.com/AsuraXowner).

## Links

- **Purchase**: [orqan.shop/product/ArqelUi](https://orqan.shop/product/ArqelUi/)
- **Discord Support**: [Orqan.lol/discord](https://Orqan.lol/discord)

## License

Licensed for use after purchase from [orqan.shop](https://orqan.shop/product/ArqelUi/). Redistribution without permission is prohibited.
