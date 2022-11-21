---
layout: post
title: HIMARS in Ukraine
subtitle: Evaluating their impact on the Russia-Ukraine war since their deployment
# gh-repo: daattali/beautiful-jekyll
# gh-badge: [star, fork, follow]
tags: [Analysis, Visualization, Ukraine, war]
comments: false
---

Following months of staging and building up forces, Russia launched a full scale invasion of Ukraine on February 24 2022.  At the start of the invasion, Ukraine was significantly outmatched in terms of military resources.

| Fleet Type | Russia | Ukraine |
| :------ |:--- | :--- |
| Infantry Fighting Vehicle	| 17,677	| 1,117
| Main Battle Tank	| 17,370	| 368
| Armored Personnel Carrier	| 14,991	| 236
| Self-Propelled Artillery Systems	| 14,396	| 902
| Anti-Air Missiles	| 5,760	| -
| Mortar Systems	| 4,942	| 0
| Towed Artillery Systems	| 4,465	| 313
| Multiple Rocket Launch Systems	| 3,709	| -
| Tactical UAV	| 2,522	| -
| Mini UAV	| -	| 72
| Light Multirole Vehicle	| 1,483	| 144
| Others	| 5,303	| 127
| Multi-Role Aircraft	| -	| 126
| Training Aircraft	| -	| 60
| Armored Support Vehicle	| -	| 50

Many countries around the world were quick to pledge support for Ukraine, with the United States dwarfing financial and military contributions, totalling to date approximately [**$18.3 billion**](https://www.state.gov/u-s-security-cooperation-with-ukraine/)


In late May, President Volodymyr Zelensky [asked for HIMARS](https://edition.cnn.com/2022/05/26/politics/us-long-range-rockets-ukraine-mlrs/index.html) - an acronym for High Mobility Artillery Rocket Systems.

![HIMARS](https://upload.wikimedia.org/wikipedia/commons/8/83/HIMARS_-_missile_launched.jpg){: .mx-auto.d-block :}

These systems can fire and reload or be mobile within a few minutes, making them difficult to be hit by opposing artillary returning fire.  Zelensky acknowledged that these systems have [made a huge difference](https://www.telegraph.co.uk/world-news/2022/11/14/ukraine-war-latest-kherson-russia-news-nova-kakhovka-dnipro/) with Poland and other Baltic states also looking to bolster their military defence with them.

Total losses of equipment and personnel are [updated daily in publically available databases](https://www.kaggle.com/datasets/piterfm/2022-ukraine-russian-war)

After extracting the dataset into a dataframe, the daily change in losses can be calculated by looking at the difference in numerical columns.

~~~
df['daily_loss'] = df['personnel'].diff().fillna(df['personnel']).fillna(0).astype(int)
~~~

A moving 30 day average can then be calculated for specific types of equipment.



In Python, this produces a 30 day rolling average of tank losses.

~~~
y = df_num['tank'].rolling(window = 30).mean().dropna()
~~~

Using the Seaborn visualisation package, we can create a custom plot.

~~~
plt.figure(figsize=(8, 6))
sns.set_theme(style = 'darkgrid')
ax = sns.lineplot( x=np.arange(datetime(2022,3,15), datetime(2022,3,15) + pd.Timedelta(len(y), 'd'), timedelta(days=1)).astype(datetime), y = y);
ax.axvspan('2022-06-23', '2022-11-17', color = 'g', alpha = 0.5, zorder = 0);
~~~

The green shaded area being the time when Ukraine first took HIMARS systems into its possession from the USA.

![Tank Loss](/assets/img/HIMARSimg/tank_loss.png){: .mx-auto.d-block :}{: w="700" h="400" }
_Daily Tank Loss, with HIMARS possession highlighted_

Given that HIMARS is a mobile artillery unit, it's deployment in June is unlikely to correlate with Russian aerial or naval losses as it is intended for use against stationary ground targets.

This is what we would expect, as typical targets for HIMARS are used best against stationary ground targets.  Other preferred targets would include Armored Personnel Carriers (APC's) and Field Artillery

![APC Loss](/assets/img/HIMARSimg/APC_loss.png){: .mx-auto.d-block :}{: w="700" h="400" }
_Daily APC Loss_

![Field Artillery Loss](/assets/img/HIMARSimg/field_artillery.png){: .mx-auto.d-block :}{: w="700" h="400" }
_Daily Field Artillery Loss_

To further demonstrate that it is the HIMARS themselves that have made the impact, we can see that there has not been the same shift in daily losses against Russian assets that are not HIMARS targets.  Take this graph of the daily losses of aircraft for example:

![Aircraft Loss](/assets/img/HIMARSimg/aircraft_loss.png){: .mx-auto.d-block :}{: w="700" h="400" }
_Daily Aircraft Loss_

That there are only 16 HIMARS systems in Ukraine to service the entire 2,500 km frontline, their visible effect to shift the momentum of the war has been nothing short of impressive.
