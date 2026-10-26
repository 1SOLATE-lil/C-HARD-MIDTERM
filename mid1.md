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

int main(void)
{
    InitWindow(SCREEN_W, SCREEN_H, "Rainbow Breakout");
    SetTargetFPS(60);

    Block blocks[ROWS][COLS];

    Color rainbow[] = {
        RED, ORANGE, YELLOW,
        GREEN, SKYBLUE, PURPLE
    };

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

    Rectangle paddle = { 375, 630, 150, 18 };
    Vector2 ball = { 450, 580 };
    Vector2 speed = { 4, -4 };

    int score = 0;
    int lives = 3;
    int remaining = ROWS * COLS;
    int started = 0;
    int gameOver = 0;
    int win = 0;

    while (!WindowShouldClose()) {
        float dt = GetFrameTime();

        if (IsKeyPressed(KEY_R) && (gameOver || win)) {
            score = 0;
            lives = 3;
            remaining = ROWS * COLS;
            started = 0;
            gameOver = 0;
            win = 0;

            paddle.x = 375;
            ball = (Vector2){ 450, 580 };
            speed = (Vector2){ 4, -4 };

            for (int r = 0; r < ROWS; r++) {
                for (int c = 0; c < COLS; c++) {
                    blocks[r][c].active = 1;
                }
            }
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

                if (ball.x < 8 || ball.x > SCREEN_W - 8)
                    speed.x *= -1;

                if (ball.y < 8)
                    speed.y *= -1;

                if (CheckCollisionCircleRec(ball, 8, paddle)
                    && speed.y > 0) {
                    speed.y = -fabsf(speed.y);

                    speed.x = (ball.x -
                        (paddle.x + paddle.width / 2)) * 0.08f;

                    if (fabsf(speed.x) < 2)
                        speed.x = speed.x < 0 ? -2 : 2;
                }

                for (int r = 0; r < ROWS; r++) {
                    for (int c = 0; c < COLS; c++) {
                        if (blocks[r][c].active &&
                            CheckCollisionCircleRec(
                                ball, 8, blocks[r][c].rect)) {
                            blocks[r][c].active = 0;
                            speed.y *= -1;
                            score += 10;
                            remaining--;
                            break;
                        }
                    }
                }

                if (ball.y > SCREEN_H) {
                    lives--;
                    started = 0;
                    ball = (Vector2){ 450, 580 };
                    speed = (Vector2){ 4, -4 };

                    if (lives <= 0)
                        gameOver = 1;
                }

                if (remaining <= 0)
                    win = 1;
            }
        }

        BeginDrawing();
        ClearBackground((Color){ 15, 17, 35, 255 });

        DrawText("RAINBOW BREAKOUT", 280, 25, 30, RAYWHITE);
        DrawText(TextFormat("SCORE: %d", score),
            25, 65, 20, WHITE);
        DrawText(TextFormat("LIVES: %d", lives),
            760, 65, 20, WHITE);

        for (int r = 0; r < ROWS; r++) {
            for (int c = 0; c < COLS; c++) {
                if (blocks[r][c].active) {
                    DrawRectangleRounded(
                        blocks[r][c].rect,
                        0.25f, 6, blocks[r][c].color);

                    DrawRectangleLinesEx(
                        blocks[r][c].rect,
                        1, RAYWHITE);
                }
            }
        }

        DrawRectangleRounded(paddle, 0.7f, 8, WHITE);
        DrawCircleV(ball, 8, GOLD);

        if (!started && !gameOver && !win)
            DrawText("PRESS SPACE TO START",
                300, 450, 24, RAYWHITE);

        if (gameOver) {
            DrawRectangle(0, 0, SCREEN_W, SCREEN_H,
                (Color){ 0, 0, 0, 190 });
            DrawText("GAME OVER", 310, 280, 50, RED);
            DrawText(TextFormat("FINAL SCORE: %d", score),
                330, 350, 25, WHITE);
            DrawText("PRESS R TO RESTART",
                310, 410, 22, RAYWHITE);
        }

        if (win) {
            DrawRectangle(0, 0, SCREEN_W, SCREEN_H,
                (Color){ 0, 0, 0, 190 });
            DrawText("RAINBOW CLEARED!",
                225, 280, 45, GOLD);
            DrawText(TextFormat("FINAL SCORE: %d", score),
                330, 350, 25, WHITE);
            DrawText("PRESS R TO RESTART",
                310, 410, 22, RAYWHITE);
        }

        EndDrawing();
    }

    CloseWindow();
    return 0;
}
