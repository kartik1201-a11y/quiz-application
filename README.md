#include <iostream>
#include <string>
#include <vector>
using namespace std;

// Custom Exception class
class InvalidOptionException {
public:
    string message;
    InvalidOptionException(string msg) : message(msg) {}
};

// Base class
class Question {
protected:
    string questionText;
    string options[4];
    int correctOption;

public:
    Question(string q, string opt1, string opt2, string opt3, string opt4, int correct) {
        questionText = q;
        options[0] = opt1;
        options[1] = opt2;
        options[2] = opt3;
        options[3] = opt4;
        correctOption = correct;
    }

    virtual void display() {
        cout << "\n" << questionText << endl;
        for (int i = 0; i < 4; i++)
            cout << i + 1 << ". " << options[i] << endl;
    }

    virtual bool checkAnswer(int userAnswer) {
        return userAnswer == correctOption;
    }
};

// Derived class
class Quiz {
    vector<Question> questions;
    int score;

public:
    Quiz() : score(0) {}

    void addQuestion(Question q) {
        questions.push_back(q);
    }

    void start() {
        cout << "\n=== 🧠 Welcome to the C++ OOP Quiz ===\n";
        int ans;

        for (int i = 0; i < questions.size(); i++) {
            questions[i].display();

            try {
                cout << "Enter your answer (1-4): ";
                cin >> ans;

                if (cin.fail() || ans < 1 || ans > 4) {
                    cin.clear();
                    cin.ignore(1000, '\n');
                    throw InvalidOptionException("❌ Invalid choice! Please enter a number between 1 and 4.");
                }

                if (questions[i].checkAnswer(ans))
                    score++;
            }
            catch (InvalidOptionException &e) {
                cout << e.message << endl;
            }
        }

        cout << "\n=== 🎯 Quiz Completed! ===" << endl;
        cout << "Your final score: " << score << "/" << questions.size() << endl;

        if (score == questions.size())
            cout << "🏆 Excellent! You got all correct!" << endl;
        else if (score >= questions.size() / 2)
            cout << "👍 Good job! You passed the quiz." << endl;
        else
            cout << "📘 Keep practicing and try again!" << endl;
    }
};

// Main function
int main() {
    Quiz quiz;

    // Adding Questions
    quiz.addQuestion(Question("1. What is the capital of India?",
                              "Mumbai", "New Delhi", "Kolkata", "Chennai", 2));

    quiz.addQuestion(Question("2. Which keyword is used for exception handling in C++?",
                              "throw", "catch", "try", "All of the above", 4));

    quiz.addQuestion(Question("3. Who invented C++?",
                              "James Gosling", "Guido van Rossum", "Bjarne Stroustrup", "Dennis Ritchie", 3));

    quiz.addQuestion(Question("4. Which of the following is not a C++ access specifier?",
                              "public", "private", "protected", "package", 4));

    quiz.addQuestion(Question("5. What is used to handle runtime errors in C++?",
                              "Polymorphism", "Exception Handling", "Overloading", "Encapsulation", 2));

    quiz.addQuestion(Question("6. Which operator is used to allocate memory dynamically in C++?",
                              "alloc", "malloc", "new", "create", 3));

    quiz.addQuestion(Question("7. Which concept allows the use of the same function name for different tasks?",
                              "Inheritance", "Encapsulation", "Polymorphism", "Abstraction", 3));

    // 🔧 FIXED LINE (escaped apostrophe in doesn't)
    quiz.addQuestion(Question("8. Which statement is true about OOP?",
                              "It focuses on functions.",
                              "It organizes code using objects.",
                              "It doesn\'t support reuse.",
                              "None of the above.", 2));

    // Start Quiz
    quiz.start();

    return 0;
}
