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
 **预处理**  
   - 分词（中文使用分词工具，英文按空格切分）。
   - 过滤停用词（如“的”、“是”等无意义词）。
   - 可选：词干提取（英文）或小写化。
 **计算TF**  
   统计每个词在**当前文档**中的频率。
 **计算IDF**  
   统计每个词在**整个语料库**中出现的文档数。
 **计算TF-IDF得分**  
   对每个词计算 `TF * IDF`，并排序取Top-N作为关键词。
# 代码核心功能说明
## get_words() 函数：
```python
def get_words(filename):
    words = []
    with open(filename, 'r', encoding='utf-8') as fr:
        for line in fr:
            line = line.strip()
            line = re.sub(r'[.【】0-9、——。，！~\*]', '', line)
            line = cut(line)
            line = filter(lambda word: len(word) > 1, line)
            words.extend(line)
    return words
all_words = []
该函数从一个文本文件中读取文本并进行处理，读取文本：通过 open() 打开文件并按行读取，去除无效字符：使用正则表达式 re.sub() 来去除常见的无效字符（如数字、标点符号等），分词：利用 jieba.cut() 对每一行文本进行分词处理。
