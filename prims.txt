#include <bits/stdc++.h>
using namespace std;
typedef pair<int, int> pii;
typedef pair<int, pair<int, int>> triple_pair;

struct CompareTriplePairs
{
    bool operator()(const triple_pair &lhs, const triple_pair &rhs) const
    {
        // Comparison logic here
        // For example, compare based on the first element of the pair
        return lhs.first > rhs.first;
    }
};

class Graph
{
    int v;
    vector<vector<pii>> adj;

public:
    Graph(int v) : v(v)
    {
        adj.resize(v);
    }

    void addEdge(int u, int v, int w)
    {
        adj[u].push_back(make_pair(v, w));
        adj[v].push_back(make_pair(u, w));
    }

    // int primMST()
    // {
    //     priority_queue<triple_pair, vector<triple_pair>, CompareTriplePairs> pq; // {weight, {source, dest}};
    //     vector<bool> visited(v, false);
    //     vector<pii> mst;
    //     pq.push({0, {0, 0}});
    //     int sum = 0;
    //     while (!pq.empty())
    //     {
    //         auto it = pq.top();
    //         pq.pop();
    //         int weight = it.first;
    //         int source = it.second.first;
    //         int dest = it.second.second;
    //         mst.push_back({source, dest});
    //         if (visited[source])
    //             continue;

    //         visited[source] = true;
    //         sum += weight;
    //         for (auto it : adj[source])
    //         {
    //             int neighbour = it.first;
    //             int wt = it.second;
    //             if (!visited[neighbour])
    //             {
    //                 pq.push({wt, {source, neighbour}});
    //             }
    //         }
    //     }
    //     return sum;
    // }

    int primMST()
    {
        priority_queue<triple_pair, vector<triple_pair>, CompareTriplePairs> pq; // {weight, {source, dest}};
        vector<bool> visited(v, false);
        vector<pii> mst;
        int sum = 0;
        pq.push({0, {0, 0}});
        while (!pq.empty())
        {
            auto it = pq.top();
            pq.pop();
            int weight = it.first;
            int source = it.second.first;
            int dest = it.second.second;
            if (visited[dest]) // If the destination is already visited, skip this edge
                continue;
            visited[dest] = true;
            sum += weight;
            mst.push_back({source, dest});
            for (auto neighbor : adj[dest])
            {
                int neighbor_node = neighbor.first;
                int neighbor_weight = neighbor.second;
                if (!visited[neighbor_node])
                {
                    pq.push({neighbor_weight, {dest, neighbor_node}});
                }
            }
        }
        for (auto it : mst)
        {
            cout << it.first << "-" << it.second << endl;
        }
        return sum;
    }
};

int main()
{
    Graph g(5);
    g.addEdge(0, 1, 4);
    g.addEdge(0, 2, 2);
    g.addEdge(0, 3, 1);
    g.addEdge(3, 2, 6);
    g.addEdge(1, 2, 10);
    // g.addEdge(1, 4, 7);
    g.addEdge(2, 4, 3);
    // g.addEdge(2, 4, 8);
    // g.addEdge(2, 5, 1);
    // g.addEdge(3, 5, 9);
    // g.addEdge(4, 5, 10);
    int minWeight = g.primMST();
    cout << "Minimum spanning tree weight: " << minWeight << endl;

    return 0;
}