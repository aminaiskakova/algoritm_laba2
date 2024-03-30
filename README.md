#include <iostream>
#include <fstream>
#include <cstdlib>
#include <ctime>

using namespace std;

template <typename T>
class Sort {
public:

	static int shell_sort(T** arr, int n)
	{
		int sravnenie = 0;
		int i, j, h;
		for (h = 1; h <= n / 9; h = h * 3 + 1);
		while (h >= 1)
		{
			for (i = h; i < n; i++)
				for (j = i - h; j >= 0 && *arr[j+h] < *arr[j]; j -= h)
				{
					swap(arr[j], arr[j + h]);
					sravnenie++;
				}
			h = (h - 1) / 3;
		}
		return sravnenie;
	}

	static int quickSort(T** arr, int l, int r) {
		int sravn = 0;
		int i, j, a = l, b = r;
		while (a < b) {
			T* x = arr[(a + b) / 2]; i = a; j = b;
			while (i < j) {
				while (*arr[i] < *x) {
					i++;
					/*sravn++;*/
				}

				while (*arr[j] > *x) {
					j--;
					/*sravn++;*/
				}
				if (i <= j)
				{
					swap(arr[i], arr[j]);
					i++; j--;
					sravn++;
				}
			}
			if (j - a < b - i)
			{
				if (a < j) quickSort(arr, a, j);
				a = i;
			}
			else {
				if (i < b) quickSort(arr, i, b);
				b = j;
			}
		}


		return sravn;
	}
};

class Participant {
public:
    int id;
    int solved;
    int time;
    int points;
    int attempts;

    Participant(int id, int solved, int time, int points, int attempts)
        : id(id), solved(solved), time(time), points(points), attempts(attempts) {}

    bool operator<(const Participant& other) const {
        if (points != other.points) return points > other.points;
        if (solved != other.solved) return solved > other.solved;
        if (time != other.time) return time < other.time;
        if (attempts != other.attempts) return attempts < other.attempts;
        return id < other.id;
    }

    friend ostream& operator << (ostream& r, Participant& x)//потоковый выввод 
    {
        r << x.points << "\t" << x.solved << "\t" << x.time << "\t" << x.attempts << "\t" << x.id << "\n";
        return r;
    }
};


int main() {
    setlocale(LC_ALL, "Russian");

    int n;
    cout << "введите колво участников: ";
    cin >> n;
    Participant** participants = new Participant * [n];
    for (int i = 0; i < n; i++) {
        int solvedTasks = rand() % 6;
        int timeSpent = rand() % 241 + 60;
        int points = rand() % 26;
        int attempts = rand() % 21;
        participants[i] = new Participant(i + 1, solvedTasks, timeSpent, points, attempts);
    }

    Sort<Participant>::shell_sort(participants, n);

    ofstream outFile("results1.txt");
    outFile << "баллы\tрешенные\tвремя\tпопытка\tID\n";
    for (int i = 0; i < n; i++)
    {
        outFile << *participants[i];
        delete participants[i];
    }
    outFile.close();
    delete[] participants;
    return 0;
}
