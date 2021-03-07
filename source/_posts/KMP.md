---
title: KMP算法
date: 2020-02-06 14:25:09
thumbnail: /thumbnails/Algo.jpg
toc: true
categories:
	- 算法 
	- KMP
tags: 
	- 算法
---
## 简介
{% asset_img 1.png This is an main image %}
kmp算法也称为字符串查找算法，可以确认当前字符串中是否包含目标字符串。 当出现字符串不匹配时，可以记录一部分之前已经匹配的文本内容，利用这些信息避免从头再去做匹配。

## 暴力解法
在深入了解KMP之前我们需要了解暴力解法。话不多说先上图！

首先我们从T[0]开始和P比较，直到T[3] 和 P[3] 不匹配。然后从T[1]开始重新和P比较。
{% asset_img 2.png This is an main image %}
***

这时T[1] 不匹配 P[1]，从T[2]开始重新和P比较.
{% asset_img 3.png %}
***
以此类推直到找到第一组存在的目标。这时如果T数组还有多余长度可以继续往下搜寻。但当前T数组已经比较完，此时结束查找。
{% asset_img 4.png %}
***

### 暴力的负面效果
在某些情况下暴力解法效率极低，我们再来看一个列子。

在这种情况下每次不匹配只会发生在P[4], 也就是说每次第5位出错前我们会经历很多次的无意义比较。这样会导致效率低下，而kmp很好得解决了这个问题。
{% asset_img 5.png %}
***
## 前缀表(prefix table)

首先，要了解两个概念："前缀"和"后缀"。 "前缀"指除了最后一个字符以外，一个字符串的全部头部组合；"后缀"指除了第一个字符以外，一个字符串的全部尾部组合。

部分匹配值 就是"前缀"和"后缀"的最长的共有元素的长度。
{% asset_img 7.png %}
***
## 前缀表实际应用
直接跳过了重复的部分
{% asset_img 7.png %}

``` java
public void KMPSearch(String pat, String txt) 
    { 
        int M = pat.length(); 
        int N = txt.length(); 
  
        // create lps[] that will hold the longest 
        // prefix suffix values for pattern 
        int lps[] = new int[M]; 
        int j = 0; // index for pat[] 
  
        // Preprocess the pattern (calculate lps[] 
        // array) 
        computeLPSArray(pat, M, lps); 
  
        int i = 0; // index for txt[] 
        while (i < N) { 
            if (pat.charAt(j) == txt.charAt(i)) { 
                j++; 
                i++; 
            } 
            if (j == M) { 
                System.out.println("Found pattern "
                                   + "at index " + (i - j)); 
                j = lps[j - 1]; 
            } 
  
            // mismatch after j matches 
            else if (i < N && pat.charAt(j) != txt.charAt(i)) { 
                // Do not match lps[0..lps[j-1]] characters, 
                // they will match anyway 
                if (j != 0) 
                    j = lps[j - 1]; 
                else
                    i = i + 1; 
            } 
        } 
    } 
  
public void computeLPSArray(String pat, int M, int lps[]) 
    { 
        // length of the previous longest prefix suffix 
        int len = 0; 
        int i = 1; 
        lps[0] = 0; // lps[0] is always 0 
  
        // the loop calculates lps[i] for i = 1 to M-1 
        while (i < M) { 
            if (pat.charAt(i) == pat.charAt(len)) { 
                len++; 
                lps[i] = len; 
                i++; 
            } 
            else // (pat[i] != pat[len]) 
            { 
                // This is tricky. Consider the example. 
                // AAACAAAA and i = 7. The idea is similar 
                // to search step. 
                if (len != 0) { 
                    len = lps[len - 1]; 
  
                    // Also, note that we do not increment 
                    // i here 
                } 
                else // if (len == 0) 
                { 
                    lps[i] = len; 
                    i++; 
                } 
            } 
        } 
    } 
```
### 复杂度分析
平均复杂性： O(log n)
未完