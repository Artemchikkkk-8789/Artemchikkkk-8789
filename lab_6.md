// student.h
#pragma once
#include <iostream>
#include <string>

class Student {
private:
    std::string surname;
    std::string name;
    std::string patronymic;
    int studentID;
    bool isStateFunded;

public:
    // Конструктор за замовчуванням
    Student();

    // Параметризований конструктор
    Student(const std::string& surname, const std::string& name, const std::string& patronymic, int studentID, bool isStateFunded);

    // Операція виводу в потік
    friend std::ostream& operator<<(std::ostream& os, const Student& student);
};








// student.cpp
#include "student.h"
#include <iostream>

// Конструктор за замовчуванням
Student::Student() {
    std::cout << "Введіть прізвище: ";
    std::cin >> surname;
    std::cout << "Введіть ім'я: ";
    std::cin >> name;
    std::cout << "Введіть по батькові: ";
    std::cin >> patronymic;
    std::cout << "Введіть номер залікової книжки: ";
    std::cin >> studentID;
    std::cout << "Чи є студент державником (1 - так, 0 - ні): ";
    std::cin >> isStateFunded;
}

// Параметризований конструктор
Student::Student(const std::string& surname, const std::string& name, const std::string& patronymic, int studentID, bool isStateFunded)
    : surname(surname), name(name), patronymic(patronymic), studentID(studentID), isStateFunded(isStateFunded) {}

// Операція виводу в потік
std::ostream& operator<<(std::ostream& os, const Student& student) {
    os << "Студент: " << student.surname << " " << student.name << " " << student.patronymic << ", ЗК: " << student.studentID
        << ", Державник: " << (student.isStateFunded ? "Так" : "Ні");
    return os;
}









// grupa.h
#pragma once
#include <iostream>
#include <string>
#include <vector>
#include "student.h"

class Grupa {
private:
    std::string groupName;
    std::string specialty;
    std::vector<Student*> students;

public:
    // Конструктор за замовчуванням
    Grupa();

    // Параметризований конструктор
    Grupa(const std::string& groupName, const std::string& specialty);

    // Деструктор
    ~Grupa();

    // Додати студента
    void addStudent(Student* student);

    // Операція виводу в потік
    friend std::ostream& operator<<(std::ostream& os, const Grupa& grupa);
};
#pragma once











// grupa.cpp
#include "grupa.h"
#include <iostream>

Grupa::Grupa() {}

Grupa::Grupa(const std::string& groupName, const std::string& specialty)
    : groupName(groupName), specialty(specialty) {}

Grupa::~Grupa() {
    // Деструктор не видаляє об'єкти Student, оскільки це агрегація
}

void Grupa::addStudent(Student* student) {
    students.push_back(student);
}

std::ostream& operator<<(std::ostream& os, const Grupa& grupa) {
    os << "Група: " << grupa.groupName << ", Спеціальність: " << grupa.specialty << "\nСтуденти:\n";
    for (const Student* student : grupa.students) {
        os << *student << std::endl;
    }
    return os;
}












#pragma once
// facultet.h
#pragma once
#include <iostream>
#include <string>
#include <vector>
#include "grupa.h"

class Facultet {
private:
    std::string facultyName;
    std::vector<Grupa*> groups;

public:
    // Конструктор за замовчуванням
    Facultet();

    // Параметризований конструктор
    Facultet(const std::string& facultyName);

    // Деструктор
    ~Facultet();

    // Додати групу
    void addGroup(Grupa* group);

    // Операція виводу в потік
    friend std::ostream& operator<<(std::ostream& os, const Facultet& facultet);
};

















// facultet.cpp
#include "facultet.h"
#include <iostream>

Facultet::Facultet() {}

Facultet::Facultet(const std::string& facultyName) : facultyName(facultyName) {}

Facultet::~Facultet() {
    for (Grupa* group : groups) {
        delete group; // Знищуємо групи, оскільки це композиція
    }
}

void Facultet::addGroup(Grupa* group) {
    groups.push_back(group);
}

std::ostream& operator<<(std::ostream& os, const Facultet& facultet) {
    os << "Факультет: " << facultet.facultyName << "\nГрупи:\n";
    for (const Grupa* group : facultet.groups) {
        os << *group << std::endl;
    }
    return os;
}





























#include <locale>
#include <iostream>
#include "student.h"
#include "grupa.h"
#include "facultet.h"

int main() {
    // Встановлення української локалі для відображення символів
    setlocale(LC_ALL, "uk_UA.UTF-8");

    // Створення студентів
    Student student1("Іваненко", "Іван", "Іванович", 12345, true);
    Student student2("Петренко", "Петро", "Петрович", 54321, false);
    Student student3;

    // Створення групи та додавання студентів
    Grupa* grupa = new Grupa("КН-323", "комп'ютерні науки");
    grupa->addStudent(&student1);
    grupa->addStudent(&student2);
    grupa->addStudent(&student3);

    // Створення факультету та додавання групи
    Facultet facultet("Факультет Комп'ютерних Наук");
    facultet.addGroup(grupa);

    // Виведення факультету
    std::cout << facultet << std::endl;

    return 0;
}
