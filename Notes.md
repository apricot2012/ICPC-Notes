
# Input/Output (IO)

### C++
- cin/cout: easy to use but slow
- De-sync with scanf/printf to get faster performance:
    - ``ios_base::sync_with_stdio(0); cin.tie(0);``
      - https://stackoverflow.com/questions/31162367/significance-of-ios-basesync-with-stdiofalse-cin-tienull
    - Don’t flush buffer: ``cout << '\n'`` is faster than ``cout << endl``
- scanf/printf: faster than cin/cout

#### Example
```cpp
 #include <iomanip>
 #include <iostream>
 using namespace std;
 int main() {
 // un-sync with scanf/printf for fast performance
    ios_base::sync_with_stdio(0); cin.tie(0);

    int a; long long b; double c; string s;
    cin >> a >> b >> c >> s;
    cout << setprecision(16) << fixed << a << " " << b << " " << c << " " << s << endl;
    // setprecision sets precision
    return 0;
 }
```

### Floating Point Errors

- Set epsilon, add before rounding up.
- Do comparisons with ``if (abs(a-b) < 1e-8) { ... }``

# Graphs

### Adjacency List
For every vertex, store a list of neighbours.
```cpp
vector<vector<int>> adj(n);
 //...for each edge e=uv
 adj[u].push_back(v);
 adj[v].push_back(u);
```
**\+** $O(n + m)$ memory to store.
**\+** $O(d)$ time to find all neighbours of a vertex with degree d.
**\-** $O(d)$ pairwise adjacency testing of two given vertices, where the degree of the first is d.
**\+** In practice, we can do fast pairwise adjacency testing using a hashtable of neighbours.

### Adjacency Matrix
For every pair of vertices, store a boolean value.
```cpp
 vector<vector<bool>> adj_matrix(n,vector<bool>(n));
 //...for each edge e=uv
 adj_matrix[u][v] = adj_matrix[v][u] = true;
```
**\+** Fast pairwise adjacency testing of two given vertices.
**\+** Useful for spectral graph theory.
**\-** In total, they take $O(n^2)$ memory to store.
**\-** It takes $O(n)$ time to find all the neighbours of a given vertex, even if the vertex has no neighbours!

## DFS
```cpp
void dfs(int u, vector<vector<int>> &adj, vector<bool> &visited) {
  if (visited[u]) return false; visited[u] = true;
  for (int neighbour_of_u : adj[u])
  dfs(neighbour_of_u, adj, visited);
}
bool are_connected(int u, int v, vector<vector<int>> &adj) {
  vector<bool> visited(adj.size(), false);
  dfs(u, adj, visited);
  return visited[v];
}
```
Finds all connected components.
*Runtime:* $O(n + m)$

## BFS
```cpp
vector<int> bfs(int u, vector<vector<int>> &adj) {
  vector<bool> visited(adj.size());
  vector<int> dist(adj.size());
  for (int i = 0; i < adj.size(); ++i) dist[i] = adj.size()+1;
  queue<int> todo; todo.push(u); dist[u] = 0;
  while (!todo.empty()) {
    int cur = todo.front(); todo.pop();
    if (visited[cur]) continue;
    visited[cur] = true;
    for (int w : adj[cur]) {
      todo.push(w); dist[w] = min(dist[w],dist[cur]+1);
    }
  }
  return dist; // dist[v] is the distance to node v
}

int shortest_path(int u, int v, vector<vector<int>>& adj) {
  return bfs(u, adj)[v];
}
```
*Runtime:* $O(n + m)$

# Code Template
```cpp
#include <bits/stdc++.h>
#define EPS 1e-8;
using namespace std;
typedef long long ll;
typedef long double ld;
typedef complex<ld> pt;
typedef vector<pt> pol;
typedef vector<ll> vi;
typedef vector<vi> vvi;
typedef vector<ld> vd;
typedef vector<vd> vvd;
typedef pair<ll, ll> pii;
typedef vector<pii> vpii;

int main() {
      ios_base::sync_with_stdio(false);
      cin.tie(NULL);
}
```

# Code Archive
## Number Theory
#### Quick Powers (For mod division maybe)
```cpp
long long mod = 1000000000 + 7;

long long findpower(long long x, long long n){
    long long result = 1;
    while (n != 0) {
        if (n % 2 == 1) {
            result = (result % mod) * (x % mod);
            result = result % mod;
            n--;
        } else if (n % 2 == 0) {
            x = ((x % mod)*(x % mod)) % mod;
            n = n/2;
        }
    }
    return result;

}
```
#### Sieve Prime Search
```cpp
void SieveOfEratosthenes(int n)
{

    memset(prime, true, sizeof(prime));

    for (int p = 2; p * p <= n; p++)
    {
        if (prime[p] == true)
        {
            for (int i = p * p; i <= n; i += p)
                prime[i] = false;
        }
    }

    for (int p = 2; p <= n; p++)
        if (prime[p])
            cout << p << " ";
}
```


## Binary Search
```cpp
int binarySearch(int arr[], int l, int r, int x)
{
    while (l <= r) {
        int m = l + (r - l) / 2;

        if (arr[m] == x)
            return m;

        if (arr[m] < x)
            l = m + 1;

        else
            r = m - 1;
    }

    return -1;
}
```
## Ternary Search
```cpp
double ternarySearch(double l, double r) {
    double eps = 1e-9;              //set the error limit here
    while (r - l > eps) {
        double m1 = l + (r - l) / 3;
        double m2 = r - (r - l) / 3;
        double f1 = f(m1);      //evaluates the function at m1
        double f2 = f(m2);      //evaluates the function at m2
        if (f1 < f2)
            l = m1;
        else
            r = m2;
    }
    return f(l);                    //return the maximum of f(x) in [l, r]
}
```
