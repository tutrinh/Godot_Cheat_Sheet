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

### Loading images from resources
```python
var res . = load("res://robi.png")
get_node("sprite").texture = res
```

### Changing Scene
```python
get_tree().change_scene("res://levels/level2.tscn")
-
var next_scene = preload("res://levels/level2.tscn")
get_tree().change_scene_to(next_scene)

```

### Preload resources
Will read file from the disk and load it at compile time.
```python
var res = preload("res://robi.png")
get_node("sprite").texture =- res
```
### Load instance of a scene
```python
var bullet = preload("res://bullet.tscn").instance()
add_child(bullet)
```
Can create new bullet without having to load them again from disk

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
---
### Clamp
```javascript
clamp() = restrict values in given range
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

### ColorRect for BG Solid Colors

### Sound Effects
**AudioStreamPlayer (node)**

### Particles

### Tween
```python
interpolate_property()
tween.start()
tween.is_active()
```

### Animation
**AnimationPlayer (Node)**


## SIGNALS
```javascript
signal hit
emit_signal("hit")
```

## PHYSICS BODY
### PhysicsBody2D Mask
If you don't want the enemies to collide with each other then in the MASK property, uncheck the first box. They won't collide with each other

## INSPECTOR
### Exports
This will let the inspector (GUI) to change the values of the variables 
```python
export (int) var min_speed
export (Array, String) var strings
```
### PackedScene
Choosing the Class/Scene you want to instance from the GUI. Drag Mob.tscn from the left, file system, and drop it in the right under the Script Variables in the inspector.
```python
export (PackedScene) var Mob
```


## INSTANCE OF A CLASS/TSCN
### GUI
Click on the chain link icon on the left panel under the Scene tab.


## CREATING A CLASSS INSIDE GDSCRIPT
```python
extends Node
class MyObject:
  extends Object
  var dict = {}

func _ready():
  var obj1 = MyObject.new()
  obj1.dict.greeting = "hello"
  var obj2= MyObject.new()
  obj2.dict.greeting = "hello2"
```

## HUD / CanvasLayer/ Sits above the Game / UI
### Control Nodes for UI
- Buttons
- Labels
- TextureRect
- TextureProgress
- TextureButtons

### Properties of Control Nodes
- Positions
- Size
- Anchors
- Margins






