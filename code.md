// Problem: 2976. Minimum Cost to Convert String I

class Solution {
public:
    long long minimumCost(string source, string target, vector<char>& original, vector<char>& changed, vector<int>& cost) {
        // Init 26x26 grid with large INF
        long long INF = 1e15;
        vector<vector<long long>> grid(26, vector<long long>(26, INF));
        // Self-conversion is free
        for (int i = 0; i < 26; i++) {
            grid[i][i] = 0;
        }
        // Fill direct rules (save cheapest)
        for (int i = 0; i < original.size(); i++) {
            int u = original[i] - 'a';
            int v = changed[i] - 'a';
            grid[u][v] = min(grid[u][v], (long long)cost[i]);
        }
        // Floyd-Warshall: k MUST be outermost
        for (int k = 0; k < 26; k++) {
            for (int i = 0; i < 26; i++) {
                for (int j = 0; j < 26; j++) {
                    grid[i][j] = min(grid[i][j], grid[i][k] + grid[k][j]);
                }
            }
        }
        // Total cost for string conversion
        long long totalCost = 0;
        for (int i = 0; i < source.length(); i++) {
            int u = source[i] - 'a';
            int v = target[i] - 'a'; 
            if (grid[u][v] >= INF) return -1; // Unreachable
            totalCost += grid[u][v];
        }
        return totalCost;
    }
};
