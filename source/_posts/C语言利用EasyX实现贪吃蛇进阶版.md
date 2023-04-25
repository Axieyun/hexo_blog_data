---
title: C语言利用EasyX实现贪吃蛇进阶版
password: 9c22ae0cfdc126a61c62f433f72055aa9e370524942ea688c4c5da1597786954
abbrlink: 98c3
date: 2021-10-17 11:15:39
tags:
      - C语言项目
---







### 代码演示



~~~c
#include<stdio.h>
#include<stdlib.h>
#include <math.h>
#include <windows.h>
#include <graphics.h> // 自己安装EasyX
#include <conio.h>
#include <map>
#include <time.h>
#include <mmsystem.h>
#pragma comment(lib, "winmm.lib")

using namespace std;


#define MAX_SNAKE 20
#define db double
// y轴最大值 
#define Y_MAX 720
// x轴最大值       
#define X_MAX 1280  
//蛇的最大节数    
#define SNAKE_SIZE_MAX 5000
//同一时间出现的食物最大数量   
#define FOOD_MAX 500       
//食物颜色的数量  
#define COLOR_NUMBER 11
#define snake_root_r 6
#define snake_body_r 4
#define DELAY 15

/************************************************结构定义***************************************************/

//枚举方向
enum DIR {                  //枚举第一个元素默认为0，依次加加 
	UP, DOWN, LEFT, RIGHT,  //上下左右
};

//蛇的结构体
typedef struct Snake {
	int dir_cnt;
	int temp_sum;           //<= 10, 蛇吃的当前分数，10分一节蛇的身体
	int sum;                //蛇吃的当前分数，10分一节蛇的身体 
	int size;               //蛇的节数
	int dir;                //蛇的方向
	int speed;              //蛇的移动速度
	// 蛇的每一节的坐标
	POINT body[SNAKE_SIZE_MAX];
	DWORD color;
} Snake;

typedef struct Food {
	POINT point; //食物坐标
	DWORD color; //食物颜色
	int r; //食物半径
	int  value;
} Food;
//定义蛇的数组，其中下标为0的蛇为自己操纵的蛇，其他为人机蛇
Snake snake[MAX_SNAKE + 5];
Food food[FOOD_MAX];

/************************************************结构操作***************************************************/
// 游戏界面窗口建立
void Graphics_Window() {
	// 初始化图形窗口
	initgraph(X_MAX, Y_MAX, SHOWCONSOLE); // 长、宽，窗口颜色
	return;
}
//食物的初始化
void Food_Init(const int& i) {
	//初始化食物的半径
	food[i].r = rand() % 6;
	// 初始化食物坐标
	food[i].point.x = rand() % X_MAX;
	food[i].point.y = rand() % Y_MAX;
	// 初始化食物颜色
	food[i].color = RGB(rand() % 256, rand() % 256, rand() % 256);
	// 初始化每个食物分数
	food[i].value = 5;
}
//蛇初始化
void Snake_Init(const int& i) {
	/***************蛇的初始化***************/
	snake[i].color = RGB(rand() % 256, rand() % 256, rand() % 256);
	snake[i].dir_cnt = 10;
	snake[i].temp_sum = 0;      // 蛇吃的当前分数
	snake[i].sum = 0;           // 蛇的总分 
	snake[i].size = rand() % 3 + 3;           // 必须snake.size >= 1, 确保蛇头存在
	snake[i].dir = rand() % 4;   // 初始化蛇的方向
	snake[i].speed = rand() % 3 + 1;          // 初始蛇的速度
	// 初始化蛇不会立马撞死在墙上
	if (rand() & 1) {
		snake[i].body[0].x = rand() % X_MAX;
		snake[i].body[0].y = rand() % Y_MAX;
	}
	else {
		snake[i].body[0].x = 300 + rand() % 300;
		snake[i].body[0].y = 300 + rand() % 300;
	}
	// 初始化蛇的其他部分
	for (int j = 1; j < snake[i].size; j++) {
		snake[i].body[j].x = snake[i].body[j - 1].x - 9;
		snake[i].body[j].y = snake[i].body[j - 1].y;
	}
}
// 游戏初始化
void Game_Init(const int& n) {
	/***************背景音乐播放***************/

	mciSendString("open ./res/snake_bgm.mp3 alias BGM", 0, 0, 0);
	mciSendString("play BGM repeat", 0, 0, 0);

	/***************食物的初始化***************/
	for (int i = 0; i < FOOD_MAX; i++) {
		Food_Init(i);
	}
	/***************蛇的初始化***************/
	for (int i = 1; i <= n; i++) {
		Snake_Init(i); //  群体蛇初始化
	}
	/***************我的蛇的初始化***************/
	snake[0].temp_sum = 0;       // 蛇吃的当前分数
	snake[0].size = 3;           // 必须snake.size >= 1, 确保蛇头存在
	snake[0].dir = rand() % 4;   // 初始化蛇的方向 
	snake[0].sum = 0;            // 蛇的总分 
	snake[0].speed = 5;          // 初始蛇的速度
	// 初始化蛇不会立马撞死在墙上 
	if (rand() & 1) {
		snake[0].body[0].x = 300 - rand() % 100;
		snake[0].body[0].y = 300 - rand() % 100;
	}
	else {
		snake[0].body[0].x = 300 + rand() % 100;
		snake[0].body[0].y = 300 + rand() % 100;
	}
	// 初始化蛇的其他部分
	for (int i = 1; i < snake[0].size; i++) {
		snake[0].body[i].x = snake[0].body[i - 1].x - 9;
		snake[0].body[i].y = snake[0].body[i - 1].y;
	}
	return;
}

/***********************键盘操作蛇***********************/
void Key_Control() {
	// 72, 80, 75, 77  上下左右键值
	//判断是否有按键，没有返回假
	if (_kbhit() == false) return;
	switch (_getch()) {
	case 'W':
	case 'w':
	case 72:
		if (snake[0].dir == DOWN) break;
		snake[0].dir = UP;
		break;
	case 'S':
	case 's':
	case 80:
		if (snake[0].dir == UP) break;
		snake[0].dir = DOWN;
		break;
	case 'A':
	case 'a':
	case 75:
		if (snake[0].dir == RIGHT) break;
		snake[0].dir = LEFT;
		break;
	case 'D':
	case 'd':
	case 77:
		if (snake[0].dir == LEFT) break;
		snake[0].dir = RIGHT;
		break;

	case ' ': //游戏停止与继续
		while (1) {
			if (_getch() == ' ') return;
		}
		break;
	}
	return;
}


//绘制圆点
void Circular_Draw(const int& x, const int& y, const int& r) {
	solidcircle(x, y, r);
	return;
}

//绘制单条蛇
void Snake_Draw(const int& j, const DWORD& colour) {
	setfillcolor(colour);
	Circular_Draw(snake[j].body[0].x, snake[j].body[0].y, snake_root_r); // 绘制头部
	// 绘制身体部分
	for (int i = 1; i < snake[j].size; i++) {
		Circular_Draw(snake[j].body[i].x, snake[j].body[i].y, snake_body_r); //绘制实心圆
	}
	return;
}


// 绘制游戏图案
void Game_Draw(const int& n) {
	//双缓冲绘图，防止闪屏
	BeginBatchDraw();
	// 设置背景颜色
	setbkcolor(RGB(255, 218, 185));
	//setbkcolor(RGB(28, 115, 119));
	cleardevice();

	//绘制食物
	for (int i = 0; i < FOOD_MAX; i++) {
		setfillcolor(food[i].color);
		Circular_Draw(food[i].point.x, food[i].point.y, food[i].r);
	}

	/***************************绘制所有蛇************************/
	// 绘制自己的蛇
	Snake_Draw(0, RED); //蛇默认为红

	//绘制人机蛇
	for (int j = 1; j <= n; j++) {
		Snake_Draw(j, snake[j].color);
	}

	EndBatchDraw();
	return;
}

// 蛇身体复制
void Copy(const int& t) {
	for (int i = snake[t].size - 1; i; i--) {
		snake[t].body[i] = snake[t].body[i - 1];
	}
}

// 单条蛇的移动
void Snake_Move(const int& t) {
	Copy(t);
	switch (snake[t].dir) {
	case 0:
		snake[t].body[0].y -= snake[t].speed;
		break;
	case 1:
		snake[t].body[0].y += snake[t].speed;
		break;
	case 2:
		snake[t].body[0].x -= snake[t].speed;
		break;
	case 3:
		snake[t].body[0].x += snake[t].speed;
		break;
	}
	return;
}

//单条蛇的身体修改
void Modify_Snake(const int& i) {
	if (snake[i].temp_sum / 10 == 0) return;
	if (snake[i].size == SNAKE_SIZE_MAX) { //预判
		return;
	}
	int k = snake[i].temp_sum / 10;

	if (snake[i].size + k < SNAKE_SIZE_MAX) {
		snake[i].size += k;
		snake[i].temp_sum -= k * 10;
	}
	else {
		snake[i].size = SNAKE_SIZE_MAX;
		snake[i].temp_sum = 0;
	}

	return;
}

//游戏规则制定
void Game_Rule(const int& n) {
	/**************************预判游戏是否结束******************************************************************/
	//人机蛇是否触墙
	for (int i = 1; i <= n; i++) {
		//预判人机操作的蛇是否碰墙，碰墙人机蛇刷新
		if (snake[i].body[0].x < 0 || snake[i].body[0].y < 0 || snake[i].body[0].x > X_MAX || snake[i].body[0].y > Y_MAX) {
			Snake_Init(i);
		}
	}
	//预判人操作的蛇是否碰墙，碰墙游戏结束
	if (snake[0].body[0].x < 0 || snake[0].body[0].y < 0 || snake[0].body[0].x > X_MAX || snake[0].body[0].y > Y_MAX) {
		printf("游戏结束\n");
		exit(0);
	}
	//暴力搜索

	for (int i = 1; i <= n; i++) {
		double dis = sqrt(pow((db)snake[i].body[0].x - (db)snake[0].body[0].x, 2) + pow((db)snake[i].body[0].y - (db)snake[0].body[0].y, 2));
		if (dis <= (db)(snake_root_r << 1)) { //我的蛇的头与人机蛇头接触，死亡
			printf("游戏结束!!!!!!!!!\n");
			printf("你的蛇已死亡，死亡最后总分为：%d\n", snake[0].sum);
			exit(0);
		}
		for (int j = 1; j < snake[i].size; j++) {
			dis = sqrt(pow((db)snake[i].body[j].x - (db)snake[0].body[0].x, 2) + pow((db)snake[i].body[j].y - (db)snake[0].body[0].y, 2));
			if (dis <= (db)(snake_root_r + snake_body_r)) { //我的蛇的头与人机蛇身体部分接触，死亡
				printf("游戏结束\n");
				exit(0);
			}
		}
	}
	int snake_vis[MAX_SNAKE + 1];
	memset(snake_vis, false, sizeof(snake_vis));

	for (int i = 1; i < n; i++) { //第i条人机蛇
		double dis;
		for (int j = i + 1; j <= n; j++) {
			if (snake_vis[j]) continue; //表示当前时间这条蛇被搜索过，且死不了，就不要搜了
			dis = sqrt(pow((db)snake[i].body[0].x - (db)snake[j].body[0].x, 2) + pow((db)snake[i].body[0].y - (db)snake[j].body[0].y, 2));
			if (dis <= (db)(snake_root_r << 1)) { //两条蛇的头接触，死亡
				//死亡加新生
				printf("序号为%d的蛇死亡，死亡最后总分为：%d\n", i, snake[i].sum);
				Snake_Init(i);
				printf("序号为%d的蛇死亡，死亡最后总分为：%d\n", j, snake[j].sum);
				Snake_Init(j);
				continue;
			}

			//判断第i条蛇头是否与第j条蛇身体触碰
			for (int k = 1; k < snake[j].size; k++) {
				dis = sqrt(pow((db)snake[i].body[0].x - (db)snake[j].body[k].x, 2) + pow((db)snake[i].body[0].y - (db)snake[j].body[k].y, 2));
				if (dis <= (db)(snake_root_r + snake_body_r)) {
					printf("序号为%d的蛇死亡，死亡最后总分为：%d\n", i, snake[i].sum);
					Snake_Init(i);
					continue;
				}
			}
		}
		snake_vis[i] = true; // 当前时间第i条蛇死不了
	}

	for (int i = 1; i <= n; i++) {
		for (int j = 1; j < snake[0].size; j++) {
			double dis = sqrt(pow((db)snake[i].body[0].x - (db)snake[0].body[j].x, 2) + pow((db)snake[i].body[0].y - (db)snake[0].body[j].y, 2));
			if (dis <= (db)(snake_root_r + snake_body_r)) {
				printf("序号为%d的蛇死亡，死亡最后总分为：%d\n", i, snake[i].sum);
				Snake_Init(i);
				break;
			}
		}
	}

	/*****************************************************搜索，给蛇加分********************************************************************/
	for (int j = 0; j <= n; j++) { // 枚举蛇
		for (int i = 0; i < FOOD_MAX; i++) { //  枚举食物
			double dis = sqrt(pow((db)food[i].point.x - (db)snake[j].body[0].x, 2) + pow((db)food[i].point.y - (db)snake[j].body[0].y, 2));
			if (dis <= (db)(food[i].r + (db)snake_root_r)) {
				if (j) {
					snake[j].temp_sum += food[i].value << 1;
					snake[j].sum += food[i].value << 1;
				} else {
					snake[j].temp_sum += food[i].value;
					snake[j].sum += food[i].value;
				}
				Food_Init(i); // 食物重新刷新
				Modify_Snake(j); // 蛇身体修改
			}
		}
	}

	return;
}

/***********************************************************增加人机蛇****************************************************************/
//人机蛇自己移动

void f(const int& n) {
	for (int i = 1; i <= n; i++) {
		if (snake[i].dir_cnt) {
			snake[i].dir_cnt--;
			Snake_Move(i);
			continue;
		}
		snake[i].dir = rand() % 4;
		snake[i].dir_cnt = rand() % 4 + 3;
	}
	return;
}


int main() {
	srand((unsigned int)time(0)); // 从当前时间生成伪随机数
	const int n = 12; //默认人机蛇的数量[1, 5]
	Graphics_Window();
	Game_Init(n);
	printf("snake(%d, %d)，dir = %d, speed = %d\n", snake[0].body[0].x, snake[0].body[0].y, snake[0].dir, snake[0].speed);
	printf("size = %d\n", snake[0].size);
	for (int i = 1; i < snake[0].size; i++) {
		printf("body(%d, %d)，dir = %d\n", snake[0].body[i].x, snake[0].body[i].y, snake[0].dir);
	}
	while (1) {
		Game_Draw(n); //绘制游戏
		Key_Control(); // 键盘操作游戏
		f(n);          // 人机蛇移动
		for (int i = 0; i <= n; i++) {
			Snake_Move(i); // 蛇移动

		}
		Game_Rule(n); // 处理蛇的游戏规则
		Sleep(DELAY); //蛇的速度延迟
	}
	return 0;
}


~~~

