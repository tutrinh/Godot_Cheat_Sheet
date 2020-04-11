# Signal

### Use past tense

Signals go first and don’t use parentheses unless they pass function parameters. Use the past tense to name signals. Append `_started` or `_finished` if the signal corresponds to the beginning or the end of an action.

```text
signal moved
signal talk_started(parameter_name)
signal talk_finished
```

### Signal Callbacks

For signal callbacks, we use Godot’s convention, `_on_NodeName_signal_name`:

```text
func _on_Quest_started(which: Quest) -> void:
  ...
  
```

You should remove `NodeName` if the object connects to itself:

```text
class_name HitBox
extends Area2D


func _ready() -> void:
  connect("area_entered", self, "_on_area_entered")
```

