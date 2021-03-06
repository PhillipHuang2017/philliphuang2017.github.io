# 05.用两个栈实现一个队列


## 5.用两个栈实现一个队列
### 题目描述  
用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。   




### 分析
**队列(Queue)** ：先进先出，队首英文为front，队尾英文为rear;   

**栈(Stack)** ：先进后出，`stack.top()`可以返回栈顶元素的引用，`stack.size()`可以返回栈中元素个数；   

**算法流程** ：   

`push`的时候就往`stack1`中`push`，`pop`的时候如果`stack2`为空，则将`stack1`中的元素依次弹出并放到`stack2`中，然后把`stack1`最底下的元素弹出并返回；如果`stack2`非空则弹出`stack2`栈顶元素并返回。      

#### STL中Queue操作  
queue 和 stack 有一些成员函数相似，但在一些情况下，工作方式有些不同：
- front()：返回 queue 中第一个元素的引用。如果 queue 是常量，就返回一个常引用；如果 queue 为空，返回值是未定义的。
- back()：返回 queue 中最后一个元素的引用。如果 queue 是常量，就返回一个常引用；如果 queue 为空，返回值是未定义的。
- push(const T& obj)：在 queue 的尾部添加一个元素的副本。这是通过调用底层容器的成员函数 push_back() 来完成的。
- push(T&& obj)：以移动的方式在 queue 的尾部添加元素。这是通过调用底层容器的具有右值引用参数的成员函数 push_back() 来完成的。
- pop()：删除 queue 中的第一个元素。size()：返回 queue 中元素的个数。empty()：如果 queue 中没有元素的话，返回 true。
- emplace()：用传给 emplace() 的参数调用 T 的构造函数，在 queue 的尾部生成对象。
- swap(queue<T> &other_q)：将当前 queue 中的元素和参数 queue 中的元素交换。它们需要包含相同类型的元素。也可以调用全局函数模板 swap() 来完成同样的操作。

#### STL中Stack操作  
堆栈操作和其他序列容器相比，stack 是一类存储机制简单、所提供操作较少的容器。下面是 stack 容器可以提供的一套完整操作：
- top()：返回一个栈顶元素的引用，类型为 T&。如果栈为空，返回值未定义。
- push(const T& obj)：可以将对象副本压入栈顶。这是通过调用底层容器的 push_back() 函数完成的。
- push(T&& obj)：以移动对象的方式将对象压入栈顶。这是通过调用底层容器的有右值引用参数的 push_back() 函数完成的。
- pop()：弹出栈顶元素。
- size()：返回栈中元素的个数。
- empty()：在栈中没有元素的情况下返回 true。
- emplace()：用传入的参数调用构造函数，在栈顶生成对象。
- swap(stack<T> & other_stack)：将当前栈中的元素和参数中的元素交换。参数所包含元素的类型必须和当前栈的相同。对于 stack 对象有一个特例化的全局函数 swap() 可以使用。



### c++代码

```c++
class Solution
{
public:
    void push(int node) {
        stack1.push(node);
    }

    int pop() {
        if(stack1.empty() && stack2.empty()) throw 'Queue is empty!';
        
        int front; //用于获取队首元素
        if(!stack2.empty()){
            front = stack2.top();
            stack2.pop();
            return front;
        }
        else{
            for(int num=stack1.size(); num>1; num--){
                stack2.push(stack1.top());
                stack1.pop();
            }
            front = stack1.top();
            stack1.pop();
        }
        return front;
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};

```


