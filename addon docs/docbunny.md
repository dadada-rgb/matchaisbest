# **Helpbook - matcha latte [ author: van huy ]**


# **Target System**

## **Get Target**

Retrieve the currently selected target

```lua
local target = matchaapi:get_target()

-- functions return a Player instance or nil
```

### **Example**

```lua
local target = matchaapi:get_target()

if target then
    print(target.Name)
else
    warn("No target found")
end
```

---

## **Set Target**

```lua
matchaapi:set_target(Player)
```

### **Example**

```lua
local Players = Service("Players")
local playerName = "roblox"
local targetPlayer = matchaapi:Get_Player(playerName)

matchaapi:set_target(targetPlayer)

local current = matchaapi:get_target()
print(current.Name)
```

### **Parameters**

* **`player` (Instance: Player)** – The player to set as target.

---

# **Checks**

## **Returns Instance or nil**

Functions that return a **character component** if found:

```lua
-- Target is a Player instance

-- Get Humanoid
matchaapi:Humanoid(Player)

-- Get UpperTorso
matchaapi:UpperTorso(Player)

-- Get HumanoidRootPart
matchaapi:HumanoidRootPart(Player)

-- Get ForceField (Instance)
matchaapi:ForceField(Player)
```

---

# **Other**

---

# Item System

## Check Owned Item

Checks whether the local player currently owns a tool whose name contains the specified text.
Searches both **Character** and **Backpack**.

```lua
local has = matchaapi:has_item("shotgun")

if has then
    print("Player has shotgun")
else
    print("Player does not have shotgun")
end
```

### Returns:

* `true` – item found
* `false` – not found

---

# Connection System

## Add Connection

Registers and stores an RBXScriptConnection.

```lua
local conn = RunService.Heartbeat:Connect(function()
    print("Heartbeat running")
end)

matchaapi:AddConnection("HeartbeatLoop", conn)
```

* If the name already exists, the old connection is disconnected and replaced.

---

## Connected

Checks if a named connection exists and is still active.

```lua
local status = matchaapi:Connected("HeartbeatLoop")
print("Status:", status)
```

Returns:
`true` or `false`.

---

## Disconnect

Disconnects a stored connection by name.

```lua
matchaapi:Disconnect("HeartbeatLoop", true)
```

Parameters:

* `Name`: string or table of names
* `Nil = true`: removes entry after disconnecting

---

## Unload

Disconnects all registered connections and clears the table.

```lua
matchaapi:Unload()
print("All connections cleared.")
```

---

# Player Search

## Get Player (Prefix Search)

Finds a player whose Name or DisplayName starts with the provided text.

```lua
local target = matchaapi:Get_Player("rob")

if target then
    print("Found:", target.Name)
else
    warn("No matching player")
end
```

Returns: `Player` or `nil`.

---

## Get Mouse Player

Returns the closest player to the mouse cursor (on-screen).

```lua
local target = matchaapi:Get_Mouse_Player()

if target then
    print("Mouse pointing at:", target.Name)
end
```

Returns: `Player` or `nil`.

---

# Instance Creator

## Create Instance

Creates an instance and applies provided properties.
If another instance with the same name exists under the same parent, it is removed.

```lua
local part = matchaapi:Create("Part", {
    Name = "MyPart",
    Parent = workspace,
    Size = Vector3.new(3, 3, 3),
    Color = Color3.new(1, 0, 0)
})

print("Created instance:", part)
```

Returns: the created instance.

---

# Asset Loading

## GetAsset

Loads multiple assets using `game:GetObjects`, clones them, and returns them in a table.

```lua
local assets = matchaapi:GetAsset({
    Gun = 123456,
    Knife = 789012
})

print(assets.Gun, assets.Knife)
```

Returns:
`{ Name = ClonedInstance }`

---

# Animation System

## Play Animation

```lua
matchaapi:AnimPlay(10488912345, 1, 0, true)
```

Parameters:

* `ID`: animation ID
* `Speed`: playback speed
* `Time`: fade in time
* `Smoothing`: boolean smoothing

Stops any animation with the same ID before playing.
Priority is set to 4.

---

## Stop Animation

```lua
matchaapi:AnimStop(10488912345)
```

Stops animation with the given ID.

---

## Check Playing

```lua
if matchaapi:IsAnimPlaying(10488912345) then
    print("Animation active")
end
```

Returns: boolean.

---

# Sound System

## Play Sound

```lua
local sound = matchaapi:PlaySound(123456, 1)
```

Plays a sound and destroys it when finished.

Returns: Sound instance.

---

## Stop Sound

```lua
matchaapi:StopSound(sound)
```

Stops and destroys the given sound.

---

# Character Utilities

## Noclip

Disables collision for every BasePart in the character.

```lua
matchaapi:Noclip(LocalPlayer.Character)
```

---

## Zero Velocity

Removes all velocity from the character.

```lua
matchaapi:ZeroVelocity(LocalPlayer.Character)
```

---

## Change State

Changes the humanoid state of the local player.

```lua
matchaapi:ChangeState(Enum.HumanoidStateType.FallingDown)
```

---

## Equip Tool

Moves a tool from Backpack to Character.

```lua
matchaapi:Equip("Knife")
```

---

## Remove Accessories

Deletes all accessories of a specific class.

```lua
matchaapi:RemoveAccessory(LocalPlayer.Character, "Accessory")
```

---

# Misc Utilities

## Tween

Simple TweenService wrapper.

```lua
matchaapi:Tween(workspace.Part, 1, {
    Position = Vector3.new(0, 10, 0)
})
```

---

## Chat

Sends a message to the TextChatService global channel.

```lua
matchaapi:Chat("Hello world")
```

---

## HttpGet Loader

Loads and executes remote Lua code.

```lua
matchaapi:HttpGet("https://example.com/script.lua")
```

Returns: function result or nil.

---

## Webhook Sender

Sends JSON data to a webhook using supported exploit HTTP functions.

```lua
matchaapi:SendWebhook("WEBHOOK_URL_HERE", {
    content = "Hello Webhook",
    username = "matchaapi"
})
```

--- 

## UI / Notifications

### Notify

```lua
--// matchaapi:notify(title, text, duration, iconid, iconColor)

matchaapi:notify("Matcha Latte", "welcome back to matcha", 5, "111495166232015", Color3.fromRGB(52, 255, 164))
```

Parameters:

* `title` (string)
* `text` (string)
* `duration` (number, optional)
* `iconid` (number)
* `iconColor` (rgb color)
---

## UI

### Add New Page

```lua
--// matchaapi:AddPage(name, icon, columns, subpages)

local NewPage = matchaapi:AddPage("combat", "111495166232015", 1, true)

```
Parameters:

* `name` (string)
* `icon` (number)
* `columns` (number, 2 or 1)
* `subpages` (true or false)
---

## Example

```lua
repeat wait() until matchaapi

local NewPage = matchaapi:AddPage("combat", "111495166232015", 1, true)
local VisualsPage = matchaapi:AddPage("Visual", "115907015044719", 2, false)

Aimbot = NewPage:SubPage({
	Name = "aimbot", 
	Icon = "111386589037485", 
	Columns = 2
})
local GeneralSection = Aimbot:Section({Name = "general2", Icon = "103174889897193", Side = 1})

local Toggle = GeneralSection:Toggle({
	Name = "enabled",
	Flag = "enabled",
	Default = false,
	Callback = function(Value)
		print(Value)
	end
})
--[[GeneralSection:Label("Exotic Dealer", "Left"):Colorpicker({
	Name = "Colorpicker",
	Flag = "Colorpicker23",
	Default = Color3.fromRGB(0, 0, 0),
	Alpha = 0,
	Callback = function(Color, Alpha)
		print(Color, Alpha)
	end
})]]
local Button = GeneralSection:Button({
	Name = "Button", 
	Callback = function()
		print("Pressed")
	end
})
local Slider = GeneralSection:Slider({
	Name = "Slider", 
	Flag = "Slider", 
	Min = 0, 
	Default = 0, 
	Max = 100, 
	Suffix = "%", 
	Decimals = 1, 
	Callback = function(Value)
		print(Value)
	end
})
GeneralSection:Label("hi bro", "Left")

local Dropdown = GeneralSection:Dropdown({
	Name = "Dropdown",
	Flag = "Dropdown",
	Items = { },
	Default = "One",
	MaxSize = 155,
	Multi = false,
	Callback = function(Value)
		print(Value)
	end
})
Dropdown:AddOption("Black")
Dropdown:AddOption("Black")
Dropdown:AddOption("Black")

local MultiDropdown = GeneralSection:Dropdown({
	Name = "Multi Dropdown",
	Flag = "MultiDropdown",
	Items = {"One", "Two", "Three"},
	Default = {"One", "Two"},
	MaxSize = 85,
	Multi = true,
	Callback = function(Value)
		print(Value)
	end
})

local Label = GeneralSection:Label("Keybind", "Left")
Label:Keybind({
	Name = "Keybind",
	Flag = "Keybind",
	Default = Enum.KeyCode.L,
	Mode = "toggle",
	Callback = function(Value)
		print(Value)
	end
})

local Toggle2 = GeneralSection:Toggle({
	Name = "enabled2",
	Flag = "enabled2",
	Default = false,
	Callback = function(Value)
		print(Value)
	end
}):Keybind({
	Name = "Keybind2",
	Flag = "Keybind2",
	Default = Enum.KeyCode.RightAlt,
	Mode = "toggle",
	Callback = function(Value)
		print(Value)
	end
})

local Textbox = GeneralSection:Textbox({
	Name = "Textbox",
	Flag = "Textbox",
	Placeholder = "Placeholder",
	Default = "Text",
	Callback = function(Value)
		print(Value)
	end
})
local Esp = VisualsPage:Section({Name = "esp", Icon = "103174889897193", Side = 1})
local World = VisualsPage:Section({Name = "world", Icon = "135799335731002", Side = 2})

```
