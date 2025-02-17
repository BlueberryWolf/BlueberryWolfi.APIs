# Comfy APIs for WEBFISHING Mod Development

A collection of APIs designed for use in the Comfy WEBFISHING mod within GDWeave, providing streamlined management for player interactions, keybinds, and more.

## Installation (for the peeps)
1. Ensure [GDWeave](https://github.com/NotNite/GDWeave) is installed and working properly
2. [Download the latest release](https://github.com/BlueberryWolf/APIs/releases/latest/download/BlueberryWolfi.APIs.zip)
3. Extract the zip to `BlueberryWolfi.APIs` and be careful to **not rename it**.
4. Place the folder in `WEBFISHING/GDWeave/Mods/BlueberryWolfi.APIs`
5. Install any other mods which use my API, like 

## Developer Usage (for the nerds)

1. Import the downloaded source code folder into your godot project directory as `mods/BlueberryWolfi.APIs`.
2. Add an autoload entry in your project settings **named** `BlueberryWolfiAPIs` (no dot) to ensure that the APIs load before your mods.
3. **Important:** When exporting your project, be careful not to include the source code of these APIs.

### Code Integration

**PlayerAPI Example:**
Importing PlayerAPI
```gdscript
var PlayerAPI

func _ready():
	PlayerAPI = get_node_or_null("/root/BlueberryWolfiAPIs/PlayerAPI")
	PlayerAPI.connect("_player_added", self, "init_player")

func init_player(player: Actor):
	# example:
	# print(PlayerAPI.get_player_name(player))
```

**KeybindsAPI Example:**
Importing KeybindsAPI
```gdscript
var _keybinds_api

func _ready():
	_keybinds_api = get_node_or_null("/root/BlueberryWolfiAPIs/KeybindsAPI")

	var pushtalk_mic_signal = KeybindAPI.register_keybind({
	  "action_name": "toggle_ptt",
	  "title": "Toggle Push to Talk",
	  "key": KEY_T,
	})
	
	_keybinds_api.connect(pushtalk_mic_signal, self, "_on_to_talk_button_down")       # key down signal is automatically created.
	_keybinds_api.connect(pushtalk_mic_signal + "_up", self, "_on_to_talk_button_up") # key up signal is automatically created.

func _on_to_talk_button_down() -> void:
	print("Push to talk on")

func _on_to_talk_button_up() -> void:
	print("Push to talk off")
```

---

## PlayerAPI

The PlayerAPI simplifies player-related tasks such as managing actors, steam IDs, and player states.

### Variables

```gdscript
PlayerAPI.local_player  # Local player as an actor
PlayerAPI.in_game       # Checks if the player is in the game
PlayerAPI.players       # List of player actors in the game
```

### Signals

```gdscript
_player_added(player)   # Triggered when a player joins, returns the player actor
_player_removed(player) # Triggered when a player leaves, returns the player actor
_ingame()               # Triggered when the local player is in-game
```

### Functions

```gdscript
is_player(node: Node) -> bool                      # Checks if a node is a Player
get_player_from_steamid(steamid: String) -> Actor  # Gets player by Steam ID
get_player_name(player: Actor) -> String           # Returns player's name
get_player_title(player: Actor) -> String          # Returns player's title
get_player_steamid(player: Actor) -> String        # Returns player's Steam ID
```

---

## KeybindsAPI

The KeybindsAPI allows mods to register custom keybinds and trigger signals for input events. Configurable in the in-game controls menu.

### Signals

```gdscript
_keybind_changed(keybind: String, title: String, input_event: InputEventKey)
```

### Functions

```gdscript
register_keybind(keybind_data: Dictionary) -> String  # Registers a keybind, returns the signal name
```
*Note: A signal with "_up" appended to the signal name is automatically created for key release.*

---

## Requirements
* Ensure [GDWeave](https://github.com/NotNite/GDWeave) is properly installed to use these APIs.
