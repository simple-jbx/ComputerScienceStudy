# 模拟

## [LeetCode 1583. 统计不开心的朋友](https://leetcode-cn.com/problems/count-unhappy-friends/)

# 图&树

## 二叉树

### [二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        //一个是另一个的祖先
        if(root == null || root.val == p.val || root.val == q.val) {
            return root;
        }

        //p q分布在左右子树
        TreeNode l = lowestCommonAncestor(root.left, p, q);
        TreeNode r = lowestCommonAncestor(root.right, p, q);

        if(l != null && r != null) {
            return root;
        }

        //p q分布在同一子树
        return l == null ? r : l;
    }
}
```

## **拓扑排序**★★★★★

### [LeetCode 207.课程表](https://leetcode-cn.com/problems/course-schedule/)

节点分为三种状态，未搜索、搜索中、已搜索

```java
class Solution {
        int[] visited;
        boolean valid = true;
        List<List<Integer>> edges = new ArrayList<>();
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        visited = new int[numCourses];

        for (int i = 0; i < numCourses; i++) {
            edges.add(new ArrayList<Integer>());
        }

        for (int[] info : prerequisites) {
            edges.get(info[1]).add(info[0]);
        }

        for (int i = 0; i < numCourses; i++) {
            if(visited[i] == 0) {
                dfs(i);
            }
        }
        return valid ;
    }

    private void dfs(int n) {
        visited[n] = 1;

        for(int v : edges.get(n)) {
            if(visited[v] == 0) {
                dfs(v);
                if(!valid) {
                    return;
                }
            } else if(visited[v] == 1) {
                valid = false;
                return;
            }

        }
        visited[n] = 2;
    }
}
```

#### [LeetCode 210.课程表II](https://leetcode-cn.com/problems/course-schedule-ii/)

```java
class Solution {

    List<List<Integer>> infos = new ArrayList<>();
    int[] visited;
    boolean vaild = true;
    List<Integer> ans = new ArrayList<>();
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        visited = new int[numCourses];

        for (int i = 0; i < numCourses; i++) {
            infos.add(new ArrayList<>());
        }

        for(int[] info : prerequisites) {
            infos.get(info[1]).add(info[0]);
        }

        for (int i = 0; i < numCourses; i++) {
            if(visited[i] == 0) {
                dfs(i);
            }
        }

        if(vaild) {
            return ans.stream().mapToInt(Integer::intValue).toArray();
        } else {
            return new int[0];
        }

    }

    private void dfs(int n) {
        visited[n] = 1;

        for(int v : infos.get(n)) {
            if(visited[v] == 0) {
                dfs(v);
                if(!vaild) {
                    return;
                }
            } else if(visited[v] == 1) {
                vaild = false;
                return;
            }
        }

        visited[n] = 2;
        ans.add(0, n);
    }
}
```

## 

## 最小生成树★★

#### [LeetCode 1584. 连接所有点的最小费用](https://leetcode-cn.com/problems/min-cost-to-connect-all-points/)

Kruskal(并查集)：贪心选（最短）边



```java
class Solution {

    class Edge {
        int len, x, y;

        public Edge(int len, int x, int y) {
            this.len = len;
            this.x = x;
            this.y = y;
        }
    }

    class DisjointSetUnion {
        int[] f;
        int[] rank;
        int n;

        public DisjointSetUnion(int n) {
            this.n = n;
            this.rank = new int[n]; //加入秩（经验）优化，把小堆加到大堆中，去掉也可以
            Arrays.fill(this.rank, 1);
            this.f = new int[n];

            for (int i = 0; i < n; i++) {
                this.f[i] = i;
            }
        }

        //查找父节点（顺带更新）
        public int find(int x) {
            return f[x] == x ? x : (f[x] = find(f[x]));
        }

        //合并两个节点
        public boolean unionSet(int x, int y) {
            int fx = find(x), fy = find(y);

            if(fx == fy) {
                return false;
            }

            if(rank[fx] < rank[fy]) {
                int temp = fx;
                fx = fy;
                fy = temp;
            }

            rank[fx] += rank[fy];
            f[fy] = fx;
            return true;
        }
    }

    public int dist(int[][] points, int x, int y) {
        return Math.abs(points[x][0] - points[y][0]) + Math.abs(points[x][1] - points[y][1]);
    }

    public int minCostConnectPoints(int[][] points) {

        int n = points.length;

        DisjointSetUnion dsu = new DisjointSetUnion(n);
        List<Edge> edges = new ArrayList<Edge>();

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                edges.add(new Edge(dist(points, i, j), i, j));
            }
        }

        //对边进行排序，方便后续选边 用堆也可以
        Collections.sort(edges, new Comparator<Edge>() {
            public int compare(Edge edge1, Edge edge2) {
                return edge1.len - edge2.len;
            }
        });

        int ret = 0, num = 1;

        for (Edge edge : edges) {
            int len = edge.len, x = edge.x, y = edge.y;
            //如果这条边上的两个点属于两个集合，则合并成功，将边的长度加入ret
            if(dsu.unionSet(x, y)) {
                ret += len;
                num++;

                if(num == n) {
                    break;
                }
            }
        }
        return ret;
    }
}
```

## 欧拉回路/欧拉通路

### Hierholzer 算法

Hierholzer 算法用于在连通图中寻找欧拉路径，其流程如下：

从起点出发，进行深度优先搜索。

每次沿着某条边从某个顶点移动到另外一个顶点的时候，都需要删除这条边。

如果没有可移动的路径，则将所在节点加入到栈中，并返回。

当我们顺序地考虑该问题时，我们也许很难解决该问题，因为我们无法判断当前节点的哪一个分支是「死胡同」分支。

不妨倒过来思考。我们注意到只有那个入度与出度差为 11 的节点会导致死胡同。而该节点必然是最后一个遍历到的节点。我们可以改变入栈的规则，当我们遍历完一个节点所连的所有节点后，我们才将该节点入栈（即逆序入栈）。

对于当前节点而言，从它的每一个非「死胡同」分支出发进行深度优先搜索，都将会搜回到当前节点。而从它的「死胡同」分支出发进行深度优先搜索将不会搜回到当前节点。也就是说当前节点的死胡同分支将会优先于其他非「死胡同」分支入栈。

这样就能保证我们可以「一笔画」地走完所有边，最终的栈中逆序地保存了「一笔画」的结果。我们只要将栈中的内容反转，即可得到答案。

#### [LeetCode 332. 重新安排行程](https://leetcode-cn.com/problems/reconstruct-itinerary/)

```java
class Solution {
    Map<String, PriorityQueue<String>> map = new HashMap<String, PriorityQueue<String>>();
    List<String> itinerary = new LinkedList<String>();

    public List<String> findItinerary(List<List<String>> tickets) {
        for (List<String> ticket : tickets) {
            String src = ticket.get(0), dst = ticket.get(1);
            if (!map.containsKey(src)) {
                map.put(src, new PriorityQueue<String>());
            }
            map.get(src).offer(dst);
        }
        dfs("JFK");
        Collections.reverse(itinerary);
        return itinerary;
    }

    public void dfs(String curr) {
        while (map.containsKey(curr) && map.get(curr).size() > 0) {
            String tmp = map.get(curr).poll();
            dfs(tmp);
        }
        itinerary.add(curr);
    }
}
```



#### [LeetCode 753. 破解保险箱](https://leetcode-cn.com/problems/cracking-the-safe/)



## 字典树★★

## 线段树

```java
//用来求区间最大/最小值
class SegmentTree {
        int n;
        int[] tree;
        boolean iSMAX;

        public SegmentTree(int[] nums, boolean isMax) {
            n = nums.length;
            tree = new int[n << 1];
            this.iSMAX = isMax;
            buildTree(nums, isMax);
        }

        private void buildTree(int[] nums, boolean isMax) {
            //构造tree的n - 2n-1部分
            for (int i = n, j = 0; i < (n << 1); i++, j++) {
                tree[i] = nums[j];
            }
            //构造tree的1-n-1部分
            for (int i = n - 1; i > 0; i--) {
                if(isMax) {
                    tree[i] = Math.max(tree[i << 1], tree[(i << 1) + 1]);
                } else {
                    tree[i] = Math.min(tree[i << 1], tree[(i << 1) + 1]);
                }
            }
        }
    
        public void update(int i, int val) {
            i += n;//nums的索引与tree的索引相差n
            tree[i] = val;
            while (i > 0) {
                int left = i;
                int right = i;
                if ((i & 1) == 0) {
                    right = i + 1;//i为左孩子
                } 
                else {
                    left = i - 1;//i为右孩子
                } 
                tree[i >> 1] = tree[left] + tree[right];
                if(iSMAX) {
                	 tree[i >> 1] = Math.max(tree[left], tree[right]);
            	} else {
                	tree[i >> 1] = Math.min(tree[left], tree[right]);
            	}
                i >>= 1;
            }
        }

        public int queryRange(int i, int j) {
            //nums的索引与tree的索引相差n
            i += n;
            j += n;
            int ans = 0;
            if(iSMAX) {
                ans = Integer.MIN_VALUE;
            } else {
                ans = Integer.MAX_VALUE;
            }

            while (i <= j) {
                //目的是维持[i,j]我左右孩子，或者一个节点本身
                if ((i & 1) == 1) {//i为右孩子
                    if(iSMAX) {
                        ans = Math.max(ans, tree[i]);
                    } else {
                        ans = Math.min(ans, tree[i]);
                    }
                    i++;
                }

                if ((j & 1) == 0) {//j为左孩子
                    if(iSMAX) {
                        ans = Math.max(ans, tree[j]);
                    } else {
                        ans = Math.min(ans, tree[j]);
                    }
                    j--;
                }
                i >>= 1;
                j >>= 1;
            }
            return ans;
        }
    }
```

### [307. 区域和检索 - 数组可修改](https://leetcode-cn.com/problems/range-sum-query-mutable/)

```java
//求区间和
class NumArray {
    private int[] segmentTree;
    private int n;

    public NumArray(int[] nums) {
        n = nums.length;
        segmentTree = new int[nums.length * 4];
        build(0, 0, n - 1, nums);
    }

    public void update(int index, int val) {
        change(index, val, 0, 0, n - 1);
    }

    public int sumRange(int left, int right) {
        return range(left, right, 0, 0, n - 1);
    }

    private void build(int node, int s, int e, int[] nums) {
        if (s == e) {
            segmentTree[node] = nums[s];
            return;
        }
        int m = s + (e - s) / 2;
        build(node * 2 + 1, s, m, nums);
        build(node * 2 + 2, m + 1, e, nums);
        segmentTree[node] = segmentTree[node * 2 + 1] + segmentTree[node * 2 + 2];
    }

    private void change(int index, int val, int node, int s, int e) {
        if (s == e) {
            segmentTree[node] = val;
            return;
        }
        int m = s + (e - s) / 2;
        if (index <= m) {
            change(index, val, node * 2 + 1, s, m);
        } else {
            change(index, val, node * 2 + 2, m + 1, e);
        }
        segmentTree[node] = segmentTree[node * 2 + 1] + segmentTree[node * 2 + 2];
    }

    private int range(int left, int right, int node, int s, int e) {
        if (left == s && right == e) {
            return segmentTree[node];
        }
        int m = s + (e - s) / 2;
        if (right <= m) {
            return range(left, right, node * 2 + 1, s, m);
        } else if (left > m) {
            return range(left, right, node * 2 + 2, m + 1, e);
        } else {
            return range(left, m, node * 2 + 1, s, m) + range(m + 1, right, node * 2 + 2, m + 1, e);
        }
    }
}
```

# 数学

## 简单四则运算（带括号）

```java
public class Main {
    public static long calculate(String s) {
        List<String> sStr = spiltStr(s);
        Stack<Character> operators = new Stack<>();
        Stack<Long> operatornums = new Stack<>();

        int sLen = sStr.size();
        for (int i = 0; i < sLen; i++) {
            String str = sStr.get(i);
            if(isNumber(str)) {
                operatornums.push(Long.valueOf(str));
            } else {
                char c = str.charAt(0);
                if(operators.isEmpty() || c == '(') {
                    operators.push(c);
                } else if(c == ')') {
                    while (operators.peek() != '(') {
                        popAndCal(operators,operatornums);
                    }
                    operators.pop();
                } else if(c == '+' || c == '-') {
                    char topC = operators.peek();
                    while (topC == '+' || topC == '-' || topC == '*' || topC == '/') {
                        popAndCal(operators, operatornums);
                        topC = operators.isEmpty() ? ' ' : operators.peek();
                    }
                    operators.push(c);
                } else if(c == '*' || c == '/') {
                    char topC = operators.peek();
                    while (topC == '*' || topC == '/') {
                        popAndCal(operators, operatornums);
                        topC = operators.isEmpty() ? ' ' : operators.peek();
                    }
                    operators.push(c);
                }
            }
        }

        while (!operators.isEmpty()) {
            popAndCal(operators, operatornums);
        }

        return operatornums.pop();
    }

    public static boolean isNumber(String s) {
        return Character.isDigit(s.charAt(0));
    }

    public static List<String> spiltStr(String s) {
        List<String> res = new LinkedList<>();
        StringBuffer sb;

        int sLen = s.length();
        for (int i = 0; i < sLen; ) {
            if(i < sLen && s.charAt(i) == ' ') {
                i++;
                continue;
            } else if(!Character.isDigit(s.charAt(i))) {
                res.add(String.valueOf(s.charAt(i++)));
            } else {
                sb = new StringBuffer();
                while (i < sLen && Character.isDigit(s.charAt(i))) {
                    sb.append(s.charAt(i));
                    i++;
                }
                res.add(sb.toString());
            }
        }
        return res;
    }

    public static void popAndCal(Stack<Character> operators, Stack<Long> opernums) {
        long num2 = opernums.pop();
        long num1 = opernums.pop();
        char op = operators.pop();
        opernums.push(exe(num1, num2, op));
    }

    public static long exe(long num1, long num2, char op) {
        long res = 0L;
        switch (op) {
            case '+' :
                res = num1 + num2;
                break;
            case '-' :
                res = num1 - num2;
                break;
            case '*' :
                res = num1 * num2;
                break;
            case '/' :
                res = num1 / num2;
                break;
            default:
                break;
        }
        return res;
    }

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        System.out.println(calculate(in.nextLine()));
    }
}
```



##  快速幂

```java
private static long quickPow(long x, long y, long mod) {
    long ans = 1L;
    while (y > 0) {
        if((y & 1) == 1) {
            ans *= x;
        }
        x *= x;
        x %= mod;
        ans %= mod;
        y >>= 1;
    }
        return ans;
}
```

##  矩阵快速幂

### [LeetCode 552. 学生出勤记录 II](https://leetcode-cn.com/problems/student-attendance-record-ii/)

```java
public long[][] pow(long[][] mat, int n) {
    long[][] ret = {{1, 0, 0, 0, 0, 0}}; //初始矩阵  相当于快速幂中的1
    while (n > 0) {
        if ((n & 1) == 1) {
            ret = multiply(ret, mat);
        }
        n >>= 1;
        mat = multiply(mat, mat);
    }
    return ret;
}

//矩阵相乘
public long[][] multiply(long[][] a, long[][] b) {
    int rows = a.length, columns = b[0].length, temp = b.length;
    long[][] c = new long[rows][columns];
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < columns; j++) {
            for (int k = 0; k < temp; k++) {
                c[i][j] += a[i][k] * b[k][j];
                c[i][j] %= MOD;
            }
        }
    }
    return c;
}
```

## 素数

[204. 计数质数](https://leetcode-cn.com/problems/count-primes/)

### 埃氏筛

```java
class Solution {
    public int countPrimes(int n) {
        int[] isPrime = new int[n];
        Arrays.fill(isPrime, 1);
        int ans = 0;
        for (int i = 2; i < n; ++i) {
            if (isPrime[i] == 1) {
                ans += 1;
                if ((long) i * i < n) {
                    for (int j = i * i; j < n; j += i) {
                        isPrime[j] = 0;
                    }
                }
            }
        }
        return ans;
    }
}
```

### 线性筛

```java
//线性筛 复杂度O(N)
class Solution {
    public int countPrimes(int n) {
        List<Integer> primes = new ArrayList<Integer>();
        int[] isPrime = new int[n];
        Arrays.fill(isPrime, 1);
        for (int i = 2; i < n; ++i) {
            if (isPrime[i] == 1) {
                primes.add(i);
            }
            for (int j = 0; j < primes.size() && i * primes.get(j) < n; ++j) {
                isPrime[i * primes.get(j)] = 0;
                //防止重复筛
                if (i % primes.get(j) == 0) {
                    break;
                }
            }
        }
        return primes.size();
    }
}
```

# 动态规划★★★★

找准状态转移方程

## 背包问题

### 0-1背包

#### 题目

有N件物品和一个容量为V的背包。放入第i件物品费用是$C_i$，得到的价值是$V_i$。求解将哪些物品放入背包可以使得总价值最大。

#### 基本思路

每种物品仅有一件，可以选择放或者不放。

用子问题定义状态：$F[i,v]$表示前$i$件物品放入一个容量为$v$的背包可以获得的最大价值。则状态转移方程是：
$$
F[i,v]=max\{F[i-1,v],F[i-1,v-C_i]+W_i\}
$$

$$
F[0...V] \enspace \leftarrow \enspace 0
\\
for \enspace i \leftarrow \enspace1 \enspace to \enspace N
\\
\qquad for \enspace v \enspace \leftarrow {C_i} \enspace to \enspace V
\\
\qquad \qquad F[i,v] \leftarrow max\{F[i-1,v],F[i-1,v-C_i]+W_i\}
$$

#### 优化空间复杂度

空间复杂度可以优化至O(V)。

$$F[i,v]$$只与$$F[i-1,v],F[i-1,v-C_i]$$有关，可以用一维数组进行优化。第二个循环需要递减计算，这样才能够保证计算$F[v]$的时候$F[v-{C_i}]$保存的是状态$F[i-1,v-{C_i}]$的值。
$$
F[0...V] \enspace \leftarrow \enspace 0
\\
for \enspace i \leftarrow \enspace1 \enspace to \enspace N
\\
\qquad for \enspace v \enspace \leftarrow V \enspace to \enspace {C_i}
\\
\qquad \qquad F[v] \leftarrow max\{F[v],F[v-C_i]+W_i\}
$$

#### 初始化的细节问题

0-1背包有两种不太相同的问法，一种是刚好装满下最大价值，另一种则不要求刚好装满。这两种问法的解决方案的区别在于初始化的时候有所不同。

对于要求恰好装满的，则应令$F[0]=0$，$F[1...V]=-\infty$，对于不要求恰好装满的，则只需令$F[0...V]=0$。

####  JAVA模板

```java
//时间复杂度：O(VN) 空间复杂度：O(VN)
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        //背包容量 value[0]表示物品数量
        int capacity = Integer.parseInt(in.nextLine()); 
        int[] value = Arrays.stream(in.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray(); 
        
        //每个物品对应价值 weight[0]表示物品数量
        int[] weight = Arrays.stream(in.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray(); 

        int[][] dp = new int[capacity + 1][value[0] + 1];
        for (int i = 1; i <= value[0]; i++) {
            int v = value[i];
            int w = weight[i];
            for (int j = 1; j <= capacity; j++) {
                dp[j][i] = j < w ? dp[j][i - 1] : Math.max(dp[j][i - 1], dp[j - w][i - 1] + v);
            }
        }
        System.out.println(dp[capacity][value[0]]);
    }
}
```

```java
//时间复杂度：O(VN) 空间复杂度：O(V)
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        //背包容量 value[0]表示物品数量
        int capacity = Integer.parseInt(in.nextLine()); 
        int[] value = Arrays.stream(in.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray(); 
        
        //每个物品对应价值 weight[0]表示物品数量
        int[] weight = Arrays.stream(in.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray(); 

        int[] dp = new int[capacity + 1];
        for (int i = 1; i <= value[0]; i++) {
            int v = value[i];
            int w = weight[i];
            for (int j = capacity; j >= w; j--) {
                dp[j] = Math.max(dp[j], dp[j - w] + v);
            }
        }
        System.out.println(dp[capacity]);
    }
}
```

### 完全背包

#### 题目

有N中物品和一个容量为V的背包，每种物品都有无线件可以用。放入第$i$种物品的费用是${C_i}$，价值是${W_i}$。求解：将哪些物品装入背包，可以使得这些物品总重量不多于V的情况下价值最大。

#### 基本思路

与0-1背包不同的是物品有无限件。对于每种物品最多有$k=\lfloor  V/{C_i} \rfloor$种状态。按照0-1背包思路可得状态转移方程：
$$
F[i,v] = max\{F[i-1,v-k{C_i}] + k{W_i} \enspace | \enspace 0 \le k{W_i} \le v\}
$$
求解$F[i,v]$的时间是$O(\frac{v}{C_i})$，总体的时间复杂度是$O(NV\sum\frac{v}{C_i})$。

#### 优化

若两件物品$i$、$j$满足${C_i} \leq {C_j}$ 且${W_i} \geq {W_j}$，则可以将物品j直接去掉，不用考虑。这个优化可以$O(N^2)$实现。

进一步的，可以将费用大于$V$的物品去掉，对于费用相同的物品选取价值最高的那个，采用HashMap一次遍历就可以完成。

#### 转化为0-1背包问题求解

基本思路中提到的转化方法效率低，时间复杂度高。高效的转化是二进制思想。把第$i$种物品拆分成费用${C_i}{2^k}$、价值为${W_i}{2^k}$的若干件物品，其中$k$取遍满足${C_i}{2^k} \leq V$的非负整数。

不管最优策略选多少件第$i$种物品，都可以表示成二进制，这样就可以把每种物品拆分成$log {\lfloor V/{C_i} \rfloor}$件物品。

#### $O(VN)$的算法

$$
F[0...V] \enspace \leftarrow \enspace 0
\\
for \enspace i \leftarrow \enspace1 \enspace to \enspace N
\\
\qquad for \enspace v \enspace \leftarrow {C_i} \enspace to \enspace V
\\
\qquad \qquad F[v] \leftarrow max\{F[v],F[v-C_i]+W_i\}
$$

与0-1背包不同的是$v \enspace \leftarrow {C_i} \enspace to \enspace V$，由于0-1背包只有选与不选两种状态，而在完全背包中，一件物品允许重复选，因此第二个for循环可以且必须这样设置。

#### JAVA模板

```java
//HDU 1114 http://acm.hdu.edu.cn/showproblem.php?pid=1114
//这个求的是恰好装满的最小值，因此初始化的时候将dp[0] = 0;其他设为无穷大 并且动态规划也是求的最小值
public class Test
{
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int T = Integer.parseInt(in.nextLine());
        while (T-- > 0) {
            int[] nums0 = Arrays.stream(in.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
            //背包容量
            int capacity = nums0[1] - nums0[0];

            //物品个数
            int N = Integer.parseInt(in.nextLine());
            //coins[i][0] 物品i的价值 conins[i][1]物品i的重量/消耗
            int[][] coins = new int[N][N];
            for (int i = 0; i < N; i++) {
                coins[i] = Arrays.stream(in.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
            }

            int[] dp = new int[capacity + 1];
            //为了防止后续计算溢出选取Integer.MAX_VALUE >> 1 作为无穷大
            Arrays.fill(dp, Integer.MAX_VALUE >> 1);
            dp[0] = 0;
            for (int i = 0; i < N; i++) {
                for (int j = coins[i][1]; j <= capacity; j++) {
                    dp[j] = Math.min(dp[j], dp[j - coins[i][1]] + coins[i][0]);
                }
            }

            if(dp[capacity] != (Integer.MAX_VALUE >> 1)) {
                System.out.println("The minimum amount of money in the piggy-bank is " + dp[capacity] + ".");
            } else {
                System.out.println("This is impossible.");
            }
        }
    }
}
```

### 多重背包

#### 题目

有N种物品和容量为V的背包。第i中物品最多有${M_i}$件可用，每件耗费空间是${C_i}$，价值是${W_i}$。求解将哪些物品装入背包可使这些物品耗费总空间不超过背包容量，且价值总和最大。

#### 基本算法

与完全背包类似，对于第$i$种物品有${M_i}+1$种策略：$[0...{M_i}]$。令$F[i,v]$表示前$i$种物品恰放入一个容量为$v$的背包的最大价值，则有状态转移方程：

$$F[i,v]=max{F[i-1, v-k*{C_i}] + k * {W_i} | 0 \leq k \leq {M_i}}$$

复杂度为$O(V\sum{M_i})$

### 混合背包

### 分组背包

### 有依赖的背包

## 典型的例题

### [LeetCode 552. 学生出勤记录 II](https://leetcode-cn.com/problems/student-attendance-record-ii/)

### [LeetCode 300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

### [LeetCode 673. 最长递增子序列的个数](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/)

### [LeetCode 1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

### [LeetCode 583. 两个字符串的删除操作](https://leetcode-cn.com/problems/delete-operation-for-two-strings/)

### References

[1] https://github.com/tianyicui/pack

# 贪心★★

# 二分法★★★★★

### [位1的个数](https://leetcode-cn.com/problems/number-of-1-bits/)

```java
//二进制中 n-1与n的区别是，n的最后一个1及后面的位取反也就是说 n与n-1在二进制位上1的个数始终差1
public class Solution {
    public int hammingWeight(int n) {
        int ret = 0;
        while (n != 0) {
            n &= n - 1;
            ret++;
        }
        return ret;
    }
}
```

```java
//二分思想 这个算法是一种合并计数器的策略。把输入数的32Bit当作32个计数器，代表每一位的1个数。然后合并相邻的2个“计数器”，使i成为16个计数器，每个计数器的值就是这2个Bit的1的个数；继续合并相邻的2个“计数器“，使i成为8个计数器，每个计数器的值就是4个Bit的1的个数。依次类推，直到将i变成一个计数器，那么它的值就是32Bit的i中值为1的Bit的个数。最后低6位即为结果。
public static int bitCount(int i) {
        i = i - ((i >>> 1) & 0x55555555);
        i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
        i = (i + (i >>> 4)) & 0x0f0f0f0f;
        i = i + (i >>> 8);
        i = i + (i >>> 16);
        return i & 0x3f;
}
```

# 二叉查找（搜索、排序，BST）树★★★★★



<div align='center'>
    <img src='./imgs/01.png' width=800px>
</div>

## [LeetCode 96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)

假设有n个结点，值为1-n，问有所少中不同的二叉搜索树。

卡特兰数

<div align='center'>
    <img src='./imgs/ProgramExercise/02.png' width=800px>
</div>



```java
class Solution {
    public int numTrees(int n) {
        // 提示：我们在这里需要用 long 类型防止计算过程中的溢出
        long C = 1;
        for (int i = 0; i < n; ++i) {
            C = C * 2 * (2 * i + 1) / (i + 2);
        }
        return (int) C;
    }
}
```

## [LeetCode 108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

将有序数组转为高度平衡的二叉搜索树，三种方法，1：中序遍历，每次以(left + right) >> 1 位置处数字为根节点；2：中序遍历，每次选取(left + right + 1) >> 1处的数字作为根节点；3：中序遍历，每次选取(left + right ) >> 1 、(left + right + 1) >> 1任一位置处的数字作为根节点。

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return createBST(nums, 0, nums.length - 1);
    }

    public TreeNode createBST(int[] nums, int left, int right) {
        if(left > right) {
            return null;
        }

        int mid = (left + right) >> 1;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = createBST(nums, left, mid - 1);
        root.right = createBST(nums, mid + 1, right);
        return root;
    }
}
```



# 并查集★★

# 分治法★★★



# 哈希表★★★★★

# 堆★★★

# 差分数组

## [LeetCode 1109. 航班预订统计](https://leetcode-cn.com/problems/corporate-flight-bookings/)

# 扫描线