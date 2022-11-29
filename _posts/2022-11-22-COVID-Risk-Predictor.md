---
layout: post
title: COVID Risk Predictor
subtitle: Using Machine Learning to predict if patients are at risk
tags: [Analysis, Visualization, Machine Learning, COVID, Health]
comments: false
---


![COVID](https://images.unsplash.com/photo-1583324113626-70df0f4deaab?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1932&q=80)
The COVID-19 Pandemic put unseen pressures on healthcare systems worldwide, and led to a substantial loss of life.

Treatment of the sick varied across the world, with some pictures emerging of families fighting for oxygen tanks and corridor space in hospitals.  Healthcare providers faced challenging ethical decisions on how to prioritise treatment with limited resources.

To complicate the picture further, in the early stages of the pandemic, testing for COVID was not always reliable.

The Mexican government released a dataset of over 1 million patients over the early stages of the pandemic.  This data can be used to generate models which predict if a patient is likely to be at risk - which I define as any combination of dying, admittance to the ICU, or needing to be intubed.

Individual probabilities can be calculated to see how certain the algorithm is that its patient classification is correct.  These probabilities can be used to prioritize patients if there were a shortage of beds or other hospital resources.

The full notebook can be seen [here](/COVID_risk_predictor.html).
