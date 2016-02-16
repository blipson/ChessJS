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

## Code Example

Show what the library does as concisely as possible, developers should be able to figure out **how** your project solves their problem by looking at the code example. Make sure the API you are showing off is obvious, and that your code is short and concise.

## Motivation

A short description of the motivation behind the creation and maintenance of the project. This should explain **why** the project exists.

## Installation

Provide code examples and explanations of how to get the project.

## API Reference

Depending on the size of the project, if it is small and simple enough the reference docs can be added to the README. For medium size to larger projects it is important to at least provide a link to where the API reference docs live.

## Tests

Describe and show how to run the tests with code examples.

## Contributors

Let people know how they can dive into the project, include important links to things like issue trackers, irc, twitter accounts if applicable.

## License

A short snippet describing the license (MIT, Apache, etc.)