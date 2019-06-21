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

### Randomize
```python
randomize() //this will turn it on
randi() % n, random integer between 0 and n-1
```

### Hide
```javascript
hide()
```

### Free
```javascript
queue_free()
```

### Disable Node
```javascript
$CollisionShape2D.disabled = true/false
```

### Path2D


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
### PackedScene
Choosing the Class/Scene you want to instance from the GUI. Drag Mob.tscn from the left, file system, and drop it in the right under the Script Variables in the inspector.
```python
export (PackedScene) var Mob
```


## INSTANCE OF A CLASS/TSCN
### GUI
Click on the chain link icon on the left panel under the Scene tab.


## HUD / CanvasLayer
### Control Nodes
- Buttons
- Labels







