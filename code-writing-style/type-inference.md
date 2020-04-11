# Type Inference

```text
var direction: Vector2 = get_move_direction()
```

Godot can infer the type of the variable for us. In that case, we only need to add a colon after the variable’s name:

```text
func get_move_direction() -> Vector2:
  var direction := Vector2(
      ...
  )
  return direction

var direction := get_move_direction() # The variable's type is Vector2
```

```text
var array := [1, "Some text"]
var text: String = array.pop_back()
```

