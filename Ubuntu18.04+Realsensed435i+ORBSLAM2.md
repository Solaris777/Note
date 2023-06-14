# Ubuntu18.04+Realsensed435i+ORBSLAM2

参考：[https://blog.csdn.net/u013494161/article/details/114209959?ops_request_misc=&request_id=&biz_id=102&utm_term=ros%E8%B7%91%E9%80%9Aorbslam2&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-3-114209959.142](https://blog.csdn.net/u013494161/article/details/114209959?ops_request_misc=&request_id=&biz_id=102&utm_term=ros跑通orbslam2&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-3-114209959.142)



**大部分内容在参考文章里面都有写，这里说一些要注意的点**



相机接线一定要使用**Type-C3.0**（一般接口为蓝色）

这里的ROS版本是**Melodic**

## 1.注册公钥

`sudo apt-key adv --keyserver [keys.gnupg.net](https://keys.gnupg.net) --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || sudo apt-key adv --keyserver hkp:[//keyserver.ubuntu.com:80](https:////keyserver.ubuntu.com:80) --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE`

这一步我是翻了墙之后才成功的，如下图

![img](https://appdocs.wpscdn.cn/api/v3/office/copy/YXcxMzlUd2lkTUNLM0E1djJzeVo0MGdpNzBETGd1MUt0R0dCVERNUlJRYVk3cjBsZUJKZDRVYzlrMnl5cGJvQnVQV21PUUxsY2VhQWFwY3FuN3VHcHZIR2dMR0V6ZlVva2Y5WjJlb0Fyc1h3cGt5dGdUandRVXFaekpiTFJhNHNmRXpnR2t1bzBnQXRoZlRXN2ExQ3crVGcyWjNLR1FwQjZFUWpkYjAxZnRNdm9lQWl6K1ZJUlVKNUtKM1FRVDJUQWdKMFY3akY4TUhPdVFGRS9iQ2lMZHFmWXc0WDA4cTNwNzVybk1mR3gzZUltb1J6QldBc1ZJM0Z0dkx2MmdTRlJyK1F3WlZZOUxvRCtwUHd2MEF0ZDdZN29lVFkvWHY5YnlYa3RzZER1OUk3aWsvUngzUFJwbDFtdmVZQg==/attach/object/LVXCABAAU4?)



## 2.添加包仓库列表

**这一步有很多以前的教程用的都是amazon的源，但是我试了n遍都没成功，最后在参考文章的评论里面找到了可行的地址**

`sudo add-apt-repository "deb https://librealsense.intel.com/Debian/apt-repo $(lsb_release -cs) main" -u`

## 3.下载对应的ROS包

`cd catkin_ws/src/`

下面这条命令最好手动去github上下载，因为最新的版本是在ROS2环境下的，我这里更改分支后选择了ROS1的版本

`git clone https://github.com/IntelRealSense/realsense-ros.git`

`git clone https://github.com/pal-robotics/ddynamic_reconfigure.git`

`cd ~/catkin_ws && catkin_make`

## 报错

若出现 `Frames didn't arrived within 5 seconds`的重复报错，

![img](https://appdocs.wpscdn.cn/api/v3/office/copy/YXcxMzlUd2lkTUNLM0E1djJzeVo0MGdpNzBETGd1MUt0R0dCVERNUlJRYVk3cjBsZUJKZDRVYzlrMnl5cGJvQnVQV21PUUxsY2VhQWFwY3FuN3VHcHZIR2dMR0V6ZlVva2Y5WjJlb0Fyc1h3cGt5dGdUandRVXFaekpiTFJhNHNmRXpnR2t1bzBnQXRoZlRXN2ExQ3crVGcyWjNLR1FwQjZFUWpkYjAxZnRNdm9lQWl6K1ZJUlVKNUtKM1FRVDJUQWdKMFY3akY4TUhPdVFGRS9iQ2lMZHFmWXc0WDA4cTNwNzVybk1mR3gzZUltb1J6QldBc1ZJM0Z0dkx2MmdTRlJyK1F3WlZZOUxvRCtwUHd2MEF0ZDdZN29lVFkvWHY5YnlYa3RzZER1OUk3aWsvUngzUFJwbDFtdmVZQg==/attach/object/NJXCABAAQE?)

根据https://github.com/IntelRealSense/librealsense/issues/5723指明Linux版本应在5.0以上，所以接下来更新Linux内核版本

https://blog.csdn.net/Megahertz66/article/details/118303541

关于卸载Linux内核

https://blog.csdn.net/weixin_43877080/article/details/107204293



若出现`/opt/ros/kinetic/lib/nodelet/nodelet: symbol lookup error: /home/xxx/catkin_ws/devel/lib//librealsense2_camera.so: undefined symbol: _ZN20ddynamic_reconfigure19DDynamicReconfigure16registerVariableIiEEvRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEET_RKN5boost8functionIFvSA_EEES9_SA_SA_`报错

参考https://blog.csdn.net/fyf8733/article/details/107382599





如果安装后无法更改内核版本或进入grub界面，参考https://blog.csdn.net/yjj350418592/article/details/121759907