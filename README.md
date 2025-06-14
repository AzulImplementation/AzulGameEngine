# Azul game engine

A game engine for Azul board game.

Original game: [https://en.wikipedia.org/wiki/Azul_(board_game)](Azul).

[![PyPI version](https://badge.fury.io/py/azul-game-engine.svg)](https://pypi.org/project/azul-game-engine/)

## Functionality

As a client of this game engine the only class you need to use is `game.py`.

### Initializing `game.py`

```python
lid = Lid()
player1 = Player(Board(wall=Wall(), floor=Floor(lid)), f"Player Name 1")
player2 = Player(Board(wall=Wall(), floor=Floor(lid)), f"Player Name 2")
game = Game([player1, player2])
 ```

 ### Using `game.py`

`game.py` class has 3 main methods for clients to use:
- `json_object`
- `execute_factory_offer_phase_with_factory`
- `execute_factory_offer_phase_with_center`

#### `json_object()`

Returns a dictionary representation of the current game state, suitable for JSON serialization.

**Returns:**
- `dict`: A dictionary containing:
  - `isRunning`: Boolean indicating if the game is still active
  - `Factory displays`: List of factory display states
  - `Center`: Current center tile state
  - `Players`: List of all player states (including pattern lines, wall, floor)
  - `Bag`: Current bag tile counts
  - `Lid`: Current lid tile counts
  - `Winners`: List of winner names (only present when game has ended)

**Example:**
```python
state = game.json_object()
```

#### `execute_factory_offer_phase_with_factory(factory_index, tile_to_take, amount_to_place_on_floor, pattern_line_index)`

Executes the current player's turn by taking tiles from a specified factory display.

**Parameters:**
- `factory_index` (int): Index of the factory display to take tiles from. Starting index 0. Ending index: 4 on two player game, 6 on 3 player game, 8 on 4 player game.
- `tile_to_take` (Tile): The specific tile color to take from the factory
- `amount_to_place_on_floor` (int): Number of tiles to place on the floor
- `pattern_line_index` (int): Index of the pattern line to place tiles on (0-4)

**Behavior:**
- Takes all tiles of the specified color from the chosen factory display
- Places remaining tiles from that factory into the center
- Places tiles on the specified pattern line (and floor on some cases)
- Advances to the next player's turn
- Triggers wall tiling phase if all factories and center are empty

**Raises:**
- `ActionNotAllowedException`: If the game has already ended or move is illegal (e.g. user wants to take a Blue tile from a factory but the factory does not have blue tiles).

**Example:**
```python
# Player takes red tiles from factory 0, places 2 on floor, rest on pattern line 1
game.execute_factory_offer_phase_with_factory(0, Tile.RED, 2, 1)
```

#### `execute_factory_offer_phase_with_center(tile_to_take, amount_to_place_on_floor, pattern_line_index)`

Executes a player's turn by taking tiles from the center area.

**Parameters:**
- `tile_to_take` (Tile): The specific tile color to take from the center
- `amount_to_place_on_floor` (int): Number of tiles to place on the floor
- `pattern_line_index` (int): Index of the pattern line to place tiles on (0-4)

**Behavior:**
- Takes all tiles of the specified color from the center
- First player to take from center receives the starting player marker (-1 point penalty)
- Places tiles on the specified pattern line (and floor on some cases)
- Advances to the next player's turn
- Triggers wall tiling phase if all factories and center are empty

**Raises:**
- `ActionNotAllowedException`: If the game has already ended or move is illegal (e.g. user wants to take a Blue tile from the center but there is no blue tile in it).

**Example:**
```python
# Player takes blue tiles from center, places 1 on floor, rest on pattern line 0
game.execute_factory_offer_phase_with_center(Tile.BLUE, 1, 0)
```

## Installing the project

Inside the directory this file is located run `pip install .`

## Running tests

After you have installed the project, run this command `pytest -vv` in the same directory as this file.

[![codecov](https://codecov.io/gh/AzulImplementation/AzulGameEngine/branch/main/graph/badge.svg)](https://codecov.io/gh/AzulImplementation/AzulGameEngine)


## License

Please read a `LICENSE` file.