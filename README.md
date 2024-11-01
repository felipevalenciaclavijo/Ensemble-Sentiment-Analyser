# Improving Simple Sentiment Analysers (e.g. VADER and TextBlob) with Heuristics derived from statistical analysis and BERT-based outcomes

## Abstract

This project is part of a comprehensive initiative to optimise sentiment analysis for a customer experience (CX) web application. The primary aim is to evaluate various sentiment analysis models —VADER, TextBlob, a fine-tuned MultilingualBERT, and a fine-tuned DistilBERT— by assessing their performance on customer reviews and classifying sentiment scores into a 5-star system. We further developed two ensemble models, blending VADER and TextBlob predictions, to leverage each model’s strengths for enhanced accuracy.

The ensemble models are built with rule-based heuristics, designed after extensive analysis of the dataset and the individual model outputs, to refine sentiment scoring accuracy and capture nuanced feedback more effectively. By combining VADER’s and TextBlob's sentiment detection, the ensembles deliver more reliable and contextually rich results.

In evaluating these models, we’ve considered both sentiment accuracy and operational efficiency, measuring processing time and resource usage (RAM and CPU). These findings will guide the optimal model setup for the CX application, enabling businesses to quickly upload reviews and receive meaningful sentiment insights for action. Through this experiment, we’ve achieved a balance of precision, scalability, and speed, providing a solid foundation for customer feedback analysis in the web app.

## Data Analysis and Experiment Results

## Descriptive Statistics

| Metric                      | Count     | Mean    | Standard Deviation |
|-----------------------------|-----------|---------|---------------------|
| **reviews.rating**          | 9999.000  | 3.982   | 1.175               |
| **vader.sentiment**         | 9999.000  | 4.347   | 1.190               |
| **textblob.sentiment**      | 9999.000  | 3.711   | 0.659               |
| **distilbert.sentiment**    | 9999.000  | 4.147   | 1.336               |
| **multilingual.sentiment**   | 9999.000  | 3.747   | 1.284               |
| **ensemble_prediction**      | 9999.000  | 4.289   | 1.054               |
| **ensemble_prediction2**     | 9999.000  | 4.289   | 1.055               |

## Shapiro-Wilk Test for Normality

| Model               | Statistics | p-value | Distribution |
|---------------------|------------|---------|--------------|
| TextBlob            | 0.880      | 0.000   | Not Normal   |
| VADER               | 0.885      | 0.000   | Not Normal   |
| DistilBERT          | 0.878      | 0.000   | Not Normal   |
| MultilingualBERT    | 0.860      | 0.000   | Not Normal   |
| Ensemble            | 0.887      | 0.000   | Not Normal   |
| Ensemble2           | 0.887      | 0.000   | Not Normal   |

## Paired T-Test Results

| Model               | t-statistic | p-value |
|---------------------|-------------|---------|
| TextBlob            | 26.688      | 0.000   |
| VADER               | -33.230     | 0.000   |
| DistilBERT          | -15.247     | 0.000   |
| MultilingualBERT    | 24.964      | 0.000   |
| Ensemble            | -30.748     | 0.000   |
| Ensemble2           | -30.725     | 0.000   |

## Wilcoxon Signed-Rank Test Results

| Model               | Statistic    | p-value |
|---------------------|--------------|---------|
| TextBlob            | 7549700.000  | 0.000   |
| VADER               | 3435417.000  | 0.000   |
| DistilBERT          | 5400486.000  | 0.000   |
| MultilingualBERT    | 3270391.500  | 0.000   |
| Ensemble            | 3613358.500  | 0.000   |
| Ensemble2           | 3613823.500  | 0.000   |

## Average Classification Time for 10,000 Reviews (CPU only, ~3.0GHz, ~14GB RAM)

| Model               | Average Time (minutes) |
|---------------------|------------------------|
| TextBlob            | 0.04                   |
| VADER               | 0.04                   |
| DistilBERT          | 11 - 20.76             |
| MultilingualBERT    | 26.37                  |
| Ensemble            | 0.04                   |
| Ensemble2           | 0.04                   |

## Summary of Model Performance

| Model                  | Accuracy       | Average Proximity | Remarks                                                              |
|------------------------|----------------|-------------------|----------------------------------------------------------------------|
| **TextBlob**           | 32.65%         | 0.8096            | Lower accuracy; higher average proximity indicates larger differences. |
| **VADER**              | 48.17%         | 0.7452            | Moderate accuracy; average proximity is better than TextBlob.        |
| **DistilBERT**         | 47.26%         | 0.7206            | Similar accuracy to VADER; closer proximity suggests better predictions. |
| **MultilingualBERT**   | 53.71%         | 0.5992            | Highest accuracy and best average proximity; most reliable model.    |
| **Ensemble**           | 48.79%         | 0.6782            | Improved accuracy over individual models; average proximity shows potential. |
| **Ensemble2**          | 48.80%         | 0.6779            | Slightly better than Ensemble; consistently good proximity.          |

*Note: In the Average Proximity results, smaller values indicate that the average difference between actual and predicted ratings is closer to the true ratings, which is desirable for accurate sentiment analysis.*


## Descriptive Comparison with reviews.rating

### TextBlob
- **Average Rating Comparison:** The average score for TextBlob sentiment (3.71) is close to the actual average rating (3.98). This model provides relatively consistent ratings, reflected in its low standard deviation (0.66), meaning it may not capture extreme sentiments as well as others.
- **Accuracy:** The accuracy of TextBlob is 32.65%, which is moderate but notably lower than other models.
- **Speed:** Processing time is very efficient at only 0.04 minutes for 10,000 reviews, making TextBlob a great choice when speed is critical.
- **Conclusion:** TextBlob is a reliable model for approximate sentiment analysis where low variability and high efficiency are valued. However, it may not capture the full range of sentiments as effectively as more advanced models.

### VADER
- **Average Rating Comparison:** VADER has a slightly higher average sentiment score (4.35) compared to reviews.rating, indicating a tendency to skew positively.
- **Accuracy:** At 48.17%, VADER has a relatively high accuracy, closely aligning with the reviews.rating benchmark and making it one of the better-performing individual models.
- **Speed:** Like TextBlob, VADER is highly efficient, taking only 0.04 minutes for classification.
- **Conclusion:** VADER balances speed with a reasonable degree of accuracy and is well-suited for quick sentiment analysis, especially when detecting positive sentiment trends.

### DistilBERT
- **Average Rating Comparison:** With an average sentiment score of 4.15, DistilBERT aligns fairly well with the actual ratings. However, its higher variability (standard deviation of 1.34) suggests it captures a broader range of sentiment nuances, which can be beneficial in complex language contexts.
- **Accuracy:** DistilBERT achieves an accuracy of 47.26%, close to VADER but with a more detailed sentiment range.
- **Speed:** At 11-20.76 minutes for classification, DistilBERT is significantly slower than VADER and TextBlob. This delay may be justified in applications requiring a deep understanding of sentiment.
- **Conclusion:** DistilBERT offers a solid trade-off between nuanced sentiment capture and processing time, ideal for contexts where precision is preferred over rapid throughput.

### MultilingualBERT
- **Average Rating Comparison:** MultilingualBERT has an average sentiment score of 3.75, aligning closely with the actual average rating. Its relatively high variability (1.28) indicates that it captures a diverse sentiment range.
- **Accuracy:** With an accuracy of 53.71%, MultilingualBERT outperforms other models in aligning with the actual ratings, suggesting it provides a superior match for complex sentiments.
- **Speed:** It is the slowest model, requiring around 26.37 minutes to process 10,000 reviews.
- **Conclusion:** MultilingualBERT is the most accurate model, ideal for applications needing fine-grained sentiment analysis. However, its processing time makes it more suitable for batch analysis rather than real-time use.

### Ensemble Models (Ensemble and Ensemble2)
- **Average Rating Comparison:** Both ensemble models show an average sentiment rating of approximately 4.29, which is slightly higher than reviews.rating but provides a balanced approach by incorporating various models’ insights.
- **Accuracy:** Both ensembles perform similarly, with an accuracy around 48.8%, falling between VADER and MultilingualBERT.
- **Speed:** Like TextBlob and VADER, the ensemble models are highly efficient, taking only 0.04 minutes to classify all reviews.
- **Conclusion:** The ensemble models provide a strong balance of speed and moderate accuracy, leveraging the strengths of individual models while maintaining fast processing times. They are ideal for situations where a holistic sentiment assessment is needed quickly.


## Overall Best Outcomes:

| Model                  | Average Sentiment | Alignment with Actual Rating | Accuracy       | Speed (10,000 reviews) | Average Proximity | Recommended Use Case                      |
|------------------------|-------------------|------------------------------|----------------|-------------------------|-------------------|-------------------------------------------|
| **TextBlob**           | 3.71              | Moderate                     | 32.65%         | 0.04 minutes            | 0.8096            | High-speed analysis, low variability      |
| **VADER**              | 4.35              | High                         | 48.17%         | 0.04 minutes            | 0.7452            | Quick analysis, positive sentiment detection |
| **DistilBERT (67M parameters)**         | 4.15              | Moderate                     | 47.26%         | 11-20.76 minutes        | 0.7206            | Complex language understanding            |
| **MultilingualBERT (110-125M parameters)**   | 3.75             | Very High                    | 53.71%         | 26.37 minutes           | 0.5992            | High accuracy, detailed sentiment         |
| **Ensemble**           | 4.29              | High                         | 48.79%         | 0.04 minutes            | 0.6782            | Balanced, fast sentiment overview         |
| **Ensemble2**          | 4.29              | High                         | 48.80%         | 0.04 minutes            | 0.6779            | Balanced, fast sentiment overview         |

*Note: In the Average Proximity results, smaller values indicate that the average difference between actual and predicted ratings is closer to the true ratings, which is desirable for accurate sentiment analysis.*


- **Best for Accuracy and Proximity:** MultilingualBERT (53.71%)
- **Best for Speed:** TextBlob, VADER, and Ensemble Models (0.04 minutes)
- **Best Balance:** Ensemble models provide a good mix of speed and accuracy for large-scale sentiment analysis.

This comparison provides insights into each model’s suitability based on accuracy, speed, and alignment with actual ratings, helping guide selection based on the specific needs of the analysis.

## An Analysis of the Future of Sentiment Analysis by ChatGPT:

In the current landscape of sentiment analysis, models like BERT, DistilBERT, and MultilingualBERT have indeed raised the bar for accuracy and understanding of nuanced language. However, there is still significant value in using simpler sentiment analysis tools like VADER and TextBlob for several reasons:

- **Speed and Efficiency:** The results from your analysis show that both VADER and TextBlob can process 10,000 reviews in just 0.04 minutes. This speed makes them ideal for real-time sentiment analysis, especially in scenarios where quick responses are critical, such as social media monitoring or customer support. In contrast, the transformer models like DistilBERT and MultilingualBERT take considerably longer (11-26 minutes) to process the same volume of data.

- **Resource Requirements:** Simpler models like VADER and TextBlob require significantly less computational power and memory compared to transformer models. In environments where resources are limited or cost-sensitive, these models can provide a practical solution without the need for heavy server infrastructure.

- **Simplicity and Interpretability:** VADER and TextBlob are easier to understand and implement. Their rule-based approaches allow users to see exactly how sentiment scores are derived. This transparency can be important for businesses looking to justify decisions based on sentiment analysis.

- **Good Enough Performance:** While the accuracy of models like MultilingualBERT is higher (53.71% compared to VADER's 48.17% and TextBlob's 32.65%), the alignment of simpler models with actual ratings is still acceptable for many applications. For instance, VADER achieved high alignment with actual ratings, making it suitable for use cases focused on positive sentiment detection.

- **Complementary Use:** There's an opportunity to use simpler models in conjunction with transformer models. For example, VADER and TextBlob could serve as initial filters to quickly gauge sentiment before more in-depth analysis with transformer models is applied. This layered approach can save resources while still providing nuanced insights.

## Conclusion

In summary, while transformer models offer advanced capabilities in sentiment analysis, simpler tools like VADER and TextBlob still have a relevant place in the field. Their speed, efficiency, and interpretability make them valuable for many practical applications. Organisations should consider the specific needs of their projects when selecting sentiment analysis tools, potentially combining multiple approaches to leverage the strengths of each. Based on the results, it seems that a hybrid strategy may be the most effective way forward, balancing speed and accuracy based on the context of use.