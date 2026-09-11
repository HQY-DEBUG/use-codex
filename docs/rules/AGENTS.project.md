# FPGA 与上位机工程规则

<!-- 使用方式：复制到目标工程根目录并命名为 AGENTS.md，与全局规则配合使用。本模板基于 ref 中的 FPGA / Vitis / Qt / Python / MATLAB 工程约定，不是 use_codex 文档资料仓库的目录改造要求。 -->

## 适用范围

- 本文件中的路径均相对于目标工程根目录。
- 按实际涉及的目录和语言应用规则；不要为了符合目录树而创建本项目不需要的模块。
- 对已有工程保留工具生成的结构、第三方接口及已有公共标识符；新建或修改自有代码时执行对应规范，不批量改写无关文件。
- 本文件已合并原零散规则，无需依赖原 `ref/` 文件夹或 Copilot 的 `applyTo` 字段。
- 通用工作习惯、仓库组织原则和代码标注遵循已安装的全局规则；此文件补充工程专属约定。安装时同时选择一份全局规则和一份项目规则。

## 项目目录

```text
项目根目录/
├── AGENTS.md
├── requirements.txt       # Python 依赖清单（需要时）
├── doc/
│   ├── code/              # 接口和模块设计说明
│   ├── ref/               # 数据手册、标准等参考资料
│   └── <库名>/            # 与实际库或模块同名的专项说明
├── PC/
│   ├── py_code/           # Python / PyQt5 上位机
│   └── qt/                # Qt C++ 工程及 .ui 等资源
├── 项目名称/
│   ├── *.tcl              # 工程 TCL 脚本
│   ├── xilinx.py          # Xilinx 工具脚本
│   ├── bit/               # .bit / .xsa / .ltx 编译产物
│   └── source/
│       ├── constraints/   # .xdc 约束
│       ├── ip/            # IP 核
│       ├── sim/           # testbench 与仿真激励
│       └── verilog/       # .v / .sv / .vh / .svh 源码
├── vitis/                 # PS 端工程，由 Vitis IDE 管理
└── matlab/                # MATLAB 脚本和函数
```

- `项目名称/` 是 FPGA 工程目录的占位名称，使用目标工程实际名称替换。
- `doc/<库名>/` 使用明确的库或模块名称，避免 `lib`、`other` 等含糊命名。
- `项目名称/source/` 可以按功能继续分层，保留 constraints、ip、sim、verilog 四类目录；`项目名称/bit/` 不存放源码。
- 不手动调整 Vitis IDE 自动生成的顶层结构。

## Python 运行环境

- 本工程自有 Python 代码使用系统全局 Python，不创建或激活 venv、virtualenv、conda env、pipenv 等隔离环境。
- 执行前确认实际解释器；使用 `python script.py` 运行，使用同一解释器的 `python -m pip install <package>` 安装依赖。
- 需要依赖清单时使用根目录 `requirements.txt`，通过 `python -m pip install -r requirements.txt` 安装到该系统解释器。
- 这项偏好限定于本工程，不推广为其他项目或外部工具运行时的环境要求。

## 命名

| 对象 | 规则 |
| --- | --- |
| C/C++ 变量、普通函数 | 小写下划线，如 `recv_buf`、`init_dma()` |
| C/C++ 宏、全局变量、成员变量 | 宏全大写下划线；全局变量加 `g_`；成员小写下划线，可加 `m_` |
| C/C++ 类和结构体 | 新命名用大驼峰，如 `UdpWorker`；已有小写下划线结构体保留 |
| Python 变量、函数、类、常量 | 变量和函数小写下划线，类大驼峰，常量全大写下划线，私有成员单下划线前缀 |
| MATLAB 变量、函数、常量 | 变量和函数小写下划线，常量全大写下划线 |
| Verilog 模块与信号 | 小写下划线；参数和宏全大写下划线 |
| Verilog 延时和复位信号 | 延时用 `_r`，多级用 `_r1`、`_r2`；低有效复位 `rstn`，高有效复位 `rst` |
| Verilog 输入输出消歧 | 必要时输入加 `_i`、输出加 `_o`；AXI/AXIS 等总线接口信号不加这些后缀 |
| Qt 信号、槽、UI 成员 | 信号如 `dataReceived`；槽如 `onDataReceived`；UI 成员如 `m_btnSend`，优先于普通 C++ 命名 |

- 名称应表达用途，避免单字母；循环变量 `i`、`j`、`k` 除外。
- C/C++、Python、Qt 布尔变量使用 `is_`、`has_`、`can_` 等前缀；不将此要求套用到 Verilog。

## C/C++：.c、.h、.cpp、.hpp

- 使用 4 个空格缩进，不用 Tab；大括号使用 K&R 风格，左括号不另起一行。
- 函数调用及参数写在同一行，不拆分参数到多行。
- 普通注释使用中文 `//`；区块标题用 `// ---- 内容 ----//`。
- 文件头允许 `/* ... */`，Doxygen 函数头允许 `/** ... */`；其他行内和独立注释不用块注释。
- 同一连续代码块中的结构体字段、成组宏和初始化表项，行尾注释纵向对齐。
- 函数头使用中文 Doxygen，说明实际参数、返回值和必要注意事项：

```cpp
/**
 * @brief  函数用途
 * @param  param1 参数说明
 * @return 返回值说明
 * @note   注意事项或接口影响（按需填写）
 */
```

- 自有状态返回型接口约定 `0` 成功、负数为错误；第三方 API、返回指针或数据值的函数遵循其真实契约。
- 检查可能失败的调用结果，如 malloc、fopen 和有状态返回值的硬件访问；错误通过日志模块记录，不直接用 printf 输出错误。
- `.c`、`.h` 文件头采用以下格式；其他自有代码文件保留等价的版本字段：

```c
/*
 * 文件 : xxx.c
 * 描述 : 模块说明
 * 版本 : v1.0
 * 日期 : YYYY/MM/DD
 *
 * 修改记录（最新在前，最多 3 条）:
 * 版本  作者  日期        修改内容
 * v1.0  ---   YYYY/MM/DD 创建文件
 */
```

## Qt C++：PC/qt/ 下的代码

- 叠加 C/C++ 规则；已有工程使用 `qt/` 目录时同样适用，不为统一路径迁移整个工程。
- GUI 线程中的 QWidget / QMainWindow 负责 UI 更新，不运行耗时网络或 IO；耗时工作放到工作线程，沿用现有 QThread 子类或 worker 对象结构。
- 跨线程消息通过 Qt 信号槽传递，使用 `Qt::QueuedConnection`；禁止从工作线程直接操作控件。
- 自定义 QObject 派生类在类声明开头放置 `Q_OBJECT`；QObject / QThread 工作类构造函数接受 `QObject *parent = nullptr` 并传给基类，QWidget 派生控件使用 `QWidget *parent = nullptr`。
- 优先使用 Qt 父子对象树管理生命周期，跨线程对象遵循所属线程的创建和销毁约定。
- 线程退出采用协作式停止：事件循环线程使用 `quit()`，等待结束后释放；自定义 run 循环需检查停止标志或中断请求并解除阻塞，不能仅依赖 quit。禁止直接 `terminate()`。
- 不假定 QThread 子类的槽自动在工作线程执行；需要工作线程接收槽调用时使用正确线程归属的 worker 对象。

## Python：自有 .py 文件

- 文件头使用模块 docstring，记录文件名、用途、版本、日期和修改记录；函数 docstring 说明用途、Args 和 Returns，类 docstring 说明用途及 Attributes。
- 行内注释使用中文 `#`；变更标注按全局规则。
- 捕获具体异常，不使用裸 `except:`，除顶层兜底外不用笼统 `except Exception:`；资源支持上下文管理时使用 `with`，确保文件和 socket 释放。
- PyQt5 的 GUI 线程只做 UI 更新；工作线程独立管理 socket 等资源，跨线程通信只用信号槽，不直接访问控件。
- 高吞吐场景中，批量队列操作使用 deque，日志通过定时器批量刷新，状态标签的 setText 更新限频。

```python
"""
文件名.py -- 模块说明
版本 : v1.0
日期 : YYYY/MM/DD

修改记录（最新在前，最多 3 条）:
    v1.0 YYYY/MM/DD 创建文件
"""
```

## Verilog / SystemVerilog：.v、.sv、.vh、.svh

- 使用 2 个空格缩进，不用 Tab；begin 另起一行，不与 always、if、else、for 等写在同一行。
- 单 bit 比较和赋值显式使用 `1'b0`、`1'b1`，不用无位宽的 `0`、`1`。
- 时序逻辑使用非阻塞赋值 `<=`，组合逻辑使用阻塞赋值 `=`。
- 端口每行一个信号，按方向、类型、位宽、名称、逗号分列对齐，末尾使用中文 `//` 注释。
- 寄存器和信号定义按类型、位宽、名称、分号分列对齐，末尾使用中文 `//` 注释。
- 组合逻辑 case 包含 default；所有组合输出在所有路径均赋值，必要时先设默认值，避免仅添加 default 却仍产生锁存器。
- 异步复位释放时检查亚稳态风险，按时钟域要求进行同步处理。
- 文件头使用 Function、Version、Date、Description、Modify 字段，修改记录遵循全局版本规则：

```verilog
/**************************************************************************/
// Function   : 模块功能
// Version    : v1.0
// Date       : YYYY/MM/DD
// Description: 详细说明
// Modify:
// version     date        modify
// v1.0        YYYY/MM/DD  创建文件
/**************************************************************************/
```

## MATLAB：.m 文件

- 文件头使用 `%` 注释记录文件、描述、版本、日期和修改记录。
- 函数声明后紧接 `%` 帮助注释，说明用途、输入和输出；普通注释使用中文 `%`，区块标题使用 `%% 内容`。
- 矩阵运算优先于循环；需要循环时简述原因。
- 避免无差别使用 clear all / close all，确有需要时在脚本顶部显式清理。
- 图形输出设置标题、坐标轴标签和单位；代码变更标注遵循全局规则。

## 项目文档：doc/ 下的自有 Markdown

- 文档正文第一行为一级标题，随后为版本日期引用块、修改记录表和分隔线。
- 修改记录倒序排列，仅保留最近 3 个版本；一般变更递增次版本，重大重构递增主版本。
- 文档头版本与最新记录一致，日期统一 `YYYY/MM/DD`；修改描述使用“新增……”等中文祈使句。
- 本规范适用于自编项目文档，不向外部参考资料、官方快照、工具生成文件批量插入版本头；AGENTS.md 自身使用清晰的规则结构即可。

```markdown
# 文档标题

> 版本：v1.0　日期：YYYY/MM/DD

## 修改记录

| 版本 | 日期 | 修改内容 |
| --- | --- | --- |
| v1.0 | YYYY/MM/DD | 创建文档 |

---

正文内容
```

## 项目验证

- 先查阅目标工程 README、构建脚本和已有测试，确定解释器、Qt、Vivado、Vitis、MATLAB 的实际版本及可用命令。
- 按修改范围执行已有编译、测试、仿真或文档检查；缺少工具、硬件或许可证时说明未验证部分，不声称完成板上验证。

<!-- 整理来源：ref/ 下 14 个 .instructions.md 与 all_rules.md。项目部分合并 project-structure 的工程目录、python-env、naming-conventions、doc-style、c-cpp-style、qt-cpp-style、python-style、verilog-style、matlab-style，以及 error-handling 的语言专属要求；通用代码标注与目录组织原则已移至全局规则。
统一与修正：保留单行 C/C++ 调用和系统 Python 偏好；Doxygen 明确列为块注释例外；类与结构体采用专门风格文件的大驼峰默认；Qt 路径以目录规范的 PC/qt 为准，兼容已有 qt；FPGA 目录沿用用户调整后的项目名称占位；日期示例统一四位年份；Markdown 修改记录降为二级标题；补充 Qt 控件 parent 类型和线程退出条件，避免将工作类约束错误套用于控件。
Qt 技术核对：https://doc.qt.io/qt-6/qthread.html 与 https://doc.qt.io/qt-6/qwidget.html 。 -->
