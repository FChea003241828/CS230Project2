#include <iostream>
#include <string>
#include <iomanip>
#include <bitset>
#include <cmath>

using namespace std;

void processHexInput(const string& hexInput);

int main() {
    cout << "; CS230 Spring 2025 Lab Assignment 1 - Chea, Fatima - Section nnnnn" << endl;
    cout << "; Compiler used: Visual Studio" << endl;
    cout << "CS230 Spring 2025 Lab 1 - Chea, Fatima" << endl;

    string input;
    char continueChoice;

    do {
        cout << "Enter four hexadecimal digits: ";
        cin >> input;

        bool isValid = true;
        for (char c : input) {
            if (!isxdigit(c)) {
                isValid = false;
                break;
            }
        }

        if (!isValid || input.length() != 4) {
            cout << "Input characters must be in the ranges of 0-9, a-z, or A-Z" << endl;
        } else {
            processHexInput(input);
        }

        do {
            cout << "Continue? enter Y or N: ";
            cin >> continueChoice;
            continueChoice = tolower(continueChoice);
        } while (continueChoice != 'y' && continueChoice != 'n');

    } while (continueChoice == 'y');

    cout << "Done, program ending." << endl;
    return 0;
}

void processHexInput(const string& hexInput) {
    unsigned short hexValue = stoi(hexInput, nullptr, 16);

    // Extract fields: sign, exponent, and significand
    unsigned short sign = (hexValue >> 15) & 0x1; // 1-bit sign
    unsigned short exponent = (hexValue >> 10) & 0x1F; // 5-bit exponent
    unsigned short significand = hexValue & 0x3FF; // 10-bit significand

    cout << "Sign: " << sign
         << " Exponent:" << bitset<5>(exponent)
         << " Significand:" << bitset<10>(significand) << endl;

    if (exponent == 0 && significand == 0) {
        cout << "Zero" << endl;
    } else if (exponent == 31) { // Check if exponent is 11111
        if (significand == 0) {
            cout << (sign ? "Negative infinity" : "Positive infinity") << endl;
        } else {
            cout << "...significand should be zero." << endl;
        }
    } else if (exponent == 0) {
        cout << "Subnormal numbers are not allowed" << endl;
    } else {
        // Calculate the float value for normalized numbers
        float value = pow(2, exponent - 15) * (1 + significand / 1024.0);
        value = sign ? -value : value;
        cout << "Value:" << value << endl;
    }
}
