# Saving-Object-States-Between-Rooms
Implemented in GameMaker Studio

Created by Josh Penn-Pierson

You do not have to give credit if you use this code in your game.

---
Explanation
-

This tutorial shows two ways to save the variables of objects in rooms so when you come back to a room you've been to before, you can initialize all of the objects with their previous variables.

I made this tutorial in response to [this question on reddit](https://www.reddit.com/r/gamemaker/comments/6h989t/saving_object_states/).

>"I am making a platformer and I would like to save the states of creatures when I leave the room. What would be the best way to do this? I'm assuming some sort of ds list but I'm only beginning to learn and understand how they function. Would this be the optimal route?"

The answer is that it depends on what you want to do. I've implemented two different ways of storing the data here.

**Version 2:** Version 2 is much more flexible (with the save and load data on each object, you can specify exactly what variables to save/load), and should be used in most cases.

**Version 1:** The only time you would use Version 1 is if you knew that all of your objects were going to have all of the same variables and you wanted to keep all of the save/load code in one place rather than spread out on each of the objects.

I've included a **template for version 2** that only has relevant code (instead of an tutorial/example) if you would like to use that to build off of.

---
Tutorial for **Save States Between Rooms 2.gmx** (Version 2)
-

Check out the code in action here: http://pennpierson.com/gm_save_states_between_rooms_2/

This version uses a 1D array inside of a 2D array inside of a ds_map, structured like this:

1D Array

    creature_data[0] = some_variable;
    creature_data[1] = another_variable;
    creature_data[2] = some_other_variable;
    // etc...

2D Array

	creatures_arr = [creature_n, "type"] = object_type;
	creatures_arr = [creature_n, "data"] = creature_n_data;
	// Repeat for all creatures (creature_n, creature_n_data)

DS_Map:

	creatures_map[? room_name] = creatures_arr;


In this version, I've redone how the data is saved and loaded. Instead of the controller object deciding which data to save and load, the saving/loading is left up to each object to define. Here's how it works

1. At the end of the level, the controller object triggers the **<user defined event 1>** in each object. In this event, the object gathers all the data it wants to save into a **1D array**. The controller object then grabs the data and adds the **object type** (index [n, 0]) and the **1D array** (index [n, 1]) into a **2D array**. From there, it adds the **2D array** into a **ds_map** (key : value ;; room_name : 2D array).
2. At the start of a level, the controller object extracts data for the **current room** (if there is any) from the **ds_map**. The data is the **2D array**. The controller cycles through all of the data in the **2D array**. For each index, it creates an **object** (type of object is index[n, 0]) at **position** (0,0), and instantiates the object with **data** (index[n, 1]). This data is the **1D array** that the object built when the level had previously ended. Then the controller triggers the **<user defined event 0>**. In this event, the object assigns its variables to the data stored in the **1D array**.

---
Using Version 2 in your game
-

If you want to use version 2 in your game, download the version 2 template and copy the code over (either straight into the events or into scripts and then call the scripts in the events).

The only areas you will need to worry about editing are these:

**obj_controller Create Event**

	objects_to_save = par_save_data; // These are the objects you want to save data for

If you change the parent object that you use to determine which objects get saved, you'll have to change this code to set objects_to_save to that different parent object.

**obj_creature User Event 1**

    // <add your variables here>
    
    // Examples:
    // my_data[0] = variable_a;
    // my_data[1] = variable_b;
    // my_data[2] = variable_c;
    // ...
    // Keep adding as many variables as you want.
    // Make sure they all have a unique index in my_data.
    // (for every my_data[n], make n unique).
    // Also, make sure that the values in User Defined 0 Event match these.

You need to add all of the variables here (into the 1D array) that you want to get saved for this object.

**obj_creature User Event 0**

    // <add your variables here (must correspond to values in User Defined Event 1)>
    // Examples:
    // variable_a = my_data[0];
    // variable_b = my_data[1];
    // variable_c = my_data[2];
    // ...

You need copy the values from User Event 1 here so that all of your variables get loaded correctly.


---
Tutorial for **Save States Between Rooms.gmx** (Version 1)
-

Check out the code in action here: http://pennpierson.com/gm_save_states_between_rooms/

This version uses 2D array inside of a ds_map, structured like this.

2D Array

	creatures_arr = [creature_n, "type"] = object_type;
	creatures_arr = [creature_n, "x"] = x_coordinate;
	creatures_arr = [creature_n, "y"] = x_coordinate;
	creatures_arr = [creature_n, "variable name 1"] = variable1;
	creatures_arr = [creature_n, "variable name 2"] = variable2;
	// etc... (continue for all variables)
	// Repeat for all creatures (creature_n)

DS_Map:

	creatures_map[? room_name] = creatures_arr;

In order to save and load the data, create the ds_map at Room End Event and then during the Room Start Event, check to see if there's any data for the current room. If there is any data, delete all of the creatures in the room and then create new ones with the variables you have stored in the ds_map.

The downside of this version is that all of the objects you are saving data for have to have all of the same variables.