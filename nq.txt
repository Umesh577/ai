#include <iostream>
#include <vector>
#include <string>
#include <unordered_set>

using namespace std;

class Solution
{
private:
    vector<vector<string>> result;
    unordered_set<int> col;
    unordered_set<int> posDiag;
    unordered_set<int> negDiag;
    int N;

    void backtrack(int row, vector<string> &board)
    {
        if (row == N)
        {
            result.push_back(board);
            return;
        }

        for (int c = 0; c < N; ++c)
        {
            if (col.count(c) || posDiag.count(row + c) || negDiag.count(row - c))
                continue;

            col.insert(c);
            posDiag.insert(row + c);
            negDiag.insert(row - c);
            board[row][c] = 'Q';

            backtrack(row + 1, board);

            col.erase(c);
            posDiag.erase(row + c);
            negDiag.erase(row - c);
            board[row][c] = '.';
        }
    }

public:
    vector<vector<string>> solveNQueens(int n)
    {
        N = n;
        vector<string> board(n, string(n, '.'));
        backtrack(0, board);
        return result;
    }
};

int main()
{
    int n;
    cout << "Enter the size of the chessboard (N x N): ";
    cin >> n;

    Solution solution;
    vector<vector<string>> solutions = solution.solveNQueens(n);

    cout << "\nSolutions:\n";
    for (const auto &solution : solutions)
    {
        for (const auto &row : solution)
        {
            cout << row << endl;
        }
        cout << endl;
    }

    return 0;
}
