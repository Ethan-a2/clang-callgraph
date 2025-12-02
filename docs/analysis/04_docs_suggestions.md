# 文档建议

## Wiki 文档结构建议
1. 快速开始（Getting Started）
   - 安装依赖（系统 clang 与 Python 包）
   - pip 安装与 `poetry` 用法
   - 最小示例与常见参数
2. 使用指南（User Guide）
   - 单文件输入与 `compile_commands.json` 输入
   - 过滤选项：`-x`/`-p` 与 YAML 配置
   - 交互模式与 `--lookup`
3. 配置参考（Configuration Reference）
   - `callgraph.yml` 字段说明与示例
   - 平台差异与 clang 版本兼容性说明
4. 故障排查（Troubleshooting）
   - 诊断输出解读（`tu.diagnostics`）
   - 常见错误（缺失头路径、宏定义、标准版本不匹配）
   - 性能建议与大项目实践
5. 输出与集成（Outputs & Integration）
   - 终端输出格式说明
   - 未来/可选的 DOT、JSON 导出与下游工具链集成
6. 贡献指南（Contributing）
   - 开发环境搭建与代码风格
   - Issue 模板与 PR 流程
   - 测试策略（若引入）

## 详细设计文档关键章节
1. 背景与目标
   - 为什么需要调用图工具、现有方案对比与取舍
2. 架构与组件
   - 模块划分与职责说明（解析、遍历、过滤、输出）
   - 数据结构：CALLGRAPH、FULLNAMES 的格式与用途
3. 名称解析与过滤策略
   - `fully_qualified*` 的规则与边界情况（模板、重载、匿名命名空间）
   - 路径/前缀过滤的设计与用户配置入口
4. 解析流程与错误处理
   - libclang Index/TU 构建、诊断收集与错误恢复策略
5. 性能与可扩展性
   - 大型项目的解析成本、并行化与缓存思路
   - 输出控制策略与图规模管理
6. 安全与兼容性
   - 依赖版本矩阵、平台支持情况
   - 配置文件与命令行参数的安全注意事项（避免执行任意命令）
7. 演进路线与里程碑
   - 功能增强、格式支持、并行/缓存、UI/服务化等阶段性目标
