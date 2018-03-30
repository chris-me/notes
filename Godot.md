- [Scripting](#scripting)
- [Snippets](#snippets)

## Scripting

### Lifecycle Methods

http://docs.godotengine.org/en/3.0/getting_started/step_by_step/scripting_continued.html#overrideable-functions

```python
func _enter_tree():
    # When the node enters the _Scene Tree_, it becomes active
    # and  this function is called. Children nodes have not entered
    # the active scene yet. In general, it's better to use _ready()
    # for most cases.
    pass

func _ready():
    # This function is called after _enter_tree, but it ensures
    # that all children nodes have also entered the _Scene Tree_,
    # and became active.
    pass

func _exit_tree():
    # When the node exits the _Scene Tree_, this function is called.
    # Children nodes have all exited the _Scene Tree_ at this point
    # and all became inactive.
    pass

func _process(delta):
    # This function is called every frame.
    pass

func _physics_process(delta):
    # This is called every physics frame.
    pass
```

## Snippets

### FPS Counter Label

```python
extends Label

var second_counter = 0
var frame_counter = 0
var fps = 0

func _process(delta):
	second_counter += delta
	frame_counter += 1
	if(second_counter >= 1):
		fps = frame_counter
		second_counter = 0
		frame_counter = 0
	text = str(fps)
```
