# Project 1: SquareWorld

Welcome to SquareWorld! In this project you will brush up on your Java skills to explore a world filled with animals.

## Getting Started

1. Download the starter code [here](https://rhodes.box.com/s/hkagopfc6owxj4trd5pniv8ikjrmpykd).  This zip file will unzip into a `src` folder. 
2. Make a new Intellij project.  Then copy the contents of this `src` folder exactly into the `src` folder in the new project.
3. There is a lot of code here, but most of it you can ignore.  To verify everything is working correctly, find the `BugDemo.java` file and run it.  You should see a window open.
4. Press the STEP button to watch the world unfold.  You can press the RUN button to watch it continuously.  You can drag the slider to slow it down or speed it up.

## Understanding the code
1. In SquareWorld, you design various objects that all reside in a large grid or board.  Each time you press STEP, the world moves forward one time unit, and all the objects in the world may take some action.  Actions may involve turning, moving, staying put, teleporting, disappearing, essentially anything they want.
2. First, we will explore the Direction and Location classes.  First, read through this [short summary of the classes](direction-and-location.md). Next, open `Location.java` and read through it.  Notice how it's nothing more than a class to represent a (row, column) location in the SquareWorld.  It has some functions to access other locations around it, such as via `getAdjacentLocation()`.  It also makes reference to the `Direction` class, which we'll look at next.
3. Open `Direction.java`.  Notice how it is a class that represents one of eight cardinal directions.  (An `enum` is a Java class that only has a fixed number of object instances that can be created.  Here they're known as `NORTH`, `NORTHEAST`, `EAST`, etc.)  `Direction` has some nice methods to get a `Direction` from a number of degrees, turn a `Direction` into degrees, etc.  Notice that zero degrees is straight north, 90 degrees is straight east, etc.  
4. Next we'll explore the Board class; read through [this documentation for it](board.md).  Open `Board.java`.  The `Board` class represents the SquareWorld board, where all the objects live and move around.  Skim through this class.  **You don't have to understand all the code here**; it's more important to get a sense of what functions are available to you.  
1. We will explore the `Actor`, `Rock`, `Flower`, `FancyFlower`, and `Bug` classes next.  Open `Actor.java`.  (Look inside the `actor` folder.)
The `Actor` class is the superclass for all objects in SquareWorld that live on the board.  Notice how `Actor`s have three fields: the board in which they live, their location within the board, and the direction they're facing. 
6. The most important method in `Actor` is `act()`.  Notice how `act()` is abstract, which means all subclasses of `Actor` should override `act()`.  The `act()` function is automatically called when the STEP button is clicked, and is called over and over when RUN is clicked. 
7. Additionally, `Actor`s can get the direction that they are facing (`getDirection()`), add themselves to a Board (`addSelfToBoard()`), remove themselves from a Board (`removeSelfFromBoard()`), as well as a few other things.

## Exploring various kinds of Actors

### Rocks and Flowers

- Open up `Rock.java` and `Flower.java`.  Notice how  `Rock` overrides `act()` but does nothing (except print some debugging information), and how  `Flower` overrides `act()` to turn 45 degrees when `act()` is called (it also prints some debugging information).
- Now open `RockAndFlowerDemo.java` and run it.  Notice how when you click STEP, the rocks don't do anything, but the Flowers all rotate 45 degrees (watch the red part of each flower).  You can also follow along with the debugging statements being printed; you can see each Rock and Flower prints where they are, and Flowers print their directions.
- **In Rock.java and Flower.java, remove the debugging print statements in `act()` because we don't need them anymore.**

### Fancy Flowers

- Open `FancyFlower.java`.  A FancyFlower is just like a normal flower, except the user can control how much it rotates during each timestep, and also it switches back and forth between rotating clockwise and counterclockwise.
- Notice the following:
  - FancyFlower extends Flower, so it inherits all the behaviors of a regular Flower.
  - FancyFlower has two instance variables, `rotatingClockwise` and `degreesPerRotation`.  These are initialized in the constructor.
  - Read through the `act()` method so you can understand how it works.
  - Open `FancyFlowerDemo.java` and run it.  Notice how it creates four different flowers, each one rotating at a different speed, and in different directions, based on the four different constructor calls in the code.

### Bugs

- Open `Bug.java`.  Notice how `Bug` does something more complicated.  It overrides `act()`, but also has other methods: `turnRight()`, `moveForward()`, and `canMoveForward()`.  Read through them to understand how they work.
- Run `BugDemo.java` to see the bug moving around.
- At this point, you should have a basic understanding of the moving parts of the project.

## Project Part A: SquareBug

Now it's time to write some code! In this part, you will write code for `SquareBug`, which is a custom version of the `Bug` class.  A `SquareBug` draws a square shape using flowers on the screen.  Once the square is drawn, the bug continues to walk around the square forever.  See a demo of how it should act [here](squarebug.mov) for a square of side length 5.

Open `SquareBug.java` and run it the corresponding `SquareBugDemo.java` file.  Notice how the `SquareBug` doesn't move.  Your goal is to fill in the parts of the code that are missing so that the SquareBug draws a square using flowers on the board.  `SquareBug` extends `Bug`, so you have access to all the things that a `Bug` already does, like knowing how to move forward, turning, and dropping a flower as it moves forward.  All you have to do is write `act()` so that the bug moves forward (call `moveForward()` at the appropriate times and turns (call `turnRight()`) at the appropriate times.  

Suggestions:

- The constructor receives a `sideLength` argument.  This is the length of the side of a square that should be drawn (the video above is a demo for a square of `sideLength` 5).  You may want to save this argument into a field so it can be accessed when `act()` is called.

- Remember, `act()` can only do one thing at once!  So, for example, don't put a `for` or `while` loop in act that walks forward a bunch of times.  Each call to `act()` should only make one movement, either a turn or a step forward.

- How should `act()` know to move forward or turn?  Can you use `sideLength` to figure it out?  Consider having a second field that keeps track of how far the bug has walked so far along the current side of the square.  Maybe when it's taken the same number of steps as the `sideLength` field, it's time to turn!

- Once the square is complete, the bug should continue walking around the square forever.

- This is easier than you think!  Remember, `SquareBug` is a `Bug`, and a `Bug` already drops a flower behind it, so you don't have to worry about writing code to drop flowers.  When it walks onto a `Flower` that already exists, the `Flower` will disappear, but when you leave that square, the `Bug` will again drop a `Flower` behind it.

## Project Part B: MazeWorld

For the second part of the project, you will write a program to get a mouse to solve a maze.  The mouse will not solve the maze optimally, but will rather use a simple strategy called the "wall follower" or "right-hand-rule," which you can read about [here](https://en.wikipedia.org/wiki/Maze_solving_algorithm#Wall_follower).  Basically it means you walk through the maze keeping your right hand on the wall at all times.  While this will result in you (or rather, the mouse) backtracking sometimes, you are guaranteed to find the exit.

These mazes will be read from files.  Open `maze1.txt`, `maze2.txt` to get a feel for them.  The first line of the file contains the dimensions of the maze in rows and columns, and the rest of the file describes each square of the maze, where a "R" is a wall (Rock), a period is an open square, "M" represents the starting location of the mouse, and "F" represents a `Flower`, which is where the mouse will finish.

All the code you'll need here is in `MazeWorld.java` and `Mouse.java`.  `Mouse.java` is the `Actor` class that will control the mouse movements, and `MazeWorld.java` is the program that reads the maze files and starts the mouse running.

**First**, you should write the code that reads the maze files.  This is mostly done for you.  Fill in the rest inside of `MazeWorld.java`: the only code you need to change is the three `if`-statement bodies in `loadBoard()`.  Inside each `if` statement, you will create either a `Rock`, a `Flower`, or a `Mouse`, and call `addSelfToBoard()` with the appropriate `Location`.

**Second**, you will need to implement the wall follower algorithm in `Mouse.java`.  See later in this file for how the algorithm works.  The key here is that `act()` can only take one step of the algorithm at a time.  

## Suggestions and hints:

- Once the mouse is directly north/south/east or west of the flower, it should remove itself from the board (use `removeSelfFromBoard()`) and **print out the number of steps it took to find the flower**.  (Hint: Use a instance variable/field in the `Mouse` class to keep track of the number of steps.)

- `Mouse` may extend `Actor`, as it does when you start, or if you want it to extend `Bug`, you can do that too.  However, this may cause some problems because `Bug`s drop `Flowers` when they move forward.  You may just want to copy and paste some of the `Bug` methods into `Mouse`, such as the turning and moving methods and modify them as appropriate.

## Wall Follower Algorithm

The wall follower algorithm only involves three actions: walk forward, turn left, and turn right.  They happen at different times, based on the following criteria:

- If mouse is facing a wall or not (the square immediately in front of the mouse is a `Rock`)

- If the square immediately to the right of the mouse is open (or is a `Rock`)

- If the *previous* move of the mouse was a turn or a walk forward.

Here is the whole algorithm:

```
if the mouse is facing a wall:
    if the square to the right of the mouse is open:
        turn right
    else:
        turn left
else:
    if the square to the right of the mouse is open 
      AND the last move was a right turn:
        move forward
    else if the square to the right is open 
      AND the last move was NOT a right turn:
        turn right
    else: 
        move forward
```

You can deduce these properties inside of the `act()` function in a few ways:

- To get your current location, call `getLocation()`.

- To see what's in an adjacent square on the board call `board.get(location)` where `location` is a `Location` object.

- You can use `Location` and `Direction` objects together to get one `Location` based on another `Location`, or a `Direction` based on another `Direction`.  For instance, here's some code that looks up the `Actor` that is immediately to the north of the current `Actor` (assuming this code goes inside `act()`):

  ```java
  Location loc = getLocation();
  Location locToTheNorth = loc.getAdjacentLocation(Direction.NORTH);
  Actor actorNorthOfMe = board.get(locToTheNorth);
  ```

- You can do math with `Directions` to calculate what it means to turn right or left if the mouse is facing various directions.

### Videos of the final product

**Don't forget that your mouse should print out the total number of steps it has taken once it finds the flower.**

Notice how in each of these videos, the mouse's tail always stays connected to the wall on its right.  The only exception is when the mouse executes a turn, where the tail comes off the wall for two steps, but then immediately connects again once the mouse finishes the turn.

- [maze1](maze1.mov) (7 steps)

- [maze2](maze2.mov) (7 steps)

- [maze3](maze3.mov) (34 steps)

- [maze4](maze4.mov) (80 steps)

- [maze5](maze5.mov) (425 steps)

## How to turn in your code

Upload the following files to Canvas:

- `SquareBug.java`
- `MazeWorld.java`
- `Mouse.java`