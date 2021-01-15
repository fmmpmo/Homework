```c++
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <string>
#include <vector>
#include <windows.h>
using namespace std;

#define MAX 100        //最大顶点数
#define SLEEP 1500     //休眠时间
const int inf = 65535; //定义无穷大

void LastIndex();  //退出登录界面
void Back();       //返回选取操作界面
void Index();      //首页
void Map();        //地图
void Create();     //创建图
void AdminLogin(); //管理员登录
void AdminMenu();  //管理员菜单
void TourLogin();  //游客登录
void TourMenu();   //游客菜单
void ShowAll();    //浏览所有信息并输出所有景点信息
void ViewOne();    //输出指定景点信息
void Dijkstra();   //指定景点到任意景点之间的距离
void Floyd();      //任意两点之间的距离
void AddInfo();    //添加景点信息
void ModInfo();    //修改景点信息
void DelInfo();    //删除景点信息
void AddPath();    //添加道路
void DelPath();    //删除道路

typedef struct {
    int id;
    string name;
    string features;
} Scene;

typedef struct {
    Scene sc[MAX];
    int graph[MAX][MAX];
    int n;
    int e;
} Graph;

Graph hbu;

int main() {
    Index();
    Sleep(3500);
    system("cls");
    system("color 07");
    Create();
    int index;
    SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                            FOREGROUND_INTENSITY | FOREGROUND_GREEN);
    cout << "✡　✱　✲　✶　✻　✼　❄　❅　❆　❉  欢迎进入HBU导航系统  "
            "❉　❆　❅　❄　✻　✼　✶　✲　✱　✡"
         << endl
         << endl;
    cout << "\t\t\t✹　✺　☄　☸  1---管理员身份登录" << endl << endl;
    cout << "\t\t\t✹　✺　☄　☸  2---游客身份登录" << endl << endl;
    cout << "\t\t\t✹　✺　☄　☸  0---退出导航系统" << endl << endl;
    cout << "\t\t\t❈❈❈❈❈❈❈❈  请选择您要进行的操作" << endl;
    cin >> index;
    switch (index) {
    case 1:
        AdminLogin();
        break;
    case 2:
        TourLogin();
        break;
    case 0:
        cout << "正在退出，请稍后...\n";
        Sleep(SLEEP);
        LastIndex();
        system("cls");
        return 0;
    default:
        cout << "输入有误，重新选择！\n";
        break;
    }
}

void Map() {
    printf("\t\t\t体检中心[1]-------操场[8]---------校门北口[9]-----银杏景观["
           "10]\n");
    printf("\t\t\t    |             /  \\              \\           /\n");
    printf("\t\t\t    |           /     \\              \\         /\n");
    printf("\t\t\t    |         /      图书馆[7]        \\       /\n");
    printf("\t\t\t    |       /______/    |  \\           \\     /\n");
    printf("\t\t\t邯郸音乐厅[2]           |   \\          餐厅[11] \n");
    printf("\t\t\t    |    \\              |    \\            /\n");
    printf("\t\t\t    |     \\             |    ——————————\\ /\n");
    printf("\t\t\t    |      \\        花园景观[6]---------校门东口[12]\n");
    printf("\t\t\t    |       \\       /   |              /\n");
    printf("\t\t\t    |     信息学部[4]   |             /\n");
    printf("\t\t\t    |        \\          |            /\n");
    printf("\t\t\t    |         \\         |           /\n");
    printf("\t\t\t    |          \\        |          /\n");
    printf("\t\t\t网计学院[3]     \\       |         /\n");
    printf("\t\t\t     \\           \\      |        /\n");
    printf("\t\t\t      \\_________  \\     |       /\n");
    printf("\t\t\t                    校门南口[5]\n\n\n");
}

void Index() {
    system("color 03");
    cout << "\t\t\t\n\nHBU\t\t\t\n\n";
    cout << "\t\t  欢迎来到HBU~~~~\n\n";
    printf("\t\t\t H\n");
    printf("\t\t\t  \n");
    printf("\t\t\t \n");
    printf("\t\t\t B\n");
    printf("\t\t\t \n");
    printf("\t\t\t \n");
    printf("\t\t\t U\n");
}

void LastIndex() {
    system("cls");
    system("color 03");
    cout << "\t\t\tHBU\t\t\t\n\n";
    cout << "\t\t  欢迎下次再来HBU~~~~\n\n";
    printf("\t\t\t H\n");
    printf("\t\t\t  \n");
    printf("\t\t\t \n");
    printf("\t\t\t B\n");
    printf("\t\t\t \n");
    printf("\t\t\t \n");
    printf("\t\t\t U\n");
    Sleep(3500);
    system("color 07");
}

void Back() {
    cout << "即将返回主界面.....\n";
    Sleep(2500);
}

void ShowAll() {
    if (hbu.n == 0) {
        cout << "当前没有可供欣赏的景点，很抱歉，浏览失败！\n\n";
        Back();
        return;
    }
    cout << "所有景点信息如下" << endl << endl;
    for (int i = 0; i < hbu.n; i++) {
        cout << "景点编号：" << hbu.sc[i].id << "\t景点名称：" << hbu.sc[i].name
             << "\t景点介绍：" << hbu.sc[i].features << "\n\n";
    }
}

void ViewOne() {
    if (hbu.n == 0) {
        cout << "当前没有可供欣赏的景点，无法查看！\n\n";
        Back();
        return;
    }
    cout << "请输入想要浏览的景点编号\n";
    int index;
    cin >> index;
    while (index < 1 || index > hbu.n) {
        cout << "编号输入不合法，请重新输入！\n";
        cin >> index;
    }
    cout << "景点编号：" << hbu.sc[index - 1].id << "\t景点名称："
         << hbu.sc[index - 1].name << "\t景点介绍："
         << hbu.sc[index - 1].features << "\n";
    Back();
}

void AddInfo() {
    if (hbu.n >= MAX) {
        cout << "当前景点总数已达上限，无法再添加！\n\n";
        Back();
        return;
    }
    ShowAll();
    cout << "请输入要增加的景点名称和景点特色描述\n";
    string name, info;
    cin >> name >> info;
    int edge;
    cout << "请输入要增加的邻接景点个数\n";
    cin >> edge;
    cout << "请依次输入要增加的邻接景点的编号及两景点间距离\n";
    for (int i = 0; i < edge; i++) {
        int id, dis;
        cin >> id >> dis;
        hbu.graph[id - 1][hbu.n] = hbu.graph[hbu.n][id - 1] = dis;
    }
    hbu.sc[hbu.n].id = hbu.n + 1;
    hbu.sc[hbu.n].name = name;
    hbu.sc[hbu.n].features = info;
    hbu.n++;
    hbu.e += edge;
    cout << "添加成功！\n\n";
    Back();
}

void ModInfo() {
    if (hbu.n == 0) {
        cout << "目前无任何景点可供观赏，很抱歉\n\n";
        Back();
        return;
    }
    ShowAll();
    int index;
    cout << "请输入要修改信息景点编号：\n";
    cin >> index;
    while (index < 1 || index > hbu.n) {
        cout << "输入有误，请重新输入\n";
        cin >> index;
    }
    cout << "请输入景点名称\n";
    string name, info;
    cin >> name;
    cout << "请输入景点特色描述\n";
    cin >> info;
    hbu.sc[index - 1].name = name;
    hbu.sc[index - 1].features = info;
    cout << "当前景点信息修改成功！\n\n";
    Back();
}

void DelInfo() {
    if (hbu.n == 0) {
        cout << "当前没有任何景点可供欣赏，无法删除！\n\n";
        Back();
        return;
    }
    ShowAll();
    cout << "请输入要删除景点编号\n";
    int index;
    cin >> index;
    while (index < 1 || index > hbu.n) {
        cout << "输入有误，请重新输入\n\n";
        cin >> index;
    }
    int cnt = 0; //用于记录删除边数，用于更新地图中边的数量
    for (int i = 0; i < hbu.n; i++) {
        if (hbu.graph[index - 1][i] < inf)
            cnt++;
    }
    for (int i = index - 1; i < hbu.n; i++) {
        hbu.sc[i] = hbu.sc[i + 1];
        hbu.sc[i].id -= 1;
    }
    for (int i = 0; i < hbu.n; i++) {
        for (int j = index - 1; j < hbu.n; j++) {
            hbu.graph[i][j] = hbu.graph[i][j + 1];
        }
    }
    for (int i = 0; i < hbu.n; i++) {
        for (int j = index - 1; j < hbu.n; j++) {
            hbu.graph[j][i] = hbu.graph[j + 1][i];
        }
    }
    hbu.n--;
    hbu.e -= cnt;
    cout << "删除景点成功！\n\n";
    Back();
}

void Dijkstra() {
    if (hbu.n == 0) {
        cout << "当前没有景点可供欣赏，无法查看！\n\n";
        Back();
        return;
    }
    if (hbu.e == 0) {
        cout << "当前没有可达道路，无法查看！\n\n";
        Back();
        return;
    }
    if (hbu.n == 1) {
        cout << "当前只有一个景点可供欣赏哦，所以没有最短路哦！\n\n";
        Back();
        return;
    }
    ShowAll();
    cout << "请输入指定景点编号\n";
    int index;
    cin >> index;
    while (index < 1 || index > hbu.n) {
        cout << "景点编号输入不合法，请重新输入！\n\n";
        cin >> index;
    }
    int dis[MAX];       //距离
    int bef[MAX];       //用于输出路径
    int vis[MAX] = {0}; //判断是否访问
    vis[index - 1] = 1;
    for (int i = 0; i < hbu.n; i++) {
        dis[i] = hbu.graph[index - 1][i];
        bef[i] = index - 1;
    }
    dis[index - 1] = 0; //自己到自己的距离是0
    bef[index - 1] = -1;
    int u;
    for (int i = 1; i < hbu.n; i++) {
        int minx = inf;
        for (int j = 0; j < hbu.n; j++) {
            if (!vis[j] && dis[j] < minx) {
                minx = dis[j];
                u = j;
            }
        }
        vis[u] = 1;
        for (int k = 0; k < hbu.n; k++) {
            if (!vis[k] && dis[k] > dis[u] + hbu.graph[u][k]) {
                dis[k] = dis[u] + hbu.graph[u][k];
                bef[k] = u;
            }
        }
    }
    bool flag = true;
    //遍历，输出路径
    for (int i = 0; i < hbu.n; i++) {
        if (i != index - 1 && dis[i] < inf) {
            flag = false;
            cout << "从" << hbu.sc[index - 1].name << "到" << hbu.sc[i].name
                 << "的距离是：" << dis[i] << "米。";
            vector<string> v;
            u = bef[i];
            v.push_back(hbu.sc[i].name);
            while (u >= 0) {
                v.push_back(hbu.sc[u].name);
                u = bef[u];
            }
            cout << "路径为：" << v[v.size() - 1];
            for (int k = v.size() - 2; k >= 0; k--) {
                cout << "->" << v[k];
            }
            cout << "\n\n";
        }
    }
    if (flag) {
        cout << "当前景点与任一景点不连通，查询失败！\n\n";
    }
    Back();
}

void Floyd() {
    if (hbu.n == 0) {
        cout << "当前没有景点可供欣赏，查询失败！\n";
        Back();
        return;
    }
    if (hbu.e == 0) {
        cout << "当前没有可达道路，无法查看！\n";
        Back();
        return;
    }
    if (hbu.n == 1) {
        cout << "当前只有一个景点可供欣赏哦，所以没有最短路哦！\n";
        Back();
        return;
    }
    ShowAll();
    int dis[MAX][MAX]; //距离
    int bef[MAX][MAX]; //用于输出路径
    for (int i = 0; i < hbu.n; i++) {
        for (int j = 0; j < hbu.n; j++) {
            dis[i][j] = hbu.graph[i][j];
            bef[i][j] = j;
        }
    }
    for (int k = 0; k < hbu.n; k++) {
        for (int i = 0; i < hbu.n; i++) {
            for (int j = 0; j < hbu.n; j++) {
                if (dis[i][j] > dis[i][k] + dis[k][j]) {
                    dis[i][j] = dis[i][k] + dis[k][j];
                    bef[i][j] = k;
                }
            }
        }
    }
    cout << "请输入要查询的两景点编号\n";
    int a, b;
    cin >> a >> b;
    while (a < 1 || a > hbu.n || b < 1 || b > hbu.n) {
        cout << "景点编号不合法，请重新输入！\n";
        cin >> a >> b;
    }
    cout << hbu.sc[a - 1].name << "与" << hbu.sc[b - 1].name << "间的最短距离为"
         << dis[a - 1][b - 1] << "米。";
    cout << "最短路径为：" << hbu.sc[a - 1].name;

    int pre = a - 1;
    while (bef[b - 1][pre] != pre) {
        pre = bef[b - 1][pre];
        cout << "->" << hbu.sc[pre].name;
    }
    cout << "->" << hbu.sc[b - 1].name << "\n";
    Back();
}

void AddPath() {
    if (hbu.n == 0) {
        cout << "当前没有景点可供欣赏，无法添加道路！\n\n";
        Back();
        return;
    }
    if (hbu.n == 1) {
        cout << "当前仅有一个景点可供欣赏，无法添加道路！\n\n";
        Back();
        return;
    }
    ShowAll();
    cout << "请输入要添加道路的两个邻接景点\n\n";
    int a, b;
    cin >> a >> b;
    if (hbu.graph[a - 1][b - 1] < inf) {
        cout << "两景点已连通，无法重复添加！\n\n";
        return;
    }
    int dis;
    cout << "请输入道路信息\n";
    cin >> dis;
    hbu.graph[a - 1][b - 1] = hbu.graph[b - 1][a - 1] = dis;
    hbu.e++;
    cout << "添加成功！\n\n";
    Back();
}

void DelPath() {
    if (hbu.e == 0) {
        cout << "当前无道路可供删除，无法删除！\n\n";
        Back();
        return;
    }
    if (hbu.n == 0) {
        cout << "当前没有景点可供欣赏，所以没有道路啊！\n\n";
        Back();
        return;
    }
    ShowAll();
    cout << "请输入要删除道路的两个邻接景点编号\n";
    int a, b;
    cin >> a >> b;
    while (a < 1 || a > hbu.n || b < 1 || b > hbu.n) {
        cout << "景点编号输入不合法，请重新输入！\n";
        cin >> a >> b;
    }
    if (hbu.graph[a][b] == inf) {
        cout << "操作错误，此两顶点间无道路，不用删除！\n\n";
        Back();
        return;
    }
    hbu.graph[a - 1][b - 1] = hbu.graph[b - 1][a - 1] = inf;
    hbu.e--;
    cout << "删除成功！\n\n";
    Back();
}

void TourLogin() {
    int cnt = 0;
    while (1) {
        cout << "请输入账号" << endl;
        string id, pwd;
        cin >> id;
        cout << "请输入密码" << endl;
        cin >> pwd;
        if (id == "tourpmo" && pwd == "880818") {
            system("cls");
            TourMenu();
            break;
        } else {
            cnt++;
            if (cnt > 0) {
                cout << "您还有" << 3 - cnt << "次机会\n";
                cin >> id >> pwd;
            } else {
                cout << "登录失败，请联系管理员！\n\n";
                system("cls");
                Map();
                cout << "是否退出导航系统？\n";
                string s;
                cin >> s;
                if (s == "Y||y") {
                    cout << "正在退出，请稍后...";
                    Sleep(SLEEP);
                    LastIndex();
                    system("cls");
                    return;
                }
                break;
            }
        }
    }
}

void TourMenu() {
    cout << "\t\t✡　✱　✲　✶　✻　✼　❄　❅　❆　❉ 欢迎进入游客系统 "
            "✡　✱　✲　✶　✻　✼　❄　❅　❆　❉"
         << endl
         << endl
         << endl;
    Map();
    Sleep(3000);
    while (1) {
        SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                FOREGROUND_INTENSITY | FOREGROUND_RED |
                                    FOREGROUND_GREEN);
        int index;
        cout << "\t\t✹　✺　☄　☸  choose 0 : 退出登录\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 1 : 浏览所有景点介绍\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 2 : 浏览特定景点介绍\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 3 : "
                "查找特定景点到其他景点的最短路径\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 4 : 查找任意两个景点之间的最短路径\n\n";
        cout << "\t\t✩  ✩  ✩  ✩  ✩  ✩ 请选择您要进行的操作：\n";

        cin >> index;
        switch (index) {
        case 0:
            cout << "正在退出，请稍后...\n";
            Sleep(SLEEP);
            LastIndex();
            system("cls");
            return;
            break;
        case 1:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            ShowAll();
            Back();
            break;
        case 2:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            ViewOne();
            break;
        case 3:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            Dijkstra();
            break;
        case 4:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            Floyd();
            break;
        default:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_RED);
            cout << "输入有误，请重新选择操作" << endl;
            break;
        }
    }
}

void AdminLogin() {
    int cnt = 0;
    while (1) {
        cout << "请输入账号" << endl;
        string id, pwd;
        cin >> id;
        cout << "请输入密码" << endl;
        cin >> pwd;
        if (id == "adminfmm" && pwd == "010315") {
            system("cls");
            AdminMenu();
            break;
        } else {
            cnt++;
            if (cnt > 0) {
                cout << "您还有" << 3 - cnt << "次机会\n";
                cin >> id >> pwd;
            } else {
                cout << "登录失败，请联系管理员！\n";
                system("cls");
                Map();
                cout << "是否退出导航系统？\n";
                string s;
                cin >> s;
                if (s == "Y||y") {
                    cout << "正在退出，请稍后...";
                    Sleep(SLEEP);
                    LastIndex();
                    system("cls");
                    return;
                }
                break;
            }
        }
    }
}

void AdminMenu() {
    cout << "\t\t✡　✱　✲　✶　✻　✼　❄　❅　❆　❉ 欢迎进入管理员系统 "
            "✡　✱　✲　✶　✻　✼　❄　❅　❆　❉"
         << endl
         << endl
         << endl;
    Map();
    Sleep(3000);
    while (1) {
        SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                FOREGROUND_INTENSITY | FOREGROUND_RED |
                                    FOREGROUND_GREEN);
        int index;
        cout << "\t\t✹　✺　☄　☸  choose 0 : 退出登录\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 1 : 浏览所有景点介绍\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 2 : 浏览特定景点介绍\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 3 : "
                "查找特定景点到其他景点的最短路径\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 4 : 查找任意两个景点之间的最短路径\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 5 : 增加景点信息\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 6 : 修改景点信息\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 7 ：删除景点信息\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 8 : 增加道路\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 9 : 删除道路\n\n";
        cout << "\t\t✹　✺　☄　☸  choose 10: 查看原始地图\n\n";
        cout << "\t\t✩  ✩  ✩  ✩  ✩  ✩ 请选择您要进行的操作：\n";
        cin >> index;
        switch (index) {
        case 0:
            cout << "正在退出，请稍后...\n";
            Sleep(SLEEP);
            LastIndex();
            system("cls");
            return;
            break;
        case 1:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            ShowAll();
            Back();
            break;
        case 2:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            ViewOne();
            break;
        case 3:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            Dijkstra();
            break;
        case 4:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            Floyd();
            break;
        case 5:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            AddInfo();
            break;
        case 6:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            ModInfo();
            break;
        case 7:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            DelInfo();
            break;
        case 8:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            AddPath();
            break;
        case 9:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            DelPath();
            break;
        case 10:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_GREEN);
            Create();
            Map();
            Back();
            break;
        default:
            SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
                                    FOREGROUND_INTENSITY | FOREGROUND_RED);
            cout << "输入有误，请重新选择操作" << endl;
            break;
        }
    }
}

void Create() {
    hbu.n = 12;
    hbu.e = 20;
    for (int i = 0; i < MAX; i++) {
        hbu.sc[i].id = i + 1;
    }
    for (int i = 0; i < MAX; i++) {
        for (int j = 0; j < MAX; j++) {
            hbu.graph[i][j] = inf;
        }
    }
    hbu.sc[0] = {1, "体检中心", "HBU体检中心---祝您身体健康"};
    hbu.sc[1] = {2, "邯郸音乐厅", "HBU邯郸音乐厅---相信音乐的力量"};
    hbu.sc[2] = {3, "网计学院", "HBU网络空间安全与计算机学院---代码改变世界"};
    hbu.sc[3] = {4, "信息学部", "HBU信息学部---全是机密"};
    hbu.sc[4] = {5, "校南门口", "HBU南门口---HBU的门面"};
    hbu.sc[5] = {6, "花园景观", "HBU花园景观---你笑起来像春天的花儿一样好看"};
    hbu.sc[6] = {7, "图书馆", "HBU图书馆---巅峰建筑，书中自有黄金屋"};
    hbu.sc[7] = {8, "操场", "HBU体育场---生命在于运动"};
    hbu.sc[8] = {9, "校北门口", "HBU北门口---致敬逝去的北街"};
    hbu.sc[9] = {10, "银杏景观", "HBU景观大道---秋天时的银杏很美，一起来拍照"};
    hbu.sc[10] = {11, "餐厅",
                  "HBU尚饮餐厅3层---干饭人，干饭魂，干饭都是人上人"};
    hbu.sc[11] = {12, "校门东口", "HBU东门口---从我这里回家"};

    hbu.graph[0][1] = hbu.graph[1][0] = 200;
    hbu.graph[0][7] = hbu.graph[7][0] = 350;
    hbu.graph[1][2] = hbu.graph[2][1] = 500;
    hbu.graph[1][7] = hbu.graph[7][1] = 480;
    hbu.graph[1][6] = hbu.graph[6][1] = 400;
    hbu.graph[1][3] = hbu.graph[3][1] = 500;
    hbu.graph[2][4] = hbu.graph[4][2] = 400;
    hbu.graph[4][3] = hbu.graph[3][4] = 400;
    hbu.graph[4][5] = hbu.graph[5][4] = 500;
    hbu.graph[4][11] = hbu.graph[11][4] = 600;
    hbu.graph[3][5] = hbu.graph[5][3] = 150;
    hbu.graph[5][6] = hbu.graph[61][5] = 160;
    hbu.graph[5][11] = hbu.graph[11][5] = 200;
    hbu.graph[6][7] = hbu.graph[7][6] = 280;
    hbu.graph[6][11] = hbu.graph[11][6] = 300;
    hbu.graph[7][8] = hbu.graph[8][7] = 200;
    hbu.graph[8][9] = hbu.graph[9][8] = 100;
    hbu.graph[8][10] = hbu.graph[10][8] = 100;
    hbu.graph[9][10] = hbu.graph[10][9] = 100;
    hbu.graph[10][11] = hbu.graph[11][10] = 100;
}
```

