# data-alignment-

data alignment 的意思就是， data 的 address 可以公平的被 1, 2, 4, 8,這些數字整除
這些數字可以發現他們都是 2 的冪 (power of 2)，亦即這些 data object 可以用 1, 2, 4, 8 byte 去 alignment。


資料位址不在 4 的倍數，會導致存取速度降低，編譯器在分配記憶體時，會按照宣告的型態去做 alignment。


struct s1 {
    char c;
    int a;
}
原本推論 char 為 1 byte，而 int 為 4 byte ，兩個加起來應該為 5 byte
然而實際上為 8 byte，由於 int 為 4 byte ，所以 char 為了要能 alignment 4 byte 就多加了 3 byte 進去 ，使得 cpu 存取速度不會因 address 不是在 4 的倍數上，存取變慢。



如果可以 可以排列整齊一下
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
typedef struct _s1 {
    char a;
    char b;
    int c;
    char d;
    char e;
} s1;
typedef struct _s2 {
    int a;
    char b;
    char c;
    char d;
    char e;
} s2;
int main() {
    printf("struct s1 size: %ld byte\n", sizeof(s1)); // 12
    printf("struct s1 size: %ld byte\n", sizeof(s2)); // 8
}




如果要取消 alignment

放__attribute__((packed))
有不同的放置位子
https://blog.csdn.net/weixin_35933684/article/details/100706328

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
typedef struct __attribute__((packed)) _s1 {
    char a;
    char b;
    int c;
    char d;
    char e;
} s1;
int main() {
    printf("struct s1 size: %ld byte\n", sizeof(s1));
}


更進階的＠＠

https://hackmd.io/@sysprog/c-memory
