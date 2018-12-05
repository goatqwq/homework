任务1：会动的蛇

```
#include<stdio.h>
#include<stdlib.h>           
#include<time.h>

#define SNAKE_MAX_LENGTH 20
#define SNAKE_HEAD 'H'
#define SNAKE_BODY 'X'
#define BLANK_CELL ' '
#define SNAKE_FOOD '$'
#define WALL_CELL '*'

int GG=0;
char map[12][12]={
    {"***********"},
    {"*XXXXH    *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"*         *"},
    {"***********"}
};
int snakeLength=5;
int snake_X[SNAKE_MAX_LENGTH] = {1,1,1,1,1};
int snake_Y[SNAKE_MAX_LENGTH] = {1,2,3,4,5};

void Output(void){/*输出*/
    int i;
	system("cls");//清屏
    for(i=0;i<12;i++){
        printf("%s\n",map[i]);
    }
}

void Gameover(void){/*游戏结束*/
    char map_GG[12][12]={
        "***********",
        "*         *",
        "*         *",
        "*         *",
        "* G a m e *",
        "*         *",
        "* O v e r *",
        "*         *",
        "*         *",
        "*         *",
        "*         *",
        "***********"
    };    
    system("cls");
    int i;
    for(i=0;i<12;i++){
        printf("%s\n",map_GG[i]);
    }
	system("pause");
}
void BodyMove(int snakelen){
    int i;
    map[snake_X[0]][snake_Y[0]]=' ';/*将蛇尾消失*/
    for(i = 0;i < snakelen - 1;++i){
        snake_X[i]=snake_X[i + 1];
        snake_Y[i]=snake_Y[i + 1];
    }
}
void HeadMove(int dy,int dx,int snakelen){
    snake_X[snakelen - 1] += dx;
    snake_Y[snakelen -1] += dy;
    map[snake_X[snakelen - 2]][snake_Y[snakelen - 2]] = SNAKE_BODY;
    map[snake_X[snakelen - 1]][snake_Y[snakelen - 1]] = SNAKE_HEAD;
}
void Snake_Move(int snakelen){
    char direct=getchar();
    switch(direct){
        case 'a':
        case 'A':if(map[snake_X[snakelen - 1]][snake_Y[snakelen - 1] - 1] == ' '){/*判断条件：头要转向的方向是安全的（没有身体没有墙）*/
            int dy = -1,dx = 0;
			BodyMove(snakelen);
            HeadMove(dy,dx,snakelen);
        }
		else{
			GG = 1;
		}break;
        case 'w':
        case 'W':if(map[snake_X[snakelen - 1] - 1][snake_Y[snakelen - 1]] == ' '){
            BodyMove(snakelen);
            int dy = 0,dx = -1;
            HeadMove(dy,dx,snakelen);
        }
		else{
			GG = 1;
		}break;
        case 's':
        case 'S':if(map[snake_X[snakelen - 1] + 1][snake_Y[snakelen - 1]] == ' '){
            BodyMove(snakelen);
            int dy = 0,dx = 1;
            HeadMove(dy,dx,snakelen);
        }
		else{
			GG = 1;
		}break;
        case 'd':
        case 'D':if(map[snake_X[snakelen - 1]][snake_Y[snakelen - 1] + 1] == ' '){
            BodyMove(snakelen);
            int dy = 1,dx = 0;
            HeadMove(dy,dx,snakelen);
        }
		else{
			GG = 1;
		}break;
        default:break;
    }
}
int main(void){
    while(GG != 1){
        Output();
        Snake_Move(snakeLength);
    }
    Gameover();
}
```

任务2：会吃的蛇

```

```