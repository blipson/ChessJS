# Chess Engine in Javascript

Hobby project by Ben Lipson. The engine can beat me consistently, and I'm about a 1300 elo chess player. So far it can read and write positions to/from the chess board and print those positions out in the following format:

8	r 	n 	b 	q 	k 	n 	b 	r
7 	p 	p 	p 	p 	p 	p 	p 	p
6
5
4
3
2 	p 	p 	p 	p 	p 	p 	p 	p
1	r 	n 	b 	q 	k 	n 	b 	r

It also obviously has a full GUI for playing the game. The thinking time can be set anywhere from 1 second to 10 seconds. It shows the best move for any given scenario, the depth analysis of how many moves ahead it's looking, the overall score of the game where positive numbers are in white's favor and negative are in black's, the number of nodes in our depth first search algorithm, the move ordering percentage, and the time it took to make the last move. The active features are that you can force the engine to make a move, start a new game at any time, flip the board, or undo the last move.

## Board Representation
The board is actually represented as a single dimensional array with 120 elements. This is because you need to represent two spaces out on the board from any direction in order to calculate when a piece moves off the board and therefore can't move to that square (this is mostly used for knights). Files and ranks are represented as numbers 0-8 where 8 means none. These are mapped to values like A1, B2, etc... to correspond to A1 = 00, B2 = 11. Similarly, the colors of the board are represented as 0 = white, 1 = black, 2 = both. Here's what the full board looks like to the engine:

![alt tag](http://1000projects.org/wp-content/uploads/2012/09/offset-representaion.jpg)

There are functions written to convert the 120 square board to the regular 64 square board and back. Here's the code to initialize them both:

	function InitSq120To64() {

		var index = 0;
		var file = FILES.FILE_A;
		var rank = RANKS.RANK_1;
		var sq = SQUARES.A1;
		var sq64 = 0;

		for(index = 0; index < BRD_SQ_NUM; ++index) {
			Sq120ToSq64[index] = 65;
		}
		
		for(index = 0; index < 64; ++index) {
			Sq64ToSq120[index] = 120;
		}
		
		for(rank = RANKS.RANK_1; rank <= RANKS.RANK_8; ++rank) {
			for(file = FILES.FILE_A; file <= FILES.FILE_H; ++file) {
				sq = FR2SQ(file,rank);
				Sq64ToSq120[sq64] = sq;
				Sq120ToSq64[sq] = sq64;
				sq64++;
			}
		}
	}

At any given time there are actually two different boards being compared against each other, one for the files and one for the ranks. There's also a GameBoard object for the actual GUI 64 square board (the array itself is 120). We can get the square in either arry from a file number and a rank number using the fileRankToSquare() method.

## Pieces
The pieces of the game are represented as the color followed by the piece name. For example, wK would mean white knight. There's a pieces array to represent all the pieces on the board.

## Forsyth–Edwards Notation (FEN)
FEN is a system of representing any given chess position as a single string. The starting position is represented as "rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1". Each rank is separated by a '/', and the piece positions are shown with the first letter of each piece. Lower case is for black, upper for white. The next letter determines who's move it is, followed by the castling permissions (if there's no castling allowed it's simply a '-'), then en pessante permissions for any pawns that might have that (again if there's none it's a '-'), followed by the half move clock (the number of half moves (where a full move is when both players make a move) since the last capture, used for draw purposes), then the fullmove number for the whole game. This is the notation used to put in and take out positions from the board, and is displayed in the GUI.

## Castling
Castling permissions are represented in a four-bit binary format where the first bit means white on king side, second means white on queen side, third means black on king side, and fourth means black on queen side. So if all permissions are allowed it'd be 1111, if none it'd be 0000. Thus the bits are represented as 1, 2, 4, and 8.

## Contributors
This project was made by Ben Lipson in 2016.

## References
Most of the necessary learning done for this project came out of the YouTube series by Blue Fever Software found here: https://www.youtube.com/playlist?list=PLZ1QII7yudbe4gz2gh9BCI6VDA-xafLog