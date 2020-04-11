# Ready / Init

Place `onready` variables right before `_init` and/or `_ready` functions to visually connect these with their usage inside the `_ready` function.

```text
onready var timer := $HungerCheckTimer
onready var ysort := $YSort
```

Next define virtual methods from Godot \(those starting with a leading `_`, e.g. `_ready`\). Always leave 2 blanks lines between methods to visually distinguish them from other code blocks.

```text
func _init() -> void:
  pass



```

```text
func _process(delta: float) -> void:
  pass
```

