[README.md](https://github.com/user-attachments/files/18824180/README.md)


Headers and Namespace*

#include <iostream> #include <vector> #include <iomanip> using namespace std;

#include <iostream>: Allows input and output operations (e.g., cout for displaying results).

#include <vector>: Enables the use of dynamic arrays (vectors) for storing the tableau.

#include <iomanip>: Used for formatting the output (e.g., setting column width).

using namespace std;: Avoids writing std:: before standard functions like cout.

Type Definition
typedef vector<vector<double>> Matrix;

Defines Matrix as a 2D vector of double values, making it easier to represent the Simplex tableau.

Function to Print the Tableau
void printTableau(const Matrix &tableau) { for (c>auto &row : tableau) { for (double val : row) cout << setw(10) << val << " "; cout << endl; } cout << endl; }

printTableau prints the Simplex tableau in a readable format.

setw(10): Sets column width to align numbers properly.

Iterates over each row and column to display values.

Function to Find the Pivot Column
int findPivotColumn(const Matrix &tableau) { int pivotCol = 0; for (size_t j = 1; j < tableau[0].size() - 1; j++) if (tableau[0][j] < tableau[0][pivotCol]) pivotCol = j; return tableau[0][pivotCol] < 0 ? pivotCol : -1; }

Finds the most negative value in the objective function row (row 0) to select the entering variable.

If no negative value is found, the solution is optimal (-1 is returned).

Function to Find the Pivot Row
int findPivotRow(c>int pivotCol) { int pivotRow = -1; double minRatio = 1e9; for (size_t i = 1; i < tableau.size(); i++) { if (tableau[i][pivotCol] > 0) { double ratio = tableau[i].back() / tableau[i][pivotCol]; if (ratio < minRatio) { minRatio = ratio; pivotRow = i; } } } return pivotRow; }

Determines which constraint row should be used for the pivot.

Uses the minimum ratio test: Divides the rightmost column value by the pivot column value.

Returns pivotRow, which represents the leaving variable.

Function to Perform Pivoting
void pivot(Matrix &tableau, int pivotRow, int pivotCol) { double pivotElement = tableau[pivotRow][pivotCol]; for (double &val : tableau[pivotRow]) val /= pivotElement; for (size_t i = 0; i < tableau.size(); i++) { if (i != pivotRow) { double factor = tableau[i][pivotCol]; for (size_t j = 0; j < tableau[i].size(); j++) tableau[i][j] -= factor * tableau[pivotRow][j]; } } }

Divides the pivot row by the pivot element to make the pivot element 1.

Uses row operations to make all other values in the pivot column zero.

Simplex Algorithm Execution
void simplex(Matrix &tableau) { while (true) { int pivotCol = findPivotColumn(tableau); if (pivotCol == -1) break; int pivotRow = findPivotRow(tableau, pivotCol); if (pivotRow == -1) { cout << "Unbounded Solution" << endl; return; } pivot(tableau, pivotRow, pivotCol); printTableau(tableau); } cout << "Optimal Solution Found: " << tableau[0].back() << endl; }

Repeatedly finds the pivot column, finds the pivot row, and performs pivoting.

If no pivot column exists (-1 is returned), the solution is optimal.

If no pivot row exists, the problem is unbounded.

Main Function
int main() { Matrix tableau = { {1, -3, -5, 0, 0, 0, 0}, {0, 1, 0, 1, 0, 0, 4}, {0, 0, 2, 0, 1, 0, 12}, {0, 3, 2, 0, 0, 1, 18} }; printTableau(tableau); simplex(tableau); return 0; }

Initializes a Simplex tableau.

Calls printTableau to display the initial table.

Calls simplex to solve the linear programming problem.

Prints the final optimal solution.

How It Works 1. The initial Simplex tableau is set up. 2. The algorithm repeatedly: - Finds the pivot column (most negative value in the first row). - Finds the pivot row (using the minimum ratio test). - Performs pivoting to update the tableau. - Repeats until the objective function row has no negative values. 3. The final result is displayed as the optimal solution.

