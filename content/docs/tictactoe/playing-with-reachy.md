---
weight: 520
title: "Playing With Reachy"
---

# Playing with Reachy

## Unwrapping the demo

<!-- **TODO: diagram** -->

## Controlling the demo

As you can see on the diagram, the demo runs in a fully autonomous way. Yet, they are a few way you can control and interact with it.

* First, the robot will start a game only once the board is cleared. It is up to you to reset the board position and to put back the pawn to their base positions. The robot will only start playing when it's done. When a game is over, a new one is directly restarted. So, at the end of a game clean up the board, and a new game will begin.

* Then, if something weird happen during a game (like someone cheating, the detection was wrong and so we don't know our current state anymore, etc.), the robot will reset the game. It will perform a shuffle move, where Reachy will overthrow all pawns present on the board. It will then wait for a new game to begin, ie when the board is cleaned again. You can use this behavior to reset the game whenever you want. Simply starts cleaning the board, the robot will be lost, do its shuffle move and start a new game.




