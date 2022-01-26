# DFS 回溯法
[2151. 基于陈述统计最多好人数](https://leetcode-cn.com/problems/maximum-good-people-based-on-statements/submissions/)

```Java
import java.util.Arrays;

class Solution {
    private static int n;
    private static int res;

    public int maximumGood(int[][] statements) {
        n = statements.length;
        res = 0;

        int[] path = new int[n];
        Arrays.fill(path, 2);
        dfs(statements, path, 0);
        return res;
    }

    // 搜索第 u 个位置, path[u] 是前面的好人对 u 的论断
    private static void dfs(int[][] statements, int[] path, int u) {
        // 这里为什么使用 pathCopy 呢，因为 dfs 完成后需要恢复现场，用pathCopy 可以不改变 path 本身，免去恢复现场这个步骤
        int[] pathCopy = Arrays.copyOf(path, path.length);
        if (u == n) {
            res = Math.max(res, numOfGood(pathCopy));
            return;
        }
        if (pathCopy[u] == 0) {
            // 坏人的论断不用管
            dfs(statements, pathCopy, u + 1);
        } else if (pathCopy[u] == 1) {
            // 需要判断该好人的论断是否与前面的好人的论断有矛盾
            for (int j = 0; j < n; j++) {
                if (statements[u][j] != 2 && pathCopy[j] != 2 && statements[u][j] != pathCopy[j]) return;
                if (statements[u][j] != 2 && pathCopy[j] == 2) pathCopy[j] = statements[u][j];
            }
            dfs(statements, pathCopy, u + 1);
        } else { // path[u] == 2
            // 他可以是好人
            pathCopy[u] = 1;
            dfs(statements, pathCopy, u);
            // 也可以是坏人
            pathCopy[u] = 0;
            dfs(statements, pathCopy, u + 1);
        }
    }

    private static int numOfGood(int[] path) {
        int cnt = 0;
        for (int j = 0; j < n; j++) {
            if (path[j] == 1) cnt++;
        }
        return cnt;
    }
}
```
- 回溯法一条路走到头后是一定要恢复进入该层 dfs 函数之前的状态的，本题目在原 path 数组上面恢复非常困难，所以直接 copy 一个新的 path，这样就可以避免这个问题。

## 组合
[77.组合](https://leetcode-cn.com/problems/combinations/)

```Java
class Solution {
    private final static int N = 25;
    private final static boolean[] st = new boolean[N];

    private static int nn, kk;

    private final static List<List<Integer>> res = new ArrayList<>();

    public List<List<Integer>> combine(int n, int k) {
        nn = n;
        kk = k;
        res.clear();

        List<Integer> path = new ArrayList<>();
        dfs(0, path, 0);
        return res;
    }
    private static void dfs(int u, List<Integer> path, int pre) {
        if (u == kk) {
            res.add(new ArrayList<>(path));
        }
        for (int i = pre + 1; i <= nn; i++) {
            if (!st[i]) {
                st[i] = true;
                path.add(i);
                dfs(u + 1, path, i);
                path.remove(path.size() - 1);
                st[i] = false;
            }
        }
    }
}
```

[17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)
```Java
class Solution {
    private final static Map<Character, char[]> m = new HashMap<>();
    private static String s;
    private static int n;
    public List<String> letterCombinations(String digits) {
        List<String> res = new ArrayList<>();
        if (digits == null || "".equals(digits)) return res;
        initMap();
        n = digits.length();
        s = digits;

        StringBuilder path = new StringBuilder();
        dfs(0, path, res);
        return res;
    }
    private static void dfs(int u, StringBuilder path, List<String> res) {
        if (u == n) {
            res.add(path.toString());
            return;
        }
        for (char c: m.get(s.charAt(u))) {
            path.append(c);
            dfs(u + 1, path, res);
            path.deleteCharAt(path.length() - 1);
        }

    }
    private static void initMap() {
        m.put('2', new char[]{'a', 'b', 'c'});
        m.put('3', new char[]{'d', 'e', 'f'});
        m.put('4', new char[]{'g', 'h', 'i'});
        m.put('5', new char[]{'j', 'k', 'l'});
        m.put('6', new char[]{'m', 'n', 'o'});
        m.put('7', new char[]{'p', 'q', 'r', 's'});
        m.put('8', new char[]{'t', 'u', 'v'});
        m.put('9', new char[]{'w', 'x', 'y', 'z'});
    }
}
```

[39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

![](./fig/prune.png)

```Java
import java.io.*;
import java.util.*;
class Solution {
    private static int[] can;
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(candidates);
        if (target < candidates[0]) return res;

        can = candidates;
        List<Integer> path = new ArrayList<>();
        dfs(res, path, target, 0);

        return res;
    }
    private static void dfs(List<List<Integer>> res, List<Integer> path, int remain, int begin) {
        if (remain == 0) {
            res.add(new ArrayList<>(path));
        }
        // 设置 begin 避免重复
        for (int i = begin; i < can.length; i++) {
            if (remain >= can[i]) {
                path.add(can[i]);
                dfs(res, path, remain - can[i], i);
                path.remove(path.size() - 1);
            }
        }
    }
}
```

[组合总和II](https://leetcode-cn.com/problems/combination-sum-ii/)

```Java
class Solution {
    private static int[] can;
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        can = candidates;

        List<List<Integer>> res = new ArrayList<>();
        List<Integer> path = new ArrayList<>();

        dfs(res, path, target, 0);
        return res;
    }
    private static void dfs(List<List<Integer>> res, List<Integer> path, int remain, int begin) {
        if (remain == 0) {
            res.add(new ArrayList<>(path));
        }

        for (int i = begin; i < can.length; i++) {
            if (i > begin && can[i] == can[i - 1]) continue; // 去重
            if (remain - can[i] >= 0) {
                path.add(can[i]);
                dfs(res, path, remain - can[i], i + 1);
                path.remove(path.size() - 1);
            }
        }
    }
    // private static int findNext(int i) {
    //     for (int j = i + 1; j < can.length; j++) {
    //         if (can[j] != can[j - 1]) return j;
    //     }
    //     return can.length;
    // }
}
```

[组合总和 III](https://leetcode-cn.com/problems/combination-sum-iii/)
```Java
class Solution {
    private static int kk;
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> path = new ArrayList<>();

        kk = k;

        dfs(res, path, n, 0, 1);
        return res;
    }
    // u 记录当前该为哪个位置添加元素了
    private static void dfs(List<List<Integer>> res, List<Integer> path, int remain, int u, int begin) {
        if (u == kk && remain == 0) {
            res.add(new ArrayList<>(path));
        }

        if (u > kk) return;
        
        for (int i = begin; i <= 9; i++) {
            if (remain - i >= 0) {
                path.add(i);
                dfs(res, path, remain - i, u + 1, i + 1);
                path.remove(path.size() - 1);
            }
        }
    }
}
```

## 分割
[131. 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)
```Java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> res = new ArrayList<>();
        List<String> path = new ArrayList<>();
        dfs(res, path, s);
        return res;
    }
    // path 记录了分割点
    private static void dfs(List<List<String>> res, List<String> path, String remain) {
        if (remain.length() == 0) {
            res.add(new ArrayList<>(path));
        }

        for (int i = 1; i <= remain.length(); i++) {
            String now = remain.substring(0, i);
            if (isPalindrome(now)) {
                path.add(now);
                dfs(res, path, remain.substring(i));
                path.remove(path.size() - 1);
            } 
        }
    }
    private static boolean isPalindrome(String s) {
        if (s.length() == 1) return true;
        int i = 0, j = s.length() - 1;
        while (i < j) {
            if (s.charAt(i) != s.charAt(j)) return false;
            i++;
            j--;
        }
        return true;
    }
}
```

[93. 复原 ip 地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

```Java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> res = new ArrayList<>();
        List<Integer> path = new ArrayList<>();

        dfs(res, path, s, 0);
        return res;
    }

    private static void dfs(List<String> res, List<Integer> path, String remain, int u) {
        if (u == 4 && remain.length() == 0) {
            res.add(concat(path));
            return;
        }
        if (u >= 4) return;
        for (int i = 1; i <= Math.min(remain.length(), 3); i++) {
            String now = remain.substring(0, i);
            int nowInt = Integer.parseInt(now);
            if ((now.length() == 1 && nowInt == 0) || (now.charAt(0) != '0' && nowInt > 0 && nowInt <= 255)) {
                path.add(nowInt);
                dfs(res, path, remain.substring(i), u + 1);
                path.remove(path.size() - 1);
            } 
        }
    }
    private static String concat(List<Integer> path) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < path.size(); i++) {
            if (i != path.size() - 1) {
                sb.append(path.get(i) + ".");
            } else {
                sb.append(path.get(i));
            }
        }
        return sb.toString();
    } 
}
```

## 子集
