# Saving-Object-States-Between-Rooms
Implemented in GameMaker Studio

This tutorial shows one way to save the variables of objects in a room so that when you come back to the room, you can initialize all of the objects with their previous variables.

Check out the code in action here: http://pennpierson.com/gm_save_states_between_rooms/

---

I made this tutorial in response to [this question on reddit](https://www.reddit.com/r/gamemaker/comments/6h989t/saving_object_states/).

"I am making a platformer and I would like to save the states of creatures when I leave the room. What would be the best way to do this? I'm assuming some sort of ds list but I'm only beginning to learn and understand how they function. Would this be the optimal route?"

The answer is that it depends on what you want to do, but if you have an unknown amount of creatures for each room, I would use a 2d-array in combination with a ds_map, structured like this.

Array:

	with (all creatures):

		creatures_arr = [creature_n, "x"] = x_coordinate;

		creatures_arr = [creature_n, "y"] = x_coordinate;

		creatures_arr = [creature_n, "variable name 1"] = variable1;

		creatures_arr = [creature_n, "variable name 2"] = variable2;

		etc ...

You'll need to cycle through all of your creatures (creature_n) and add all the data you want into the array. Then create a ds_map (key and value pairs), where the key is the room's name and the value is the array data.

DS_Map:

	creatures_map[? room_name] = creatures_arr;
	
Create the ds_map at Room End Event and then during the Room Start Event, check to see if there's any data for the current room. If there is any data, delete all of the creatures in the room and then create new ones with the variables you have stored in the ds_map.

---

I've implemented all of this in an example project that you can download.