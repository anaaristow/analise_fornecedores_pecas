Is a descriptive analysis enough to make a decision?

In this study, I try to show why you cannot take conclusions before analyzing some statistics.

Case: Your manager asks you to choose between two suppliers. To do so, he gives you a sheet containing the supplier (A or B) and the dimension of the item. The requirement is 4.80 ± 0.3 mm.

# Choosing a Supplier

The Problem: your mnager provides you with data samples from two suppliers and you need to choose the best one.

- Requirement: dimension of 4.80 ± 0.3 mm. 

**Here's my approch to conduct the study:**
1. Descriptive analysis
2. Error estimation
3. Hypothesis testing

## Descriptive analysis

**Box-Plot**
```python
def plot_box_plot(df):
    colors = ['#87CEEB', '#90EE90']

    plt.figure(figsize=(10, 6))
    sns.boxplot(x='supplier', y='dimension', data=df, palette=colors, hue='supplier', dodge=False, linewidth=1)
    plt.title('Boxplot - Dimensao Peça por Maquina')
    plt.xlabel('Supplier')
    plt.ylabel('Dimension')
    plt.show()

plot_box_plot(df)
```
![Alt text](https://github.com/anaaristow/machinechoose/blob/main/images/box_plot.png?raw=true)

**Histogram**
```python
def plot_histograms(df):
    colors = ['#87CEEB', '#90EE90']

    for i, supplier in enumerate(df['supplier'].unique()):
        supplier_data_dimensao = df[df['supplier'] == supplier]['dimension']
        plt.figure(figsize=(6, 4))
        
        plt.hist(supplier_data_dimensao, bins=10, alpha=0.5, color=colors[i])
        plt.title(f'Histogram - Dimension - Supplier {supplier}')
        plt.xlabel('Dimension')
        plt.ylabel('Frequency')
        plt.show()
        
plot_histograms(df)
```
![Alt text](https://github.com/anaaristow/machinechoose/blob/main/images/hist_A.png?raw=true)
![Alt text](https://github.com/anaaristow/machinechoose/blob/main/images/hist_B.png?raw=true)

## Error estimation
```python
def calculate_error(df):
    df['error'] = 0
    df.loc[(df['dimension'] > (4.8 + 0.3)) | (df['dimension'] < (4.8 - 0.3)), 'error'] = 1
    return

calculate_error(df)

df.groupby('supplier')['error'].agg(['mean']).rename({"mean":"percentage"}, axis =1)


Percentage of error for Supplier A is: 0%
Percentage of error for Supplier B is: 36%
```

### Let's choose Suplier A!!!
With Supplier A, not only the majority of the data is centered around the desired **target** of 4.8, but there's not item outside the requirement dimension. Also, the boxplot reveals that A exhibits **less** variability compared to B.

So, you might be thinking, choose Supplier A, obviously! 

![elgif](https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExbXphOWlma3IydWM5azMwd3VjaHdpazh2bzk0MmdxdGduN21jczRwbSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/SaKvh2fShKr1hYZssC/giphy.gif)

## Hypothesis testing

To ensure that Supplier A is consistanlly better and to ensure that this results were not obtained randomly, let's conduct a hypothesis test.

- Null Hypothesis (H0): The dimension between Supplier A and Supplier B are the same 

- Alternative Hypothesis (H1): The dimension are not the same

```python
result = smf.ols('dimension ~ supplier', data=df).fit()

print(result.summary().tables[1])
```
![Alt text](https://github.com/anaaristow/machinechoose/blob/main/images/result_smf_ols.png?raw=true)

This table shows us a lot of things, but to keep it simple, let's focus on the highlighted part in red.

The **p-value** found was 0.233. With an α (level of significance) of 5% I can't reject the Null Hypothesis because **0,233 > 0,05**. 

## Conclusion
In the end, I don't have enough data to statistically prove that one supplier is better than the other. So, based on this data alone, it's not possible to draw a conclusion.