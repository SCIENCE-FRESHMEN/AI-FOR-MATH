#智能AI教学助手 后端API服务 - 完整版

## 🎉 最新更新

### 新增功能模块

1. **学习记忆存储系统** (`memory_storage.py`)
   - 完整的学习记录存储和查询
   - 自动初始化20条示例数据
   - 支持按标签和难度筛选
   - 自动统计薄弱知识点和已掌握知识点

2. **统计数据计算模块** (`statistics_calculator.py`)
   - 每周活动趋势统计
   - 知识点分布分析
   - 难度分布统计
   - 成功率趋势计算

3. **每日一题推荐系统** (`daily_recommender.py`)
   - 智能推荐算法（支持3种策略）
   - 7道高质量题目池
   - 基于薄弱知识点的个性化推荐
   - 完整的历史记录和统计

## 快速开始

### 1. 安装依赖

```bash
cd backend
pip install -r requirements.txt
```

### 2. 配置环境变量

复制 `.env.example` 为 `.env` 并填入你的API Key：

```bash
cp .env.example .env
```

编辑 `.env` 文件：
```
LLM_API_KEY=your_deepseek_api_key
LLM_BASE_URL=https://api.deepseek.com/v1
LLM_MODEL=deepseek-chat
```

### 3. 启动服务

```bash
python app.py
```

服务将在 `http://localhost:8000` 启动

### 4. 查看API文档

访问 `http://localhost:8000/docs` 查看自动生成的API文档

## API端点详解

### 1. POST /solve - 智能解题

**请求体**:
```json
{
  "question": "题目内容"
}
```

**响应**:
```json
{
  "success": true,
  "answer": "最终答案",
  "steps": [
    {"type": "reasoning", "content": "推理内容"},
    {"type": "calculation", "content": "计算说明", "code": "Python代码"}
  ],
  "statistics": {
    "total_steps": 8,
    "reasoning_steps": 5,
    "calculation_steps": 3,
    "time_used": "45秒"
  }
}
```

**新功能**: 解题成功后自动保存到学习记忆

---

### 2. POST /generate - 生成题目

**请求体**:
```json
{
  "difficulty_level": "中等",
  "problem_type": "函数",
  "topic_keywords": ["导数", "不等式"],
  "requirements": "需要参数讨论"
}
```

**响应**:
```json
{
  "success": true,
  "problem": "题目文本",
  "latex": "LaTeX源码",
  "evaluation": {
    "overall_score": 8.5,
    "originality_score": 9.0,
    "solvability_score": 8.0,
    "complexity_score": 8.5,
    "knowledge_coverage_score": 9.0,
    "educational_value_score": 8.5
  },
  "validation": {
    "success": true,
    "answer": "标准答案"
  },
  "iterations": 1
}
```

---

### 3. GET /statistics - 统计数据 ✨ 新增真实数据

**响应**:
```json
{
  "weekly_data": [
    {"day": "周一", "solved": 3, "generated": 2},
    ...
  ],
  "knowledge_data": [
    {"name": "函数", "value": 15, "color": "#3b82f6"},
    ...
  ],
  "difficulty_data": [
    {"name": "简单", "value": 20, "color": "#10b981"},
    ...
  ],
  "progress_data": [
    {"month": "1月", "rate": 65},
    ...
  ],
  "success_rate": 0.76,
  "weekly_count": 34,
  "mastered_topics": 12,
  "streak_days": 15
}
```

**数据来源**: 基于学习记忆自动计算

---

### 4. GET /memory - 学习记录 ✨ 新增真实数据

**查询参数**:
- `tag` (可选): 按标签筛选，如 "函数"、"导数"
- `difficulty` (可选): 按难度筛选，如 "简单"、"中等"、"困难"

**响应**:
```json
{
  "total": 20,
  "success_rate": 0.85,
  "weak_points": ["立体几何", "复数"],
  "mastered_points": ["函数", "导数", "三角函数"],
  "records": [
    {
      "id": 1,
      "question": "求函数 f(x) = x³ - 3x + 1 在区间 [-2, 2] 上的最大值和最小值",
      "answer": "最大值为 3，最小值为 -1",
      "tags": ["函数", "导数"],
      "difficulty": "中等",
      "success": true,
      "steps": 8,
      "timestamp": "2025-01-20 14:30:25",
      "time_used": "45秒"
    },
    ...
  ]
}
```

**初始数据**: 包含20条示例学习记录

---

### 5. GET /daily - 每日一题 ✨ 新增智能推荐

**查询参数**:
- `strategy` (可选): 推荐策略
  - `balanced` (默认): 平衡模式，70%薄弱知识点，30%复习
  - `weak`: 专注薄弱知识点
  - `random`: 随机推荐

**响应**:
```json
{
  "date": "2025-01-20",
  "question": "已知函数 f(x) = ln(x) - ax...",
  "tags": ["函数", "导数", "不等式"],
  "difficulty": "困难",
  "source": "系统推荐",
  "strategy": "balanced",
  "answer": "完整答案",
  "hint": "解题提示",
  "streak": 3,
  "total_completed": 20,
  "success_rate": 0.75,
  "history": [
    {"date": "2025-01-19", "completed": true, "success": true},
    ...
  ]
}
```

**题目池**: 包含7道高质量题目，涵盖多个知识点

---

### 6. POST /daily/submit - 提交答案

**请求体**:
```json
{
  "questionId": 1,
  "answer": "答案内容"
}
```

**响应**:
```json
{
  "success": true,
  "correct": true,
  "feedback": "答案正确"
}
```

---

### 7. POST /plot/execute - 执行Python代码

**请求体**:
```json
{
  "code": "import matplotlib.pyplot as plt..."
}
```

**响应**:
```json
{
  "success": true,
  "image": "data:image/png;base64,..."
}
```

---

### 8. POST /plot/generate - AI生成绘图代码

**请求体**:
```json
{
  "description": "绘制一个圆和一条直线"
}
```

**响应**:
```json
{
  "success": true,
  "code": "完整的Python代码",
  "explanation": "代码说明"
}
```

---

## 数据存储

### 学习记忆数据

**存储位置**: `backend/data/memory_records.json`

**数据结构**:
```json
{
  "records": [
    {
      "id": 1,
      "question": "题目内容",
      "answer": "答案",
      "tags": ["标签1", "标签2"],
      "difficulty": "难度",
      "success": true,
      "steps": 8,
      "timestamp": "2025-01-20 14:30:25",
      "time_used": "45秒"
    }
  ]
}
```

**初始化**: 首次启动时自动创建20条示例数据

**自动更新**: 每次解题成功后自动添加记录

---

## 项目结构

```
backend/
├── app.py                      # FastAPI主应用（已更新）
├── config.py                   # 配置文件
├── llm_client.py              # LLM调用客户端
├── python_executor.py         # Python代码执行器
├── prompts.py                 # Prompt模板
├── memory_storage.py          # ✨ 学习记忆存储模块（新增）
├── statistics_calculator.py   # ✨ 统计数据计算模块（新增）
├── daily_recommender.py       # ✨ 每日一题推荐模块（新增）
├── requirements.txt           # Python依赖
├── .env.example              # 环境变量示例
├── data/                     # ✨ 数据存储目录（新增）
│   └── memory_records.json   # 学习记忆数据
└── README.md                 # 本文档
```

---

## 核心功能实现

### 1. 学习记忆系统

**功能特性**:
- ✅ JSON文件持久化存储
- ✅ 自动初始化示例数据（20条）
- ✅ 支持按标签和难度筛选
- ✅ 自动统计薄弱知识点（成功率<60%）
- ✅ 自动统计已掌握知识点（成功率≥80%）
- ✅ 解题成功后自动保存记录

**使用示例**:
```python
from memory_storage import memory_storage

# 添加记录
record_id = memory_storage.add_record({
    "question": "题目",
    "answer": "答案",
    "tags": ["函数"],
    "difficulty": "中等",
    "success": True,
    "steps": 5,
    "time_used": "30秒"
})

# 查询记录
records = memory_storage.get_records(tag="函数", difficulty="中等")

# 获取统计
stats = memory_storage.get_statistics()
```

---

### 2. 统计数据计算

**功能特性**:
- ✅ 每周活动趋势（解题数、生成数）
- ✅ 知识点分布（饼图数据）
- ✅ 难度分布（饼图数据）
- ✅ 成功率趋势（折线图数据）
- ✅ 综合统计（成功率、本周解题数、掌握知识点数、连续天数）

**使用示例**:
```python
from statistics_calculator import statistics_calculator

# 获取所有统计数据
all_stats = statistics_calculator.get_all_statistics()

# 获取单项数据
weekly_data = statistics_calculator.get_weekly_data()
knowledge_data = statistics_calculator.get_knowledge_data()
```

---

### 3. 每日一题推荐

**功能特性**:
- ✅ 7道高质量题目池
- ✅ 3种推荐策略（balanced/weak/random）
- ✅ 基于薄弱知识点的智能推荐
- ✅ 完整的题目信息（题目、答案、提示）
- ✅ 学习统计（连续天数、完成数、成功率）
- ✅ 7天历史记录

**题目池内容**:
1. 函数与导数综合题（困难）
2. 数列递推与求和（中等）
3. 三角函数与解三角形（中等）
4. 椭圆与直线综合（困难）
5. 函数极值与方程（中等）
6. 立体几何与空间向量（困难）
7. 等差数列与裂项求和（中等）

**使用示例**:
```python
from daily_recommender import daily_recommender

# 获取每日一题（平衡模式）
question = daily_recommender.get_daily_question(strategy="balanced")

# 获取每日一题（薄弱知识点模式）
question = daily_recommender.get_daily_question(strategy="weak")
```

---

## 注意事项

1. **数据持久化**: 学习记录保存在 `data/memory_records.json`，请勿删除
2. **API Key安全**: 不要将 `.env` 文件提交到版本控制
3. **CORS配置**: 已配置允许前端跨域访问
4. **代码执行安全**: Python执行器有安全限制，禁止危险操作
5. **超时设置**: LLM调用默认超时60秒
6. **错误处理**: 所有API都有完整的错误处理机制

---

## 开发建议

- 使用 `uvicorn app:app --reload` 启动开发模式
- 查看 `/docs` 端点测试API
- 查看日志排查问题
- 根据需要调整 `config.py` 中的配置
- 定期备份 `data/memory_records.json` 文件

---

## 与前端的完整对接

### 前端页面 → 后端接口映射

| 前端页面 | 后端接口 | 数据状态 |
|---------|---------|---------|
| SolveProblem | POST /solve | ✅ 真实数据 + 自动保存 |
| GenerateQuestion | POST /generate | ✅ 真实数据 |
| LearningMemory | GET /memory | ✅ 真实数据（20条示例） |
| Statistics | GET /statistics | ✅ 真实数据（基于记忆计算） |
| DailyQuestion | GET /daily | ✅ 真实数据（智能推荐） |
| DailyQuestion | POST /daily/submit | ✅ 正常工作 |
| GeometryPlot | POST /plot/execute | ✅ 真实数据 |
| GeometryPlot | POST /plot/generate | ✅ 真实数据 |

---

## 更新日志

### v1.1.0 (2025-01-20)

**新增功能**:
- ✨ 学习记忆存储系统（memory_storage.py）
- ✨ 统计数据计算模块（statistics_calculator.py）
- ✨ 每日一题推荐系统（daily_recommender.py）
- ✨ 20条示例学习记录
- ✨ 7道高质量题目池
- ✨ 解题成功自动保存记录

**改进功能**:
- 🔧 /statistics 接口返回真实统计数据
- 🔧 /memory 接口返回真实学习记录
- 🔧 /daily 接口实现智能推荐算法
- 🔧 /solve 接口自动保存解题记录

**修复问题**:
- 🐛 修复 /daily/submit 接口参数问题

---

## 技术栈

- **FastAPI**: 现代化的Python Web框架
- **Pydantic**: 数据验证和设置管理
- **OpenAI SDK**: LLM调用
- **Matplotlib**: 图形绘制
- **JSON**: 数据持久化

---

## 联系方式

如有问题或建议，请联系开发团队。
