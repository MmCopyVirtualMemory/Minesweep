# Minesweep

Minesweeper bot that outputs a board with optimal moves based on the current state of the input board.

Works based on 2 fundamental ideas of minesweeper:
* If the number of tiles surrounding a number is equal to the number then all tiles are bombs.
* If the number of bombs surrounding a number is equal to the number then all the remaining tiles are safe.

```cpp
#include <vector>
#include <iostream>
#include <map>

//! = mine
//? = not shown
//1-8 = num mines
std::vector<std::vector<char>> ProcessMinesweeperBoard(std::vector<std::vector<char>> board) //place flags and clicks
{
	std::map<char, int> nums =
	{
		{ '0', 0 },
		{ '1', 1 },
		{ '2', 2 },
		{ '3', 3 },
		{ '4', 4 },
		{ '5', 5 },
		{ '6', 6 },
		{ '7', 7 },
		{ '8', 8 }
	};
	auto GetType = [&](char c) -> std::pair<int, int>
	{
		int type = 0; //1 = num; 2 = mine; 3 = unk
		int num = 0;
		try
		{
			num = nums.at(c);
			type = 1;
		}
		catch (std::exception e)
		{
			switch (c)
			{
			case '!': //flag
			{
				type = 2;
				break;
			}
			case '?': //unk
			{
				type = 3;
				break;
			}
			}
		}
		return { type, num };
	};
	std::vector<std::vector<char>> moves = board;
	for (int i = 0; i < moves.size(); i++)
	{
		for (int j = 0; j < moves[i].size(); j++)
		{
			char current = moves[i][j];
			auto [type, num] = GetType(current);
			if (type == 1)
			{
				std::vector<std::pair<int, int>> nearby_bombs;
				std::vector<std::pair<int, int>> nearby_tiles;
				for (int k = i - 1; k <= i + 1; k++)
				{
					for (int l = j - 1; l <= j + 1; l++)
					{
						char other = ' ';
						bool good = true;
						try
						{
							other = moves.at(k).at(l);
						}
						catch (std::exception e)
						{
							good = false;
						}
						if (good)
						{
							auto [other_type, other_num] = GetType(other);
							if (other_type == 2)
							{
								nearby_bombs.push_back({ k, l });
							}
							if (other_type == 3)
							{
								nearby_tiles.push_back({ k, l });
							}
						}
					}
				}
				if (nearby_bombs.size() == num)
				{
					for (auto tile : nearby_tiles)
					{
						moves[tile.first][tile.second] = 'S'; //safe
					}
				}
				else if (nearby_bombs.size() + nearby_tiles.size() == num)
				{
					for (auto tile : nearby_tiles)
					{
						moves[tile.first][tile.second] = '!'; //bomb
					}
				}
			}
		}
	}
	return moves;
}

int main() 
{
	std::vector<std::vector<char>> board = 
	{
		{ '1', '2', '1' },
		{ '!', '?', '?' },
		{ '1', '2', '1' }
	};
	std::vector<std::vector<char>> moves = ProcessMinesweeperBoard(board);
}
```
