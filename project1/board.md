# The Board Class

The board class represents a 2 dimensional grid or board, on which different objects can reside.  Each cell of the board is represented by a Location object, which stores the row and column of that cell.  Each cell can hold at most one object.

Here are the most common methods you will use on a Board object.

- Get the dimensions of a board:
  `int getNumRows(), int getNumCols()`
  These methods return the number of rows and columns on the board.
- `boolean isValid(Location loc)`
  Returns true if Location `loc` is "valid", meaning part of this particular board.  In other words, any location where the row and/or column are too big for the board returns false.
- `ArrayList<Location> getOccupiedLocations()`
  Returns an ArrayList of all occupied locations on the board.
- `T get(Location loc)`
  Returns the object at the specified location on the board, or null if there is no object there.  The `T` here represents the type of object on the board.  This is the same type parameter used when the Board object is first declared.  For these projects, `T` is almost always `Actor` or a subclass.
- `ArrayList<Location> getValidAdjacentLocations(Location loc) `
  Return an ArrayList of all the *valid* Locations that are adjacent to this one.  For a Location in the middle of the board, there are 8 adjacent Locations, but for Locations along an edge, there are only 5 adjacent Locations, and corners only have 3.
- `ArrayList<Location> getEmptyAdjacentLocations(Location loc)`
  Return an ArrayList of all the valid, adjacent Locations to this one that happen to be empty.
- `ArrayList<Location> getOccupiedAdjacentLocations(Location loc)`
  Return an ArrayList of all the valid, adjacent Locations to this one that happen to contain an object.
- `ArrayList<T> getNeighbors(Location loc)`
  Return an ArrayList of all the *contents* of each valid, adjacent Location to this one.  This list might be empty if there are no objects surrounding this one.