# SLAM笔记

## Linux系统操作

### 系统命令行

* `sudo apt-get install xxx`：安装xxx
* `cd home`：切换到home目录
* `cd ..`：切换到上级目录
* `touch xxx.cpp`：创建一个cpp文件
* `mkdir xxx/`：创建文件夹
* ` rmdir xxx/`：删除文件夹
* `dir`：查看有哪些文件
* `pwd`：显示当前文件的绝对路径
* `cat 文件名`：查看文件内容
* `vi`：修改文件



系统文档内的操作

* `:wq`：保存退出
* `dd`：删除所在行
* `cc`：插入字符



### 有关编程的命令行

* `g++ xxx.cpp`：**编译**一个.cpp文件
* `./xxx.xxx`：**执行**编译好的xxx文件
* `cmake .`：调用cmake对所在目录工程进行cmake编译（多加一个就对上一层目录的文件进行编译）
* `make`：用make命令对工程进行编译，生成可执行程序



### **git命令行**

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



### 使用git流程

* 在github中创建一个新的仓库
* 在需要上传文件的文件夹中初始化仓库，选择Git Bash Here
* `git init`
* `git add` 文件名*
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



## C++相关知识

### STL容器

#### vector类

##### 初始化

- **直接法**：`vector<T> v1`

- 初始化并**赋值**：`vector<T> v2(v1)`（将v1的元素赋值给v2，类型必须相同）

- **列表**初始化：`vector<T> v1={1，2}`：v1的元素为1，2

- **构造对象**初始化：`vector<int> v1(1`

  

  `0,1)`：v1拥有10个相同元素，均为1

注：组成vector的元素也可以是vector，如`vector<vector<int>>`，代表一个vector，里面的元素均是`vector<int>`类



##### 操作

* **添加**元素：`v.push_back(t)`：向v的尾端添加一个值为t的元素
* **检测**元素：`v.empty()`：没有元素返回真，否则返回假
* **检索**元素：`v.size()`：返回v中元素的个数



### 范围for循环

利用定义的变量遍历数组或者容器其的每一个值

```C++
vector<int> vec;
 
//下面两者是等价的遍历
for (auto iter = vec.begin(); iter != vec.end(); iter++) { // before c++11
    cout << *iter << endl;
}
 
for (auto i : vec) { // c++11基于范围的for循环
    cout << "i" << endl;
}
```



如果要**修改**vec的内容：

```C++
for (auto& i : vec) { 
    i++;             增加vec元素中的值
}
 
for (auto i : vec) { 
    cout << "i" << endl;   输出vec的值
}
```

使用**&**符号后表示它被声明为**引用变量**，成为了**元素本身**的别名，因此可以修改





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

![image-20220718000919251](D:/Typora图像/image-20220718000919251.png)

* 对于**Matrix**,norm返回的是矩阵的**佛罗贝尼乌斯范数**

![image-20220718000936091](D:/Typora图像/image-20220718000936091.png)

##### normalize函数

把自身的各元素除以它的范数，**返回值为void**（自身修改），用于**四元数归一化**



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

* 创建任意维度的稠密数组存储图像：`cv:Mat image`

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



##### chrono库

用于测试一段代码的执行时间

```C++
#include<chrono>
chrono::steady_clock::time_point  start =  chrono::steady_clock::now()
auto start = chrono::steady_clock::now();
 
auto end = chrono::steady_clock::now();
 
auto diff = end - start;
cout<<"遍历图像用时: "<<chrono::duration <double> (diff).count()<<" s"<<endl;
//cout << chrono::duration <double, milli> (diff).count() << " ms" << endl;(毫秒)
//cout << chrono::duration <double, nano> (diff).count() << " ns" << endl;（纳秒）
```



##### 格式化函数（方便查找文件）

* 首先用占位符表示文件路径：`boost::format fmt("../%s/%d.%s")`
* 然后代入具体路径：`fmt % "color" % 1 % "png"`
* 综上表示读取文件的路径为：`../color/1.png`
* 将图片推入容器中：`colorImgs.push_back(cv::imread(fmt.str(),1))`



### 曲线拟合问题

#### P134 手写高斯牛顿法

* 生成一个服从均值为0，标准差为val的高斯分布随机数：`rng.gaussian(val)`
* 判断是否为非法数字函数：`isnan(x)`，方程无解，则x为非法字符nan(not a number)，函数返回0；否则返回1
* 矩阵的转置：`Matrix.transpose()`
* 求解线性方程Hx=b：
  * QR分解：`H.colPivHouseholderQr().solve(b);`
  * cholesky分解：`H.ldlt().solve(b);`

