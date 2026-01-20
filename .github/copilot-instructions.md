# Happy-LLM 代码库 Copilot 指引

## 项目概览
- 这是教程型仓库，主体内容在 docs/ 各章节：每章含讲解 Markdown 与对应代码示例。
- 代码示例分散在 docs/chapter*/code 与 docs/chapter7/{Agent,RAG}，优先保持“章节—示例”对应关系。

## 关键结构与数据流
- 第六章训练实践使用 Transformers + DeepSpeed：脚本入口在 docs/chapter6/code/pretrain.py 与 docs/chapter6/code/finetune.py，参数示例见 docs/chapter6/code/pretrain.sh 与 docs/chapter6/code/finetune.sh。
- Agent 示例在 docs/chapter7/Agent/src：`Agent` 调用 OpenAI 兼容接口并基于 `function_to_json` 生成工具 schema；工具函数集中在 docs/chapter7/Agent/src/tools.py，demo 在 docs/chapter7/Agent/demo.py。
- RAG 示例在 docs/chapter7/RAG：
  - `ReadFiles` 从 ./data 读取 md/txt/pdf，基于 tiktoken 分块（见 docs/chapter7/RAG/utils.py）。
  - `VectorStore` 计算 embedding 并持久化到 storage/（JSON 文件）（见 docs/chapter7/RAG/VectorBase.py）。
  - `OpenAIChat` 用上下文模板拼接问题并调用模型（见 docs/chapter7/RAG/LLM.py）。

## 依赖与环境
- 依赖按章节拆分：例如 docs/chapter6/code/requirements.txt 与 docs/chapter7/{Agent,RAG}/requirements.txt。
- RAG 使用 dotenv 读取 `OPENAI_API_KEY` 与 `OPENAI_BASE_URL`（见 docs/chapter7/RAG/Embeddings.py 与 docs/chapter7/RAG/LLM.py）。
- Agent 示例在 demo 中显式配置 OpenAI 兼容 `base_url` 与 `api_key`（见 docs/chapter7/Agent/demo.py）。

## 常见开发流程（以现有脚本为准）
- 训练：优先参考 docs/chapter6/code/pretrain.sh 与 docs/chapter6/code/finetune.sh 的参数结构（DeepSpeed、bf16、gradient checkpointing、swanlab 记录）。
- RAG 数据准备：默认从 docs/chapter7/RAG/data 读取，向量库写入 storage/。

## 代码风格与约定
- 示例代码以可读性为主，函数/类名较直接；尽量保持示例最小可运行。
- 保持相对路径与目录结构不变（例如 ./data、storage/、autodl-tmp/），避免引入新的全局配置层。

## 修改建议
- 修改示例时同时更新对应章节 Markdown 说明，确保教程与代码一致。
- 新增依赖请写入对应章节的 requirements.txt，而非根目录统一管理。