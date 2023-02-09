# Project 3: SortedList and No Thanks

In this project, you will implement a doubly-linked list that maintains a list of integers in sorted order. In other words, the user has no control over where in the list items are inserted; they will automatically be placed into the correct position to keep the list in sorted order at all times. We will call this data structure a `SortedList`. You will then use this data structure to develop an artificial intelligence program to play the card game [No Thanks](https://en.wikipedia.org/wiki/No_Thanks!_(game)).

## General Guidelines

Here are a few important guidelines:

- You should only edit the files that you will end up submitting at the end of the project.
- You should not use any libraries other than the Java standard library.
- If you are asked to write code that prints output, you should **aim to match the sample output exactly, character for character.**

You may use any code editor you like.

## Getting Started

These instructions are specific to IntelliJ, but you may adapt them to any other code editor if you like.

1. Download the starter code [here](code.zip).
2. Unzip the downloaded `code.zip` file, which should give you a `src` folder.
3. Make a new IntelliJ project using `File > New > Project...`. In the "New Project" pop-up window, select "New Project" (not "Empty Project"), and make sure that the language is Java and the build system is IntelliJ. Pay attention to the project location you choose.
4. Outside of IntelliJ, navigate to the folder where your project was created. Replace the `src` folder there with the `src` folder you unzipped.
5. Return to IntelliJ, and you should see the `src` folder you unzipped.
6. To verify everything is working correctly, open the `SortedListTester.java` file and run it by pressing the green arrow next to the `main` function. (It should compile without error, though the output won't yet be meaningful.)

### Background: Generics

Java has a feature called *generics*, which allows us to create a class that depends on a type. For example, the built-in `ArrayList` class is generic, in order to allow clients to work with array-lists of any type. This is done by including `<E>` when defining the class; for example:

```java
public class ArrayList<E> {

    private E[] data;
    private int size;

    // ...

    public E get(int index) {
        // ...
    }

    // ...

}
```

In the example code above, `E` represents the element type of the array-list. Then we've used `E[]` to refer to arrays of type `E`. Notice also that the `get()` method returns an object of type `E`.

As you know, when we declare or construct an array-list in Java, we include the element type. For example, to construct an array-list of ints in Java, we write:

```java
// The `Integer` on the left is required; the `Integer` on the right is optional.
ArrayList<Integer> lst = new ArrayList<Integer>();
```

## Background: Comparable

We want our `SortedList` class to be generic, with one restriction. Since the objects must be kept in sorted order, there must be some way to compare them. This is straightforward for simple things like integers and doubles, but what if we made a `SortedList<Dog>`?  How do we determine if one `Dog` should come before or after another `Dog` in sorted order?  Java has a built-in mechanism for this.

Java contains an interface called `Comparable`.  ([Javadoc](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/lang/Comparable.html)) This interface may be implemented by any class that has a need to be sorted or stored in a data structure that does sorting, such as our `SortedList`.  Many existing Java classes implement Comparable, such as all the numeric classes (Integer, Double, etc), String, and many built-in classes.

`Comparable` only has one method that must be implemented: `int compareTo(other)`. This method's job is to compare "this" object against another object (`other`) to see which one is bigger or smaller.  If this object is smaller, this function should return a negative integer (often `-1`).  If this object is bigger, it should return a positive integer (often `+1`).  If the two objects are equal, it should return `0`.

Here's an example of this method.  Suppose we wanted to write a `compareTo` method for a hypothetical `Time` class, which represents the time of day in a 24-hour format.  It might look like this:

```java
public class Time implements Comparable<Time> {
  private int hour;    // 0-23
  private int minute;  // 0-59
  
  // other functions here....
  
  int compareTo(Time other) {
    if (hour < other.hour)
      return -1;  // we are smaller
    else if (hour > other.hour)
      return 1;   // we are bigger
    else {
      // we have the same hour, compare minutes
      if (minute < other.minute)
        return -1;
      else if (minute > other.minute)
        return 1;
      else
        return 0; // both hour and minute match, times are equal
    }
  }
}
```

Notice how all this function does is compare "this" `Time` against another `Time` argument passed in, and see which time is bigger or if they are the same.  Every `compareTo` function **must** return a negative integer, a positive integer, or zero according to the rules above.

That's all you need to understand about `Comparable` for now.

## How SortedList will work

SortedLists are based on the following node structure, which follows the standard doubly-linked list paradigm: 

```java
class Node {
    public E data;
    public Node next;   
    public Node prev;   
}
```

The `SortedList` structure itself maintains two pointers, one to the head of the list and one to the tail. The items within the list (the integers stored) increase in value from head to tail. The `SortedList` class also stores the number of items currently in the list (the size).

`SortedList`s have a few operations you will need to implement:

- Add an item to a SortedList. Given a new item, this operation places item in the correct spot within the SortedList. You may assume item does not already exist in the list (items will never be duplicated). 
- Remove an item from a SortedList. Given an item, this operation removes item from the SortedList if it is present in the list, and does nothing if item is not present.
- Test if an item is present in a SortedList or not.
- Clear the SortedList. This operation removes all the items from the SortedList. 
- Retrieve the size of the SortedList.
- Two methods for displaying a SortedList as a string (one internal, one external).

## Part A: Implementing SortedList [80 points of your grade]

- Implement all the methods in SortedList that are not already implemented. I suggest going in this order:

  - Write `add()` first, at least a simple version of it. Remember that `add()` should determine the correct position of the given item, by comparing it against the items already in the list. (If you get stuck with this, you might write a version of `add()` that just adds the item to the end of the list, and then come back and fix it later.) Some hints for writing `add()` are in the section below.

    **Important note**: Because you don't know the type of objects being stored in the SortedList, you can't compare them using `<` or `>`. Java will flag these as errors and your code won't compile.  Instead, you must call `compareTo` on these `E` objects.  For instance, if you have two `E` objects called `a` and `b`, and you would normally write `if (a < b)`, you should write this as `if (a.compareTo(b) < 0)`. Remember that if `a` is "less than" b, then the compareTo method will return a negative number, so this should make sense to you.  Here's a basic translation table:

    ```java
    REGULAR WAY         WITH COMPARABLES
    ------------------------------------
    if (a < b)          if (a.compareTo(b) < 0)
    if (a > b)          if (a.compareTo(b) > 0)
    if (a <= b)         if (a.compareTo(b) <= 0)
    if (a >= b)         if (a.compareTo(b) >= 0)
    if (a == b)         if (a.compareTo(b) == 0)   // or (a.equals(b))
    ```

    The conclusion here is that you **must** do all your comparisons in SortedList with `compareTo`, not by using `<` or `>`.
    **Do not forget to modify the `size` variable in the SortedList when you add an item to it!**

  - Write `toInternalString()`.  This will be the most helpful function you'll have during this project.  See the comments in the code for how to do this, but basically this function should traverse the list in both directions (not at the same time) and print the list going each way.  You should also print the size from counting the items in the traversal in both directions.  All of this should go into the string that is returned.  Use project 2 as a guide for how to build this string if you want.

  - At this point you should be able to run SortedListTester and see some meaningful output.  You should see items being added to your SortedList.

  - Now go back and fix `add()` if it wasn't working 100% the first time.  Remember, this function should add an item to its correct sorted position within the list.

  - Write `get()` and testing code for that.

  - Write `size()` and `clear()` and testing functions for those.  These functions should be very easy to write.

  - Write `contains()` and testing code.

  - Write `remove()` and testing code.  This function should not crash if the item is not in the list, but rather just do nothing.

  - Finally, write `toString()`, which is just a simplified version of `toInternalString()`.  

## General hints and suggestions

- Make sure to test all your operations on empty lists, lists with one element, and lists with more than one element. Also make sure to test your operations where manipulation of the list will involve the head, the tail, or both together.
- I recommend splitting `add` into a number of cases:
  - Adding an item to an empty list (because you will need to change head and tail).
  - What to do if the item should go before the head (because this will change head).
  - What to do if the item should go after the tail (because this will change tail).
  - All other cases - traverse the list to find the correct spot to insert the new item.
- You can split up remove similarly.  My code uses five cases: list is empty, list only has one element, removing the head, removing the tail, and everything else.

## Part B: No Thanks! [20 points of your grade]

The second part of this project involves writing code to play a simple game called No Thanks.  The game is oriented around a sorted list of cards (numbers), and therefore our SortedList class will come in handy here.

### Rules of the game

First, get aquainted with the game.  It is very easy to understand.

- Rules in video form: <https://www.youtube.com/watch?v=Lw2ua5B0TTM>
  - Another video with some strategy: <https://www.youtube.com/watch?v=F4c_U3CQ7RM>
- Rules as a PDF: <https://www.fgbradleys.com/rules/rules4/NoThanks%20-%20rules.pdf>
  - More explanation: <https://www.ultraboardgames.com/no-thanks/game-rules.php>

### What you need to do for Part B

In part B, you will be:

- Writing a function to determine the "card score" for a player (scoring the cards in their hand at the end of the game)
- Writing an artificial intelligence player to play the game.  If you would like, you may also enter your No Thanks AI player in a contest against other players from our class.

### Implementing `getCardScore()` [10 points]

- Open up `CardScore.java` in the `nothanks` package, and find the function `getCardScore()`; this is the function you will write.
- This function should calculate the "card score" for a SortedList of Integers.  Remember, each card (integer) scores the number of points on the card (the integer itself), *except* if that card is part of a sequence of cards in a row with no gaps (like 10-11-12 or 24-25-26-27).  In that case, only the *lowest* card in the sequence is scored, and the rest are ignored.
  - For example, the card score for the SortedList [10 12] would be 10 + 12 = 22.
  - The score for [10 11 12] would be just 10.
  - The score for [8 10 11 12 14] would be 8 + 10 + 14 = 32.
- To iterate through a SortedList, the most efficient way is to use a for-each loop. For example, `for (int card : cards) { ...`. This is more efficient than using `size()` and `get()`, since `get()` takes O(n) time.
- You should test your function by running the `main()` method in the `CardScore` class. You are encouraged to write additional tests in `main()` to make sure your function works!
- Note that in the real game, once the "card score" is calculated, the number of counters/chips each player has is subtracted.  This is not part of the `getCardScore()` function, however.

### Implementing the AI player [10 points]

- Open up `NoThanksRunner.java`.  This class will take care of running either a single game of No Thanks, or running lots of games in a row.
- You can run the main function write out of the box, and it will simulate a game of No Thanks with four players who play randomly.  
- Try switching the call from `playOneGame` to `playManyGames` in `main()` and it will play 100,000 games in a row, showing you final scores for each game.  You'll notice that with four random players, each one wins about 25% of the time.
- Take a look at `Player.java`.  This is a Java interface that every AI player must implement.  It has a single method, with all the information the AI player needs to know.
- Take a look at the three existing AI players: AlwaysTakePlayer, AlwaysRejectPlayer, and RandomPlayer.  Notice how each one implements a different strategy (although all three strategies aren't very intelligent), returning `true` to take a card (and the chips on the card), or `false` to reject the card.
- **Here's what you must do**: Make a new class that implements Player.  The name of your class should be `SomethingSomethingPlayer` where the `SomethingSomething` can be whatever you want, but try to make it unique enough that someone else in the class won't come up with the same idea.  For example,  name your player after yourself, your pet, or just pick some random adjectives (SuperCrazyFunPlayer?). Inappropriate player names will be changed.
  - In your class, write the `offeredCard` function in any way you want.  Try to be smart!  You have access to all the information in the game, including your current cards, the other players' cards, the number of chips you currently have, and the number of chips on the card (that you will collect if you take the card).  The only information you don't have is the number of chips each other player has, but with enough cleverness, you can deduce this in your program if you want.
- To test your player, add your player as one of the four players in the game in `main()` inside `NoThanksRunner`.  It doesn't matter what order the players are added in; the starting player is chosen randomly each time. 
- Your grade for this part of the project [10 points of the final grade] will be determined by how smart your AI player is versus 3 random players, so the bar is pretty low.  If you can win 30% of the time (note that you will win 25% of the time just by chance), you will get 5 points.  If you can win 90% of the time, you will get the full 10 points.  Somewhere in the middle will get between 5-10 points.
- I will pit everyone's players head-to-head and the best AI player in the class will receive extra credit points on the assignment.  

## Grading

Your project will be graded on 

- correctness of your output
- appropriateness of your algorithmic choices, including big-oh times: Your implementations should be as fast as is reasonably possible (in other words, do not use an O(n^2) algorithm where an O(n) will do.)
- thoroughness of your testing code
- intelligence of your AI player
- code style, including appropriate choices regarding comments, naming of variables, design and readability of your code, etc.

## Submitting your code

To submit, upload these four files: SortedList.java, SortedListTester.java, CardScore.java, and your AI player class.

