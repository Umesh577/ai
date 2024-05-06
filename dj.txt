#include <iostream>
#include <vector>
#include <queue>
#include <climits>

using namespace std;

#define INF INT_MAX

// Data structure to represent a graph edge
struct Edge
{
    int dest, weight;
};

// Data structure to represent a graph node
struct Node
{
    int vertex, dist;
};

// Comparison function for priority queue
struct compare
{
    bool operator()(const Node &a, const Node &b)
    {
        return a.dist > b.dist;
    }
};

// Function to perform Dijkstra's algorithm
void dijkstra(vector<vector<Edge>> &graph, int source)
{
    int V = graph.size();
    vector<int> dist(V, INF);
    vector<bool> visited(V, false);

    // Priority queue to store vertex-distance pairs
    priority_queue<Node, vector<Node>, compare> pq;

    // Add source vertex to the priority queue
    pq.push({source, 0});
    dist[source] = 0;

    while (!pq.empty())
    {
        int u = pq.top().vertex;
        pq.pop();

        // If vertex u is already visited, skip
        if (visited[u])
            continue;

        // Mark vertex u as visited
        visited[u] = true;

        // Update the distance of all adjacent vertices of u
        for (const Edge &e : graph[u])
        {
            int v = e.dest;
            int weight = e.weight;
            if (!visited[v] && dist[u] != INF && dist[u] + weight < dist[v])
            {
                dist[v] = dist[u] + weight;
                pq.push({v, dist[v]});
            }
        }
    }

    // Print the shortest distances from the source vertex
    cout << "Shortest distances from vertex " << source << ":\n";
    for (int i = 0; i < V; ++i)
    {
        if (dist[i] == INF)
            cout << i << ": INF\n";
        else
            cout << i << ": " << dist[i] << "\n";
    }
}

int main()
{
    int V, E;
    cout << "Enter the number of vertices and edges: ";
    cin >> V >> E;

    vector<vector<Edge>> graph(V);

    cout << "Enter the edges and weights (source destination weight):\n";
    for (int i = 0; i < E; ++i)
    {
        int u, v, w;
        cin >> u >> v >> w;
        graph[u].push_back({v, w});
        // For undirected graph, uncomment the next line
        // graph[v].push_back({u, w});
    }

    int source;
    cout << "Enter the source vertex: ";
    cin >> source;

    dijkstra(graph, source);

    return 0;
}
