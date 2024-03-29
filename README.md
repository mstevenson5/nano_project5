# nano_project5
Martin Stevenson

18th Feb 2021

## Project Introduction
In a Stroop task, participants are presented with a list of words, with each word displayed in a color of ink. The participant’s task is to say out loud the color of the ink in which the word is printed. The task has two conditions: a congruent words condition, and an incongruent words condition. In the congruent words condition, the words being displayed are color words whose names match the colors in which they are printed: for example RED, BLUE. In the incongruent words condition, the words displayed are color words whose names do not match the colors in which they are printed: for example PURPLE, ORANGE. In each case, we measure the time it takes to name the ink colors in equally-sized lists. Each participant will go through and record a time from each condition.

## Question 1:
### What is our independent variable? What is our dependent variable?
* Independent: Word sets, both congruent and incongruent words
* Dependent: Time taken to read each word set.

## Question 2:
### What is an appropriate set of hypotheses for this task? What kind of statistical test do you expect to perform? Justify your choices.

Test the existence of the Stroop Effect, where the time taken to read the incongruent word set is greater when compared to the time taken to read the congruent word set. If the mean time taken between the congruent and incongruent word sets is significantly different then we have observed the Stoop Effect. 
* Null Hypothesis: μ<sub>C</sub> - μ<sub>I</sub>  ≥ 0
* Alternative Hypothesis: μ<sub>C</sub> - μ<sub>I</sub> < 0

Where: μ<sub>C</sub> = Mean Congruent, μ<sub>I</sub> = Mean Incongruent

The Null Hypothesis is that the mean congruent time taken is more than that of the mean incongruent time taken.

As the time is recorded for each wordset per particpant a **Paired T-test** can be used. This is perferred over using Z-scores due to the limited number of records available within the dataset i.e. less than 30.

### My Test:
* Congruent Set: 12.404s
* Incongruent Set: 23.092s

## Question 3:
### Report some descriptive statistics regarding this dataset. Include at least one measure of central tendency and at least one measure of variability.

```python
%pylab inline
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
from scipy import stats

raw_data = pd.read_csv('stroopdata.csv')
raw_data.describe()
```

|      | Congruent  | Incongruent|
|------|------------|------------|
|count | 24.000000  | 24.000000  |
|mean  | 14.051125  | 22.015917  |
|std   | 3.559358   | 4.797057   |
|min   | 8.630000   | 15.687000  |
|25%   | 11.895250  | 18.716750  |
|50%   | 14.356500  | 21.017500  |
|75%   | 16.200750  | 24.051500  |
|max   | 22.328000  | 35.255000  |

The above description of the data set provides some initial insights with regards to the means and standard deviation of the word sets.

## Question 4:
### Provide one or two visualizations that show the distribution of the sample data. Write one or two sentences noting what you observe about the plot or plots.

```python
x_axis = range(0, raw_data.shape[0])
plt.bar(x_axis, raw_data.Congruent)
plt.bar(x_axis, raw_data.Incongruent, bottom = raw_data.Congruent)
plt.show()
```

![plot](https://github.com/mstevenson5/nano_project5/blob/main/stacked_bar.png) 

```python
raw_data.boxplot()
plt.xlabel("Word Set")
plt.ylabel("Time Taken (s)")
plt.title('Boxplot of time taken per word set')
plt.show()
```

![plot](https://github.com/mstevenson5/nano_project5/blob/main/box_plot.png)

```python
raw_data.plot.hist( bins=15, rwidth=0.9)
plt.title('Time taken to read wordsets')
plt.xlabel('Time Taken')
plt.ylabel('Count')
```

![plot](https://github.com/mstevenson5/nano_project5/blob/main/hist.png)

### Observations
* From the stacked bar chart it is clear that the Incongruent wordset takes longer to read for each person in the population than the Congruent wordset.
* The box plot and histogram confirm this finding and clearly show that the Incongruent wordset takes longer to read.

## Question 5:
### Now, perform the statistical test and report your results. What is your confidence level and your critical statistic value? Do you reject the null hypothesis or fail to reject it? Come to a conclusion in terms of the experiment task. Did the results match up with your expectations?

Paired T-Test

```python
mean_congruent = np.mean(raw_data.Congruent)
mean_incongruent = np.mean(raw_data.Incongruent)

sdev_congruent = np.std(raw_data.Congruent, ddof=1)
sdev_incongruent = np.std(raw_data.Incongruent, ddof=1)

num_samples = raw_data.shape[0]
df = num_samples - 1

diff = np.subtract(raw_data.Congruent,raw_data.Incongruent)
mean_diff = np.mean(diff)
sdev_diff = np.std(diff, ddof=1)

stderr = np.sqrt(((sdev_congruent ** 2) / num_samples) + ((sdev_incongruent ** 2) / num_samples))

t = mean_diff / (sdev_diff / np.sqrt(df))

# alpha = 0.05
crit = stats.t.ppf(0.975, df)

p = 1 - stats.t.cdf(np.abs(t), df)
```

| Result   | Value      |
|----------|------------|
| df       | 23         |
| Std Error| 1.2193     |
| T        | ±7.8518    |
| C        | 2.0687     |
| P        | 2.9511e-08 |


* As the P value is less than the alpha level of 0.05 the null hypothesis is rejected and conclude with 95% confidence that it takes a longer time to read the incongruent wordset than the congruent wordset.
* The data shows that Congruent samples were easier for all participants to read. This is due to the word and the color of the word matching, whereas with the incongruent samples they were different. This lead to a delay in providing the answer due to 2 different peices of infomation about the word require processing. 
* This is consistent with my own test results, as it took me almost double the time to read the incongruent wordset and I often found that I would need to pause before providing my answer with the incongruent dataset.
* Several theroies exist as to why incongruent dataset is more difficult to read:
> * Selective Attention Theory
> 
> * Automaticity Theory
> 
> * Speed of Processing Theory
> 
> * Parallel Distributed Processing
> 
* Further tests could be done to further prove the theory, where the words are rotated or in difference cases.

### References
1. https://en.wikipedia.org/wiki/Stroop_effect
2. https://faculty.washington.edu/chudler/java/ready.html
3. https://en.wikipedia.org/wiki/T-statistic
4. https://www.statisticshowto.com/probability-and-statistics/hypothesis-testing/t-score-vs-z-score/
5. https://en.wikipedia.org/wiki/Student%27s_t-test
6. https://lesley.edu/article/what-the-stroop-effect-reveals-about-our-minds#:~:text=Stroop%20found%20that%20subjects%20took,printed%20word%20in%20experiment%20one.
