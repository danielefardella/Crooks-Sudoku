# Crooks_Sudoku
Implementation of Crook's Algorithm for solving sudoku puzzles in Python

Unless otherwise explicitly stated in the file, all files in this repository are under the creative commons license Attribution 4.0 International (CC BY 4.0). See https://creativecommons.org/licenses/by/4.0/. "Crook's Algorithm.pdf" exists here as a convenience to the user under fair use. I claim no ownership over that work. All rights to that work belong to the owner, J. F. Crook, and the American Mathematical Society. More information can be found here: https://www.ams.org/about-us/copyright. The paper is publicly available here: http://www.ams.org/notices/200904/rtx090400460p.pdf.

Feel free to email me with any questions at dopdahl16@gmail.com. And please contact me if you're able to track down a way to contact Mr.Crook (Dr.Crook?), as I haven't found any sort of email for him.

**Current state of the project:**
- I've implemented everything in Crook's paper up until the section "A Sudoku Puzzle Whose Solution Requires Random Choice". I hope to next implement Crook's random choice method using vertices and trees. As such, as the project stands right now, the program PenAndPaperSudoku.py cannot solve *every* sudoku puzzle. It can only solve the puzzles that do not require random choice (which is the majority in any case).

### Development Notes:
1.
On page 463, in Crook's paper, he introduces the idea of forced cells, defining them as "a box that is missing [a certain] number and... is [the] one and only cell into which this number can be entered." Essentially, a forced cell is one that we can fill in without the use of preemptive sets, using only the numbers that have been given to us at the start of the puzzle. Interestingly, many easier sudoku puzzles can be solved using only forced cells, without the need of preemptive sets. Sudoku1.txt (printed below) is one such example. Preemptive sets are not needed in order to solve this puzzle. It can be solved completely by the use of forced cells.
```
Sudoku1.txt
3 x 1 x 7 9 x 2 5
x x x 6 x x 4 1 7
x x x x 1 5 3 x x
x 9 x x 4 7 x x 2
x x 4 3 x 8 x 7 x
x 8 x 9 6 x 5 3 x
7 x 5 x 9 6 x 4 8
2 1 x 5 x x 7 x 6
x 4 x 7 x 1 2 5 x
```
In any case, in building my program, I found it interesting to discover that on page 464 in Crook's paper, he claims that "There are two forced numbers in Shortz 301: 1 in `c (2, 3)` and 9 in `c (2, 6)`." (Shortz 301 is the puzzle that he uses as an example) I found this strange, because when I used my method for finding forced cells, I found three forced numbers in Shortz 301! In addition to the `1` in `c (2, 3)` and the `9` in `c (2, 6)`, I found a `7` at `c (5, 9)`. At first glance, one might be skeptical of the validity of my findings. But they are justified. If we look at the 9th column, in descending order, we have `[ x, x, 4, 3, x, x, x, 5, 8 ]`, where an "x" represents a blank space. Now, taking a look at the other `7`'s on the board, we can see that the `7` that must necessarily appear somewhere in this row (by definition of the solution of a Sudoku puzzle - see page 461 of Crook's paper) cannot appear in the top two open spaces (`c (1, 9)` & `c (2, 9)`) because there is already a `7` in that 3x3 box. Furthermore, we see that the `7` that must appear in this row cannot appear in the bottom two open spaces (`c (6, 9)` & `c (7, 9)`), as both those open spaces already have a `7` in their row. This leaves us with one option in which to place the `7` that must go in this row: `c (5, 9)`, by definition, a forced cell.
Crook does go on to say that the `7` in question is actually a "hidden singleton", and he gives pretty much the same justification for why that `7` must go where it does, but I do not understand what the difference is between a hidden singleton and a forced cell... Maybe forced cells can only be found by eliminating possibilities via rows and columns? I don't know. I would love to be able to ask Crook.
```
Shortz 301 (untouched)
x 3 9 5 x x x x x
x x x 8 x x x 7 x
x x x x 1 x 9 x 4
1 x x 4 x x x x 3
x x x x x x x x x
x x 7 x x x 8 6 x
x x 6 7 x 8 2 x x
x 1 x x 9 x x x 5
x x x x x 1 x x 8
```
```
Shortz 301 (forced cells filled in according to Crook)
x 3 9 5 x x x x x
x x 1 8 x 9 x 7 x
x x x x 1 x 9 x 4
1 x x 4 x x x x 3
x x x x x x x x x
x x 7 x x x 8 6 x
x x 6 7 x 8 2 x x
x 1 x x 9 x x x 5
x x x x x 1 x x 8
```
```
Shortz 301 (forced cells filled in according to me)
x 3 9 5 x x x x x
x x 1 8 x 9 x 7 x
x x x x 1 x 9 x 4
1 x x 4 x x x x 3
x x x x x x x x 7
x x 7 x x x 8 6 x
x x 6 7 x 8 2 x x
x 1 x x 9 x x x 5
x x x x x 1 x x 8
```
2.
If you have trouble figuring out how to identify preemptive sets on your own, as I did, you may find it helpful to read [this](http://damienzhang.com/sudoku/) blog post by Damien Zhang, as I did. I employ his observations about preemptive sets in my code.
> So a way to check whether a set of cells is a preemptive set is to simply calculate the union of the markup of the cells (which gives you the set of numbers that those cells can contain), and then compare the size of that union to the number of cells. If those two quantities are equal, then you have a preemptive set.
