[HJ51 输出单向链表中倒数第k个结点](https://www.nowcoder.com/practice/54404a78aec1435a81150f15f899417d?tpId=37&tqId=21274&rp=1&ru=/exam/oj/ta&qru=/exam/oj/ta&sourceUrl=%2Fexam%2Foj%2Fta%3FtpId%3D37%26type%3D37&difficulty=undefined&judgeStatus=undefined&tags=&title=)
```Java
import java.io.*;
import java.util.*;
public class Main{
    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedInputStream(System.in));
        while (in.hasNext()) {
            int n = in.nextInt();
            ListNode head = new ListNode(-1);
            ListNode cur = head;
            for (int i = 0; i < n; i++) {
                cur.next = new ListNode(in.nextInt());
                cur = cur.next;
            }
            int k = in.nextInt();
            System.out.println(findKthNode(head, k));
        }
        
    }
    private static int findKthNode(ListNode head, int k) {
        if (k == 0) return 0; // 题目没说
        ListNode fast = head, slow = head;
        while (k-- > 0) {
            fast = fast.next;
        }
        while (fast != null) {
            fast = fast.next; 
            slow = slow.next;
        }
        return slow.val;
    }
}


class ListNode{
    int val;
    ListNode next;
    ListNode(int val) {
        this.val = val;
    }
}
```
- 链表。

[HJ16 购物单](https://www.nowcoder.com/practice/f9c6f980eeec43ef85be20755ddbeaf4?tpId=37&rp=1&ru=%2Fexam%2Foj%2Fta&qru=%2Fexam%2Foj%2Fta&sourceUrl=%2Fexam%2Foj%2Fta%3FtpId%3D37%26type%3D37%26page%3D1%26difficulty%3D3&difficulty=3&judgeStatus=&tags=&title=&gioEnter=menu)
```Java
import java.io.*;
import java.util.*;
public class Main{
    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedInputStream(System.in));
        int m = in.nextInt(), n = in.nextInt();
        int[][] v = new int[n + 1][3], w = new int[n + 1][3];
        for (int i = 1; i <= n; i++) {
            int val = in.nextInt(), weight = in.nextInt() * val, q = in.nextInt();
            if (q == 0) {
                v[i][0] = val;
                w[i][0] = weight;
            } else {
                if (v[q][1] == 0) {
                    v[q][1] = val;
                    w[q][1] = weight;
                } else {
                    v[q][2] = val;
                    w[q][2] = weight;
                }
            }
        }
        
        int[] f = new int[m + 1];
        for (int i = 1; i <= n; i++) {
            for (int j = m; j >= v[i][0]; j--) {
                int a = v[i][0], b = v[i][1], c = v[i][2];
                int wa = w[i][0], wb = w[i][1], wc = w[i][2];
                
                f[j] = Math.max(f[j], f[j - a] + wa);
                if (j >= (a + b)) f[j] = Math.max(f[j],f[j - a - b] + wa + wb);
                if (j >= (a + c)) f[j] = Math.max(f[j], f[j - a - c] + wa + wc);
                if (j >= (a + b + c)) f[j] = Math.max(f[j], f[j - a - b - c] + wa + wb + wc);
            }
        }
        System.out.println(f[m]);
    }
}
```
- 将原问题转化成 0-1 背包问题，对每一个主物品，与它的附属物品只有四种搭配(a, ab, ac, abc)
- 该问题里面的价值是重要性和体积的乘积，可以在初始化的过程中就完成。

[HJ17 坐标移动](https://www.nowcoder.com/practice/119bcca3befb405fbe58abe9c532eb29?tpId=37&tqId=21240&rp=1&ru=/exam/oj/ta&qru=/exam/oj/ta&sourceUrl=%2Fexam%2Foj%2Fta%3FtpId%3D37%26type%3D37%26page%3D1%26difficulty%3D3&difficulty=3&judgeStatus=undefined&tags=&title=)
```Java
import java.io.*;
import java.util.*;
public class Main{
    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedInputStream(System.in));
        while (in.hasNext()) {
            String[] line = in.next().split(";");
            int x = 0, y = 0;
            for (String str: line) {
                if (str.length() == 0) continue;
                char c = str.charAt(0);
                try {
                    int val = Integer.parseInt(str.substring(1, str.length()));
                    if (c == 'A') {
                        x -= val;
                    } else if (c == 'D') {
                        x += val;
                    } else if (c == 'W') {
                        y += val;
                    } else if (c == 'S') {
                        y -= val;
                    }
                } catch(Exception e) {
                    continue;
                }
            }
            System.out.println(x + "," + y);
        }
    }
}
```

[HJ24 合唱队](https://www.nowcoder.com/practice/6d9d69e3898f45169a441632b325c7b4?tpId=37&tqId=21247&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=%2Fexam%2Foj%3Ftab%3D%25E5%2590%258D%25E4%25BC%2581%25E7%25AC%2594%25E8%25AF%2595%25E7%259C%259F%25E9%25A2%2598%26topicId%3D37%26page%3D1%26difficulty%3D3&difficulty=3&judgeStatus=undefined&tags=&title=)
```Java
import java.io.*;
import java.util.*;
public class Main{
    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedInputStream(System.in));
        while (in.hasNext()) {
            int N = in.nextInt();
            int[] h = new int[N + 1];
            for (int i = 1; i <= N; i++) {
                h[i] = in.nextInt();
            }
            int ans = 0x3f3f3f3f;
            int[] a = LIS(h); // a[i] 表示以 i 结尾的最长递增子序列的长度
            int[] b = LDS(h); // b[i] 表示以 i 开头的最长递减子序列的长度
            
            for (int i = 1; i < h.length; i++) {
                ans = Math.min(ans, (N - a[i] - b[i] + 1));
            }
            System.out.println(ans);
        }
    }
    // 最长上升子序列的长度
    private static int[] LIS(int[] a) {
        // f[i] 表示以 a[i] 结尾的最长上升子序列的长度
        int[] f = new int[a.length];
        int n = a.length - 1;
        for (int i = 1; i <= n; i++) {
            f[i] = 1;
            for (int j = 1; j < i; j++) {
                if (a[j] < a[i]) f[i] = Math.max(f[i], f[j] + 1);
            }
        }
        return f;
    }
    // 最长下降子序列的长度
    private static int[] LDS(int[] a) {
        // f[i] 表示以 a[i] 作为开头的最长下降子序列的长度
        int[] f = new int[a.length];
        int n = a.length - 1;
        for (int i = n; i >= 1; i--) {
            f[i] = 1; // 只有自己
            for (int j = n; j > i; j--) {
                if (a[j] < a[i]) f[i] = Math.max(f[i], f[j] + 1);
            }
        }
        return f;
    }
}
```
- 借用了最长递增子序列的知识。

[HJ26 字符串排序](https://www.nowcoder.com/practice/5190a1db6f4f4ddb92fd9c365c944584?tpId=37&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3D%25E5%2590%258D%25E4%25BC%2581%25E7%25AC%2594%25E8%25AF%2595%25E7%259C%259F%25E9%25A2%2598%26topicId%3D37%26page%3D1%26difficulty%3D3&difficulty=3&judgeStatus=&tags=&title=&gioEnter=menu)
```Java
import java.io.*;
import java.util.*;
public class Main{
    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedInputStream(System.in));
        while (in.hasNext()) {
            String now = in.nextLine();
            System.out.println(convert(now));
        }
    }
    private static String convert(String now) {
        StringBuilder sb = new StringBuilder();
        for (int j = 0; j < 26; j++) {
            for (char c: now.toCharArray()) {
                if (c - 'a' == j || c - 'A' == j) {
                    sb.append(c);
                }
            }
        }
        StringBuilder res = new StringBuilder();
        int i, j;
        for (i = 0, j = 0; i < now.length() || j < sb.length();) {
            char c = now.charAt(i);
            if (!isAlpha(c)) {
                res.append(c);
                i++;
            } else {
                res.append(sb.charAt(j));
                j++;
                i++;
            }
        }
        return res.toString();
    }
    private static boolean isAlpha(char c) {
        return (c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z');
    }
}
```

[HJ27 查找兄弟单词](https://www.nowcoder.com/practice/03ba8aeeef73400ca7a37a5f3370fe68?tpId=37&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3D%25E5%2590%258D%25E4%25BC%2581%25E7%25AC%2594%25E8%25AF%2595%25E7%259C%259F%25E9%25A2%2598%26topicId%3D37%26page%3D1%26difficulty%3D3&difficulty=3&judgeStatus=&tags=&title=&gioEnter=menu)
```Java
import java.io.*;
import java.util.*;
public class Main{
    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedInputStream(System.in));
        while (in.hasNext()) {
            int n = in.nextInt();
            String[] candidates = new String[n];
            for (int i = 0; i < n; i++) candidates[i] = in.next();
            String x = in.next();
            int k = in.nextInt();
            
            char[] xChars = x.toCharArray();
            Arrays.sort(xChars);
            String xNew = new String(xChars);
            
            
            List<String> bro = new ArrayList<>();
            for (String can: candidates) {
                if (can.length() > x.length()) continue;
                char[] canChars = can.toCharArray();
                Arrays.sort(canChars);
                String canNew = new String(canChars);
                if (!can.equals(x) && canNew.equals(xNew)) {
                    bro.add(can);
                }   
            }
            Collections.sort(bro);
            try{
                System.out.println(bro.size());
                System.out.println(bro.get(k - 1));
            } catch(Exception e) {
            }
        }
    }
}
```

[HJ29 字符串加解密](https://www.nowcoder.com/practice/2aa32b378a024755a3f251e75cbf233a?tpId=37&tqId=21252&rp=1&ru=/exam/oj/ta&qru=/exam/oj/ta&sourceUrl=%2Fexam%2Foj%2Fta%3FtpId%3D37%26type%3D37%26page%3D1%26difficulty%3D3&difficulty=3&judgeStatus=undefined&tags=&title=)
```Java
import java.io.*;
import java.util.*;
public class Main{
    public static String L1 = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    public static String L2 = "BCDEFGHIJKLMNOPQRSTUVWXYZAbcdefghijklmnopqrstuvwxyza1234567890";
    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedInputStream(System.in));
        while (in.hasNext()) {
            String toEncode = in.next();
            String toDecode = in.next();
            System.out.println(encode(toEncode));
            System.out.println(decode(toDecode));
        }
    }
    private static String encode(String s) {
        StringBuilder sb = new StringBuilder();
        for (char c: s.toCharArray()) {
            sb.append(L2.charAt(L1.indexOf(c)));
        }
        return sb.toString();
    }
    private static String decode(String s) {
        StringBuilder sb = new StringBuilder();
        for (char c: s.toCharArray()) {
            sb.append(L1.charAt(L2.indexOf(c)));
        }
        return sb.toString();
    }
}
```

[HJ32 密码截取]()
```Java
import java.io.*;
import java.util.*;
public class Main{
    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedInputStream(System.in));
        while(in.hasNext()) {
            String s = in.next();
            System.out.println(maxPalindrome(s));
        }
    }
    private static int maxPalindrome(String s) {
        int res = 1;
        boolean[][] f = new boolean[s.length()][s.length()];
        for (int i = s.length() - 1; i >= 0; i--) {
            for (int j = i; j < s.length(); j++) {
                if (i == j) f[i][j] = true;
                else if (j - i == 1 && s.charAt(i) == s.charAt(j)) {
                    f[i][j] = true;
                    res = Math.max(res, 2);
                } else { // j - i > 1
                    if (s.charAt(i) == s.charAt(j) && f[i + 1][j - 1]) {
                        f[i][j] = true;
                        res = Math.max(res, j - i + 1);
                    }
                }
            }
        }
        return res;
    }
}
```
- 本题目原型 [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)。
- 难点: 
    - f[i][j] 定义为 **s[i ~ j] 是否是回文串**，如此**也不需要多开辟数组**。
        - i == j : f[i][j] = true;
        - j - i == 1: 检查 s[i] 是否等于 s[j]
        - j - i > 1: 检查 s[i] 与 s[j] 以及 f[i + 1][j - 1]
    - 由于求 f[i][j] 的时候需要 f[i + 1][j - 1], 所以**从下到上，从左到右**遍历。

[HJ41 称砝码](https://www.nowcoder.com/practice/f9a4c19050fc477e9e27eb75f3bfd49c?tpId=37&tqId=21264&rp=1&ru=/exam/oj/ta&qru=/exam/oj/ta&sourceUrl=%2Fexam%2Foj%2Fta%3FtpId%3D37%26type%3D37%26page%3D1%26difficulty%3D3&difficulty=3&judgeStatus=undefined&tags=&title=)
```Java
import java.io.*;
import java.util.*;
public class Main{
    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedInputStream(System.in));
        while (in.hasNext()) {
            int n = in.nextInt();
            int[] m = new int[n];
            int[] x = new int[n];
            for (int i = 0; i < n; i++) {
                m[i] = in.nextInt();
            } 
            for (int i = 0; i < n; i++) {
                x[i] = in.nextInt();
            }
            System.out.println(compute(m, x));
        }
    }
    private static int compute(int[] m, int[] x) {
        Set<Integer> set = new HashSet<>();
        set.add(0);
        for (int i = 0; i < m.length; i++) {
            for (int j = 1; j <= x[i]; j++) {
                List<Integer> ll = new ArrayList<>();
                ll.addAll(set);
                for (int l : ll) {
                    set.add(l + m[i]);
                }
            }
        }
        return set.size();
    }
}
```
- 用**哈希表**，每次往里面加砝码就可以了。

[HJ107 求解立方根](https://www.nowcoder.com/practice/caf35ae421194a1090c22fe223357dca?tpId=37&tqId=21330&rp=1&ru=/exam/oj/ta&qru=/exam/oj/ta&sourceUrl=%2Fexam%2Foj%2Fta%3FtpId%3D37%26type%3D37%26page%3D1%26difficulty%3D3&difficulty=3&judgeStatus=undefined&tags=&title=)
```Java
import java.io.*;
import java.util.*;
public class Main{
    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedInputStream(System.in));
        while (in.hasNext()) {
            double val = in.nextDouble();
            double left = -25.0, right = 25.0;
            while (right - left > 1e-3) {
                double mid = (right + left) / 2;
                if (mid * mid * mid > val) {
                    right = mid;
                } else {
                    left = mid;
                }
            }
            System.out.println(String.format("%.1f", left));
        }
    }
}
```
- 经典浮点数二分。
- 注意用 String.format 可以实现四舍五入保留小数的效果。

[HJ38 求小球落地5次后所经历的路程和第5次反弹的高度](https://www.nowcoder.com/practice/2f6f9339d151410583459847ecc98446?tpId=37&tqId=21261&rp=1&ru=/exam/oj/ta&qru=/exam/oj/ta&sourceUrl=%2Fexam%2Foj%2Fta%3FtpId%3D37%26type%3D37%26page%3D1%26difficulty%3D3&difficulty=3&judgeStatus=undefined&tags=&title=)
```Java
import java.io.*;
import java.util.*;
public class Main{
    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedInputStream(System.in));
        double pre = 23.0 / 8.0;
        while (in.hasNext()) {
            double n = in.nextDouble();
            System.out.println(String.format("%.6f", n * pre));
            System.out.println(String.format("%.6f", n / 32));
        }
    }
}
```
- 可以直接利用题目中的结论，不需要再自己算了。

[HJ75 公共子串计算](oder.com/practice/98dc82c094e043ccb7e0570e5342dd1b?tpId=37&tqId=21298&rp=1&ru=/exam/oj/ta&qru=/exam/oj/ta&sourceUrl=%2Fexam%2Foj%2Fta%3FtpId%3D37%26type%3D37%26page%3D1%26difficulty%3D3&difficulty=3&judgeStatus=undefined&tags=&title=)
```Java
import java.io.*;
import java.util.*;
public class Main{
    public static void main(String[] args) {
        Scanner in = new Scanner(new BufferedInputStream(System.in));
        while (in.hasNext()) {
            String s = in.next();
            String t = in.next();
            System.out.println(LCS(s, t));
        }
    }
    private static int LCS(String s, String t) {
        // f[i][j] 表示s中以第 i 字符结尾，和 t中以第 j 字符结尾的最长公共子串的长度
        int[][] f = new int[s.length() + 1][t.length() + 1];
        int ans = 0;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 1; j <= t.length(); j++) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) {
                    f[i][j] = f[i - 1][j - 1] + 1;     
                    ans = Math.max(ans, f[i][j]);
                }
            }
        }
        return ans;
    }
}
```
- 公共子串，需要确保连续，所以必须要以**第 i，第 j**结尾。



