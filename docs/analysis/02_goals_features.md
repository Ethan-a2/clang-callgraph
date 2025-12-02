# 项目目标、演进与关键特性

## 项目目标与演进
- 短期目标：
  - 提供可靠的 C/C++ 调用图生成工具，支持通过 `compile_commands.json` 与单文件输入。
  - 支持路径/前缀过滤与额外 clang 参数，以适配不同工程环境。
  - 提供交互式查询与直接查找（`--lookup`）。
- 中期目标：
  - 增强对模板、重载与命名空间的处理准确度；支持跨 TU 的关联（在同一编译数据库中）。
  - 输出格式增加 DOT/Graphviz、JSON，以便与其它工具链集成。
  - 增加批量查询与脚本化模式（非交互）。
- 长期目标：
  - 处理大型代码库的性能优化（并行解析、增量缓存）。
  - 兼容更多 Clang 版本与平台；完善错误恢复与诊断报告。
  - 增加 UI 前端或 Web 服务封装以便团队使用。

## 核心组件、关键特性与核心功能
- 核心组件：
  - 参数/配置解析（`read_args`、`load_config_file`）
  - 编译数据库读取（`read_compile_commands`）
  - 解析与遍历（`analyze_source_files`、`show_info`）
  - 名称与过滤（`fully_qualified*`、`is_excluded`）
  - 输出与交互（`print_callgraph`、`print_calls`、`ask_and_print_callgraph`）
- 关键特性：
  - 通过 libclang 精确解析 AST，识别 `CALL_EXPR` 并建立调用图。
  - 过滤：基于文件路径与符号前缀隐藏非关注区域（如 `std::` 等）。
  - 名称分辨：同时维护唯一全名与易读显示名，匹配与输出更友好。
  - 交互模式与直接查找：支持 CLI 交互选择与 `--lookup` 直查。
- 核心算法与复杂度：
  - AST 遍历算法：对每个翻译单元进行深度优先遍历，收集调用边；
    - 时间复杂度：约 O(Σ|AST_nodes|)；在大型项目中接近线性但受诊断/解析耗时影响。
    - 空间复杂度：O(|V|+|E|)，其中 V 为函数/方法集合，E 为调用关系；另有名称索引与递归栈。
  - 打印调用链：DFS + 访问集（`so_far`）避免重复；深度上限为 15 防止爆炸。

## 关键用例
1. 针对小型项目单文件：
   - 命令：`clang-callgraph file.cpp -std=c++17 -Iinclude/ --lookup main`
   - 行为：解析该文件，打印 `main` 的调用图。
2. 针对 CMake 项目：
   - 命令：`clang-callgraph build/compile_commands.json -x std:: -p /usr/include`
   - 行为：读取编译数据库，过滤标准库与系统头路径，交互选择函数并打印调用图。
3. 调试诊断：
   - 当解析失败时，输出 `tu.diagnostics` 相关信息，基于 `-I`、`-D`、`-std=` 等参数修复。
