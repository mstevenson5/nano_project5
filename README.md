# nano_project5

## Project Introduction


## Question 1:
### What is our independent variable? What is our dependent variable?
* Independent: Word sets, both congruent and incongruent words
* Dependent: Time taken to read each word set.

## Question 2:
### What is an appropriate set of hypotheses for this task? What kind of statistical test do you expect to perform? Justify your choices.

Test the existence of the Stroop Effect, where the time taken to read the incongruent word set is greater when compared to the time taken to read the congruent word set. If the mean time taken between the congruent and incongruent word sets is significantly different then we have observed the Stoop Effect. 
*Null Hypothesis: μ_C-μ_I  ≤ 0
*Alternative Hypothesis: μ_C-μ_I  > 0

The Null Hypothesis is that the mean congruent time taken is less than that of the mean incongruent time taken.

### My Test:
* Congruent Set: 10.404s
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


## Question 5:
### Now, perform the statistical test and report your results. What is your confidence level and your critical statistic value? Do you reject the null hypothesis or fail to reject it? Come to a conclusion in terms of the experiment task. Did the results match up with your expectations?
