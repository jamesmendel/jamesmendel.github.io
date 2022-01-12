---
layout: projects
categories: project
title: "Porting Pong to my Matrix"
description: "Bringing the classic video game to my 16x16 display"
fontawesome: true
---

This is one of the first games I wrote from scratch. I wrote this before I had taken an in-depth class on object oriented programming, so its not the prettiest (although not too bad at under 200 lines!), but I wanted to leverage object oriented programming to create a simple game board with two paddles and a ball with self-contained logic.

## Demo
You can see the game in action at this [YouTube Video](https://youtu.be/23wG-5Td9DA){:target="_blank"}.

## Overview
There are three main sections:
- Paddle class
- Ball class
- Game loop

In the game loop, there is a check to see if the board needs reinitializing. If so, 2 paddle and 1 ball instances are created with preset initial conditions. The ball has is spawned in the center of the board with randomized `x` and `y` velocity components. Collisions are checked, and if one is found, the balls x component gets flipped. If the ball's posistion is outside of the game board, it resets. 

The two paddles are both controlled with simple logic. When the ball is on a paddle's half of the board, it will move at a maximum rate of 1 unit per frame towards the ball's current `y` coordinate.

## Full Code
Heres the full game in C++:
```cpp
class Pong : public Animation
{
public:
    using Animation::Animation;

    struct Paddle;
    struct Ball;
    bool game_init = false;

    struct Paddle
    {
        uint8_t x;
        uint8_t y;
        uint8_t height;
        CRGB color;
        void draw(){
            for(uint8_t i = 0; i < this->height; i++){
                if(i == 0){
                    leds[topoXY(this->x, this->y)] = this->color;
                }
                else if(i % 2 == 1){
                    leds[topoXY(this->x, this->y + i)] = this->color;
                }
                else{
                    leds[topoXY(this->x, this->y - (i-1) )] = this->color;
                }
            }
        }
        // dir  true->up, 
        //      false->down
        void move(bool dir){
            if(dir && this->y + this->height/2 < MATRIX_HEIGHT-1)
                this->y++;
            if(!dir && this->y - this->height/2 > 0)
                this->y--;
        }
        void cpuMove(Ball *ball){
            if(this->y > (int)ball->y)
                this->move(0);
            else if (this->y < (int)ball->y)
                this->move(1);
            else
                return;
        }
    };
    struct Ball
    {
        float x;
        float y;
        float vx;
        float vy;
        // Ball color
        CRGB color;
        void draw(){
            int _x = this->x;
            int _y = this->y;
            leds[topoXY(_x, _y)] = this->color;
        }
        void move(){
            this->x = this->x + this->vx;
            this->y = this->y + this->vy;
        }
        float random(){
            return ((random8(0,255)-127.0f) / 100.0f);
        }

        // Returns true if ball coordinates intersect with a paddle.
        bool checkCollision(Paddle *p){
            //if position is on right half of screen
            if(p->x < MATRIX_WIDTH/2){
                if(this->x == p->x + 1 && ((this->y >= p->y - p->height/2) && (this->y <= p->y + p->height/2)))
                {
                    this->color = p->color;
                    return true;
                }
            }
            else
            {
                if(this->x == p->x - 1 && ((this->y >= p->y - p->height/2) && (this->y <= p->y + p->height/2)))
                {
                    this->color = p->color;
                    return true;
                }
            }
            return false;
        }
        // returns false if ball position needs to be reset
        // returns true if ball needs to bounce to remain on playfield
        bool checkBounds(){
            if(this->x < 1 || this->x >= MATRIX_WIDTH-1.0f)
                return false;
            if(this->y < 1 || this->y >= MATRIX_HEIGHT-1.0f)
                this->vy = this->vy*-1;
                return true;
        }
        void bounce(){
            this->vx = this->vx * -1;
        }
    };
    Paddle p1, p2, p3;
    Ball ball;

    void reset()
    {
        game_init = false;
        ball.x = MATRIX_WIDTH/2;
        ball.y = MATRIX_HEIGHT/2;
        ball.vy = ball.random();
        Serial.println("PONG: Reset");
        // your standard reset function
    }

    void gameLoop()
    {
        // initialize paddles and ball
        if(!game_init){
            p1 = {0, MATRIX_HEIGHT/2, 3, CRGB(255, 128, 192)};
            p2 = {MATRIX_WIDTH - 1, MATRIX_HEIGHT/2, 3, CRGB(0, 128, 128)};
            ball = {MATRIX_WIDTH/2.0f, MATRIX_HEIGHT/2.0f, -1.0f, ball.random(), CRGB(255,255,255)};
            game_init = true;   
        }

        //'cpu' control
        if(ball.x < MATRIX_WIDTH/2){
            p1.cpuMove(&ball);
        } else{
            if(p1.y != MATRIX_WIDTH/2){
                p1.move(p1.y - MATRIX_WIDTH/2 < 0 ? 1:0);
            }
        }
        if(ball.x >= MATRIX_WIDTH/2)
            p2.cpuMove(&ball);
        else{
            if(p2.y != MATRIX_WIDTH/2){
                p2.move(p2.y - MATRIX_WIDTH/2 < 0 ? 1:0);
            }
        }
        //ball collision
        if (ball.checkCollision(&p1)){
            ball.bounce();
            Serial.println("PING");
        }
        if (ball.checkCollision(&p2)){
            ball.bounce();
            Serial.println("PONG");
        }
        //update ball position
        ball.move();

        if(!ball.checkBounds())
            reset();
        //draw objects
        p1.draw();
        p2.draw();
        ball.draw();

        Serial.printf("BALL: x=%f, y=%f, vx=%f vy=%f\n",ball.x,ball.y,ball.vx,ball.vy);

    }

    void loop()
    {
        for(uint16_t i = 0; i < NUM_LEDS; i++)
        {
            leds[i] = CRGB::Black;
        }
        gameLoop();
        FastLED.show();
        delay(50);
    }
};
```