### 用`map`和`pair`一起使用

```c++
    unordered_map<int, pair<string, int>> tableid;
    unordered_map<string, pair<double, int>> result;

void checkIn(int id, string stationName, int t) {
        tableid[id] = { stationName ,t };
    }

void checkOut(int id, string stationName, int t) {
        string name = tableid[id].first + stationName;
        t -= tableid[id].second;
        result[name].first += (double)t;
        result[name].second++;
    }

```

```c++
 using Start = pair <string, int>;
 using StartEnd = pair <string, string>;
 using SumAndAmount = pair <int, int>;


unordered_map <int, Start> startInfo;

```

### map和struct

```c++
struct Node {
        // 哈希表存储关注人的 Id
        unordered_set<int> followee;
        // 用链表存储 tweetId
        list<int> tweet;
    };


unordered_map<int, Node> user;

    // 初始化
    void init(int userId) {
        user[userId].followee.clear();
        user[userId].tweet.clear();
    }
```

