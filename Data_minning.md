

---
## Data Mining: Association Discovery
---
**Example Association Rule**
> If a customer buys diapers, in 60% of cases, he/she also buys beers. This happens in 3% of all transactions.
---
**The task**
>Given a set of transactions T, the goal of
association rule mining is to ==find== all rules having
>1. support ≥ minsup threshold
>2. confidence ≥ minconf threshold
---
**==Some Definition==**
>1. Itemset: 
◆A collection of one or more items
Example: {Milk, Bread, Diaper}
k itemset: An itemset that contains k items
>2. Support count: 
◆Frequency of occurrence of an itemset(在Transaction中出現的次數)
>3. Support:
◆ ==Fraction== of transactions that contain an itemset(比例)
>4. Frequent Itemset
◆An itemset whose support is ==greater than or equal to a minsup threshold==
>5. Confidence ( c )
◆ Measures how often items in Y appear in transactions that contain X (比例)
---
**Method**
>**Brute force approach**:
>step1. List all possible association rules
>Step2. Compute the support and confidence for each rule
>But Computationally prohibitive
---
**Example**
>![](https://i.imgur.com/cprpnLA.png)
>
>{Milk,Diaper} → {Beer} (s=0.4, c=0.67) 
>>Milk,Diaper,Beer = 3 T:5 s=0.4
>>Milk,Diaper = 2 c = 0.67
>>
>{Milk,Beer} → {Diaper} (s=0.4, c=1.0)
>{Diaper,Beer} → {Milk} (s=0.4, c=0.67)
>{Beer} → {Milk,Diaper} (s=0.4, c=0.67)
>{Diaper} → {Milk,Beer} (s=0.4, c=0.5)
>{Milk} → {Diaper,Beer} (s=0.4, c=0.5)
>
>Observations: 
>1. All the above rules are ==binary partitions== of the same itemset: {Milk, Diaper, Beer}
>2. Rules originating from the ==same itemset== have identical support but
can have different confidence
>3. Thus, we may ==decouple== the support and confidence requirements
>
>Problem Decomposition
>1. ==Find== all items (itemsets) with support larger than s % ==large itemset== L~k~
>2. Use the large itemsets to ==generate the rules== with conference >= c%
---
**Promblem comprehend**
>![](https://i.imgur.com/YbAT3r3.png)
>Match each transaction against every candidate
>Complexity ~ O(NMw) => Expensive since M = 2^d^
>
>1. Reduce the number of candidates(M)
 Use ==pruning== techniques to reduce M
>2. Reduce the number of transactions (N)
 Used by ==DHP and vertical based mining algorithms==
>3. Reduce the number of comparisons (NM)
 Use efficient ==data structures(hash structure)== to store the candidates or transactions
 No need to match every candidate against every
transaction
---
**Reducing Number of Candidates (M)**
>**Apriori principle**
>If an itemset is frequent, then all of its >subsets must also be frequent
>(if p then q equivalent if not q then not p)
>
>==so if itemsets is not frequent, then its superset must also be not frequent.==
>
>example:
>![](https://i.imgur.com/pbgptdp.png)
---
**Illustrating Apriori Principle step by step**
>Example
>![](https://i.imgur.com/qNroFn1.png)
>
>Suppose: Minimum Support count= 3
>
>**1-itemsets**(6)
>
>![](https://i.imgur.com/s43zJzn.png)
>
>Because Coke and Eggs are not greater than 3, they are be pruning.
>
>**2-itemsets**(6)
>
>![](https://i.imgur.com/TWIFtFf.png)
>
>Because {Beer, Bread} and {Beer, Milk} are not greater than 3, they are be pruning.
>
>**3-itemsets**(1)
>
>![](https://i.imgur.com/AkFFZJR.png)
>
>Because all are not greater than 3, they are be pruning.
>there are four candidcate itemset, but only {Bread, Diaper, Milk} is the candidate itemset. Because {Beer, Bread} and {Beer, Milk} are not candidate itemsets, we can prune the others.
>
> **Conclusion:** 
> 
> If every subset is considered: C^6^~1~+C^6^~2~+C^6^~3~ → 6+15+20=41
> With support-based pruning: 6+6+1=16
---
**Apriori Algorithm**
> 1. F~k~ : frequent k itemsets 
> 2. L~k~ : candidate k itemsets
> 
>**Algorithm:**
>
>Let k=1
>
>Generate F~1~ = {frequent 1 itemsets}
>
>Repeat until F~k~ is empty
>>**Candidate Generation** : Generate L~k+1~ from F~k~
>>
>>**Candidate Pruning** : Prune candidate itemsets in L~k+1~ containing subsets of length k that are infrequent
>>
>>**Support Counting** : Count the support of each candidate in L~k+1~ by scanning the DB
>>
>>**Candidate Elimination** : Eliminate candidates in L~k+1~ that are infrequent, leaving only those that are frequent => F~k+1~
>>
---
**Factors Affecting Complexity**
>1. Choice of minimum support threshold
>2. Dimensionality (number of items) of the data set
>3. Size of database
>4. Average transaction width
>
---
**FP Growth**
>**Bottlenecks of the Apriori approach**
>1. Breadth first (i.e., level wise) search
>2. Often generates a huge number of candidates
>
>**The FPGrowth Approach**
>1. Depth first search
>2. Avoid explicit candidate generation
---
**FP tree create**
>Example:
>
>TID Items bought (ordered) frequent items
>1. 100
f, a, c, d, g, i, m, p f, c, a, m, p
>2. 200
a, b, c, f, l, m, o f, c, a, b, m
>3. 300
b, f, h, j, o, w f, b
>4. 400
b, c, k, s, p c, b, p
>5. 500
a, f, c, e, l, p, m, n f, c, a, m, p
>
>min_support = 3
>1. Scan DB once, find frequent 1 itemset
> (delete frequent lower than 3)
> 
>![](https://i.imgur.com/5SwToD1.png)
>
>2. Sort frequent items in frequency descending
order, f list
> f list: F list = f c a b m p
>3. Scan DB again, construct FP tree
>
>![](https://i.imgur.com/f8SXbH5.png)
---
**From Conditional Pattern bases to Conditional
FP trees**
>![](https://i.imgur.com/6lMvlSN.png)
>
>Take m-conditional pattern base for example
>
>![](https://i.imgur.com/WqDl65F.png)
>
> And we can get m, fm, cm, am, fcm, fam, cam, fcam are frequent patterns
---
**Closed Itemset**
>Definition:
>
>An itemset X is closed if none of its immediate supersets has the same support as the itemset X.
>
>Example:
>
>hint: yellow label are closed itesets.
>
>![](https://i.imgur.com/8KFTgyQ.png)












