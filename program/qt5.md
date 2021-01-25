#### 生成pro工程文件

```shell
qmake -project
```

#### 编译ui文件

```shell
uic -o ui_headfile.h headfile.ui
```

#### 国际化

```cpp
QObject::tr("Chinese string")
//显示非英文字符串
```

#### 模态对话框与非模态对话框

- **模态对话框**：关闭它之前不能与其他窗口交互：

```cpp
//调用`exec()`函数，或
QDialog *dialog = new QDialog(this);
dialog->setModal(true); //调用setModal函数
dialog->show();
//区别：exec()只有当对话框关闭才返回;
//     show()调用后控制权交回;
```

- **非模态对话框**：

`new`创建指针变量，再使用`show（）`函数显示

#### **信号与槽的自动关联**

自动关联将关联函数整合到槽命名中：

```cpp
void on_pushButton_2_clicked();
//on_部件对象名_信号名
```
**使用注意事项**
- 信号和槽需要继承自QObject或其子类；
- 在类声明的最开始处添加Q_OBJECT宏；
- 槽中参数的类型要和信号参数的类型对应，且不能比信号的参数多；
- 信号只用声明，没有定义，且返回值为void类型；


#### qDebug
```cpp
qDebug() << "color: " << color;
```

#### 事件系统

事件先传递给获得焦点的窗口部件，如果该部件忽略该事件，那么这个事件传递给其父部件。

**pazzle p119事件过滤器**

#### 对象树与拥有权 

- 应用程序销毁窗口部件时会自动销毁子部件，若不是顶层窗口，则只是隐藏；
- QT可重定义父部件，如使用布局管理器时；

#### 发布release版本

- 将`build-xxx-release`目录内可执行文件`.exe`拷贝出；
- 运行 `Qt xxx (MinGW xxx 64-bit)`->`cd path`进入`.exe`路径；
- 执行`windeployqt xxx.exe`