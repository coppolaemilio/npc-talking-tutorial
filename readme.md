A very common use of dialogues is when speaking to an NPC. There are many ways of achieving this, but here you have a simple one to follow step by step:

# Character movement.
Let's create a character and add basic movement to it:

1. Make the KinematicBody2D
2. Add a Sprite to it
3. Add a collision shape
4. Add a script:

```gdscript
extends KinematicBody2D

export (int) var speed = 200

var velocity = Vector2()

func get_input():
    velocity = Vector2()
    if Input.is_action_pressed("ui_right"):
        velocity.x += 1
    if Input.is_action_pressed("ui_left"):
        velocity.x -= 1
    if Input.is_action_pressed("ui_down"):
        velocity.y += 1
    if Input.is_action_pressed("ui_up"):
        velocity.y -= 1
    velocity = velocity.normalized() * speed

func _physics_process(delta):
    get_input()
    velocity = move_and_slide(velocity)
```

# NPC

1. Create an Area2D node
2. Add a collision shape with the range for your dialog to be able to trigger
3. Add a Sprite
4. Add an indicator for when the Player can talk with the NPC
5. Add a script to the Area2D node

```gdscript
extends Area2D

var active = false

func _ready():
	connect("body_entered", self, '_on_NPC_body_entered')
	connect("body_exited", self, '_on_NPC_body_exited')


func _process(delta):
	$QuestionMark.visible = active


func _input(event):
	if get_node_or_null('DialogNode') == null:
		if event.is_action_pressed("ui_accept") and active:
			var dialog = Dialogic.start('timeline-1')
			add_child(dialog)


func _on_NPC_body_entered(body):
	if body.name == 'Player':
		active = true


func _on_NPC_body_exited(body):
	if body.name == 'Player':
		active = false
```

And that's it!