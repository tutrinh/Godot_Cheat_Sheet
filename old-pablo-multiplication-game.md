# Old Pablo Multiplication Game

## Structure

Singleton -  Global

```text
global.gd

hold variables where be accessibile from every where
load and save progress
load and save data

```

## Data directory

hold the json files, data, settings

## Scenes

* Game.gd
* Gameover.gd
* Levels.gd
* Main-menu.gd
* Wrong.gd

## Variables

```text
const ADDITIONAL_TIME = 3.0
var countdown = 30
```

## Preload Textures

```text
var texture01 = preload("res://assets/symbols/large-card-red.png") 
var texture02 = preload("res://assets/symbols/large-card-blue.png")
```

## \_init

```text
func _init():
	# Called when the node is added to the scene for the first time.
	# Initialization here
	pass
```

## \_ready

```text
func _ready():
	# Called when the node is added to the scene for the first time.
	# Initialization here
	pass
```

## Update every frame

```text
set_process(true) #Enable Update function

func _process(delta):
# Called every frame. Delta is time since last frame.
# Update game logic here.
    pass
```

## Tween

```text
onready var tween = Tween.new()

func _ready():
	# Called when the node is added to the scene for the first time.
	# Initialization here
	add_child(tween
	tween.connect("tween_completed", self, "tween_completed")
```

## Timer

```text
var timer = Timer.new()

func _ready():
	
	self.value = current_count
	# Called when the node is added to the scene for the first time.
	# Initialization here
	#timer.set_timer_process_mode(Timer.TIMER_PROCESS_IDLE)
	timer.set_wait_time(countdown)
	timer.set_one_shot(true)
	timer.connect("timeout", self, "gameover")
	add_child(timer)
	
	func start_game_timer():
	set_process(true) #Enable Update function
	timer.start()
```

## Export for Inspector to change value

```text
export (bool) var active = true
export(String) var scene_to_load
```

## Signal

```text
signal level_pressed(level_num)
signal timer_over
signal progress_timer_flash(time) // with parameters

//with arguments
emit_signal("progress_timer_flash", countdown)

```

## Enum - List

```text
enum {ACTIVE, DEACTIVE}
```

