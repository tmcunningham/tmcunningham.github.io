---
Title: Planning for drunks
---
# Planning for Drunks Agent Based Model

## **[View this project on GitHub](https://github.com/tmcunningham/planning-for-drunks)**

This project simulates drunks leaving a pub and moving randomly around an environment until they find their home. There are 25 drunks and each has an assigned house.

## Significant improvements

Aside from the basic assignment requirements (to have the drunks move randomly until they found their home), I made two major additions to the programme:

### 1. Drunks will sober up and eventually remember where their home is

Drunks have a ```drunk_level``` attribute, which determines "how drunk" they are. The drunk level decreases when they visit locations they have previously been to (the idea being that recognising a place helps them to sober up). When the ```drunk_level``` drops to a certain level, the drunk's speed increases. When the drunk completely sobers up (i.e. ```drunk_level = 0```), then the drunk will not move randomly and instead move in the direction of their house.

### 2. Drunks cannot move through buildings

To make the simulation more realistic, I stopped the drunks being able to walk through buildings. Instead, when they are at the edge of a building, they will randomly move along it.

## Development issues

By far the most difficult part of developing the programme was stopping the drunks from walking through buildings. Initially I attempted to use a while loop in the ```Drunk.move()``` method, as in the following code:

```python
def move(self):
        # If not already at home, move up down left or right at random
        if not self.is_home:
            (new_x, new_y) = self.x, self.y
		
		# Infinite loop that will break when new co-ordinates are not in a building
		while True:
                if self.drunk_level > 0:
                    # Call random once so using the same random number - noticed a bug
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
                # This is currently really slowing everything down

                if (new_x, new_y) not in self.other_building_coords:
                    break

            (self.x, self.y) = new_x, new_y


            # Add one to environment to show route taken
            self.town[self.y][self.x] += 1
            # If drunk doesn't move, will still add
            self.town[self.y][self.x] += 3

            # If reached one of home coordinates, set to be at home
            if (self.x, self.y) in self.home_coords:
                self.is_home = True
```

However, this proved to be far too computationally intensive and caused the programme to crash.