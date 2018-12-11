# 智能蛇游戏设计
## 1.设计思路
确定蛇头移动一步后不是墙或蛇身，这之后从中选出距离钱最短的作为下一步移动的方向
## 2.代码
```
#include<stdio.h>
#include<stdlib.h>           
#include<time.h>
#include<windows.h>
#include <math.h>

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
int x;
int y;
void Create_Food(void){
	x = rand()%10+1;
    y = rand()%10+1;
    if(map[x][y] == ' '){
    	map[x][y]='$';
	}    
	else{
		Create_Food();
	}     
};
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
}
void BodyMove(int snakelen){
	map[snake_X[0]][snake_Y[0]]=' ';/*将蛇尾消失*/
    int i;
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
void HeadChange(int dy,int dx,int snakelen){
	snakeLength += 1;
	snake_X[snakelen] = snake_X[snakelen - 1] + dx;
	snake_Y[snakelen] = snake_Y[snakelen - 1] + dy;
	map[snake_X[snakelen - 1]][snake_Y[snakelen - 1]] = SNAKE_BODY;
    map[snake_X[snakelen]][snake_Y[snakelen]] = SNAKE_HEAD;
}
void Snake_Move(int snakelen,char direct){
    switch(direct){
        case 'a':
        case 'A':
		if(map[snake_X[snakelen - 1]][snake_Y[snakelen - 1] - 1] == ' '){/*判断条件：脖子不在头的要转向的方向*/
			BodyMove(snakelen);
			int dy = -1,dx = 0;
            HeadMove(dy,dx,snakelen);
        }
        else if(map[snake_X[snakelen - 1]][snake_Y[snakelen - 1] - 1] == '$'){
			int dy = -1,dx = 0;
            HeadChange(dy,dx,snakelen);
            Create_Food();
		}
		else{
			GG = 1;
		}break;
        case 'w':
        case 'W':
		if(map[snake_X[snakelen - 1] - 1][snake_Y[snakelen - 1]] == ' '){
			BodyMove(snakelen);
            int dy = 0,dx = -1;
            HeadMove(dy,dx,snakelen);
        }
        else if(map[snake_X[snakelen - 1] - 1][snake_Y[snakelen - 1]] == '$'){
			int dy = 0,dx = -1;
            HeadChange(dy,dx,snakelen);
            Create_Food();
		}
		else{
			GG = 1;
		}break;
        case 's':
        case 'S':
		if(map[snake_X[snakelen - 1] + 1][snake_Y[snakelen - 1]] == ' '){
			BodyMove(snakelen);
            int dy = 0,dx = 1;
            HeadMove(dy,dx,snakelen);
        }
        else if(map[snake_X[snakelen - 1] + 1][snake_Y[snakelen - 1]] == '$'){
			int dy = 0,dx = 1;
            HeadChange(dy,dx,snakelen);
            Create_Food();
		}
		else{
			GG = 1;
		}break;
        case 'd':
        case 'D':
		if(map[snake_X[snakelen - 1]][snake_Y[snakelen - 1] + 1] == ' '){ 
		    BodyMove(snakelen);
            int dy = 1,dx = 0;
            HeadMove(dy,dx,snakelen);
        }
        else if(map[snake_X[snakelen - 1]][snake_Y[snakelen - 1] + 1] == '$'){
			int dy = 1,dx = 0;
            HeadChange(dy,dx,snakelen);
            Create_Food();
		}
		else{
			GG = 1;
		}break;
        default:break;
    }
}
char whereGoNext(int Hx,int Hy,int $x,int $y){
	char move[] = {'w','a','s','d'};
	int distance[4] = {0};
	int move_x[] = {-1,0,1,0};    
	int move_y[] = {0,-1,0,1};
	int min = 20;
	for(int i = 0;i < 4;++i){
		distance[i] = abs(Hx + move_x[i] - $x) + abs(Hy + move_y[i] - $y);
		if(map[Hx + move_x[i]][Hy + move_y[i]] == '*'||map[Hx + move_x[i]][Hy + move_y[i]] == 'X'){
			distance[i] = 20;
		}
		min = distance[i] < min?distance[i]:min;
	}
	for(int t = 0;t < 4;++t){
		if(distance[t] == min){
			return move[t];
		} 
	}
	return 0;
}
int main(void){
	srand((unsigned)time(NULL));
	Create_Food();
    while(GG != 1&&snakeLength <= SNAKE_MAX_LENGTH){
    	char ch = whereGoNext(snake_X[snakeLength - 1] ,snake_Y[snakeLength - 1] ,x,y);
        Output();
		Sleep(100);
        Snake_Move(snakeLength,ch);
    }
    Gameover();
    return 0;
}
```
## 3.创意
* 用一个全局变量记录已吃的食物个数，将之与sleep的时间挂钩，可以实现蛇的移动速度随游戏进行越来越快  
* 与之前的人为操控的贪吃蛇代码结合，稍加修饰便可以实现人机贪吃蛇大战，如争夺食物，通过卡身位使电脑控制的蛇死亡