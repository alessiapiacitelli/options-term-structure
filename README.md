# Econometrics Final Project "Options Term Structure with the Principal Component Analysis"

**Alessia Piacitelli, Abhinav Mangat**  
New York University  
ECON-UB 251: ECONOMETRICS  
Professor Sebastiano Manzan  
May 14th, 2024

---

## Abstract

This project investigates the options term structure for the National Stock Exchange of India using Principal Component Analysis (PCA). The term structure of options refers to the relationship between the prices of options and their maturities, providing crucial insights into market expectations and risk assessments. By analysing the clustering of options by industry and examining volatility patterns relative to days until expiry, we aim to uncover the latent structures influencing option pricing and investment strategies. We divided our dataset into estimation and testing segments and performed industry-specific analyses to reveal unique informational structures within each sector. Subsequently, we develop and test a trading strategy based on these components, focusing on sectors such as Technology & Telecommunications, Financial Services, and Manufacturing & Engineering. Our findings suggest that specific industries exhibit distinct patterns that can be leveraged to predict option prices and inform trading decisions. However, the model's predictive power is somewhat limited by data challenges and noise. This study underscores the potential of PCA in financial market analysis and provides a foundation for future research to enhance predictive accuracy and trading strategies.

---

## Introduction

The landscape of financial markets is continually shaped by the dynamic interplay of myriad factors ranging from macroeconomic policies to investor psychology. In this project, we perform the study of the options term structure for the National Stock Exchange of India. In recent years, the Indian National Stock Exchange has emerged as a powerhouse for options trading, surpassing even the most prominent global financial markets. Remarkably, they accounted for over 78% of the total options trading volume in 2023, solidifying India's position as a dominant player in the derivatives landscape (Mandavia, 2024). This rapid growth in options trading volume underscores the importance of understanding the underlying structures and factors influencing this market.

![Option Trading Volume, Worldwide (in $bn)](alessiapiacitelli/options-term-structure/mages/intro.png)

---

## Data

For this study, data collection posed a significant challenge in terms of processing and accessibility. Initially, we had access to daily data spanning the 2000-2020 period for Futures and Options, totaling over 112 million observations. To overcome processing limitations, we adopted a strategy of lazy loading and chunk processing, leveraging the multiprocessing package in Python. Focusing specifically on options, we narrowed our scope to include only OPTIDX and OPTSTK instruments, ensuring comparability in scale and aggregating effects. This refinement allowed us to reduce our dataset to 1,021,254 observations spanning the period from 2019-11-25 to 2020-05-25.

![Data Snapshot](path/to/data_snapshot.png)

At this juncture, the limitations posed by the data primarily revolved around its size and frequency. While initially, the extensive size and high frequency of the data appeared promising for our study, they presented significant challenges. The sheer size of the dataset undermined our ability to obtain a clear and transparent understanding of the underlying patterns. Moreover, the high frequency introduced considerable noise into the data, complicating our analysis. Furthermore, conducting additional exercises with such a dataset would have necessitated data rolling, which proved to be a challenge given the aforementioned limitations.

---

## Exploratory Data Analysis

Initially, through various graphical representations, we aim to convey not only the statistical outputs of our analyses but also the underlying patterns and insights that emerge from our data. This section will guide through a series of visualisations, each designed to highlight different aspects of market behaviour and the effects of economic variables. The insights gleaned from these analyses inform the subsequent sections of our study, particularly in identifying which industries offer the most explanatory power in the PCA and regression analyses.

![Industry Distribution Analysis](path/to/industry_distribution_analysis.png)

The bar chart showcases the number of contracts per industry, revealing significant variation in market participation across sectors. The Technology & Telecommunications sector dominates with the highest number of contracts, exceeding 250,000. This is followed by the Manufacturing & Engineering and Financial Services sectors, each with substantial contract counts, highlighting their critical roles in the market. Conversely, industries such as Media, Entertainment, and Chemicals display minimal contract activity, indicating less engagement or lower market interest in these sectors. This disparity in contract distribution underscores the varying levels of investor confidence and market activity across industries. High contract volumes in sectors like Technology & Telecommunications may reflect rapid innovation and growth, attracting more traders. In contrast, lower volumes in sectors such as Media and Chemicals suggest either market saturation or lesser perceived profitability. Understanding these patterns is crucial for developing targeted trading strategies, as it helps identify sectors with high liquidity and potential for better returns.

![Volatility Time Series](path/to/volatility_time_series.png)

The volatility time series revealed that there isn't a consistent trend indicating that volatility uniformly increases or decreases during any particular day or month. However, the data reveals significant fluctuations in market volatility, particularly during the peak of the COVID-19 pandemic. During the early months of 2020, average daily volatility remained relatively low and stable. However, starting in March 2020, coinciding with the global escalation of the COVID-19 crisis, volatility surged dramatically. This period marked by heightened uncertainty and rapid changes in market conditions saw average daily ranges exceeding 140, indicating extreme market reactions and investor behaviour driven by pandemic-related news and economic impacts.

![Volatility Analysis](path/to/volatility_analysis.png)

The graph indicates a high degree of volatility in options with very few days to expiry, peaking dramatically at several points. Notably, there are significant volatility spikes at approximately 0, 100, 200, and 300 days to expiry. These peaks suggest that certain intervals before expiration are associated with heightened market activity and uncertainty. Volatility tends to be lower and more stable in the intermediate periods between these spikes, indicating periods of relative market calm. The reasons for these volatility peaks could be manifold, including the release of market-moving news, earnings reports, or other economic events that coincide with these intervals. Understanding these volatility patterns is crucial for traders and investors. The spikes near expiration dates highlight the increased risk and potential for rapid price movements as contracts near their expiry, necessitating more vigilant risk management and strategic planning. These insights also underscore the importance of considering the time to expiry as a critical factor in options trading strategies, as it significantly influences volatility and, consequently, the pricing and risk of options contracts.

![Volatility Analysis, Financial Sector](path/to/volatility_analysis_financial_sector.png)

Because we aim to uncover specific patterns and detailed insights, we conducted industry-specific analysis, delving into the unique volatility dynamics within different sectors. The first graph illustrates the average daily volatility against the days to expiry for options in the Financial Services sector. Notably, there is a significant peak in volatility around 20 days to expiry, where the average daily volatility rises to approximately 18. This suggests a period of heightened market uncertainty and increased trading activity as options approach this critical threshold. Volatility then sharply declines after this peak but remains elevated until around 40 days to expiry, after which it stabilises at lower levels. The pattern indicates that as options near their expiry, market participants in the Financial Services sector may engage in more aggressive trading or hedging activities, leading to increased price fluctuations.

![Volatility Analysis, Media Sector](path/to/volatility_analysis_media_sector.png)

The second graph focuses on the Media sector, showing the average daily volatility against the days to expiry. Similar to the Financial Services sector, a distinct peak in volatility is observed around 20 days to expiry, reaching approximately 6. This spike suggests increased trading activity or market reactions to specific events as options approach this expiration window. After this peak, volatility decreases steadily, with occasional minor spikes, and stabilises at lower levels as the expiry date approaches. The volatility pattern in the Media sector indicates that significant price movements are concentrated in the period leading up to the 20-day mark, potentially driven by sector-specific news or market sentiment shifts.

![Moving Average](path/to/moving_average.png)

The scatter plot above depicts the 20-day moving average of closing prices against the days to expiry for options contracts. The data points reveal an intriguing pattern in how the moving average of closing prices evolves as options approach their expiration dates. It appears that the moving average tends to stabilise as the days to expiry increase. In the initial periods, especially within the first 40 days, there is significant fluctuation and higher variability in the moving average of closing prices. This high volatility during the early stages could be attributed to market participants' speculative activities and rapid reactions to market news or events.

As the days to expiry extend beyond 40 days, the moving average shows a tendency to become more stable and less volatile. This stabilisation might suggest a correlation between the days to expiry and the moving average closing prices, indicating that as options approach their expiration, market prices settle into more predictable patterns. This could be due to decreased uncertainty and reduced speculative trading as the expiration date approaches, leading to a convergence towards the intrinsic value of the options.

---

## Static Principal Component Analysis

In this section, the goal is to explore the informational content of the dynamics of futures term structure over a wide range of industries taking into account trading maturities up to 12 months. For this, we have first standardised:
- Strike price
- Closing price
- Settlement price
- Traded volume
- Open interest
- Average traded price
- Number of contracts

### Technology & Telecommunications Sector

**Percentage of Variance by Component**  
![Percentage of Variance by Component, Tech Sector](path/to/variance_by_component_tech.png)

This graph depicts the explained variance by each principal component in the Technology & Telecommunications sector. The first principal component explains a significant proportion of the variance, underscoring its dominance in capturing the main trends within the data. Subsequent components, while contributing less to the overall variance, still hold value in explaining more nuanced variations in the dataset.

**First Principal Component Loadings**  
![First Principal Component Loadings, Tech Sector](path/to/component_loadings_tech.png)

The loadings of the first principal component highlight the most influential variables in the Technology & Telecommunications sector. A higher loading indicates a stronger contribution of the variable to the component. In this sector, certain variables such as closing price and traded volume show substantial loadings, reflecting their significant impact on the principal component. This suggests that these variables are critical in explaining the overall variance and trends in the Technology & Telecommunications options market.

### Financial Services Sector

**Percentage of Variance by Component**  
![Percentage of Variance by Component, Financial Sector](path/to/variance_by_component_financial.png)

The graph for the Financial Services sector shows a similar pattern to the Technology & Telecommunications sector, with the first principal component explaining a substantial portion of the variance. This indicates that a single principal component can capture a major part of the information in the dataset, simplifying the complexity of the data while retaining most of its informational content.

**First Principal Component Loadings**  
![First Principal Component Loadings, Financial Sector](path/to/component_loadings_financial.png)

In the Financial Services sector, the loadings of the first principal component highlight the predominant variables that contribute to the overall variance. The closing price and average traded price emerge as significant variables, indicating their critical roles in shaping the principal component and, by extension, the underlying patterns in this sector's options market.

---

## Factor Analysis

Next, we constructed four factors to determine the sources of term structure variation in each sector. We aim to identify the most significant factors influencing the term structure and their respective loadings. The factors constructed include:
- Market Level (Factor 1)
- Risk Premia (Factor 2)
- Liquidity (Factor 3)
- Market Sentiment (Factor 4)

### Factor Analysis Results

**Factor Loadings for Technology & Telecommunications Sector**  
![Factor Loadings, Tech Sector](path/to/factor_loadings_tech.png)

The factor loadings for the Technology & Telecommunications sector reveal that the Market Level factor has the highest loading, indicating that general market conditions significantly influence this sector. The Risk Premia and Liquidity factors also show substantial loadings, reflecting the importance of risk considerations and market liquidity in this sector. The Market Sentiment factor, while still relevant, has a lower loading compared to the other factors, suggesting that investor sentiment plays a relatively smaller role in explaining term structure variations in this sector.

**Factor Loadings for Financial Services Sector**  
![Factor Loadings, Financial Sector](path/to/factor_loadings_financial.png)

In the Financial Services sector, the factor loadings highlight the Market Level and Risk Premia factors as the most influential, similar to the Technology & Telecommunications sector. However, the Liquidity factor shows a lower loading, indicating that liquidity considerations might be less critical in this sector compared to the others. The Market Sentiment factor has a moderate loading, suggesting that investor sentiment plays a more significant role in this sector's term structure variations than in the Technology & Telecommunications sector.

---

## Conclusion

Our study reveals that the first principal component in each sector explains a significant proportion of the variance in the options data, with key variables such as closing price and traded volume having substantial loadings. This suggests that these variables are crucial in understanding the underlying patterns in the options market. Additionally, our factor analysis identifies Market Level and Risk Premia as the most significant factors influencing term structure variations across different sectors. These insights can inform trading strategies and risk management practices, highlighting the importance of considering sector-specific dynamics in options trading.

## References

Mandavia, M. (2024). *India Tops Derivatives Trading Volume Worldwide*. [The Times of India](https://timesofindia.indiatimes.com/business/india-business/india-tops-derivatives-trading-volume-worldwide/articleshow/86543019.cms).

---

## Acknowledgements

We would like to extend our gratitude to Professor Sebastiano Manzan for his invaluable guidance and support throughout this project. We also appreciate the assistance provided by our classmates and the resources offered by New York University.

---

## Appendices

### Appendix A: Data Processing Code

```python
import pandas as pd

# Sample data processing code
data = pd.read_csv('options_data.csv')
processed_data = data.dropna().reset_index(drop=True)
processed_data.to_csv('processed_options_data.csv', index=False)
