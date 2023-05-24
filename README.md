
# Skript-AnvilGUI

A Skript Addon that allows users to create Anvil GUIs to read user input.
The skript works with Paper/Spigot 1.12+ and requires Skript 2.6+

# Tutorial

An in-depth tutorial written by me can be found on SkriptHub: <TO BE DONE>.
	
[![SkriptHubViewTheDocs](http://skripthub.net/static/addon/ViewTheDocsButton.png)](http://skripthub.net/docs/?addon=Skript-AnvilGUI)


[![SkriptHubViewTheDocs](http://skripthub.net/static/addon/ViewTheDocsButton.png)](http://skripthub.net/docs/?addon=Skript-AnvilGUI)


# Documentation

## Effects

### Open Anvil GUI
```
open (anvilgui|anvil gui) %anvil% to %players%
```

Open an Anvil GUI to a player. Note that once the AnvilGUI is opened to a player, you can't change it "live" while it's being shown.
Therefore you first must set the slots of the GUI, and then open it to the player.

<details>
	<summary>Open Anvil GUI</summary>

        ```
            command /openanvil:
                trigger:
                    set {_gui} to a new anvil gui
                    #set here slots and title or text before opening it.
                    #check expressions and examples below to learn how.
                    open anvil gui {_gui} to player
        ```
</details>

## Types

### Anvil GUI
```
%anvil%
anvil gui
event-anvil gui
```

This type holds all the information about an Anvil GUI.
You can create a new object with the new anvil gui expression and edit its content (such as the items) with the specific expressions.

<details>
	<summary>Anvil GUI</summary>

		```
            on anvil gui click:
                set title of event-anvil gui to event-text
                open anvil gui event-anvil gui to player
                #this will move the text in the anvil gui input to the name of the inventory
		```

</details>

## Events

### Anvil GUI Close
```
on anvil gui close
```

Fires whenever an anvil gui created with this addon is closed. You can access the following values from this event:
event-text (content of the GUI when the GUI was closed), event-anvil gui (the original anvil object. You can access items in the anvil GUI or its inventory title with expressions below.)

<details>
	<summary>Uncloseable AnvilGUI</summary>

        ```
            on anvil gui close:
                if title of event-anvil gui is "You can't close this unless you type 1234":
                    if event-text is not "1234":
                        cancel event
		```
</details>

### Anvil GUI Click
```
on anvil gui click
```

Fires whenever an anvil gui created with this addon is clicked. You can access the following values from this event:
event-integer (index of slot that was clicked), event-text (content of the GUI when the GUI was clicked),
event-anvil gui (the original anvil object. You can access items in the anvil GUI or its inventory title with expressions below.)

<details>
	<summary>Anvil GUI Password</summary>

        ```
		    command /password:
                trigger:
                    set {_gui} to a new anvil gui named "&0Insert password" with default text "password"
                    open anvil gui {_gui} to player

            on anvil gui click:
                if event-integer is 2: #clicked the output item slot
                    if title of event-anvil gui is "&0Insert password":
                        if event-text is "1234":
                            close player's inventory
                            send message "&aCorrect password."
                        else:
                            set text of event-anvil gui to "Wrong password"
                            open anvil gui event-anvil gui to player #reopen the anvil gui, but with a different text.
		```
</details>

## Expressions

### New Anvil GUI
```
new anvil gui
[a] new anvil gui (named|with title) %string% with [default] text %string%
```

Returns a new anvil gui. If no title and default text are specified, it will use some placeholder text.

### Anvil GUI TItle
```
[the] title of %anvil%
```

Access the title (inventory name) of an Anvil GUI. In 1.12.2, the name of the AnvilGUI will not change when displayed.
This is not really an issue (other than for aesthetic) as the title is still saved and can be accessed (to check for specific anvil GUIs in events).

### Anvil GUI Text
```
text of %anvil%
```

Access the default text of an AnvilGUI. This will be filled when the anvil is opened to a player.

### Anvil GUI Item
```
(left|right|output) (item|slot) of %anvil%
```

Access a specific item in the anvil GUI. You can set these to any itemstack. Please note that the server wipes and overwrites
the items in the output GUI when opened, so as of right now only changing the left and right item works.

<details>
	<summary>Anvil GUI Set Item</summary>

	```
        command /anvilgui:
            trigger:
                set {_gui} to a new anvil gui
                set left item of {_gui} to barrier
                set right item of {_gui} to emerald block
                set output item of {_gui} to barrier #this might be erased by the server, you can't do nothing about it.
                open anvil gui {_gui} to player
	```
</details>

## FAQ

#### Output items in the GUI are not being set

This is a known issue with the original AnvilGUI library. Until they find a workaround, I can't do much on this addon.


