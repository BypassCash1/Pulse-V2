# Pulse UI V1 - Documentation

Complete documentation for Pulse UI V1, a lightweight Roblox UI library.

## Installation

### Method 1: Loadstring (Recommended)
```lua
local PulseUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/yourusername/pulse-ui/main/ui.lua"))()
```

### Method 2: Local Require
```lua
local PulseUI = require(script.Parent.ui)
```

---

## Creating a Window

### Basic Window
```lua
local Window = PulseUI:CreateWindow({
    Title = "My Script",
    FooterText = "Made by YourName"
})
```

### Window with Home Tab
```lua
local Window = PulseUI:CreateWindow({
    Title = "My Script",
    FooterText = "v1.0.0",
    
    HomeTab = {
        SupportedExecutors = {"Synapse X", "Script-Ware", "Solara"},
        UnsupportedExecutors = {"Krnl", "JJSploit"},
        DiscordInvite = "yourinvite",
        Backdrop = 0, -- 0 = game thumbnail, or asset ID
        IconStyle = 1, -- 1 = solid, 2 = outline
        
        Changelog = {
            {
                Title = "Version 1.0",
                Date = "January 20, 2026",
                Description = "Initial release\n- Feature 1\n- Feature 2",
            }
        }
    }
})
```

---

## Window Methods

### Set Discord Handler
```lua
Window:SetCopyDiscordHandler(function()
    if setclipboard then
        setclipboard("discord.gg/yourinvite")
    end
end)
```

### Set Theme
```lua
-- Available themes: Default, Starlight, Ocean, Violet, Emerald, Mono
Window:SetTheme("Starlight")
```

### Notifications
```lua
Window:Notify({
    Title = "Success",
    Content = "Action completed!",
    Duration = 3,
    Icon = 6031097225 -- Optional icon ID
})
```

### Toggle Key
```lua
Window:SetToggleKey(Enum.KeyCode.RightShift)
```

### Fullscreen
```lua
Window:SetFullscreen(true)
```

### Destroy Window
```lua
Window:Destroy()
```

---

## Creating Tabs

```lua
local MainTab = Window:CreateTab("Main")
local SettingsTab = Window:CreateTab("Settings")
```

---

## Creating Groups

Groups organize controls within tabs. Use "left" or "right" for column placement.

```lua
local PlayerGroup = MainTab:CreateGroup("Player", "left")
local ESPGroup = MainTab:CreateGroup("ESP", "right")
```

---

## Controls

### Toggle
```lua
PlayerGroup:AddToggle("God Mode", false, function(state)
    print("God Mode:", state)
end, "godmode") -- Flag for config system
```

### Toggle with Color Picker
```lua
ESPGroup:AddToggleColor("Player ESP", false, Color3.fromRGB(255, 0, 0), function(state, color)
    print("ESP:", state, "Color:", color)
end, "esp.enabled", "esp.color")
```

### Checkbox
```lua
PlayerGroup:AddCheckbox("Show FPS", false, function(state)
    print("Show FPS:", state)
end, "showfps")
```

### Slider
```lua
PlayerGroup:AddSlider("Walk Speed", 16, 200, 16, function(value)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
end, "walkspeed")
```

### Dropdown
```lua
local locations = {"Spawn", "Shop", "Bank"}
TeleportGroup:AddDropdown("Location", locations, "Spawn", function(value)
    print("Selected:", value)
end, "location")
```

### Button
```lua
PlayerGroup:AddButton("Reset Character", function()
    game.Players.LocalPlayer.Character.Humanoid.Health = 0
end)
```

### TextBox
```lua
ConfigGroup:AddTextBox("Config Name", "default", function(text)
    print("Config name:", text)
end, "configname")
```

### Keybind
```lua
ControlsGroup:AddKeybind("Toggle UI", Enum.KeyCode.RightShift, function(key)
    print("Key set to:", key.Name)
end, "togglekey")
```

### Color Picker
```lua
VisualsGroup:AddColorPicker("Ambient Color", Color3.fromRGB(255, 255, 255), function(color)
    game.Lighting.Ambient = color
end, "ambientcolor")
```

### Label
```lua
InfoGroup:AddLabel("Script loaded successfully!")
```

---

## Config System

### Flags
Flags allow you to save/load values automatically.

```lua
-- Get a flag value
local walkSpeed = Window:GetFlag("walkspeed") or 16

-- Set a flag value
Window:SetFlag("walkspeed", 50)
```

### Save Config
```lua
Window:SaveConfig("myconfig")
```

### Load Config
```lua
Window:LoadConfig("myconfig")
```

### List Configs
```lua
local configs = Window:ListConfigs()
for _, name in ipairs(configs) do
    print(name)
end
```

### Delete Config
```lua
Window:DeleteConfig("myconfig")
```

---

## Complete Example

```lua
-- Load library
local PulseUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/yourusername/pulse-ui/main/ui.lua"))()

-- Create window
local Window = PulseUI:CreateWindow({
    Title = "My Script",
    FooterText = "v1.0",
    HomeTab = {
        SupportedExecutors = {"Synapse X", "Solara"},
        DiscordInvite = "yourinvite",
        Changelog = {
            {Title = "v1.0", Date = "Jan 2026", Description = "Initial release"}
        }
    }
})

-- Set Discord handler
Window:SetCopyDiscordHandler(function()
    setclipboard("discord.gg/yourinvite")
end)

-- Create tabs
local Main = Window:CreateTab("Main")
local Visuals = Window:CreateTab("Visuals")

-- Create groups
local Player = Main:CreateGroup("Player", "left")
local Teleport = Main:CreateGroup("Teleport", "right")
local ESP = Visuals:CreateGroup("ESP", "left")

-- Add controls
Player:AddToggle("God Mode", false, function(state)
    -- Your god mode code
end, "godmode")

Player:AddSlider("Walk Speed", 16, 200, 16, function(value)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
end, "walkspeed")

Player:AddButton("Reset", function()
    game.Players.LocalPlayer.Character.Humanoid.Health = 0
end)

local locations = {"Spawn", "Shop", "Bank"}
Teleport:AddDropdown("Location", locations, "Spawn", function(value)
    print("Teleport to:", value)
end, "location")

Teleport:AddButton("Teleport", function()
    local loc = Window:GetFlag("location") or "Spawn"
    print("Teleporting to", loc)
end)

ESP:AddToggleColor("Player ESP", false, Color3.fromRGB(255, 0, 0), function(state, color)
    -- Your ESP code
end, "esp.enabled", "esp.color")

-- Notifications
Window:Notify({
    Title = "Loaded",
    Content = "Script loaded successfully!",
    Duration = 3
})

-- Auto-load config
task.spawn(function()
    task.wait(1)
    local configs = Window:ListConfigs()
    if #configs > 0 then
        Window:LoadConfig(configs[1])
    end
end)
```

---

## All Available Methods

### Window Methods
- `Window:CreateTab(name)` - Create a new tab
- `Window:SetTheme(name)` - Change theme
- `Window:Notify(options)` - Show notification
- `Window:SetToggleKey(keycode)` - Set UI toggle key
- `Window:SetFullscreen(bool)` - Toggle fullscreen
- `Window:SetFooterText(text)` - Change footer text
- `Window:SetCopyDiscordHandler(func)` - Set Discord copy callback
- `Window:SaveConfig(name)` - Save current config
- `Window:LoadConfig(name)` - Load config
- `Window:DeleteConfig(name)` - Delete config
- `Window:ListConfigs()` - List all configs
- `Window:GetFlag(flag)` - Get flag value
- `Window:SetFlag(flag, value)` - Set flag value
- `Window:Destroy()` - Destroy window

### Tab Methods
- `Tab:CreateGroup(name, side)` - Create a group ("left" or "right")

### Group Methods
- `Group:AddToggle(label, default, callback, flag)`
- `Group:AddToggleColor(label, defaultState, defaultColor, callback, flagToggle, flagColor)`
- `Group:AddCheckbox(label, default, callback, flag)`
- `Group:AddSlider(label, min, max, default, callback, flag)`
- `Group:AddDropdown(label, options, default, callback, flag)`
- `Group:AddButton(text, callback)`
- `Group:AddTextBox(label, defaultText, callback, flag)`
- `Group:AddKeybind(label, defaultKeyCode, callback, flag)`
- `Group:AddColorPicker(label, defaultColor, callback, flag)`
- `Group:AddLabel(text)`

---

## Themes

Available themes:
- `Default` - Purple accent
- `Starlight` - Purple with softer colors
- `Ocean` - Blue accent
- `Violet` - Purple/pink accent
- `Emerald` - Green accent
- `Mono` - Grayscale

```lua
Window:SetTheme("Ocean")
```

---

## Tips

1. **Use Flags**: Always provide flags for controls you want to save in configs
2. **Config Names**: Use descriptive names for configs
3. **Notifications**: Keep notification text short and clear
4. **Groups**: Organize related controls in the same group
5. **Callbacks**: Callbacks are called when values change

---

## Support

For issues or questions, join the Discord: discord.gg/yourinvite
