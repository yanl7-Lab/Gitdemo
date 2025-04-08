# TF-IDF 关键词提取算法

## 简介
TF-IDF（Term Frequency-Inverse Document Frequency）是一种用于评估词语在文档或语料库中重要性的统计方法。它通过结合词频（TF）和逆文档频率（IDF）来识别文档的关键词。

## 算法原理
- **TF（词频）**：衡量词在文档中的出现频率。  
  `TF(t) = (词t在文档中的出现次数) / (文档总词数)`  
- **IDF（逆文档频率）**：衡量词在语料库中的普遍重要性。  
  `IDF(t) = log[(总文档数) / (包含词t的文档数 + 1)]`  
- **TF-IDF**：综合TF与IDF，值越高表示词越重要。  
  `TF-IDF(t) = TF(t) * IDF(t)`

## 流程步骤
1. **预处理**  
   - 分词（中文使用分词工具，英文按空格切分）。
   - 过滤停用词（如“的”、“是”等无意义词）。
   - 可选：词干提取（英文）或小写化。

2. **计算TF**  
   统计每个词在**当前文档**中的频率。

3. **计算IDF**  
   统计每个词在**整个语料库**中出现的文档数。

4. **计算TF-IDF得分**  
   对每个词计算 `TF * IDF`，并排序取Top-N作为关键词。

## 实现逻辑（Python示例）
```python
import math
from collections import defaultdict

# 示例语料库（3个文档）
corpus = [
    "自然语言处理是人工智能的重要方向。",
    "深度学习推动了自然语言处理的发展。",
    "人工智能和深度学习正在改变世界。"
]

# 预处理：分词+去停用词（示例停用词）
stopwords = {"是", "的", "和", "正在", "了"}
doc_words = []
for doc in corpus:
    words = [word for word in jieba.cut(doc) if word not in stopwords]  # 中文分词需使用jieba
    doc_words.append(words)

# 统计文档总数和词频
total_docs = len(doc_words)
df = defaultdict(int)  # 包含词t的文档数
tf_dicts = []

for words in doc_words:
    tf = defaultdict(float)
    total_words = len(words)
    for word in words:
        tf[word] += 1 / total_words  # 计算TF
    tf_dicts.append(tf)
    for word in set(words):  # 每个词在每文档只计一次
        df[word] += 1

# 计算IDF
idf = defaultdict(float)
for word in df:
    idf[word] = math.log(total_docs / (df[word] + 1e-10))  # 防止除以0

# 提取单个文档的关键词（以第1个文档为例）
doc_id = 0
scores = {}
for word in tf_dicts[doc_id]:
    scores[word] = tf_dicts[doc_id][word] * idf[word]

# 输出Top-3关键词
keywords = sorted(scores.items(), key=lambda x: x[1], reverse=True)[:3]
print("关键词:", keywords)