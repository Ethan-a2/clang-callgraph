# 入门路径与练习题目、最佳实践

## 入门路径
1. 安装环境：
   - 系统安装 clang 与 libclang（如 `sudo apt install clang-14 libclang-14-dev`）。
   - Python 环境准备与 `pip install .` 安装本工具。
2. 快速试用：
   - 使用一个最小 C++ 文件（含两个函数的相互调用）运行 `clang-callgraph file.cpp --lookup foo`。
3. 使用编译数据库：
   - 在 CMake 项目中启用 `CMAKE_EXPORT_COMPILE_COMMANDS`，用 `clang-callgraph build/compile_commands.json` 进行交互查询。
4. 配置过滤：
   - 创建 `callgraph.yml`，设置 `excluded_prefixes` 和 `excluded_paths`，验证过滤效果。

## 练习题目
1. 编写一个包含类方法与虚函数的示例项目，运行工具并观察虚函数标记输出；尝试通过 `-x` 隐藏某命名空间。
2. 在包含第三方库的项目中，使用 `-p /usr` 与 YAML 配置排除系统头，比较有无过滤时的输出差异与解析性能。
3. 构造一个循环调用或递归调用的示例，观察 `print_calls` 的深度限制行为；修改深度参数并评估输出规模与可读性。
4. 扩展命令行：在不改变主逻辑的前提下添加一个 `--format json` 的占位选项（后续实现），讨论设计方案。

## 最佳实践
- 依赖管理：确保 Python `clang` 版本与系统 `libclang` 匹配；将版本要求记录在文档与 CI。
- 配置复用：将常用的 `-I`/`-D`/`-std` 参数与过滤策略放入 `callgraph.yml`，在不同项目中复用。
- 性能控制：
  - 优先使用 `--lookup` 精准查询，避免交互式大范围探索。
  - 合理设置过滤项以缩小解析与输出规模。
- 可维护性：保持函数职责单一、命名清晰；为核心数据结构与流程补充简明注释。
- 可扩展性：为未来的并行解析与导出格式预留接口层（例如独立的导出模块）。
