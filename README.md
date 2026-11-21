#include <iostream>
#include <vector>
#include <limits>
#include <algorithm>
#include <iomanip>

int displayMenuAndGetChoice();
double getValidDouble(const std::string& prompt);
void addGrade(std::vector<double>& grades);
void displayAllGrades(const std::vector<double>& grades);
void calculateAverageGrade(const std::vector<double>& grades);
void findHighestAndLowestGrade(const std::vector<double>& grades);
void countGradesAboveThreshold(const std::vector<double>& grades);

int main() {
    std::vector<double> grades;
    int choice = 0;

    std::cout << std::fixed << std::setprecision(2);

    do {
        choice = displayMenuAndGetChoice();

        switch (choice) {
            case 1:
                addGrade(grades);
                break;
            case 2:
                displayAllGrades(grades);
                break;
            case 3:
                calculateAverageGrade(grades);
                break;
            case 4:
                findHighestAndLowestGrade(grades);
                break;
            case 5:
                countGradesAboveThreshold(grades);
                break;
            case 6:
                std::cout << "Exiting program. Goodbye!\n";
                break;
        }
    } while (choice != 6);

    return 0;
}

int displayMenuAndGetChoice() {
    int choice = 0;

    while (true) {
        std::cout << "\n--- Grade Management Menu ---\n"
                  << "1. Add a Grade\n"
                  << "2. Display All Grades\n"
                  << "3. Calculate Average Grade\n"
                  << "4. Find Highest and Lowest Grade\n"
                  << "5. Count Grades Above Threshold\n"
                  << "6. Quit\n"
                  << "Enter your choice (1-6): ";

        if (std::cin >> choice && choice >= 1 && choice <= 6) {
            
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
            return choice;
        } else {
            std::cout << "Invalid input. Please enter a number between 1 and 6.\n";
            std::cin.clear(); // Reset error flags
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n'); // Discard bad input
        }
    }
}

double getValidDouble(const std::string& prompt) {
    double value = 0.0;

    while (true) {
        std::cout << prompt;
        if (std::cin >> value) {
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
            return value;
        } else {
            std::cout << "Invalid input. Please enter a valid numeric value.\n";
            std::cin.clear();
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
        }
    }
}

void addGrade(std::vector<double>& grades) {
    double grade = getValidDouble("Enter the grade to add: ");
    grades.push_back(grade);
    std::cout << "Grade " << grade << " added successfully.\n";
}

void displayAllGrades(const std::vector<double>& grades) {
    if (grades.empty()) {
        std::cout << "No grades recorded yet.\n";
        return;
    }

    std::cout << "Stored Grades: ";
    for (size_t i = 0; i < grades.size(); ++i) {
        std::cout << grades[i];
        if (i < grades.size() - 1) {
            std::cout << ", ";
        }
    }
    std::cout << "\n";
}

void calculateAverageGrade(const std::vector<double>& grades) {
    if (grades.empty()) {
        std::cout << "Cannot calculate average: No grades recorded yet.\n";
        return;
    }

    double sum = 0.0;
    for (double grade : grades) {
        sum += grade;
    }

    double average = sum / grades.size();
    std::cout << "Average Grade: " << average << "\n";
}

void findHighestAndLowestGrade(const std::vector<double>& grades) {
    if (grades.empty()) {
        std::cout << "Cannot find highest and lowest: No grades recorded yet.\n";
        return;
    }

    auto minmax = std::minmax_element(grades.begin(), grades.end());
    std::cout << "Lowest Grade: " << *minmax.first << "\n";
    std::cout << "Highest Grade: " << *minmax.second << "\n";
}

void countGradesAboveThreshold(const std::vector<double>& grades) {
    if (grades.empty()) {
        std::cout << "No grades recorded yet to compare against a threshold.\n";
        return;
    }

    double threshold = getValidDouble("Enter the grade threshold: ");
    int count = 0;

    for (double grade : grades) {
        if (grade > threshold) {
            ++count;
        }
    }

    std::cout << "Number of grades above " << threshold << ": " << count << "\n";
}
