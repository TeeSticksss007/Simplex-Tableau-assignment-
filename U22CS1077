#include <iostream>
#include <vector>
#include <iomanip>

using namespace std;

typedef vector<vector<double>> Matrix;

void printTableau(const Matrix &tableau) {
    for (const auto &row : tableau) {
        for (double val : row)
            cout << setw(10) << val << " ";
        cout << endl;
    }
    cout << endl;
}

int findPivotColumn(const Matrix &tableau) {
    int pivotCol = 0;
    for (size_t j = 1; j < tableau[0].size() - 1; j++)
        if (tableau[0][j] < tableau[0][pivotCol])
            pivotCol = j;
    return tableau[0][pivotCol] < 0 ? pivotCol : -1;
}

int findPivotRow(const Matrix &tableau, int pivotCol) {
    int pivotRow = -1;
    double minRatio = 1e9;
    for (size_t i = 1; i < tableau.size(); i++) {
        if (tableau[i][pivotCol] > 0) {
            double ratio = tableau[i].back() / tableau[i][pivotCol];
            if (ratio < minRatio) {
                minRatio = ratio;
                pivotRow = i;
            }
        }
    }
    return pivotRow;
}

void pivot(Matrix &tableau, int pivotRow, int pivotCol) {
    double pivotElement = tableau[pivotRow][pivotCol];
    for (double &val : tableau[pivotRow])
        val /= pivotElement;
    for (size_t i = 0; i < tableau.size(); i++) {
        if (i != pivotRow) {
            double factor = tableau[i][pivotCol];
            for (size_t j = 0; j < tableau[i].size(); j++)
                tableau[i][j] -= factor * tableau[pivotRow][j];
        }
    }
}

void simplex(Matrix &tableau) {
    while (true) {
        int pivotCol = findPivotColumn(tableau);
        if (pivotCol == -1) break;
        int pivotRow = findPivotRow(tableau, pivotCol);
        if (pivotRow == -1) {
            cout << "Unbounded Solution" << endl;
            return;
        }
        pivot(tableau, pivotRow, pivotCol);
        printTableau(tableau);
    }
    cout << "Optimal Solution Found: " << tableau[0].back() << endl;
}

int main() {
    Matrix tableau = {
        {1, -3, -5, 0, 0, 0, 0},
        {0, 1, 0, 1, 0, 0, 4},
        {0, 0, 2, 0, 1, 0, 12},
        {0, 3, 2, 0, 0, 1, 18}
    };
    printTableau(tableau);
    simplex(tableau);
    return 0;
}
