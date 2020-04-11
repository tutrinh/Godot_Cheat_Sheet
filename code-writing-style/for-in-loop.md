# For In Loop

```text
func _set_elements(elements: int) -> bool:
...
  for i in range(elements):
    var e := Element.new()
    e.node_a = "../StaticBody2D"
    e.position = skin_viewport_staticbody.position
...
```

```text
for member in party.get_members():
    if member != leader:
      path = [member.position] + path
      path.pop_back()
    member.walk(path)
  return true
```

