# TF-IDF 关键词提取算法

## 简介
TF-IDF（Term Frequency-Inverse Document Frequency）是一种用于评估词语在文档或语料库中重要性的统计方法。它通过结合词频（TF）和逆文档频率（IDF）来识别文档的关键词。

## 算法原理
- **TF（词频）**：衡量词在文档中的出现频率。  
  TF(t) = (词t在文档中的出现次数) / (文档总词数)  
- **IDF（逆文档频率）**：衡量词在语料库中的普遍重要性。  
  IDF(t) = log[(总文档数) / (包含词t的文档数 + 1)] 
- **TF-IDF**：综合TF与IDF，值越高表示词越重要。  
  TF-IDF(t) = TF(t) * IDF(t)

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

# 邮件分类系统 - 基于多项式朴素贝叶斯分类器
## 算法基础
### 多项式朴素贝叶斯分类器
- **贝叶斯定理**：  
  \[
  P(Y|X) = \frac{P(X|Y) \cdot P(Y)}{P(X)}
  \]  
  其中，\( Y \) 是邮件类别，\( X \) 是特征向量。
- **条件独立性假设**：  
  假设特征之间在给定类别 \( Y \) 的条件下是相互独立的。
- **多项式分布**：  
  适用于文本分类任务，特征为词频分布。

## 数据处理流程
### 1. 分词处理
- 使用分词工具（如NLTK或jieba）将邮件文本分割为单词或短语。
### 2. 停用词过滤
- 去除无意义的高频词（如“的”、“是”、“和”等），以减少噪声数据。
### 3. 词频统计
- 使用 `CountVectorizer` 将文本转换为词频矩阵。
### 4. 数据划分
- 将数据集划分为训练集和测试集（通常为70%训练、30%测试）。

## 特征构建过程
### 高频词特征选择
- **数学表达**：选择在邮件中出现频率最高的单词作为特征。
- **实现差异**：简单直接，但可能忽略低频但重要的词汇。

### TF-IDF特征加权
- **数学表达**：  
  \[
  \text{TF-IDF}(t, d) = \text{TF}(t, d) \cdot \text{IDF}(t)
  \]  
  其中，\( \text{TF}(t, d) \) 是词 \( t \) 在文档 \( d \) 中的频率，\( \text{IDF}(t) \) 是词 \( t \) 在所有文档中的逆文档频率。
- **实现差异**：突出重要词汇，抑制高频但无意义的词汇。

### 特征对比
| 特征方法         | 优点                                  | 缺点                                  |
|------------------|---------------------------------------|---------------------------------------|
| 高频词特征选择   | 简单直接，计算效率高                  | 可能忽略低频但重要的词汇              |
| TF-IDF特征加权   | 突出重要词汇，抑制无意义词汇          | 计算复杂度较高                        |
