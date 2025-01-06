# Mutation testing - Stephanie Bercy Section 

---
The mutation testing concept in simple words means introducing errors, bugs, in form of (bad code) and implementing test
with the sole purpose of detecting theses bugs #mutations #mutants.

My mutation testing is base on adding an incorrect move to a piece by calling a method, which is implemented and used by another piece (as a Legal move). 
---

## Let's start

### Some context: 
- 2 squares
- both (squares) from same player - same color - chosen (squares) positions on board  *'e2'* and one square above on the right  *'f3'*.
- pieces: 2 pawns pawn1; pawn2 - which can only move 1 square at a time *Legally*
- the introoduced mutations are diagonals movements left and rigth (which are not legal for pawn) these are used by hooks, queens and Kings
- pieces added to squares
- the assertion 1 set is that when diagonalrigth is sent as message to pawn1 the position on board should be 'f3'
- the assertion 2 set is : when diagonalleft is sent to pawn2 his position on the board should be 'e4'
  
  ---
  
  ## Visual aid

    ![image](https://github.com/user-attachments/assets/723131c6-e0e6-4779-b9dd-f0f4123842b1)


    
  ### As a result, my test have failed for both assertions as the legal move defined from project for pieces Pawns on board is one (1) square up.

![image](https://github.com/user-attachments/assets/0f93e680-a398-4e8f-bcf5-ee5da6572646)
![image](https://github.com/user-attachments/assets/19e068d0-6244-404d-9fa6-43f489c4ded6)

---
### here is the code implemented in my testing method  ```pharo testTargetSquaresLegalWhitePawn ```

```
  	pharo
testTargetSquaresLegalWhitePawn 
    | pawn pawn2 square square2 board col|
    board := MyChessBoard empty .
    square := MyChessSquare new.
    square2 := MyChessSquare new.	
	
     square color: Color white.
	  square2 color: Color white.
	
    square name: 'e2'.
	 square name: 'f3'.		
			
     pawn := MyPawn new.
	  pawn2 := MyPawn new.
	
    pawn color: Color white.
	 pawn2 color: Color white.
	
    board at: 'e2' put: pawn .
	 board at: 'f3' put: pawn2.
	
     square board: board .
	square2 board: board.
  
    square contents: pawn.
	 square2 contents: pawn.	 
		
    pawn upRightDiagonalLegal: true. "mutation added"
	 pawn2 upLeftDiagonalLegal: true.

    col := (pawn targetSquaresLegal: true) . 

    self assert: (pawn targetSquaresLegal: true) size equals: 1.
    self assert: ((pawn targetSquaresLegal: true) first name) equals: 'f3'.
	self assert: ((pawn2 targetSquaresLegal: true) first name) equals: 'e4'.

```
## Output 
![image](https://github.com/user-attachments/assets/a216185d-31d6-4fde-8d4d-c38a601e8d15)

## Conclusion 

My test should stop any deviations from the *legal* moves of the PAWNS - the test failed as expecteed, this means the test suite is ok and trustworthy. If the test had passed despite the *legal not * move -the mutation- it would have indicated there're some flaws in the test logic. 

---

# Bethuel Lafalaise

## Tests 101
Testing is the foundation of reliable software. It ensures confidence in the system, especially during changes. A test should create a context, perform an action, and verify the results using assertions.
- Tests verify that past functionalities still work after changes.
- They enable future evolution by reducing the fear of change and catching unnoticed bugs.
- Automation is critical. A unit test must be automated to be valid.

```
TestCase subclass: #SetTest

SetTest >> testAdd
   | empty |
   empty := Set new.
   empty add: 5. "Stimulus"
   empty add: 5.
   self assert: empty size equals: 1.
```

## Tests
Tests are like life insurance. They increase trust in your system, reduce fear of change, and support understanding of the code. Automated tests can be reused millions of times to validate behaviors.

Characteristics of Good Tests:
- Check extreme and complex cases.
- Provide good coverage of code.
- Act as active, up-to-date documentation.

```
TestCase subclass: #ColorTest

ColorTest >> testConvert
   self assert: Color white convert equals: '#FFFFFF'.
   self assert: Color black convert equals: '#000000'.
```

## Test-Driven Development (TDD)
TDD is a methodology where you write tests first, then code to pass the tests. It encourages clean APIs, solid functionality, and fewer regressions.

How it works ?
- Write a failing test.
- Write the code to make it pass.
- Refactor if needed and rerun all tests.

## Xtreme Test-Driven Development (XTDD)
XTDD enhances TDD by integrating live debugging. Developers can define methods or add instance variables on the fly during testing.

Advantages:
- Maintains developer flow.
- Allows interaction with objects directly in the debugger
