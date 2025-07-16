# README - statsManager Module for Roblox Studio (Luau)

## Overview

The `statsManager` module, created by **flashy3467dieux**, is a lightweight and efficient player state management system for Roblox Studio, written in Luau. It allows developers to assign, retrieve, and clear individual player states based on their `UserId`. The module integrates **SignalPlus** for real-time event handling, enabling reactive gameplay features when a player's state changes.

Source: SignalPlus on Roblox DevForum

## Features

- **State Management**: Store and retrieve player states (e.g., "Running", "Idle") using `UserId` as the key.
- **Real-Time Updates**: Uses `SignalPlus` to fire events whenever a player's state changes.
- **Memory Safety**: Includes a method to clear player states to prevent memory leaks.
- **Type Checking**: Ensures valid input for state updates with warning messages for invalid inputs.

## Installation

1. Download the `statsManager` module and place it in your Roblox Studio project (e.g., in `ServerScriptService` or `ReplicatedStorage`).
2. Ensure the `SignalPlus` module is included in your project and accessible to `statsManager`.
3. Require the module in your script:

   ```lua
   local statsManager = require(path.to.statsManager)
   ```

## API Reference

### `statsManager.setStat(player: Player, stat: string)`

Sets the current state of a player and triggers a change signal.

- **Parameters**:
  - `player`: The `Player` instance whose state is being set.
  - `stat`: A string representing the new state (e.g., "Running", "Jumping").
- **Behavior**: If `stat` is not a string, a warning is issued, and no state is set.

### `statsManager.getStat(player: Player): string`

Retrieves the current state of a player.

- **Parameters**:
  - `player`: The `Player` instance whose state is being queried.
- **Returns**: The player's state as a string, or `"Idle"` if no state is set.
- **Behavior**: Issues a warning if the player has no state.

### `statsManager.removeStat(player: Player)`

Clears the saved state of a player to prevent memory leaks.

- **Parameters**:
  - `player`: The `Player` instance whose state should be removed.
- **Usage**: Call this when a player leaves the game.

### `statsManager.onStatChanged(callback: (Player, string) -> ())`

Connects a callback function to handle state change events.

- **Parameters**:
  - `callback`: A function that receives the `Player` instance and the new state string.
- **Returns**: An `RBXScriptConnection` to allow disconnection of the callback.

## Usage Examples

### Example 1: Setting and Retrieving Player States

```lua
local statsManager = require(game.ServerScriptService.statsManager)

-- Set a player's state
local player = game.Players:GetPlayerByUserId(123456)
statsManager.setStat(player, "Running")

-- Retrieve the player's state
local currentState = statsManager.getStat(player)
print(currentState) -- Output: Running

-- Set an invalid state (will warn)
statsManager.setStat(player, 123) -- Warning: The stat entered in argument isn't a string
```

### Example 2: Listening for State Changes

```lua
local statsManager = require(game.ServerScriptService.statsManager)

-- Connect to state changes
statsManager.onStatChanged(function(player, newState)
    print(player.Name .. " changed state to: " .. newState)
end)

-- Change a player's state
local player = game.Players:GetPlayerByUserId(123456)
statsManager.setStat(player, "Jumping") -- Output: PlayerName changed state to: Jumping
```

### Example 3: Cleaning Up When a Player Leaves

```lua
local statsManager = require(game.ServerScriptService.statsManager)
local Players = game:GetService("Players")

Players.PlayerRemoving:Connect(function(player)
    statsManager.removeStat(player)
    print("Removed state for " .. player.Name)
end)
```

## Notes

- Ensure `SignalPlus` is properly configured in your project for event handling.
- Always call `removeStat` when a player leaves to avoid memory leaks.
- The module assumes states are strings for simplicity and consistency.
- For large-scale projects, consider pooling states to optimize performance further.

## Contributing

Feel free to contribute to the module by submitting feedback or improvements to **flashy3467dieux** on the Roblox DevForum or via the module's source repository (if available).