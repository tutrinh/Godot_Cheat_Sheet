# Godot_Cheat_Sheet
Godot 3.1 Cheat Sheet

Basic GD scripts
https://docs.godotengine.org/en/3.1/getting_started/scripting/gdscript/gdscript_basics.html?highlight=setget

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

## Onready Keyword
When using nodes, it’s common to desire to keep references to parts of the scene in a variable. As scenes are only warranted to be configured when entering the active scene tree, the sub-nodes can only be obtained when a call to *Node._ready()* is made.
```python
var my_label

func _ready():
    my_label = get_node("MyLabel")
```

This can get a little cumbersome, especially when nodes and external references pile up. For this, GDScript has the onready keyword, that defers initialization of a member variable until _ready is called. It can replace the above code with a single line:
```python
onready var my_label = get_node("MyLabel")
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

### Saving Game
[http://docs.godotengine.org/en/3.1/tutorials/io/saving_games.html] (Saving)

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

### TYPES
#### Array
```
var arr = []
PoolIntArray // Array of integers
PoolStringArray // Array of Strings
PoolColorArray // Array of Color Objects

```
### Dictionary
Associative container which contains values referenced by unique keys.
```
var d = {4: 5, "A key": "A value", 28: [1, 2, 3]}
d["Hi!"] = 0
d = {
    22: "value",
    "some_key": 2,
    "other_key": [2, 3, 4],
    "more_key": "Hello"
}
```

### Casting
```
var my_int: int
my_int = "123" as int # The string can be converted to int
my_int = Vector2() as int # A Vector2 can't be converted to int, this will cause an error
```

### Strong Type
```
const A: int = 5
const B: Vector2 = Vector2()
```

### Enums
Enums are basically a shorthand for constants, and are pretty useful if you want to assign consecutive integers to some constant.

```
enum {TILE_BRICK, TILE_FLOOR, TILE_SPIKE, TILE_TELEPORT}
# Is the same as:
const TILE_BRICK = 0
const TILE_FLOOR = 1
const TILE_SPIKE = 2
const TILE_TELEPORT = 3

enum State {STATE_IDLE, STATE_JUMP = 5, STATE_SHOOT}
# Is the same as:
const State = {STATE_IDLE = 0, STATE_JUMP = 5, STATE_SHOOT = 6}
# Access values with State.STATE_IDLE, etc.
```

### Functions
```
func my_function(a: int, b: String):
    pass
  
func my_function(int_arg := 42, String_arg := "string"):
    pass
    

```
### Functions with Return Type
```
The return type of the function can be specified after the arguments list using the arrow token (->):
func my_int_function() -> int:
    return 0
    
void_function() -> void:
    return # Can't return a value
```

### Functions by References
Contrary to Python, functions are not first class objects in GDScript. This means they cannot be stored in variables, passed as an argument to another function or be returned from other functions. This is for performance reasons.

To reference a function by name at runtime, (e.g. to store it in a variable, or pass it to another function as an argument) one must use the call or funcref helpers:

```python
# Call a function by name in one step.
my_node.call("my_function", args)

# Store a function reference.
var my_func = funcref(my_node, "my_function")
# Call stored function reference.
my_func.call_func(args)

```

### Static Functions
A function can be declared static. When a function is static, it has no access to the instance member variables or self. This is mainly useful to make libraries of helper functions:

```python
static func sum2(a, b):
    return a + b
```

## Statements and Control Flow
### if/else/elif
```
if [expression]:
    statement(s)
elif [expression]:
    statement(s)
else:
    statement(s)
```
#### Shorthand
```
if 1 + 1 == 2: return 2 + 2
else:
    var x = 3 + 3
    return x
```
Sometimes you might want to assign a different initial value based on a boolean expression. In this case, ternary-if expressions come in handy:
```
var x = [value] if [expression] else [value]
y += 3 if y < 10 else -1
```

### For
```
for x in [5, 7, 11]:
    statement # Loop iterates 3 times with 'x' as 5, then 7 and finally 11.

var dict = {"a": 0, "b": 1, "c": 2}
for i in dict:
    print(dict[i])

for i in range(3):
    statement # Similar to [0, 1, 2] but does not allocate an array.

for i in range(1,3):
    statement # Similar to [1, 2] but does not allocate an array.

for i in range(2,8,2):
    statement # Similar to [2, 4, 6] but does not allocate an array.

for c in "Hello":
    print(c) # Iterate through all characters in a String, print every letter on new line.
```

### Match
A match statement is used to branch execution of a program. It’s the equivalent of the switch statement found in many other languages, but offers some additional features.

```
match [expression]:
    [pattern](s):
        [block]
    [pattern](s):
        [block]
    [pattern](s):
        [block]
```
Same as switch statements
```
match x:
    1:
        print("We are number one!")
    2:
        print("Two are better than one!")
    "test":
        print("Oh snap! It's a string!")
```
**Wildcard pattern**
This pattern matches everything. It’s written as a single underscore.

It can be used as the equivalent of the default in a switch statement in other languages.

```
match x:
    1:
        print("It's one!")
    2:
        print("It's one times two!")
    _:
        print("It's not 1 or 2. I don't care tbh.")
```

### While
```python
var i = 0

while i < strings.size():
    print(strings[i])
    i += 1
```

## Classes
By default, all script files are unnamed classes. In this case, you can only reference them using the file’s path, using either a relative or an absolute path. For example, if you name a script file character.gd

```python
# Inherit from Character.gd

extends res://path/to/character.gd

# Load character.gd and create a new node instance from it

var Character = load("res://path/to/character.gd")
var character_node = Character.new()
```
Instead, you can give your class a name to register it as a new type in Godot’s editor. For that, you use the ‘class_name’ keyword. You can add an optional comma followed by a path to an image, to use it as an icon. Your class will then appear with its new icon in the editor:

```python
# Item.gd

extends Node

class_name Item, "res://interface/icons/item.png"
```
```python
# Saved as a file named 'character.gd'.

class_name Character

var health = 5

func print_health():
    print(health)

func print_this_script_three_times():
    print(get_script())
    print(ResourceLoader.load("res://character.gd"))
    print(Character)
```

### Inheritance
```python
# Inherit/extend a globally available class.
extends SomeClass

# Inherit/extend a named class file.
extends "somefile.gd"

# Inherit/extend an inner class in another file.
extends "somefile.gd".SomeInnerClass
```
To check if a given instance inherits from a given class, the is keyword can be used:
```
# Cache the enemy class.
const Enemy = preload("enemy.gd")

# [...]

# Use 'is' to check inheritance.
if (entity is Enemy):
    entity.apply_damage()
```
### Super
```python
func some_func(x):
    .some_func(x) # Calls same function on the parent class.
```

### Inner Classes
A class file can contain inner classes. Inner classes are defined using the class keyword. They are instanced using ttheir ** ClassName.new()** function.
```python
# Inside a class file.

# An inner class in this class file.
class SomeInnerClass:
    var a = 5
    func print_value_of_a():
        print(a)

# This is the constructor of the class file's main class.
func _init():
    var c = SomeInnerClass.new()
    c.print_value_of_a()
```

Read more [https://docs.godotengine.org/en/3.1/getting_started/scripting/gdscript/gdscript_basics.html?highlight=setget#built-in-types]



### Colors
https://docs.godotengine.org/en/3.1/classes/class_color.html#class-color


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

### Range
```python
range(n) # Will go from 0 to n-1
range(b, n) # Will go from b to n-1
range(b, n, s) # Will go from b to n-1, in steps of s
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
Signals are a way to send notification messages from an object that other objects can listen to in a generic way. Create custom signals for a class using the signal keyword.
```javascript
signal hit
emit_signal("hit")
```
```pythong
# Signal with no arguments
signal your_signal_name

# Signal with two arguments
signal your_signal_name_with_args(a, b)
```
Here’s an example that creates a custom **signal** in one script and connects the custom **signal** to a method in a separate script, using the *Object.connect()* method:

```python
# your_notifier.gd

signal data_found

var your_data = 42
```

```python
# your_handler.gd

func your_handler():
   print("Your handler was called!")
```

```python
# your_game.gd

func _ready():
   var notifier = your_notifier.new()
   var handler = your_handler.new()

   notifier.connect("data_found", handler, "your_handler")
```

```python
func pressed_handler(which):
   print("A button was pressed! Button's name was:", which.get_name())

func _ready():
   for button in get_node("buttons").get_children()
      # Connect the button's 'pressed' signal to our 'pressed_handler' method
      # Bind the button to the connection so we know which button was pressed
      button.connect("pressed", self, "pressed_handler", [button])
```

Signals are generated by the *Object.emit_signal()* method which broadcasts the signal and arguments.

```python
# your_notifier.gd

signal data_found(data)

var your_data = 42

func _process(delta):
   if delta == your_data:
      emit_signal("data_found", your_data)
```

```python
# your_handler.gd

func your_handler(data, obj):
   print("Your handler was called from: ", obj.get_name(), " with data: ", data)
```

```python
# your_game.gd

func _ready():
   var notifier = your_notifier.new()
   var handler = your_handler.new()

   notifier.connect("data_found", handler, "your_handler", [notifier])
```

## YIELD
GDScript offers support for coroutines via the yield built-in function. Calling yield() will immediately return from the current function, with the current frozen state of the same function as the return value. Calling resume on this resulting object will continue execution and return whatever the function returns. Once resumed, the state object becomes invalid. Here is an example:

```python
func my_func():
   print("Hello")
   yield()
   print("world")

func _ready():
    var y = my_func()
    # Function state saved in 'y'.
    print("my dear")
    y.resume()
    # 'y' resumed and is now an invalid state.
```

```python
Hello
my dear
world
```
It is also possible to pass values between yield() and resume(), for example:
```python
func my_func():
   print("Hello")
   print(yield())
   return "cheers!"

func _ready():
    var y = my_func()
    # Function state saved in 'y'.
    print(y.resume("world"))
    # 'y' resumed and is now an invalid state.
```
Will print:
```python
Hello
world
cheers!
```

### Yield and Signals
The real strength of using yield is when combined with signals. yield can accept two arguments, an object and a signal. When the signal is received, execution will recommence. Here are some examples:

```python
# Resume execution the next frame.
yield(get_tree(), "idle_frame")

# Resume execution when animation is done playing.
yield(get_node("AnimationPlayer"), "finished")

# Wait 5 seconds, then resume execution.
yield(get_tree().create_timer(5.0), "timeout")
```
Coroutines themselves use the completed signal when they transition into an invalid state, for example:
```python

```

```python
func my_func():
    yield(button_func(), "completed")
    print("All buttons were pressed, hurray!")

func button_func():
    yield($Button0, "pressed")
    yield($Button1, "pressed")
    
# my_func will only continue execution once both buttons have been pressed.
```


## PHYSICS BODY
### PhysicsBody2D Mask
If you don't want the enemies to collide with each other then in the MASK property, uncheck the first box. They won't collide with each other

## Setters/getters
It is often useful to know when a class’ member variable changes for whatever reason. It may also be desired to encapsulate its access in some way.

```javascript
var variable = value setget setterfunc, getterfunc
```

Whenever the value of variable is modified by an external source (i.e. not from local usage in the class), the setter function (setterfunc above) will be called. This happens before the value is changed. The setter must decide what to do with the new value. Vice versa, when variable is accessed, the getter function (getterfunc above) must return the desired value.

```javascript
var myvar setget my_var_set, my_var_get

func my_var_set(new_value):
    my_var = new_value

func my_var_get():
    return my_var # Getter must return a value.
```

Either of the setter or getter functions can be omitted:

```javascript
# Only a setter.
var my_var = 5 setget myvar_set
# Only a getter (note the comma).
var my_var = 5 setget ,myvar_get
```



---

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
[http://docs.godotengine.org/en/3.1/tutorials/io/background_loading.html#doc-background-loading] (More info)

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






