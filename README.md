```markdown
## 📁 项目结构
```
Ayuan/
└── Tcp-Server-MS/
    ├── .vscode/          # VSCode 配置文件
    ├── example/          # 示例代码（可选）
    ├── source/           # 核心源码
    │   ├── echo/         # TCP 回显服务
    │   │   ├── echo.hpp  # 回显服务核心逻辑
    │   │   ├── main.cc   # 入口文件
    │   │   └── Makefile  # 编译脚本
    │   ├── http/         # HTTP 静态资源服务
    │   │   ├── wwwroot/  # 静态资源目录（HTML/TXT等）
    │   │   ├── http.hpp  # HTTP 协议解析与响应
    │   │   ├── main.cc   # 入口文件
    │   │   ├── Makefile  # 编译脚本
    │   │   ├── mime      # MIME 类型映射
    │   │   ├── statu     # HTTP 状态码映射
    │   │   └── server.hpp# 服务器核心封装
    │   └── test/         # 测试用例
    │       ├── client*.cpp # 客户端测试代码
    │       ├── server.cc   # 测试服务器
    │       ├── tcp_*.cc    # 基础 TCP 客户端/服务器
    │       └── Makefile    # 编译脚本
    ├── WebBench-master/  # WebBench 压测工具
    ├── .gitignore        # Git 忽略文件
    └── LICENSE           # MIT 开源协议
```
```

---

### 🚀 操作步骤（一步到位）
1.  点击 **Edit** 进入编辑模式
2.  **全选**你现在的所有内容（`Ctrl+A` / `Cmd+A`）
3.  直接粘贴我下面给你的**完整 README 模板**（包含标题、特性、目录树等，排版已经全部调好）
4.  拉到页面底部，点击 **Commit changes** 保存

---

### 📝 完整可用 README（直接复制替换）
```markdown
# Ayuan
基于 Reactor 模式的高性能并发网络服务器，灵感源自 muduo 库。采用 C++11 实现「One Thread One Loop」主从 Reactor 架构，支持 TCP 回显服务与 HTTP/1.1 静态资源服务，专为低延迟、高并发场景设计。

---

## ✨ 核心特性
- **高性能架构**：主从 Reactor + One Thread One Loop 线程模型，充分利用多核 CPU，支持万级并发连接
- **高效 IO 模型**：基于 Linux epoll 边缘触发（ET）实现 IO 多路复用，避免无效轮询
- **连接管理**：智能指针管理连接生命周期，时间轮定时器自动清理非活跃连接（O(1) 操作复杂度）
- **协议支持**：
  - TCP 回显服务：验证基础数据传输能力
  - HTTP/1.1 协议解析：支持 GET/HEAD 请求，可提供静态资源（HTML/TXT/文件等）服务
- **工程化设计**：模块化封装（Server/Channel/Buffer/Connection），代码可复用、易扩展
- **测试工具**：内置客户端测试用例与 WebBench 性能压测工具

---

## 🛠️ 技术栈
- **语言**：C++11
- **平台**：Linux（依赖 epoll 机制）
- **核心组件**：
  - Reactor 事件驱动模型
  - epoll IO 多路复用
  - 时间轮定时器
  - 自动扩容缓冲区（解决 TCP 粘包/拆包）
- **构建工具**：Makefile

---

## 🚀 快速开始
### 1. 克隆项目
```bash
git clone https://github.com/amwqe/Ayuan.git
cd Ayuan/Tcp-Server-MS
```

### 2. 编译运行 TCP 回显服务
```bash
cd source/echo
make
./main  # 默认监听 8080 端口
```
测试：
```bash
telnet localhost 8080
# 输入任意内容，服务器会原样返回
```

### 3. 编译运行 HTTP 静态资源服务
```bash
cd source/http
make
./main  # 默认监听 8080 端口
```
测试：
浏览器访问 `http://localhost:8080/index.html`，或访问 `http://localhost:8080/test.txt` 查看静态资源。

### 4. 运行测试用例
```bash
cd source/test
make
./server  # 启动测试服务器
./client1 # 启动测试客户端
```

---

## 📁 项目结构
```
Ayuan/
└── Tcp-Server-MS/
    ├── .vscode/          # VSCode 配置文件
    ├── example/          # 示例代码（可选）
    ├── source/           # 核心源码
    │   ├── echo/         # TCP 回显服务
    │   │   ├── echo.hpp  # 回显服务核心逻辑
    │   │   ├── main.cc   # 入口文件
    │   │   └── Makefile  # 编译脚本
    │   ├── http/         # HTTP 静态资源服务
    │   │   ├── wwwroot/  # 静态资源目录（HTML/TXT等）
    │   │   ├── http.hpp  # HTTP 协议解析与响应
    │   │   ├── main.cc   # 入口文件
    │   │   ├── Makefile  # 编译脚本
    │   │   ├── mime      # MIME 类型映射
    │   │   ├── statu     # HTTP 状态码映射
    │   │   └── server.hpp# 服务器核心封装
    │   └── test/         # 测试用例
    │       ├── client*.cpp # 客户端测试代码
    │       ├── server.cc   # 测试服务器
    │       ├── tcp_*.cc    # 基础 TCP 客户端/服务器
    │       └── Makefile    # 编译脚本
    ├── WebBench-master/  # WebBench 压测工具
    ├── .gitignore        # Git 忽略文件
    └── LICENSE           # MIT 开源协议
```

---

## 📝 设计思路
本项目参考 muduo 库的经典设计，核心思想：
- **One Thread One Loop**：每个线程绑定一个 EventLoop，避免线程切换开销
- **主从 Reactor**：
  - Main Reactor：负责监听新连接，将连接分发至 Sub Reactor
  - Sub Reactor：负责已连接客户端的 IO 事件处理
- **事件驱动**：通过 epoll 异步监听 IO 事件，回调对应业务逻辑，实现高并发处理
- **缓冲区设计**：自动扩容的用户态缓冲区，解决 TCP 粘包/拆包问题

---

## 📊 性能测试
使用 WebBench 工具进行压测：
```bash
cd WebBench-master
make
./webbench -c 1000 -t 30 http://localhost:8080/index.html
```
可测试服务器在 1000 并发连接、30 秒压测下的 QPS 与响应延迟。

---

## 📄 开源协议
本项目采用 **MIT License** 开源协议，可自由使用、修改和分发，详见 [LICENSE](./LICENSE) 文件。

---

## 🙏 致谢
- 本项目架构设计参考自陈硕的《Linux多线程服务端编程：使用muduo C++网络库》
- Reactor 模式经典论文：*Reactor: An Object Behavioral Pattern for Demultiplexing and Dispatching Handles for Synchronous Events*
```

---
