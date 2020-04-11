# Setters / Getters

Define setters and getters when properties alter the object’s state or if changing the property triggers methods

Start with a leading underscore when writing setters or getters for private variables just like the variable.

```text
var animation_length := 1.5
var tile_size := 40
var side_length := 5 setget set_side_length, get_side_length

var _count := 0 setget _set_count, _get_count
var _state := Idle.new()
```

