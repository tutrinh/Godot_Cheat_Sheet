# Variables

Public and Private

After that enums, constants, exported, public \(regular name\), and pseudo-private \(starting with `_`\) variables, in this order.

Enum type names should be in `CamelCase` while the enum values themselves should be in `ALL_CAPS_SNAKE_CASE`. This order is important because exported variables might depend on previously defined enums and constants.

```text
enum TileTypes { EMPTY=-1, WALL, DOOR }

const MAX_TRIALS := 3
const TARGET_POSITION := Vector2(2, 56)

export var number := 0
export var is_active := true
```

Const is all caps

## Boolean

_Note:_ for booleans, always include a name prefix like `is_`, `can_`, or `has_`.

```text
export var is_active := true
```



