---
layout: post
title: 2015-2016 ACM ICPC Baltic Selection Contest
tags: [contest]
categories: [contest]
published: true
---

### A题：AHB 水题，直接上代码：

{% highlight c++ %}
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;
typedef long long Long;
const int maxn = 1e5 + 10;
const int maxm = 256;

int main() 
{
	char s1[20], s2[20];

	while (scanf("%s%s", s1, s2) != EOF)
	{
		int l1 = (int) strlen(s1), l2 = (int) strlen(s2);
		bool flag = false;
		for (int i = 0; i < l1; i++)
		{
			int v1 = s1[i]-'0', v2 = s2[i]-'0';
			int v = max(v1,v2)-min(v1,v2);
			if (v == 0 && !flag)
			{
				continue;
			}
			flag = true;
			printf("%d", v);
		}
		if (!flag) printf("0");		
		puts("");
	}
	
    return 0;
{% endhighlight %}

### B题：Wet Boxes 这个题据说是线段树，暂时没做出来

### C题： Minimax Tree

#### 题意：

给出一棵n个节点的树，叶节点都有一个value值，其余节点每个节点可以
放上一个max开关或者一个min开关, 如果u是max开关，那么val[u] = 
max(val[v]), v是u的子节点，其中min开关和max开关的个数都是给定的，
并且max开关+min开关 = n-叶子节点数，让你安排每个中间节点是max还是min,
输出根节点的最大值和最小值

#### 题解：

因为求最大值和最小值都是一样的方法，所以我们以求最大值来分析，
首先，我们先考虑把最大的叶子节点u传上去，因为u最大，所以，我们
在往上的路径分叉处只能用max开关，为了能让u传上去，肯定应该让不是该路径上的
其他分叉都尽量用min开关以节省max开关，现在我们考虑u的兄弟节点v，如果要把u的兄弟节点
v传上去，因为u比v大，所以我们在u和v的父节点f上肯定用min开关最划算，那么现在我们就把
问题转化成了把f上的节点传到根节点去(即min(u,v))。
所以说：如果当前考虑的节点u够大，那么我们就直接让接下来的路程都用max开关，否则就让他
用min开关传到父节点去。

代码：

{% highlight c++ %}
#include <cmath>
#include <queue>
#include <vector>
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;

#define vsz(u) ((int) G[u].size())
typedef long long Long;
typedef vector<int>::iterator vIte;
const int maxn = 1e5 + 10;
const int inf = 1e9;
const double eps = 1e-8;

vector<int> G[maxn];
int val[maxn], minv[maxn], maxv[maxn];
int ansmn, ansmx, mn, mx, lev;

void dfs(int u, int judge)
{
	minv[u] = inf; maxv[u] = 0;
	if (vsz(u) == 0)
	{
		minv[u] = maxv[u] = val[u];
	}
	for (vIte it = G[u].begin(); it != G[u].end(); ++it)
	{
		dfs(*it, judge+(vsz(u)!=1));
		minv[u] = min(minv[u], minv[*it]);
		maxv[u] = max(maxv[u], maxv[*it]);
	}
	if (judge <= mx) ansmx = max(ansmx, minv[u]);
	if (judge <= mn) ansmn = min(ansmn, maxv[u]);
}
int main() 
{
	int n, k;

	while (scanf("%d%d",&n, &k) != EOF)
	{
		for (int i = 0; i < maxn; i++)
		{
			G[i].clear();
		}
		for (int i = 2; i <= n; i++)
		{
			int p; scanf("%d", &p);
			G[p].push_back(i);
		}
		int zero = 0;
		for (int i = 1; i <= n; i++)
		{
			scanf("%d", &val[i]);
			zero += !val[i];
		}
		mn = k, mx = zero-k, lev = n-zero;
		ansmn = inf; ansmx = 0;
		dfs(1, 0);
		printf("%d %d\n", ansmn, ansmx);
	}

    return 0;
}
{% endhighlight %}

### D题：Journey 

给一个n*m的矩阵，由.和#组成，要从左上角走到右下角的花费，其中
奇数步的花费为a,偶数步的花费为b, 直接先求从左上角到右下角的最短路step
然后答案就是step/2*(a+b) + (step&1)，因为第一步是奇数步。

代码：

{% highlight c++ %}

#include <iostream>
#include <cstdio>
#include <queue>
#include <cstring>
using namespace std;

const int maxn = 507;
int n, m;
int dire[][2] = { {-1,0},{1,0},{0,-1},{0,1} };
char mapp[maxn][maxn];
bool vis[maxn][maxn];
struct Data 
{
    int x, y, step;
    Data() {}
    Data(int x1, int y1, int step1) {x = x1, y = y1, step = step1;}
}cur;
queue<Data> que;
bool safe(int x, int y) {
    if (0 <= x && x < n && 0 <= y && y < m) return true;
    return false;
}
int bfs() {
    memset(vis, false, sizeof(vis));
    while (!que.empty()) que.pop();
    que.push(Data(0,0,0));
    vis[0][0] = true;
    while (!que.empty()) {
        cur = que.front();
        que.pop();
        if (cur.x == n-1 && cur.y == m-1) return cur.step;
        for (int i = 0; i < 4; i++) {
            int x = cur.x + dire[i][0], y = cur.y + dire[i][1];
            if (safe(x, y)) {
                if (mapp[x][y] != '#' && !vis[x][y]) {
                    que.push(Data(x,y,cur.step+1));
                    vis[x][y] = true;
                }
            }
        }
    }
    return -1;
}
int main() {
    while (scanf("%d%d", &m, &n) != EOF) {
        int a, b;
        scanf("%d%d", &a, &b);
        for (int i = 0; i < n; i++) {
            scanf("%s", mapp[i]);
        }
        int ans = bfs();
        if (ans == -1) printf("IMPOSSIBLE\n");
        else {
            int cost;
            cost = (ans / 2) * (a + b);
            if (ans & 1) cost += b;
            printf("%d\n", cost);
        }
    }
    return 0;
}
{% endhighlight %}

### E题：Permutation Polygon 

给一个n个点，点是1-n顺时针的，n条边的图，问有多少个交点。

建图的时候建立从小点到大点建有向图，然后从小到大扫描每个点，用
树状数组维护即可。

{% highlight c++ %}
#include <cstdio>
#include <vector>
#include <cstring>
#include <algorithm>
using namespace std;
#define pb push_back
const int maxn = 1e5 + 10;
typedef long long Long;
typedef vector<int>::iterator vIte;
Long C[maxn];
int n;
vector<int> G[maxn];

inline int lowbit(int x)
{
	return x&(-x);
}
void add(int x, int v)
{
	while (x <= n)
	{
		C[x] += v;
		x += lowbit(x);
	}
}
Long sum(int p)
{
	Long ret = 0;
	while (p > 0)
	{
		ret += C[p];
		p -= lowbit(p);
	}
	return ret;
}
int main()
{
	scanf("%d", &n);

	for (int i = 1; i <= n; i++)
	{
		int v; scanf("%d", &v);
		G[min(i,v)].pb(max(i,v));
	}
	Long ans = 0;
	for (int i = 1; i <= n; i++)
	{
		add(i,-sum(i));
		for (vIte it = G[i].begin(); it != G[i].end(); ++it)
		{
			ans += sum((*it)-1);
		}
		for (vIte it = G[i].begin(); it != G[i].end(); ++it)
		{
			add(*it,1);
		}
	}
	printf("%I64d\n", ans);

	return 0;
}
{% endhighlight %}

### F题：Unusual Sum

水题：直接化简一下就ok了：

代码：

{% highlight c++ %}
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;
typedef long long Long;
const int maxn = 1e5 + 10;
const int maxm = 256;


int main() 
{
	int t; scanf("%d", &t);

	while (t--)
	{
		double x, y; scanf("%lf%lf", &x, &y);
		printf("%.10f\n", (y+1-x)/(x*(y+1)));
	}
	
    return 0;
}
{% endhighlight %}

### G题：Robot Walk

水模拟：
代码：

{% highlight c++ %}
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
#define maxn 100005

char str[maxn];
char op[maxn];
char ans[maxn];
int acnt = 0;
int main()
{
	int n, m, x;
	scanf("%d%d", &n, &x);
	scanf("%s", str);
	scanf("%d", &m);
	scanf("%s", op);
	x--;
	ans[acnt++] = str[x];
	for (int i = 0; op[i] != '\0'; i++)
	{
		if (op[i] == 'L') x--;
		else x++;
		ans[acnt++] = str[x];
	}
	ans[acnt] = '\0';
	puts(ans);
	return 0;
}
{% endhighlight %}

### H题：Game of Corners

水题有点多：

{% highlight c++ %}
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;

int main()
{
	int n, m;
	scanf("%d%d", &n, &m);
	if (n > m) swap(n, m);
	printf("%I64d\n", (long long)n*(m+1));
	return 0;
}
{% endhighlight %}

### I题：Shell Game 

用前视图看，题目就转化为在一个梯形里面放圆，然后直接二分圆的半径，
看是否能放进去就行了。

{% highlight c++ %}
#include <cmath>
#include <queue>
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;
typedef long long Long;
const int maxn = 5e2 + 10;
const double eps = 1e-8;

bool ok(double m, double r, double R, double h)
{
	r*=2; R*=2;
	double seta = atan(h/((R-r)/2.0));
	double afa = atan(m/(R/2.0));
	double bata = seta - afa;
	double edge = sqrt(m*m+(R/2.0)*(R/2.0));
	double ans = edge*sin(bata);
	return ans >= m;
}
int main() 
{
	double r, R, h;

	while (scanf("%lf%lf%lf", &r, &R, &h) != EOF)
	{
		double low = 0, high = h/2.0;

		while (high-low >= eps)
		{
			double mid = (low+high) / 2.0;
			if (ok(mid,r,R,h))
			{
				low = mid;
			}
			else high = mid;
		}
		printf("%.7f\n", low);
	}
		
    return 0;
}
{% endhighlight %}

### J题：Narrow Bus

给一个空队列，有n个操作，（1<=n<=1e5), 一是F:从队首进入一个数，
二是B:从队尾进入一个数，三是第O i: 第i个进入的数选择两边中人少的一边出队，
所有在他前方的依次出队，从队列的另一边依次进入，输出地i个人出队的时候，有多少人
要因此出队。用Splay模拟即可

代码：
{% highlight c++ %}
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
const int maxn = 1e5 + 10;
const int inf = 1e8;
struct node
{
	int l, r, f, sz, v;
	void clear()
	{
		l = r = f = sz = 0;
	}
} nd[maxn];
int rt, tot;
void init()
{
	tot = 0;
	rt = 0;
}
int newNode() 
{
	nd[++tot].clear();
	return tot;
}
void up(int x)
{
	nd[x].sz = 1;
	if (nd[x].l) nd[x].sz += nd[nd[x].l].sz;
	if (nd[x].r) nd[x].sz += nd[nd[x].r].sz;
}
void r_rorate(int x)
{
	int f = nd[x].f, l = nd[x].l;
	if (nd[f].l == x) nd[f].l = l;
	else nd[f].r = l;
	nd[l].f = f;
	nd[x].l = nd[l].r; nd[nd[x].l].f = x;
	nd[x].f = l; nd[l].r = x;
	up(x);
}
void l_rorate(int x)
{
	int f = nd[x].f, r = nd[x].r;
	if (nd[f].l == x) nd[f].l = r;
	else nd[f].r = r;
	nd[r].f = f;
	nd[x].r = nd[r].l; nd[nd[x].r].f = x;
	nd[x].f = r; nd[r].l = x;
	up(x);
}
void splay(int x, int rootf)
{
	for (; nd[x].f != rootf; )
	{
		int f = nd[x].f, g = nd[f].f;
		if (g == rootf) {
			if (nd[f].l == x)
				r_rorate(f);
			else 
				l_rorate(f);
		}
		else {
			if (nd[g].l == f)
				if (nd[f].l==x) r_rorate(g), r_rorate(f);
				else l_rorate(f), r_rorate(g);
			else
				if (nd[f].r == x) l_rorate(g), l_rorate(f);
				else r_rorate(f), l_rorate(g);
		}
	}
	up(x);
	if (!rootf) rt = x;
}
int getFirst(int x)
{
	for (; nd[x].l; x = nd[x].l) ;
	return x;
}
int getLast(int x)
{
	for (; nd[x].r; x = nd[x].r) ;
	return x;
}
int Gao(int x)
{
	splay(x, 0);
	int ret = inf;
	if (nd[x].l) ret = min(ret, nd[nd[x].l].sz);
	else ret = 0;
	if (nd[x].r) ret = min(ret, nd[nd[x].r].sz);
	else ret = 0;
	if (nd[x].r == 0)
	{
		nd[nd[x].l].f = 0;
		rt = nd[x].l;
		nd[x].l = 0;
		return ret;
	}
	nd[nd[x].r].f = 0;
	int xx = getLast(x);
	splay(xx, 0);
	nd[xx].r = nd[x].l;
	if (nd[x].l) nd[nd[x].l].f = xx;
	if (nd[x].l) splay(nd[x].l, 0);
	nd[x].r = 0;
	return ret;
}
int main()
{
	int n;

	while (scanf("%d", &n) != EOF)
	{
		init();
		nd[0].clear();
		for (int i = 0; i < n; i++)
		{
			nd[0].l = nd[0].r = 0;
			char cmd[3]; scanf("%s", cmd);
			if (cmd[0] == 'F')
			{
				int id = newNode();
				int x = getFirst(rt);
				splay(x, 0);
				if (x) nd[x].l = id;
				nd[id].f = x;
				splay(id, 0);
			}
			else if (cmd[0] == 'B')
			{
				int x = getLast(rt);
				int id = newNode();
				splay(x, 0);
				if (x) nd[x].r = id;
				nd[id].f = x;
				splay(id, 0);
			}
			else
			{
				int id; scanf("%d", &id);
				printf("%d\n", Gao(id));
			}
		}
	}


	return 0;
}
{% endhighlight %}


### K题： Profact

1e5组数据，输入一个a(1<=a<=1e18),问a是否能用若干个阶乘的乘积得到。

因为20的阶乘就已经超过了1e18,直接把所有的组合情况放进set，然后二分查找即可。

{% highlight c++ %}
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <set>
using namespace std;
#define LL long long
#define INF 1e18
LL num[20];
set<LL> Set;
void solve(LL x, int s)
{
	for (int i = s; i<20; i++)
	{
		if (INF/x < num[i]) return;
		Set.insert(x*num[i]);
		solve(x*num[i], i);
	}
}
void init()
{
	num[1] = 1;
	for (int i = 2; i<20; i++)
		num[i] = num[i-1]*i;
	Set.insert(1);
	solve(1, 2);
}
int main()
{
	init();
	int t;
	scanf("%d", &t);
	while (t--)
	{
		LL x;
		scanf("%I64d", &x);
		if (Set.find(x) != Set.end()) puts("YES");
		else puts("NO");
	}
	return 0;
}
{% endhighlight %}

### L题： Emoticons

水模拟

{% highlight c++ %}
#include <cstdio>
#include <cstring>
using namespace std;
const int maxn = 1e5+7;
char str[maxn];
int main() {
    int n;
    while (scanf("%d", &n) != EOF) {
        scanf("%s", str);
        int sm = 0, sa = 0;
        for (int i = 0; i < n; i++) {
            if (str[i] == ':') {
                if (i - 1 >= 0) {
                    if (str[i-1] == '(') sm++;
                    if (str[i-1] == ')') sa++;
                }
                if (i + 1 < n) {
                    if (str[i+1] == '(') sa++;
                    if (str[i+1] == ')') sm++;
                }
            }
        }
        if (sm > sa) printf("HAPPY\n");
        else if (sm < sa) printf("SAD\n");
        else printf("BORED\n");
    }
    return 0;
}
{% endhighlight %}


































