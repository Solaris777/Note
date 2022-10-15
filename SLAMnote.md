# SLAM笔记

## Linux系统操作

### 系统命令行

* `sudo apt-get install xxx`：安装xxx
* `sudo apt-get remove xxx`：卸载xxx（apt在线安装卸载）
* `sudo dpkg -r xxx `：卸载xxx（deb文件格式卸载）
* `cd home`：切换到home目录
* `cd ..`：切换到上级目录
* `touch xxx.cpp`：创建一个cpp文件
* `mkdir xxx/`：创建文件夹
* ` rmdir xxx/`：删除文件夹
* `dir`：查看有哪些文件
* `pwd`：显示当前文件的绝对路径
* `cat 文件名`：查看文件内容
* `vi`：修改文件
* `./xxx.sh`：运行xxx应用程序（脚本文件一般在bin文件夹里）



vim操作

* `:wq`：保存退出
* `dd`：删除所在行
* `cc`：插入字符



### 有关编程的命令行

* `g++ xxx.cpp`：**编译**一个.cpp文件
* `./xxx.xxx`：**执行**编译好的xxx文件
* `cmake .`：调用cmake对所在目录工程进行cmake编译（多加一个就对上一层目录的文件进行编译）
* `make`：用make命令对工程进行编译，生成可执行程序



## **git仓库操作**

* `git init`：初始化仓库*

* `git add`：添加一个文件到暂存区（index）
* `git add .`：将修改加入暂存区*
* `git commit -m " "`：把文件从暂存区推送到仓库（repository）,并添加注释*
* `git rm`：删除文件
* `git status`：查看仓库信息
* `git log`：查看仓库记录
* `git log --pretty=oneline`：简化记录信息
* `git reset --hard 时间线`：回到某个版本
* `git reflog`：查看仓库详细记录
* `git branch`：查看分支
* `git branch 分支名`：添加分支
* `git checkout 分支名`：切换分支（`git checkout -b 分支名`：创建并切换）
* `git branch origin -M 分支名`：更换分支名*
* `git merge`：合并分支
* `git branch -d  分支名`：删除其他分支（-D为强制删除）
* `git remote add origin 仓库地址`：关联远程仓库*
* `git pull origin 分支名`：将代码更新和远程仓库一致*
* `git pull origin main --allow-unrelated-histories `：上一条被拒绝时使用*
* `git push origin main`：推送文件到远程仓库（不用带sudo，第一次在push后面加上-u）*
* `git clone 仓库的SSH`：克隆某个仓库的所有内容（不用带sudo）*
* `git rm 文件名`：删除单个文件（`git rm * -r`删除全部文件）
* `ssh-keygen -t rsa -C 邮箱地址`：获取SSH



#### 使用git流程

* 在github中创建一个新的仓库
* 在需要上传文件的文件夹中初始化仓库，选择Git Bash Here
* `git init`
* 上传文件*
  * 上传某个文件：`git add "文件名"`
  * 上传全部文件：`git add .`

* `git commit -m "注释"`*
* `git branch -M main`
* `git remote add origin git@github.com:用户名/仓库名`
* `git pull origin 分支名`：将代码更新和远程仓库一致*
* `git push -u origin main` （第一次提交）
* `git push origin main`  （以后提交）*

注：带*为重复使用流程



## CMake编译

### 步骤

* 创建CMakeLists.txt，并在里面作声明（源代码文件、库文件等）
* 创建build文件夹，存放中间文件
* 在build文件夹中对上层目录进行cmake ..编译，生成MakeFile文件
* 进行make编译，./执行



### CmakeLists分文件编程

* 声明版本：`cmake_minimum_required( VERSION 2.8 )`
* 声明工程名：`project( xxx )`
* 添加外部头文件：`include_directories( "/usr/local/include/eigen3" )`
* 添加可执行程序：`add_execubale(文件名 源代码文件)`
* 添加库文件：`add_library(库名 库文件)`
* 添加共享库文件：`add_library(库名 SHARED 库文件)`
* 添加注释：`#`

注：SLAM工程中经常修改的地方

* OpenCV的版本为3
* "/usr/include/eigen3"改为"/usr/local/include/eigen3"
* 绝对路径改为相对路径
* `target_link_libraries()`括号最后加上fmt
* `set(CMAKE_CXX_FLAGS "-std=c++11 -O2")`改为`set(CMAKE_CXX_STANDARD 14)`



## C++相关知识

### main函数

`int main(int argc,char **argv)`

* main函数的参数值是从**操作系统命令行上获得的**，在**终端**输入**文件路径+文件名**即可传递参数
* argc指的是**文件个数**，注意：文件本身也算一个参数
* argv即**指向字符串的指针数组**， **argv[0]代表文件本身**，argv[1]代表传入的第一个文件

### STL容器

#### vector类

##### 类对象初始化

- **直接法**：`vector<T> v1`

- 初始化并**赋值**：`vector<T> v2(v1)`（将v1的元素赋值给v2，类型必须相同）

- **列表**初始化：`vector<T> v1={1，2}`：v1的元素为1，2

- **构造对象**初始化：`vector<int> v1(1`

  

  `0,1)`：v1拥有10个相同元素，均为1

注：组成vector的元素也可以是vector，如`vector<vector<int>>`，代表一个vector，里面的元素均是`vector<int>`类



##### 初始化列表

```C++
class foo
{
public:
foo(string s, int i):name(s), id(i){} ; // 初始化列表
private:
string name ;int id ;
};
```

* 写在**构造函数**后面，以冒号开头，以成员名(值)的方式赋值



##### 操作

* **添加**元素：`v.push_back(t)`：向v的尾端添加一个值为t的元素
* **检测**元素：`v.empty()`：没有元素返回真，否则返回假
* **检索**元素：`v.size()`：返回v中元素的个数



### 范围for循环

利用定义的变量遍历数组或者容器其的每一个值

* 定义：`for(declaration : expression) statement`

  * expression:必须是一个序列，即初始值列表、数组、或容器，这些类型都拥有能返回迭代器的begin和end成员

  * declatation:定义一个变量，序列的每个元素都得能转换成该变量的类型，通常用**auto**作为其类型，需要修改序列的内容则加上**引用&**
  * statement:执行语句

* 用例

```C++
vector<int> vec;
 
//下面两者是等价的遍历
for (auto iter = vec.begin(); iter != vec.end(); iter++) 
{ 
    cout << *iter << endl;
}
 
for (auto i : vec) 
{ 
    cout << "i" << endl;
}
```

如果要**修改**vec的内容：

```C++
for (auto& i : vec) 
{ 
    i++;             //增加vec元素中的值
}
```

使用**&**符号后表示它被声明为**引用变量**，成为了**元素本身**的别名而不是元素的副本，因此可以修改



### 谓词与lambda表达式

* 谓词：

  * 定义：返回**bool类型**的仿函数称为谓词，意义为**改变算法的排序方式**

  * 分类：如果operator()接受一个参数，叫做**一元谓词**；如果operator()接受的两个参数，叫做**二元谓词**

  * 案例：一般用于排序算法、查找算法中（sort、find_if），`sort(begin,end,cmp)`中第三个参数即谓词，函数体一般为判断语句

  * ```C++
    bool cmp(int a,int b)
    {
        return a<b;
    }
    ```

* lambda表达式：

  * 定义：`[capture list] (parameter list) -> return type { function body }`，当谓词需要大于两个参数时使用，类似于函数，但其可能定义在函数内部

  * [捕获列表]：即lambda**所在函数中定义的局部变量**的列表

    * 若为空，则不能使用所在函数的变量
    * 若为一个逗号分割的名字列表，则可以使用列表内的变量
    * 若为&，则为隐式捕获列表，采用**引用捕获**方式，捕获函数的实体的引用，**允许改变其大小**
    * 若为=，则为隐式捕获列表，采用**值捕获**方式，捕获函数的实体，**不允许改变其大小**

  * (parameter)：参数列表（可省略）

  * ->：尾置返回（一般不写）

  * return type：返回值类型（一般不写）

  * 案例：

    ```C++
    sort(word.begin(),word.end(),[] (const string &a,const string &b) {return a.size() < b.size()})
    ```




### 数据类型

#### size_t

在32位系统上定义为unsigned int，在64位系统上定义为unsigned long



### 内联函数inline

#### 定义

在c/c++中，为了解决一些**频繁调用的小函数**大量消耗栈空间（**栈内存**）的问题，特别的引入了inline修饰符，表示为内联函数（相当于把函数调用部分直接换成函数体）

#### 使用限制

inline只适合函数体内部代码简单的函数使用，函数体内**不能有循环**，**要在调用和声明前定义**



### 关键字

#### explicit关键字

###### 作用

explicit关键字只能用于修饰**只有一个参数**的类**构造函数**，它的作用是表明该构造函数是**显示**的，而非隐式的。类构造函数默认情况下即声明为implicit（隐式）

###### 例子

```C++
class CxString  // 没有使用explicit关键字的类声明, 即默认为隐式声明  
{  
public:  
    char *_pstr;  
    int _size;  
    CxString(int size)  
    {  
        _size = size;                // string的预设大小  
        _pstr = malloc(size + 1);    // 分配string的内存  
        memset(_pstr, 0, size + 1);  
    }
};

    // 下面是调用:  
  
    CxString string1(24);     // 这样是OK的, 为CxString预分配24字节的大小的内存  
    CxString string2 = 10;    // 这样是OK的, 为CxString预分配10字节的大小的内存  

//加了explicit关键字后第二条调用无效
```



#### static关键字

###### 作用

1. 修饰**全局变量**时，表明该全局变量只对定义在**同一文件中**的函数可见
2. 修饰**局部变量**时，表明该变量的值**不会因为函数终止而丢失**
3. 修饰**函数**时，表明该函数**只在同一文件中调用**
4. 修饰**类的数据成员**时，表明对该类所有对象，这个数据成员都**只有一个实例**。即该实例归所有对象共有
5. 修饰**类的成员函数**，静态成员函数不能引用非静态成员，非静态成员函数可以引用静态成员变量（被static修饰的东西在程序一开始就分配空间了）
6. 详细见https://blog.csdn.net/zhizhengguan/article/details/81183602



#### const关键字

###### 作用

1. 修饰**局部变量**，表明该变量不能再被修改，定义时必须初始化
2. **常量指针（底层const）**：`const int *n或int const *n`即指针指向的**内容**是常量
   * 不能通过指针改变变量的值，但是还是可以通过其他的**引用**改变变量的值
   * 指针指向的值不能改变，但是指针本身可以**指向其他的地址**
3. **指针常量（顶层const）**：`int *const n;`即指针**本身**是个常量，不能再指向其他的地址
   * 指针常量指向的地址不能改变，但是地址中保存的**数值是可以改变的**，可以通过其他指向该地址的指针来修改
   * 区分常量指针和指针常量：看const**在谁的右边**，**谁就是不能改变的**。例如`int *const a`const在*的右边，所以指针（本身）是不能改变的；`const int *a`等价于`int const *a`const在int的右边，所以指针指向的值是不能改变的
4. **指向常量的常指针**：`const int* const p;`即指针指向的位置不能改变并且也不能通过这个指针改变变量的值，但是依然可以通过其他的指针改变变量的值
5. 修饰**全局变量**：全局变量都尽量使用const进行修饰，因为一旦有一个函数改变了全局变量的值，它也会影响到其他引用这个变量的函数，导致除了bug后很难发现
6. 修饰**成员变量**：不能在类定义外部初始化，只能通过构造函数**初始化列表**进行初始化，并且必须有构造函数。（不同类对其const数据成员的值可以不同，所以不能在类中声明时初始化）
7. 修饰**成员函数**：const对象不能调用非const成员函数；非const对象都可以调用

注：

* 执行对象拷贝时有限制，常量的底层const不能赋值给非常量的底层const
* 使用命名的强制类型转换函数const_cast时，只能改变运算对象的底层const



### 文本文件操作

#### 操作文件三大类

* ofstream：写操作
* ifstream：读操作
* fstream：读写操作

#### 文件打开方式

* `ios::in`为读文件而打开文件
* `ios::out`为写文件而打开文件
* `ios::ate`初始位置：文件尾
* `ios::app`追加方式写文件
* `ios::trunc`如果文件存在先删除，再创建

#### 写文件

1. 包含头文件`#include<fstream>`
2. 创建流对象`ofstream ofs;`
3. 打开文件`ofs.open("文件路径",打开方式)`
4. 写入数据`ofs<<"写入的数据"`
5. 关闭文件`ofs.close();`

#### 读文件

1. 包含头文件`#include<fstream>`
2. 创建流对象`ifstream ifs;`
3. 打开文件`ifstream fin(argv[1])`
4. 判断是否打开成功`if(!fin)`
5. 读取文件`while(!fin.eof()) {fin >> name}` （遇到空格停止）
6. 关闭文件`ifs.closea();`



### boost::format的使用

#### 直接输出

类似于printf，它的直接输出格式为`	std::cout << ( boost::format("%s") % "Hello boost!") << std::endl;`，即%后面接字符内容

#### 与string结合使用

```C++
	string s1;
	boost::format fmt("%s");
	fmt % "Hello boost!!";
	s1 = fmt.str();
	std::cout << s1 << std::endl;
```

只需加上`str()`函数将boost::format类型转换为string类型

#### 占位符格式

```C++
 std::cout << (boost::format("%1% %2%") % "Hello" % "boost!!!") << std::endl;  // "Hello"对应占位符%1%，"boost!!!"对应占位符%2%

std::cout << (boost::format("%2% %1% %3%") % "Hello" % "boost" % "!!!") << std::endl;  // "Hello"对应占位符%1%，"boost"对应占位符%2%  "!!!"对应占位符 %3%
//输出boost Hello !!!
```



## 十四讲代码理解

### 三维刚体运动（eigen库、pangolin库）

#### P67 line14

如果STL容器中的元素是Eigen库数据结构，例如这里定义一个vector容器，元素是Matrix4d ，如下所示：

`vector<Eigen::Matrix4d>`

这个编译不会出错，只有在运行的时候出错。解决的方法很简单，定义改成下面的方式：

`vector<Eigen::Matrix4d,Eigen::aligned_allocator<Eigen::Matrix4d>>`;

当vector中元素是Eigen中的类型的时候，必须调用Eigen::aligned_allocator<>分配内存空间，否则编译不会报错，运行会报错 



#### eigen库

注：本书中#include<Eigen/Core>均要改为#include<eigen3/Eigen/Core>

##### **norm**函数：

* 对于**Vector**,norm返回的是向量的**二范数**

* 对于**Matrix**,norm返回的是矩阵的**佛罗贝尼乌斯范数**



##### normalize函数

把自身的各元素除以它的范数，**返回值为void**（自身修改），用于**四元数归一化**

* `Eigen::Vector3d`的两种表达方式：
  * (row,col)表示某行某列，一般用于矩阵
  * [row]表示某行，一般用于列向量



##### normalized函数

类似normalize函数，但返回一个新的Vector



#### pangolin库

* 绘制图像：`DrawTrajectory(vector) `  (自定义函数)
* 终止函数：`assert()`，如果条件返回假，则终止程序执行



#### P63 

##### 各种形式的转换

* 四元数---->旋转向量：     ` v_rotate = AngleAxisd (q)`

* 旋转向量----->四元数：    `q = Quaterniond (v_rotate)`（x,y,z,w）

* 旋转向量----->旋转矩阵： `R = v_rotate.matrix() 或 R = v_rotate.toRotationMatrix()`

* 旋转矩阵----->旋转向量： `v_rotate = AngleAxisd (R)`

* 四元数------>旋转矩阵：    `R = Matrix3d (q)`

* 旋转矩阵----->四元数：     `q = Quaterniond (R)`

* 四元数----->变换矩阵：     `T.rotate(q) / Isometry3d T(q), T.pretranslate(t) `

  ​                                            //（第一个使用前先初始化变换矩阵T  `Isometry3d T = Isometry3d::Identity();`）

* 旋转矩阵----->欧拉角：     ` euler_angle = R.eulerAngles(2,1,0)` //zyx

* 欧拉角------>旋转矩阵： R=AngleAxisd(euler_angle[0],Vector3d::UnitZ())*AngleAxisd(euler_angle[1],Vector3d::UnitY())*AngleAxisd(euler_angle[2],Vector3d::UnitX())

##### 功能函数

* 四元数直接赋值：               `Eigen::Quaterniond(w,x,y,z)`

* 四元数归一化：                   `q.normalize()`
* 欧式变换矩阵求逆：           `T.inverse()`
* 初始化单位阵：                  `Matrix3d::Identity()`
* 转换成矩阵：                      `.matrix()`

* 向量初始化：                     `Vector3d v1(1,2,3)`



### 李群与李代数(Sophus库)

#### P87

* 旋转矩阵构造李群：         `Sophus::SO3d SO3_R(R)`，四元数构造同理
* 李群--->李代数：               `Vector3d so3 = SO3_R.log()`
* 李代数--->李群：               `Sophus::SO3d::exp(so3)`
* 向量--->反对称矩阵：       `Sophus::SO3d::hat(so3)`
* 反对称矩阵--->向量：       `Sophus::SO3d::vee(Sophus::SO3d::hat(so3))`



### 相机与图像（OpenCV库）

#### P109

##### OpenCV库

* `int main(int argc,char **argv)`

argc表示输入参数的个数，argv是指针数组，存放输入的参数（可以是文件）

只需在终端输入文件的位置+文件名便按顺序匹配到argv数组中

* 创建任意维度的**稠密数组**存储图像：`cv:Mat image`

* 读取图像：`cv::imread(图片路径，图片标志)`（图片标志：彩色忽略透明色通道为1，灰度为0，全通道为-1）

* 显示图像：`cv::imshow("标题名",图像)`

* 暂停程序：`cv::waitKey(0)`

* 切割函数：`cv::Rect(int x,int y,int width,int height)`（左上角x坐标  左上角y坐标 矩形的宽 ，矩形的高）

* 关闭窗口：`cv::destroyAllWindows()`

* 像素的数据格式：基本数据类型+通道数

  CV_8UC1：单通道，每个通道是unsigned char类型的数据

  8U->unsigned char ； 16S-> short int ； 16U->unsigned short int ； 32F->float ； 64F->double

* 获取图像信息：`image.cols/image.rows/image.channels()`

* 获取指定数组元素：`image.at<uchar> (行,列)`（灰度图像）；

  `image.at<Vector3d> (行,列)[x]`（RGB图像，x=012对应BGR）

* 判断图像类型：`image.type`
* 深拷贝：`image_clone = image.clone()`

* 浅拷贝：`image_another = image`
* 智能指针：`cv::Ptr<数据类型> 指针名`（指向某种数据类型的指针）
* 用指针指向SGBM算法初始化对象：`cv::Ptr<cv::StereoSGBM> sgbm = cv::StereoSGBM::create(神奇的参数)`
* SGBM算法计算函数：`sgbm->compute(left,right,disparity_sgbm)`（输入左右图像，输出视差图）
* 视差图像素->普通图像像素：`disparity_sgbm.convertTo(输出图像名，输出类型，比例因子)`



关于遍历图像中的像素见下文：

https://blog.csdn.net/qq_41451702/article/details/121978707?spm=1001.2014.3001.5502

注：终端输入图片命令：`可执行文件所在文件夹名称/可执行文件名称 图片1 图片2 ...`



##### chrono库

用于测试一段代码的执行时间

```C++
#include<chrono>
chrono::steady_clock::time_point t1 = chrono::steady_clock::now();

//运行的代码段

chrono::steady_clock::time_point t2 = chrono::steady_clock::now();
chrono::duration<double> time_used = chrono::duration_cast<chrono::duration<double>>(t2-t1);
cout << "solve time cost = " << time_used.count() << "seconds. " << endl;

```



##### 格式化函数（方便查找文件）

* 首先用占位符表示文件路径：`boost::format fmt("../%s/%d.%s")`
* 然后代入具体路径：`fmt % "color" % 1 % "png"`
* 综上表示读取文件的路径为：`../color/1.png`
* 将图片推入容器中：`colorImgs.push_back(cv::imread(fmt.str(),1))`



### 曲线拟合问题（手写高斯牛顿法、Ceres库和g2o库）

#### P134 手写高斯牛顿法

###### 详解代码步骤

1. 定义H,b,迭代次数
2. 计算误差e以及累加和cost
3. 定义雅克比矩阵J，定义增量方程中H和b的更新方式
4. 解出dx，并进行对估计值更新

###### 注解

* 生成一个服从均值为0，标准差为val的高斯分布随机数：`rng.gaussian(val)`
* 判断是否为非法数字函数：`isnan(x)`，方程无解，则x为非法字符nan(not a number)，函数返回0；否则返回1
* 矩阵的转置：`Matrix.transpose()`
* 求解线性方程Hx=b：
  * QR分解：`H.colPivHouseholderQr().solve(b);`
  * cholesky分解：`H.ldlt().solve(b);`



#### P138 Ceres库的使用

* 介绍：

![image-20220904193317925](/home/hzc/Note/image-20220904193317925.png)

* 使用流程
  * **构建代价函数**cost function，其形式为结构体，并**重载()运算符**
  * 通过代价函数**构建待求解的优化问题**
  * **配置求解器参数**并求解问题



贴一波线性拟合的helloworld代码：

```C++
#include <iostream>
#include <ceres/ceres.h>

using namespace std;
using namespace ceres;
//第一部分：构建代价函数f(x)=10-x
struct CostFunctor {
    template <typename T>
    bool operator()(const T* const x, T* residual) const 
    {
        residual[0] = T(10.0) - x[0];
        return true;
    }
};

//主函数
int main(int argc, char** argv) 
{
    google::InitGoogleLogging(argv[0]);

    // 寻优参数x的初始值，为5
    double initial_x = 5.0;
    double x = initial_x;

    // 第二部分：构建寻优问题
    Problem problem;
    CostFunction* cost_function = new AutoDiffCostFunction<CostFunctor, 1, 1>(new CostFunctor); //使用自动求导，将之前的代价函数结构体传入，第一个1是输出维度，即残差的维度，第二个1是输入维度，即待寻优参数x的维度，用代价函数结构体初始化。
    problem.AddResidualBlock(cost_function, NULL, &x); //向问题中添加误差项，本问题比较简单，添加一个就行。

    //第三部分： 配置并运行求解器
    Solver::Options options;
    options.linear_solver_type = ceres::DENSE_QR; //配置增量方程的解法
    options.minimizer_progress_to_stdout = true;//输出到cout
    Solver::Summary summary;//优化结果
    Solve(options, &problem, &summary);//求解!!!

    std::cout << summary.BriefReport() << "\n";//输出优化的简要信息
    //最终结果
    std::cout << "x : " << initial_x
              << " -> " << x << "\n";
    return 0;
}

```

第一部分

* `CostFunctor`结构体：用于重载()运算符，构造某个**参数类型为(向量)指针**，**返回值为bool类型**的代价函数

* ``google::InitGoogleLogging(argv[0]);`固定格式



第二部分

* `ceres::AutoDiffCostFunction<CostFunctor, int residualDim, int paramDim>(CostFunctor* functor);`模板参数依次为仿函数（functor）类型CostFunctor，残差维数residualDim和待优化变量维数paramDim，接受参数类型为仿函数指针CostFunctor *，返回类型为CostFunction *
* `Problem::AddResidualBlock(CostFunction *cost_funtion,LossFunction *loss_function,double *x0,double *x1...)`向Problem类传递残差块信息，参数分别为代价函数，损失函数（可以取NULL），估计参数



第三部分

* `options.linear_solver_type`配置求解器类型，默认为SPARSE_NORMAL_CHOLESKY（cholesky分解），否则为DENSE_QR（QR分解）
* `options.minimizer_progress_to_stdout = true`固定格式
* `options.minimizer_progress_to_stdout`迭代直到输出
* `ceres::Solve(options, &problem, &summary);`求解函数，参数分别为求解器配置options的引用，问题的地址&problem，求解结果的地址&summary



贴部分非线性拟合的helloworld的代码：

```C++
//1.代价函数结构体
//附带x y的构造函数
struct ExponentialResidual {
    ExponentialResidual(double x, double y) : x_(x), y_(y) {}   //初始化成员变量（初始化列表）

    template <typename T> bool operator()(const T* const m, const T* const c, T* residual) const {
        residual[0] = y_ - exp(m[0] * x_ + c[0]);
        return true;
    }

private:
    const double x_;
    const double y_;
};

    //构建寻优问题
    //用for循环将残差项加入到problem中
    //kNumObservations为样本数
    Problem problem;
    for (int i = 0; i < kNumObservations; ++i) 
    {
        problem.AddResidualBlock(new       AutoDiffCostFunction<ExponentialResidual, 1, 1, 1>(new ExponentialResidual(data[2 * i], data[2 * i + 1])),NULL,&m, &c);
    }
    //这里把上面的两步合并成一步

    //配置并运行求解器
    Solver::Options options;
    options.max_num_iterations = 25;
    options.linear_solver_type = ceres::DENSE_QR;
    options.minimizer_progress_to_stdout = true;
```

* `options.max_num_iterations`设定最大迭代次数
* 注：非线性拟合与线性拟合大同小异，注意以下几点：
  * x y作为代表某些样本数的变量，由for循环代入函数表达式中，看作常量
  * 需要设定**最大迭代次数**
  * 函数后面加**const**修饰的原因：防止误操作导致()重载函数内的变量被修改



#### P144 g2o库的使用

![img](https://images2015.cnblogs.com/blog/606958/201603/606958-20160321233900042-681579456.png)

![image-20220907151834661](/home/hzc/Note/image-20220907151834661.png)

![g2o](/home/hzc/Note/g2o.png)

* 详解代码步骤：

1. 定义顶点的类型(继承于g2o::BaseVertex)

   Eigen字对齐

   重写四个虚函数：

   1. `virtual void setToOriginImpl()`顶点重置函数
   2. `virtual void oplusImpl(const double *update)`顶点更新函数，主要用于在使用增量方程计算出增量 Δ x 后，用来更新x (k+1) = x(k) + Δ x; 
   3. `virtual bool read(istream &in) {}`存盘（留空）
   4. `virtual bool write(ostream &out) const {}`读盘（留空）

2. 定义边的类型(继承于g2o::BaseUnaryEdge)（一元边：BaseUnaryEdge,二元边：BaseBinaryEdge,多元边：BaseMultiEdge）

   Eigen字对齐

   构造函数（继承）

   重写四个虚函数：

   1. `virtual void computeError()`计算曲线误差函数
   2. `virtual void linearizeOplus()`计算雅克比矩阵
   3. `virtual bool read(istream &in) {}`存盘（留空）
   4. `virtual bool write(ostream &out) const {}`读盘（留空）

3. 创建数据以及插入数据

4. 构建图优化
   1. 配置块求解器BlockSolver类型`g2o::BlockSolver<g2o::BlockSolverTraits<3, 1>>`，参数分别为优化变量维度，误差维度
   2. 配置线性方程求解器LinearSolver，从PCG,CSparse,Choldmod,Dense中选一个作为求解方法，类型为`g2o::LinearSolverDense<BlockSolverType::PoseMatrixType>`
   3. 配置总求解器solver，并从GN,LM,Dogleg优化算法中选一个，再用上述块求解器BlockSolver初始化
   4. 配置图模型SparseOptimizer，设置求解器`setAlgorithm`，打开调试输出`setVerbose`
   5. 往图中增加顶点
      1. 利用类指针实例化对象
      2. 设置优化变量`setEstimate`
      3. 设置顶点编号`setId`
      4. 向SparseOptimizer中添加顶点
   6. 添加边
      1. 利用类指针实例化对象
      2. 定义边的编号`setId`
      3. 设置连接的顶点`setVertex`
      4. 设置观测数值`setMeasurement`
      5. 设置信息矩阵（协方差矩阵之逆）`setInformation`
      6. 向SparseOptimizer中添加边

5. 启动优化

   1. 设置初始值`initializeOptimization`
   2. 设置迭代次数`optimize`
   3. 输出优化值



* 代码解释：

https://blog.csdn.net/qq_41451702/article/details/122990734?spm=1001.2014.3001.5502



* 额外注解：
  * `EIGEN_MAKE_ALIGEND_OPERATOR_NEW`Eigen的使用时内存对齐
  * `override`关键字，如果派生类在虚函数声明时使用了override描述符，那么该函数必须重载其基类中的同名函数，否则代码将无法通过编译。
  * `static_cast<type-id>(expression)`**static_cast**把expression转换为type-id类型
  * `CurveFittingEdge(double x) : BaseUnaryEdge(), _x(x) {}  `子类的构造函数中同时构造父类的构造函数以及变量初始化（初始化列表）
  * `CurveFittingVertex *v = new CurveFittingVertex();`创建顶点实例，使用指针操作能够减少内存占用
  * `typedef typename std::vector<T>::size_type size_type;`**typedef**作用为创建别名；**typename**作用为告诉编译器std::vector<T>::size_type是一个类型而不是一个成员，并且这里不能用**class**
  * `_estimate`位于顶点的头文件内，用于存储优化变量（顶点）,函数`estimate()`可以返回`_estimate`
  * `_measurement`、`_error`、`_vertices`、`_jacobianOplusXi`位于边的头文件内，分别存储观测值、误差、指向超边连接的顶点的指针向量（连接一个顶点值为0）、雅克比矩阵



### OpenCV提取和匹配ORB特征（OpenCV库）

#### 详解代码步骤

1. 读取图像(imread)
2. 初始化关键点(KeyPoint)，描述子(descriptors)，特征点检测器(FeatureDetector)，描述子提取器(DescriptorExtractor)，描述子匹配器(DescriptorMatcher)--BruteForce-Hamming
3. 检测Oriented FAST角点位置(detect)
4. 计算BRIEF描述子(compute)
5. 输出含角点图像
6. BRIEF描述子匹配(match)
7. 匹配点对筛选
   1. 计算最大距离和最小距离(minmax_lelment排序，first对应最大值，second对应最小值)
   2. 利用for 循环选出distance<=max(2*min,30)的good_matches
8. 绘制匹配结果(drawMatches+imshow+waitKey)



#### 注解

* `assert()`断言函数，括号内值为真则编译通过，反则报错
* `Ptr<>`OpenCV中的智能指针模板类，自动转换成该类型的指针
* 关键点类型为`vector<KeyPoint>`,描述子类型为`Mat`,特征点检测器类型为`Ptr<FeatureDetetor>`,描述子提取器类型为`Ptr < DescriptorExtractor> `,描述子匹配器类型为`Ptr<DescriptorMatcher>`
* `ORB::create()`由于OpenCV中的ORB类是一个纯虚类，无法进行实例化创建对象，因此通过该函数调用
* `DescriptorMatcher::create("BruteForce-Hamming")`初始化描述子匹配器类型
* `drawKeypoints()`输出标记关键点的图像，函数参数分别为：原图，原图关键点，带有关键点的输出图像，后面两个为默认值
* `match()`对BRIEF描述子进行匹配，函数参数分别为：图1的描述子，图2的描述子，装载所有匹配描述子的容器
* `minmax_element()`是返回**指向范围**内最小和最大元素的一对迭代器,它将较小的值作为第一个元素返回(**first表示**)，较大的值作为的第二个元素返回(**second表示**)。
* `drawMatches()`画出所有的匹配结果，函数参数分别为：图1，图1关键点，图2，图2关键点，两张图片的关键点匹配数组，承接图像，默认值



### 2D-2D 对级约束求解相机运动

#### 详解代码步骤

1. 提取特征点+特征点匹配
2. 估计两张图像间运动
   1. 转换匹配点形式(vector< Point2f >)
   2. 计算基础矩阵(FundamentalMat)/本质矩阵(EssentialMat)/单应矩阵(Homography)（特征点共面）,选择method为8点法
   3. 从本质矩阵中恢复R,t(recoverPose)
3. 验证对级约束



#### 注解

* **Mat_**和**Mat**的区别：
  * Mat_ 是**定义时**指定类型，如`Mat_<double> M(20,20);` `M(i,j) = ....`
  * Mat 是**使用时**指定类型，如`M.at<double>(i,j) = ....`（给某行某列的元素赋值）
  
* 通过at读取Mat类的单通道矩阵元素：`a.at<数据类型>(x,y);`

* queryIdx是图1中匹配的关键点的对应编号

* trainIdx是图2中匹配的关键点的对应编号

* 特征点的位置信息存储在keypoints关键点里面，使用`keypoints_1[good_matchers[i].queryIdx].pt` 转换成像素坐标(x,y)

* `pixel2cam`函数作用为将像素坐标转换为归一化坐标，即
  $$
  \frac{X}{Z}=\frac{u-C_x}{f_x}
  \\
  \frac{Y}{Z}=\frac{v-C_y}{f_y}
  $$

* `void triangulatePoints(InputArray projMatr1, 
                         InputArray projMatr2, 
                         InputArray projPoints1, 
                         InputArray projPoints2, 
                         OutputArray points4D)`

  projMatr1：第一个相机位姿（4x3的矩阵）
  projMatr2：第二个相机位姿（4x3的矩阵）
  projPoints1：第一个相机坐标系下的特征点坐标（需要转化为归一化坐标）
  projPoints2：第二个相机坐标系下的特征点坐标

  points4D：输出三角化后的特征点的3D坐标。但需要注意的是，输出的3D坐标是齐次坐标，共四个维度，因此需要将前三个维度除以第四个维度以得到非齐次坐标xyz

* OpenCV `circle()`函数`circle(CvArr* img, CvPoint center, int radius, CvScalar color, int thickness=1, int lineType=8, int shift=0)`

  这个函数其实就是画圆

  img为源图像

  center为画圆的圆心坐标

  radius为圆的半径

  color为设定圆的颜色，CV_RGB(255, 0,0)设置为红色，CV_RGB(255, 255,255)设置为白色，CV_RGB(0, 0,0)设置为黑色 

  thickness 如果是正数，表示组成圆的线条的粗细程度。否则，-1表示圆是否被填充

  line_type 线条的类型。默认是8

  shift 圆心坐标点和半径值的小数点位数

* 齐次坐标->非齐次坐标：对某一行的元素全部除以第n个元素值，并输出(n-1)个元素，例如
  $$
  (x_1,y_1,z_1,1)->(x_1,y_1,z_1)
  $$



### 3D-2D PnP问题求解

#### 使用EPnP求解位姿

###### 详解代码步骤

1. 特征点提取+匹配+筛选
2. `solvePnP`函数求解出旋转向量和平移向量
3. 用`Rodrigues`公式实现旋转向量 ->旋转矩阵

###### 注解

* `ushort d = d1.ptr<unsigned short>(int(keypoints_1[m.queryIdx].pt.y))[int(keypoints_1[m.queryIdx].pt.x)];`其原型为`ushort d = d1.ptr<ushort>(y)[x];`，目的是**指定到d1矩阵的y行x列个像素**

* OpenCV提供的PnP函数：`solvePnP(pts_3d,pts_2d,K,disCoeffs,rvec,tvec,false,flags)`

  `disCoeffs`为相机的**畸变系数**

  `rvec`为输出的**旋转向量**，世界坐标系->相机坐标系

  `tvec`为输出的**平移向量**，世界坐标系->相机坐标系

  `flags`默认使用SOLVEONO_ITERATIVE迭代法

* `Rodrigues(r,R)`旋转向量->旋转矩阵



#### 使用g2o进行BA优化（再用g2o）

###### 详解代码步骤

1. 创建一个BlockSolver（类型命名简化）

   配置块求解器BlockSolver类型，参数分别为优化变量维度，误差维度

2. 创建线性求解器LinearSolver，并用上面定义的BlockSolver初始化（类型命名简化）

   配置线性方程求解器LinearSolver，从PCG,CSparse,Choldmod,Dense中选一个作为求解方法

3. 创建总求解器solver，并从GN/LM/DogLeg中选一个作为迭代策略，再用上述块求解器BlovkSolver初始化

4. 创建图优化的核心：稀疏优化器（SparseOptimizer）

5. 定义图的顶点和边，并添加到SparseOptimizer

   1. 定义顶点的类型(继承于g2o::BaseVertex)

      Eigen字对齐

      重写四个虚函数：

      1. `virtual void setToOriginImpl()`顶点重置函数
      2. `virtual void oplusImpl(const double *update)`顶点更新函数，主要用于在使用增量方程计算出增量 Δ x 后，用来更新x (k+1) = x(k) + Δ x; 
      3. `virtual bool read(istream &in) {}`存盘（留空）
      4. `virtual bool write(ostream &out) const {}`读盘（留空）

   2. 定义边的类型(继承于g2o::BaseUnaryEdge)（一元边：BaseUnaryEdge,二元边：BaseBinaryEdge,多元边：BaseMultiEdge）

      Eigen字对齐

      构造函数（继承）

      重写四个虚函数：

      1. `virtual void computeError()`计算曲线误差函数
      2. `virtual void linearizeOplus()`计算雅克比矩阵
      3. `virtual bool read(istream &in) {}`存盘（留空）
      4. `virtual bool write(ostream &out) const {}`读盘（留空）

   3. 往图中增加顶点

      1. 利用类指针实例化对象
      2. 设置优化变量`setEstimate`
      3. 设置顶点编号`setId`
      4. 向SparseOptimizer中添加顶点

   4. 添加边（x个数据对应for循环次数为x）

      1. 利用类指针实例化对象
      2. 定义边的编号`setId`
      3. 设置连接的顶点`setVertex`
      4. 设置观测数值`setMeasurement`
      5. 设置信息矩阵（协方差矩阵之逆）`setInformation`
      6. 向SparseOptimizer中添加边

6. 设置优化参数，开始执行优化

   1. 设置初始值`initializeOptimization`
   2. 设置求解器`Algorithm`
   3. 设置迭代次数`optimize`
   4. 输出优化值

###### 注解

* 函数传递参数时尽量使用引用
* for循环尽量使用++i，减少内存使用
* 虚函数实现时可在()后面加上`override`
* `typedef vector<Eigen::Vector2d, Eigen::aligned_allocator<Eigen::Vector2d>> VecVector2d;`创建一个元素为Vector2d的容器
* `ushort d = img_d.ptr<unsigned short>(int(keypoint_1[m.queryIdx].pt.y))[(int(keypoint_1[m.queryIdx].pt.x))];`**深度图像**的每一个像素值表示相机与物体的距离，这个距离通常以**毫米**为单位
* `pos_pixel.head<2>()`的作用:提取前两维的值
* `setInformation(Eigen::Matrix2d::Identity())`若待优化变量服从高斯分布则去协方差矩阵之逆，若不服从高斯分布则取待优化变量矩阵的单位阵

###### 遗留问题

* `d/5000.0`怎么理解
* `typedef g2o::LinearSolverDense<BlockSolverType::PoseMatrixType> LinearSolverType;    `为什么尖括号里面是PoseMatrixType



### 3D-3D ICP问题求解

#### 详解代码步骤

1. 分别求质心p1,p2
2. 分别求去质心坐标q1,q2
3. 求矩阵W = q1 * q2^T
4. 对W进行SVD分解求出U和V（使用JacobiSVD函数）

#### 注解

* `JacobiSVD<Eigen::Matrix3d> svd(W,ComputerFullU | ComputerFullV)`

  参数一为进行计算的矩阵，参数二有四个取值：

  `ComputeFullU`:在Jacobisvd中用于表示要计算方阵U,用svd.matrixU()取出

  `ComputeFullV`:在Jacobisvd中用于表示要计算方阵V,用svd.matrixV()取出

* `_jacobianOplusXi.block<3,3>(0,0) = -Eigen::Matrix3d::Identity();`

  `_jacobianOplusXi.block<3, 3>(0, 3) = Sophus::SO3d::hat(xyz_trans);`

  <>内为指定雅克比矩阵的块矩阵的大小；()内表示插入的位置，从0开始

* keypoints包含了特征点的二维坐标信息，若为深度图像则可通过二维坐标

* 类型转换：

  * Mat类型转换为Eigen类型

  ```C++
  Mat K = (Mat_<double>(3, 3) << 520.9, 0, 325.1, 0, 521.0, 249.7, 0, 0, 1);
  Eigen::Matrix3d K_eigen;
  K_eigen<<K.at<double>(0,0),K.at<double>(0,1),K.at<double>(0,2),
           K.at<double>(1,0),K.at<double>(1,1),K.at<double>(1,2),
           K.at<double>(2,0),K.at<double>(2,1),K.at<double>(2,2);
  ```

  * Eigen类型转换为Mat类型

  ```C++
  Eigen::Matrix3d R ;
  Mat R_mat;
  R_mat = (Mat_<double>(3,3)<<
           R(0,0), R(0,1), R(0,2), 
           R(1,0), R(1,1), R(1,2), 
           R(2,0), R(2,1), R(2,2) );
  ```



### LK光流的使用

#### OpenCV光流

###### 详解代码步骤

1. 提取GFTT关键点
2. 存入关键点坐标
3. 调用`cv::calcOpticalFlowPyrLK()`函数
4. 画出带光流的图像
   1. 颜色转换`cv::cvtColor`：灰度->BGR
   2. 调用`cv::circle`和`cv::line`函数标点
5. 显示图像`cv::imshow()`和`cv::waitKey()`

###### 注解

* `cv::Ptr<cv::GFTTDetector> create( int maxCorners=1000, 
                                  double qualityLevel=0.01,
                                  double minDistance=1, 
                                  int blockSize=3, 
                                  bool useHarrisDetector=false, 
                                  double k=0.04 );`

  * maxCorners：检测到的**最大角点数量**；

  * qualityLevel：输出角点的质量等级，取值范围是 [ 0 , 1 ]；如果某个候选点的角点响应值小于（qualityLeve * 最大角点响应值），则该点会被抛弃，相当于判定某候选点为角点的**阈值**；

  * minDistance：两个角点间的**最小距离**，如果某两个角点间的距离小于minDistance，则会被认为是同一个角点；

  * mask：如果有该掩膜，则只计算掩膜内的角点；

  （一般只用设置前三个参数）

  * blockSize：计算角点响应值的邻域大小，默认值为3；如果输入图像的分辨率比较大，可以选择比较大的blockSize；

  * useHarrisDector：布尔类型，如果为true则使用Harris角点检测；默认为false，使用shi-tomas角点检测算法；

  * k：只在使用Harris角点检测时才生效，也就是计算角点响应值时的系数k。

* `cv::calcOpticalFlowPyrLK(img1, img2, pt1, pt2, status, error)`

  * pt1:第一个图像的关键点坐标

  * status:输出**状态向量**（uchar类型），如果找到相应特征的流，则向量的每个元素设置为1，否则设置为0

  * error:输出每个点的**匹配误差**（float类型）

* `void circle(img,center,radius,color,thickness,lineType,shift)`
  * color通常用`cv::Scalar(B,G,R)`函数表示，每个通道取值0~255，例如绿色为cv::Scalar(0,255,0);
  * thickness:圆线条的粗细
  * 后面两个为默认值，`line()`函数调参类似
  
* `cv::cvtColor(img2,img2_single,CV_GRAY2BGR)`图像颜色转换函数
  
  * img2:输入图像
  * img2_single:输出图像
  * CV_GRAY2BGR:转换的标识，表示从灰度空间转换到BGR颜色空间



#### 高斯牛顿法实现光流

###### 详解代码步骤

被**双线性插值法**和**`parallel_for_**函数卡住了

###### 注解

* `resize(size,0)`分配容器内存大小，第二个参数为初始值，`reserve()`设置容器容量大小，初始值为随机数
* cv::Range类用于表示**矩阵的多个连续行或列**，范围从start到end，包括start但不包括end
* `floor()`函数：返回一个小于传入参数的最大整数
* `parallel_for_(Range(0,kp1.size()),
                std::bind::(&OpticalflowTracker::calculateOpticalFlow,&tracker,placeholders::_1));`
  * Range表示范围，即并行计算哪些需要追踪的点
  * bind是一个绑定函数，表示调用OpticalFlowTracker中的calculateOpticalFlow()，即计算光流
  * std::placeholders::_1是占位符，表示传入的参数是tracker.calculateOpticalFlow()的第一个参数



### 直接法

双线性插值法(BilinearInterpolatedValue)

该部分介绍少，算法难，暂时**跳过**



### 用Ceres解决BA问题

#### 详解代码步骤

1. 传入BAL数据集
2. 设置参数块、代价函数（误差项）、损失函数（HuberLoss）、预测值（公式）
3. 配置求解器

#### 注解

* Pc->归一化坐标时要乘以-1
* `double *camera = cameras + camera_block_size * bal_problem.camera_index()[i];`即用指针进行逐个存入数据的操作，camera_block_size为参数块的大小，bal_problem.camera_index()[i]为当前指针的位置



### 用g2o解决BA问题

全局优化：两个顶点分别为**相机位姿**和**路标点**，单元边为误差函数

#### 详解代码步骤

1. 读取BAL数据集并初始化，写入.ply文件
   1. `Normalize()`：归一化
   2. `Perturb(0.1,0.5,0.5)`设置参数
2. 编写SolveBA()
   1. 创建姿态和内参（焦距、畸变参数）结构体以及写入函数
   2. 再用g2o
   3. 将BA优化后的数据写入
3. 再次写入第二个.ply文件

#### 注解

* `g2o::BaseBinaryEdge<int D,typename E,typename VertexXi,typename VertexXj>`继承**二元边**，模板参数分别为二元边维度、二元边类型、连接第一个顶点的类型、连接第二个顶点的类型
* 手动设置边缘化顶点`v.setMarginalized(true)`
* `e->setVertex(0,vertex_camera[bal_Problem.camera_index()[i]]);`把第i个相机的顶点参数传入
* `auto v0 = (VertexCamera* )_vertices[0]`强制转换为顶点类型
* `setMarginalized(true)`手动设置边缘化顶点
* `e->setInformation(Matrix2d::Identity())`注意是协方差**矩阵**之逆，不能用Vector2d



### 位姿图优化

#### g2o原生位姿图

###### 详解代码步骤

1. 用ifstream读取.g2o文件
2. 设定g2o的BlockSolver,LinearSolver,solver,optimizer类型
3. 进行逐个字符判断，存入顶点、边的信息
4. 启动求解器

###### 注解

* g2o默认的顶点`g2o::VertexSE3`，默认的边`g2o::EdgeSE3`
* `v->read(fin)`相当于`v->setEstimate()`，将fin里的信息传入顶点
* 打开方式`g2o_viewer result.g2o`



#### 李代数上的位姿图优化

暂时跳过



### 词袋模型

#### 创建字典

###### 详解代码步骤

1. 存入一组图像
2. 提取特征点并计算描述子
3. 定义字典`DBow3::Vocabulary vocabulary`
4. 字典初始化`vocabulary.create(descriptors)`
5. 保存`vocabulary.save(xxx.yml.gz)`



###### 注解

* `to_string(int val)`函数：将数值转化为字符串，返回对应的字符串

* `detectAndCompute(image,Mat(),keypoints,descriptor)`参数分别为图像、掩膜、关键点、描述子（掩膜通常为Mat()）



#### 相似度计算

###### 直接比较图像

1. 创建字典
2. 创建词袋向量`DBow3::BowVector v1`
3. 计算词袋向量`vocabulary.transform(descriptors[i],v1)`
4. 相似度计算`double score = vocabulary.score(v1,v2)`

###### 比较图像与数据库

1. 创建字典
2. 创建数据库`DBow3::Database db(vocabulary,false,0)`
3. 添加描述子`db.add(descriptors[i])`
4. 定义相似度`DBow3::QueryResults ret`
5. 相似度计算`db.query(descriptors[i],ret,4)`，其中4代表max_result，即相似度最高的四个结果

###### 增加字典规模

类似创建字典



### 建图

#### 单目稠密重建

###### 详解代码步骤

1. 读取图像以及相机外参（读文件流操作）

2. 选取第一张图像作为参考图像（ref）

3. 遍历后面每个图像，进行对极匹配

   1. 参考帧向量（参考帧三维坐标） = 归一化坐标 * 深度值
   2. 以深度均值投影的像素为中心，3σ为半径，取最大/最小深度投影的像素
   3. 极线 = 最大深度投影像素 - 最小深度投影像素
   4. 在极线上以深度均值点为中心，左右各取半极线长度进行搜索，计算NCC，只有当NCC>=0.85时才认为深度均值点像素 = 参考点像素

4. 更新图像

   1. 用三角化计算深度 

      1. 像素坐标->三维坐标，并进行归一化

      2. 分别设归一化后的坐标为x1，x2，则根据方程
         $$
         d_1x_1 = d_2(Rx_2) + t
         $$
         左右两边分别乘以x1，Rx2得
         $$
         \left[\begin{matrix}
         x_1x_1 & -Rx_2x_1\\
         x_1Rx_2 & -Rx_2Rx_2
         \end{matrix}\right]
         \left[\begin{matrix}
         d_1\\
         d_2
         \end{matrix}\right]
         =
         \left[\begin{matrix}
         tx_1\\
         tRx_2
         \end{matrix}\right]
         $$
         求解方程组解得d1，d2
         $$
         p' = \frac{d_1x_1+d_2Rx_2+t}{2}
         $$
         深度值depth = p'.norm();

         参考帧三维坐标 = 归一化坐标 * 深度值

   2. 计算不确定性（见P313）

   3. 高斯融合（将计算出来的μ和σ赋值给深度图中的像素点）

5. 计算误差并保存深度图像

###### 注解

* 位姿文件表示的是**Twc**，即相机坐标系->世界坐标系，其中的参数分别为tx, ty, tz, qx, qy, qz, qw
* 对一个向量进行`Eigen::normalize()`运算，得出该向量的归一化向量（即 **方向**）
* 有两种归一化：
  1. 将三维空间点投影到归一化平面上
  2. 将一个坐标除以自身的二范数（上面的归一化，便于计算）

###### 遗留问题

* 为什么`acos(f_ref.dot(t) / t_norm);`用f_ref而不是用p

#### RGBD稠密地图重建

###### 详解代码步骤

1. 载入色彩图和深度图，载入位姿文件并转换成矩阵形式
2. 定义点以及点云图的格式，并创建对象
3. 像素坐标->世界坐标，坐标+RGB=点
4. 使用统计滤波器去除孤立点
5. 使用体素滤波器进行降采样
6. 保存为.pcd文件`pcl::io::savePCDFileBinary("文件名",点云图)`

###### 注解

* `filter()`函数报错：把C++11改成C++14 or 共享库加上fmt后缀

* 打开方式`pcl_viewer map.pcd`

* 位姿文件（平移向量 四元数） -> 4x4变换矩阵T

  其中四元数先转换成旋转矩阵R，然后构成
  $$
  T=
  \left[\begin{matrix}
  R & t\\
  0^T & 1
  \end{matrix}\right]
  $$
  其中R大小为3x3，t大小为1x3

###### 遗留问题

* 为什么`point[2] = double(d) / depthScale;`

#### 点云转化网格图

一堆函数调用+`terminate called after throwing an instance of 'std::logic_error'
  what():  basic_string::_M_construct null not valid`报错

#### 八叉树地图(octomap)

###### 详解代码步骤

1. 载入色彩图和深度图，载入位姿文件并转换成变换矩阵形式
2. 设置分辨率为0.01`octomap::OcTree tree(0..01)`
3. 将像素坐标转换为世界坐标，并存入点云`octomap::Pointcloud`
4. 将点云存入八叉树地图`tree.insertPointCloud(cloud,t)`参数分别为点云，原点（设为平移向量）
5. 更新中间节点的**占据信息**并写入磁盘`tree.updateInnerOccupancy()`+`tree.writeBinary(文件名.bt)`

###### 注解

* 代码中的`vector<Eigen::Isometry3d> poses`要改为`vector<Eigen::Isometry3d,Eigen::aligned_allocator<Eigen::Isometry3d>> poses`



### 设计SLAM系统