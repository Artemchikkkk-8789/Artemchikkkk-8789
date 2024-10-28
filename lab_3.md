#include <iostream>
#include <stdexcept>
#include <limits>

class Integer {
private:
    int value;
public:
    Integer(int v = 0) : value(v) {}

    // Перевантаження операцій
    Integer operator+(const Integer& other) {
        long long result = static_cast<long long>(value) + other.value;
        if (result > std::numeric_limits<int>::max() || result < std::numeric_limits<int>::min()) {
            throw std::overflow_error("Переповнення при додаванні");
        }
        return Integer(static_cast<int>(result));
    }

    Integer operator-(const Integer& other) {
        long long result = static_cast<long long>(value) - other.value;
        if (result > std::numeric_limits<int>::max() || result < std::numeric_limits<int>::min()) {
            throw std::overflow_error("Переповнення при відніманні");
        }
        return Integer(static_cast<int>(result));
    }

    Integer operator*(const Integer& other) {
        long long result = static_cast<long long>(value) * other.value;
        if (result > std::numeric_limits<int>::max() || result < std::numeric_limits<int>::min()) {
            throw std::overflow_error("Переповнення при множенні");
        }
        return Integer(static_cast<int>(result));
    }

    Integer operator/(const Integer& other) {
        if (other.value == 0) {
            throw std::runtime_error("Ділення на нуль");
        }
        return Integer(value / other.value);
    }

    void print() const {
        std::cout << "Значення: " << value << std::endl;
    }
};

int main() {
    try {
        Integer objA(10);
        Integer objB(5); // Х може бути заміненим на будь-яке число
        Integer objC;

        objC = objA + objB;
        objC.print();

        objC = objA - objB;
        objC.print();

        objC = objA * objB;
        objC.print();

        objC = objA / objB;
        objC.print();
    } catch (const std::exception& e) {
        std::cerr << "Помилка: " << e.what() << std::endl;
    }
    return 0;
}
