#include <iostream>
#include <locale>
#include <conio.h>

using namespace std;

class Kasa {
private:
    int carCount;
    double totalRevenue;

public:
    Kasa() : carCount(0), totalRevenue(0.0) {}

    void payingCar() {
        carCount++;
        totalRevenue += 0.50;
    }

    void nopayCar() {
        carCount++;
    }

    void display() const {
        wcout << L"Кількість автомобілів: " << carCount << endl;
        wcout << L"Сумарна виручка: $" << totalRevenue << endl;
    }
};

int main() {
    setlocale(LC_ALL, "ukr");
    Kasa tollBooth;

    wcout << L"Натисніть 'P' для оплати, 'N' для безкоштовного проїзду, або 'Esc' для виходу." << endl;

    char key;
    while (true) {
        key = _getch();

        if (key == 'P' || key == 'p') {
            tollBooth.payingCar();
        } else if (key == 'N' || key == 'n') {
            tollBooth.nopayCar();
        } else if (key == 27) { 
            tollBooth.display();
            break;
        }
    }

    return 0;
}
