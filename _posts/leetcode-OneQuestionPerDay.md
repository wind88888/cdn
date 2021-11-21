---
title: leetcode-OneQuestionPerDay
date: 2021-11-15 16:39:00
tags: leetcode
---

# 一、每日一题

## 第一周：11.15 -> 11.21

### 1.灯泡开关

![](https://i.loli.net/2021/11/21/HDZq2i56QoYjgCR.png)

**方法一:暴力**

```c
int bulbSwitch(int n){
    if (n == 0) {
        return 0;
    }
    bool arr[n];
    memset(arr, true, sizeof(arr));//

    for (int i = 2; i <= n; i++) {
        for (int j = 0; j < n; j++) {
            if ((j + 1) % i == 0) {
                arr[j] = !arr[j];
            }
        }
    }

    int res = 0;
    for (int k = 0; k < n; k++) {
        if (arr[k]) {
            res++;
        }
    }

    return res;
}
```

**复杂度分析**

- 时间复杂度：O($n^2$)。
- 空间复杂度：O(1)。

**方法二：数学法**

![](https://i.loli.net/2021/11/21/CNpxbPU9okhWXIu.png)

```c
int bulbSwitch(int n){
    return sqrt(n+0.5);
}
```

**复杂度分析**

- 时间复杂度：O(1)。
- 空间复杂度：O(1)。

### 2.[完美矩形](https://leetcode-cn.com/problems/perfect-rectangle/)

![](https://i.loli.net/2021/11/21/c74lApw3LGvEnWi.png)

![](https://i.loli.net/2021/11/21/jevJcE7aHwFV4ri.png)

```c
//存储坐标点(x,y)
typedef pair<int, int> Point;

class Solution {
public:
    bool isRectangleCover(vector<vector<int>>& rectangles) {
        //记录各点的出现次数
        map<Point, int> cnt;
        int minX = rectangles[0][0], minY = rectangles[0][1], maxX = rectangles[0][2], maxY = rectangles[0][3];
        long area = 0;

        //遍历每个数组：累加小矩阵面积，获得四个角点坐标，记录小矩阵角点次数
        for (auto & rect : rectangles) {
            int x = rect[0], y = rect[1], a = rect[2], b = rect[3];
            area += (long) (a - x) * (b - y);

            minX = min(x, minX);
            minY = min(y, minY);
            maxX = max(a, maxX);
            maxY = max(b, maxY);

            Point point1({x, y});
            Point point2({x, b});
            Point point3({a, y});
            Point point4({a, b});

            cnt[point1] += 1;
            cnt[point2] += 1;
            cnt[point3] += 1;
            cnt[point4] += 1;
        }

        //大矩阵的四个顶点
        Point pointMinMin({minX, minY});
        Point pointMinMax({minX, maxY});
        Point pointMaxMin({maxX, minY});
        Point pointMaxMax({maxX, maxY});

        //条件1：各矩阵面积等于大矩阵面积
        if (area != (maxX - minX) * (maxY - minY)) {
            return false;
        }
        //条件2：(1)大矩阵四个顶点出现1次
        if (cnt[pointMinMin] != 1 || cnt[pointMinMax] != 1 || cnt[pointMaxMin] != 1 || cnt[pointMaxMax] != 1) {
            return false;
        }

        //条件2：(2)所有角点(除了大矩阵的四个顶点)，出现2或者4次
        cnt.erase(pointMinMin);
        cnt.erase(pointMinMax);
        cnt.erase(pointMaxMin);
        cnt.erase(pointMaxMax);

        for (auto & entry : cnt) {
            int value = entry.second;
            if (value != 2 && value != 4) {
                return false;
            }
        }

        return true;
    }
};
```

**复杂度分析**

- 时间复杂度：O(n)。
- 空间复杂度：O(n)。

### 3.最大单词长度乘积

![](https://i.loli.net/2021/11/21/PREAv87BuVCNWOM.png)

方法一：暴力

```c
class Solution {
public:
    bool isNotSameElem(string word1, string word2) {
        for (int i = 0; i < word1.size(); i++) {
            if (word2.find(word1[i]) != string::npos) {
                return false;
            }
        }

        return true;
    }
    int maxProduct(vector<string>& words) {
        int res = 0;
        for (auto & word1 : words) {
            for (auto & word2 : words) {
                if (word1 == word2) {
                    continue;
                }

                if (isNotSameElem(word1, word2)) {
                    int len = word1.size() * word2.size();
                    res = max(res, len);
                }
            }
        }

        return res;
    }
};
```

![](https://i.loli.net/2021/11/21/Qqh5AfXyBJgcPSv.png)

方法二：位运算

![](https://i.loli.net/2021/11/21/KEYeWS6tLVw8M9s.png)

```c
class Solution {
public:
    int maxProduct(vector<string>& words) {
        int size = words.size();
        // vector<int> masks(size);
        int masks[size];
        memset(masks, 0, sizeof(masks));
        for (int i = 0; i < size; i++) {
            string word = words[i];
            int wordSize = word.size();
            for (int j = 0; j < wordSize; j++) {
                masks[i] |= 1 << (word[j] - 'a');
            }
        }
        int res = 0;
        for (int i = 0; i < size; i++) {
            for (int j = i + 1; j < size; j++) {
                if ((masks[i] & masks[j]) == 0) {
                    int len = words[i].size() * words[j].size();
                    res = max(res, len);
                }
            }
        }

        return res;
    }
};
```

**sizeof知识点补充：**

sizeof以字节为单位给出对象的大小：

> - 整型数组
>
>   - ```c
>     int fwre[] = {1,2,3};
>     cout << sizeof(fwre) << endl;//3*4=12
>     ```
>
> - 字符数组
>
>   - sizeof:
>
>     ```c
>     char tret[20] = "fhtgdg";
>     cout << sizeof(tret) << endl;//20 * 1 = 20
>     cout << strlen(tret) << endl;//6
>     cout << sizeof(char) << endl;//1
>     
>     #define PARAMETER "YOU ARE PERSON."(15个字符)
>     cout << sizeof(PARAMETER) << endl;//16 * 1 = 16； 因为还有一个‘\0’结束符
>     cout << strlen(PARAMETER) << endl;//15
>     ```

 [进阶](https://blog.csdn.net/Namcodream521/article/details/85315955)：

```c
#include <stdio.h>

int main(void)
{
    char ca[] = {"123456"};
    char *pca = "123456";
    				printf("sizeof(ca)=%d,sizeof(pca)=%d,sizeof(*pca)=%d\n",sizeof(ca),sizeof(pca),sizeof(*pca));

    char cb[3][6] = {"123456","123456","123456"};
    char *pcb[] = {"123456","123456","123456"};

        printf("sizeof(cb)=%d,sizeof(pcb)=%d,sizeof(*pcb)=%d,sizeof(**pcb)=%d\n",sizeof(cb),sizeof(pcb),sizeof(*pcb),sizeof(**pcb));

    return 0;
}
```

```c
//32位
$ gcc sz.c -o sz -m32
[nereus@nereusp cpp]$ ./sz
sizeof(ca)=7,sizeof(pca)=4,sizeof(*pca)=1
sizeof(cb)=18,sizeof(pcb)=12,sizeof(*pcb)=4,sizeof(**pcb)=1
 
//64位
$ gcc sz.c -o sz
$ ./sz
sizeof(ca)=7,sizeof(pca)=8,sizeof(*pca)=1
sizeof(cb)=18,sizeof(pcb)=24,sizeof(*pcb)=8,sizeof(**pcb)=1
```

> - sizeof(数组名)：返回的是数组的大小(**一维字符数组有字符结束符，二维没有**)
> - sizeof(一维数组指针)：返回的是数组单个元素指针的大小，即系统指针的长度，32位系统为4，64位系统位8
> - sizeof(*一维数组指针)：返回的是数组单个元素对应类型的大小
> - sizeof(二维数组指针)：返回的是二维数组行指针的大小，32位系统为行数×4，64位系统位行数×8
>   - sizeof(pcb)等同于sizeof(pcb[0])+sizeof(pcb[1])+sizeof(pcb[2])（32位系统）
> - sizeof(*二维数组指针)：返回的是系统指针的长度，32位系统为4，64位系统位8
> - sizeof(**二维数组指针)：返回的是数组单个元素指针的大小，即char的大小



**[memset函数讲解](https://www.cplusplus.com/reference/cstring/memset/)：**

```c
void * memset ( void * ptr, int value, size_t num );
Sets the first num bytes of the block of memory pointed by ptr to the specified value (interpreted as an unsigned char).

当给int数组初始化的话：
int masks[5];
//数组必须初始化，否则相应地址里的值就是原本的值，不是自己赋的值
初始化为0：
方法一：
int ma[5] = {0}; //第一个为0，后面就默认都初始化为0
方法二：
memset(masks, 0, sizeof(masks));//masks={0,0,0,0,0};

初始化为1：
只能循环赋值，不能使用上述方法一和二
int ma[5] = {0}; //masks={1,0,0,0,0};
memset(masks, 1, sizeof(masks));//masks={16843009,16843009,16843009,16843009,16843009};
因为根据memset的定义，是每一个字节的首位赋值为value,又因为int型是4个字节，所以总共20个字节，运行函数之后是：
{pow(2,0) + pow(2,8) + pow(2,16) +pow(2,24), pow(2,0) + pow(2,8) +pow(2,16) +pow(2,24), pow(2,0) + pow(2,8) + pow(2,16) + pow(2,24), pow(2,0) + pow(2,8) +pow(2,16) +pow(2,24), pow(2,0) + pow(2,8) + pow(2,16) + pow(2,24)};
```

### 4.二叉树的坡度

![](https://i.loli.net/2021/11/21/3CqvQ4se8HO2Pgu.png)

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

int res;

int dfs(struct TreeNode *node)
{
    if (node == NULL)
    {
        return 0;
    }
    int lsum = dfs(node->left);
    int rsum = dfs(node->right);
    res += abs(lsum - rsum);
    return lsum + rsum + node->val;
}

int findTilt(struct TreeNode* root){
    res = 0;
    dfs(root);
    return res;
}
```

### 5.整数替换

![](https://i.loli.net/2021/11/21/8UkSLAGatQcevnI.png)

方法一：BFS

```java
class Solution {
    public int integerReplacement(int n) {
        Queue<Long> queue = new LinkedList<>();
        int steps = 0;
        queue.add((long)n);

        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                long num = queue.poll();
                if (num == 1) {
                    return steps;
                }
                if ((num & 1) == 0) {
                    queue.offer(num / 2);
                } else {
                    queue.offer(num - 1);
                    queue.offer(num + 1);
                }
            }
            steps++;
        }
        return -1;
    }
}
```

方法二：递归

```c
int integerReplacement(int n){
    if (n == 1) {
        return 0;
    }

    if ((n & 1) == 0) {
        return 1 + integerReplacement(n / 2);
    }

    return 2 + fmin(integerReplacement(n / 2), integerReplacement((n / 2) + 1));
}
```

### 6.最长和谐子序列

![](https://i.loli.net/2021/11/21/q56KajJuR3W4Bfz.png)

方法一：滑动窗口

```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {

        sort(nums.begin(), nums.end());

        int size = nums.size();
        int begin = 0;
        int res = 0;
        for (int end = 0; end < size; end++) {
            
            //更新窗口长度
            //不能大于是因为可能整个数组的差都不大于1：【1，2，2，1】
            if (nums[end] == nums[begin] + 1) {
                res = max(res, end - begin + 1);
            }

            //开始新的窗口
            while (nums[begin] < nums[end] - 1) {
                begin++;
            }
        }

        return res;
    }
};
```

方法二：哈希表

```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        map<int, int> m;
        int res = 0;

        for (int num : nums) {
            m[num]++;
        }

        for (auto [key, value] : m) {
            if (m.count(key + 1)) {
                res = max(res, value + m[key + 1]);
            }
        }

        return res;
    }
};
```

### 7.N叉树的最大深度

![](https://i.loli.net/2021/11/21/ywEAUu3F2ZOInig.png)

![](https://i.loli.net/2021/11/21/wXFTljphuHfgUob.png)

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    int maxDepth(Node* root) {
        if (root == nullptr) {
            return 0;
        }
        int res = 0;
        vector<Node*> childs = root->children;
        for (auto child : childs) {
            int depth = maxDepth(child);
            res = max(depth, res);
        }

        return res+1;
    }
};
```



# 二、周赛

## 第一周：11-21

结果：两题

### 1.两栋颜色不同且距离最远的房子

![](https://i.loli.net/2021/11/21/cbnBgAT1P2L4NjO.png)

```java
class Solution {
    public int maxDistance(int[] colors) {
        int res = 0;
        int len = colors.length;
        
        for (int i = 0; i < len; i++) {
            for (int j = i+1; j < len; j++) {
                if (colors[j] == colors[i]) {
                    continue;
                }
                
                res = Math.max(res, j-i);
            }
        }
        
        return res;
    }
}
```



### 2.给植物浇水

![](https://i.loli.net/2021/11/21/e3uIwAQCBz7WVOn.png)

![](https://i.loli.net/2021/11/21/Ni5xtzWIuLP8TsU.png)

模拟法：

```java
class Solution {
    public int wateringPlants(int[] plants, int capacity) {
        int len = plants.length;
        int num = capacity;
        int res = 0;
        
        for (int end = 0; end < len; end++) {
            //足够的水去消耗
            while (end < len && plants[end] <= num) {
                num -= plants[end];
                end++;
            }
            end--;
            res += (end + 1) * 2;//往返
            
            num = capacity;//打满水
            //end++;
        }
        //最后一次没有回去
        return res - len;
    }
}
```

### 3.区间内查询数字的频率

![](https://i.loli.net/2021/11/21/IN6uM3Hs7QkBpLg.png)

方法一：暴力查询(超时)

```java
class RangeFreqQuery {
    private int[] arrys;
    public RangeFreqQuery(int[] arr) {
        int len = arr.length;
        arrys = new int[len];
        for (int i = 0; i < len; i++) {
            arrys[i] = arr[i];
        }
    }
    
    public int query(int left, int right, int value) {
        int res = 0;
        for (int i = left; i <= right; i++) {
            if (arrys[i] == value) {
                res++;
            }
        }
        return res;
    }
}

/**
 * Your RangeFreqQuery object will be instantiated and called as such:
 * RangeFreqQuery obj = new RangeFreqQuery(arr);
 * int param_1 = obj.query(left,right,value);
 */
```

方法二：二分查找 -> 寻找左边界(超时)

```java
class RangeFreqQuery {
    private int[] arrys;
    public RangeFreqQuery(int[] arr) {
        int len = arr.length;
        arrys = new int[len];
        for (int i = 0; i < len; i++) {
            arrys[i] = arr[i];
        }
    }
    
    public int query(int left, int right, int value) {
        int res = 0;
        int[] copy = Arrays.copyOfRange(arrys, left, right+1);
        Arrays.sort(copy);

        int tmpL = 0;
        int tmpR = copy.length;

        int targetIndex = 0;

        while (tmpL < tmpR) {
            int mid = tmpL + (tmpR - tmpL) / 2;
            if (copy[mid] == value) {
                tmpR = mid;
            } else if (copy[mid] > value) {
                tmpR = mid;
            } else {
                tmpL = mid + 1;
            }
        }

        targetIndex = tmpL;

        while (targetIndex < copy.length && copy[targetIndex] == value) {
            res++;
            targetIndex++;
        }

        return res;
    }
}

/**
 * Your RangeFreqQuery object will be instantiated and called as such:
 * RangeFreqQuery obj = new RangeFreqQuery(arr);
 * int param_1 = obj.query(left,right,value);
 */
```

应该是query()的复制数组和排序导致的复杂度过高，应该尝试将排序仿真构造函数中。

方法三：[对位置进行二分](https://leetcode-cn.com/problems/range-frequency-queries/solution/java-er-fen-by-merickbao-2-phux/)(太牛了)

```java
class RangeFreqQuery {
    List<List<Integer>> all = new ArrayList<>();

    public RangeFreqQuery(int[] arr) {
        for (int i = 0; i <= 10000; i++) {
            all.add(new ArrayList<>());
        }
        for (int i = 0; i < arr.length; i++) {
            // 下标是按顺序加入的，所以是有序的，所以后面可以进行二分
            all.get(arr[i]).add(i);
        }
     }
    
    public int query(int left, int right, int value) {
        if (all.get(value).size() == 0) return 0;
        List<Integer> now = all.get(value);
        // 第一次二分找左端点下标
        int l = 0, r = now.size() - 1;
        while (l < r) {
            int mid = (r - l) / 2 + l;
            if (now.get(mid) < left) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        int a = l;
        // 不存在这样的左端点
        if (now.get(a) > right || now.get(a) < left) return 0;
        // 第二次二分，找右端点的下标
        l = a;
        r = now.size() - 1;
        while (l < r) {
            int mid = (r - l) / 2 + l;
            if (now.get(mid) < right) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        int b = l;
        if (now.get(b) > right) {
            b--;
        }
        return b - a + 1;
    }
}

/**
 * Your RangeFreqQuery object will be instantiated and called as such:
 * RangeFreqQuery obj = new RangeFreqQuery(arr);
 * int param_1 = obj.query(left,right,value);
 */
```

### 4.[k 镜像数字的和](https://leetcode-cn.com/problems/sum-of-k-mirror-numbers/)



