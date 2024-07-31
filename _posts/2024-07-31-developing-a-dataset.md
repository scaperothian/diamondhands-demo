---
title: Developing a Dataset
date: 2024-07-31 00:00:00
description: Discussion on how and why we created our dataset.
image:
author_staff_member:
---
When devising our trading strategy, the team sought to understand what might be of relevance and interest, both in the pursuit of building a prediction model, and in the development of ancillary assets to assist in the development of learning, education and promotion of insights related to crypto. As such we consulted a number of theoretical sources, which included strategies and papers related to crypto in general, the underlying technology, crypto trading, crypto investing and crypto arbitrage (LINK TO DICTIONARY). In reviewing the literature it was apparent that considerable work was being devoted to the development, testing and implementation of strategies, however the inputs were manifesting into dramatically different outputs varying in beliefs, expectations, goals, strategies, implementation and execution.

In order to develop a dataset several key components were required with the most fundamental, we needed to identify a crypto currency (or pairing) with which to focus. Once the pairing was identified, we would then need to determine the exchanges where trades could be compared, monitored and executed. Several factors were considered in the decisioning processes, including; data availability, liquidity, volume, volatility, and platform equivalency. Starting with Data Availability, our foundational dataset and model requires a consistent, high quality and readily available data source. As we planned to capture and identify trader related behavioral components, the need for accurate, consistent and granular information was high. Next, we needed to insure that whatever markets we identified were liquid, in the sense that transactions continued to frequently occur, had real time prices which were reflective of prevailing market conditions, and when we ultimately deploy our model, we will be able to execute trades to capitalize on our strategy (specifically there are numerous platforms and exchanges were instantaneous transaction implementation is not possible). As supported in the literature, we identified that while asset volatility is required to support the continued availability and development of a effective market, that extreme price volatility is undesirable, both in the development of a long term valued asset (and stable currency), but most importantly, extreme volatility in the context of a arbitrage model can be catastrophic, presenting risk we do not seek to introduce. Lastly, equivalency is needed to ensure that the underlying platforms where trades will be executed are identical (or near identical). Differences in platforms (whether real or perceived) can result in pricing differences, which could artificially suggest the presence of arbitrage, but simply be the manifestation of operational risk or performance issues. As an example, in the past 10 years a number of previously well respected trading and storage platforms have failed for various operational, risk, fraud related reasons including; FTX (2022), Mt. Gox (2014),Bitfinex (2016),QuadrigaCX (2019),Cryptopia (2019),Celsius (2022),BlockFi (2022), and Zipmex (2022).

Ultimately the decision was made to focus on the currency pairing on Uniswap pools, and the specific currency pairing of WETH/USDC. The Uniswap platform was identified as the ideal platform as it is a transparent and open source, decentralized protocol. These attributes, specifically transparency and consistency remove the majority of counterparty risk associated with individual exchanges or companies. Additionally as it is decentralized and open source, it is widely accepted throughout the crypto community, resulting in familiarity, trust, and consistent performance. Additionally, as this is a standardized technical protocol, it is utilized across a wide range of currencies, pairs and pools, as such the foundational components of our data pipeline can be easily extended, allowing for leverage and scale in the pursuit of model expansion. WETH and USDC were chosen distinctly, with WETH being chosen for several desirable properties, and best in class technical innovations related to the Ethereum Blockchain which were not directly available in a number of peers. USDC was chosen specifically for its stability, USDC is a stable coin (which means based by US dollar currency on a 1:1 ratio) as such we are reducing operational risk to a single currency, and given its 1:1 value with the US dollar, it makes comparison pricing very easy to interpret and comprehend, which we believe a very desirable asset for new and beginning traders (as the complexity of the underlying technical is considerable). Additionally, as USDC is within the currency pairing, there remains an opportunity for an individual to liquidate their position at any point in time, to completely eliminate exposure related to holding a crypto asset.

Having identified the currency pairing and platform, the next activity was related to sourcing the data. Given the choice of dataset, there were dozens of sites which publish real, or near real time information related to these pools, and could serve as a great source. The group focused on a particular API which was available on a  [TheGraph.com](http://thegraph.com/), which provided robust transaction data via API at no cost (Please refer to Appendix \_\_ ). While this provided the necessary foundational information, through development a number of additional variables were identified as important (or potentially important requiring additional exploration) and a supplement source was identified from [Etherscan.com](http://etherscan.com/) which provided information related to gas limits and historical trader activity. It is noted that while the initial dataset was built on this API, it subsequently stopped working, as the host has taken it down, fortunately as per our requirements there are hundreds of viable alternatives, with the current model being serviced by \_\_\_\_\_\_\_\_ (available in Appendix).

Having now selected our strategy, and obtained our foundational data source, it was time for the initial EDA and feature engineering (please refer to the data dictionary for specific datum level analysis). The group started with a simple summary, aggregation and interpretation of the dataset, which included looking at Value Counts of top 5 occurrences of all variables, counting the total number of unique instances, missing, zero values, NAs, and statistical components for numeric variables (Mean, Median, Max, Min, Sum, Std). The observations are stored in the GIT Repo for all variables (LINK!!!!! Include comments related to Interpretation if can not be included in HTML). These observations led into the creation of a number of new elements, which were variants and transformations which we believed would perform better within the context of the model, specifically noting about some of the unique data formats which are common in their Ethereum ecosystem but not prevalent elsewhere (ie. Wei /Gwei ).

At this point we were presented with a unique challenge, specifically how to structure the model to enable comparison analysis between pools. While several approaches were considered, the datasets were merged on time, with an outer join, ensuring that all transactions were included within the dataset and the transactions from each unique pool would be opposite its closest comparable. Unfortunately as each pool is distinct there is not a natural primary key, as such many transactions are left without a comparable as hundreds of transactions can occur within a particular pool without a corresponding contra entry. In order to bridge this shortcoming and ensure that every transaction is relative to its most recent pool entry, we have forward filled the dataset, such that the previous transaction within the pool is available for comparison. While this approach resolved challenges for modeling, it created some unique feature engineering challenges, specifically transaction duplication. While the issue of duplication was easily handled with the creation of binary flags to enable aggregation and summarization of unique transactions, it is an area of caution for individuals exploring the data and expanding upon feature engineering.

It was at this point we were able to engineer the most critical features required to create a baseline prediction. Utilizing the now available peer pool pricing and fee data (\_\_\_\_, \_\_\_\_\_ Gas Used, Gas Price, and Uniswap Transaction Fees) we were able to create pool pricing differences at transaction execution time net of fees (percent change), and through extension, create the target variable, identifying whether arbitrage was possible at the time of the transaction (swap\_go\_nogo).

With a primary data set in place, the group sought to explore additional features which could be added into the model. As crypto investing is still in its infancy, consensus with respect to optimal feature selection is still hotly debated. Instead the group took a broader perspective and looked towards financial markets, where investors have been purchasing and selling financial assets for centuries. Technical analysis is a long-standing discipline which focuses on price, volume, frequency and directional indicators over time to predict movements in asset pricing, with thousands of generally accepted and utilized features utilized to predict stock, currency and commodity movements. Borrowing from this theory, we have engineered a series of activity based features which attempt to capture User, Pool, Daily and Market movements across varying periods of time. The rationale for inclusion is twofold, 1) capturing specifically what people know into a model is not practical, however capturing people's behavior in the form of their activity is relatively simple (and given financial motivation, how people might be the best proxy for what they know), 2) the best indication of how the market is likely to react to a particular circumstance, is likely similar to how it has acted in the past.