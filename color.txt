#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

const int INF = 9999;

class Graph
{
    int V;
    vector<vector<int>> adj;

public:
    Graph(int V) : V(V)
    {
        adj.resize(V, vector<int>(V, 0));
    }

    void addEdge(int u, int v)
    {
        adj[u][v] = 1;
        adj[v][u] = 1;
    }

    bool isSafe(int v, vector<int> &color, int c)
    {
        for (int i = 0; i < V; ++i)
            if (adj[v][i] && c == color[i])
                return false;
        return true;
    }

    bool graphColoringUtil(vector<int> &color, int m, int v)
    {
        if (v == V)
            return true;

        for (int c = 1; c <= m; ++c)
        {
            if (isSafe(v, color, c))
            {
                color[v] = c;
                if (graphColoringUtil(color, m, v + 1))
                    return true;
                color[v] = 0; // Backtrack
            }
        }
        return false;
    }

    bool graphColoring(int m)
    {
        vector<int> color(V, 0);
        if (!graphColoringUtil(color, m, 0))
        {
            cout << "Solution does not exist.";
            return false;
        }

        cout << "Solution exists: ";
        for (int i = 0; i < V; ++i)
            cout << color[i] << " ";
        return true;
    }
};

int main()
{
    int V = 4;
    Graph g(V);
    g.addEdge(0, 1);
    g.addEdge(0, 2);
    g.addEdge(1, 2);
    g.addEdge(1, 3);
    g.addEdge(2, 3);
    int m = 3; // Number of colors
    g.graphColoring(m);
    return 0;
}

// https://chat.openai.com/share/fbe80553-5396-48a2-9d34-1ff959efa213
