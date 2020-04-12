# Code Writing Style



```text
class_name StateMachine
extends Node


signal state_changed(previous, new)

export var initial_state := NodePath()

onready var state: State = get_node(initial_state) setget set_state
onready var _state_name := state.name


func _init() -> void:
    add_to_group("state_machine")


func _ready() -> void:
    connect("state_changed", self, "_on_state_changed")
    state.enter()


func _unhandled_input(event: InputEvent) -> void:
    state.unhandled_input(event)


func _physics_process(delta: float) -> void:
    state.physics_process(delta)


# Replaces the current state with the state at `target_state_path` if the path
# is valid. Passes the `msg` dictionary to the new state's `enter` function.
func transition_to(target_state_path: String, msg: Dictionary = {}) -> void:
    if not has_node(target_state_path):
        return

    var target_state := get_node(target_state_path)
    assert(target_state.is_composite == false)

    state.exit()
    self.state = target_state
    state.enter(msg)
    Events.emit_signal("player_state_changed", state.name)


func set_state(value: State) -> void:
    state = value
    _state_name = state.name


func _on_state_changed(previous: Node, new: Node) -> void:
    print("state changed")
    emit_signal("state_changed")
```

