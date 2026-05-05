<style>
 body {
  font-family: "楷体"
}
</style>

<h1><center>区间变换</center></h1>

### 选最少的点覆盖所有区间

```cpp
int calc(vector<pii> a) {
    sort(a.begin(), a.end(), [&](pii x, pii y) -> bool {
        return x.second == y.second ? x.first < y.first : x.second < y.second;
    });
    int res = 0, r = -inf;
    for (auto u : a) {
        res += (u.first > r);
        r = (u.first > r) ? u.second : r;
    }
    return res;
}
```

### 选最多的区间互不相交

同【选最少的点覆盖所有区间】。

### 区间分成组内区间无交的最少组数

```cpp
int calc(vector<pii> a) {
    sort(a.begin(), a.end());
    priority_queue<int, vector<int>, greater<int>> q;
    for (auto [l, r] : a) {
        if (q.size() && q.top() < l)
            q.pop();
        q.push(r);
    }
    return q.size();
}
```

### 选最少的区间覆盖整段区间

```cpp
int calc(int s, int t, vector<pii> a) {
    sort(a.begin(), a.end());
    int res = 1, r = -inf;
    for (auto u : a) {
        if (u.first <= s) {
            r = max(r, u.second);
        } else {
            if (u.first > r) // 注意边界取等还是 +1
                return -1;
            res++;
            s = r;
            r = max(r, u.second);
        }
        if (r >= t)
            return res;
    }
    return -1;
}
```

### 区间求并

```cpp
int calc(vector<pii> a) { 
    sort(a.begin(), a.end());
    int res = 0, l = -inf + 1, r = -inf;
    for (auto u : a) {
        if (u.first <= r) {
            r = max(r, u.second);
        } else {
            res += r - l + 1;
            tie(l, r) = u;
        }
    }
    res += r - l + 1;
    return res;
}
```