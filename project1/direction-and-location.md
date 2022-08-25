# The Location and Direction classes

The Location class represents a single (row, column) location on the 2-d board, whereas the Direction class represents one of the eight basic cardinal directions (north, south, east, west, northeast, southeast, southwest, northwest).

Here's a little bit more about what you can do with these classes:

## Direction

The Direction class is actually an `enum` with stands for "enumerated type," which just means that this class can only take on a restricted range of values, in this case, one of the eight directions.  Other than that, Direction acts like a normal class.

Here's what you can do with a Direction object.  All of these methods, except `fromDegrees`, must be called on an existing Direction object.

- Create a new Direction object:
  `Direction fromDegrees(int degrees)`
  Use this static method to create a new Direction object.  0 degrees is directly north (up), 90 degrees is directly east (right) and so on.
  Example: `Direction eastDirection = Direction.fromDegrees(90);`
- Create a new direction by rotating an existing one:
  `Direction rotateClockwise(int degrees)` and
  `Direction rotateCounterClockwise(int degrees)`.  
  Use these methods on a Direction object to get a new Direction based on a rotation of the existing one.
- Return the Direction as a number of degrees, 0-359:
  `int getDegrees()`

## Location

Here's what you can do with a Location object.  All of these methods, except the constructor, must be called on an existing Location object.

- Construct a new Location object:
  `Location(int r, int c)`
  You will probably rarely have to use this, since most Locations are created from existing Locations. 
- Get the row and column of a Location:
  `int getRow(), int getCol()` 
- Get a Location adjacent to this one:
  `Location getAdjacentLocation(Direction d)`
- Get the Direction from one Location towards a second Location:
  `Direction getDirectionToward(Location target)`

## An example of using Directions and Locations

```java
Location loc1 = new Location(1, 2);
Location loc2 = new Location(3, 2);
Direction eastDirection = Direction.fromDegrees(90);
Location loc3 = loc1.getAdjacentLocation(eastDirection);  // loc3 is now (1, 3)
Direction southDirection = eastDirection.rotateClockwise(90);
Location loc4 = loc2.getAdjacentLocation(southDirection); // loc4 is now (4, 2)
```

