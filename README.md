# ArqelUi

A modern, feature-complete key system UI for Roblox executors with Junkie integration, persistent storage, and keyless mode.

![ArqelUi Preview](https://images.haunt.gg/WRr32V6V.png)

## Installation
```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobru1/expert-octo-doodle/refs/heads/main/ArqelUi.luau", true))()
```

## Quick Start
```lua
local Arqel = loadstring(game:HttpGet("https://raw.githubusercontent.com/Cobru1/expert-octo-doodle/refs/heads/main/ArqelUi.luau", true))()

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

## 📚 Documentation

**Full documentation:** [cobru.gitbook.io/arqelui](https://cobru.gitbook.io/arqelui/)

## Links

- **Purchase Source:** [orqan.shop/product/ArqelUi](https://orqan.shop/product/ArqelUi/)
- **Discord:** [Orqan.lol/discord](https://Orqan.lol/discord)

## License

Free to use via loadstring. Modifications require source purchase from [orqan.shop](https://orqan.shop/product/ArqelUi/).

## Credits

Inspired by [Sentinel Key System](https://github.com/AsuraXowner/Sentinel-Open-Source) by [gamerfoxy](https://github.com/AsuraXowner)
