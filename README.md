# Assignment 2 - Evaluating Hawkishness and Dovishness of FOMC Communication

### Ziyi Wang (zw3917)

## 1. Introduction

In this project, we explore the sentiment of the Federal Open Market Committee (FOMC) communications during 2018-2019 and 2024-2025, focusing on Meeting Minutes, Fed speeches, and Press Conference transcripts, with a specific focus on tariff.

By utilizing FOMC-RoBERTa, fine-tuned model for FOMC hawkish-dovish-neutral classification task introduced in a ACL 2023 paper, we assess the tone of these statements, calculating the sentiment polarity score and categorizing them into "Hawkishness" and "Dovishness" based on the text in each document.

With the oveall and tariff-focused sentiment polarity score, we plot some pictures to compare the sentiment in a FOMC text and the 2-year/10-year yield spread to see how the market reacted to the FOMC text.

## 2. Data Collection

During working on this project, we collect data fro the government to form a corpus of the Federal Open Market Committee (FOMC) communications, especially FOMC minutes, press conference and FED speeches. We use Python to crawl data from https://www.federalreserve.gov/, and remove the html form or other unrelated things and save the data together with the clean text.

To analyze the market reaction, we are adviced to use the Fed Funds futures. As it is not easy to find the data, I use the 2-year/10-year yield spread from https://fred.stlouisfed.org. This spread reflects market expectations about future economic conditions and monetary policy. When the spread narrows or inverts, it often signals investor concern about economic slowdown or potential rate cuts. Because Federal Reserve communications, including FOMC meeting minutes, speeches, and press conferences play a central role in shaping market expectations about interest rates, analyzing how the 2-year/10-year spread responds to these texts provides a valuable lens for evaluating the real-time impact of the Fed’s guidance on market sentiment and perceived policy direction.

## 3. Model Selection

Instead of FinBERT, the model introduced in class, we use FOMC-RoBERTa to do the sentiment analysis in this project.

FOMC-RoBERTa is a domain-specific transformer-based language model developed by Gao et al. (2023) and introduced in their SSRN working paper “FOMC-RoBERTa: A Domain-Specific Language Model for FOMC Texts.” Unlike general-purpose financial models such as FinBERT, FOMC-RoBERTa is pre-trained and fine-tuned exclusively on Federal Reserve communications—including FOMC statements, meeting minutes, speeches, and press conferences—allowing it to capture the nuanced and context-specific language used in monetary policy discourse.

This tailored training gives FOMC-RoBERTa a clear advantage in evaluating the sentiment, tone, and latent signals embedded in Fed communications, especially when distinguishing between hawkish and dovish language. For a task that seeks to quantify how the FOMC addresses topics like tariffs and assess the subsequent reaction in financial markets, such specialization is crucial. Empirical comparisons in the original study demonstrate that FOMC-RoBERTa significantly outperforms FinBERT and other general finance NLP models in sentiment classification and forward-return predictability, making it particularly well-suited for our analysis of monetary policy tone and its effect on yield spreads.

## 4. Implementation Steps

To implement the work in this project, we followed several steps as introduced below:

1. Collect all the minutes, press conference and speech data during 2018-2019 and 2024-2025. To prepare for later analysis, we also collect the 2-year yield and 10-year yield data from https://fred.stlouisfed.org. With these data we can calculate the 2-year/10-year spread according to its definition.
2. Clean the text, extract the date and main content from the data collected and save them into a file.
3. For each article, split the text up into small segments, and calculate the sentiment polarity score by using the FOMC-RoBERTa model. We can get the percentage of Hawkish segments and Dovish segments from the result, and compute a weighted average polarity score for each document by taking the sum of label scores weighted by their predicted probabilities across all segments. This score reflects the overall monetary policy stance conveyed in the text.
4. Besides the overall sentiment score, we also create a wordlist containing words and phrases that are related to tariff. By using this wordlist, we try to filter the segments to get ones that are related to tariff, and calculated the tariff-related sentiment polarity score using these segments for each article.
5. Finally, we analysis the result by drawing some pictures to compare the yield spread and the sentiment score.

## 5. Analysis

We can classify the pictures we draw into three kinds, and analysis them separately. During our analysis, a positive polarity score in your model indicates a hawkish tone, while a negative score represents a dovish tone. The yield spread used is 2-Year minus 10-Year Treasury yield, which is a well-known indicator of market expectations for economic growth and monetary policy trajectory. A narrowing or negative spread signals expected economic slowdown or tighter monetary policy.

#### Correlation

![overall_sentment_vs_spread](https://github.com/user-attachments/assets/33e9e69a-6678-4730-8755-1921261e73e8)

Across both sample periods (2018–2019 and 2024–2025), we observe a light negative correlation between the FOMC sentiment polarity scores and the 2-year/10-year Treasury yield spread. In our setup, a higher polarity score indicates a more hawkish tone, which aligns with expectations: when the Federal Reserve signals tightening (e.g., rate hikes), short-term yields (2Y) rise more than long-term yields (10Y), causing the spread to narrow or even invert.

The scatter plots clearly depict this inverse relationship. Blue (2018–2019) points cluster in the upper-left quadrant (hawkish → higher spreads), while orange (2024–2025) points cluster lower, consistent with more negative spreads and higher hawkishness.

Scatterplots for FOMC minutes, press conferences, and Fed speeches confirm this pattern, with more hawkish points (right side of the x-axis) generally associated with lower or negative spreads (lower y-axis values). The negative slope is more visually prominent in 2024–2025, indicating stronger market sensitivity to hawkish signals in the post-pandemic tightening cycle.

Besides, from all the scatterplots from the two period, we can see the sentiment scores are overall higher for 2024-2025, which indicates a more hawkish trend in FOMC communications in 2024-2025, compared to 2018-2019. This aligns with the policy trends sensed by us.

#### Trend

![overall_sentiment_spread_changing_1819](https://github.com/user-attachments/assets/732d60ba-c551-47f1-a257-3ecdf7d140d6)
![overall_sentiment_spread_changing_2425](https://github.com/user-attachments/assets/52442d7d-62c3-460a-bef5-86dd1c63661d)


Figure 2&3, the temporal plots showing the overall sentiments of each communication, capturing how yield spreads evolve following sentiment events offer deeper insight into market responsiveness.

* **2018–2019:** Hawkish sentiment scores from FOMC show a moderate negative correlation with the 2y/10y spread. Hawkishness is generally followed by narrowing spreads.
* **2024–2025:** The correlation remains negative but with higher volatility. Especially in Fed speeches, hawkish sentiment tends to precede a more significant drop in spreads, often into negative territory. This suggests a market that is more sensitive or reactive to policy tone.

![tariff_sentiment_spread_changing_1819](https://github.com/user-attachments/assets/90fcf569-4000-420a-92ef-4bbf642e90ee) 
![tariff_sentiment_spread_changing_2425](https://github.com/user-attachments/assets/52f6cef9-5170-4bee-8ceb-98ffcbb8469d)

Figure  shows the sentiment of tariff-related segments and how the yield spread changes overtime. As the segments are selected ones, we can see how the financial market reacts to the tariff-related sentiment.

- **2018–2019:** Tariff mentions were more frequently dovish, signaling concerns about growth risks from trade policy. As shown in both scatter and timeline plots, dovish tariff-related communications often led to modest increases in the 2y/10y spread, consistent with a market interpreting these as rate-cut supportive.
- **2024–2025:** In contrast, tariff-related sentiment becomes hawkishly framed, often tied to inflationary implications of trade restrictions. Hawkish tariff sentiment is followed by spread narrowing or deeper inversions, indicating the market now sees tariffs as potentially rate-hike justified.

#### Detailed reaction after each speech

![tariff_sentiment_vs_spread](https://github.com/user-attachments/assets/c30efe6c-cae3-4a0e-b877-841e628625af)

![spread_after_speech_1819](https://github.com/user-attachments/assets/2676226b-fbcb-4b43-911b-d0803f0883f0)
![spread_after_minutes_2425](https://github.com/user-attachments/assets/3822fb39-b055-46e7-bab3-912f01ec311b)


Specific events highlight how market expectations adjust around sentiment changes. To show the detailed change before and after a specific communication, we selected the texts (minutes with polarity >0.5 for 2024-2025; speeches with polarity >0.1 for 2018-2019) with a more significant tariff-related polarity score among all the texts.

From the result, we can see that though some specific reactions are not as expected, generally a more hawkish (positive) communication would lead to a drop or slower incline in the spread, while a more dovish (negative) one would lead to a rise. The timing plots support the view that  2024–2025 speeches trigger sharper, faster spread movements, whereas in 2018–2019, reactions are smoother and less volatile.

For example, on June 12, 2024, a highly hawkish tariff-related message (score = 1.00) was followed by a significant drop in spread from –0.40% to –0.46% within 2 days. On March 28, 2019, a strongly dovish tariff sentiment (score = –0.99) coincided with a modest increase in spread, consistent with easing expectations.

#### Possible reasons to explain the unsual changes

As there are some unexpected market reaction after some of the communications, some possible reasons can be given to explain.

As there may be some rumors, in some cases market already priced In the sentiment. If market participants anticipated the hawkish or dovish message in advance via economic indicators, prior Fed comments, or leaks—then the yield spread may have already adjusted before the speech. In such cases, even a highly hawkish sentiment score may not cause further steepening of the curve, or may even reverse on “confirmation.”

Although we have chose a good model, there can be mixed or ambiguous messaging in the communication. Fed speeches or FOMC communications often contain mixed signals. A hawkish statement on inflation could be paired with dovish comments on labor markets or financial conditions. The sentiment model may pick up on the dominant tone, but markets could focus on a different aspect, leading to a muted or opposite reaction.


## 6. Conclusion

This project combines advanced NLP techniques with fixed income market data to investigate how financial markets respond to monetary policy communications.

The overall analysis confirmed expectations: hawkish sentiment from the Fed—especially in recent years—is increasingly associated with a drop in the yield spread, consistent with economic slowdown expectations and potential policy over-tightening.

Our analysis reveals several consistent and compelling patterns:

- There is a clear negative correlation between the hawkishness of Fed communications and the 2y/10y yield spread. Hawkish tones are typically followed by a narrowing spread or deeper curve inversions, suggesting markets anticipate higher short-term rates in response to tighter policy stances.

- Compared to 2018–2019, communications in 2024–2025 elicit stronger and more immediate reactions. Overall there's a more hawkish trend in 2024-2025. Yield spreads adjust more sharply following hawkish events, especially Fed speeches, reflecting heightened uncertainty in a post-pandemic, inflation-sensitive economy.
- The tone of tariff-related content has also evolved. While such mentions in 2018–2019 were mostly dovish—framed as downside risks to growth—more recent references in 2024–2025 are often hawkish, linked to inflationary pressures. The market’s reaction aligns with this shift, with yield spreads responding more aggressively to hawkish tariff sentiment in the later period.

Looking ahead, potential improvements include refining the sentiment model to better capture topic-specific polarity, using intraday yield data to enhance event timing resolution, and incorporating additional financial indicators ike the Fed Future fund or using news sentiment for a more holistic market response model.
