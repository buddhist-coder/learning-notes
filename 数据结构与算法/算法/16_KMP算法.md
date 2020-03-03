# KMP算法

- ## 应用场景-字符串匹配问题

  有一个字符串str1 = "ABCDABCD ABCDE"，和一个子串str2 = "ABCDE"，现在要判断str1是否包含str2，如果存在就返回第一次出现的位置，如果没有，则返回-1。

- ## 暴力匹配算法

  如果用暴力匹配的思路，并假设现在`str1`匹配到`i`位置，子串`stri2`匹配到`j`位置，则有：

  1. 如果当期字符匹配成功(即`str1[i]=str2[j]`)，则`i++,j++`，继续匹配下一个字符；
  2. 如果匹配失败（即`str1[i]!=str2[j]`），令`i=i-(j-1),j=0`。相当于每次匹配失败时，`i`回溯，`j`被置为0；
  3. 用暴力匹配方法解决的话会有大量的回溯，每次只移动一位，若是不匹配，移动到下一位接着判断，浪费了大量时间。

  ```java
  package com.xie.algorithm;
  
  public class ViolenceMatch {
      public static void main(String[] args) {
          String str1 = "ABCDABCD ABCDE";
          String str2 = "ABCDE";
          int index = violenceMatch(str1, str2);
          System.out.println("index = " + index);
          /**
           * index = 9
           */
      }
  
      //暴力匹配算法实现
      public static int violenceMatch(String str1, String str2) {
          char[] s1 = str1.toCharArray();
          char[] s2 = str2.toCharArray();
          int s1Len = s1.length;
          int s2Len = s2.length;
  
          int i = 0, j = 0;
  
          while (i < s1Len && j < s2Len) {
              if (s1[i] == s2[j]) {
                  i++;
                  j++;
              } else {
                  i = i - (j - 1);
                  j = 0;
              }
          }
          //找到了
          if (j == str2.length()) {
              return i - j;
          } else {
              return -1;
          }
      }
  }
  
  ```

- ## KMP算法介绍

  1. KMP算法是一个解决模式串在文本串中是否存在过，如果出现过，返回最早出现的位置。
  2. Knuth-Morris-Pratt字符串查找算法，由此三人于1977年联合发表，故取三人姓氏命名此算法。
  3. KMP算法就是利用之前判断过信息，通过一个next数组，保存模式串中前后最长公共子序列的长度，每次回溯时，通过next数组找到前面匹配过的位置，省去了大量计算时间。

- ## 代码案例

  ```java
  package com.xie.algorithm;
  
  public class KMPAlgorithm {
      public static void main(String[] args) {
          String str1 = "BBC ABCDAB ABCDABCDABDE";
          String str2 = "ABCDABD";
          int index = kmpSerach(str1, str2, kmpNext(str2));
          System.out.println("index = " + index);
          /**
           * index = 15
           */
      }
  
      /**
       * KMP算法
       *
       * @param str1 源字符串
       * @param str2 子串
       * @param next 子串对应的部分匹配表
       * @return 如果是-1就没有匹配到，否则就返回第一个匹配位置
       */
      public static int kmpSerach(String str1, String str2, int[] next) {
          for (int i = 0, j = 0; i < str1.length(); i++) {
              //str1.charAt(i)!=str2.charAt(j), 去调整j的大小
              //KMP算法的核心点
              while (j > 0 && str1.charAt(i) != str2.charAt(j)) {
                  j = next[j - 1];
              }
              if (str1.charAt(i) == str2.charAt(j)) {
                  j++;
              }
              if (j == str2.length()) {
                  return i - j + 1;
              }
          }
          return -1;
      }
  
      //获取到子串的部分匹配值
      public static int[] kmpNext(String dest) {
          //创建一个next数组保存部分匹配值
          int[] next = new int[dest.length()];
          next[0] = 0;//如果字符串是长度为1，部分匹配值就是0
  
          for (int i = 1, j = 0; i < dest.length(); i++) {
              //当dest.charAt(i) != dest.charAt(j)时，我们需要从next[j-1]获取新的j
              //这是KMP算法的核心
              while (j > 0 && dest.charAt(i) != dest.charAt(j)) {
                  j = next[j - 1];
              }
              //当dest.charAt(i) == dest.charAt(j)时，部分匹配值就加1
              if (dest.charAt(i) == dest.charAt(j)) {
                  j++;
              }
              next[i] = j;
          }
          return next;
      }
  }
  ```
  