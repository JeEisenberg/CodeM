![](..\pic\div\shark.gif)

# Idea

Idea安装结束后

![](..\pic\idea_Open.png)

- **重点说明：** IntelliJ IDEA 是没有类似 Eclipse 的工作空间的概念（`Workspaces` ），最大单元就是 `Project`。如果你同时观察多个项目的情况，IntelliJ IDEA 提供的解决方案是打开多个项目实例，你可以理解为开多个项目窗口。
- 命令 `Create New Project` 创建一个新项目。
- 命令 `Import Project` 导入一个已有项目。
- 命令 `Open` 打开一个已有项目，可以直接打开 Eclipse 项目，但是由于两者 IDE 下的项目配置不一样，所以项目还是需要配置的。
- 命令 `Check out from Version Control` 可以通过服务器上的项目地址 Checkout Github 上面项目或是其他 Git 托管服务器上的项目。
- 对于首次创建或打开的新项目，IntelliJ IDEA 都会创建项目索引，如上图标注 1 所示。大型项目在创建索引过程中可能必须会卡顿，所以 **强烈建议** 创建索引过程最好不要动项目。

## Project 和 Module 介绍

这两个概念是 IntelliJ IDEA 的必懂知识点之一，请务必要学会。

如果你是 Eclipse 用户，并且已经看了上面给的链接，那 IntelliJ IDEA 首先告诉你一个非常重要的事情：IntelliJ IDEA 没有类似 Eclipse 工作空间（workspace）的概念的。很多从 Eclipse 转过来的人总是下意识地要再同一个窗口管理 n 个项目，这在 IntelliJ IDEA 是无法得到。IntelliJ IDEA 提供的体验是：一个 Project 打开一个 Window 窗口。

对于 Project，IntelliJ IDEA 是这样解释的：

> - Whatever you do in IntelliJ IDEA, you do that in the context of a project. A project is an organizational unit that represents a complete software solution. It serves as a basis for coding assistance, bulk refactoring, coding style consistency, etc.
> - Your finished product may be decomposed into a series of discrete, isolated modules, but it's a project definition that brings them together and ties them into a greater whole.
> - Projects don't themselves contain development artifacts such as source code, build scripts, or documentation. They are the highest level of organization in the IDE, and they define project-wide settings as well as collections of what IntelliJ IDEA refers to as modules and libraries.
>
> > - 链接地址：<https://www.jetbrains.com/idea/help/project.html>

对于 Module，IntelliJ IDEA 是这样解释的：

> - A module is a discrete unit of functionality which you can compile, run, test and debug independently.
> - Modules contain everything that is required for their specific tasks: source code, build scripts, unit tests, deployment descriptors, and documentation. However, modules exist and are functional only in the context of a project.
> - Configuration information for a module is stored in a .iml module file. By default, such a file is located in the module's content root folder.
> - Development teams, normally, share the .iml module files through version control.
>
> > - 链接地址：<https://www.jetbrains.com/idea/help/module.html>

通过上面的介绍我们知道，在 IntelliJ IDEA 中 Project 是最顶级的级别，次级别是 Module。一个 Project 可以有多个 Module。目前主流的大型项目结构都是类似这种多 Module 结构，这类项目一般是这样划分的，比如：core Module、web Module、plugin Module、solr Module 等等，模块之间彼此可以相互依赖。通过这些 Module 的命名也可以看出，他们之间应该都是处于同一个项目业务情况下的模块，彼此之间是有不可分割的业务关系的。

所以我们现在总结：一个 `Project` 是由一个或多个 `Module` 组成，模块之间尽量是处在同一个项目业务的的情况下，彼此之间互相依赖关联。这里用的是 `尽量`，因为 IntelliJ IDEA 的 Project 是一个没有具备任何编码设置、构建等开发功能的，主要起到一个项目定义、范围约束、规范等类型的效果，也许我们可以简单地理解为就是一个单纯的目录，只是这个目录命名上必须有其代表性的意义。

## Idea常见快捷键

```java
package com.array.sort;

/**
 * @Auther: GavinLim
 * @Date: 2021/6/27 - 06 - 27 - 15:12
 * @Description: com.array.sort
 * @version: 1.0
 */
class Student{
    private String name;
    private int age;
    private  char gender;

    public Student(String name, int age, char gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    public Student() {

    }

    public void setName(String name) {
        this.name = name;
    }
}
public class Kuaijiejian {
    public Kuaijiejian() {
    }
    public static void main(String[] args) {
        {
            System.out.println("删除行");//ctrl+y
            System.out.println("快捷键练习");
            System.out.println("快捷键练习");//ctrl+d
            System.out.println("代码上移或者下移动");//ctrl+shift+up\down
            System.out.println("代码选中");//ctrl+shift+right\left
            System.out.println("搜索代码内容--替换等操作");
            System.out.println("快速插入代码--比如构造方法，setter/getter方法");//alt+insert
            Student student = new Student();//alt+enter  快速导包或者生成变量
        }
        System.out.println("单行注释");//ctrl+/
        System.out.println("多行注释");//ctrl+shift+/
        System.out.println("重命名");//shift+F6
        System.out.println("for循环--fori");//fori
        for (int i = 0; i < 10; i++) {

        }
        System.out.println("代码块包围");//ctrl+alt+t
        System.out.println("代码提示");//alt+/
        
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210627162302837.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



**Ctrl**

| 快捷键           | 介绍                                                         |
| ---------------- | ------------------------------------------------------------ |
| Ctrl + F         | 在当前文件进行文本查找 `（必备）`                            |
| Ctrl + R         | 在当前文件进行文本替换 `（必备）`                            |
| Ctrl + Z         | 撤销 `（必备）`                                              |
| Ctrl + Y         | 删除光标所在行 或 删除选中的行 `（必备）`                    |
| Ctrl + X         | 剪切光标所在行 或 剪切选择内容                               |
| Ctrl + C         | 复制光标所在行 或 复制选择内容                               |
| Ctrl + D         | 复制光标所在行 或 复制选择内容，并把复制内容插入光标位置下面 `（必备）` |
| Ctrl + W         | 递进式选择代码块。可选中光标所在的单词或段落，连续按会在原有选中的基础上再扩展选中范围 `（必备）` |
| Ctrl + E         | 显示最近打开的文件记录列表                                   |
| Ctrl + N         | 根据输入的 **类名** 查找类文件                               |
| Ctrl + G         | 在当前文件跳转到指定行处                                     |
| Ctrl + J         | 插入自定义动态代码模板                                       |
| Ctrl + P         | 方法参数提示显示                                             |
| Ctrl + Q         | 光标所在的变量 / 类名 / 方法名等上面（也可以在提示补充的时候按），显示文档内容 |
| Ctrl + U         | 前往当前光标所在的方法的父类的方法 / 接口定义                |
| Ctrl + B         | 进入光标所在的方法/变量的接口或是定义出，等效于 `Ctrl + 左键单击` |
| Ctrl + K         | 版本控制提交项目，需要此项目有加入到版本控制才可用           |
| Ctrl + T         | 版本控制更新项目，需要此项目有加入到版本控制才可用           |
| Ctrl + H         | 显示当前类的层次结构                                         |
| Ctrl + O         | 选择可重写的方法                                             |
| Ctrl + I         | 选择可继承的方法                                             |
| Ctrl + +         | 展开代码                                                     |
| Ctrl + -         | 折叠代码                                                     |
| Ctrl + /         | 注释光标所在行代码，会根据当前不同文件类型使用不同的注释符号 `（必备）` |
| Ctrl + [         | 移动光标到当前所在代码的花括号开始位置                       |
| Ctrl + ]         | 移动光标到当前所在代码的花括号结束位置                       |
| Ctrl + F1        | 在光标所在的错误代码出显示错误信息                           |
| Ctrl + F3        | 调转到所选中的词的下一个引用位置                             |
| Ctrl + F4        | 关闭当前编辑文件                                             |
| Ctrl + F8        | 在 Debug 模式下，设置光标当前行为断点，如果当前已经是断点则去掉断点 |
| Ctrl + F9        | 执行 Make Project 操作                                       |
| Ctrl + F11       | 选中文件 / 文件夹，使用助记符设定 / 取消书签                 |
| Ctrl + F12       | 弹出当前文件结构层，可以在弹出的层上直接输入，进行筛选       |
| Ctrl + Tab       | 编辑窗口切换，如果在切换的过程又加按上delete，则是关闭对应选中的窗口 |
| Ctrl + Enter     | 智能分隔行                                                   |
| Ctrl + End       | 跳到文件尾                                                   |
| Ctrl + Home      | 跳到文件头                                                   |
| Ctrl + Space     | 基础代码补全，默认在 Windows 系统上被输入法占用，需要进行修改，建议修改为 `Ctrl + 逗号` `（必备）` |
| Ctrl + Delete    | 删除光标后面的单词或是中文句                                 |
| Ctrl + BackSpace | 删除光标前面的单词或是中文句                                 |
| Ctrl + 1,2,3...9 | 定位到对应数值的书签位置                                     |
| Ctrl + 左键单击  | 在打开的文件标题上，弹出该文件路径                           |
| Ctrl + 光标定位  | 按 Ctrl 不要松开，会显示光标所在的类信息摘要                 |
| Ctrl + 左方向键  | 光标跳转到当前单词 / 中文句的左侧开头位置                    |
| Ctrl + 右方向键  | 光标跳转到当前单词 / 中文句的右侧开头位置                    |
| Ctrl + 前方向键  | 等效于鼠标滚轮向前效果                                       |
| Ctrl + 后方向键  | 等效于鼠标滚轮向后效果                                       |

**Alt**

| 快捷键          | 介绍                                                         |
| --------------- | ------------------------------------------------------------ |
| Alt + `         | 显示版本控制常用操作菜单弹出层                               |
| Alt + Q         | 弹出一个提示，显示当前类的声明 / 上下文信息                  |
| Alt + F1        | 显示当前文件选择目标弹出层，弹出层中有很多目标可以进行选择   |
| Alt + F2        | 对于前面页面，显示各类浏览器打开目标选择弹出层               |
| Alt + F3        | 选中文本，逐个往下查找相同文本，并高亮显示                   |
| Alt + F7        | 查找光标所在的方法 / 变量 / 类被调用的地方                   |
| Alt + F8        | 在 Debug 的状态下，选中对象，弹出可输入计算表达式调试框，查看该输入内容的调试结果 |
| Alt + Home      | 定位 / 显示到当前文件的 `Navigation Bar`                     |
| Alt + Enter     | IntelliJ IDEA 根据光标所在问题，提供快速修复选择，光标放在的位置不同提示的结果也不同 `（必备）` |
| Alt + Insert    | 代码自动生成，如生成对象的 set / get 方法，构造函数，toString() 等 |
| Alt + 左方向键  | 按左方向切换当前已打开的文件视图                             |
| Alt + 右方向键  | 按右方向切换当前已打开的文件视图                             |
| Alt + 前方向键  | 当前光标跳转到当前文件的前一个方法名位置                     |
| Alt + 后方向键  | 当前光标跳转到当前文件的后一个方法名位置                     |
| Alt + 1,2,3...9 | 显示对应数值的选项卡，其中 1 是 Project 用得最多             |

**Shift**

| 快捷键               | 介绍                                                         |
| -------------------- | ------------------------------------------------------------ |
| Shift + F1           | 如果有外部文档可以连接外部文档                               |
| Shift + F2           | 跳转到上一个高亮错误 或 警告位置                             |
| Shift + F3           | 在查找模式下，查找匹配上一个                                 |
| Shift + F4           | 对当前打开的文件，使用新Windows窗口打开，旧窗口保留          |
| Shift + F6           | 对文件 / 文件夹 重命名                                       |
| Shift + F7           | 在 Debug 模式下，智能步入。断点所在行上有多个方法调用，会弹出进入哪个方法 |
| Shift + F8           | 在 Debug 模式下，跳出，表现出来的效果跟 `F9` 一样            |
| Shift + F9           | 等效于点击工具栏的 `Debug` 按钮                              |
| Shift + F10          | 等效于点击工具栏的 `Run` 按钮                                |
| Shift + F11          | 弹出书签显示层                                               |
| Shift + Tab          | 取消缩进                                                     |
| Shift + ESC          | 隐藏当前 或 最后一个激活的工具窗口                           |
| Shift + End          | 选中光标到当前行尾位置                                       |
| Shift + Home         | 选中光标到当前行头位置                                       |
| Shift + Enter        | 开始新一行。光标所在行下空出一行，光标定位到新行位置         |
| Shift + 左键单击     | 在打开的文件名上按此快捷键，可以关闭当前打开文件             |
| Shift + 滚轮前后滚动 | 当前文件的横向滚动轴滚动                                     |

**Ctrl + Alt**

| 快捷键                | 介绍                                                         |
| --------------------- | ------------------------------------------------------------ |
| Ctrl + Alt + L        | 格式化代码，可以对当前文件和整个包目录使用 `（必备）`        |
| Ctrl + Alt + O        | 优化导入的类，可以对当前文件和整个包目录使用 `（必备）`      |
| Ctrl + Alt + I        | 光标所在行 或 选中部分进行自动代码缩进，有点类似格式化       |
| Ctrl + Alt + T        | 对选中的代码弹出环绕选项弹出层                               |
| Ctrl + Alt + J        | 弹出模板选择窗口，讲选定的代码加入动态模板中                 |
| Ctrl + Alt + H        | 调用层次                                                     |
| Ctrl + Alt + B        | 在某个调用的方法名上使用会跳到具体的实现处，可以跳过接口     |
| Ctrl + Alt + V        | 快速引进变量                                                 |
| Ctrl + Alt + Y        | 同步、刷新                                                   |
| Ctrl + Alt + S        | 打开 IntelliJ IDEA 系统设置                                  |
| Ctrl + Alt + F7       | 显示使用的地方。寻找被该类或是变量被调用的地方，用弹出框的方式找出来 |
| Ctrl + Alt + F11      | 切换全屏模式                                                 |
| Ctrl + Alt + Enter    | 光标所在行上空出一行，光标定位到新行                         |
| Ctrl + Alt + Home     | 弹出跟当前文件有关联的文件弹出层                             |
| Ctrl + Alt + Space    | 类名自动完成                                                 |
| Ctrl + Alt + 左方向键 | 退回到上一个操作的地方 `（必备）`                            |
| Ctrl + Alt + 右方向键 | 前进到上一个操作的地方 `（必备）`                            |
| Ctrl + Alt + 前方向键 | 在查找模式下，跳到上个查找的文件                             |
| Ctrl + Alt + 后方向键 | 在查找模式下，跳到下个查找的文件                             |

**Ctrl + Shift**

| 快捷键                   | 介绍                                                         |
| ------------------------ | ------------------------------------------------------------ |
| Ctrl + Shift + F         | 根据输入内容查找整个项目 或 指定目录内文件 `（必备）`        |
| Ctrl + Shift + R         | 根据输入内容替换对应内容，范围为整个项目 或 指定目录内文件 `（必备）` |
| Ctrl + Shift + J         | 自动将下一行合并到当前行末尾 `（必备）`                      |
| Ctrl + Shift + Z         | 取消撤销 `（必备）`                                          |
| Ctrl + Shift + W         | 递进式取消选择代码块。可选中光标所在的单词或段落，连续按会在原有选中的基础上再扩展取消选中范围 `（必备）` |
| Ctrl + Shift + N         | 通过文件名定位 / 打开文件 / 目录，打开目录需要在输入的内容后面多加一个正斜杠 `（必备）` |
| Ctrl + Shift + U         | 对选中的代码进行大 / 小写轮流转换 `（必备）`                 |
| Ctrl + Shift + T         | 对当前类生成单元测试类，如果已经存在的单元测试类则可以进行选择 |
| Ctrl + Shift + C         | 复制当前文件磁盘路径到剪贴板                                 |
| Ctrl + Shift + V         | 弹出缓存的最近拷贝的内容管理器弹出层                         |
| Ctrl + Shift + E         | 显示最近修改的文件列表的弹出层                               |
| Ctrl + Shift + H         | 显示方法层次结构                                             |
| Ctrl + Shift + B         | 跳转到类型声明处                                             |
| Ctrl + Shift + I         | 快速查看光标所在的方法 或 类的定义                           |
| Ctrl + Shift + A         | 查找动作 / 设置                                              |
| Ctrl + Shift + /         | 代码块注释 `（必备）`                                        |
| Ctrl + Shift + [         | 选中从光标所在位置到它的顶部中括号位置                       |
| Ctrl + Shift + ]         | 选中从光标所在位置到它的底部中括号位置                       |
| Ctrl + Shift + +         | 展开所有代码                                                 |
| Ctrl + Shift + -         | 折叠所有代码                                                 |
| Ctrl + Shift + F7        | 高亮显示所有该选中文本，按Esc高亮消失                        |
| Ctrl + Shift + F8        | 在 Debug 模式下，指定断点进入条件                            |
| Ctrl + Shift + F9        | 编译选中的文件 / 包 / Module                                 |
| Ctrl + Shift + F12       | 编辑器最大化                                                 |
| Ctrl + Shift + Space     | 智能代码提示                                                 |
| Ctrl + Shift + Enter     | 自动结束代码，行末自动添加分号 `（必备）`                    |
| Ctrl + Shift + Backspace | 退回到上次修改的地方                                         |
| Ctrl + Shift + 1,2,3...9 | 快速添加指定数值的书签                                       |
| Ctrl + Shift + 左方向键  | 在代码文件上，光标跳转到当前单词 / 中文句的左侧开头位置，同时选中该单词 / 中文句 |
| Ctrl + Shift + 右方向键  | 在代码文件上，光标跳转到当前单词 / 中文句的右侧开头位置，同时选中该单词 / 中文句 |
| Ctrl + Shift + 左方向键  | 在光标焦点是在工具选项卡上，缩小选项卡区域                   |
| Ctrl + Shift + 右方向键  | 在光标焦点是在工具选项卡上，扩大选项卡区域                   |
| Ctrl + Shift + 前方向键  | 光标放在方法名上，将方法移动到上一个方法前面，调整方法排序   |
| Ctrl + Shift + 后方向键  | 光标放在方法名上，将方法移动到下一个方法前面，调整方法排序   |

**Alt + Shift**

| 快捷键                 | 介绍                                                         |
| ---------------------- | ------------------------------------------------------------ |
| Alt + Shift + N        | 选择 / 添加 task                                             |
| Alt + Shift + F        | 显示添加到收藏夹弹出层                                       |
| Alt + Shift + C        | 查看最近操作项目的变化情况列表                               |
| Alt + Shift + F        | 添加到收藏夹                                                 |
| Alt + Shift + I        | 查看项目当前文件                                             |
| Alt + Shift + F7       | 在 Debug 模式下，下一步，进入当前方法体内，如果方法体还有方法，则会进入该内嵌的方法中，依此循环进入 |
| Alt + Shift + F9       | 弹出 `Debug` 的可选择菜单                                    |
| Alt + Shift + F10      | 弹出 `Run` 的可选择菜单                                      |
| Alt + Shift + 左键双击 | 选择被双击的单词 / 中文句，按住不放，可以同时选择其他单词 / 中文句 |
| Alt + Shift + 前方向键 | 移动光标所在行向上移动                                       |
| Alt + Shift + 后方向键 | 移动光标所在行向下移动                                       |

**Ctrl + Shift + Alt**

| 快捷键                 | 介绍                  |
| ---------------------- | --------------------- |
| Ctrl + Shift + Alt + V | 无格式黏贴            |
| Ctrl + Shift + Alt + N | 前往指定的变量 / 方法 |
| Ctrl + Shift + Alt + S | 打开当前项目设置      |
| Ctrl + Shift + Alt + C | 复制参考信息          |

**其他**

| 快捷键        | 介绍                                                         |
| ------------- | ------------------------------------------------------------ |
| F2            | 跳转到下一个高亮错误 或 警告位置 `（必备）`                  |
| F3            | 在查找模式下，定位到下一个匹配处                             |
| F4            | 编辑源                                                       |
| F7            | 在 Debug 模式下，进入下一步，如果当前行断点是一个方法，则进入当前方法体内，如果该方法体还有方法，则不会进入该内嵌的方法中 |
| F8            | 在 Debug 模式下，进入下一步，如果当前行断点是一个方法，则不进入当前方法体内 |
| F9            | 在 Debug 模式下，恢复程序运行，但是如果该断点下面代码还有断点则停在下一个断点上 |
| F11           | 添加书签                                                     |
| F12           | 回到前一个工具窗口                                           |
| Tab           | 缩进                                                         |
| ESC           | 从工具窗口进入代码文件窗口                                   |
| 连按两次Shift | 弹出 `Search Everywhere` 弹出层                              |

**官网快捷键资料**

> - Windows / Linux：<https://www.jetbrains.com/idea/docs/IntelliJIDEA_ReferenceCard.pdf>
> - Mac OS X：<https://www.jetbrains.com/idea/docs/IntelliJIDEA_ReferenceCard_Mac.pdf>

**第三方快捷键资料**

> - 来自 eta02913：<http://xinyuwu.iteye.com/blog/1005454>

## 英文标点在IDEA中不起作用

今天偶然间因为不小心按错了键,导致中文标点在Idea中不是英文标点-----你可能会有些迷惑,先看截图
![在这里插入图片描述](https://img-blog.csdnimg.cn/5690195a69af4c5cbbeecb5882e2d882.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)中文状态下在idea中输入分号,发现不对劲了你可能以为是系统的BUG
![在这里插入图片描述](https://img-blog.csdnimg.cn/072665b4a3d24d62a4aae1b9ffb3a05d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)一开始我也这么认为的,直到 按下ctrl+。 键我才发现，原来中文状态可以锁定为中文标点，
![在这里插入图片描述](https://img-blog.csdnimg.cn/fd79689e352a4afa8d7f9ae8078ede9d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)切回来不就完了。问题解决；



## 由于编码问题引起的编译错误

- 编译报错：找不到符号、未结束的字符串文字 等的解决办法：
- 由于 UTF-8 编码文件有分 有BOM 和 无BOM 之分，默认情况下 IntelliJ IDEA 使用的编译器是 javac，而此编译只能编译 无BOM 的文件，有很多 Eclipse 用户在使用 IntelliJ IDEA 开发 Eclipse 项目的时候常常会遇到此问题。主要是因为 Eclipse 的编译器是 Eclipse，此编译器支持 有BOM 的文件编译。顾，解决办法是对于此文件进行 BOM 去除。
- 批量去除 BOM，你可以 Baidu：批量去除 BOM、批量转换无 BOM 等关键字，网络上已有提供各种方案。
- 除了通过去除 BOM 还有设置 IntelliJ IDEA 的编译器为 Eclipse，但是一般不建议这样做。
- 如果上述问题都无法解决，而且你也确认 IntelliJ IDEA 各个配置编码的地方都是 UTF-8，报错文件编码也是是 UTF-8 无 BOM 的话，那还有一种可能也会出现这种情况：项目配置文件有问题。项目编码的配置文件在：/项目目录/.idea/encodings.xml。如果你会修改此文件可以进行修改，如果不会，那就删除掉 .idea 整个目录，重启 IntelliJ IDEA 重新配置这个项目即可。

<center>

[<div   style="float:left;width:180px;heigh:20px"/>![](../pic/prepage.png)</div>]()

[<div   style="float:right;width:180px;heigh:20px"/>![](../pic/nextpage.png)</div>](https://blog.csdn.net/weixin_54061333?spm=1010.2135.3001.5421)

</center>

------

<p style="text-align:center"><b>扫描下方二维码,回复'Idea'获取更多Idea操作</b></p>

![](..\pic\div\weichatcode.png)