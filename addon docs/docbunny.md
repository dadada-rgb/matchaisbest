# **Helpbook bunny - matcha [ author: van huy ]**


# **Target System**

## **Get Target**

Retrieve the currently selected target for **main-mode targeting** or **ragebot targeting**.

```lua
-- Get main target
local targetMain = bunnyapi:get_target_main()

-- Get rage target
local targetRage = bunnyapi:get_target_rage()

-- Both functions return a Player instance or nil
```

### **Example**

```lua
local target = bunnyapi:get_target_main() -- or bunnyapi:get_target_rage()

if target then
    print(target.Name)
else
    warn("No target found")
end
```

---

## **Set Target**

```lua
-- Set main target
bunnyapi:set_target_main(Player)

-- Set rage target
bunnyapi:set_target_rage(Player)
```

### **Example**

```lua
local Players = Service("Players")
local playerName = "roblox"
local targetPlayer = bunnyapi:Get_Player(playerName)

bunnyapi:set_target_main(targetPlayer)

local current = bunnyapi:get_target_main()
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
bunnyapi:Humanoid(Player)

-- Get UpperTorso
bunnyapi:UpperTorso(Player)

-- Get HumanoidRootPart
bunnyapi:HumanoidRootPart(Player)

-- Get ForceField (Instance)
bunnyapi:ForceField(Player)
```

---

## **Returns boolean / BoolValue / nil**

Status-related checks:

```lua
-- Knocked status (BoolValue or nil)
bunnyapi:Is_KO(Player)

-- Dead status (BoolValue or nil)
bunnyapi:Is_Dead(Player)

-- Attacking status (BoolValue or nil)
bunnyapi:Is_Attacking(Player)

-- LocalPlayer reloading (boolean)
bunnyapi:Is_Reloading()

-- Same crew (boolean)
bunnyapi:Is_Crew(Player, Target)
```

---

### **Example:**

```lua
local target = bunnyapi:get_target_main()

if target then
    
    if bunnyapi:Is_KO(target) then
        return print("Target is knocked")
    end

    if bunnyapi:Is_Crew(LocalPlayer, target) then
        return warn("Target is in my crew")
    end

    if bunnyapi:Is_Reloading() then
        return print("Reloading ammo...")
    end

end
```

---

# **Other**

---

# Shoot System

```lua
bunnyapi:ShootGun(tool, targetPart, finalPos, direction)
```
## Example:

```lua
local char = LocalPlayer.Character
local tool = char and char:FindFirstChild("[LMG]")  -- Example

local targetPart = lockedTarget.Character.UpperTorso
local finalPos = predictedPosition
local dir = (finalPos - tool.Handle.Position).Unit

bunnyapi:ShootGun(tool, targetPart, finalPos, dir)
```
# Item System

## Check Owned Item

Checks whether the local player currently owns a tool whose name contains the specified text.
Searches both **Character** and **Backpack**.

```lua
local has = bunnyapi:has_item("shotgun")

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

# Shop System

## Find Shop Item

```lua
local shop = bunnyapi:Find_Item("lmg", false)
if shop then
    print("Found shop:", shop.Name)
else
    warn("Shop not found")
end
```

### Parameters:

* `Name` (string) – item name to find
* `Type` (boolean or nil)

  * `true` = find ammo
  * `false` = find gun
  * `nil` = treated as `false`

### Returns:

* Shop instance, or `nil`

---

## Buy Item

quick buy item (any item like weapon,misc item)

```lua
--//buy gun
bunnyapi:buy_item("lmg", false) -- false or "gun"

--//buy ammo
bunnyapi:buy_item("lmg", true) -- true or "ammo"
```

---

# Tool Cache

Fetch information about the tool currently equipped by the local player.

```lua
local data = bunnyapi:Cache_Tool()

if data then
    print("Equipped:", data.Instance.Name)
    print("Ammo:", data.Ammo, "/", data.MaxAmmo)
else
    warn("No tool equipped")
end
```

### Returns (table or nil)

| Field     | Type     | Description                   |
| --------- | -------- | ----------------------------- |
| Instance  | Tool     | The equipped tool             |
| Handle    | BasePart | The tool’s handle             |
| Offset    | Vector3  | Offset calculated from handle |
| Ammo      | number   | Current ammo count            |
| MaxAmmo   | number   | Maximum possible ammo         |
| Gun       | boolean  | True if tool is a gun         |
| Shotgun   | boolean  | True if tool is a shotgun     |
| Automatic | boolean  | True if gun is automatic      |
| Client    | boolean  | True if tool is not a weapon  |

---

# Connection System

## Add Connection

Registers and stores an RBXScriptConnection.

```lua
local conn = RunService.Heartbeat:Connect(function()
    print("Heartbeat running")
end)

bunnyapi:AddConnection("HeartbeatLoop", conn)
```

* If the name already exists, the old connection is disconnected and replaced.

---

## Connected

Checks if a named connection exists and is still active.

```lua
local status = bunnyapi:Connected("HeartbeatLoop")
print("Status:", status)
```

Returns:
`true` or `false`.

---

## Disconnect

Disconnects a stored connection by name.

```lua
bunnyapi:Disconnect("HeartbeatLoop", true)
```

Parameters:

* `Name`: string or table of names
* `Nil = true`: removes entry after disconnecting

---

## Unload

Disconnects all registered connections and clears the table.

```lua
bunnyapi:Unload()
print("All connections cleared.")
```

---

# Player Search

## Get Player (Prefix Search)

Finds a player whose Name or DisplayName starts with the provided text.

```lua
local target = bunnyapi:Get_Player("rob")

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
local target = bunnyapi:Get_Mouse_Player()

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
local part = bunnyapi:Create("Part", {
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
local assets = bunnyapi:GetAsset({
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
bunnyapi:AnimPlay(10488912345, 1, 0, true)
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
bunnyapi:AnimStop(10488912345)
```

Stops animation with the given ID.

---

## Check Playing

```lua
if bunnyapi:IsAnimPlaying(10488912345) then
    print("Animation active")
end
```

Returns: boolean.

---

# Sound System

## Play Sound

```lua
local sound = bunnyapi:PlaySound(123456, 1)
```

Plays a sound and destroys it when finished.

Returns: Sound instance.

---

## Stop Sound

```lua
bunnyapi:StopSound(sound)
```

Stops and destroys the given sound.

---

# Character Utilities

## Noclip

Disables collision for every BasePart in the character.

```lua
bunnyapi:Noclip(LocalPlayer.Character)
```

---

## Zero Velocity

Removes all velocity from the character.

```lua
bunnyapi:ZeroVelocity(LocalPlayer.Character)
```

---

## Change State

Changes the humanoid state of the local player.

```lua
bunnyapi:ChangeState(Enum.HumanoidStateType.FallingDown)
```

---

## Equip Tool

Moves a tool from Backpack to Character.

```lua
bunnyapi:Equip("Knife")
```

---

## Remove Accessories

Deletes all accessories of a specific class.

```lua
bunnyapi:RemoveAccessory(LocalPlayer.Character, "Accessory")
```

---

# Misc Utilities

## Tween

Simple TweenService wrapper.

```lua
bunnyapi:Tween(workspace.Part, 1, {
    Position = Vector3.new(0, 10, 0)
})
```

---

## Chat

Sends a message to the TextChatService global channel.

```lua
bunnyapi:Chat("Hello world")
```

---

## HttpGet Loader

Loads and executes remote Lua code.

```lua
bunnyapi:HttpGet("https://example.com/script.lua")
```

Returns: function result or nil.

---

## Webhook Sender

Sends JSON data to a webhook using supported exploit HTTP functions.

```lua
bunnyapi:SendWebhook("WEBHOOK_URL_HERE", {
    content = "Hello Webhook",
    username = "bunnyapi"
})
```

--- 

## UI / Notifications

### Notify

```lua
bunnyapi:notify("hi", 1)
```

Parameters:

* `text` (string)
* `duration` (number, optional)

---

## UI

### Add New Tab

```lua
local tab = bunnyapi:AddTab("vanhuy no1")
```

## Example

```lua
repeat wait() until bunnyapi

local tab = bunnyapi:AddTab("Title")
local Plugin = tab:AddLeftGroupbox("Example")

Plugin:AddToggle("Toggle 1", {
    Text = "Toggle 1",
    Default = false,
    Callback = function(v)
        print("Toggle:", v)
    end
})
Plugin:AddSlider("MySlider", {
	Text = "This is my slider!",
	Default = 0,
	Min = 0,
	Max = 5,
	Rounding = 1,
	Compact = false,
	Callback = function(Value)
		print("[cb] MySlider was changed! New value:", Value)
	end,
	Tooltip = "I am a slider!", -- Information shown when you hover over the slider
})
--double slider
Plugin:AddSlider("MySlider1", {
	Text = "1",
	Default = 0,
	Min = 0,
	Max = 5,
	Rounding = 1,
	Compact = false,
}):AddSlider("MySlider2", {
	Text = "2",
	Default = 0,
	Min = 0,
	Max = 5,
	Rounding = 1,
	Compact = false,
})
Plugin:AddButton("Print Target", function()
    print(bunnyapi:bunnyapi:get_target_main())
end)

Plugin:AddButton("Check Reload", function()
    print("Reloading:", bunnyapi:Is_Reloading())
end)

Plugin:AddButton("Notify", function()
    bunnyapi:notify("hi", 5)
end)
--//other thing like dropdown etc based on linoria example
--https://raw.githubusercontent.com/violin-suzutsuki/LinoriaLib/refs/heads/main/Example.lua
```
