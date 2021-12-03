---
Title: Planning for drunks
---
# Planning for Drunks Agent Based Model

## **[View this project on GitHub](https://github.com/tmcunningham/planning-for-drunks)**

This project simulates drunks leaving a pub and moving around an environment until they find their home. There are 25 drunks and each has an assigned house. Initially, the drunks cannot remember where they live and move randomly, but they gradually sober up and eventually start moving towards their homes.

Practical information on how to run the model can be found in the README on GitHub - this page covers the intentions of the project and the development process in slightly more detail.

## Requirements

This project was completed as an assignment for the Programming for Social Science course taught by the University of Leeds in 2021. The core requirement for this assignment was to build a programme that would do the following:
- Pull in the data file and finds out the pub point and the home points.
- Draws the pub and homes on the screen.
- Models the drunks leaving their pub and reaching their homes, and stores how many drunks pass through each point on the map.
- Draws the density of drunks passing through each point on a map.
- Saves the density map to a file as text.

## Significant improvements

Aside from the basic assignment requirements, I made two major additions to the programme:

### 1. Drunks will sober up and eventually remember where their home is

Drunks have a ```drunk_level``` attribute, which determines "how drunk" they are. The drunk level decreases when they visit locations they have previously been to (the idea being that recognising a place helps them to sober up). When the ```drunk_level``` drops to a certain level, the drunk's speed increases. When the drunk completely sobers up (i.e. ```drunk_level = 0```), then the drunk will not move randomly and instead move in the direction of their house.

### 2. Drunks cannot move through buildings

To make the simulation more realistic, I stopped the drunks being able to walk through buildings. Instead, when they are at the edge of a building, they will move along the edge of it in a random direction. This was surprisingly challenging to implement.

## Development process



### Stopping drunks moving through buildings

By far the most difficult part of developing the programme was stopping the drunks from walking through buildings. Initially I attempted to use a while loop in the ```Drunk.move()``` method, as in the following code:

```python
# Infinite loop that will break when new co-ordinates are not in a building
while True:
    if self.drunk_level > 0:
        # Call random once so using the same random number
	random_num = random.random()
	closest_home_y = new_y
	closest_home_x = new_x

    else:
	# If drunk level == 0 set closest home coords
	closest_home_y = min([t[1] for t in self.home_coords],
			     key = lambda y: abs(y - new_y))   
	closest_home_x = min([t[0] for t in self.home_coords],
			     key = lambda x: abs(x - new_x))
	random_num = 1

    if random_num < 0.25 or closest_home_y > new_y:
        new_y = (self.y + self.speed) % len(self.town)
    elif random_num < 0.5 or closest_home_y < new_y:
        new_y = (self.y - self.speed) % len(self.town)
    elif random_num < 0.75 or closest_home_x > new_x:
        new_x = (self.x + self.speed) % len(self.town[0])
    else:
        new_x = (self.x - self.speed) % len(self.town[0])                


    # Update x and y if they are not building co-ordinates
    if (new_x, new_y) not in self.other_building_coords:
        break

(self.x, self.y) = new_x, new_y
```

However, this caused the programme to crash and I could not seem to make it work using a while loop.

## Testing

I created a separate test module, test_drunk_functions.py, for testing the model's functions and methods of the drunk class. While I created most of these tests after the functions (and I had already done some ad-hoc testing of the functions), it was helpful to know that the functions worked in the way I expected. the tests also helped me spot some minor issues - for exmaple, a string printed by the ```drunk_functions.gen_function``` function had douple spaces which meant it failed the ```TestGenFunction.test_past_iterations``` test.
