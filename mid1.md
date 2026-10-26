#include "raylib.h"
#include <math.h>

#define SCREEN_W 900
#define SCREEN_H 700
#define ROWS 6
#define COLS 10
#define BLOCK_W 78
#define BLOCK_H 25
#define BLOCK_GAP 7

typedef struct {
    Rectangle rect;
    Color color;
    int active;
} Block;

void ResetGame(Block blocks[ROWS][COLS], Rectangle *paddle,
               Vector2 *ball, Vector2 *speed, int *score,
               int *lives, int *remaining, int *started,
               int *gameOver, int *win)
{
    Color rainbow[ROWS] = { RED, ORANGE, YELLOW, GREEN, SKYBLUE, PURPLE };

    for (int r = 0; r < ROWS; r++) {
        for (int c = 0; c < COLS; c++) {
            blocks[r][c].rect = (Rectangle){
                25 + c * (BLOCK_W + BLOCK_GAP),
                100 + r * (BLOCK_H + BLOCK_GAP),
                BLOCK_W, BLOCK_H
            };
            blocks[r][c].color = rainbow[r];
            blocks[r][c].active = 1;
        }
    }

    *paddle = (Rectangle){ 375, 630, 150, 18 };
    *ball = (Vector2){ 450, 580 };
    *speed = (Vector2){ 4, -4 };
    *score = 0;
    *lives = 3;
    *remaining = ROWS * COLS;
    *started = 0;
    *gameOver = 0;
    *win = 0;
}

int main(void)
{
    InitWindow(SCREEN_W, SCREEN_H, "Rainbow Breakout");
    SetTargetFPS(60);

    Block blocks[ROWS][COLS];
    Rectangle paddle;
    Vector2 ball, speed;
    int score, lives, remaining, started, gameOver, win;

    ResetGame(blocks, &paddle, &ball, &speed, &score, &lives,
              &remaining, &started, &gameOver, &win);

    while (!WindowShouldClose()) {
        float dt = GetFrameTime();

        if (IsKeyPressed(KEY_R) && (gameOver || win)) {
            ResetGame(blocks, &paddle, &ball, &speed, &score, &lives,
                      &remaining, &started, &gameOver, &win);
        }

        if (!gameOver && !win) {
            if (IsKeyDown(KEY_LEFT) || IsKeyDown(KEY_A))
                paddle.x -= 420 * dt;
            if (IsKeyDown(KEY_RIGHT) || IsKeyDown(KEY_D))
                paddle.x += 420 * dt;

            if (paddle.x < 0)
                paddle.x = 0;
            if (paddle.x + paddle.width > SCREEN_W)
                paddle.x = SCREEN_W - paddle.width;

            if (IsKeyPressed(KEY_SPACE))
                started = 1;

            if (started) {
                ball.x += speed.x;
                ball.y += speed.y;

                if (ball.x < 8) {
                    ball.x = 8;
                    speed.x = fabsf(speed.x);
                }
                if (ball.x > SCREEN_W - 8) {
                    ball.x = SCREEN_W - 8;
                    speed.x = -fabsf(speed.x);
                }
                if (ball.y < 8) {
                    ball.y = 8;
