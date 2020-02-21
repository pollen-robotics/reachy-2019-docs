---
weight: 500
title: "TicTacToe Playground"
---

# TicTacToe Playground

The TicTacToe Playground is the first setup we designed with Reachy. We wanted to create a demo to emphasize Reachy's interactivity both with humans and when grasping and moving objects. We also wanted to wrap it as a game as it constrains the interaction (turn-taking, game rules are already known) and well it's fun :smile:

This setup requires a Reachy:
* with a Right Arm to move the pawn
* and a Head to look and analyze the board.

The robot is attached to a table where the board and the pawn are setup. The dimensions of the table, the board, and the fixation where chosen to permit Reachy to reach and grab easily objects on the table surface.

<!-- TODO: image -->

The rest of this section will show you how to run and play the TicTacToe demonstration. It will also give you details on how to customize or adapt some of its key aspects to really fit your needs.

## The TicTacToe demo

The demo lets you play [TicTacToe](https://en.wikipedia.org/wiki/Tic-tac-toe) against your Reachy. Reachy plays with the cylinder pawn and you play with the square ones. 

The demo has been made to be fully autonomous. So, Reachy decides who goes first (it's actually random) and let you know: it will either point to itself or point to you.

Reachy will detects when you place a pawn on the board and automatically plays in response. The game will 
last until one the player wins or if all pawns have been played. The robot will then react to the end of the game, differently whether it wins or loses.
Then, it waits for the board to be cleaned and automatically restarts a new game.

<!-- TODO: full game video -->

## Key technical aspects

This demonstration emphasises several key aspects of the robot:

* It involves complex motions that have been defined, saved and can be re-played (grasping a pawn, place it on a pre-defined location).
* It uses the camera, vision and machine learning for recognizing the board and analyzing its configuration.
* It also relies on a simple machine learning algorithm for correctly playing the game itself (what next move should I make).

