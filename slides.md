---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## CPC 2022/05/11 二分搜
# persist drawings in exports and build
drawings:
  persist: false
# allow dowoload
download: true
---

# 0511 二分搜

上課投影片

---

# 對於影片有任何問題嗎?

---

# UVa 10474

- 給定一組數字，先將它們排序後，詢問數字 $x$，在第幾個位置。

<v-clicks>

- 二分搜基本練習

</v-clicks>

---

# UVa 11057

- 給定一組數字，找出兩個數字，數字和為 $M$，如果有多組解，輸出差距最小的一組。

<v-clicks>

- 暴力
    - 排序
    - 雙指標
- 二分搜
    - 排序
    - 對於每個數字 $x$，尋找是否有 $M-x$

</v-clicks>

---

# UVa 11413

- 給定 $N$ 個數字，要把它切割成 $M$ 組數字，設 $M$ 組數字和中最大值為 $X$，請求出最小的 $X$。

<v-clicks>

- 提一下語意問題
    - 最小化最大值 $X$ 是最小化問題，$X$ 可把它視為每種組合的分數
- 對 $X$ 二分搜，判斷方法：就頭開始取數字，每一組盡可能累積到 $X$，取完 $M$ 組後，看是否還有數字為取到。

</v-clicks>

---

# UVa 12190

- 給定電力價格表，你和你的鄰居合併計算電費為 $A$ 元，分開計算為 $B$ 元，請問如果分開計算的情況下，你的電費為何(你的電費比較便宜)?

<v-clicks>

- $F(X)$ 計算使用 $X$ 度電的電費
- 先二分搜兩個人總共使用的電量 $total$
- 再二分搜你使用的電量 $x$
    - 如果 $F(total-x)-F(x)==B$ 輸出 $x$
    - 如果 $F(total-x)-F(x)<B$ 或是 $total-x<x$，$x=R-1$
    - 否則 $x=L+1$ 

</v-clicks>

---

# UVa 10232

- 一開始位置在 $0$，依序抵達座標 $r_1,r_2,...,r_n$，找出最小的 $k$ 滿足：
- 每一次最多只能前進 $k$ 步，如果這次前進 $k$ 步，之後最多只能前進 $k-1$ 步，以此類推

<v-clicks>

- 貪心
    - 逆向求解
    - 一開始設答案 $ans=0$，每次將 $ans$ 根據兩相鄰點之間的差距 $D$ 更新答案
    - $ans=D\to ans+=1$
    - $ans=max(ans,D)$
- 二分搜
    - 枚舉答案 $k$
    - 根據題目規則測試 $k$
    - 兩相鄰點之間的差距 $D$
    - 如果 $D==K\to K-=1$
    - 如果 $D>K\to$ fail

</v-clicks>

---

# 三分搜

- 用途：二次曲線找極值

---

# 三分搜 - 觀察

<img src="/trinarySearch.png" class="m-4 h-100 rounded shadow" />

---

# 三分搜 - 觀察

- 對於凸函數（例如 $y=F(x)=x^2$ )，我們想要找尋其極值，意謂其左右兩側皆各自遞增/遞減，我們可以利用三分搜來解決（二分搜只能解決全體單調性，不能解決有兩邊的）。
考慮三分後從左到右四個採樣點的關係
    -  $S(a) < S(b) < S(c) < S(d)$ ，此時最小值一定不在最右邊
    -  $S(a) > S(b) < S(c) < S(d)$ ，此時最小值一定不在最右邊
    -  $S(a) > S(b) > S(c) < S(d)$ ，此時最小值一定不在最左邊
    -  $S(a) > S(b) > S(c) > S(d)$ ，此時最小值一定不在最左邊
- 這段描敘還可以再簡化：
    -  $S(b) < S(c)$ ，此時最小值一定不在最右邊
    -  $S(b) > S(c)$ ，此時最小值一定不在最左邊

---

# 三分搜 - 程式碼

```cpp
double trinary_search(double L, double R)
{
    for (int i = 0; i < 300; ++i)
    {
        double mL = (L + L + R) / 3, mR = (L + R + R) / 3;
        if (f(mR) > f(mL))
            R = mR;
        else
            L = mL;
    }
    return L;
}
```

---

# UVa 01476

- 給定 $N$ 個開口向上的二次曲線 $S_i$，令 $F(x)=max{S_i(x)}$，求 $F(x)$ 的最小值。
    - 多個二次函數取最大值的結果還是一個二次函數，利用三分搜搜尋極值。