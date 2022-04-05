---
layout: projects
categories: project
title: "Exploration 02: Conway's Game of Life"
description: "Implementing Conway's famous Game of Life onto my 16x16 LED Matrix"
fontawesome: true
---

The rules are simple. The results are implicative of the emergent behavior resulting from an elementary ruleset. This algorithm, who is analagous to life itself, dictates the following action for each cell in the 256 body grid of my 16x16 display:
1. Overpopulation: A cell who has 4 or more neighbors dies from hunger.
2. Loneliness: A cell who has 1 or fewer neighbors dies from seclusion.
3. Reproduction: A cell who has exactly three neighbors thrives in their community.
4. Stasis: All other cells continue to the next day.


These simple rules display mesmerizing patterns from random initial conditions with one thing in common between generations -- the inevitable reset of the board to start again.

## Demo
You can see the game in action at this [YouTube Video](https://youtube.com/shorts/en1mnJsYW9g?feature=share){:target="_blank"}.

## Overview
Extending the same Animation class that I've used for all of my matrix implementations, the `GameOfLife` class takes care of calculating the board state for every generation, as well as detecting boards that die out completely or get stuck in periodic patterns. Each board state is calculated in the `void generate()` function. The board is iterated with a nested `for` loop and each cell and a count of its neighbors is tallied and stored for the remainder of the loop cycle. Then, each of the four game conditions are checked against the count and the resulting state for that cell is logged in another matrix.

For each instance of a cell changing state, the LifeDeltas variable is increased. For example, if there were one cell left on the board, in the next state they would die and LifeDeltas would be set to 1. If the number of toggles becomes periodic over `DELTA_GENS` samples (8), the game is deemed to be in an infinite loop and the board is reset to new random initial conditions.

The full code is given below:

```cpp
#pragma once
#define DELTA_GENS 8

class GameOfLife : public Animation
{
public:
  using Animation::Animation;

  int board[MATRIX_WIDTH][MATRIX_HEIGHT] = {};
  int lifeCount = 0;                    // Number of cells alive on the board
  int lifeDeltas[DELTA_GENS] = {0};     // A rolling count of the DELTA_GENS most recent deltas.
  int generations = 0;                  // Number of generations a board survived

  void reset() {
    wipeAnimation();

    generations = 0;
    for(int x = 0; x < MATRIX_WIDTH; x++) {
      for(int y = 0; y < MATRIX_HEIGHT; y++) {
        board[x][y] = bool(random(2));
      }
    }
    for(int i = 0; i < DELTA_GENS; i++){
      lifeDeltas[i] = 0;
    }

    renderBoard();

    // fade in at new game 
    for(uint8_t b=5; b <= Config.brightness; b++) {
      FastLED.setBrightness(b);
      FastLED.show();
      delay(10);
    }

    delay(750);  //show the initial conditions for longer duration.
  }

  void loop() {
    generate();
    renderBoard();

    generations++;
    if((lifeCount == 0 || lifeDeltaPass()) && generations > 1 || generations > 500){
      delay(1200);
      reset();
    }
    FastLED.setBrightness(Config.brightness);
    delay(100);
  }

  // The game of life:
  // Overpopulation: 4 or more cells around us are alive.
  // Lonliness: 1 or fewer live neighbors.
  // Birth: Exactly 3 live neighbors.
  // Stasis: All other cases, state remains the same.
  void generate() {
    int next[MATRIX_HEIGHT][MATRIX_WIDTH] = {0};
    int lifeDelta = 0;

    for(int x = 1; x < MATRIX_WIDTH-1; x++) {
      for(int y = 1; y < MATRIX_HEIGHT-1; y++) {

        int neighbors = 0;
        for(int i = -1; i <= 1; i++) {
          for(int j = -1; j <= 1; j++) {
            neighbors += board[x+i][y+j];
          }
        }
        //subtract self (included above)
        neighbors -= board[x][y];
        
        //GOL Rules:
        if(board[x][y] == 1 && neighbors >= 4) {
          next[x][y] = 0;
          lifeDelta++;
        }
        else if(board[x][y] == 1 && neighbors <= 1) {
          next[x][y] = 0;
          lifeDelta++;
        }
        else if(board[x][y] == 0 && neighbors == 3) {
          next[x][y] = 1;
          lifeDelta++;
        }
        else {
          next[x][y] = board[x][y];
        }

      }
    }
    addDelta(lifeDelta);

    lifeCount = 0;
    for(int x = 0; x < MATRIX_WIDTH; x++) {
      for(int y = 0; y < MATRIX_HEIGHT; y++) {
        board[x][y] = next[x][y];
        if(next[x][y] == 1)
          lifeCount++;
      }
    }
  }


  void addDelta(int del) {
    for (int i = DELTA_GENS-1; i > 0; i--) {
      lifeDeltas[i] = lifeDeltas[i-1];
    }
    lifeDeltas[0] = del;
  }

  bool lifeDeltaPass() const {
    int first = lifeDeltas[0];

    //check all deltas, if there are any unequal to the first, return the 1.
    //otherwise, we know they are all the same. return 0:
    //-->no movement has occurred in DELTA_GENS generations
    for (int i = 0; i < DELTA_GENS; i++) {
      if(lifeDeltas[i] != first)
        return false;
    }
    return true;
  }

  // ------------------------------------------------
  // ANIMATION HELPERS --- NOT CRUCIAL FOR GAME LOGIC
  // ------------------------------------------------
  void renderBoard() const {
    for (int x = 0; x < MATRIX_WIDTH; x++) {
      for (int y = 0; y < MATRIX_HEIGHT; y++) {
        leds[topoXY(x, y)] = board[x][y] == 1 ? CRGB(255, 255, 255) : CRGB(0, 0, 0);
      }
    }
    FastLED.show();
  }

  // horizontal wipe
  void wipeAnimation() {
    for(int i = 0; i < MATRIX_WIDTH/2; i++) {
      for(int j = 0; j < MATRIX_HEIGHT; j++) {
        board[i][j] = 1;
        board[MATRIX_WIDTH-1-i][j] = 1;
      }
      renderBoard();
      delay(60);
    }
    delay(100);
    for(int i = 0; i < 25; i++) {
      fadeToBlackBy(leds, NUM_LEDS, 25);
      FastLED.show();
      delay(15);
    }
    betterClear();
    delay(750);
  }

 
  void resetBoard() {
    for(int x = 0; x < MATRIX_WIDTH; x++) {
      for(int y = 0; y < MATRIX_HEIGHT; y++) {
        board[x][y] = 0;
      }
    }
  }
};
```