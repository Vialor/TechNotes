```C++
std::memset(dp, 1, sizeof(dp));
```

```C++
# Leetcode 261 DFS
class Solution {
public:
    bool validTree(int n, vector<vector<int>>& edges) {
        if (edges.size() != n-1) return false;
        vector<vector<int>> graph;
        graph.resize(n);
        set<int> visited;

        for (auto& edge:edges) {
            graph[edge[0]].push_back(edge[1]);
            graph[edge[1]].push_back(edge[0]);
        }

        std::function<void(int)> DFS = [&](int i) {
            visited.insert(i);
            for (auto& neighbor:graph[i]) {
                if (!visited.contains(neighbor)) {
                    DFS(neighbor);
                }
            }
        };
        DFS(0);

        return visited.size() == n;
    }
};

# 127 BFS
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_map<string, int> hash;
        for(string s:wordList){
            hash[s]=0;
        }
        hash[beginWord]=1;
        
        queue<string> q;
        q.push(beginWord);
        while(!q.empty()){
            string now = q.front();
            q.pop();
            for(int i=0;i<now.size();i++){
                string check=now;
                for(int j=0;j<26;j++){
                    check[i]='a'+j;
                    if(hash.count(check)){
                        if(check==endWord) return hash[now]+1;
                        if(hash[check]==0){
                            hash[check]=hash[now]+1;
                            q.push(check);
                        }
                    }
                    
                }
            }
        }
        return 0;
    }
};
```