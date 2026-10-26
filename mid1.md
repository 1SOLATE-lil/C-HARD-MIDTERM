
#include "raylib.h"
#include <stdbool.h>

#define WIDTH 800
#define HEIGHT 600
#define ROWS 7
#define COLS 10

int main(void)
{
    InitWindow(WIDTH, HEIGHT, "Rainbow Block Breaker");
    SetTargetFPS(60);

    Rectangle paddle = { 350, 540, 100, 15 };
    Vector2 ball = { 400, 400 };
    Vector2 speed = { 4, -4 };

    Rectangle blocks[ROWS][COLS];
    bool alive[ROWS][COLS];

    Color colors[ROWS] = {
        RED, ORANGE, YELLOW, GREEN,
        BLUE, (Color) { 75, 0, 130, 255 }, VIOLET
    };

    int score = 0;
    int lives = 3;
    bool gameOver = false;
    bool win = false;

    for (int r = 0; r < ROWS; r++) {
        for (int c = 0; c < COLS; c++) {
            blocks[r][c] = (Rectangle){
                40 + c * 72, 70 + r * 30, 66, 23
            };
            alive[r][c] = true;
        }
    }

    while (!WindowShouldClose()) {
        if (!gameOver && !win) {
            if (IsKeyDown(KEY_LEFT))
                paddle.x -= 7;
            if (IsKeyDown(KEY_RIGHT))
                paddle.x += 7;

            if (paddle.x < 0)
                paddle.x = 0;
            if (paddle.x + paddle.width > WIDTH)
                paddle.x = WIDTH - paddle.width;

            ball.x += speed.x;
            ball.y += speed.y;

            if (ball.x < 8 || ball.x > WIDTH - 8)
                speed.x *= -1;
            if (ball.y < 8)
                speed.y *= -1;

            if (CheckCollisionCircleRec(ball, 8, paddle)
                && speed.y > 0) {
                speed.y = -4;
                speed.x = (ball.x - (paddle.x + paddle.width / 2)) / 12;
            }

            for (int r = 0; r < ROWS; r++) {
                for (int c = 0; c < COLS; c++) {
                    if (alive[r][c] &&
                        CheckCollisionCircleRec(ball, 8, blocks[r][c])) {
                        alive[r][c] = false;
                        speed.y *= -1;
                        score += 10;
                    }
                }
            }

            if (ball.y > HEIGHT) {
                lives--;
                ball = (Vector2){ 400, 400 };
                speed = (Vector2){ 4, -4 };

                if (lives <= 0)
                    gameOver = true;
            }

            win = true;
            for (int r = 0; r < ROWS; r++) {
                for (int c = 0; c < COLS; c++) {
                    if (alive[r][c])
                        win = false;
                }
            }
        }

        if ((gameOver || win) && IsKeyPressed(KEY_R)) {
            score = 0;
            lives = 3;
            gameOver = false;
            win = false;
            ball = (Vector2){ 400, 400 };
            speed = (Vector2){ 4, -4 };
            paddle.x = 350;

            for (int r = 0; r < ROWS; r++)
                for (int c = 0; c < COLS; c++)
                    alive[r][c] = true;
        }

        BeginDrawing();
        ClearBackground((Color) { 18, 18, 35, 255 });

        DrawText("RAINBOW BLOCK BREAKER", 225, 15, 25, RAYWHITE);
        DrawText(TextFormat("SCORE: %d", score), 20, 20, 20, GOLD);
        DrawText(TextFormat("LIVES: %d", lives), 670, 20, 20, PINK);

        for (int r = 0; r < ROWS; r++) {
            for (int c = 0; c < COLS; c++) {
                if (alive[r][c]) {
                    DrawRectangleRec(blocks[r][c], colors[r]);
                    DrawRectangleLinesEx(blocks[r][c], 1, RAYWHITE);
                }
            }
        }

        DrawRectangleRec(paddle, SKYBLUE);
        DrawCircleV(ball, 8, WHITE);

        if (gameOver) {
            DrawText("GAME OVER", 285, 260, 40, RED);
            DrawText("Press R to Restart", 295, 315, 20, RAYWHITE);
        }

        if (win) {
            DrawText("YOU WIN!", 315, 260, 40, GOLD);
            DrawText("Press R to Restart", 295, 315, 20, RAYWHITE);
        }

        DrawText("Move: LEFT / RIGHT", 20, 570, 18, LIGHTGRAY);
        EndDrawing();
    }

    CloseWindow();
    return 0;
}
