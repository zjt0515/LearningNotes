```c++
#include <iostream>
#include <ctime>
#include <cstdio>
#include <cstdlib>
#include <graphics.h>
#include <mmsystem.h>
#pragma comment(lib,"winmm.lib")

#define SIZE 40		//图片宽度，即每个格子的宽度，
#define N 5			//每个难度显示的记录条数
#define M N*3		//3个难度，一共15条记录

using namespace std;
//*************************************************函数声明*************************************************
void MainWindow();		//主界面
void SettingWindow();	//设置界面
void RankWindow();		//排行界面

void StartGame();				//开始游戏
void InitGame();				//游戏初始化
void MouseClick();				//点击
void IsMine(int x, int y);	//点击后流程处理
void Show();					//每次处理后更新图片
void Judge(int x, int y);		//(点击后)胜利或失败判断

void Expand(int x, int y);				//打开0后炸开一片的递归
void Protect(int x, int y);				//第一次不能点到雷处理
void Mark(int x, int y);				//右键标记

void LoadImages1();//加载图片
void LoadImages2();
void PrintNearBy();//打印下面的数字，测试专用
void Exit();//安全退出函数


void ReadFile();	//文件读入
void Compare();		//数据比较并替换
void WriteFile();	//重新写入文件

typedef struct User //用户结构体
{
	int Level;		//用户选择的难度，分为1,2,3
	char name[20];	
	int time;
}User;
//*************************************************全局变量*************************************************
int row = 9;		//格子行数
int col = 9;		//格子列数
int number = 10;	//地雷数量

IMAGE Img[20];		//存图片
int cnt = 0;		//存已经打开的格子数量
time_t TimeStart;	//存开始时间
time_t TimeEnd;		//存结束时间
int** NearBy = NULL;//二维数组1
char** Map = NULL;	//二维数组2

User Top[M];					//排行榜用户
User User2[N];
User User3[N];
User CurrentUser = {1,"佚名"};	//存当前游玩用户信息，默认是难度1，佚名

void InitGame()
{
	initgraph(col * SIZE, row * SIZE);
	cnt = 0;//重置打开的格子个数记录器
	//初始化
	NearBy = new int* [row + 2];
	for (int i = 0; i < row + 2; i++)
	{
		NearBy[i] = new int[col + 2];
	}

	Map = new char* [row + 2];
	for (int i = 0; i < row + 2; i++)
	{
		Map[i] = new char[col + 2];
	}
	//初始化
	for (int i = 1; i <= row; i++)
	{
		for (int j = 1; j <= col; j++)
		{
			NearBy[i][j] = 0;
		}
	}
	for (int i = 1; i <= row; i++)
	{
		for (int j = 1; j <= col; j++)
		{
			Map[i][j] = '*';
		}
	}

	//随机埋地雷
	srand((unsigned)time(NULL));	//注意该行代码不能放到循环内
	int x, y;						//存储雷的坐标
	for (int i = 0; i < number;)
	{
		x = rand() % row + 1;	//随机数x范围1~row
		y = rand() % col + 1;	//随机数y范围1~col
		if (NearBy[x][y] == 0)	//要考虑到重复埋雷的问题
		{
			NearBy[x][y] = -1;
			i++;
		}
	}

	//计算每个位置周围地雷数量
	for (int i = 1; i <= row; i++)
	{
		for (int j = 1; j <= col; j++) {
			if (NearBy[i][j] == -1)
			{
				for (int p = i - 1; p <= i + 1; p++)
				{
					for (int q = j - 1; q <= j + 1; q++)
					{
						if (NearBy[p][q] != -1)//不是地雷就++，(不用排除自己，因为自己就是地雷，不会通过这条语句)
						{
							NearBy[p][q]++;
						}
					}
				}
			}
		}
	}
	PrintNearBy();
	Show();
}

//开始游戏
void StartGame()
{
	InitGame();
	mciSendString("play ./music/start.mp3", 0, 0, 0);
	time(&TimeStart);
	MouseClick();
}


//判定是否是地雷，是否不是地雷
void IsMine(int x, int y)
{
	//遇到已经打开的或者已经标记了的直接return
	if (Map[x][y] == '@' || Map[x][y] != '*')return;
	//遇到了地雷
	else if (NearBy[x][y] == -1)
	{
		//第一次就遇到雷要保护
		if (cnt == 0)
		{
			Protect(x, y);
			Expand(x, y);
		}
		else
		{
			mciSendString("play ./music/boom.mp3", 0, 0, 0);
			Expand(x, y);
		}
	}
	//不是地雷
	else
	{
		if (NearBy[x][y]==0)
		{
			mciSendString("play ./music/expand.mp3", 0, 0, 0);
			Expand(x, y);
		}
		else
		{
			Expand(x, y);
		}
		
		//PrintNearBy();
	}
	Judge(x, y);//输入式检测是否输赢
}

//右键标记
void Mark(int x, int y)
{
	
	if (Map[x][y] == '@')
	{
		Map[x][y] = '&';
		cout << "问号成功" << endl;
		Show();
	}
	else if (Map[x][y] == '&')
	{
		Map[x][y] = '*';
		cout << "取消标记成功" << endl;
		Show();
	}
	else
	{
		Map[x][y] = '@';
		cout << "标记成功" << endl;
		Show();
	}
}

//递归点开格子
void Expand(int x, int y)
{
	Map[x][y] = NearBy[x][y] + 48;
	if (NearBy[x][y] == -1)return;//这条代码是为了让该函数可以给 打开雷\0\1-8三种情况  同时使用的
	cnt++;//记录下已经翻开的格子
	if (NearBy[x][y] > 0)return;
	if (!(x >= 1 && x <= row && y >= 1 && y <= col))return;
	for (int i = x - 1; i <= x + 1; i++)
	{
		for (int j = y - 1; j <= y + 1; j++)
		{
			
			if (Map[i][j] == '*')
			{
				Expand(i, j);
			}
		}
	}
}

//用户点击
void MouseClick()
{
	ExMessage msg;
	while (true)
	{
		if (peekmessage(&msg))
		{
			int x = msg.y / SIZE + 1;
			int y = msg.x / SIZE + 1;
			switch (msg.message)
			{
			case WM_LBUTTONDOWN: mciSendString("play ./music/click.wav", 0, 0, 0); IsMine(x, y); break;
			case WM_RBUTTONDOWN: mciSendString("play ./music/rightclick.wav", 0, 0, 0); Mark(x, y); break;
			}
		}
	}
}

//第一次不能点到雷
void Protect(int x, int y)
{
	int a, b;
	srand((unsigned)time(NULL));	//注意该行代码不能放到循环内
	do
	{
		a = rand() % row + 1;	//随机数x范围1~row
		b = rand() % col + 1;	//随机数y范围1~col

	} while (NearBy[a][b] == -1);//找到一个没雷的位置把雷偷偷转移走

	NearBy[a][b] = -1;
	NearBy[x][y] = 0;
	//先把下面原来炸弹周围的点--
	for (int i = x - 1; i <= x + 1; i++)
	{
		for (int j = y - 1; j <= y + 1; j++)
		{
			if ((i != x || j != y) && NearBy[i][j] != -1)
			{
				NearBy[i][j]--;
			}
			if (NearBy[i][j] == -1)
			{
				NearBy[x][y]++;
			}
		}
	}

	//新位置加一个雷，周围++
	for (int i = a - 1; i <= a + 1; i++)
	{
		for (int j = b - 1; j <= b + 1; j++)
		{
			if ((i != x || j != y) && NearBy[i][j] != -1)
			{
				NearBy[i][j]++;
			}
		}
	}
}

//安全退出
void Exit()
{
	//释放new出来的
	for (int i = 0; i < row; i++)
	{
		delete[] NearBy[i];
	}
	delete[] NearBy;

	for (int i = 0; i < row; i++)
	{
		delete[] Map[i];
	}
	delete[] Map;
	//关闭窗口和程序
	closegraph();
	exit(0);
}

//测试代码
void PrintNearBy()
{
	//system("cls");
	cout << "下图仅用于测试" << endl;
	for (int k = 0; k <= col; k++)
	{
		printf("%3d", k);
	}
	cout << endl;
	cout << "---------------------" << endl;
	for (int i = 1; i <= row; i++)
	{
		printf("%d |", i);
		for (int j = 1; j <= col; j++)
		{
			printf("%3d", NearBy[i][j]);
		}
		cout << endl;
	}
	cout << "---------------------" << endl;
	cout << cnt << endl;
}

//胜利或失败判断，每输入一次后判断一下
void Judge(int x, int y)
{
	Show();
	if (NearBy[x][y] == -1)
	{
		for (int i = 1; i <= row; i++)
		{
			for (int j = 1; j <= col; j++)
			{
				if (NearBy[i][j] == -1)
				{
					Map[i][j] = NearBy[i][j] + 48;
				}
			}
		}
		Show();
		putimage(y * SIZE - SIZE, x * SIZE - SIZE, &Img[13]);
		HWND choice = GetHWnd();
		int ch = MessageBox(choice, "你失败了，是否再来一局？", "失败", MB_OKCANCEL);
		if (ch == IDOK)
		{
			StartGame();
		}
		else
		{
			initgraph(640, 640);
			MainWindow();
		}
	}

	if (cnt == row * col - number)
	{
		HWND choice = GetHWnd();
		time(&TimeEnd);
		int time = TimeEnd - TimeStart;
		CurrentUser.time = time;
		ReadFile();
		Compare();
		WriteFile();
		char str[50];
		sprintf(str, "您获胜了，耗时:%ds，是否再玩一次", time);
		int ch = MessageBox(choice, str, "很好", MB_OKCANCEL);
		if (ch == IDOK)
		{
			StartGame();
		}
		else
		{
			initgraph(640, 640);
			MainWindow();
		}
	}
} 
//*************************************************主函数*************************************************
int main()
{
	LoadImages2();
	initgraph(640, 640);
	MainWindow();
	return 0;
}
//*************************************************图形化函数*************************************************

//显示图片1-8,空白,炸弹，棋子等图片！(根据map中的数字/符号显示对应图片)
void Show()
{
	for (int i = 1; i <= row; i++)
	{
		for (int j = 1; j <= col; j++)
		{
			//如果没有被翻开就放置图片10
			if (Map[i][j] == '*')
			{
				putimage(j * SIZE - SIZE, i * SIZE - SIZE, &Img[10]);
			}
			//如果数字被翻开了就显示对应图片
			else if (Map[i][j] >= 48 && Map[i][j] <= 56)
			{
				for (int k = 0; k < 9; k++)
				{
					if (Map[i][j] == k + 48)
					{
						putimage(j * SIZE - SIZE, i * SIZE - SIZE, &Img[k]);
					}
				}

			}
			//如果翻开了地雷就显示地雷图片
			else if (Map[i][j] != '*' && Map[i][j] != '@' && Map[i][j] != '&' && NearBy[i][j] == -1)
			{
				putimage(j * SIZE - SIZE, i * SIZE - SIZE, &Img[9]);
			}
			//如果被标记了就显示标记图片
			else if (Map[i][j] == '@')
			{
				putimage(j * SIZE - SIZE, i * SIZE - SIZE, &Img[11]);
			}
			//被问号了就显示问号
			else if (Map[i][j] == '&')
			{
				putimage(j * SIZE - SIZE, i * SIZE - SIZE, &Img[12]);
			}
		}
	}
}

//加载经典图片
void LoadImages1()
{
	char path[50];
	loadimage(&Img[19], "./images1/bg.png", 640, 640);
	for (int i = 0; i < 15; i++)
	{
		sprintf(path, "./images1/%d.jpg", i);
		loadimage(&Img[i], path, SIZE, SIZE);
	}
}
//加载图片2
void LoadImages2()
{
	char path[50];
	loadimage(&Img[19], "./images2/bg.png", 640, 640);
	for (int i = 0; i < 15; i++)
	{
		sprintf(path, "./images2/%d.png", i);
		loadimage(&Img[i], path, SIZE, SIZE);
	}
}





//主界面
void MainWindow()
{
	setbkcolor(WHITE);
	cleardevice();
	putimage(0, 0, &Img[19]);
	settextcolor(BLACK);
	settextstyle(20, 0, "VonwaonBitmap 16px");
	outtextxy(140, 20, CurrentUser.name);
	settextstyle(40, 0, "VonwaonBitmap 16px");
	outtextxy(40, 250, "开始游戏");
	outtextxy(40, 325, "登录");
	outtextxy(40, 400, "设置");
	outtextxy(40, 475, "排行");
	outtextxy(40, 550, "退出");
	ExMessage msg;
	while (true)
	{
		if (peekmessage(&msg, EM_MOUSE))
		{
			if (msg.message == WM_LBUTTONDOWN)
			{
				//开始游戏
				if (msg.x >= 40 && msg.x <= 200 && msg.y >= 250 && msg.y <= 290)
				{
					cleardevice();
					StartGame();
				}
				//登录
				if (msg.x >= 40 && msg.x <= 120 && msg.y >= 325 && msg.y <= 365)
				{
					int isok = InputBox(CurrentUser.name, 10, "请输入您的用户名，最多使用10个字符，无法使用空白名", "登录", NULL, 0, 0, false);
					if (isok == 1 )
					{
						HWND hwnd = GetHWnd();
						MessageBox(hwnd, "登录成功", "提示", MB_OK);
						settextstyle(20, 0, "VonwaonBitmap 16px");
						outtextxy(140, 20, CurrentUser.name);
					}

				}
				//设置
				if (msg.x >= 40 && msg.x <= 120 && msg.y >= 400 && msg.y <= 440)
				{
					SettingWindow();
				}
				//排行
				if (msg.x >= 40 && msg.x <= 120 && msg.y >= 475 && msg.y <= 515)
				{
					RankWindow();
				}
				//退出
				if (msg.x >= 40 && msg.x <= 120 && msg.y >= 550 && msg.y <= 590)
				{
					Exit();
				}
			}
		}
	}

}

//设置界面
void SettingWindow()
{
	cleardevice();
	settextcolor(BLACK);
	settextstyle(40, 0, "VonwaonBitmap 16px");
	outtextxy(40, 50, "难度:");
	outtextxy(180, 50, "简单");
	outtextxy(300, 50, "普通");
	outtextxy(420, 50, "困难");
	outtextxy(40, 125, "外观:");
	outtextxy(180, 125, "经典");
	outtextxy(300, 125, "WIN7");
	outtextxy(500, 540, "返回");
	ExMessage msg;
	HWND hwnd = GetHWnd();
	while (true)
	{
		if (peekmessage(&msg, EM_MOUSE))
		{
			if (msg.message == WM_LBUTTONDOWN)
			{
				//简单
				if (msg.x >= 180 && msg.x <= 260 && msg.y >= 50 && msg.y <= 90)
				{
					CurrentUser.Level = 1;
					row = 9;
					col = 9;
					number = 10;
					MessageBox(hwnd, "简单模式设置成功", "提示", MB_OK);
				}
				//普通
				if (msg.x >= 300 && msg.x <= 380 && msg.y >= 50 && msg.y <= 90)
				{
					CurrentUser.Level = 2;
					row = 16;
					col = 16;
					number = 40;
					MessageBox(hwnd, "普通模式设置成功", "提示", MB_OK);
				}
				//困难
				if (msg.x >= 420 && msg.x <= 500 && msg.y >= 50 && msg.y <= 90)
				{
					CurrentUser.Level = 3;
					row = 16;
					col = 30;
					number = 99;
					MessageBox(hwnd, "困难模式设置成功", "提示", MB_OK);
				}
				//经典外观
				if (msg.x >= 180 && msg.x <= 260 && msg.y >= 125 && msg.y <= 205)
				{
					LoadImages1();
					MessageBox(hwnd, "更改成功", "提示", MB_OK);
				}
				//WIN7外观
				if (msg.x >= 300 && msg.x <= 380 && msg.y >= 125 && msg.y <= 165)
				{
					LoadImages2();
					MessageBox(hwnd, "更改成功", "提示", MB_OK);
				}
				//返回主界面
				if (msg.x >= 500 && msg.x <= 580 && msg.y >= 540 && msg.y <= 580)
				{
					MainWindow();
				}
				
			}
		}
	}
}
void RankWindow()
{
	cleardevice();
	outtextxy(500, 540, "返回");
	outtextxy(20, 20, "简单:");
	outtextxy(20, 220, "普通:");
	outtextxy(20, 440, "困难:");
	settextstyle(20, 0, "VonwaonBitmap 12px");
	ReadFile();
	for (int i = 1; i <= M; i++)
	{
		char time[20];
		sprintf(time, "%d", Top[i].time);
		if (i < 6)
		{
			outtextxy(180, 40 + 30 * i, Top[i].name);
			outtextxy(300, 40 + 30 * i, time);
		}
		else if (i < 11)
		{
			outtextxy(180, 80 + 30 * i, Top[i].name);
			outtextxy(300, 80 + 30 * i, time);
		}
		else
		{
			outtextxy(180, 120 + 30 * i, Top[i].name);
			outtextxy(300, 120 + 30 * i, time);
		}

		
	}
	ExMessage msg;
	while (true)
	{
		if (peekmessage(&msg, EX_MOUSE))
		{
			if (msg.message == WM_LBUTTONDOWN)
			{
				if (msg.x >= 500 && msg.x <= 580 && msg.y >= 540 && msg.y <= 580)
				{
					MainWindow();
				}
			}
		}
	}
}

//*************************************************文件函数*************************************************

//读入文件中的数据
void ReadFile()
{
	int i = 1;                       //默认结构体数组0的位置不存储数据，直接从1开始
	FILE* fp;                        /*定义文件指针*/
	fp = fopen("./rank.txt", "r");     /*打开文本文件准备读入数据*/
	if (fp == 0)                     /*如果文件打开失败 */
	{
		printf("source file error\n");
		exit(1);
	}
	while (!feof(fp))               /*当文件未结束时进入循环*/
	{
		/*结构体数组的下标加1然后继续读入*/
		fscanf(fp, "%d%s%d", &Top[i].Level, Top[i].name, &Top[i].time);//从文件中读入难度姓名和时间
		i++;
	}
	//测试代码，不重要
	for (int i = 1; i <= M; i++)
	{
		printf("%d%s%d\n", Top[i].Level, Top[i].name, Top[i].time);
	}
	fclose(fp);                     /*关闭文件*/
}

//数据比较，当当前玩家获胜时，要比比他和排行榜上的成绩
void Compare()
{	
	int last = 0;
	int i = 0;
	int cnt = 0;
	switch (CurrentUser.Level)//由于把不同难度排行的数据都定义在了一个结构体数组中，即，1-5存储简单难度，6-10存储普通难度，11-15困难
	{
		//要获取每个难度排行榜最后一名的下标
	case 1: i = last = 5; break;
	case 2: i = last = 10; break;
	case 3: i = last = 15; break;
	}
	//下面的代码就是具体比较的过程
	if (CurrentUser.time>=Top[i].time)
	{
		return;//最后一名都比不过直接return吧
	}
	else
	{
		//跟倒数第二名开始比
		do
		{
			//最多比5次，防止和其他赛道的比
			if (cnt == 5)
			{
				break;
			}
			i--;
			cnt++;
		} while (CurrentUser.time < Top[i].time );//出while后说明当前用户比不过top[i]，但能比过top[i+1]
		//cnt：比了cnt次，当前用户就是倒数第cnt名，例如，只比了1次，就说明没比过倒数第二名，就是倒数第一名，比了2次，就说明没比过倒数第三名，就是倒数第二名
	}
	cout << cnt << endl << i<< endl;
	int king = ++i;//现在++i就是当前用户应该取代的地方，但别急着取代，我们应该先把i以及下面的废物向下移位
	for (int k = 0; k < cnt - 1; last--,k++)//进行cnt-1次移位(例如，倒数cnt名，不难得出要进行cnt-1次移位)
	{
		strcpy(Top[last].name, Top[last-1].name);
		Top[last].time = Top[last-1].time;
	}
	//新王登基
	strcpy(Top[king].name, CurrentUser.name);
	Top[king].time = CurrentUser.time;
}

//数据重新写入
void WriteFile()
{
	int i = 1;//默认结构体数组0的位置不存储数据
	FILE* fp;
	fp = fopen("./rank.txt", "w");   /*以w方式打开文本文件准备写入内容*/
	if (fp == 0)	      					 /* 如果文件打开失败 */
	{
		printf("create new file error\n");	//报错并退出程序
		exit(1);
	}
	for (i = 1; i <= M; i++)
	{ 
		fprintf(fp, "%d %s %d\n", Top[i].Level, Top[i].name, Top[i].time);//将结构体数组1-15的数据写入文件
	}
	fclose(fp);              /*关闭文件*/
}
```