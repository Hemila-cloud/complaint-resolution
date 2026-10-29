# Consumer Complaint Resolution Recommendation System

## Project Overview

The **Consumer Complaint Resolution Recommendation System** is an NLP-based framework designed to recommend possible resolution categories for new consumer complaints by analyzing similar historical complaints and their recorded company responses.

Traditional complaint classification systems primarily identify the category of a complaint. This project goes a step further by retrieving similar past complaints, comparing their textual meaning and structured attributes, and using historical resolution outcomes to generate ranked recommendations.

The framework combines Sentence-BERT embeddings, FAISS similarity search, structured contextual features, hybrid ranking, historical response aggregation, and explainability to support evidence-based complaint resolution.

## Problem Statement

Consumer complaint databases contain large volumes of historical complaints and company responses. However, identifying relevant past cases and understanding their recorded outcomes can be challenging.

Traditional classification methods categorize complaints but do not necessarily recommend resolution pathways based on similar historical cases. Text-only retrieval may also overlook important structured information, such as the product, issue, and company associated with a complaint.

Therefore, this project proposes a context-aware recommendation framework that combines semantic similarity and structured complaint information to retrieve relevant historical cases and recommend possible resolution categories.

## Objectives

- To preprocess and analyze consumer complaint narratives.
- To represent complaint text using transformer-based semantic embeddings.
- To retrieve semantically similar historical complaints using FAISS.
- To incorporate structured complaint attributes into contextual similarity calculation.
- To combine semantic and contextual similarity through hybrid ranking.
- To aggregate historical company response outcomes and generate ranked resolution recommendations.
- To provide supporting historical cases and insights into the recommendation process.
- To evaluate the framework against traditional classification and retrieval baselines.

## Dataset

The project uses the **Consumer Financial Protection Bureau (CFPB) Consumer Complaint Database**.

Dataset source: https://www.consumerfinance.gov/data-research/consumer-complaints/

The dataset contains complaint narratives and structured information, including:

- Product
- Sub-product
- Issue
- Sub-issue
- Company
- State
- Timely response
- Company response

The project processes complaint narratives and relevant structured attributes to support historical complaint retrieval and resolution recommendation.

## Methodology

### 1. Data Collection and Preprocessing

Consumer complaint records are collected from the CFPB database. The data is cleaned by handling missing values, removing duplicate records, filtering unsuitable narratives, and processing the complaint text. Relevant structured attributes are prepared for contextual comparison.

### 2. Feature Representation

The system represents complaints using textual and structured information.

**TF-IDF:** Converts complaint text into sparse numerical vectors and serves as a traditional baseline.

**Sentence-BERT:** Uses the `all-MiniLM-L6-v2` model to convert complaint narratives into dense semantic embeddings of 384 dimensions. These embeddings capture similarities in meaning, even when complaints use different words.

**Structured features:** Represent complaint attributes such as product, sub-product, issue, sub-issue, company, and timely response.

### 3. Semantic Retrieval Using FAISS

The historical complaint embeddings are indexed using FAISS (Facebook AI Similarity Search). When a new complaint is submitted, its embedding is compared with the indexed historical embeddings to retrieve the Top-10 semantically similar complaints.

Cosine similarity is used to measure the similarity between complaint embeddings.

### 4. Context-Aware Similarity

The system compares the structured attributes of the new complaint with those of the retrieved historical complaints.

The contextual similarity score is calculated using the following weights:

| Attribute | Weight |
|---|---:|
| Product | 0.25 |
| Sub-product | 0.15 |
| Issue | 0.25 |
| Sub-issue | 0.20 |
| Company | 0.10 |
| Timely response | 0.05 |

These weights give greater importance to product and issue information while also considering other complaint attributes.

### 5. Hybrid Ranking

Semantic and contextual similarity scores are combined to calculate a final hybrid score.

**Hybrid ranking equation:**

`H = 0.70 × S + 0.30 × C`

Where:

- `H` = Hybrid similarity score
- `S` = Semantic similarity score
- `C` = Contextual similarity score

The semantic component receives a weight of 70%, while the contextual component receives a weight of 30%. The retrieved complaints are ranked according to their hybrid scores.

### 6. Historical Response Aggregation

After ranking the similar complaints, the system extracts their recorded company response outcomes. The resolution categories considered include:

- Closed with explanation
- Closed with monetary relief
- Closed with non-monetary relief

The hybrid scores of complaints belonging to each category are aggregated. The resulting scores are used to rank resolution categories and generate recommendations based on historical evidence.

These scores represent relative historical support, not calibrated probabilities of future resolution.

### 7. Explainability

The framework provides supporting evidence for its recommendations through ranked historical complaints, their similarity scores, and information about the semantic and contextual components.

Feature-contribution analysis can help explain how the components influence the hybrid ranking. Any specific explainability technique, such as SHAP, should be reported only when it is implemented in the project code.

### 8. Evaluation

The system is evaluated using traditional classification and information retrieval baselines.

**Classification metrics:**
- Accuracy
- Precision
- Recall
- F1-score

**Retrieval metrics:**
- Precision@10
- Recall@10
- Mean Reciprocal Rank (MRR)
- Normalized Discounted Cumulative Gain at 10 (nDCG@10)

These metrics help assess classification performance and the relevance and ranking quality of retrieved complaints.

## Novelty and Contributions

### 1. Intelligent Resolution Recommendation

The framework goes beyond complaint classification by using historical company response outcomes to recommend possible resolution categories for a new complaint.

### 2. Retrieval-Augmented Complaint Analysis

The system retrieves similar historical complaints and uses their recorded outcomes as supporting evidence for resolution recommendations.

### 3. Hybrid Complaint Representation

The framework combines transformer-based semantic understanding with structured contextual attributes. This allows historical cases to be ranked using both textual meaning and complaint-specific information.

### 4. Evidence-Based Recommendations

The recommended resolution categories are supported by retrieved historical complaints and aggregated response scores, improving the transparency of the recommendation process.

## Technologies Used

- **Programming Language:** Python
- **Data Processing:** Pandas, NumPy
- **Natural Language Processing:** Sentence-BERT
- **Embedding Model:** `all-MiniLM-L6-v2`
- **Similarity Search:** FAISS
- **Text Representation Baseline:** TF-IDF
- **Machine Learning Baselines:** Logistic Regression, Linear SVM, Random Forest
- **Evaluation:** Scikit-learn and information retrieval metrics
- **Development Environment:** Jupyter Notebook / Google Colab

## Reported Experimental Results

The following results are from the reported retrieval baseline comparison.

| Method | Precision@10 | Recall@10 | MRR | nDCG@10 |
|---|---:|---:|---:|---:|
| TF-IDF + Cosine Similarity | 0.5610 | 0.0289 | 0.8150 | 0.8329 |
| Sentence-BERT + Cosine Similarity | 0.6060 | 0.0288 | 0.8833 | 0.8733 |
| Proposed Hybrid Method | 0.8910 | 0.0709 | 0.9701 | 0.9661 |

The reported baseline comparison shows improvements in Precision@10, MRR, and nDCG@10 for the proposed hybrid method relative to the listed baselines.

*Note: These values belong to the reported retrieval comparison. Results from different evaluation setups should be interpreted separately.*

## Expected Output

For a new consumer complaint, the system produces:

1. A ranked list of similar historical complaints.
2. Semantic, contextual, and hybrid similarity scores.
3. Historical company response categories associated with retrieved complaints.
4. Ranked resolution recommendations based on aggregated historical evidence.
5. Supporting cases and information that helps explain the recommendations.

## Limitations

- Recommendations depend on the quality, coverage, and diversity of historical complaint records.
- Imbalanced resolution categories may affect the recommendations.
- Historical outcomes do not guarantee the resolution of a new complaint.
- The quality of retrieval depends on the embedding model, contextual features, ranking weights, and evaluation criteria.
- Further evaluation using larger datasets, independent relevance criteria, and real-world feedback is needed to establish generalizability.

## Future Enhancements

- Evaluate the framework on larger and more diverse complaint datasets.
- Investigate different semantic-to-contextual weighting configurations.
- Improve handling of underrepresented resolution categories.
- Explore domain-specific transformer models for financial complaint narratives.
- Extend the explanation module with more detailed evidence for individual recommendations.
- Evaluate recommendation usefulness through user feedback and additional real-world complaint scenarios.

## Conclusion

This project presents a hybrid, context-aware approach to consumer complaint resolution recommendation. By combining Sentence-BERT semantic embeddings, FAISS retrieval, structured contextual similarity, and historical response aggregation, the system uses previous complaints to support ranked resolution recommendations. The framework aims to extend complaint analysis beyond classification and provide transparent, evidence-based assistance for understanding possible resolution pathways.

## References

1. Consumer Financial Protection Bureau. Consumer Complaint Database. https://www.consumerfinance.gov/data-research/consumer-complaints/
2. Sentence Transformers documentation. https://www.sbert.net/
3. FAISS documentation. https://faiss.ai/
4. Scikit-learn documentation. https://scikit-learn.org/
