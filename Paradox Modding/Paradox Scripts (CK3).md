Basic Elements
Scopes
Triggers & Effects
Trigger-Effect Grammar
Trigger Logic
Modifier
Weight modifiers
Affecting modifiers
Activities
Decisions
Character Interactions
On_action
Events
Character Events
Letter Events
Variables & Variable Lists
# Basic Elements
## Scopes
[https://ck3.paradoxwikis.com/Scopes](https://ck3.paradoxwikis.com/Scopes)
Access by relations `mother.court_owner.culture`
Access by groups `culture:english`
  
Global Scopes `this prev root`
Define Scopes `save_scope_as=[scopename]` define `scope:scopename` to be the current scope
  
## Triggers & Effects
[https://ck3.paradoxwikis.com/Triggers](https://ck3.paradoxwikis.com/Triggers)
[https://ck3.paradoxwikis.com/Effects](https://ck3.paradoxwikis.com/Effects)
### Trigger-Effect Grammar
```Lua
if = { limit = { <triggers> } <effects> }
else_if = { limit = { <triggers> } <effects> }
else = { <effects> }
random_<scope> = {limit = { <triggers> } <effects>}
every_<scope> = {limit = { <triggers> } <effects>}
while = { # Repeats until set iteration count is reached
    limit = { <triggers> }
    <effects>
}
while = { count = 3 <effects> } # Default max of 1000.
switch = {
	trigger = simple_assign_trigger
	case_1 = { <effects> }
	case_2 = { <effects> }
	case_n = { <effects> }
	fallback = { <effects> }
}
hidden_effect = { <effects> }
```
### Trigger Logic
```Lua
any_vassal = {
	has_trait = brave
	count = all # default 1
}
OR = {
	gold > 200
	AND = {
		has_trait = brave
		has_trait = arrgant
	}
}
# also NOT NAND NOR
```
## Modifier
### Weight modifiers
modifiers that change weights on game choice

> appear in `weight` (random_), `ai_chance` (events) , `ai_accept` and `ai_will_do` (decisions) blocks
```Lua
weight = {
	base = 100
	modifier = {
		has_trait = brave
		add = 100
	}
	modifier = {
		has_trait = zealous
		add = -50
	}
	modifier = {
		has_trait = craven
		multiply = 0
	}
}
```
### Affecting modifiers
Modifiers that affect scopes over time
[https://ck3.paradoxwikis.com/Modifier_list](https://ck3.paradoxwikis.com/Modifier_list)
# Activities
## Decisions
```Lua
# Display
desc
major
is_shown
is_valid
# AI
ai_potential
ai_check_interval
ai_will_do
# Effect
cost
effect
```
## Character Interactions
```Lua
# Display
desc
is_shown
can_send
common_interaction
interface_priority
is_highlighted
highlighted_reason
# AI
ai_accept # or auto_accept = yes
ai_frequency
ai_will_do
# Effect
on_accept = {
	scope:actor = {}
}
on_decline = {
	scope:recipient = {}
}
popup_on_receive = yes/no
scheme = slope/murder/...
pause_on_receive = yes/no
```
## On_action
```Lua
trigger
effect
events
on_actions
```
## Events
[https://ck3.paradoxwikis.com/Event_modding](https://ck3.paradoxwikis.com/Event_modding)
### Character Events
```Lua
namespace = example_events
# <namespace>.<eventid>
example_events.1001 = {
	type = character_event
	title = example_events.1001.t
	desc = example_events.1001.desc
	theme = realm
	Left_portrait = {
		character = <character scope>
		animation = bold
	}
	# right_portrait lower_left_portrait lower_center_portrait lower_right_portrait
	immediate = { <effects> }
	<triggers>
	option = { 
		trigger= <triggers>
		ai_chance = <weight modifier>
		name = example_events.1001.a
		<effects>
	}
	option = {
		trigger = <triggers>
		ai_chance = <weight modifier>
		name = example_events.1001.b
		<effects>
	}
	after = {}
}
```
### Letter Events
```Lua
example_events.1002 = {
	type = letter_event
	opening = {
		desc = example_events.1002.opening
	}
	desc = example_events.1002.desc
	sender = {
		character = <character scope>
		animation = bold
	}
	immediate = { <effects> }
	<triggers>
	option = { 
		trigger = <triggers>
		ai_chance = <weight modifier>
		name = example_events.1001.a
		<effects>
	}
	option = {
		trigger = <triggers>
		ai_chance = <weight modifier>
		name = example_events.1001.b
		<effects>
	}
	after = {}
}
```
# Variables & Variable Lists
```Lua
## Local scope
set_variable/change_variable  = {
	name = <varname>
	value = 
}
# access
var:<varname>
# remove
remove_variable = <varname>
## Global
set_global_variable
change_global_variable
remove_global_variable
global_var:globalvarname
```
```Lua
## Local scope
add_to_variable_list/remove_list_variable  = {
	name = <varname>
	target = <scope>
}
random_in_list/every_in_list/any_in_list  = {
	variable = <varname>
	<effects>
}
## Global
add_to_global_variable_list
random_in_global_list
every_in_global_list
any_in_global_list
remove_list_global_variable
```