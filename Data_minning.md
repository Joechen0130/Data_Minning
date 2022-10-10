

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
 Use efficient ==data structures== to store the candidates or transactions
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





