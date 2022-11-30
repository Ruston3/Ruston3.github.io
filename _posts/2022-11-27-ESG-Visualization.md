---
layout: post
title: ESG Analytics
subtitle:
tags: [Analysis, Visualization]
comments: false
---

In 2006, the United Nations published a report titled the Principles for Responsible Investment.  Environmental, Social, and Governance sustainability factors were included in financial evaluations.

Personally, this field is highly interesting, as it provides a means to some degree to rein in unethical growth and development in venture capital and private markets.

From Kaggle, I have come into possession of a list of 2018 ESG ratings from over 15,000 companies worldwide in the following format.

|    | Company Name              | Country   | Sector                            | Subsector                           | Overall ESG RATING   |   Overall ESG SCORE |   Environmental SCORE |   Social SCORE |   Governance SCORE |
|---:|:-------------------------|---------:|:----------|:----------------------------------|:------------------------------------|:---------------------|--------------------:|----------------------:|---------------:|-------------------:|
|  0 | 0921706 BC LTD         | CA        | Telecommunication Services        | Wireless Telecommunication Services | A                    |                 6.6 |                  10   |            7.4 |                2.7 |
|  1 | 1 MADISON OFFICE FEE LLC  | US        | Real Estate Management & Services | Office REITs                        | BBB                  |                 4.7 |                   8.5 |            5.8 |                2.6 |

Using [Pandas](https://pandas.pydata.org/), [Plotly](https://plotly.com/), and [Python](https://www.python.org/), I will perform some elementary analysis and visualization of the dataset with the aim of being able to extract some insights.

With Plotly, one can fairly quickly generate visuals based off a subsection of the dataset.  The below is a 3 dimensional plot of 40 US/UK Real estate companies of their three respective scores, where each point represents a company.  The companies are colour coded according to their subsector within Real Estate.

```python
fig = px.scatter_3d(data_frame = dfeng[dfeng['Sector'] == 'Real Estate Development & Diversified Activities'],
              x = "Environmental SCORE",
              y = "Social SCORE",
              z = "Governance SCORE",
              hover_name = 'Company Name',
              color = 'Subsector')
```

<iframe src="/pages/RealEstate.html" style="width: 800px; height: 600px; border: 3px"></iframe>


Additionally, we can easily create an interactive sunburst diagram to showcase the Sectors and Subsectors of all companies considered in the dataset.  Clicking on a Sector will expand its view for other subsectors for easier viewing.

<iframe src="/pages/Sunburst.html" style="width: 800px; height: 600px; border: 3px"></iframe>

We could then extract highlights of the dataset to see our highest and lowest performers using boolean masking to reduce as needed.

The below code looks first for the highest average score across the three metrics in each sector, and if there are still multiple companies in each sector, takes the one with the highest overall score.

```python
df['Avg Score'] = (df['Environmental SCORE'] + df['Social SCORE'] + df['Governance SCORE'])/3
idx = df.groupby(['Sector'])['Avg Score'].transform(max) == df['Avg Score']
dfmax = df[idx].sort_values(by = 'Sector')
edx = dfmax.groupby(['Sector'])['Overall ESG SCORE'].transform(max) == dfmax['Overall ESG SCORE']
dfmax = dfmax[edx].sort_values(by = 'Sector')
dfmax.sort_values(by="Avg Score", ascending = False).head(10)
```

This table is a list of the top 10 companies, each with a unique sector.

| Rank   | Company Name                               | Country   | Sector                                                  | Subsector                             | Overall ESG RATING   |   Overall ESG SCORE |   Environmental SCORE |   Social SCORE |   Governance SCORE |   Avg Score |
|---:|:-------------------------------------------|:----------|:--------------------------------------------------------|:--------------------------------------|:---------------------|--------------------:|----------------------:|---------------:|-------------------:|------------:|
| 1 | GUIMARANIA I SOLAR SPE SA                  | BR        | Utilities                                               | Gas Utilities                         | AAA                  |                10   |                  10   |            9   |                7.4 |     8.8     |
| 2 | GIBSON ENERGY INC                          | CA        | Oil & Gas Refining, Marketing, Transportation & Storage | Oil & Gas Storage & Transportation    | AAA                  |                10   |                   8.6 |           10   |                7.3 |     8.63333 |
| 3 | WORLEY US FINANCE SUB LIMITED              | US        | Energy Equipment & Services                             | Oil & Gas Equipment & Services        | AAA                  |                10   |                   9.6 |           10   |                6.1 |     8.56667 |
| 4 | SYDNEY AIRPORT FINANCE COMPANY PTY LIMITED | AU        | Transportation Infrastructure                           | Airport Services                      | AAA                  |                10   |                  10   |            9.1 |                6.6 |     8.56667 |
| 5 | BRAMBLES FINANCE PLC                       | AU        | Commercial Services & Supplies                          | Diversified Support Services          | AAA                  |                10   |                   9.7 |            8.4 |                7.3 |     8.46667 |
| 6 | SIMS LIMITED                               | US        | Steel                                                   | Steel                                 | AAA                  |                10   |                   8.6 |            8.9 |                7.7 |     8.4     |
| 7 | INTERNATIONAL FINANCE CORPN LTD            | IN        | Supranationals & Development Banks                      | nan                                   | AAA                  |                10   |                   8.2 |            8.5 |                8.4 |     8.36667 |
| 8 | RELX INC                                   | US        | Professional Services                                   | Research & Consulting Services        | AAA                  |                10   |                  10   |            7.7 |                7.3 |     8.33333 |
| 9 | Telia Company AB                           | SE        | Telecommunication Services                              | Integrated Telecommunication Services | AAA                  |                10   |                  10   |            7.5 |                7.4 |     8.3     |
| 10 | JUPITER FUND MANAGEMENT PLC                | GB        | Asset Management & Custody Banks                        | Asset Management & Custody Banks      | AAA                  |                 9.7 |                  10   |            5.9 |                8.6 |     8.16667 |


A table such as this could prove useful to the sustainably conscious investor, who may prefer to invest with individual stocks over ESG funds.  ESG based investing [continues to trend upwards.](https://www.nerdwallet.com/article/investing/best-esg-funds)


Finally, analysis could be performed on another axis or column.  For example, one could look at the average ESG performance of companies in a country as a tool to measure Government efforts to promote sustainable business.  The following is the average ESG score of companies in the dataset belowing to Great Britain, the United states, Canada and Saudi Arabia respectively:

```python
df[df['Country'] == 'GB']['Overall ESG SCORE'].mean()
df[df['Country'] == 'US']['Overall ESG SCORE'].mean()
df[df['Country'] == 'CA']['Overall ESG SCORE'].mean()
df[df['Country'] == 'SA']['Overall ESG SCORE'].mean()
```

| Country | Number of Companies | Average ESG Score (mean) |
|---:|---:---|:----------------------|
| GB | 948 | 6.541645 |
| US | 4684 | 5.104169 |
| CA | 497 | 5.781488 |
| SA | 87 | 2.258620 |
