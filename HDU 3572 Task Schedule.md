题型：网络流+判断满流

**思路**：
将每天视为一个点，那么第 i 个任务可以在第 si 到 ei天之间执行，也就是任务 i 可以和 [si,ei] 之间的点连一条容量为1的边，代表被执行了一天‘
将源点与第 i 个任务连一条容量为 pi 的边，代表这个任务要被执行的次数
让每天与汇点之间连一条容量为 m 的边，表示这天最多执行 m 个任务
如果网络的最大流等于需要被执行的总天数，则YES,否则NO。
```C++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <queue>
#include <cstring>
#include <map>
 
using namespace std;
const int INF = 0x3f3f3f3f;
const int maxn = 2000 + 100;
const int maxm = 1000000 + 100;
typedef pair<int,int> P;
int n,m;
int l[maxn];//记录层数
int h[maxn];//链式前向星
int cur[maxn];
int tot = 0;
struct edge
{
 int to;
 int c;
 int next;
 edge(int x = 0, int y = 0, int z = 0) : to(x), c(y), next(z) {}
}es[maxm*2];//记录边 注意是2倍
 
void add_edge(int u, int v, int c)
{
   es[tot] = edge(v,c,h[u]);
   h[u] = tot++;
}
 
 
 
bool bfs(int s, int t)
{
  memset(l,0,sizeof(l));
  l[s] = 1;
  queue <int> q;
  q.push(s);
  while(!q.empty())
  {
   int u = q.front();
   q.pop();
   //cout << u <<  " " <<l[u] << endl;
   if(u == t)  return true;
   for(int i = h[u]; i != -1; i = es[i].next)
       {
        int v = es[i].to;
        if(!l[v] && es[i].c) {l[v] = l[u] + 1; q.push(v);}
       }
  }
  return false;
}
 
int dfs(int x, int t, int mf)
{
 
   if(x == t) return mf;
   int ret = 0;
   for(int &i = cur[x]; i != -1; i = es[i].next)
   {
    //  cout << x << " " << l[es[i].to]  << " "<< "mf = " << mf << endl;
     if(es[i].c && l[x] == l[es[i].to] - 1)
     {
      // cout << es[i].c << " "<< l[es[i].to] << endl;
       int f = dfs(es[i].to,t,min(es[i].c,mf - ret));
       es[i].c -= f;
       es[i^1].c += f;
       ret += f;
       if(ret == mf) return ret;
     }
   }
   return ret;
}
 
int dinic(int s, int t)
{
 int ans = 0;
 while(bfs(s,t))  {
   for(int i =
      0; i <= t; i++) cur[i] = h[i];
   int f = dfs(s,t,INF);
   ans += f;
   //if(f == 0) return ans;
   //cout << f << endl;
 }
 return ans;
}
int main()
{
  int T;
  scanf("%d",&T);
  int TT = T;
   while(T--)
   {
   scanf("%d%d",&n,&m);
   tot = 0;
   memset(h,-1,sizeof(h));
   int  t = 500 + n + 1;
   int p,s,e;
   int res = 0;
   for(int i = 1; i <= 500; i++) {add_edge(i,t,m);add_edge(t,i,0);}
   for(int i = 1; i <= n; i++)
   {
    scanf("%d%d%d",&p,&s,&e);
    add_edge(0,500+i,p);
    add_edge(500+i,0,0);
    res += p;
    for(int j = s; j <= e; j++)
    {
      add_edge(i+500,j,1);
      add_edge(j,i+500,0);//增加反向边
    }
   }
   int ans = dinic(0,t);
   printf("Case %d: ",TT-T);
   if(ans >= res) printf("Yes\n");
   else printf("No\n");
   printf("\n");
   }
   return 0;
}
```
