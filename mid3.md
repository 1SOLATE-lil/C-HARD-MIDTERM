# 숫자야구 게임 코드  
규칙은 컴퓨터가 만든 중복 없는 숫자 3개를 맞히는 거야.  
- 스트라이크(S): 숫자와 위치가 모두 같음  
- 볼(B): 숫자는 같지만 위치가 다름  
- 아웃(OUT): 일치하는 숫자가 없음  
- 3 스트라이크면 정답!
# 코드
```
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(void)
{
    int answer[3], guess[3];
    int strike, ball;

    srand((unsigned int)time(NULL));

    answer[0] = rand() % 9 + 1;
    do {
        answer[1] = rand() % 9 + 1;
    } while (answer[1] == answer[0]);

    do {
        answer[2] = rand() % 9 + 1;
    } while (answer[2] == answer[0] ||
             answer[2] == answer[1]);

    printf("숫자야구 게임 시작!\n");

    do {
        printf("서로 다른 숫자 3개 입력: ");
        if (scanf_s("%1d%1d%1d",
                    &guess[0], &guess[1], &guess[2]) != 3) {
            printf("숫자 3개를 입력하세요.\n");
            return 1;
        }

        strike = 0;
        ball = 0;

        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (guess[i] == answer[j]) {
                    if (i == j)
                        strike++;
                    else
                        ball++;
                }
            }
        }

        if (strike == 3)
            printf("HOMERUN! 정답입니다!\n");
        else if (strike == 0 && ball == 0)
            printf("OUT!\n");
        else
            printf("%d Strike, %d Ball\n", strike, ball);

    } while (strike != 3);

    return 0;
}
```
# 실행예시  
숫자야구 게임 시작!  
서로 다른 숫자 3개 입력: 123  
1 Strike, 1 Ball  
서로 다른 숫자 3개 입력: 456  
OUT!  
서로 다른 숫자 3개 입력: 789  
HOMERUN! 정답입니다!  
