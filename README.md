# Minesweep

Minesweeper bot that outputs a board with optimal moves based on the current state of the input board.

Works based on 2 fundamental ideas of minesweeper:
1. If the number of tiles surrounding a number is equal to the number then all tiles are bombs.
2. If the number of bombs surrounding a number is equal to the number then all the remaining tiles are safe.
