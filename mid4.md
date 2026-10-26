# 피라미드형태의 문자 출력  
홀수는 *, 짝수는 #  
# 코드  
```
#include <stdio.h>

int main() {
    int n;
    scanf_s("%d", &n);

    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < n - i; j++)
            printf(" ");

        for (int j = 0; j < 2 * i - 1; j++)
            printf(i % 2 ? "*" : "#");

        printf("\n");
    }
}
```
