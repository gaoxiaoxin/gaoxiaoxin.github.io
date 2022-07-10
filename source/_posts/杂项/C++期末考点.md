---
title: C++期末考试考点
data: 2020-06-16
categories:
  - [杂项]
---

### 前言

1. 创作动机：希望将自己在准备期末复习时候的知识点进行总结，并且分享给他人。希望与兄弟们一起提升。只总结了笔试的知识点，机试，好像也有。

2. 要求： 本文档因为时间急，任务重，在细节方面可能会有一些问题，如果有问题，希望大家提出来。从而减少大家因为本文档丢分的可能性。
3. 注 ： 本文档，大部分均来源于课本，只是里面穿插了一些作者的思考(很少...)。读百遍，不如手敲一遍。希望大家看完本文档可以有自己的思考，最好对照着课本。
4. 如果有看不懂的，那就是作者太菜，大家可以问问身边的大佬。
5. 寄语 ： 希望大家在今天的 c++考试中旗开得胜，成功上岸。

### 单项选择题

1. 类 ： 是面向对象程序设计的最基本的概念，是 c++最强力的特征，是进行封装和数据隐藏的工具，他将数据与操作紧密的结合起来

2. 构造函数/析构函数 ： 构造函数 => 特殊的成员函数，主要用于为对象分配空间，进行初始化

   析构函数 : => 函数销毁时使用，可以在里面清除已经申请的空间。

3. 重载函数 ： 静态连编 ， 通过参数的不同，来调用函数名字相同的函数
4. 参数的默认值： 在函数自定义的时候，为函数的形参赋值，如果在函数调用的时候没有传入该函数，则将形参所对应的值赋值给形参 ；

5. this 指针： 对象 p->属性 p；

6. 引用 : & , `const`

7. 友元函数: 既可以是不属于任何类的非成员函数，也可以是另外一个类的成员函数。不是当前类的成员函数，而是独立于当前类的外部函数 ！！! 只是在这个类内定义一下，friend 就相当于一个钥匙 。

   - 友元函数在类外声明的时候不需要加 域运算符：：
   - 友元函数不是类的成员，所以不能直接访问对象的数据成员，也不能通过 this 指针来访问，必须通过作为入口参数传递进来的对象名来访问引用该对象的数据成员 ， `friend void disp (Gril &x){ cout << x.name << }`

8. 抽象类 ： 至少含有一个纯虚函数的类，抽象类因为里面有纯虚函数，所以不能创建对象，但可以声明指向类的指针变量，此指针可以指向它的派生类，进而实现多态性。

9. 纯虚函数 ：在基类说明的虚函数，在基类里面没有定义，但是要求在它的派生类中根据需要对它进行定义，

   纯虚函数声明的一般形式 ：

   ```c++
   virtual 函数类型 函数名 (参数表) = 0 ;
   ```

   此格式与一般的虚函数定义格式基本相同，只是在后面多了 “=0” 。 声明为纯虚函数之后，不在给出函数的具体实现。

   - 纯虚函数作用是在基类里面为派生类保留一个函数的名字。
   - 纯虚函数不能被调用

10. 流输出数据格式化控制 ：

    这个由于，本人也没看。。。所以大家看看书吧。在==书 295==

### 阅读程序写结果

1. - 类： 是面向对象程序设计的最基本的概念，是 c++最强力的特征，是进行封装和数据隐藏的工具，他将数据与操作紧密的结合起来；

   - `const`成员函数 ： ==成员函数后==加`const`后我们称为这个函数为**常函数**

:::
类型说明符(void / int ) 函数名(参数表) const ;
:::

     1. `const ` 是函数类型的一个组成部分 ，因此在声明函数和定义函数时都要有关键字`const`在调用的时候不必加 `const`；
     2. 常函数内不可以修改成员属性;

- `const`对象 ： ==对象前==加`const`称该对象为常对象

  :::const 类名 对象名[参数表]；或者
  :::类名 const 对象名[参数表]

  1.  常对象在定义对象的时候必须初始化，而且不能被更新 ；
  2.  常对象只能调用常成员函数, 不能调用普通的成员函数 ；

- 常成员函数和普通成员函数的访问特性比较

  |     数据成员     | 普通成员函数             | 常成员函数               |
  | :--------------: | ------------------------ | ------------------------ |
  |   普通数据成员   | 可以访问，也可以改变值   | 可以访问，但不可以改变值 |
  |    常数据成员    | 可以访问，但不可以改变值 | 可以访问，但不可以改变值 |
  | 常对象的数据成员 | 不允许访问和改变值       | 可以访问，但不可以改变值 |

2. 基类、派生类对象的构造和析构过程

   1. 基类、派生类对象的构造和析构的执行顺序

      通常情况下 ， 当创建派生类对象时，首先执行基类的构造函数，然后执行派生类的构造函数；当撤销派生类对象时，则先执行派生类的析构函数，然后再执行基类的析构函数。

```c++ 构造函数 mark:1,6-7
      #include<iostream>
      using namespace std;
      class Base{
      public :
      	Base(){
      		cout << "Base 的 构造函数已经启动" << endl;
      	}
      	~Base(){
      		cout << "Base 的 析构函数已经启动" << endl;
      	}
      };
      class Derived : public Base
      {
      public :
        	Derived(){
       		cout << "Derived 的 构造函数已经启动" << endl;
       	}
       	~Derived(){
       		cout << "Derived 的 析构函数已经启动" << endl;
       	}
       };
      int main() {
      	Derived d1;
      	return 0;
      }
```

      结果为 ：

      ![image-20210615224616177](https://tva3.sinaimg.cn/large/006xVoHRly8grjsz6w8xrj30dn03wmxf.jpg)

2.  派生类构造函数和析构函数的构造规则

    1. 派生类不能继承基类中的构造函数和析构函数， 当基类含有带参数的构造函数时，派生类必须定义构造函数，以提供把参数传递给基类构造函数的途径。

       ```
       //格式为：
       派生类名(参数总表):基类名(参数表)
       {
       	派生类新增数据成员的初始化语句
       }
       ```

       - 其中基类的构造函数的参数，通常来源于==派生类的构造函数的参数总表==，==也可以用常数值==；

       - 当基类函数中，使用了默认的构造函数，或者使用了没有参数的构造函数，那么派生类中的构造函数就不需要加 `:基类名(参数表)`；

       - 但是如果基类函数中，构造函数中哪怕带有一个参数，那么派生类中的构造函数也必须在 该派生类的构造函数中定义该参数，并且向基类构造函数传递过去！！！

         ```c++
         class Base{
         public :
         	Base(int n)
         	{
         		i = n;
         	}
         	void showi()
         	{
         		cout << "i = " << i << endl;
         	}
         private :
         	int i ;
         }
         class Derived : public Base {
         	public :
             // 构造函数参数表中只有基类的一个参数，
             // 但也必须有！！！
         		Derived (int n) : Base (n)
         		{   }
         }
         ```

3.  含有对象成员（子对象）的派生类的构造函数

    当派生类中含有内嵌的对象成员（子对象）时，其构造函数形式为:

    ```
    派生类名（参数总表）: 基类名(参数表 0)， 对象成员名1(参数表 1)， ······，对象成员名n (参数表 n)
    {
    	派生类新增数据成员的初始化语句
    }
    ```

    1. 在定义派生类的构造函数时，构造函数的执行顺序如下 ：

       1.基类构造，对基类数据成员初始化 ---》 2. 调用内嵌对象成员的构造函数，进行初始化 --- 》 3. 执行派生类的构造函数进行初始化。

    2. 析构函数执行顺序与构造函数的执行顺序相反。

       ```c++
       #include<iostream>
       using namespace std;
       class Base{
       public :
       	Base(int i)
       	{
       		x = i;
       		cout << "Base 的 构造函数已经启动" << endl;
       	}
       	~Base(){
       		cout << "Base 的 析构函数已经启动" << endl;
       	}
       	void show(){
       		cout << "x = " << x << endl;
       	}
       private :
       	int x;
       };
       class Derived : public Base
       {
       public :
         	Derived(int i) : Base (i) , b1(i)
       	{
        		cout << "Derived 的 构造函数已经启动" << endl;
        	}
        	~Derived(){
        		cout << "Derived 的 析构函数已经启动" << endl;
        	}
       private :
       	Base b1;
        };
       int main() {
       	Derived d1(10);
       	d1.show();
       	return 0;
       }
       ```

       结果为：

       ![image-20210615232656325](https://tva4.sinaimg.cn/large/006xVoHRly8grjt2gi8mcj30d406ut9c.jpg)

    ​

4.  虚函数的基本调用方法，基类对象的指针如何动态确定对象，从而调用相应的虚函数。

    ==最好看一下课本 236~238==

    虚函数 ： 在基类中被关键字 virtual 说明 ，并在派生类中重新定义的函数。

    ​ ==作用==：允许在派生类中重新定义与基类同名的函数 ，并且可以通过基类指针或引用来访问基类和派生类中的同名函数。

    ​ ==注==：1.在派生类中重新定义时，重新定义的虚函数必须和基类里面的一模一样！！!==理解不了去看课本 239==

    ​ 2.虚函数不能是友元函数和静态成员函数。

    ==利用动态联编，实现动态的多态性。==

    1.  虚函数的基本调用方法： 见上面作用 ； == 》 ==通过基类指针或引用来访问基类和派生类中的同名函数。== 这个就是。

        详细如下：

        ```c++
        #include<iostream>
        using namespace std;
        class B0
        {
           public:
           	 virtual void print (char * p)
           	 {
           	 	cout << p<< "print()" << endl;
        	 }
        };
        class B1 : public B0
        {
           public :
           	 virtual void print (char * p)
        	  {
        		cout << p<< "print()" << endl;
              }
        };
        class B2 : public B1
        {
           public :
           	 virtual void print (char * p)
        	  {
        		cout << p<< "print()" << endl;
              }
        };
        int main() {
        	B0 ob0 ,* op;
        	op = & ob0;
        	op -> print("B0::");
        	B1 ob1 ;
        	op = & ob1;
        	op -> print("B1::");
        	B2 ob2 ;
        	op = & ob2;
        	op -> print("B2::");
        	return 0;
        }
        ```

        ![image-20210615235443587](https://tva3.sinaimg.cn/large/006xVoHRly8grjt3b37n8j306y048glh.jpg)

        不推荐使用对象名和点运算符的方式调用虚函数，这种是静态联编，没有利用虚函数的特性。只用通过基类指针访问虚函数时才能获得运行时的多态性。

    2.  基类对象的指针如何动态确定对象，从而调用相应的虚函数 ：

        通过指针所指的派生类的对象来决定对象。参考上面代码

5.  构造函数重载（有参、无参、拷贝构造）后，调用哪个？

    静态联编实现的多态，通过参数的个数，种类不同而决定调用那个构造函数。

6.  虚基类构造函数调用顺序 ？

    1.  使用虚基类的原因以及用处：

        假设有一个基类 Base ， 和从基类中派生出的两个派生类 Base1 , Base2 ,但现在又从类 Base1 和 Base2 中派生出一个类 Derived ;

        ![image-20210616000607143](https://tva1.sinaimg.cn/large/006xVoHRly8grjt5e95kpj30i309f3yh.jpg)

        那么在 Derived 中会保留两份 Base 上的成员 。如果在 Derived 中调用 Base 上的成员，就会出现二义性，为避免二义性，所以引入虚函数。==详情请看课本 170~172==

    2.  虚基类在派生类中声明，其语法为 ：

        ```
        class 派生类名 ： virtual 继承方式 基类名{
         ······
        }
        ```

    3.  现在开始就是题目的解答了 ：：：

        1.  ==！！！== 建立一个对象时，如果这个对象中含有从虚基类继承来的成员，则虚基类的成员是由==最远== 派生类的构造函数通过调用虚基类的构造函数进行初始化，忽略其他派生类的构造函数对虚基类的构造函数的调用。

        2.  ```c++
            #include<iostream>
            using namespace std;
            class Base{
            public :
            	Base(int sa)
            	{
            		a = sa;
            		cout << "Base 的 构造函数已经启动" << endl;
            	}
            	~Base(){
            		cout << "Base 的 析构函数已经启动" << endl;
            	}
            	void show(){
            		cout << "a = " << a << endl;
            	}
            private :
            	int a;
            };
            class Base1 : virtual public Base
            {
            public :
            	Base1(int sa,int sb) : Base(sa)
            	{
            		b = sb;
            		cout << "Base1 的 构造函数已经启动" << endl;
            	}
            	~Base1(){
            		cout << "Base1 的 析构函数已经启动" << endl;
            	}
            	void show(){
            		cout << "b = " << b << endl;
            	}
            private :
            	int b;
            };
            class Base2 : virtual public Base
            {
            public :
            	Base2 (int sa,int sc) : Base(sa)
            	{
            		c = sc;
            		cout << "Base2 的 构造函数已经启动" << endl;
            	}
            	~Base2(){
            		cout << "Base2 的 析构函数已经启动" << endl;
            	}
            	void show(){
            		cout << "c = " << c << endl;
            	}
            private :
            	int c;
            };
            class Derived : public Base1,public Base2
            {
            public :
              	Derived(int sa,int sb,int sc,int sd) : Base (sa), Base1(sa, sb), Base2(sa, sc)
            	{
            		d = sd;
             		cout << "Derived 的 构造函数已经启动" << endl;
             	}
             	~Derived(){
             		cout << "Derived 的 析构函数已经启动" << endl;
             	}
            private :
            	int d;
             };
            int main() {
            	Derived d1(2, 4, 6, 8);
            	return 0;
            }
            ```

            ![image-20210616003120129](https://tva1.sinaimg.cn/large/006xVoHRly8grjt3bpa15j30c90743zd.jpg)

7.  对象成员、基类构造析构调用

    掌握构造和析构的基本用法。

### 程序填空题，并写出程序的运行结果

1.  类内定义/类外定义成员函数

    1. 类内定义，没啥说的，就是在自己的类里面定义一个函数
    2. 类外定义，就是在类里面进行函数的声明，在类外面进行函数的定义，为了防止函数的定义不知道自己姓啥，在类的定义前面挂了一个 姓 => 类名。

    ==详情请看课本 53==

    ```
    // 1. 类内定义
    class Base{
    public :
    	Base(int sa)
    	{
    		a = sa;
    		cout << "Base 的 构造函数已经启动" << endl;
    	}
    	~Base(){
    		cout << "Base 的 析构函数已经启动" << endl;
    	}
    	void show(){
    		cout << "a = " << a << endl;
    	}
    private :
    	int a;
    };
    // 其中void show 就是类内定义。
    // 2. 类外定义
    class Base{
    public :
    	Base(int sa)
    	{
    		a = sa;
    		cout << "Base 的 构造函数已经启动" << endl;
    	}
    	~Base(){
    		cout << "Base 的 析构函数已经启动" << endl;
    	}
    	void show();
    private :
    	int a;
    };
    void Base::show() {
    	cout << "a = " << a <<endl;
    }
    // 其中void show 就是类外定义。
    ```

2.  静态数据成员的基本用法、调用，特别是书上例子。 ==课本 96==

    1. 联想 c 语言中的静态数据类型 ，c 语言中的静态数据类型是在函数中定义，在每次函数销毁的时候进行保留，自身不被销毁，从而将上次调用函数得到的数据传递给下一个该函数。 那么 c++ 中的静态成员函数有着异曲同工之妙，只不过是将 c 语言中的 函数 扩展到了 c++ 中的 类 ，函数的多次调用扩展到了，该类多个对象对该数据的调用。 ==从而在 c++中实现同一个类中的多个对象对指定数据的共享==

    2. 使用方法：

       ```
       static 数据类型 数据成员名；
       ```

    3. 注意事项：

       1. 静态数据成员的初始化与普通成员不同，静态成员初始化应在类外单独进行，而且应在定义对象之前进行，一般在主函数 main 之前 。
       2. 静态数据成员属于类，而不像普通数据成员那样属于某一对象，因此可以使用 ==”类名::“==,访问静态的数据成员。 =》 类名:: 静态数据成员名

    4. 既然都说要看书上的栗子，那必然需要看一眼，==课本 98==

       ！！！ 课本上栗子的一个小 bug , 在==dev c++==中写 `strlen,strcpy` 需要引入头文件 `string.h` , 课本上引入的是 `string`; 具体原因在这里不做说明。

       ```c++
       #include<iostream>
       #include<string.h>
       using namespace std;
       class Student
       {
       public :
       	Student(char * name1, char * stu_no1, float score1); //
       	~Student();
       	void show();
       	void show_count_sum_ave();
       private :
       	char * name;
       	char * stu_no;
       	float score ;
       	static int count ;
       	static float sum ;
       	static float ave ;
       };
       Student::Student(char * name1, char * stu_no1, float score1)
       {
       	name = new char[strlen(name1) + 1];
       	strcpy(name,name1);
       	stu_no = new char[strlen(stu_no1) + 1];
       	strcpy(stu_no,stu_no1);
       	score = score1;
       	++count;
       	sum = sum + score;
       	ave = sum / count;
       }
       Student::~Student()
       {
       	delete []name;
       	delete []stu_no;
       	--count;
       	sum = sum - score;
       }
       void Student::show()
       {
       	cout << "\n姓名: " << name;
       	cout << "\n学号: " << stu_no;
       	cout << "\n成绩: " << score;
       }
       void Student::show_count_sum_ave()
       {
       	cout << "\n学生人数: " << count ;
       	cout << "\n平时成绩: " << ave;
       }
       int Student::count= 0 ;     // 静态数据成员初始化
       float Student::sum = 0.0 ;
       float Student::ave = 0.0 ;
       int main() {
       	Student stu1("李明", "08150201", 90);
       	stu1.show();
       	stu1.show_count_sum_ave();
       	cout << "\n----------------\n";
       	Student stu2("张大伟", "08150202", 80);
       	stu2.show();
       	stu2.show_count_sum_ave();
       	return 0;
       }
       ```

       ![image-20210616013533331](https://tva2.sinaimg.cn/large/006xVoHRly8grjt84ml43j30hy0b5q3h.jpg)

3.  构造函数初始化成员列表，初始化成员变量和成员对象；

    1. 构造函数初始化成员列表：

       先看一个使用赋值语句初始化数据成员的栗子 ：

       ```c++
       class Base
       {
       public :
       	Base (int a1, int b1){
       		a = a1;
       		b = b1;
               cout << "a = " << a << " b = " << b ;
       	}
       private :
       	int a;
       	int b;
       };
       ```

       其实使用成员初始化列表对数据成员初始化 就是 使用赋值语句初始化数据成员的==变式==；

       将 等号赋值语句 用 ：(冒号) 拉出来，然后还是一一对应。

       下面看一个使用成员初始化列表对数据成员初始化的栗子：

       ```c++
       class Base
       {
       public :
       	Base (int a1, int b1):a(a1),b(b1)
       	{
       		cout << "a = " << a << " b = " << b ;
       	}
       private :
       	int a;
       	int b;
       };
       ```

    2. 初始化成员变量和成员对象

       可能就是等号赋值？？？ ， 理解不了。。。，是我语文不好了。。。

4.  引用参数传递

    1. 建立引用，实质上是为变量另外起一个别名，其实也就是相当于一个人有了大名，然后又起了小名；小名就是他的别名。他还是他，就是多了个名字。对别名引用的数据进行修改，就是对变量本身进行修改；

       举个栗子 ： 有个人，他大名叫张三，小名叫小明，那么有人给张三 100 元，和有人给小明 100 元是一样的。都是张三得到了 100 元。

    2. 格式如下：

       ```
       类型 & 引用名 = 已定义的变量名
       ```

    3. 注意事项 ：

       1. 引用名在声明引用时，必须立即对它初始化，不能声明后再赋值。

          ```c++
          // 错误示范
          int i ;
          int &j ;
          j = i;
          // 正确示范
          int i ;
          int &j = i;
          ```

       2. 别名也可以有别名；

    4. 下面先看一个栗子：

       ```c++
       #include<iostream>
       using namespace std;
       void swap(int &m, int &n){
       	int temp ;
       	temp = m ;
       	m = n ;
       	n = temp;
       }
       int main()
       {
       	int a = 5,b = 10;
       	cout << "a = " << a <<" b = "<< b << endl;
       	swap(a,b);
       	cout << "a = " << a <<" b = "<< b << endl;
       	return 0;
       }
       ```

       在这个程序中，我们可以看到，a, b 是两个基本数据类型 ，在他们被当成参数传入 swap 中时，他们的别名分别是 m, n; 并且通过对别名进行操作，交换了二者的顺序。

       这里说说我的理解：张三（a) ,在外面（main）人们(其他数据）与他发生事件，是与张三发生事件，而在 家里（swap）中，家人都会亲切的呼喊他的小名==小明==，从而与小明发生事件。那么张三和小明转换的关键在哪里？？？分析一下，就可以看出是因为在 swap 中为 a(张三) 起了别名 。

       所以我说这些废话的意义在哪里呢？？？这里是要提醒你，起别名是在调用该数据的函数的参数中起！！！==注意参考注意事项第一条== ，这里是符合第一条的！！！

5.  基类对象、派生类对象、虚函数的基本用法，通过基类对象的指针调用虚函数；

    上面有讲到。

6.  运算符重载：双目运算符、一元运算符（++前置、后置）

    1.  运算符重载：

        c++为运算符重载提供了一种方法，即在进行运算符重载时，定义一个运算符重载函数 operator ， 后面跟一个要重载的运算符

        例：

        |    函数     | 功能 |
        | :---------: | :--: |
        | operator +  |  加  |
        | operator -  |  减  |
        | operator \* |  乘  |
        | operator <  | 小于 |

        1. 一言不合举栗子

        ```c++
        // 重载 + ，实现对两个对象的加法操作
        #include<iostream>
        using namespace std;
        class Base
        {
        public :
        	Base (int a1 = 0, int b1 = 0){
        		a = a1;
        		b = b1;
        	}
        	~Base()	{
        	};
        	int a;
        	int b;
        };
        // 一般不直接引入对象，一般引入对象的别名，并且在不打算改变对象数据时，加上const，防止不下心改变对象数据。
        Base operator + (const Base &base1, const Base &base2){
        	Base base3;
        	base3.a = base1.a + base2.a;
        	base3.b = base1.b + base2.b;
        	return base3;
        }
        int main()
        {
        	Base b1(2,4), b2(1,3), b3;
        	b3 = b1 + b2; // 等价于b3 = operator+ (b1,b2);
        	cout << "b3.a = " << b3.a << "  b3.b = " << b3.b << endl;
        	return 0;
        }
        ```

        结果为： b3.a = 3 b3.b = 7

        2. 但是对上面的栗子进行反思，我们为了能在类外引用 a 和 b ,而将 a,b 两个对象数据暴露为公有数据类型。这不符合 c++ 中封装的理念！！！所以想办法更改，那么在 Base 中 为运算符重载函数加上友元标签，将 a,b 设为私有，则有：

        ```c++
        #include<iostream>
        using namespace std;
        class Base
        {
        public :
        	Base (int a1 = 0, int b1 = 0){
        		a = a1;
        		b = b1;
        	}
        	~Base()	{
        	};
        	void show(){
        		cout << " a = " << a << "   b = " << b << endl;
        	}
        	friend Base operator + (const Base &, const Base &);
        private :
        	int a;
        	int b;
        };
        Base operator + (const Base &base1, const Base &base2){
        	Base base3;
        	base3.a = base1.a + base2.a;
        	base3.b = base1.b + base2.b;
        	return base3;
        }
        int main()
        {
        	Base b1(2,4), b2(1,3), b3;
        	b3 = b1 + b2;
        	cout << "b3: " ;
        	b3.show();
        	return 0;
        }
        ```

        这样就满足了 封装 。

    2.  双目运算符：

        1. 类外友元函数重载 ：有些头大，书上也没讲啥，上面那个就是一个双目运算符重载；

        没啥说的，给你们看看书上的代码吧

        ```
        见书204页，哈哈，去看书吧，我估计我打上去，就是我更加熟练了。。。
        其实，仔细看完书上的代码，你就会发现，它就是用我上面的方法将四个运算符实现了重载，并且堆到了一起。。。
        ```

        2.  类内成员运算符重载 ：

            其实和上面 类外友元函数重载 区别不是太大，就是第三人称转换成了第一人称 。 不会有人不理解吧！！！不理解扣我，我给讲哈。

            在上面类外友元函数重载中，是将 随机俩个对象，进行运算。而在 类内成员运算符重载中 ，其实就是将两个对象中的一个替换为我（调用该方法的对象），另外一个随机。

            1.  格式：

                      ```

                // 类内
                class X {
                函数类型 operator 运算符 (形参表){
                函数体
                }
                }
                // 类外
                class X {
                函数类型 operator 运算符 (形参表);
                }
                函数类型 X::operator 运算符 (形参表){
                函数体
                }
                ```

            2.  简单实现一下：

        ```c++
        #include<iostream>
        using namespace std;
        class Base
        {
        public :
        	Base (int a1 = 0, int b1 = 0){
        		a = a1;
        		b = b1;
        	}
        	~Base()	{
        	};
        	void show(){
        		cout << " a = " << a << "   b = " << b << endl;
        	}
        	Base operator + (const Base &base1)
        	{
        	Base base2;
        	base2.a = base1.a + a;
        	base2.b = base1.b + b;
        	return base2;
        	}
        private :
        	int a;
        	int b;
        };
        int main()
        {
        	Base b1(2,4), b2(1,3), b3;
        	b3 = b1 + b2;
        	cout << "b3: " ;
        	b3.show();
        	return 0;
        }
        ```

    3.  一元（单目）运算符（++前置、后置）==课本 216，不妨一看==

        1. 在 c++中通过在运算符函数参数表中是否插入关键字 int 来区分前缀++，--和后缀 ++ ， -- ；

           例如 ： 前缀 `ob.operator++()`

           ​ 后缀`ob.operator++(int)`

        2. 在使用函数类型调用时，将后缀中的`int`替换为 0;

        3. 举个栗子：代码实现：

           ```c++
           // 1. 使用成员函数重载

           #include<iostream>
           using namespace std;
           class Three {
           public :
           	Three (int i1 = 0, int i2 = 0, int i3 = 0);
           	void show();
           	Three operator --();
           	Three operator --(int);
           private :
           	int i1, i2, i3;
           };
           Three::Three (int i1 ,int i2, int i3)
           {
           	// this-> 主要是用来区分对象成员与参数
           	this->i1 = i1;
           	this->i2 = i2;
           	this->i3 = i3;
           }
           void Three::show()
           {
           	cout << "i1 = " << i1 << "  i2 = " << i2 << "  i3 = " << i3 << endl;
           }
           Three Three::operator --()
           {
           	--i1;
           	--i2;
           	--i3;
           	return *this ;
           }
           Three Three::operator --(int)
           {
           	Three temp(*this);
           	i1--;
           	i2--;
           	i3--;
           	return temp ;
           }
           int main()
           {
           	Three obj1(4, 5, 6), obj2;
           	obj1.show();
           	--obj1;
           	obj1.show();
           	obj2 = obj1 --;
           	obj2.show();
           	obj1.show();
           	return 0;
           }
           ```

           ![image-20210616043016354](https://tva4.sinaimg.cn/large/006xVoHRly8grjt3co5asj30bh05baa4.jpg)

           ++ 就是将重载运算符中的 -- 换成 ++；

           ```c++
           // 2. 友元函数重载
           #include<iostream>
           using namespace std;
           class Three {
           public :
           	Three (int i1 = 0, int i2 = 0, int i3 = 0);
           	void show();
           	friend Three operator ++(Three &);
           	friend Three operator ++(Three &, int);
           private :
           	int i1, i2, i3;
           };
           Three::Three (int i1 ,int i2, int i3)
           {
           	// this-> 主要是用来区分对象成员与参数
           	this->i1 = i1;
           	this->i2 = i2;
           	this->i3 = i3;
           }
           void Three::show()
           {
           	cout << "i1 = " << i1 << "  i2 = " << i2 << "  i3 = " << i3 << endl;
           }
           Three operator ++(Three &three)
           {
           	++three.i1;
           	++three.i2;
           	++three.i3;
           	return three ;
           }
           Three operator ++(Three & three, int)
           {
           	three.i1++;
           	three.i2++;
           	three.i3++;
           	return three ;
           }
           int main()
           {
           	Three obj1(4, 5, 6), obj2;
           	obj1.show();
           	++obj1;
           	obj1.show();
           	obj1++;
           	obj1.show();
           	return 0;
           }
           ```

           ![image-20210616044005752](https://tva4.sinaimg.cn/large/006xVoHRly8grjt3e2gz2j30a103l0sm.jpg)
