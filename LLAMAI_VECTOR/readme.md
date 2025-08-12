运行顺序：
1、构建知识库：python init_kb

本地embedding_model = BAAI--bge-large-zh-v1.5 

生成本地知识库： ./LLAMAI_VECTOR\data\faiss_index

2、转换：python LLAMAI_VECTOR/modules/retrieval/build_bm25_from_faiss.py
生成bm25检索库：.\LLAMAI_VECTOR\data\bm25_index

3、接口启动：python app.py

host="192.168.0.192", port=8080


4、调用接口传参

'''
接口传参：
{
  "query": "API网关",  ///用户请求
  "filters": {"device_model": "unknown", "is_public": true},   ///技术筛选
  "top_k": 5  ///返回重排序结果
}

返回：

{
  "results": [ ... ]
}

doc_id	string	文档在索引里的唯一编号
text	string	原始正文（支持多段换行）
tech_tag	list[string]	技术/业务标签，示例为 ["unknown"]
device_model	string	设备型号，示例为 "unknown"
is_public	bool	是否公开
score	float	向量检索给出的原始分数（值域较大，示例 22.53）
rerank_score	float	重排序模型给出的置信度（0–1，示例 0.997）
merged_score	float	融合后的最终排序分（值域 0–1，示例 0.242）




'''





.\LLAMAI_VECTOR\modules\retrieval\hybrid_search.py  修改self.embedder为本地embedding
.\LLAMAI_VECTOR\modules\retrieval\reranker.py        修改__init__(model_name)为本地reranker