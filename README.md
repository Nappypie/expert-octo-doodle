# ArqelUi

A modern key system UI for Roblox executors. Supports custom validation, Junkie SDK integration, persistent key storage, and keyless mode.

![ArqelUi](https://raw.githubusercontent.com/Cobruhehe/Raft-Game/refs/heads/main/Screenshot_20260301-042539_Roblox.jpg)
![ArqelUi](https://raw.githubusercontent.com/Cobruhehe/Raft-Game/refs/heads/main/Screenshot_20260301-042522_Roblox.jpg)


## Installation

```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobruhehe/expert-octo-doodle/refs/heads/main/ArqelUi.luau"))()
```

## Links

- **Purchase**: [Orqan.shop](https://orqan.shop/product/ArqelUi/)
- **Discord**: [Orqan.lol/discord](https://Orqan.lol/discord)

## License

Free to use via GitHub loadstring. Modifications require source code purchase from [orqan.shop](https://orqan.shop/product/ArqelUi/).

---

## Quick Start

```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobruhehe/expert-octo-doodle/refs/heads/main/ArqelUi.luau"))()

Arqel.Appearance.Title = "My Script"
Arqel.Links.GetKey = "https://your-key-link.com"
Arqel.Links.Discord = "https://discord.gg/invite"

Arqel.Callbacks.OnVerify = function(key)
    return key == "YOUR_VALID_KEY"
end

Arqel.Callbacks.OnSuccess = function()
    loadstring(game:HttpGet("YOUR_SCRIPT_URL"))()
end

Arqel:Launch()
```

---

## Usage Examples

### Standard Key System

```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobruhehe/expert-octo-doodle/refs/heads/main/ArqelUi.luau"))()

Arqel.Appearance.Title = "Premium Script"
Arqel.Appearance.Subtitle = "Enter your license key"
Arqel.Links.GetKey = "https://pastebin.com/your-key"
Arqel.Links.Discord = "https://discord.gg/server"

Arqel.Callbacks.OnVerify = function(key)
    return key == "PREMIUM_2024"
end

Arqel.Callbacks.OnSuccess = function()
    loadstring(game:HttpGet("YOUR_SCRIPT_URL"))()
end

Arqel:Launch()
```

### Keyless Mode

```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobruhehe/expert-octo-doodle/refs/heads/main/ArqelUi.luau"))()

Arqel.Appearance.Title = "Free Script"
Arqel.Options.Keyless = true

Arqel.Callbacks.OnSuccess = function()
    loadstring(game:HttpGet("YOUR_SCRIPT_URL"))()
end

Arqel:Launch()
```

### Keyless Mode Without UI

```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobruhehe/expert-octo-doodle/refs/heads/main/ArqelUi.luau"))()

Arqel.Options.Keyless = true
Arqel.Options.KeylessUI = false

Arqel.Callbacks.OnSuccess = function()
    loadstring(game:HttpGet("YOUR_SCRIPT_URL"))()
end

Arqel:Launch()
```

### Junkie SDK Integration

```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobruhehe/expert-octo-doodle/refs/heads/main/ArqelUi.luau"))()

Arqel.Appearance.Title = "My Script"
Arqel.Links.Discord = "https://discord.gg/server"

Arqel:LaunchJunkie({
    Service = "YOUR_SERVICE_NAME",
    Identifier = "YOUR_IDENTIFIER",
    Provider = "YOUR_PROVIDER_NAME"
})

-- Keys are automatically validated through Junkie
```

### HTTP API Validation

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

### With Shop Integration

```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobruhehe/expert-octo-doodle/refs/heads/main/ArqelUi.luau"))()

Arqel.Appearance.Title = "Premium Script"
Arqel.Links.GetKey = "https://your-key-link.com"
Arqel.Links.Discord = "https://discord.gg/server"

-- Shop configuration
Arqel.Shop.Enabled = true
Arqel.Shop.Title = "Get Lifetime Key"
Arqel.Shop.Subtitle = "One-time payment • Instant delivery"
Arqel.Shop.ButtonText = "Purchase"
Arqel.Shop.Link = "https://yourshop.com/product/key"

Arqel.Callbacks.OnVerify = function(key)
    return key == "VALID_KEY"
end

Arqel.Callbacks.OnSuccess = function()
    loadstring(game:HttpGet("YOUR_SCRIPT_URL"))()
end

Arqel:Launch()
```

---

## Configuration Reference

### Appearance

```lua
Arqel.Appearance = {
    Title = "Arqel",
    Subtitle = "Enter your key to continue",
    Icon = "rbxassetid://95721401302279",
    IconSize = UDim2.new(0, 30, 0, 30)
}
```

### Links

```lua
Arqel.Links = {
    GetKey = "",
    Discord = ""
}
```

### Storage

```lua
Arqel.Storage = {
    FileName = "Arqel_Key",
    Remember = true,
    AutoLoad = true
}
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `FileName` | string | `"Arqel_Key"` | Base name for saved key file |
| `Remember` | boolean | `true` | Save valid keys locally |
| `AutoLoad` | boolean | `true` | Auto-validate saved keys on launch |

### Options

```lua
Arqel.Options = {
    Keyless = nil,
    KeylessUI = false,
    Blur = true,
    Draggable = true
}
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Keyless` | boolean/nil | `nil` | `nil` (auto-detect), `true` (force keyless), `false` (disable) |
| `KeylessUI` | boolean | `false` | Show keyless UI (`true`) or skip entirely (`false`) |
| `Blur` | boolean | `true` | Background blur effect |
| `Draggable` | boolean | `true` | Window dragging |

### Theme

```lua
Arqel.Theme = {
    Accent = Color3.fromRGB(139, 0, 0),
    AccentHover = Color3.fromRGB(170, 20, 20),
    Background = Color3.fromRGB(15, 15, 15),
    Header = Color3.fromRGB(20, 20, 20),
    Input = Color3.fromRGB(25, 25, 25),
    Text = Color3.fromRGB(255, 255, 255),
    TextDim = Color3.fromRGB(120, 120, 120),
    Success = Color3.fromRGB(50, 205, 110),
    Error = Color3.fromRGB(245, 70, 90),
    Warning = Color3.fromRGB(255, 180, 50),
    StatusIdle = Color3.fromRGB(180, 80, 80),
    Discord = Color3.fromRGB(88, 101, 242),
    DiscordHover = Color3.fromRGB(114, 137, 218),
    Divider = Color3.fromRGB(45, 45, 70),
    Pending = Color3.fromRGB(60, 60, 60)
}
```

### Shop

```lua
Arqel.Shop = {
    Enabled = false,
    Icon = "",
    Title = "Get Premium Access",
    Subtitle = "Instant delivery • 24/7 support",
    ButtonText = "Buy",
    Link = ""
}
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Enabled` | boolean | `false` | Enable shop section in UI |
| `Icon` | string | `""` | Custom icon (rbxassetid or URL). Empty uses main logo |
| `Title` | string | `"Get Premium Access"` | Shop section heading |
| `Subtitle` | string | `"Instant delivery • 24/7 support"` | Description text below title |
| `ButtonText` | string | `"Buy"` | Text on purchase button |
| `Link` | string | `""` | URL copied when buy button clicked |

**Note:** Shop section only appears when `Enabled = true`.

### Changelog

```lua
Arqel.Changelog = {
    {Version = "v1.2.0", Date = "Jan 20, 2025", Changes = {"Added new feature", "Fixed bug"}},
    {Version = "v1.1.0", Date = "Jan 15, 2025", Changes = {"Improved UI"}},
    {Version = "v1.0.0", Date = "Jan 10, 2025", Changes = {"Initial release"}}
}
```

Changelog button only appears when entries exist.

### Callbacks

```lua
Arqel.Callbacks = {
    OnVerify = nil,
    OnSuccess = nil,
    OnFail = nil,
    OnClose = nil
}
```

#### OnVerify

```lua
-- Simple
Arqel.Callbacks.OnVerify = function(key)
    return key == "VALID_KEY"
end

-- Detailed
Arqel.Callbacks.OnVerify = function(key)
    return {
        valid = false,
        error = "KEY_EXPIRED",
        message = "Your key has expired"
    }
end
```

#### OnSuccess

```lua
Arqel.Callbacks.OnSuccess = function()
    loadstring(game:HttpGet("YOUR_SCRIPT_URL"))()
end
```

#### OnFail

```lua
Arqel.Callbacks.OnFail = function(errorMsg)
    print("Failed:", errorMsg)
end
```

#### OnClose

```lua
Arqel.Callbacks.OnClose = function()
    print("User closed the key system")
end
```

---

## Error Codes

| Code | Message |
|------|---------|
| `KEY_INVALID` | Key not found in system |
| `KEY_EXPIRED` | License has expired |
| `HWID_BANNED` | Hardware banned (auto-kicks) |
| `KEY_INVALIDATED` | License was revoked |
| `ALREADY_USED` | One-time key already used |
| `HWID_MISMATCH` | HWID limit reached |
| `SERVICE_NOT_FOUND` | Service not found |
| `SERVICE_MISMATCH` | Wrong service |
| `PREMIUM_REQUIRED` | Premium required |
| `ERROR` | Network error |

---

## Notification System

```lua
Arqel:Notify(title, message, duration, iconType)
```

**Icon Types:** `"info"`, `"success"`, `"error"`, `"warning"`, `"shield"`, `"key"`, `"copy"`, `"discord"`, `"close"`

```lua
Arqel:Notify("Success", "Key validated!", 2, "success")
Arqel:Notify("Error", "Invalid key", 4, "error")
```

---

## Utility Functions

```lua
local savedKey = Arqel:GetSavedKey()  -- Returns saved key or nil
Arqel:ClearSavedKey()                  -- Deletes saved key
```

---

## Global Variables

| Variable | Description |
|----------|-------------|
| `getgenv().SCRIPT_KEY` | Validated key, `"KEYLESS"`, `"_CLOSED_"`, or `nil` |
| `getgenv().ArqelLoaded` | `true` if UI is active |
| `getgenv().ArqelClosed` | `true` if user closed without validating |
| `getgenv().Arqel` | Reference to Arqel module |

---

## Methods

### Launch()

Standard key system with custom validation.

```lua
Arqel:Launch()
```

### LaunchJunkie(config)

Junkie SDK integration with automatic validation.

```lua
Arqel:LaunchJunkie({
    Service = "YOUR_SERVICE_NAME",
    Identifier = "YOUR_IDENTIFIER", 
    Provider = "YOUR_PROVIDER"
})
```

---

## Advanced Examples

### Multiple Key Tiers

```lua
local KEYS = {
    ["BASIC_KEY"] = "basic",
    ["PREMIUM_KEY"] = "premium"
}

Arqel.Callbacks.OnVerify = function(key)
    local tier = KEYS[key]
    if tier then
        getgenv().USER_TIER = tier
        return true
    end
    return false
end

Arqel.Callbacks.OnSuccess = function()
    if getgenv().USER_TIER == "premium" then
        loadstring(game:HttpGet("PREMIUM_URL"))()
    else
        loadstring(game:HttpGet("BASIC_URL"))()
    end
end
```

### Webhook Logging

```lua
local HttpService = game:GetService("HttpService")

Arqel.Callbacks.OnSuccess = function()
    pcall(function()
        HttpService:PostAsync("WEBHOOK_URL", HttpService:JSONEncode({
            embeds = {{
                title = "Login",
                description = "User: " .. game.Players.LocalPlayer.Name,
                color = 3066993
            }}
        }))
    end)
end
```

### Custom Theme

```lua
-- Purple theme
Arqel.Theme.Accent = Color3.fromRGB(138, 43, 226)
Arqel.Theme.AccentHover = Color3.fromRGB(159, 95, 226)
Arqel.Theme.Background = Color3.fromRGB(10, 10, 15)
```

### Shop with Custom Icon

```lua
Arqel.Shop.Enabled = true
Arqel.Shop.Icon = "rbxassetid://123456789"
Arqel.Shop.Title = "Premium Upgrade"
Arqel.Shop.Subtitle = "Unlock all features"
Arqel.Shop.ButtonText = "Get Now"
Arqel.Shop.Link = "https://shop.example.com"
```

### Minimal Shop Setup

```lua
Arqel.Shop.Enabled = true
Arqel.Shop.Link = "https://yourshop.com/buy"
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Key not saving | Check executor supports `writefile`, ensure `Storage.Remember = true` |
| Validation not working | Define `Callbacks.OnVerify` before `Launch()` |
| UI not appearing | Check `getgenv().ArqelLoaded` isn't already `true` |
| Junkie failing | Verify Service, Identifier, Provider values |
| Shop not showing | Set `Shop.Enabled = true` |
