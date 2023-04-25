---
title: 实现vector模块
tags: C++基础
abbrlink: a3e
date: 2022-01-22 21:41:59
password:
---







~~~c
#include <iostream>
#include <vector>
#include <string.h>
#include <stdlib.h>
#include <string>

using std::cout;
using std::endl;


namespace master{

    template<typename T>
    class vector{
    private:
        T* _begin; //数组的起始地址
        T* _capacity; //数组最后一个元素后面 元素地址
        T* _end;  //插入元素的最后一个元素后面 元素地址


    public:

        inline T* begin() const { return _begin; }
        inline T* end() const { return _end; }
        inline T* capacity() const { return _capacity; }

        // 空返回true，非空返回false
        inline bool empty() { return (this->_end == this->_begin); }

        //获取当前元素个数
        size_t size() const {
            return (this->_end - this->_begin);
        }

        //获取可以容纳元素最大数量
        size_t maxSize() const {
            return _capacity - _begin;
        }

        // flag = true：默认数据转移
        void reserve(size_t n, bool flag = true) {
            if(n > maxSize()) {

                size_t countVal = this->size();
                T* p = nullptr;

                if (this->_begin != nullptr) { //本来容器有数据
                    // cout << "本来容器有数据!" << endl;
                    p = (T*)realloc(this->_begin, n * sizeof(T));

                    if (p == nullptr) { // realloc扩容失败
                        // cout << "realloc扩容失败!" << endl; 
                        p = (T*)malloc(n * sizeof(T));

                        if (p == nullptr) {
                            cout << "malloc failed!" << endl;
                            return ;
                        }
                    
                        if (flag == true) {
                            // 原来数据转移到新空间上
                            // cout << "原来数据转移到新空间上!" << endl;
                            for (size_t i = 0; i < countVal; ++i) {
                                *(p + i) = *(this->_begin + i);
                            }
                            
                            //释放原本空间 
                            delete[] this->_begin;  
                            this->_begin = nullptr;                     
                        }

                    }
                
                } else { //本来容器没有数据
                    p = (T*)malloc(n * sizeof(T));
                }

                
                this->_begin = p;

                if (flag == true) this->_end = this->_begin + countVal;
                else this->_end = this->_begin;

                this->_capacity = this->_begin + n;
            }
        }

        void swap(master::vector<T>& v) { //万能匹配
            std::swap(this->_begin, v._begin);
            std::swap(this->_end, v._end);
            std::swap(this->_capacity, v._capacity);
        }

    public:
        vector()
        : _begin(nullptr)
        , _end(nullptr)
        , _capacity(nullptr) {

        }

        vector(size_t&& size, const T& value = T{} ) // 调用T的默认构造
        : _begin(nullptr)
        , _end(nullptr)
        , _capacity(nullptr) {
            reserve(size, false);
            while (size--) {
                *_end = value;
                ++_end;
            }
        }

        // 左值拷贝构造
        explicit vector(const master::vector<T>& v)
        : _begin(nullptr)
        , _end(nullptr)
        , _capacity(nullptr) {
            if (this == &v) return ;
            reserve(v.maxSize());
            for (size_t i = 0; i < v.size(); i++) {
                *_end = *(v._begin + i);
                ++_end;
            }
        }

        // 右值拷贝构造
        explicit vector(master::vector<T>&& v) 
        : _begin(nullptr)
        , _end(nullptr)
        , _capacity(nullptr) {
            if (this == &v) return ;
            swap(v);
            v._begin = nullptr;
            v._end = nullptr;
            v._capacity = nullptr;
        }


        master::vector<T>& operator=(const master::vector<T>& v) {
            if (this == &v) return *this;
            reserve(v.maxSize(), false);
            for (size_t i = 0; i < v.size(); i++) {
                *_end = *(v._begin + i);
                ++_end;
            }
            return *this;
        }
        master::vector<T>& operator=(master::vector<T>&& v) {
            if (this == &v) return *this;
            swap(v);
            v._begin = nullptr;
            v._end = nullptr;
            v._capacity = nullptr;
            return *this;
        }   


        virtual ~vector() {
            if (_begin != nullptr) {
                //手动调用元素析构函数
                for (size_t i = 0; i < size(); ++i) {
                    _begin[i].~T();
                }
            }
            _begin = nullptr;
            _end = nullptr;
            _capacity = nullptr;
            //delete _begin;
        }     


    public:

        T& operator[](size_t index) const {
            return *(_begin + index);
        }

        T& back() const {
            return *(_end - 1);
        }


        void push_back(const T& value) {
            if (_end == _capacity) {
                size_t len = maxSize();
                len = (len + 1) << 1;
                reserve(len);
            }
            cout << "push&" << endl;
            *_end = std::move(value); //右值 赋值
            ++_end;
        }

        void push_back(T&& value) {
            if (_end == _capacity) {
                size_t len = size();
                len = (len + 1) << 1;
                reserve(len);
            }
            // cout << "push&&" << endl;
            *_end = std::move(value);
            ++_end;
        }        

        void pop_back() {
            if (!this->empty()) {
                --_end;
            }
        }


        void outVector() const {
            for (size_t i = 0; i < size(); i++) {
                cout << *(_begin + i) << " ";
            }
            cout << "\n";
        }
    };

}


void testString() {
    cout << "string test\n";
    master::vector<std::string> arr1(10, "sd3d23");


    arr1.outVector();
    cout << arr1.size() << endl;
    cout << arr1.maxSize() << endl;


    cout << arr1[1] << endl;

    arr1[3] = "dsvdchfgskjhgskjdhfska";



    // arr1.outVector();

    arr1.pop_back();
    arr1.pop_back();
    arr1.pop_back();

    arr1.outVector();

    cout << arr1.back() << endl;
    arr1.push_back("fdvdfv");
    arr1.outVector();
    return ;
}

void testInt() {
    return ;
}


void testChar() {
    master::vector<char> v(11, '4');
    v.outVector();
    v[2] = '5';
    v[7] = '6';
    v.outVector();
    v.pop_back();
    v.pop_back();
    v.outVector();
    v.push_back('3');
    v.push_back('&');
    v.outVector();    
    return ;
}
int main() {
    testChar();
    return 0;  
}
~~~







### 版本二





~~~c
#include <iostream>
#include <vector>
#include <string.h>
#include <stdlib.h>
#include <string>

using std::cout;
using std::endl;


namespace master{

    template<typename T>
    class vector{
    private:
        T* _begin; //数组的起始地址
        T* _capacity; //数组最后一个元素后面 元素地址
        T* _end;  //插入元素的最后一个元素后面 元素地址

    public:
        class iterator{
        private:
            master::vector<T>* pVector;
            size_t pos;
        public:

            iterator() : pVector(nullptr), pos(0) {
            }

            iterator(master::vector<T>* vector, size_t offset = 0)
            : pVector(vector)
            , pos(offset) {
            }
            bool operator!=(const iterator& it) const {
                if (this->pVector == it.pVector) 
                    return pos != it.pos;
                else return false;
            }

            iterator& operator=(const iterator& it) {
                cout << "++" << endl;
                if (&it == this) return *this;
                pVector = it.pVector;
                pos = it.pos;
                return *this;
            }
            iterator& operator=(const master::vector<T>& v) {
                cout << "------" << endl;
                if (v.begin() == this) return *this;
                *this = v.begin();
                return *this;
            }
            // iterator& operator=(iterator&& it) {
            //     if (&it == this) return *this;
            //     pVector = it.pVector;
            //     pos = it.pos;
            //     it.pos = 0;
            //     it.pVector = nullptr;
            //     return *this;
            // }

            iterator& operator++() {
                ++pos;
                return *this;
            }
            iterator operator++(int) {
                iterator it{pVector, pos};
                pos++;
                return it;
            }

            iterator& operator--() {
                --pos;
                return *this;
            }
            iterator operator--(int) {
                iterator it(pVector, pos);
                --pos;
                return it;
            }

            iterator operator+(size_t offset) {
                return iterator{pVector, pos + offset};
            }
            iterator operator-(size_t offset) {
                return iterator{pVector, pos - offset};
            }

            T& operator*() const {
                return *(pVector->_begin + pos);
            }

            T* operator->() const {
                return (pVector->_begin + pos);
            }          

            T& operator[](size_t offset) const {
                return *(pVector->_begin + offset);
            }  



        };

    public:

        inline iterator begin() const { return iterator{this}; }
        inline iterator end() { return iterator{this, size()}; }
        inline iterator capacity() const { return iterator{this, maxSize()}; }

        // 空返回true，非空返回false
        inline bool empty() { return (this->_end == this->_begin); }

        //获取当前元素个数
        size_t size() const {
            return (this->_end - this->_begin);
        }

        //获取可以容纳元素最大数量
        size_t maxSize() const {
            return _capacity - _begin;
        }

        // flag = true：默认数据转移
        void reserve(size_t n, bool flag = true) {
            if(n > maxSize()) {

                size_t countVal = this->size();
                T* p = nullptr;

                if (this->_begin != nullptr) { //本来容器有数据
                    // cout << "本来容器有数据!" << endl;
                    p = (T*)realloc(this->_begin, n * sizeof(T));

                    if (p == nullptr) { // realloc扩容失败
                        // cout << "realloc扩容失败!" << endl; 
                        p = (T*)malloc(n * sizeof(T));

                        if (p == nullptr) {
                            cout << "malloc failed!" << endl;
                            return ;
                        }
                    
                        if (flag == true) {
                            // 原来数据转移到新空间上
                            // cout << "原来数据转移到新空间上!" << endl;
                            for (size_t i = 0; i < countVal; ++i) {
                                *(p + i) = *(this->_begin + i);
                            }
                            
                            //释放原本空间 
                            delete[] this->_begin;  
                            this->_begin = nullptr;                     
                        }

                    }
                
                } else { //本来容器没有数据
                    p = (T*)malloc(n * sizeof(T));
                }

                
                this->_begin = p;

                if (flag == true) this->_end = this->_begin + countVal;
                else this->_end = this->_begin;

                this->_capacity = this->_begin + n;
            }
        }

        void swap(master::vector<T>& v) { //万能匹配
            std::swap(this->_begin, v._begin);
            std::swap(this->_end, v._end);
            std::swap(this->_capacity, v._capacity);
        }

    public:
        vector()
        : _begin(nullptr)
        , _end(nullptr)
        , _capacity(nullptr) {

        }

        vector(size_t&& size, const T& value = T{} ) // 调用T的默认构造
        : _begin(nullptr)
        , _end(nullptr)
        , _capacity(nullptr) {
            reserve(size, false);
            while (size--) {
                *_end = value;
                ++_end;
            }
        }

        // 左值拷贝构造
        explicit vector(const master::vector<T>& v)
        : _begin(nullptr)
        , _end(nullptr)
        , _capacity(nullptr) {
            if (this == &v) return ;
            reserve(v.maxSize());
            for (size_t i = 0; i < v.size(); i++) {
                *_end = *(v._begin + i);
                ++_end;
            }
        }

        // 右值拷贝构造
        explicit vector(master::vector<T>&& v) 
        : _begin(nullptr)
        , _end(nullptr)
        , _capacity(nullptr) {
            if (this == &v) return ;
            swap(v);
            v._begin = nullptr;
            v._end = nullptr;
            v._capacity = nullptr;
        }


        master::vector<T>& operator=(const master::vector<T>& v) {
            if (this == &v) return *this;
            reserve(v.maxSize(), false);
            for (size_t i = 0; i < v.size(); i++) {
                *_end = *(v._begin + i);
                ++_end;
            }
            return *this;
        }
        master::vector<T>& operator=(master::vector<T>&& v) {
            if (this == &v) return *this;
            swap(v);
            v._begin = nullptr;
            v._end = nullptr;
            v._capacity = nullptr;
            return *this;
        }   


        virtual ~vector() {
            if (_begin != nullptr) {
                //手动调用元素析构函数
                for (size_t i = 0; i < size(); ++i) {
                    _begin[i].~T();
                }
            }
            _begin = nullptr;
            _end = nullptr;
            _capacity = nullptr;
            //delete _begin;
        }     


    public:

        T& operator[](size_t index) const {
            return *(_begin + index);
        }

        T& back() const {
            return *(_end - 1);
        }


        void push_back(const T& value) {
            if (_end == _capacity) {
                size_t len = maxSize();
                len = (len + 1) << 1;
                reserve(len);
            }
            cout << "push&" << endl;
            *_end = std::move(value); //右值 赋值
            ++_end;
        }

        void push_back(T&& value) {
            if (_end == _capacity) {
                size_t len = size();
                len = (len + 1) << 1;
                reserve(len);
            }
            // cout << "push&&" << endl;
            *_end = std::move(value);
            ++_end;
        }        

        void pop_back() {
            if (!this->empty()) {
                --_end;
            }
        }


        void outVector() const {
            for (size_t i = 0; i < size(); i++) {
                cout << *(_begin + i) << " ";
            }
            cout << "\n";
        }
    };

}




struct Point{
    double x, y;
    Point(double x = 0.0, double y = 0.0) : x(x), y(y) {

    }
};


void testClass() {
    master::vector<Point> arr1(10, {1.3, 2.3});
    master::vector<Point>::iterator it = &arr1;
    cout << it->x << endl;
    cout << it[5].x << endl;
    it[5].x = 23.3;
    cout << it[5].x << endl;

}

void testString() {
    cout << "string test\n";
    master::vector<std::string> arr1(10, "sd3d23");

    master::vector<std::string>::iterator it = &arr1;
    for (; it != arr1.end(); it++) {
        cout << *it << endl;
    }

    it = &arr1;


    arr1.outVector();
    cout << arr1.size() << endl;
    cout << arr1.maxSize() << endl;


    cout << arr1[1] << endl;

    arr1[3] = "dsvdchfgskjhgskjdhfska";



    // arr1.outVector();

    arr1.pop_back();
    arr1.pop_back();
    arr1.pop_back();

    arr1.outVector();

    cout << arr1.back() << endl;
    arr1.push_back("fdvdfv");
    arr1.outVector();
    return ;
}

void testInt() {
    return ;
}


void testChar() {
    master::vector<char> v(11, '4');
    v.outVector();
    v[2] = '5';
    v[7] = '6';
    v.outVector();
    v.pop_back();
    v.pop_back();
    v.outVector();
    v.push_back('3');
    v.push_back('&');
    v.outVector();    
    return ;
}
int main() {
    testClass();
    return 0;  
}
~~~

