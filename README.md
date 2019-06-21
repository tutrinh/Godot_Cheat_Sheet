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
get_node("sprite").texture = res
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

---

## SINGLETON / AUTOLOAD
Singleton pattern is useful tool for solving the persistent info between scenes.
### Concept
- Create objs that are always loaded no matter which snce is running
- Can store global variables
- Can handle switching scenes and between scene transitions
- Act like a singleton

### Autoload a script to act like a singleton
To autoload a script:
Project > Project Settings > Autoload Tab > Select the tscn/gd script
- Add the gd script
- Give it a name
- Check the enabled

```
Assuming you name it PlayerVariables is the name in the AutoLoad slot
PlayerVariables.health -= 10

Another way if the enabled is not checked
var player_vars = get_node("/root/PlayerVariables")
player_vars.health -= 10

```

### Creating a custom scene switcher using Autoload
- Create a Global.gd
- Add it to Autoload, Project > Project Settings > AutoLoad Tab
- Autoloaded nodes are always first
- This means that the last child of root is always the loaded scene.
- 

```
extends Node

var current_scene = null

func _ready():
    var root = get_tree().get_root()
    current_scene = root.get_child(root.get_child_count() - 1)
# Now we need a function for changing the scene. This function needs to free the current scene and replace it with the requested one.

func goto_scene(path):
    # This function will usually be called from a signal callback,
    # or some other function in the current scene.
    # Deleting the current scene at this point is
    # a bad idea, because it may still be executing code.
    # This will result in a crash or unexpected behavior.

    # The solution is to defer the load to a later time, when
    # we can be sure that no code from the current scene is running:

    call_deferred("_deferred_goto_scene", path)


func _deferred_goto_scene(path):
    # It is now safe to remove the current scene
    current_scene.free()

    # Load the new scene.
    var s = ResourceLoader.load(path)

    # Instance the new scene.
    current_scene = s.instance()

    # Add it to the active scene, as child of root.
    get_tree().get_root().add_child(current_scene)

    # Optionally, to make it compatible with the SceneTree.change_scene() API.
    get_tree().set_current_scene(current_scene)


```

Using Object.call_deferred(), the second function will only run once all code from the current scene has completed. Thus, the current scene will not be removed while it is still being used (i.e. its code is still running).

Finally, we need to fill the empty callback functions in the two scenes:

```
# Add to 'Scene1.gd'.

func _on_Button_pressed():
    Global.goto_scene("res://Scene2.tscn")
```

```
# Add to 'Scene2.gd'.

func _on_Button_pressed():
    Global.goto_scene("res://Scene1.tscn")
```

## Background loading
### ResourceInteractiveLoader

The ResourceInteractiveLoader class allows you to load a resource in stages. Every time the method poll is called, a new stage is loaded, and control is returned to the caller. Each stage is generally a sub-resource that is loaded by the main resource. For example, if you’re loading a scene that loads 10 images, each image will be one stage.

### Usage
[http://docs.godotengine.org/en/3.1/tutorials/io/background_loading.html#doc-background-loading](More info)

---

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






