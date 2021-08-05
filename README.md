# Market Basket Analysis



## Introduction


Market Basket Analysis is a data mining technique used to understand customer purchasing patterns and to uncover meaningful correlations between different entities according to their co-occurrence in the data set, and to extract significant knowledge about the unexpected trends and hidden patterns.


It is conceptually an efficient primal stone in marketing and sales research that may be used to handle strategic business decisions as its about understanding the consumer’s behaviour patterns on his purchases. This overall technique led to find the affinities amongst the items in consumer’s shopping baskets and the idea of it being able to affect homogeneous customers. Hence this technique has obtained an added value to recommendation systems. It opens a new opportunity for them to improve customer support thereby, decreasing their customer acquisition costs and increasing the customer retention rate.


In order to imply this Market Basket Analysis approach in various domains more organizations are discovering ways to gain useful insights into associations and hidden relationships. As industry leaders continue to explore the technique’s value, a predictive version of market basket analysis is making in-roads across many sectors in an effort to identify sequential purchases. Yet lack of knowledge on individual’s purchase, co-occurrences within their purchases are slightly neglected and it may results in the omit of mindsets behind their every purchase or transaction.




---

## Abstract


### Formal Definition of the Problem


Let $I = \{i_1, i_2, \dots, i_n\}$ be a set of $n$ binary attributes called items.



Let $D = {t_1, t_2, \dots, t_m}$ be a set of transactions called the database.



Each element in $D$ has a unique ID called Transaction ID and contains a subset of the elements of $I$



A rule is defined to be an implication relation of the form $X \implies Y$, where $X, Y \subseteq I$ and $X \cap Y = \emptyset$.


### Measures



The following measures are used to get the statistical significance of an itemset.



#### Support


Support is a measure of how frequently the itemset appears in the dataset. The support of $X$ which is restricted to $T$, where $T$ is defined as the proportion of transactions in the dataset.



$$\mbox{Supp}(X) = \frac{\mbox{frequency}(X \subseteq T)}{\mbox{frequency}(T)}$$



#### Confidence



Confidence is a measure of how often the **rule** $X \implies Y$ has been found to be true.



$$\mbox{Conf}(X \implies Y) = \frac{\mbox{Supp}(X \cup Y)}{\mbox{Supp}(X)}$$



#### Lift



The problem with **confidence** is, it only considers the $\mbox{Supp}(X)$, not $\mbox{Supp}(Y)$. So we define another measure that also considers the popularity of $Y$.



$$\mbox{Lift}(X \implies Y) = \frac{\mbox{Supp}(X \cup Y)}{\mbox{Supp}(X) \times \mbox{Supp}(Y)}$$


### Bottle Neck



But there is a problem here, since $X \subset I$ we might need to consider all the possible subset of $I$, and if $I$ has $n$ elements then, there are $2^n$ possible subset. Even if $I$ has like $100$ items, $2^{100}$ is more the than the number of atoms in the universe. If $n = 200$, it would take more than the **life of the universe** to compute all the implication relations among all of them. Since we can't compute the measures of each and every Subset, so we need to **make some tradeoffs** and cut down the number of subsets we need to compute. This is where the **cleverness** of Apirori Algorithm enters, and will be explained in the section below.



---



## Algorithms



### Apriori Algorithm



Although there are many algorithms out there to do market basket analysis. They are either not as efficient in terms of time or space as Apriori Algorithm. So we are going to use Apriori Algorithm to find the support of **some elements of the power set, $\mathcal{P}(I)$**. The following is pseudocode of the Apriori Algorithm.



```
1. Apriori(T, ε)
2. 	L(1) ← {large 1 - itemsets}	
3.	k ← 2

4.  while L(k−1) is not empty
5.      C(k) ← {c = Union(a, {b}) : a in L(k−1) and b not in a, {s ⊆ c : |s| = k − 1} ⊆ L(k−1)}

6.      for transactions t in T
7.          D(t) ← {c in C(k) : c ⊆ t}
8.          for candidates c in D(t)
9.              count[c] ← count[c] + 1

10.     L(k) ← {c in C(k) : count[c] ≥ ε}
11.     k ← k + 1

12. return Union(L(k))
```



#### $k$-itemset



A $k$-itemset is a set that contains $k$ items




#### $L_k$      


Set of large $k$-itemsets with minimum support. 
Each member of this set has two fields:
1. itemset
2. support count


#### $C_k$


Set of candidate $k$-itemsets contains potentially large itemsets.


Each member of this set has two fields:


1. itemset


2. support count

#### Example


To understand the cleverness of the algorithm, we need to work out an example.



Consider the following transaction table:



| TID | Milk | Bread | Butter | Beer | Diapers |
|:---:|:----:|:-----:|:------:|:----:|:-------:|
|  1  |  1   |   1   |   0    |  0   |    0    |
|  2  |  0   |   0   |   1    |  0   |    0    |
|  3  |  0   |   0   |   0    |  1   |    1    |
|  4  |  1   |   1   |   1    |  0   |    0    |
|  5  |  0   |   1   |   0    |  0   |    0    |


Let the minimum support count, "$\varepsilon =  1$".


According to line 2, we need to initiate $L_1$ to be large $1$-itemset.


| $1$-itemset | Support Count |
| ----------- |:-------------:|
| Milk        |       2       |
| Bread       |       3       | 
| Butter      |       2       |
| Beer        |       1       |
| Diapers     |       1       |


And in line 3, $k \leftarrow 2$
To find $L_2$


$L_2$:

1. $L_1$ is not empty, execute while loop in line 4
2. Generate $C_2$, we know from the line 5, $C_2$  is the subsets of all set that contains $2$ elements of $L_1$, and those two elements are not equal.


$C_2$ = {{Milk, Bread}, {Milk, Butter}, {Milk, Beer}, {Milk, Diapers}, {Bread, Butter}, {Bread, Beer}, {Bread, Diapers}, {Butter, Beer}, {Butter, Diapers}, {Beer, Diapers}}


4. Now run the **For Loop**, to get the support count of each element.
5. If support count of each  element in $C_2$.

| $C_2$             | Support Count |
| ----------------- |:-------------:|
| {Milk, Bread}     |       2       |
| {Milk, Butter}    |       1       |
| {Milk, Beer}      |       0       |
| {Milk, Diapers}   |       0       |
| {Bread, Butter}   |       1       |
| {Bread, Beer}     |       0       |
| {Bread, Diapers}  |       0       |
| {Butter, Beer}    |       0       |
| {Butter, Diapers} |       0       |
| {Beer, Diapers}   |       1       |

6.  Now, if the support count of each element in $C_2 \geq$ Minimum Support Count, i.e. $\varepsilon = 1$, add that element to $L_2$

| $L_2$             | Support Count |
| ----------------- |:-------------:|
| {Milk, Bread}     |       2       |
| {Milk, Butter}    |       1       |
| {Bread, Butter}   |       1       |
| {Beer, Diapers}   |       1       |

8.  Then, increment $k$, loop again.

**It might seem like all the 5 elements are in $L_2$, no useless item was removed, but as we run the algorithm more times, you'll see!**


$L_3$:
Running loop again we get

| $C_3$                    | Support Count |
| ------------------------ |:-------------:|
| {Milk, Bread, Butter}    |       1       |
| {Milk, Bread, Beer}      |       0       |
| {Milk, Bread, Diapers}   |       0       |
| {Milk, Butter, Beer}     |       0       |
| {Milk, Butter, Diapers}  |       0       |
| {Milk, Beer, Diapers}    |       0       |
| {Bread, Butter, Beer}    |       0       |
| {Bread, Butter, Diapers} |       0       |
| {Bread, Beer, Diapers}   |       0       |
| {Butter, Beer, Diapers}  |       0       |

So $L_3$ will be

| $L_3$                 | Support Count |
| --------------------- |:-------------:|
| {Milk, Bread, Butter} |       1       |




$L_4$:

To generate, $L_4$,

$L_3$ is not empty so run the while loop, but to generate $C_4$ we need need to choose $4$ elements from {Milk, Bread, Butter}, but there are only 3 elements in there, so $C_4$ is empty, thus $L_4$ is empty.

**While loop terminates, since $L_4$ is empty**


> The cleverness of the apriori algorithm is,
> 
> Instead of checking all the implication relations of subsets in the powerset $\mathcal{P}(I)$ of itemset $I$ for support and confidence or lift, it removes all the elements of powerset that doesn't have the minimum support.
> Reason is, let minimum support be $\varepsilon > 0$.
>
> Suppose, 
> $$\mbox{Supp}(X) < \varepsilon$$
> then the confidence,
>$$\begin{align}
\mbox{Conf}(X \implies Y) &= \frac{\mbox{Supp}(X \cup Y)}{\mbox{Supp}(X)} \\ \\
\mbox{Conf}(X \implies Y) &= \frac{\mbox{Supp}(X \ \mbox{and} \ Y)}{\mbox{Supp}(X)}
\end{align}$$
>and 
$$\mbox{Supp}(X \ \mbox{and}  \ Y) \leq \mbox{Supp}(X) < \varepsilon$$
>So,
$$\mbox{Conf}(X \implies Y) < \varepsilon$$
>Similarly if our measure is **lift**. So,  we don't need to calculate **confidence or lift** for all the subsets that contain $X$, this is how apirori algorithm cut down the number of subsets.

### Generating Association Rules

To generation strong association rules from itemsets where strong association rules satisfy both minimum support and minimum confidence or minimum  lift, do as follows:

For each frequent itemset $l$ of $L_{k-1}$, generate all non-empty subsets of $l$.

For every non-empty subset $s$ of $l$, output the rule "$s \implies (l -s)$" if $\mbox{measure}(s \implies (l-s)) \geq$ minimum measure, where measures are either confidence or lift.

For the above example.

The frequent item set are $L_{k-1}$ which is $L_3$, 

$L_3$ has only 1 item, and let confidence be our measure.

so , $l =$ {Milk, Bread, Butter}

let $p = s-l$

$$
\mbox{Conf}(s \implies p) = \frac{\mbox{Supp}(s \cup p)}{\mbox{Supp}(s)}
$$

First Let $s =$ {Milk}, so $p =$ {Bread, Butter}

| s               | p               | Confidence |
| --------------- | --------------- | ---------- |
| {Milk}          | {Bread, Butter} | 0.5        |
| {Bread}         | {Milk, Butter}  | 0.333..    |
| {Butter}        | {Milk, Bread}   | 0.5        |
| {Milk, Bread}   | {Butter}        | 0.5        |
| {Milk, Butter}  | {Bread}         | 1          |
| {Bread, Butter} | {Milk}          | 1          |


Let minimum confidence = 1.

So, our strong association rules are 


1. {Milk, Butter} $\implies$ {Bread}
2. {Bread, Butter} $\implies$ {Milk}


This not possible in real world application, because if minimum confidence is 1, then we saying we are 100% confident about this happening. Since it is a small dataset, we get these wired results. So, we need a big dataset. We went around the internet and found the biggest dataset we could find on the internet. It is so big, it took my computer 2-3 minutes to loadup data into RAM.



---

## Data Collection 

Data was collected from Instacart, an American company that operates a grocery delivery and pick up service in the United States and Canada. This anonymized dataset contains a sample of over 3 million grocery orders.

Source: https://www.kaggle.com/c/instacart-market-basket-analysis/data


---

## Data Description
  
The dataset contains relational set of files describing customers' orders over time. For each user, 4 to 100 orders are provided with the sequence of products purchased in each order. The data of the order's week and hour of the day as well as a relative measure of time between orders is provided.

#### Data


```python
os.listdir("../data")
```

The following are the files in the dataset.

    ['aisles.csv',
     'departments.csv',
     'orders.csv',
     'order_products__prior.csv',
     'order_products__train.csv',
     'products.csv',
     'sample_submission.csv']



```python
aisles = pd.read_csv("../data/aisles.csv")
departments = pd.read_csv("../data/departments.csv")
orders = pd.read_csv("../data/orders.csv")
prior = pd.read_csv("../data/order_products__prior.csv")
train = pd.read_csv("../data/order_products__train.csv")
products = pd.read_csv("../data/products.csv")
```


#### alisles.csv

It contains all the alisles in the instacart website. although we don't use it in our implementation, some else might use to get some extra knowledge.\

```python
len(aisles.aisle.unique())
```




    134


``` python
aisles.info()
```

	<class 'pandas.core.frame.DataFrame'>
	RangeIndex: 134 entries, 0 to 133
	Data columns (total 2 columns):
	 #   Column    Non-Null Count  Dtype 
	---  ------    --------------  ----- 
	 0   aisle_id  134 non-null    int64 
	 1   aisle     134 non-null    object
	dtypes: int64(1), object(1)
	memory usage: 2.2+ KB
	
	

```python
aisles.aisle.unique()
```




	array(['prepared soups salads', 'specialty cheeses', 'energy', granola bars', 'instant foods', 'marinades meat preparation', 'other', 'packaged meat', 'bakery desserts', 'pasta sauce', 'kitchen supplies', 'cold flu allergy', 'fresh pasta', 'prepared meals', 'tofu meat alternatives', 'packaged seafood', 'fresh herbs', 'baking ingredients', 'bulk dried fruits vegetables', 'oils vinegars', 'oral hygiene', 'packaged cheese', 'hair care', 'popcorn jerky', 'fresh fruits', 'soap', 'coffee', 'beers coolers','red wines', 'honeys syrups nectars', 'latino foods', 'refrigerated', 'packaged produce', 'kosher foods', 'frozen meat seafood', 'poultry counter', 'butter', 'ice cream ice', 'frozen meals', 'seafood counter', 'dog food care', 'cat food care', 'frozen vegan vegetarian', 'buns rolls', 'eye ear care', 'candy chocolate', 'mint gum', 'vitamins supplements', 'breakfast bars pastries', 'packaged poultry', 'fruit vegetable snacks', 'preserved dips spreads', 'frozen breakfast', 'cream', 'paper goods', 'shave needs', 'diapers wipes', 'granola', 'frozen breads doughs', 'canned meals beans', 'trash bags liners', 'cookies cakes', 'white wines', 'grains rice dried goods', 'energy sports drinks', 'protein meal replacements', 'asian foods', 'fresh dips tapenades', 'bulk grains rice dried goods', 'soup broth bouillon', 'digestion', 'refrigerated pudding desserts', 'condiments', 'facial care', 'dish detergents', 'laundry', 'indian foods', 'soft drinks', 'crackers', 'frozen pizza', 'deodorants', 'canned jarred vegetables', 'baby accessories', 'fresh vegetables', 'milk', 'food storage', 'eggs', 'more household', 'spreads', 'salad dressing toppings', 'cocoa drink mixes', 'soy lactosefree', 'baby food formula', 'breakfast bakery', 'tea', 'canned meat seafood', 'lunch meat', 'baking supplies decor', 'juice nectars', 'canned fruit applesauce', 'missing', 'air fresheners candles', 'baby bath body care', 'ice cream toppings', 'spices seasonings', 'doughs gelatins bake mixes', 'hot dogs bacon sausage', 'chips pretzels', 'other creams cheeses', 'skin care', 'pickled goods olives', 'plates bowls cups flatware', 'bread', 'frozen juice', 'cleaning products', 'water seltzer sparkling water', 'frozen produce', 'nuts seeds dried fruit', 'first aid', 'frozen dessert', 'yogurt', 'cereal', 'meat counter', 'packaged vegetables fruits', 'spirits', 'trail mix snack mix', 'feminine care', 'body lotions soap', 'tortillas flat bread', 'frozen appetizers sides', 'hot cereal pancake mixes', 'dry pasta', 'beauty', 'muscles joints pain relief', 'specialty wines champagnes'], 
		dtype=object)

It contains details about, 134 aisles. and they are as seen above.

### departments.csv

It contains all the departments, although we don't use it in our implementation, some else might use to get some extra knowledge.

```python
len(departments.department.unique())
```




    21

The dataset contains contains 21 departments.

```python
departments.department.unique()
```




    array(['frozen', 'other', 'bakery', 'produce', 'alcohol', 'international','beverages', 'pets', 'dry goods pasta', 'bulk', 'personal care', 'meat seafood', 'pantry', 'breakfast', 'canned goods', 'dairy eggs', 'household', 'babies', 'snacks', 'deli', 'missing'],
	dtype=object)


### orders.csv

It contains all the orders

```python
orders.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 3421083 entries, 0 to 3421082
    Data columns (total 7 columns):
     #   Column                  Dtype  
    ---  ------                  -----  
     0   order_id                int64  
     1   user_id                 int64  
     2   eval_set                object 
     3   order_number            int64  
     4   order_dow               int64  
     5   order_hour_of_day       int64  
     6   days_since_prior_order  float64
    dtypes: float64(1), int64(5), object(1)
    memory usage: 182.7+ MB
    


```python
len(orders.order_id.unique())
```




    3421083




```python
len(orders.user_id.unique())
```




    206209



### order_products_*.csv

This * represents prior and train.


These files give information about which products were ordered and in which order they were added in the cart. It also tells us that if the product was reordered or not.



```python
prior.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 32434489 entries, 0 to 32434488
    Data columns (total 4 columns):
     #   Column             Dtype
    ---  ------             -----
     0   order_id           int64
     1   product_id         int64
     2   add_to_cart_order  int64
     3   reordered          int64
    dtypes: int64(4)
    memory usage: 989.8 MB
    


```python
len(prior.order_id.unique())
```




    3214874




```python
len(prior.product_id.unique())
```




    49677


```python
train.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1384617 entries, 0 to 1384616
    Data columns (total 4 columns):
     #   Column             Non-Null Count    Dtype
    ---  ------             --------------    -----
     0   order_id           1384617 non-null  int64
     1   product_id         1384617 non-null  int64
     2   add_to_cart_order  1384617 non-null  int64
     3   reordered          1384617 non-null  int64
    dtypes: int64(4)
    memory usage: 42.3 MB
    


```python
len(train.order_id.unique())
```




    131209




```python
len(train.product_id.unique())
```




    39123



### products.csv

This file contains the list of total 49688 products and their aisle as well as department. The number of products in different aisles and different departments are different.


```python
len(products.product_name.unique())
```




    49688




```python
len(products.aisle_id.unique())
```




    134




```python
len(products.department_id.unique())
```




    21


---

## Code and Results

```python
# Import Packages

# Pandas for rading CSV files
import pandas as pd 

# Numpy for arrays, array is a much more efficient data structure than builtin python list for our purpose
import numpy as np 


# mlxtend is a machine learing library that contations alot of ml tools
from mlxtend.frequent_patterns import apriori
from mlxtend.frequent_patterns import association_rules
```


```python
# Import DataSets
order = pd.read_csv("../data/orders.csv")
product = pd.read_csv("../data/products.csv")
prior = pd.read_csv("../data/order_products__prior.csv")
train = pd.read_csv("../data/order_products__train.csv")

# Now we are merging prior and train datasets to get the complete order dataset.
train = train.append(prior,ignore_index = True)

train['reordered'] = 1 
```


```python
productCount = train.groupby("product_id",as_index = False)["order_id"].count()
```


```python
# Top 100 most frequently purchased products
freq_product = 100

productCount = productCount.sort_values("order_id",ascending = False)
topProduct = productCount.iloc[0:freq_product,:]
topProduct = topProduct.merge(product,on = "product_id")
productId= topProduct.loc[:,["product_id"]]



# Here order_id is the count, so we need to sort the data frame with repect to order_id

df = train[0:0]
for i in range(0,99):
    pId = productId.iloc[i]['product_id'] 
    stDf = train[train.product_id == pId ]
    df = df.append(stDf,ignore_index = False)
```


```python
# Here we are defining the basket to analysis
basket = df.groupby(['order_id', 'product_id'])['reordered'].sum().unstack().reset_index().fillna(0).set_index('order_id')
```


```python
# This function is used to convert database into a boolean database as seen below

def encode(x):
    if x <= 0:
        return 0
    if x >= 1:
        return 1    
```


```python
# here we encoding the basket df with our encode funtion
basket_sets = basket.applymap(encode)
```


```python
# This is the size of the basket_sets
basket_sets.size
```




    241667217

That is **over 240 million!**


```python
# Get frequent itemsets using apriori
frequent_itemsets = apriori(basket_sets, min_support=0.001, use_colnames=True, low_memory = True)
```


```python
# Print frequnt items sets
frequent_itemsets
```



</style>
<table>
  <thead>
    <tr>
      <th></th>
      <th>support</th>
      <th>itemsets</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.015279</td>
      <td>(196)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.016088</td>
      <td>(3957)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.015144</td>
      <td>(4210)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.031514</td>
      <td>(4605)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.015439</td>
      <td>(4799)</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2518</th>
      <td>0.001043</td>
      <td>(40706, 47626, 47766)</td>
    </tr>
    <tr>
      <th>2519</th>
      <td>0.001013</td>
      <td>(47626, 47766, 45007)</td>
    </tr>
    <tr>
      <th>2520</th>
      <td>0.001462</td>
      <td>(47626, 49683, 47766)</td>
    </tr>
    <tr>
      <th>2521</th>
      <td>0.001439</td>
      <td>(13176, 47209, 21137, 21903)</td>
    </tr>
    <tr>
      <th>2522</th>
      <td>0.001662</td>
      <td>(13176, 47209, 21137, 27966)</td>
    </tr>
  </tbody>
</table>
<p>2523 rows × 2 columns</p>
</div>





```python


# Now we generate rules:

rules = association_rules(frequent_itemsets, metric="lift", min_threshold=1)

# Print rules
rules
```




<table>
  <thead>
    <tr>
      <th></th>
      <th>antecedents</th>
      <th>consequents</th>
      <th>antecedent support</th>
      <th>consequent support</th>
      <th>support</th>
      <th>confidence</th>
      <th>lift</th>
      <th>leverage</th>
      <th>conviction</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>(5876)</td>
      <td>(3957)</td>
      <td>0.037381</td>
      <td>0.016088</td>
      <td>0.001157</td>
      <td>0.030948</td>
      <td>1.923701</td>
      <td>0.000555</td>
      <td>1.015335</td>
    </tr>
    <tr>
      <th>1</th>
      <td>(3957)</td>
      <td>(5876)</td>
      <td>0.016088</td>
      <td>0.037381</td>
      <td>0.001157</td>
      <td>0.071911</td>
      <td>1.923701</td>
      <td>0.000555</td>
      <td>1.037205</td>
    </tr>
    <tr>
      <th>2</th>
      <td>(13176)</td>
      <td>(3957)</td>
      <td>0.161785</td>
      <td>0.016088</td>
      <td>0.003762</td>
      <td>0.023252</td>
      <td>1.445357</td>
      <td>0.001159</td>
      <td>1.007335</td>
    </tr>
    <tr>
      <th>3</th>
      <td>(3957)</td>
      <td>(13176)</td>
      <td>0.016088</td>
      <td>0.161785</td>
      <td>0.003762</td>
      <td>0.233837</td>
      <td>1.445357</td>
      <td>0.001159</td>
      <td>1.094043</td>
    </tr>
    <tr>
      <th>4</th>
      <td>(21137)</td>
      <td>(3957)</td>
      <td>0.112891</td>
      <td>0.016088</td>
      <td>0.002259</td>
      <td>0.020013</td>
      <td>1.243979</td>
      <td>0.000443</td>
      <td>1.004005</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>6417</th>
      <td>(21137, 27966)</td>
      <td>(13176, 47209)</td>
      <td>0.014556</td>
      <td>0.026530</td>
      <td>0.001662</td>
      <td>0.114147</td>
      <td>4.302641</td>
      <td>0.001275</td>
      <td>1.098908</td>
    </tr>
    <tr>
      <th>6418</th>
      <td>(13176)</td>
      <td>(47209, 21137, 27966)</td>
      <td>0.161785</td>
      <td>0.003389</td>
      <td>0.001662</td>
      <td>0.010270</td>
      <td>3.030382</td>
      <td>0.001113</td>
      <td>1.006953</td>
    </tr>
    <tr>
      <th>6419</th>
      <td>(47209)</td>
      <td>(13176, 21137, 27966)</td>
      <td>0.090483</td>
      <td>0.005011</td>
      <td>0.001662</td>
      <td>0.018363</td>
      <td>3.664351</td>
      <td>0.001208</td>
      <td>1.013602</td>
    </tr>
    <tr>
      <th>6420</th>
      <td>(21137)</td>
      <td>(13176, 47209, 27966)</td>
      <td>0.112891</td>
      <td>0.004891</td>
      <td>0.001662</td>
      <td>0.014718</td>
      <td>3.009076</td>
      <td>0.001109</td>
      <td>1.009974</td>
    </tr>
    <tr>
      <th>6421</th>
      <td>(27966)</td>
      <td>(13176, 47209, 21137)</td>
      <td>0.058418</td>
      <td>0.006463</td>
      <td>0.001662</td>
      <td>0.028443</td>
      <td>4.401036</td>
      <td>0.001284</td>
      <td>1.022623</td>
    </tr>
  </tbody>
</table>
<p>6422 rows × 9 columns</p>
</div>


## Conclusion 

Since we have more than 6400 association rules, we want to filter out those unimportant rules, so we choose minimum confidence as 0.4 and minimum lift as 2. 

```python
# We can filter the dataframe using standard pandas code.

rules[ (rules['lift'] >= 2) & (rules['confidence'] >= 0.4) ]
```




<div>
<table>
  <thead>
    <tr>
      <th></th>
      <th>antecedents</th>
      <th>consequents</th>
      <th>antecedent support</th>
      <th>consequent support</th>
      <th>support</th>
      <th>confidence</th>
      <th>lift</th>
      <th>leverage</th>
      <th>conviction</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3319</th>
      <td>(4605, 16797)</td>
      <td>(24852)</td>
      <td>0.002171</td>
      <td>0.201259</td>
      <td>0.001021</td>
      <td>0.470377</td>
      <td>2.337169</td>
      <td>0.000584</td>
      <td>1.508131</td>
    </tr>
    <tr>
      <th>3403</th>
      <td>(4920, 28204)</td>
      <td>(24852)</td>
      <td>0.002436</td>
      <td>0.201259</td>
      <td>0.001143</td>
      <td>0.469055</td>
      <td>2.330598</td>
      <td>0.000652</td>
      <td>1.504375</td>
    </tr>
    <tr>
      <th>3408</th>
      <td>(4920, 45066)</td>
      <td>(24852)</td>
      <td>0.002709</td>
      <td>0.201259</td>
      <td>0.001174</td>
      <td>0.433238</td>
      <td>2.152632</td>
      <td>0.000628</td>
      <td>1.409304</td>
    </tr>
    <tr>
      <th>3421</th>
      <td>(4920, 47766)</td>
      <td>(24852)</td>
      <td>0.003384</td>
      <td>0.201259</td>
      <td>0.001409</td>
      <td>0.416414</td>
      <td>2.069043</td>
      <td>0.000728</td>
      <td>1.368678</td>
    </tr>
    <tr>
      <th>3426</th>
      <td>(4920, 49683)</td>
      <td>(24852)</td>
      <td>0.002387</td>
      <td>0.201259</td>
      <td>0.001051</td>
      <td>0.440268</td>
      <td>2.187563</td>
      <td>0.000570</td>
      <td>1.427005</td>
    </tr>
    <tr>
      <th>3446</th>
      <td>(5876, 8277)</td>
      <td>(13176)</td>
      <td>0.002505</td>
      <td>0.161785</td>
      <td>0.001007</td>
      <td>0.402060</td>
      <td>2.485155</td>
      <td>0.000602</td>
      <td>1.401839</td>
    </tr>
    <tr>
      <th>3482</th>
      <td>(5876, 27966)</td>
      <td>(13176)</td>
      <td>0.004337</td>
      <td>0.161785</td>
      <td>0.001762</td>
      <td>0.406309</td>
      <td>2.511417</td>
      <td>0.001061</td>
      <td>1.411871</td>
    </tr>
    <tr>
      <th>3572</th>
      <td>(21137, 8174)</td>
      <td>(13176)</td>
      <td>0.003079</td>
      <td>0.161785</td>
      <td>0.001345</td>
      <td>0.436876</td>
      <td>2.700356</td>
      <td>0.000847</td>
      <td>1.488510</td>
    </tr>
    <tr>
      <th>3583</th>
      <td>(27966, 8174)</td>
      <td>(13176)</td>
      <td>0.002255</td>
      <td>0.161785</td>
      <td>0.001119</td>
      <td>0.496094</td>
      <td>3.066386</td>
      <td>0.000754</td>
      <td>1.663437</td>
    </tr>
    <tr>
      <th>3590</th>
      <td>(47209, 8174)</td>
      <td>(13176)</td>
      <td>0.003491</td>
      <td>0.161785</td>
      <td>0.001628</td>
      <td>0.466385</td>
      <td>2.882751</td>
      <td>0.001064</td>
      <td>1.570824</td>
    </tr>
    <tr>
      <th>3614</th>
      <td>(8277, 27966)</td>
      <td>(13176)</td>
      <td>0.004447</td>
      <td>0.161785</td>
      <td>0.001808</td>
      <td>0.406541</td>
      <td>2.512850</td>
      <td>0.001088</td>
      <td>1.412423</td>
    </tr>
    <tr>
      <th>3620</th>
      <td>(47209, 8277)</td>
      <td>(13176)</td>
      <td>0.006522</td>
      <td>0.161785</td>
      <td>0.002798</td>
      <td>0.429020</td>
      <td>2.651796</td>
      <td>0.001743</td>
      <td>1.468029</td>
    </tr>
    <tr>
      <th>3679</th>
      <td>(8424, 47766)</td>
      <td>(24852)</td>
      <td>0.003468</td>
      <td>0.201259</td>
      <td>0.001537</td>
      <td>0.443303</td>
      <td>2.202643</td>
      <td>0.000839</td>
      <td>1.434784</td>
    </tr>
    <tr>
      <th>3806</th>
      <td>(47209, 16759)</td>
      <td>(13176)</td>
      <td>0.002646</td>
      <td>0.161785</td>
      <td>0.001132</td>
      <td>0.427930</td>
      <td>2.645058</td>
      <td>0.000704</td>
      <td>1.465232</td>
    </tr>
    <tr>
      <th>3852</th>
      <td>(19057, 22935)</td>
      <td>(13176)</td>
      <td>0.002632</td>
      <td>0.161785</td>
      <td>0.001055</td>
      <td>0.400996</td>
      <td>2.478579</td>
      <td>0.000630</td>
      <td>1.399349</td>
    </tr>
    <tr>
      <th>3858</th>
      <td>(19057, 27966)</td>
      <td>(13176)</td>
      <td>0.003660</td>
      <td>0.161785</td>
      <td>0.001650</td>
      <td>0.450811</td>
      <td>2.786489</td>
      <td>0.001058</td>
      <td>1.526279</td>
    </tr>
    <tr>
      <th>3864</th>
      <td>(19057, 30391)</td>
      <td>(13176)</td>
      <td>0.002650</td>
      <td>0.161785</td>
      <td>0.001102</td>
      <td>0.415611</td>
      <td>2.568910</td>
      <td>0.000673</td>
      <td>1.434343</td>
    </tr>
    <tr>
      <th>3870</th>
      <td>(19057, 47209)</td>
      <td>(13176)</td>
      <td>0.005673</td>
      <td>0.161785</td>
      <td>0.002476</td>
      <td>0.436421</td>
      <td>2.697544</td>
      <td>0.001558</td>
      <td>1.487309</td>
    </tr>
    <tr>
      <th>4284</th>
      <td>(22825, 27966)</td>
      <td>(13176)</td>
      <td>0.002929</td>
      <td>0.161785</td>
      <td>0.001262</td>
      <td>0.430709</td>
      <td>2.662235</td>
      <td>0.000788</td>
      <td>1.472384</td>
    </tr>
    <tr>
      <th>4290</th>
      <td>(47209, 22825)</td>
      <td>(13176)</td>
      <td>0.003950</td>
      <td>0.161785</td>
      <td>0.001778</td>
      <td>0.450218</td>
      <td>2.782820</td>
      <td>0.001139</td>
      <td>1.524631</td>
    </tr>
    <tr>
      <th>4452</th>
      <td>(27966, 30391)</td>
      <td>(13176)</td>
      <td>0.003762</td>
      <td>0.161785</td>
      <td>0.001534</td>
      <td>0.407819</td>
      <td>2.520749</td>
      <td>0.000926</td>
      <td>1.415471</td>
    </tr>
    <tr>
      <th>4475</th>
      <td>(39928, 27966)</td>
      <td>(13176)</td>
      <td>0.003123</td>
      <td>0.161785</td>
      <td>0.001306</td>
      <td>0.418153</td>
      <td>2.584627</td>
      <td>0.000801</td>
      <td>1.440612</td>
    </tr>
    <tr>
      <th>4488</th>
      <td>(27966, 41950)</td>
      <td>(13176)</td>
      <td>0.002478</td>
      <td>0.161785</td>
      <td>0.001009</td>
      <td>0.406942</td>
      <td>2.515331</td>
      <td>0.000608</td>
      <td>1.413379</td>
    </tr>
    <tr>
      <th>4494</th>
      <td>(27966, 44359)</td>
      <td>(13176)</td>
      <td>0.002686</td>
      <td>0.161785</td>
      <td>0.001082</td>
      <td>0.402776</td>
      <td>2.489577</td>
      <td>0.000647</td>
      <td>1.403518</td>
    </tr>
    <tr>
      <th>4506</th>
      <td>(47209, 27966)</td>
      <td>(13176)</td>
      <td>0.010984</td>
      <td>0.161785</td>
      <td>0.004891</td>
      <td>0.445323</td>
      <td>2.752565</td>
      <td>0.003114</td>
      <td>1.511177</td>
    </tr>
    <tr>
      <th>4560</th>
      <td>(47209, 35951)</td>
      <td>(13176)</td>
      <td>0.003818</td>
      <td>0.161785</td>
      <td>0.001528</td>
      <td>0.400322</td>
      <td>2.474411</td>
      <td>0.000911</td>
      <td>1.397775</td>
    </tr>
    <tr>
      <th>4566</th>
      <td>(47209, 37646)</td>
      <td>(13176)</td>
      <td>0.004976</td>
      <td>0.161785</td>
      <td>0.002000</td>
      <td>0.402025</td>
      <td>2.484940</td>
      <td>0.001195</td>
      <td>1.401757</td>
    </tr>
    <tr>
      <th>4582</th>
      <td>(39928, 47209)</td>
      <td>(13176)</td>
      <td>0.004016</td>
      <td>0.161785</td>
      <td>0.001764</td>
      <td>0.439208</td>
      <td>2.714771</td>
      <td>0.001114</td>
      <td>1.494700</td>
    </tr>
    <tr>
      <th>4682</th>
      <td>(28204, 16797)</td>
      <td>(24852)</td>
      <td>0.003187</td>
      <td>0.201259</td>
      <td>0.001562</td>
      <td>0.490231</td>
      <td>2.435818</td>
      <td>0.000921</td>
      <td>1.566869</td>
    </tr>
    <tr>
      <th>4693</th>
      <td>(45066, 16797)</td>
      <td>(24852)</td>
      <td>0.003243</td>
      <td>0.201259</td>
      <td>0.001590</td>
      <td>0.490399</td>
      <td>2.436652</td>
      <td>0.000938</td>
      <td>1.567385</td>
    </tr>
    <tr>
      <th>4703</th>
      <td>(47626, 16797)</td>
      <td>(24852)</td>
      <td>0.004942</td>
      <td>0.201259</td>
      <td>0.002093</td>
      <td>0.423574</td>
      <td>2.104618</td>
      <td>0.001099</td>
      <td>1.385678</td>
    </tr>
    <tr>
      <th>4710</th>
      <td>(16797, 47766)</td>
      <td>(24852)</td>
      <td>0.005457</td>
      <td>0.201259</td>
      <td>0.002276</td>
      <td>0.417161</td>
      <td>2.072752</td>
      <td>0.001178</td>
      <td>1.370431</td>
    </tr>
    <tr>
      <th>4715</th>
      <td>(49683, 16797)</td>
      <td>(24852)</td>
      <td>0.003735</td>
      <td>0.201259</td>
      <td>0.001662</td>
      <td>0.444834</td>
      <td>2.210254</td>
      <td>0.000910</td>
      <td>1.438743</td>
    </tr>
    <tr>
      <th>4785</th>
      <td>(28842, 20114)</td>
      <td>(26209)</td>
      <td>0.002472</td>
      <td>0.060080</td>
      <td>0.001042</td>
      <td>0.421611</td>
      <td>7.017504</td>
      <td>0.000894</td>
      <td>1.625065</td>
    </tr>
    <tr>
      <th>4792</th>
      <td>(20114, 31717)</td>
      <td>(26209)</td>
      <td>0.002523</td>
      <td>0.060080</td>
      <td>0.001073</td>
      <td>0.425463</td>
      <td>7.081618</td>
      <td>0.000922</td>
      <td>1.635960</td>
    </tr>
    <tr>
      <th>4798</th>
      <td>(20114, 47626)</td>
      <td>(26209)</td>
      <td>0.002734</td>
      <td>0.060080</td>
      <td>0.001127</td>
      <td>0.412108</td>
      <td>6.859342</td>
      <td>0.000962</td>
      <td>1.598799</td>
    </tr>
    <tr>
      <th>5150</th>
      <td>(21137, 49683)</td>
      <td>(24852)</td>
      <td>0.005023</td>
      <td>0.201259</td>
      <td>0.002062</td>
      <td>0.410570</td>
      <td>2.040004</td>
      <td>0.001051</td>
      <td>1.355107</td>
    </tr>
    <tr>
      <th>6049</th>
      <td>(49683, 27845)</td>
      <td>(24852)</td>
      <td>0.002620</td>
      <td>0.201259</td>
      <td>0.001103</td>
      <td>0.421044</td>
      <td>2.092048</td>
      <td>0.000576</td>
      <td>1.379623</td>
    </tr>
    <tr>
      <th>6067</th>
      <td>(47626, 28204)</td>
      <td>(24852)</td>
      <td>0.003530</td>
      <td>0.201259</td>
      <td>0.001505</td>
      <td>0.426433</td>
      <td>2.118823</td>
      <td>0.000795</td>
      <td>1.392585</td>
    </tr>
    <tr>
      <th>6073</th>
      <td>(28204, 47766)</td>
      <td>(24852)</td>
      <td>0.003991</td>
      <td>0.201259</td>
      <td>0.001811</td>
      <td>0.453911</td>
      <td>2.255352</td>
      <td>0.001008</td>
      <td>1.462656</td>
    </tr>
    <tr>
      <th>6079</th>
      <td>(49683, 28204)</td>
      <td>(24852)</td>
      <td>0.002984</td>
      <td>0.201259</td>
      <td>0.001381</td>
      <td>0.462859</td>
      <td>2.299811</td>
      <td>0.000780</td>
      <td>1.487021</td>
    </tr>
    <tr>
      <th>6126</th>
      <td>(34969, 49683)</td>
      <td>(24852)</td>
      <td>0.002818</td>
      <td>0.201259</td>
      <td>0.001135</td>
      <td>0.402675</td>
      <td>2.000775</td>
      <td>0.000568</td>
      <td>1.337195</td>
    </tr>
    <tr>
      <th>6173</th>
      <td>(43961, 47766)</td>
      <td>(24852)</td>
      <td>0.002869</td>
      <td>0.201259</td>
      <td>0.001159</td>
      <td>0.403769</td>
      <td>2.006213</td>
      <td>0.000581</td>
      <td>1.339650</td>
    </tr>
    <tr>
      <th>6190</th>
      <td>(45066, 47626)</td>
      <td>(24852)</td>
      <td>0.004108</td>
      <td>0.201259</td>
      <td>0.001679</td>
      <td>0.408697</td>
      <td>2.030695</td>
      <td>0.000852</td>
      <td>1.350813</td>
    </tr>
    <tr>
      <th>6197</th>
      <td>(45066, 47766)</td>
      <td>(24852)</td>
      <td>0.004756</td>
      <td>0.201259</td>
      <td>0.002065</td>
      <td>0.434318</td>
      <td>2.158002</td>
      <td>0.001108</td>
      <td>1.411996</td>
    </tr>
    <tr>
      <th>6202</th>
      <td>(45066, 49683)</td>
      <td>(24852)</td>
      <td>0.002723</td>
      <td>0.201259</td>
      <td>0.001225</td>
      <td>0.449759</td>
      <td>2.234724</td>
      <td>0.000677</td>
      <td>1.451620</td>
    </tr>
    <tr>
      <th>6251</th>
      <td>(49683, 47766)</td>
      <td>(24852)</td>
      <td>0.006543</td>
      <td>0.201259</td>
      <td>0.002769</td>
      <td>0.423214</td>
      <td>2.102829</td>
      <td>0.001452</td>
      <td>1.384813</td>
    </tr>
    <tr>
      <th>6293</th>
      <td>(28842, 47626)</td>
      <td>(26209)</td>
      <td>0.002886</td>
      <td>0.060080</td>
      <td>0.001159</td>
      <td>0.401561</td>
      <td>6.683790</td>
      <td>0.000986</td>
      <td>1.570621</td>
    </tr>
    <tr>
      <th>6300</th>
      <td>(28842, 47766)</td>
      <td>(26209)</td>
      <td>0.003199</td>
      <td>0.060080</td>
      <td>0.001286</td>
      <td>0.402152</td>
      <td>6.693615</td>
      <td>0.001094</td>
      <td>1.572171</td>
    </tr>
    <tr>
      <th>6312</th>
      <td>(47626, 31717)</td>
      <td>(26209)</td>
      <td>0.003462</td>
      <td>0.060080</td>
      <td>0.001418</td>
      <td>0.409586</td>
      <td>6.817353</td>
      <td>0.001210</td>
      <td>1.591967</td>
    </tr>
    <tr>
      <th>6397</th>
      <td>(47209, 21137, 21903)</td>
      <td>(13176)</td>
      <td>0.003469</td>
      <td>0.161785</td>
      <td>0.001439</td>
      <td>0.414738</td>
      <td>2.563516</td>
      <td>0.000877</td>
      <td>1.432205</td>
    </tr>
    <tr>
      <th>6411</th>
      <td>(47209, 21137, 27966)</td>
      <td>(13176)</td>
      <td>0.003389</td>
      <td>0.161785</td>
      <td>0.001662</td>
      <td>0.490270</td>
      <td>3.030382</td>
      <td>0.001113</td>
      <td>1.644428</td>
    </tr>
  </tbody>
</table>
</div>







---


## Future Remarks

#### Improving of Apriori Algorithm

Although Apriori is a significant break through historically, it suffers from a number of inefficiencies and trade-offs. Candidate generation generates larger number of subsets, so the algorithm scans the database too many times, so both the time and space complexity of this algorithms are very high, $O (2^{|D|})$, where $|D|$ is the total number of items present in the database. Although we many ways to solves, none were hopeful. All the method we tried, ended up at the crazy town of *np completeness*.

Calculating support of $L_1$ is easy, you can just calculate the all the support values and compare it to the minimum support. Now, Let's say, $A , B \in L_1$. We know that $\mbox{Supp}(A)$ is the just the probability of $A$ happening in the database. Now to find the $\mbox{Supp}(A \cup B)$.

$$
\begin{align}

\mbox{Supp}(A \cup B ) &\equiv P(A \cup B) \\\\
& = P(A) + P(B) - P(A \cap B)

\end {align}
$$

So, to get support value without scanning database again, we need to know  $P(A \cap B)$.

Now to know  $P(A \cap B)$, we need to know $P (A\cup B)$, Now to know $P(A \cap B)$ we need to know $P( A \cup B)$, unless we have some other equation, we can't get solutions of two variable from one equation. We don't have any other information. So the only way to get $P(A \cup B)$ is to scan database again. There are some clever work around for some specific cases, but there are none for general case. 

If you get a thought like "Yeah, why don't we just calculate all the unions in just one scan of the dataset". Sure, you can and you will get a **constant time** algorithm, but the memory it would take to store all that information is astronomical. It would be unfair to this number to call it astronomical, because astronomical would be so tiny compared to this number with just small input sizes. We tried alot to improve this algorithm, we always ended up one of these extremes.

Unless someone with a giga-brain came up a super genius proof  to show $p = np$, I don't think there is any general solution!!


#### Instacart Dataset

I think there is much more potential here, if we have some beefier hardware, we would be able to analysis this data more effectively, and may be we would be able to get some more useful data. We tried to implement our own apriori algorithm, memory management is quite complicated and out of the scope of this project, so we decided to use some standard machine learning library instead. Even with that low memory algorithm, it took like 8GB memory and 10 minutes of time. We can improve in this regard.

