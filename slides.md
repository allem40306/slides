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
  ## CPC 2022/03/25 Greedy
# persist drawings in exports and build
drawings:
  persist: false
# allow dowoload
download: true
---

# 0311 貪心

上課投影片

---

# 對於影片有任何問題嗎?

---

# 題解內容

- 例題大多都已經講過講法，這裡會多著墨在時間複雜度的觀點
    - 分析枚舉和貪心的時間複雜度

---

# 貪心的時間複雜度

- 大多為 $O(N\log N)$，如果資料不需要排序，複雜度則降為 $O(N)$
- 貪心的題目常出現要求一種排序或集合，使用枚舉的時間複雜度為 $O(N!)$ 和 $O(2^N)$，如果 $N$ 過大，使用枚舉就會 TLE

---

# UVa 12405

- 有一個 $N\times 1$ 的稻田，有些稻田現在有種植作物，為了避免被動物破壞，需要放置稻草人，稻草人可以保護該塊稻田和左右兩塊稻田，求最少需要多少稻草人

<v-clicks>

- 暴力
    - 枚舉所有有可能的擺放方式，檢查是否有作物沒被保護
    - $O(2^N)$
- 貪心
    - 從左到右掃描稻田，如果第 $i$ 塊稻田有作物，就把稻草人放到第 $i+1$ 塊稻田，這樣能保護 $[i,i+2]$ 塊稻田，接著從第 $i+3$ 稻田掃描
    - $O(N)$

</v-clicks>

---

# UVa 11729

- 給定每個士兵接收指令和執行指令的時間，每個士兵需要聽完指令才能執行，求從第一個士兵聽取指令，到所有士兵完成指令，所需花得最少時間

<v-clicks>

- 枚舉
    - 枚舉聽取指令的順序
    - $O(N!)$
- 貪心
    - 執行時間長的先聽取指令
    - $O(N\log N)$

</v-clicks>

---

# UVa 12694

- 給定多個租借教室的需求，每個需求包含開始和結束時間，教室同時間只能接受一個需求，問這間教室最多可以接受幾個需求

<v-clicks>

- 最多不重疊區段​
- 暴力
    - 枚舉所有可能的接受集合，檢查是否有任兩個需求的是否時間重疊
    - $O(2^N)$
- 貪心
    - 將所有時間以右界排序，每取到一個沒有重疊的線段，就將答案 $+1$​
    - $O(N)$

</v-clicks>

---

# 變形
## UVa 10382
- 給定一張高 $L$ 寬 $W$ 的草坪，有多個灑水器在草坪正中間，假設灑水器的位置和半徑為 $(\frac{L}{2},y_i)$ 和 $r_i$，問至少要擠個灑水器，才能讓整個草坪都灑滿水。

<v-clicks>

- 這是先算出每個灑水器與矩形上下兩邊的交界 $(L_i,R_i)$，題目就轉成最多不重疊區段問題。

</v-clicks>

<img src="/uva10382.png" class="m-4 h-40 rounded shadow" />

---

# UVa 10954

- 給定一些數字，每次選取兩個數字加總合併，合併成本為兩數的和，求最小維護成本

<v-clicks>

- 暴力：枚舉合併順序
    - 複雜度 $O(N!)$
    - 實作複雜
- 貪心：每次選最小兩個數字，合併完之後放回去
    - 需要使用 priority_queue 維護最小值

</v-clicks>

---

# ZeroJudge b966​

- 給定一維座標上一些線段，求這些線段所覆蓋的長度，重疊的部分只能算一次

<v-clicks>

- 區間覆蓋​問題
- 暴力：枚舉所有座標
    - $O(NR)$
- 貪心
    - 將所有線段以左界由小到大排序，左界相同右界由大到小排序​
    - 紀錄目前最多可以覆蓋到的右界​
    - 只要可以向右延伸就加入答案，並扣掉重疊的部分​
    - $O(N)$

</v-clicks>

---

# 其他例題

- 換硬幣問題
- 刪數字問題
- 最小化最大延遲問題
- Moore-Hodgson's Algorithm / 烏龜塔定理

---

# 換硬幣問題

- 給定硬幣幣值 $x_0=1,x_1,x_2,x_3,...,x_M,x_i<x_j$ and $x_j|x_i,\forall i<j$
- 問最少需要幾個硬幣才能湊出 $C$ 元

<v-clicks>

- 從大幣值的硬幣開始使用，如果用剩餘的再用更小的餘額，以此類推
- 如果幣值之間沒有整除，就不能用 Greedy

</v-clicks>

---

## 刪數字問題

- 給定一個數字 $N(\le 10^{100})$，需要刪除 $K$ 個數字，請求出刪除 $K$ 個數字後最小的數字為何

<v-clicks>

- 想法 1
    - 將最大的數字移除
    - 1879 -> 187
- 想法 2
    - 將 $\{ N_i |  N_i<N_{i+1} \}$ 當中最左邊的移除
- 記得移除多餘的 $0$
    - 10002 -> 2

</v-clicks>

---

# 最小化最大延遲問題

- 給定 $N$ 個工作，每個工作的需要處理時長為 $T_i$，期限是 $D_i$，第 $i$ 個工作延遲的時間為 $L_i=max(0,T_i-D_i)$，求一種工作排序使 $max{L_i}$ 最小。

<v-clicks>

- 按照到期時間從早到晚排序

</v-clicks>

<!-- https://blog.csdn.net/qq_37657182/article/details/102853659 -->

---

# Moore-Hodgson's Algorithm / 烏龜塔定理

- 給定 $N$ 個工作 $Job$，每個工作的需要處理時長為 $T_i$，期限是 $D_i$，求一種工作排序使得逾期工作數量最小。

<v-clicks>

- 將 $Job$ 依照到期時間從早到晚排序
- 找出第一個逾期的工作 index $k$，如果找不到就結束
- 將 $Job[1:k]$ 未被標記移除的工作當中，選出最耗時個工作 $Job[x]$
    - priority_queue
- 將 $Job[x]$ 標記移除
- 重複執行步驟 2
- 所有被標記的工作，移到 Job 的最後面，即為逾期工作數量最小的排序

</v-clicks>

<!-- https://nicknick0630.github.io/2019/03/07/Moore-Hodgson-s-Algorithm-Minimizing-the-number-of-late-jobs/ 和 https://nicknick0630.github.io/2019/03/07/UVa-10154-Weights-and-Measures/ -->