---
title: C语言利用EasyX实现贪吃蛇基础版
tags:
  - C语言项目
abbrlink: b13a
date: 2021-10-17 00:30:31
password:
---







#### 运行环境



**Visual Studio 2019**



#### EasyX得自己安装



### 具体操作看沃CSDN



[https://blog.csdn.net/m0_51570792/article/details/120806547](https://editor.csdn.net/md?not_checkout=1&articleId=120806547)





#### 代码



~~~c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <windows.h>
#include <graphics.h> //头文件得自己安装
#include <conio.h>    // 我本地编译conio.h与graphics.h顺序不能换
#include <time.h>
#include <mmsystem.h>
#pragma comment(lib, "winmm.lib")


#define db double
// y轴最大值 
#define Y_MAX 720
// x轴最大值       
#define X_MAX 1280
//蛇的最大节数    
#define SNAKE_SIZE_NAX 500
//同一时间出现的食物最大数量
#define FOOD_MAX 500       
//食物颜色的数量  
#define COLOR_NUMBER 11
#define snake_root_r 6
#define snake_body_r 4
#define DELAY 30

/************************************************结构定义***************************************************/

//枚举蛇的方向
enum DIR {                  //枚举第一个元素默认为0，依次加加 
	UP, DOWN, LEFT, RIGHT,  //上下左右
};

//蛇的结构体
typedef struct Snake {
	int sum_value;          //蛇吃的当前分数，100分一节蛇的身体 
	int size;               //蛇的节数
	int dir;                //蛇的方向
	int speed;              //蛇的移动速度
	// 蛇的每一节的坐标
	POINT body[SNAKE_SIZE_NAX];
} Snake;

typedef struct Food {
	POINT point; //食物坐标
	DWORD color; //食物颜色
	int r; //食物半径
	int  value;
} Food;

Snake snake;
Food food[FOOD_MAX];

/************************************************结构操作***************************************************/
// 游戏界面窗口建立
void Graphics_Window() {
	// 初始化图形窗口
	initgraph(X_MAX, Y_MAX/*, SHOWCONSOLE*/); // 长、宽，窗口颜色
	return ;
}
//食物的初始化
void Food_Init(int i) {
	//初始化食物的半径
	food[i].r = rand() % 6;
	// 初始化食物坐标
	food[i].point.x = rand() % X_MAX;
	food[i].point.y = rand() % Y_MAX;
	// 初始化食物颜色
	food[i].color = RGB(rand() % 256, rand() % 256, rand() % 256); // 随机生成颜色
	// 初始化每个食物分数
	food[i].value = rand() % 11;
}
// 游戏初始化
void Game_Init() {
	/***************背景音乐播放***************/

	mciSendString("open ./res/snake_bgm.mp3 alias BGM", 0, 0, 0);
	mciSendString("play BGM repeat", 0, 0, 0);

	/***************食物的初始化***************/
	for (int i = 0; i < FOOD_MAX; i++) {
		Food_Init(i);
	}
	/***************蛇的初始化***************/
	snake.sum_value = 0;      // 蛇吃的当前分数
	snake.size = 3;           // 必须snake.size >= 1, 确保蛇头存在
	snake.dir = rand() % 4;   // 初始化蛇的方向 
	snake.speed = 6;          // 初始蛇的速度
	// 初始化蛇不会立马撞死在墙上 
	if (rand() & 1) {
		snake.body[0].x = 300 - rand() % 300;
		snake.body[0].y = 300 - rand() % 300;
	} else {
		snake.body[0].x = 300 + rand() % 300;
		snake.body[0].y = 300 + rand() % 300;
	}
	// 初始化蛇的其他部分
	for (int i = 1; i < snake.size; i++) {
		snake.body[i].x = snake.body[i - 1].x - 9;
		snake.body[i].y = snake.body[i - 1].y;
	}
	return;
}

// 蛇身体复制
void Copy() {
	for (int i = snake.size - 1; i; i--) {
		snake.body[i] = snake.body[i - 1];
	}
}

// 蛇的移动
void Snake_Move() {
	Copy();
	switch (snake.dir) {
		case 0 :
			snake.body[0].y -= snake.speed;
			break;
		case 1 :
			snake.body[0].y += snake.speed;
			break;
		case 2 :
			snake.body[0].x -= snake.speed;
			break;
		case 3 :
			snake.body[0].x += snake.speed;
			break;
	}
	return ;
}

// 键盘操作蛇
void Key_Control() {
	// 72, 80, 75, 77  上下左右键值
	//判断是否有按键，没有返回假
	if (_kbhit() == false) return ;
	switch (_getch()) {
		case 'W' :
		case 'w' :
		case 72  :
			if (snake.dir == DOWN) break;
			snake.dir = UP;
			break;
		case 'S':
		case 's':
		case 80 :
			if (snake.dir == UP) break;
			snake.dir = DOWN;
			break;
		case 'A':
		case 'a':
		case 75 :
			if (snake.dir == RIGHT) break;
			snake.dir = LEFT;
			break;
		case 'D':
		case 'd':
		case 77 :
			if (snake.dir == LEFT) break;
			snake.dir = RIGHT;
			break;

		case ' ' : //游戏停止与继续
			while (1) {
				if (_getch() == ' ') return ;
			}
			break;
	}
	return;
}

// 绘制游戏图案
void Game_Draw() {
	//双缓冲绘图，防止闪屏
	BeginBatchDraw();
	// 设置背景颜色
	setbkcolor(RGB(255, 218, 185));
	//setbkcolor(RGB(28, 115, 119));
	cleardevice();

	for (int i = 0; i < FOOD_MAX; i++) {
		setfillcolor(food[i].color);
		solidcircle(food[i].point.x, food[i].point.y, food[i].r);
	}

	setfillcolor(GREEN);
	solidcircle(snake.body[0].x, snake.body[0].y, snake_root_r);
	for (int i = 1; i < snake.size; i++) {
		solidcircle(snake.body[i].x, snake.body[i].y, snake_body_r); //绘制实心圆
	}
	EndBatchDraw();
	return;
}

//蛇的身体修改
void Modify_Snake() {
	if (snake.sum_value / 10 == 0) return ;
	int k = snake.sum_value / 10;
	if (snake.size + k < SNAKE_SIZE_NAX) snake.size += k;
	else snake.size = SNAKE_SIZE_NAX;
	snake.sum_value -= k * 10;
	Copy();
	return ;
}

//游戏规则制定
void Game_Rule() {
	if (snake.body[0].x < 0 || snake.body[0].y < 0 || snake.body[0].x > X_MAX || snake.body[0].y > Y_MAX) {
		printf("游戏结束\n");
		exit(0);
	}

	for (int i = 0; i < FOOD_MAX; i++) {
		double dis = sqrt(pow((db)food[i].point.x - (db)snake.body[0].x, 2) + pow((db)food[i].point.y - (db)snake.body[0].y, 2));
		if (dis <= (db)(food[i].r + (db)snake_root_r)) {
			snake.sum_value += food[i].value;
			Food_Init(i);
			Modify_Snake();
		}
	}

	return ;
}

int main() {
	srand((unsigned int)time(0));

	Graphics_Window();
	Game_Init();
	while (1) {
		Game_Draw();
		Key_Control();
		Snake_Move();
		Game_Rule();
		Sleep(DELAY);
	}
	return 0;
}

~~~

