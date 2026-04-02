# C언어로 된 코드를 C++로 바꾸기(바이브코딩)
#### C언어로 된 테트릭스 게임을 C++ Class로 변경하시오.
#### 베이스 코드: https://github.com/raysan5/raylib-games/blob/master/classics/src/tetris.c

```c++
#include "raylib.h"
#include <ctime>
#include <cstdlib>

#define SQUARE_SIZE 20
#define GRID_W 12
#define GRID_H 20

static const int SHAPES[7][4][4] = {
    {{0,0,0,0},{1,1,1,1},{0,0,0,0},{0,0,0,0}},
    {{0,1,1,0},{0,1,1,0},{0,0,0,0},{0,0,0,0}},
    {{0,1,0,0},{1,1,1,0},{0,0,0,0},{0,0,0,0}},
    {{1,0,0,0},{1,1,1,0},{0,0,0,0},{0,0,0,0}},
    {{0,0,1,0},{1,1,1,0},{0,0,0,0},{0,0,0,0}},
    {{0,1,1,0},{1,1,0,0},{0,0,0,0},{0,0,0,0}},
    {{1,1,0,0},{0,1,1,0},{0,0,0,0},{0,0,0,0}}
};

class Tetris {
private:
    int grid[GRID_W][GRID_H];
    int piece[4][4];
    int nextPiece[4][4];
    int px, py;
    int score;
    bool gameOver;

    int fallCounter;
    int fallSpeed;

public:
    Tetris() { srand(time(NULL)); Init(); }

    void CopyShape(int dest[4][4], int idx) {
        for (int i = 0; i < 4; i++)
            for (int j = 0; j < 4; j++)
                dest[i][j] = SHAPES[idx][j][i];
    }

    void Init() {
        score = 0;
        gameOver = false;
        fallCounter = 0;
        fallSpeed = 30;

        for (int i = 0; i < GRID_W; i++)
            for (int j = 0; j < GRID_H; j++)
                grid[i][j] = (i == 0 || i == GRID_W - 1 || j == GRID_H - 1) ? 2 : 0;

        CopyShape(nextPiece, rand() % 7);
        SpawnPiece();
    }

    void SpawnPiece() {
        px = GRID_W / 2 - 2;
        py = 0;

        for (int i = 0; i < 4; i++)
            for (int j = 0; j < 4; j++)
                piece[i][j] = nextPiece[i][j];

        CopyShape(nextPiece, rand() % 7);
    }

    bool CheckCollision(int nx, int ny, int test[4][4]) {
        for (int i = 0; i < 4; i++)
            for (int j = 0; j < 4; j++)
                if (test[i][j] && grid[nx + i][ny + j] != 0)
                    return true;
        return false;
    }

    void MergePiece() {
        for (int i = 0; i < 4; i++)
            for (int j = 0; j < 4; j++)
                if (piece[i][j]) grid[px + i][py + j] = 1;
    }

    void ClearLines() {
        for (int j = 0; j < GRID_H - 1; j++) {
            bool full = true;
            for (int i = 1; i < GRID_W - 1; i++)
                if (grid[i][j] == 0) full = false;

            if (full) {
                score += 100;
                for (int y = j; y > 0; y--)
                    for (int x = 1; x < GRID_W - 1; x++)
                        grid[x][y] = grid[x][y - 1];
            }
        }
    }

    void Rotate() {
        int temp[4][4] = { 0 };
        for (int i = 0; i < 4; i++)
            for (int j = 0; j < 4; j++)
                temp[i][j] = piece[3 - j][i];

        if (!CheckCollision(px, py, temp)) {
            for (int i = 0; i < 4; i++)
                for (int j = 0; j < 4; j++)
                    piece[i][j] = temp[i][j];
        }
    }

    int GetGhostY() {
        int gy = py;
        while (!CheckCollision(px, gy + 1, piece)) gy++;
        return gy;
    }

    void Update() {
        if (gameOver) return;

        if (IsKeyPressed(KEY_LEFT) && !CheckCollision(px - 1, py, piece)) px--;
        if (IsKeyPressed(KEY_RIGHT) && !CheckCollision(px + 1, py, piece)) px++;
        if (IsKeyPressed(KEY_UP)) Rotate();

        fallSpeed = IsKeyDown(KEY_DOWN) ? 5 : 30;

        fallCounter++;

        if (fallCounter >= fallSpeed) {
            if (!CheckCollision(px, py + 1, piece)) py++;
            else {
                MergePiece();
                ClearLines();
                SpawnPiece();
                if (CheckCollision(px, py, piece)) gameOver = true;
            }
            fallCounter = 0;
        }
    }

    void Draw() {
        int offsetX = 200;
        int offsetY = 50;

        for (int i = 0; i < GRID_W; i++)
            for (int j = 0; j < GRID_H; j++) {
                if (grid[i][j] == 1)
                    DrawRectangle(offsetX + i * SQUARE_SIZE, offsetY + j * SQUARE_SIZE, SQUARE_SIZE, SQUARE_SIZE, GRAY);
                else if (grid[i][j] == 2)
                    DrawRectangle(offsetX + i * SQUARE_SIZE, offsetY + j * SQUARE_SIZE, SQUARE_SIZE, SQUARE_SIZE, LIGHTGRAY);
            }

        int gy = GetGhostY();
        for (int i = 0; i < 4; i++)
            for (int j = 0; j < 4; j++)
                if (piece[i][j])
                    DrawRectangleLines(offsetX + (px + i) * SQUARE_SIZE,
                        offsetY + (gy + j) * SQUARE_SIZE,
                        SQUARE_SIZE, SQUARE_SIZE, LIGHTGRAY);

        for (int i = 0; i < 4; i++)
            for (int j = 0; j < 4; j++)
                if (piece[i][j])
                    DrawRectangle(offsetX + (px + i) * SQUARE_SIZE,
                        offsetY + (py + j) * SQUARE_SIZE,
                        SQUARE_SIZE, SQUARE_SIZE, DARKGRAY);

        // NEXT
        DrawText("NEXT", 520, 50, 20, BLACK);
        for (int i = 0; i < 4; i++)
            for (int j = 0; j < 4; j++)
                if (nextPiece[i][j])
                    DrawRectangle(520 + i * SQUARE_SIZE,
                        80 + j * SQUARE_SIZE,
                        SQUARE_SIZE, SQUARE_SIZE, DARKGRAY);

        DrawText(TextFormat("SCORE: %d", score), 500, 200, 20, BLACK);

        if (gameOver)
            DrawText("GAME OVER", 300, 200, 40, RED);
    }
};

int main() {
    InitWindow(800, 450, "Tetris");
    SetTargetFPS(60);

    Tetris game;

    while (!WindowShouldClose()) {
        BeginDrawing();
        ClearBackground(RAYWHITE);

        game.Update();
        game.Draw();

        EndDrawing();
    }

    CloseWindow();
}
```
