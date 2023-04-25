---
title: C++实现简单的分数类
tags:
  - C++基础
abbrlink: '3e83'
date: 2022-01-16 20:25:42
password:
---







~~~c++
#include <iostream>
#include <algorithm>


using std::cout;
using std::endl;


class Fraction{
private:
    int _molecule; //分子
    int _denominator; // 分母


    inline int gcd(int a, int b) {
        return a ? gcd(b % a, a) : b;
    }

    inline int lcm(int a, int b) {
        return a * b / gcd(a, b);
    }

public:

    ~Fraction() {

    }


    Fraction() : _molecule(1), _denominator(1) {

    }

    // a / b
    Fraction(int a, int b) : _molecule(a), _denominator(b) {
        
    }
    Fraction(const Fraction& f) : _molecule(f._molecule), _denominator(f._denominator){

    }

    //f1 + f2
    Fraction operator+(const Fraction& f) {
        int lc = lcm(f._denominator, _denominator);
        Fraction f1{f._molecule * (lc / _denominator) + _molecule * (lc / f._denominator), lc};
        int gc = gcd(f1._denominator, f1._molecule);
        f1._denominator /= gc, f1._molecule /= gc;
        return f1;
    }
    //f1 - f2
    Fraction operator-(const Fraction& f) {
        int lc = lcm(f._denominator, _denominator);
        Fraction f1{_molecule * (lc / f._denominator) - f._molecule * (lc / _denominator), lc};
        return f1;
    }
    // -f
    Fraction operator-() {
        Fraction f{-_molecule, _denominator};
        return f;
    }
    // f1 = f
    Fraction operator=(const Fraction& f) {
        Fraction f1{f._molecule, f._denominator};
        return f1;
    }    
    // f1 /= a
    Fraction& operator/=(int a) {
        int gc = gcd(a, _molecule);
        if (gc == 1) {
            _denominator *= a;
        } else {
            _molecule /= gc;
            _denominator *= a / gc;
        }
        return *this;
    } 
    // f1 / a
    Fraction operator/(int a) {
        Fraction f{*this};
        int gc = gcd(a, f._molecule);
        if (gc == 1) {
            f._denominator *= a;
        } else {
            f._molecule /= gc;
            f._denominator *= a / gc;
        }
        return f;
    } 
    // f1 *= a
    Fraction& operator*=(int a) {
        _molecule *= a;
        int gc = gcd(_molecule, _denominator);
        _molecule /= gc;
        _denominator /= gc;
        return *this;
    }     

    //f1 * f2
    Fraction operator*(const Fraction& f) {
        Fraction f1{_molecule * f._molecule, _denominator * f._denominator};
        int gc = gcd(f1._molecule, f1._denominator);
        return (f1 /= gc);
    }
    // f1 == f2
    bool operator==(const Fraction& f) const {
        // _molecule * f._denominator == f._molecule * _denominator
        return _molecule == f._molecule && _denominator == f._denominator;
    }
    // f1 < f2
    bool operator<(const Fraction& f) const {
        // a/c < b/d => ad < bc;
        return _molecule * f._denominator < f._molecule * _denominator;
    }
    // f1 > f2
    bool operator>(const Fraction& f) const {
        // a/c > b/d => ad > bc;
        return _molecule * f._denominator > f._molecule * _denominator;
    }

    friend std::ostream & operator<<(std::ostream& os, const Fraction& f) {
        os << f._molecule << "/" << f._denominator;
        return os;
    }
};


int main() {
    Fraction f1{1, 6};
    Fraction f2{1, 4};
    Fraction f3 = f2;
    cout << &f3 << " " << &f2 << endl;
    cout << f1 << endl;
    cout << f2 << endl;
    cout << f1 + f2 << endl;
    cout << f1 * f2 << endl;
    cout << -f1 << endl;
    cout << (f1 + f2) / 2 << endl;


    if (f1 == f2) {
        cout << " equalin\n";
    } else if (f1 < f2) {
        cout << " smaller\n";
    } else {
        cout << "bigger\n";
    }
}


~~~

