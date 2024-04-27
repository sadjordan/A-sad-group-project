//A-sad-group-project

#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>

using namespace std;

class RaceTrack {
private:
    int size;
    vector<vector<char> > track;

public:
    RaceTrack(int s) : size(s) {
        track.resize(size, vector<char>(size, ' '));
    }

    void occupyBlock(int row, int col) {
        if (row >= 0 && row < size && col >= 0 && col < size)
            track[row][col] = 'X';
    }

    bool isBlockOccupied(int row, int col) const {
        if (row >= 0 && row < size && col >= 0 && col < size)
            return track[row][col] == 'X';
        return false;
    }

    void display() const {
        for (int i = 0; i < size; ++i) {
            cout << "+---";
        }
        cout << "+" << endl;

        for (int i = 0; i < size; ++i) {
            cout << "|";
            for (int j = 0; j < size; ++j) {
                cout << " " << track[i][j] << " |";
            }
            cout << endl;
            for (int j = 0; j < size; ++j) {
                cout << "+---";
            }
            cout << "+" << endl;
        }
    }
    void setStartingPoint(int row, int col) {
        if (row >= 0 && row < size && col >= 0 && col < size)
            track[row][col] = 'S';
    }
};

class Racer {
private:
    string name;
    int row;
    int col;
    int minSteps;
    int maxSteps;

public:
    Racer(string n, int minS, int maxS) : name(n), row(0), col(0), minSteps(minS), maxSteps(maxS) {}

    void move(int steps) {
        col += steps;
    }

    int getRow() const {
        return row;
    }

    int getCol() const {
        return col;
    }

    string getName() const {
        return name;
    }

    int generateSteps() const {
        return rand() % (maxSteps - minSteps + 1) + minSteps;
    }
};

int main() {
    srand(time(nullptr));

    const int trackSize = 8; // Number of blocks in each row/column
    RaceTrack track(trackSize);

    Racer superman("Superman", 3, 5);
    Racer flash("Flash", 2, 6);

    track.setStartingPoint(0, 0); // Set starting point for racers

    cout << "Initial Track:" << endl;
    track.display();
    cout << endl;

    while (true) {
        int supermanSteps = superman.generateSteps();
        int flashSteps = flash.generateSteps();

        superman.move(supermanSteps);
        flash.move(flashSteps);

        if (superman.getCol() >= trackSize && flash.getCol() >= trackSize) {
            cout << "Race is tied!" << endl;
            break; // Both racers reached the finish line at the same time
        }

        if (superman.getCol() >= trackSize) {
            cout << "Superman wins!" << endl;
            break; // Superman reached the finish line
        }

        if (flash.getCol() >= trackSize) {
            cout << "Flash wins!" << endl;
            break; // Flash reached the finish line
        }
    }

    return 0;
}
