### code
``` C++
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;
#define N 310
#define INF 1<<25
#define clr(x) memset(x,0,sizeof(x))


int w[N][N];
int la[N],lb[N];
int va[N],vb[N];
int match[N],slack[N];
int n;

bool dfs(int x){
    va[x]=1;
    for(int y=1;y<=n;y++){
        if(vb[y])continue;//只尝试一次
        int need=la[x]+lb[y]-w[x][y];
        if(!need){
            vb[y]=1;
            if(!match[y]||dfs(match[y])){
                match[y]=x;
                return true;
            }
        }
        else if(slack[y]>need)
            slack[y]=need;
    }
    return false;
}

int KM(){
    int i,j;
    clr(match);
    clr(la);
    clr(lb);
    for(i=1;i<=n;i++)
        for(j=1;j<=n;j++)
            if(w[i][j]>la[i])
                la[i]=w[i][j];


    for(i=1;i<=n;i++){
        for(j=1;j<=n;j++)
            slack[j]=INF;
        while(true){
            clr(va);
            clr(vb);
            if(dfs(i))
                break;
            int d=INF;
            for(j=1;j<=n;j++){
                if(!vb[j]&&d>slack[j])
                    d=slack[j];
            }
            for(j=1;j<=n;j++){
                if(va[j])
                    la[j]-=d;
                if(vb[j])
                    lb[j]+=d;
            }
        }
    }
    int res=0;
    for(i=1;i<=n;i++)
        res+=w[match[i]][i];
    return res;
}
int main(){
    while(~scanf("%d",&n)){
        clr(w);
        for(int i=1;i<=n;i++)
            for(int j=1;j<=n;j++)
                scanf("%d",&w[i][j]);
        printf("%d\n",KM());
    }
    return 0;
}
```
