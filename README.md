# Csnake\

```cpp
#include <bits/stdc++.h>
#include <windows.h>
#include <conio.h>

#define edgex 60  
#define edgey 28 

using namespace std;
char kbinput;  
list<int> snakex;  
list<int> snakey;  
int endgame,score;  

typedef struct f{
    int x,y;
}foodtyp;

foodtyp food;

void to(int x, int y){
    COORD pos;
    HANDLE hOutput;
    pos.X = x;
    pos.Y = y;
    hOutput = GetStdHandle(STD_OUTPUT_HANDLE);
    SetConsoleCursorPosition(hOutput, pos);
}

void print(){
    auto x=snakex.begin();
    auto y=snakey.begin();
    for(;x!=snakex.end();x++,y++){  
        to(*x,*y);
        cout << "■";
    }  
}

void creatfood(){
    int checkfood=1;
    while(true){
        food.x=(rand()%(edgex-1))+1;
        food.y=(rand()%(edgey-1))+1;
        if(food.x%2!=0) continue;

        for(auto itx=snakex.begin(),ity=snakey.begin();itx!=snakex.end();itx++,ity++){
            if(*itx==food.x&&*ity==food.y){
                checkfood=0;
                break;
            } 
        }  
        if(checkfood) break;
    }
    to(food.x,food.y);
    cout << "■";
}

void setup(){
    int dx[]={2,0,-2,0,0};
    int dy[]={0,1,0,-1,0};
    int x=0,y=0,p=0;
    kbinput='a',endgame=score=0;
    while(p!=4){
        to(x,y);
        cout << "■";
        if(x+dx[p]>edgex||y+dy[p]>edgey||x+dx[p]<0||y+dy[p]<0){
            p++;
        }
        x+=dx[p],y+=dy[p];
    }  
    for(int i=0;i<=10;i+=2){
        snakex.push_back(24+i);
        snakey.push_back(20);
    }  
    print();
    to(80,12),cout << "Score = " << score;
    srand(time(0));
    creatfood();
}

void getmove(){
    if(kbinput!='s'&&GetAsyncKeyState(VK_UP)) kbinput='w';
    if(kbinput!='w'&&GetAsyncKeyState(VK_DOWN)) kbinput='s';
    if(kbinput!='a'&&GetAsyncKeyState(VK_RIGHT)) kbinput='d';
    if(kbinput!='d'&&GetAsyncKeyState(VK_LEFT)) kbinput='a';
}

int check(int x,int y){
    if(x==0||x==edgex||y==0||y==edgey) return 1;
    for(auto cx=snakex.begin(),cy=snakey.begin();cx!=snakex.end();cx++,cy++){
        if(*cx==x&&*cy==y) return 1;
    }  
    return 0;
}

void movesnake(){
    int movex=snakex.front();
    int movey=snakey.front();
    if(kbinput=='w') movey--;
    if(kbinput=='a') movex-=2;
    if(kbinput=='s') movey++;
    if(kbinput=='d') movex+=2;
    if(check(movex,movey)){
        endgame=1;
        return;
    }
    snakex.push_front(movex);
    snakey.push_front(movey);
   if(snakex.front()==food.x&&snakey.front()==food.y){
        to(88,12),cout << ++score;
        creatfood();  //創造新的食物
    }else{
        to(snakex.back(),snakey.back());
        cout << "  ";
        snakex.pop_back();
        snakey.pop_back();
    }
    print();
}

int main(){
    setup();
    while(endgame==0){
        getmove();
        movesnake();
        Sleep(100);
    }
    to(80,14),cout << "Game Over!";
    to(80,16),system("pause");
    return 0;
}
```
