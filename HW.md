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
