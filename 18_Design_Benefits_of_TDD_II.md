# Video 18 - Design Benefits of TDD II


## Tic Tac Toe Game
If using Java create a new directory named "src".  Inside of the src directory create a package called "game".  Create another directory called "test" and inside it create a package also called "game". The important part of this is that there are two directories and they have the same package name inside them.  Create "TiicTacToeTest" class in the test directory.


## Canary Test
Canary test is a sacrificial test that ensures the environment is set up right.  Example:

```java
package game;

import org.junit.Test;
import static org.junit.Assert.assertTrue;

public class TicTacToeTest() {
	@Test
	public void Canary() {
		assertTrue(true);
	}
}
```


NOTE:  Write Canary tests for each new test project...DO NOT write canary tests for every test class.




## Tracking Tasks (aka Tests)
Keep tasks on a piece of paper (actual paper is recommended...)
1. Game not won by anyone when the game starts 
1. Pick the first player
1. Pick the second player
1. First player places peg at some location (positive test)
1. Second player places peg at some location (positive test)
1. place peg at occupied position (negative test)
1. place peg out of row range (negative test)
1. place peg out of column range (negative test)
1. Set first peg should not be other than X and O


## Practices
- jot down tests that come to your mind.  Dont worry if test is good or not, just write it down.
- After coming up with a few tests, pick one that is easy and can be written quickly and imoplement it.
- Pick a test that is a **value**.
 - write a failing test first, then write minimal code to make that test pass.
 - Jot down a positive, negative, and exception test
 - Strive for simplicity; reduce and remove accidental complexity.
 - Every test should have an assert
 - DO NOT write more than one independent assert per test.
 - Keep tests isolated from each other
 - Tests have to be **FAIR **- **Fast, Automated, Isolated, Repeatable**
 - When tests pass, take a look at the code to see if it can be simle, can be refactored.  *"Any time you have an opportunity to write a comment, you have an opportunity*
*      to refactor the code into a self standing method that is easier to communicate."*
 -design decisions have to be doncumented through tests cases
 
## Principles
- **YAGNI** principle:  "**You're Not Gonna Need It** (yet)".  Meaning the classes youre thinking about typically are not be needed yet.  We want to postpone things until the last
    responsible moment.
-Last Responsible Moment:  The moment beyond which you cannot postpone a decision.
- Keep it **DRY - Don't Repeat Yourself**
## Tic Tac Toe Game Code


TicTacToe.java

```java
package game;

public class TicTacToe{
    public enum Peg {NONE, FIRST};

    public String winner() {
        //will cause GameWonByNoneAtStart() test to fail
        //return null;
        //minimal code to pass GameWonByNoneAtStart()
        return "none";
    }
    
    public Peg placePeg(int row, int column){
        return Peg.FIRST;
    }
}
```




TicTacToeTest.java

```java
package game;

import org.junit.Test;
import static org.junit.Assert.assertTrue;

//importing Peg to use in the placeFirstPeg test
import game.TicTacToe.Peg;

public class TicTacToeTest(){

	@Test
	public void Canary(){
		assertTrue(true);
	}

        //add tests from the tasks list 
        @Test
        public void GameWonByNoneAtStart(){
                //To test this first we need a Tic Tac Toe game
                TicTacToe ticTacToe = new TicTacToe();
                //nobody has won the game yet
                assertEquals("none", ticTacToe.winner());
        }
        
        
        
        //----------------------------------------------------------------------------------------------------------------------------------
        //EVERYTHIHNG IN THIS SECTION WILL BE REMOVED.  THESE TESTS WERE USED TO SHOW HOW TESTS SHOULD EVOLVE BASED ON THE YAGNI PRINCIPLE.
        //NOTE THAT THEY ARE ALL THE SAME TEST, BUT THE BECOME SIMPLER AS TEHY ARE REWRITTEN.
        //
        //First attempt at pickFirstPlayer test...This is not ideal.  Remember to "listen to the whispers of the test".  This test feels a bit        //heavy.  5 things must be created for this test to work.  So start asking questions.  Why are we setting the first player?  Because 
        //we need to know who the first player is.  Then why are we creating class?  Well the domain model said we need a player class.  Yes 
        //but why are we giving the name of the player?  We'll we need to know the name of the player because if the player wins we would need
        //to report it.  BUT WAIT, is the player winning right now?  No we havent even started the game yet.  So then why are we concerned 
        //about who the winner is?  We're not yet!  This is the YAGNI principle explained in the "Principles" section above.  Realize that 
        //creating the player can be postponed until we need to know the score.  
        @Test
        public void pickFirstPlayer(){
                TicTacToe ticTacToe = new TicTacToe();
                //set new player attributes as name = Venkat and peg type is 'X'
                ticTacToe.setFirstPlayer(new PLayer("Venkat", new X()));
                //test that the players name = Venkat and that he is using the X pegs 
                assertEquals("Venkat", ticTacToe.getFirstPlayer().getName());
                assertEquals("X", ticTacToe.getFirstPlayer().getPeg().getFace());
        }
        //So lets rewrite this test with the YAGNI principle in mind.  The identity of the player is not important to us at this moment.  In
        //fact, the game Tic tac toe doesnt care about the identity of the player at this level...it may caare about it at the overall level,         //but not at this level.
        @Test
        public void pickFirstPlayer(){
                TicTacToe ticTacToe = new TicTacToe();
                //Note that we have gotten rid of the player at this point.  But this is still unneccessarily complex.  Here we would need to                 //create 1 method, 1 class, and 1 function at least to make this test work.  
                ticTacToe.setFirstPlayerPeg(new X());
                assertEquals("X", ticTacToe.getFirstPlayerPeg().getFace());
        }
        //2nd rewite of this test.  
        @Test
        public void pickFirstPlayer(){
                //We've removed the peg class, but what if someone enters a W or Z?
                ticTacToe.setFirstPlayerPeg("X");
                assertEquals("X", ticTacToe.getFirstPlayerPeg());
        }
        //3rd rewite of this test.  
        @Test
        public void pickFirstPlayer(){
                //Setting the first player peg in this way eliminates the problem of setting anything other than an X or O
                ticTacToe.setFirstPlayerPegIsX(true);
                assertEquals("X", ticTacToe.getFirstPlayerPeg());
        }
        //but we want an action to perform, so lets try something different.  What is the first action someone will try with the tic
        //tac toe board?  They will try to place a peg.  It doesnt matter what the visual representation of the pegs are...they could be X 
        //and O, W and L, Goat and Tiger, etc.  What is fundamental is that players take turns starting with the first player.  So we can
        //eliminate all of these tests by realizing that we can place the first peg when making the first move.  
        //MEANING, ALL OF THESE PickFirstPlayer() TESTS ARE UNNECESSARY AND SHOULD BE REMOVED.  THIS IS HOW TESTING CAN INFLUENCE 
        //DESIGN.
        //
        //-------------------------------------------------------------------------------------------------------------------------------
        
        @Test
        public void placeFirstPeg(){
                TicTacToe ticTacToe = new TicTacToe();
                //do we care about the cell or just position?
                //ticTacoToe.placePeg(new Cell());
                //no need to create cell, just give position.
                assertEquals(Peg.First, ticTacToe(0, 1));
        }
        
        @Test
        public void placeSecondPeg(){
                //Notice how we're creating a new TicTacToe game with each test.  Remember to keep it DRY.  This can be done with @before and 
                //@after methods.  
                TicTacToe ticTacToe = new TicTacToe();

        }
        
}
```






## @Before and @After:  Using setup and teardown methods to keep code DRY


TicTacToe.java

```java
package game;

public class TicTacToe{
private final int SIZE = 3;
    private Peg currentPeg;
    private Peg[][] cells = new Peg[SIZE][SIZE]
    public enum Peg {FIRST, SECOND};

    public TicTacToe(){
        for (int i = 0; i < SIZE; i++){
                for (int j=0; j<SIZE;j++){
                        cells[i][j] = Peg.NONE;
                }
        }
        currentPeg = Peg.FIRST;
    }

    public String winner() {
        return "none";
    }
    
    public Peg placePeg(int row, int column){
        if (cells[row][column] == Peg.NONE){
                cells[row][column] = currentPeg;
                currentPeg = togglePeg();
        }
        return cells[row][column];
    }
}
```




TicTacToeTest.java

```java
package game;

import org.junit.Test;
import static org.junit.Assert.assertTrue;

//importing Peg to use in the placeFirstPeg test
import game.TicTacToe.Peg;

public class TicTacToeTest(){

//---------------------------------------------------------------------------------------------------------------------------------------
//Each test will now be sandwiched between a Setup and Tear down method.  Think of a restaurant, every customer gets a new table, and when 
//theyre done someone preps the table for the next customer.
        //set up a new game for each test.  When done this way, we dont repeat creating a new game in each test. We also
        //prevent passing around any TicTacToe instances that have been polluted by previous tests.
        @Before
        public void setUp(){
                System.out.println("setUp");
                ticTacToe = new TicTacToe();
        }
        
        //The tear down method.  This will happen after each test.  Often we dont need a tear down.
        @After
        public void inTheEnd(){
                System.out.println("done...")
        }
//-------------------------------------------------------------------------------------------------------------------------------------------      

	@Test
	public void Canary(){
		assertTrue(true);
	}

        //add tests from the tasks list 
        @Test
        public void GameWonByNoneAtStart(){
                //nobody has won the game yet
                assertEquals("none", ticTacToe.winner());
        }
        

        @Test
        public void placeFirstPeg(){
                assertEquals(Peg.First, ticTacToe(0, 1));
        }
        
        @Test
        public void placeSecondPeg(){
                ticTacToe.placePeg(0, 2);
                assertEquals(peg.SECOND, ticTacToe.placePeg(0, 1));
        }
        
        @Test
        public void PlacePegAtOccupiedPosition() {
                //place peg at location 2,1 
                ticTacToe.placePeg(2, 1);
                //here were placing another peg in the same spot as the one before.  Right now were ignoring FIRST and SECOND.  This 
                //will lead to a problem with the 
                assertEquals(Peg.FIRST, placePeg(2, 1);
        }
        
        @Test
        public void placePegOutOfRowRange() {
        
                try{
                        ticTactoe.placePeg(0, 7);
                        fail("Expected exception for stepping out of bounds");
                } catch(IndexOutOfBoundsException ex){
                        //:)
                }
        }
        
}
```






















