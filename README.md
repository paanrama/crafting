# crafting

Adds semi-realistic crafting to minetest, and removes the craft grid.

By rubenwardy  
License: LGPLv2.1+

## Limitations

Any recipes must be designed such that any particular item can only be used
as one of the ingredients.

For example, you can't have the following recipe as `default:wood` could
be used twice: `default:wood, group:wood`.

## API

* crafting.register_type(name)
	* Register a type `type` used when searching for recipes

* crafting.register_recipe(def)
	* Returns id.
	* `def` a table with the following fields:
		* `type`   - one of the registered types.
		* `output` - the result of the craft, eg: `default:stone 3`.
		* `items`  - A list of ingredients, eg: `{"stone", "wood 3"}`.
		* `always_known` - If true, this recipe will never need to be unlocked.

* crafting.get_recipe(id)
	* Get recipe by ID

* crafting.get_all_for_player(player, type)
	* Returns a list of results, each a table
		* `items` - a key-value table, key being item name and value being a table:
			* `have` - how many the player has
			* `need` - how many of this item needed
		* `recipe` - the recipe
		* `craftable` - is craftable?		
	* `craftable` is a table of the items the player can craft with the items they have.
	* `uncraftable` is a table of items the player knows about, but is missing items for.

* crafting.get_all(type, item_hash, unlocked)
	* Returns same as above.
	* `item_hash` - a table with keys being item names or group names (eg: `group:wood`)
	                and the value being the number required.
	* `unlocked`  - a list of outputs the player has unlocked.

* crafting.find_required_items(inv, listname, recipe)
	* Returns a list of stacks to take, or nil if the required items could not
	  be found.

* crafting.has_required_items(inv, listname, recipe)
	* Returns true if the `"main"` list in `inv` contains the required items.

* crafting.perform_craft(inv, listname, recipe)
	* Will try to take items and put output in the `"main"` list in `inv`.
	* Returns true on success.

* crafting.make_result_selector(player, type, size, context)
	* Generates a paginated form which a search box.
	* `type`    - craft type.
	* `size`    - how many slots to show on a page.
	* `context` - server-side storage between show and submit, can be `{}`.
		* `crafting_page` - page to show

* crafting.result_select_on_receive_results(player, type, context, fields)
	* Handles form submissions for the result selector.
	* Returns true if the formspec should be shown again.
