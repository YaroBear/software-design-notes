# Video 19 - Design Benefits of TDD III


## Tic Tac Toe game continued




## Tracking Tasks (aka Tests)
---------------------------------------------------------------------------------------------------
Keep tasks on a piece of paper (actual paper is recommended...)
1. Game not won by anyone when the game starts 
1. Pick the first player
1. Pick the second player
1. First player places peg at some location (positive test)
1. Second player places peg at some location (positive test)
1. place peg at occupied position (negative test)
1. place peg out of row range (negative test)
1. place peg out of column range (negative test)
1. Set first peg should not be other than X and O                                                                                                                                                                                                                                -------------------------------NEW TESTS-----------------------------------------------
1. Win by row match
1. declare winner for row match
1. no win by row match
1. Win by column match
1. declare winner for column match
1. Win by left diagonal match
1. declare winner for left diagonal match
1. Win by right diagonal match
1. declare winner for right diagonal match
1. place peg after a game is won


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
 --------------------------------NEW PRACTICES----------------------------------------------------------
 
 
 
## Principles
- **YAGNI** principle:  "**You're Not Gonna Need It** (yet)".  Meaning the classes youre thinking about typically are not be needed yet.  We want to postpone things until the last
    responsible moment.
-Last Responsible Moment:  The moment beyond which you cannot postpone a decision.
- Keep it **DRY - Don't Repeat Yourself**
-------------------------------NEW PRINCIPLES--------------------------------------------------------------
- **SRP **- Single Responsibility Principle
- **SLAP **- Single Level of Abstraction Principle








## Game code
TicTacToe.java

```java
package game;

public class TicTacToe{
private final int SIZE = 3;
    private Peg currentPeg;
    private Peg[][] cells = new Peg[SIZE][SIZE];
    
    public Peg isARowMatch(){
        for (int i=0;i<SIZE;i++){
            if (cells[i][0] == cells[i][1] && cells[i][1] == cells[i][2] && cells[0][j] != Peg.NONE)
                    return cells[0][j];
        }
        return Peg.NONE;
    }
    
    public Peg checkColumnMatch(){
        for(int j=0;j<SIZE;j++){
                if (cells[0][j] == cells[1][j] && cells[1][j] == cells[2][j] && cells[i][0] != Peg.NONE)
                    return cells[i][0];

        }
        return Peg.NONE;
    }
    
    public enum Peg {NONE, FIRST, SECOND};

    public TicTacToe(){
        for (int i = 0; i < SIZE; i++){
                for (int j=0; j<SIZE;j++){
                        cells[i][j] = Peg.NONE;
                }
        }
        currentPeg = Peg.FIRST;
    }

    public Peg winner() {
        Peg winner = checkRowMatch();
        if (winner != Peg.NONE)
            return winner;
        winner = checkColumnMatch();
        return winner;
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
import static org.junit.Assert.assertFalse;

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
                assertEquals(Peg.NONE, ticTacToe.winner());
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
        
        @Test
        public void WinByRowMatch(){
                //here the first player will win by filling up a row
                //first players turn
                ticTacToe.placePeg(0,1);
                //second players turn
                ticTacToe.placePeg(1,1);
                //first players turn
                ticTacToe.placePeg(0,0);
                //second players turn
                ticTacToe.placePeg(2,1);
                //first players turn.  First player has filled in row 0
                ticTacToe.placePeg(0,2);
                assertEquals(Peg.FIRST, ticTacToe.isARowMatch());
        }
        
        @Test
        public void noWinByRowMatch(){
                //first players turn
                ticTacToe.placePeg(0,1);
                //second players turn
                ticTacToe.placePeg(1,1);
                //first players turn
                ticTacToe.placePeg(0,0);
                //second players turn
                ticTacToe.placePeg(2,1);
                assertEquals(Peg.NONE, ticTacToe.isARowMatch());
        }
        
        @Test
        public void winnerReportsRowMatch(){
                ticTacToe = new TicTacToe(){
                    @Override
                    public Peg isARowMatch(){
                        return Peg.SECOND;
                    }
                };
                assertEquals(Peg.SECOND, ticTacToe.winner());
        }
        
        @Test
        public void WinByColumnMatch(){
                //here the second player will win by matching a column
                //first players turn
                ticTacToe.placePeg(0,2);
                //second players turn
                ticTacToe.placePeg(1,1);
                //first players turn
                ticTacToe.placePeg(2,2);
                //second players turn
                ticTacToe.placePeg(0,1);
                //first players turn.  
                ticTacToe.placePeg(0,0);
                //second players turn.  Second player has filled in col 1
                ticTacToe.placePeg(2,1);
                assertEquals(Peg.SECOND, ticTacToe.checkColumnMatch());
        }
        
        @Test
        public void noWinByRowMatch(){
                //first players turn
                ticTacToe.placePeg(0,1);
                //second players turn
                ticTacToe.placePeg(1,1);
                //first players turn
                ticTacToe.placePeg(0,0);
                //second players turn
                ticTacToe.placePeg(2,1);
                assertEquals(Peg.NONE, ticTacToe.isARowMatch());
        }
        
        @Test
        public void winnerReportsColumnMatch(){
                ticTacToe = new TicTacToe(){
                    @Override
                    public Peg isARowMatch(){
                        return Peg.NONE;
                    }
                    @Override
                    public Peg checkColumnMatch(){
                        return Peg.FIRST;
                    }
                };
                assertEquals(Peg.FIRST, ticTacToe.winner());
        }
        
        
        
}
```




## Game UI package


Create a new src  package called "game.ui".  In game.ui create a new class called "TicTacToeFrame".


TicTacToeFrame.java

```java
package game.ui;

import game.TicTacToe;
import game.TicTacToe.Peg;
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class TicTacToeFrame extends JFrame{
    private static final int SIZE = 3;
    TicTacToe ticTacToe;
    
    @Override
    protected void frameInit(){
        super.FrameInit();
        ticTacToe = new TicTacToe();
        setLayout(new GridLayout(3,3));
        for (int i=0;i<SIZE;i++){
            for (int j=0;j<SIZE;j++){
                TicTacToeCell cell = new TicTacToeCell(i,j);
                getContentPane().add(cell);
                cell.addActionListener(new CellClickedHandler());
            }
        }
    }
    
    public static void main(String[] args){
        JFrame frame = new TicTacToeFrame();
        frame.setSize(150,150);
        frame.setVisible(true);
    }
    
    public class CellClickedHandler implements ActionListener{
        @Override
        public void actionPerformed(ActionEvent actionEvent){
            TicTacToeCell cell = (TicTacToeCell) actionEvent.getSource();
            
            TicTacToe.peg placed = ticTacToe.placePeg(cell.row, cell.column);
            if(placed == Peg.FIRST)
                cell.setText("X");
            if(placed == Peg.SECOND)
                cell.setText("O");
            
            Peg winner = ticTacToe.winner();
            if (winner != Peg.NONE){
                cell,
                JOptionPane.showMessageDialog("We have a winner!: " + winner.toString());
            }
        }
    }
}
```




TicTacToeCell



```java
package game.ui;

import java.awt.*;

public class TicTacToeCell extends JButton{
    public final int row;
    public final int column;
    public TicTacToeCell(int theRow, int theColumn){
        row = theRow;
        column = theColumn;
        setSize(50,50);
    }
}
```






## Summary and final thoughts
- Let tests drive the code you are writing.  
- Write minimal code code
- Seek pragmatic and simple design
- Eliminate duplication
- TDD mantra:  Red, Green, Refactor
- Untested code is unfinished code
- Tests act as a safety net while refactoring
- Test on each platform the code will be run on
- Don't forget functional tests
- Who writes unit  tests?  Developers/programmers
- When should unit tests be written?  As the programmer is writing the code, not after.
- Unit tests and legacy code. "Put an end to legacy code by not writing more legavy code"




