# Godot_Cheat_Sheet
Godot 3.1 Cheat Sheet

### Naming Convention

- Class - PascalCase
- Variables - snake_case
- Const - ALL_CAPS

```javascript
Object/Class is a .tscsn
```
### Get Node
```javascript
get_node("NameOfNode") same as $NameOfNode
```
### Clamp
```javascript
clamp() = restrict values in given range
```

## SCRIPTS
### Update Function
```python
func _process(delta):
  if condition:
    do something
  else:
    do something else
    
```

### Ready
When the node enters the scene tree
```javascript
func _ready():
  screensize = get_viewport_rect().size
```

### Hide
```javascript
hide()
```

### Free
```javascript
queue.free()
```

### Disable Node
```javascript
$CollisionShape2D.disabled = true/false
```

## SIGNALS
```javascript
signal hit
```

## PHYSICS BODY
### PhysicsBody2D Mask
If you don't want the enemies to collide with each other then in the MASK property, uncheck the first box. They won't collide with each other

## INSPECTOR
### Exports
This will let the inspector (GUI) to change the values of the variables 
```python
export (int) var min_speed
```







