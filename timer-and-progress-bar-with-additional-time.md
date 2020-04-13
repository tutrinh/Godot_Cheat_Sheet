# Timer and Progress Bar with Additional Time

Call \_process every 1 second using delta

```text
extends ProgressBar

# class member variables go here, for example:
# var a = 2
# var b = "textvar"


var timer = Timer.new()
export (float, 0.0, 100.0, .1) var start_time = 15.0
export (float, 0.0, 100.0, .1) var end_time   = 20.0
var t = 1
const ADDITIONAL_TIME = 3.0

func _ready():
    self.value = 20
    # Called when the node is added to the scene for the first time.
    # Initialization here
    timer.set_timer_process_mode(Timer.TIMER_PROCESS_IDLE)
    timer.set_wait_time(end_time - start_time)
    timer.set_one_shot(true)
    timer.connect("timeout", self, "gameover")
    add_child(timer)
    timer.start()
    set_process(true)
    
func _process(delta): # Print testing

    if(t >= 1):
        t -= 1
        print(timer.get_time_left())

    t += delta

#func _process(delta):
#    # Called every frame. Delta is time since last frame.
#    # Update game logic here.
#    pass
func gameover():
    print("GAME OVER - TIME RAN OUT")
    set_process(false)
    
func _on_Button_pressed(): # Button for testing

    # Must get time before stopping timer.
    var new_time = timer.get_time_left() + ADDITIONAL_TIME

    timer.stop()
    timer.set_wait_time(new_time)
    timer.start()
```

