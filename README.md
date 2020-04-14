# Godot\_Cheat\_Sheet

## Godot\_Cheat\_Sheet

Godot 3.1 Cheat Sheet

Basic GD scripts [https://docs.godotengine.org/en/3.1/getting\_started/scripting/gdscript/gdscript\_basics.html?highlight=setget](https://docs.godotengine.org/en/3.1/getting_started/scripting/gdscript/gdscript_basics.html?highlight=setget)

### Settings

Settings

Stretch M 2d Aspect expand Shrink 1

iPhone 8 Iphone x

Size: 1280 x 720 iPhone landscape = 1560 x 720 iPHone x portrait = 720X1560 Stretch Mode: 2d Allo HiDPI: checked Aspect: expand for iPhone X, looks good

### Images

Sketch, export image as @2x png. Mode &gt; Uncompressed in the import tab next to the Scene tab. This works best.

#### Naming Convention

* Class - PascalCase
* Variables - snake\_case
* Const - ALL\_CAPS

```javascript
Object/Class is a .tscsn
```

#### Scene Tree

```python
get_tree()
```

#### Useful functions

```python
# Create timer
# This creaates a one shot timer
# Basically creating a delay
create_timer ( float time_sec, bool pause_mode_process=true )

func _ready():
    print("start")
    yield(get_tree().create_timer(10), "timeout")
    print("10 seconds after")

# Get Current Scene
current_scene

# Change Scene
change_scene(path)

# Get Nodes in Group
get_nodes_in_group ( String group )

# Has Group
has_group ( String name ) const

# Reload Current Scene
reload_current_scene ( )
```

#### Get Node

```javascript
get_node("NameOfNode") same as $NameOfNode
```

#### Get Node Path from Export Inspector

```javascript
export (NodePath) var targetPath
```

#### Parent Node

```javascript
get_node(“/root/parent”)
```

#### Assigning Node in the Editor using the Export keyword from your script

```python
var target
export(NodePath) var targetPath

func _ready():
    target = get_get_node(targetPath)
```

#### Add to Group

```python
add_to_group("enemies")
```

#### Calling Members of the Group

```python
func _on_discovered(): # This is a purely illustrative function.
    get_tree().call_group("enemies", "player_was_discovered")
The above code calls the function player_was_discovered on every member of the group enemies.
```

#### Getting Members from the Group

```python
var enemies = get_tree().get_nodes_in_group("enemies")
```

#### Instance vs. New

**Creating a node use new\(\)**

```python
var s
func _ready():
    s = Sprite.new() # Create a new sprite!
    add_child(s) # Add it as a child of this node.
```

**Instance is for Scenes**

Instancing a scene from code is done in two steps. The first one is to load the scene from your hard drive:

```python
var scene = preload("res://myscene.tscn") # Will load when parsing the script.
```

```python
var node = scene.instance()
add_child(node)
```

The advantage of this two-step process is that a packed scene may be kept loaded and ready to use so that you can create as many instances as desired. This is especially useful to quickly instance several enemies, bullets, and other entities in the active scene.

### Resource? \(.tres\)

#### Scriptable Objects \(.tres\)

Objects that can store data, alternative to jSON

```python
ClassStats.gd
# When creating a new gdscript change the inherit from node to resource
extends Resource
class_name ClassStats

export(String) var class_type ="Knight"
export(int) var health = 100
export(int) var strength = 100
export(Image) var profile = null
```

```python
# Create a new resource that will hold the data from ClassStats Class
# Wizard.tres
Now you can change the values in the editor
This will store predefined data from the ClassStats with the updated values
```

**Usage**

```python
From another node
# load the resource that that the data you want to use
onready var character_data = load("res://Wizard.tres")
func _ready():

    character_data.profile
    character_data.health
```

### Onready Keyword

When using nodes, it’s common to desire to keep references to parts of the scene in a variable. As scenes are only warranted to be configured when entering the active scene tree, the sub-nodes can only be obtained when a call to _Node.\_ready\(\)_ is made.

```python
var my_label

func _ready():
    my_label = get_node("MyLabel")
```

This can get a little cumbersome, especially when nodes and external references pile up. For this, GDScript has the onready keyword, that defers initialization of a member variable until \_ready is called. It can replace the above code with a single line:

```python
onready var my_label = get_node("MyLabel")
```

```python
onready var bg = $ColorRect

func _ready():
    bg.color = Color("FFFFFF")
```

### Pausing a Game

[https://docs.godotengine.org/en/3.1/tutorials/misc/pausing\_games.html](https://docs.godotengine.org/en/3.1/tutorials/misc/pausing_games.html)

```python
get_tree().paused = true
```

Make sure to _white-list nodes_ you don't want to be affected by the pause In the node's inspector, scroll down to Pause. Change to one

* **Inherit:** Process depending on the state of the parent, grandparent, etc. The first parent that has a non-Inherit state.
* **Stop:** Stop the node no matter what \(and children in Inherit mode\). When paused this node will not process.
* **Process:** Process the node no matter what \(and children in Inherit mode\). Paused or not this node will process.

```python
func _on_pause_button_pressed():
    get_tree().paused = true
    $pause_popup.show()
```

### Saving Game

Save settings from a game.

* Create Settings.gd, this is your global settings
* Add to Project &gt; Project Settings &gt; Autoload &gt; Add Settings.gd

```python
var enable_sound = true
var enable_music = true

# PRELOADING THE IMAGE TEXTURES
var sound_buttons = {true: preload("res://assets/images/buttons/audioOn.png"),
                    false: preload("res://assets/images/buttons/audioOff.png")}
var music_buttons = {true: preload("res://assets/images/buttons/musicOn.png"),
                    false: preload("res://assets/images/buttons/musicOff.png")}

var settings_file = "user://settings.save"

# SAVE function
func save_settings():
    var f = File.new()
    f.open(settings_file, File.WRITE)
    f.store_var(enable_sound)
    f.store_var(enable_music)
    f.close()

# LOAD function
    var f = File.new()
    if f.file_exists(settings_file):
        f.open(settings_file, File.READ)
        enable_sound = f.get_var()
        enable_music = f.get_var()
        f.close()
```

#### Loading images from resources

```python
var res = load("res://robi.png")
get_node("sprite").texture = res
```

#### Changing Scene

```python
get_tree().change_scene("res://levels/level2.tscn")
-
var next_scene = preload("res://levels/level2.tscn")
get_tree().change_scene_to(next_scene)
```

#### Current Scene

```javascript
get_tree().reload_current_scene()
```

#### Saving Game

\[[http://docs.godotengine.org/en/3.1/tutorials/io/saving\_games.html](http://docs.godotengine.org/en/3.1/tutorials/io/saving_games.html)\] \(Saving\)

#### Preload resources

Will read file from the disk and load it at compile time.

```python
var res = preload("res://robi.png")
get_node("sprite").texture = res
```

#### Load instance of a scene

```python
var bullet = preload("res://bullet.tscn").instance()
add_child(bullet)
```

Can create new bullet without having to load them again from disk

### SCRIPTS

#### TYPES

**Array**

```javascript
var arr = []
PoolIntArray // Array of integers
PoolStringArray // Array of Strings
PoolColorArray // Array of Color Objects
```

#### Dictionary

Associative container which contains values referenced by unique keys.

```javascript
var d = {4: 5, "A key": "A value", 28: [1, 2, 3]}
d["Hi!"] = 0
d = {
    22: "value",
    "some_key": 2,
    "other_key": [2, 3, 4],
    "more_key": "Hello"
}
```

#### Casting

```javascript
var my_int: int
my_int = "123" as int # The string can be converted to int
my_int = Vector2() as int # A Vector2 can't be converted to int, this will cause an error
```

#### Strong Type

```javascript
const A: int = 5
const B: Vector2 = Vector2()
```

#### Enums

Enums are basically a shorthand for constants, and are pretty useful if you want to assign consecutive integers to some constant.

```javascript
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

#### Vector2, built ins

Useage: Vector2.RIGHT

* ZERO = Vector2\( 0, 0 \) — Zero vector.
* ONE = Vector2\( 1, 1 \) — One vector.
* INF = Vector2\( inf, inf \) — Infinite vector.
* LEFT = Vector2\( -1, 0 \) — Left unit vector.
* RIGHT = Vector2\( 1, 0 \) — Right unit vector.
* UP = Vector2\( 0, -1 \) — Up unit vector.
* DOWN = Vector2\( 0, 1 \) — Down unit vector.

#### Functions

```javascript
func my_function(a: int, b: String):
    pass

func my_function(int_arg := 42, String_arg := "string"):
    pass
```

#### Functions with Return Type

```javascript
The return type of the function can be specified after the arguments list using the arrow token (->):
func my_int_function() -> int:
    return 0

void_function() -> void:
    return # Can't return a value
```

#### Functions by References

Contrary to Python, functions are not first class objects in GDScript. This means they cannot be stored in variables, passed as an argument to another function or be returned from other functions. This is for performance reasons.

To reference a function by name at runtime, \(e.g. to store it in a variable, or pass it to another function as an argument\) one must use the call or funcref helpers:

```python
# Call a function by name in one step.
my_node.call("my_function", args)

# Store a function reference.
var my_func = funcref(my_node, "my_function")
# Call stored function reference.
my_func.call_func(args)
```

#### Static Functions

A function can be declared static. When a function is static, it has no access to the instance member variables or self. This is mainly useful to make libraries of helper functions:

```python
static func sum2(a, b):
    return a + b

// like a utility function to perform an operation
```

### Statements and Control Flow

#### if/else/elif

```python
if [expression]:
    statement(s)
elif [expression]:
    statement(s)
else:
    statement(s)
```

**Shorthand**

```python
if 1 + 1 == 2: return 2 + 2
else:
    var x = 3 + 3
    return x
```

Sometimes you might want to assign a different initial value based on a boolean expression. In this case, ternary-if expressions comes in handy:

```python
var x = [value] if [expression] else [value]
y += 3 if y < 10 else -1
```

#### For

```python
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

#### Match

A match statement is used to branch execution of a program. It’s the equivalent of the switch statement found in many other languages, but offers some additional features.

```python
match [expression]:
    [pattern](s):
        [block]
    [pattern](s):
        [block]
    [pattern](s):
        [block]
```

Same as switch statements

```python
match x:
    1:
        print("We are number one!")
    2:
        print("Two are better than one!")
    "test":
        print("Oh snap! It's a string!")
```

**Wildcard pattern** This pattern matches everything. It’s written as a single underscore.

It can be used as the equivalent of the default in a switch statement in other languages.

```python
match x:
    1:
        print("It's one!")
    2:
        print("It's one times two!")
    _:
        print("It's not 1 or 2. I don't care tbh.")
```

```python
func _on_button_pressed(button):
    if settings.enable_sound:
        $Click.play()
    match button.name:
        "Home":
            change_screen($TitleScreen)
        "Play":
            change_screen(null)
            yield(get_tree().create_timer(0.5), "timeout")
            emit_signal("start_game")
        "Settings":
            change_screen($SettingsScreen)
        "Sound":
            settings.enable_sound = !settings.enable_sound
            button.texture_normal = sound_buttons[settings.enable_sound]
        "Music":
            settings.enable_music = !settings.enable_music
            button.texture_normal = music_buttons[settings.enable_music]
```

**Using For and Match for Groups**

```python
func _ready():
    register_buttons()

func register_buttons():
    var buttons = get_tree().get_nodes_in_group("buttons)
    # for loop
    for button in buttons:
        button.connect("pressed", self, "_on_button_pressed", [button.name] #creating argument)

func _on_button_pressed(name):
    match name:
         "Home":
            change_screen($TitleScreen)
        "Play":
            change_screen(null)
            yield(get_tree().create_timer(0.5), "timeout")
            emit_signal("start_game")
        "Settings":
            change_screen($SettingsScreen)
```

#### While

```python
var i = 0

while i < strings.size():
    print(strings[i])
    i += 1
```

### Classes

By default, all script files are unnamed classes. In this case, you can only reference them using the file’s path, using either a relative or an absolute path. For example, if you name a script file character.gd

```python
# Inherit from Character.gd

extends res://path/to/character.gd

# Load character.gd and create a new node instance from it

var Character = load("res://path/to/character.gd")
var character_node = Character.new()
```

Instead, you can give your class a name to register it as a new type in Godot’s editor. For that, you use the ‘class\_name’ keyword. You can add an optional comma followed by a path to an image, to use it as an icon. Your class will then appear with its new icon in the editor:

Example of Class File

```python
Helper Class
//SomeClass.gd

func _init():
    print("Shake class")

// This has to be a static function, only member of the class
// Then the parent is able to call this function         
static func makeWork():
    print("Make work")

static func Sum(a, b):
    return a + b

func localSum(a, b):
    return a + b



// Usage        
// Using the Class
Parent.gd

extends Node2D

var SomeClass = preload("res://classes/Shake.gd")
var myClass

func _ready():
    SomeClass.new() // by doing this you can only call the static functions
    // because you are directly calling the class
    // Calling static functions only
    SomeClass.Sum(2,3)
    // - - - - - - - - - - - - - - - - - 
    // However by assigning the Class to a variable 
    myClass = SomeClass.new()
    // Can access static and non-static functions
    // Now you can access the func without the static (Class functions only)
    myClass.localSum(1,2)
```

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

More example of using _class\_name_ \(will create a new type object in Godot\) to created classes and make new objects from them

**Bullet.gd**

```python
Bullet.gd

extends Area2D

export var damaage := 15
export var move_speed := 500

var direction : Vector2

func _ready() -> void: #onready
    set_as_toplevel(true)
    connect("body_entered", self, "_on_body_entered") # make a signal connection

func _process(delta:float) -> void: #calls every second
    position += direction * move_speed * delta

func _on_body_entered(body) -> void:
    if body.is_a_parent_of(self):
        return
    if body is Enemy: #a custom type, new class called Enemy, Enemy.gd
        body.damage(damage) #calling function damage from Enemy object and passing damage parameter
    queue_free() # destroying self once hit

func initialize(_direction: Vector2) -> void:
    direction = _direction
```

**Enemy.gd, custom class**

```python
Enemy.gd has a Enemy.tscn because it contains Animation Player
extends Kinematic2D
class_name Enemy

onready var animation_player : AnimationPlayer = $AnimationPlayer
onready var collision_shape : CollisionShape2D = $CollisionShape2D

export var health := 100

func damage(value: int) -> void:
    if health == 0:
        return
    health = max(0, health - value)
    if health == 0:
        animation_player.play("die")
        collision_shape.queue_free()
        yield(animation_player, "animation_finished") #waiting for animation to finished
        queue_free() # Enemy destroy self put in que for next cycle
    animation_player.plya("damage")
```

Another Example

**Gun class**

```python
Gun.gd, Has Gun.tscn attached because of AnimationPlayer

extends Node2D
class_name Gun

onready var animated_player : AnimationPlayer = $AnimationPlayer
onready var barrel : Position2D = $Sprite/Barrel
onready var sprite : Sprite = $Sprite
onready var timer : timer = $Timer

export var fire_rate := 5 #setting default values with export
export var bullet : PackedScene # Declared for bullet.tscn 

var direction := Vector2.RIGHT

func _ready() -> void:
    timer.wait_time = 1.0 / fire_rate

func _shoot() -> void:
    animation_player.play("shoot)
```

**Pistol inherit from Gun.gd**

```python
Pistol.gd

extends Gun

func _shoot() -> viod:
    .shoot() # Calling function from super (Gun.gd) to shoot because has same function name
    timer.start()
    var new_bullet := bullet.instance()
    add_child(new_bullet)
    new_bullet.global_position = barrel.global_position
```

**Shotgun inherit from Gun.gd**

```python
extends Gun

export(int, 5, 25) var spread_percentage := 25
export var projectiles := 4

func _ready() -> void:
    randomize() #turn on randomizing

# Loop
func _shoot() -> void:
    .shoot() # Calling function shoot from super b/c same function name
    for i in range(projectiles):
        var new_bullet = bullet.instance()
        var bullet_y_spread = randf() * (spread_percentage / 1000.0) * sign(rand_range(-1, 1)) 
        # what is sign 
        # sign(-6) # returns -1
        # sign(0)  # returns 0
        # sign(6)  # returns 1
        new_bullet.initialize(direction + Vector2(0, bullet_y_spread)).normalized())
        add_child(new_bullet)
        new_bullet.global_position = barrel.global_postion
```

#### Inheritance

```python
# Inherit/extend a globally available class.
extends SomeClass

# Inherit/extend a named class file.
extends "somefile.gd"

# Inherit/extend an inner class in another file.
extends "somefile.gd".SomeInnerClass
```

To check if a given instance inherits from a given class, the is keyword can be used:

```python
# Cache the enemy class.
const Enemy = preload("enemy.gd")

# [...]

# Use 'is' to check inheritance.
if (entity is Enemy):
    entity.apply_damage()
```

#### Super

```python
func some_func(x):
    .some_func(x) # Calls same function on the parent class.
```

#### Inner Classes

A class file can contain inner classes. Inner classes are defined using the class keyword. They are instanced using their **ClassName.new\(\)** function.

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

Read more \[[https://docs.godotengine.org/en/3.1/getting\_started/scripting/gdscript/gdscript\_basics.html?highlight=setget\#built-in-types](https://docs.godotengine.org/en/3.1/getting_started/scripting/gdscript/gdscript_basics.html?highlight=setget#built-in-types)\]

#### Colors

[https://docs.godotengine.org/en/3.1/classes/class\_color.html\#class-color](https://docs.godotengine.org/en/3.1/classes/class_color.html#class-color)

#### Update Function

```python
func _process(delta):
  if condition:
    do something
  else:
    do something else
```

#### Ready

When the node enters the scene tree

```javascript
func _ready():
  screensize = get_viewport_rect().size
```

#### Format Strings

```python
# Define a format string with placeholder '%s'
var format_string = "We're waiting for %s."

# Using the '%' operator, the placeholder is replaced with the desired value
var actual_string = format_string % "Godot"

print(actual_string)
# Output: "We're waiting for Godot."
```

There is also another way to format text in GDScript, namely the String.format\(\) method. It replaces all occurrences of a key in the string with the corresponding value. The method can handle arrays or dictionaries for the key/value pairs.

```python
# Define a format string
var format_string = "We're waiting for {str}"

# Using the 'format' method, replace the 'str' placeholder
var actual_string = format_string.format({"str": "Godot"})

print(actual_string)
# Output: "We're waiting for Godot"
```

```python
var format_string = "%s was reluctant to learn %s, but now he enjoys it."
var actual_string = format_string % ["Estragon", "GDScript"]

print(actual_string)
# Output: "Estragon was reluctant to learn GDScript, but now he enjoys it."
```

#### Clamp

```javascript
clamp() = restrict values in given range
```

#### Randomize

```python
randomize() //this will turn it on
randi() % n, random integer between 0 and n-1
```

#### Range

```python
range(n) # Will go from 0 to n-1
range(b, n) # Will go from b to n-1
range(b, n, s) # Will go from b to n-1, in steps of s
```

#### Hide

```javascript
hide()
```

#### Free

```javascript
queue_free()
```

#### Disable Node

```javascript
$CollisionShape2D.disabled = true/false
```

#### Path2D

#### ColorRect for BG Solid Colors

#### Sound Effects

Use program call _Audacity_ to convert to OGG, WAV **AudioStreamPlayer \(node\)** Audio files: **OGG** for looping music, **WAV** for Sound Effects

ASP.stream = load\("res://Assets/Sounds/BG/loop1.ogg"\) ASP.play\(\)

**Creating AudioStreamPlayer Node from code**

```python
var asp = AudioStreamPlayer.new()

func _ready():
    # set the stream
    asp.stream = load("res://Assets/Sounds/BG/loop1.ogg)
    add_child(asp)
    asp.play()
```

#### Particles

#### Tween

```python
interpolate_property()
tween.start()
tween.is_active()
```

**Tweening Back and Forth**

```python
onready var tween = $Tween

onready var tween_values = [0, 30]

func _ready():
    _start_tween()

func _start_tween():
    tween.interpolate_property($Capsule, "rotation_degrees", tween_values[0], tween_values[1], 1,Tween.TRANS_LINEAR,Tween.EASE_IN_OUT)
    tween.start()
    yield(tween, "tween_completed")
    _on_tween_completed()


func _on_tween_completed():
    tween_values.invert()
    _start_tween()
```

#### VisibilityNotifier

```python
onready var visibility_notifier = get_node("VisibilityNotifier")

func _ready():
    visibility_notifer.connect("screen_entered", self, "_on_visibility_notifier_screen_entered")
```

#### VisibilityEnabler2D

Enable/disable based on screen visibility

#### Animation

**AnimationPlayer \(Node\)**

### SIGNALS

Signals are a way to send notification messages from an object that other objects can listen to in a generic way. Create custom signals for a class using the signal keyword.

```javascript
signal hit
emit_signal("hit")
```

```python
# Signal with no arguments
signal your_signal_name

# Signal with two arguments
signal your_signal_name_with_args(a, b)
```

Here’s an example that creates a custom **signal** in one script and connects the custom **signal** to a method in a separate script, using the _Object.connect\(\)_ method:

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
   owner_of_signal.connection("signal_name", who_is_listening, "who_is_listening_function_name")
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

Signals are generated by the _Object.emit\_signal\(\)_ method which broadcasts the signal and arguments.

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
   owner_of_signal.connection("signal_name", who_is_listening, "who_is_listening_function_name", [owner_of_signal has the data])
```

#### Custom Signals

```python
signal my_signal
signal my_signal_with_arguments(x, y)
```

```python
func _ready():
   emit_signal("my_signal")
   emit_signal("my_signal_with_arguments", 1, 2)
```

```python
# VIA CODE
# CONNECT with options of DEFERRED OR ONESHOT (Connect then disconnect)
object.connect("my_signal", self, "_on_Timer_my_signal", [], CONNECT_DEFERRED | CONNECT_ONESHOT)

# DISCONNECT
disconnect("my_signal", self, "_on_my_signal")
```

```python
# In myCustomObject.tscn
signal my_signal

func emit():
   emit_signal("my_signal")
```

```python
# Create an instance of the so-called myCustomObject.tscn scene.
func _ready():
   var mco = load("res://myCustomObject.tscn").instance()
   mco.connect("my_signal", self, "_on_my_signal")
   add_child(mco)   
   get_child(0).emit()

func _on_my_signal():
   print("signal")
```

#### Listen for Multiple Buttons

```javascript
#Listen for the signal from the buttons
    for button in $LevelsContainer/GridContainer.get_children():
        #passing the button object to the func _on_TextureButton_level_pressed

        button.connect("level_pressed", self, "_on_level_button_pressed", [button])
```

```javascript
func _on_level_button_pressed(level_num, obj):
    pass
```

### YIELD

GDScript offers support for coroutines via the yield built-in function. Calling yield\(\) will immediately return from the current function, with the current frozen state of the same function as the return value. Calling resume on this resulting object will continue execution and return whatever the function returns. Once resumed, the state object becomes invalid. Here is an example:

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

It is also possible to pass values between yield\(\) and resume\(\), for example:

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

#### Yield and Signals

The real strength of using yield is when combined with signals. yield can accept two arguments, an object and a signal. When the signal is received, execution will recommence. Here are some examples:

```python
# Resume execution the next frame.
yield(get_tree(), "idle_frame")

# Resume execution when animation is done playing.
yield(get_node("AnimationPlayer"), "finished")

# Wait 5 seconds, then resume execution.
yield(get_tree().create_timer(5.0), "timeout")

# Creating delay
yield(get_tree().create_timer(2), "timeout")
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

### CALL DEFERRED

Waiting for block to finished then go to next callback

```javascript
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

### PHYSICS BODY

#### PhysicsBody2D Mask

If you don't want the enemies to collide with each other then in the MASK property, uncheck the first box. They won't collide with each other

### Setters/getters

It is often useful to know when a class’ member variable changes for whatever reason. It may also be desired to encapsulate its access in some way.

```javascript
var variable = value setget setterfunc, getterfunc
```

Whenever the value of variable is modified by an `external source` \(i.e. not from local usage in the class\), the setter function \(setterfunc above\) will be called. This happens `before the value is changed`. The setter must decide what to do with the new value. Vice versa, when variable is accessed, `the getter function` \(getterfunc above\) `must return the desired value`.

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

```text

```

### INSPECTOR

#### Exports

This will let the inspector \(GUI\) to change the values of the variables

```python
export (int) var min_speed
export (Array, String) var strings
```

#### Eport Enums as List

```python
enum DROPOFF { linear,square,none }
export(DROPOFF) var dropoff = DROPOFF.linear
```

### Tool for Seeing Changes in the Editor with Changes from Inspector

**When you add at the top of a script the** _**tool**_ **keyword, it will be executed not only during the game, but also in the editor.** Running a script in the editor can be useful for doing many things, but it’s mostly used in level design to show things that would otherwise be visible only during game play.

```javascript
// Skull.gd
tool
extends Node2D

enum STATE { disabled,active }
export(STATE) var state = STATE.active

// preload the textures
var skull_texture = {'disabled': preload("res://assets/ui/skull-disabled.png"),
                    'active': preload("res://assets/ui/skull-active.png")}

func _ready():
    match state:
        1:
            get_node("skull").texture = skull_texture.active

// Need the Engine.editor_hint
func _process(delta):
    if Engine.editor_hint:
        if state == STATE.active:
            get_node("skull").texture = skull_texture.active
        else:
            get_node("skull").texture = skull_texture.disabled
```

#### PackedScene

Choosing the Class/Scene you want to instance from the GUI. Drag Mob.tscn from the left, file system, and drop it in the right under the Script Variables in the inspector.

```python
export (PackedScene) var Mob
```

### INSTANCE OF A CLASS/TSCN

#### GUI

Click on the chain link icon on the left panel under the Scene tab.

### CREATING A CLASS INSIDE GDSCRIPT

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

### SINGLETON / AUTOLOAD

Singleton pattern is useful tool for solving the persistent info between scenes.

#### Concept

* Create objs that are always loaded no matter which snce is running
* Can store global variables
* Can handle switching scenes and between scene transitions
* Act like a singleton

#### Autoload a script to act like a singleton

To autoload a script: Project &gt; Project Settings &gt; Autoload Tab &gt; Select the tscn/gd script

* Add the gd script
* Give it a name
* Check the enabled

```python
Assuming you name it PlayerVariables is the name in the AutoLoad slot
PlayerVariables.health -= 10

Another way if the enabled is not checked
var player_vars = get_node("/root/PlayerVariables")
player_vars.health -= 10
```

#### Creating a custom scene switcher using Autoload

* Create a Global.gd
* Add it to Autoload, Project &gt; Project Settings &gt; AutoLoad Tab
* Autoloaded nodes are always first
* This means that the last child of root is always the loaded scene.
* \`\`\`javascript

  extends Node

var current\_scene = null

func \_ready\(\): var root = get\_tree\(\).get\_root\(\) current\_scene = root.get\_child\(root.get\_child\_count\(\) - 1\)

## Now we need a function for changing the scene. This function needs to free the current scene and replace it with the requested one.

func goto\_scene\(path\):

```text
# This function will usually be called from a signal callback,
# or some other function in the current scene.
# Deleting the current scene at this point is
# a bad idea, because it may still be executing code.
# This will result in a crash or unexpected behavior.

# The solution is to defer the load to a later time, when
# we can be sure that no code from the current scene is running:

call_deferred("_deferred_goto_scene", path)
```

func \_deferred\_goto\_scene\(path\):

```text
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

```text
Using Object.call\_deferred\(\), the second function will only run once all code from the current scene has completed. Thus, the current scene will not be removed while it is still being used \(i.e. its code is still running\).

Finally, we need to fill the empty callback functions in the two scenes:

```javascript
# Add to 'Scene1.gd'.

func _on_Button_pressed():
    Global.goto_scene("res://Scene2.tscn")
```

```javascript
# Add to 'Scene2.gd'.

func _on_Button_pressed():
    Global.goto_scene("res://Scene1.tscn")
```

### Background loading

#### ResourceInteractiveLoader

The ResourceInteractiveLoader class allows you to load a resource in stages. Every time the method poll is called, a new stage is loaded, and control is returned to the caller. Each stage is generally a sub-resource that is loaded by the main resource. For example, if you’re loading a scene that loads 10 images, each image will be one stage.

#### Usage

\[[http://docs.godotengine.org/en/3.1/tutorials/io/background\_loading.html\#doc-background-loading](http://docs.godotengine.org/en/3.1/tutorials/io/background_loading.html#doc-background-loading)\] \(More info\)

### HUD / CanvasLayer/ Sits above the Game / UI

#### Control Nodes for UI

* Buttons
* Labels
* TextureRect
* TextureProgress
* TextureButtons

#### Properties of Control Nodes

* Positions
* Size
* Anchors
* Margins

