+++
author = "Larissa"
categories = ["Python"]
tags = ["network-analysis", "twitter", "Spring2017", "jupyter-notebook"]
date = "2017-05-20"
description = "Identifying Influencer, Experts and Broker within the Network Structure of Twitter"
featuredpath = "date"
linktitle = ""
title = "Labelling Output"
type = "post"
+++

```python
import networkx as nx
import pylab as plt
import pandas as pd
from collections import defaultdict
```


```python
g = nx.read_edgelist("C:/Users/laris/Desktop/PROJECT-WM/output.txt", delimiter=",", create_using=nx.DiGraph(), nodetype=int)
```


```python
#udg = nx.Graph(g)
```


```python
def dict_sort(cent_dict, pos):
    # Create ordered tuple of centrality data
    cent_items=[(b,a) for (a,b) in cent_dict.items()]
    # Sort in descending order
    cent_items.sort()
    cent_items.reverse()
    return tuple(reversed(cent_items[0:pos]))
```


```python
def snowball_sampling(g, center, max_depth=1, current_depth=0, taboo_list=[], subsample=nx.DiGraph()):
    # if we have reached the depth limit of the search, bomb out.
    print(center, current_depth, max_depth, taboo_list)
    if current_depth==max_depth:
        print('out of depth')
        return subsample
    if center in taboo_list:
        print('taboo')
        return subsample #we've been here before
    else:
        taboo_list.append(center) # we shall never return
    #read_lj_friends(g, center)
    for node in g.neighbors(center):
        subsample.add_node(center)
        subsample.add_edge(center,node)
        snowball_sampling(g, node, current_depth=current_depth+1, max_depth=max_depth, taboo_list=taboo_list, subsample=subsample)
    return subsample
```

First, we try to seperate BROKER and IMPORTANT USERS, because we know that we can't really seperate between INFLUENCER and EXPERTS just on the follower/following network. We will define BROKER as accounts who have a special position in the network and are channelling a lot of traffic. IMPORTANT USERS are in this case just accounts with a high amount of follower and therefore a central (IN) position in the network. 

<h1>preprocessing</h1>


```python
nx.info(g)
```




    'Name: \nType: DiGraph\nNumber of nodes: 24101\nNumber of edges: 26958\nAverage in degree:   1.1185\nAverage out degree:   1.1185'




```python
print(g.number_of_selfloops())
g.remove_edges_from(g.selfloop_edges())
print(g.number_of_selfloops())
```

    3
    0
    


```python
#print(nx.number_weakly_connected_components(g))
#nx.weakly_connected_component_subgraphs(g, copy=True)
# bleibt offen, ist ueberhaupt notwending?
```

<h1>recognition of IMPORTANT nodes</h1>


```python
#for key in dict_sort(nx.in_degree_centrality(g), 4566): #1%
    #print(key)
```


```python
#1503
#sample = snowball_sampling(g, 1503, max_depth=2, taboo_list=[])
#nx.write_pajek(sample, "test_sample_1503_social_2.net")
```


```python
#2642
#sample = snowball_sampling(g, 88, max_depth=2, taboo_list=[])
#nx.write_pajek(sample, "test_sample_88_social_2.net")
```

<h2>generation of data frame for IMPORTANT / POPULAR nodes</h2>


```python
important_nodes = {}
zwischen = dict_sort(nx.in_degree_centrality(g), 241)
rev = defaultdict( list )
for v, k in zwischen:
    rev[k] = v
for node in g.nodes():
    if node in rev:
        important_nodes[node]=1
    else:
        important_nodes[node]=0
df_important = pd.DataFrame.from_dict(important_nodes, orient='index')
```


```python
#df_important.sum(0)
```


```python
#df_important.head(20)
```

<h1>recognition of BROKER nodes</h1>


```python
# funktioniert noch nicht
#betw_cent = nx.betweenness_centrality(g, normalized=True)
#print(dict_sort(betw_cent, 4566))
```


```python
broker_nodes = {}
b_zwischen = dict_sort(nx.betweenness_centrality(g, k=300), 241)
b_rev = defaultdict( list )
for v, k in b_zwischen:
    b_rev[k] = v
for node in g.nodes():
    if node in b_rev:
        broker_nodes[node]=1
    else:
        broker_nodes[node]=0
df_broker = pd.DataFrame.from_dict(broker_nodes, orient='index')
```


```python
#df_broker.head(20)
```


```python
#df_broker.sum(0)
```

<h1>merging of resutls</h1>


```python
result = pd.concat([df_important, df_broker], axis=1)
```


```python
#result.rename(index=str, columns={"A": "important", "B": "broker"})
# geht nicht?
```


```python
#result.head(20)
```


```python
result.sum(0)
```




    0    241
    0    241
    dtype: int64




```python
result.describe()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>24101.000000</td>
      <td>24101.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.010000</td>
      <td>0.010000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.099499</td>
      <td>0.099499</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
result.to_csv('result_output.csv')
```


```python
#nx.write_pajek(g, "twitter7_network.net")
```


```python
def plot_degree_distribution(): 
    degs = {} 
    for n in g.nodes(): 
        deg = g.degree(n) 
        if deg not in degs: 
            degs[deg] = 0 
        degs[deg] += 1 
    items = sorted(degs.items())
    items = sorted(degs.items()) 
    fig = plt.figure() 
    ax = fig.add_subplot(111) 
    ax.plot([k for (k,v) in items], [v for (k, v) in items]) 
    ax.set_xscale('log') 
    ax.set_yscale('log') 
    plt.title("twitter7 Network Follower Degree Distribution") 
    fig.savefig("degree_distribution_social_twitter7.png")

#plot_degree_distribution()

def plot_indegree_distribution(): 
    degs = {} 
    for n in g.nodes(): 
        deg = g.in_degree(n) 
        if deg not in degs: 
            degs[deg] = 0 
        degs[deg] += 1 
    items = sorted(degs.items())
    items = sorted(degs.items()) 
    fig = plt.figure() 
    ax = fig.add_subplot(111) 
    ax.plot([k for (k,v) in items], [v for (k, v) in items]) 
    ax.set_xscale('log') 
    ax.set_yscale('log') 
    plt.title("twitter7 Network Following In-Degree Distribution") 
    fig.savefig("INdegree_distribution_social_twitter7.png")

#plot_indegree_distribution()

def plot_outdegree_distribution(): 
    degs = {} 
    for n in g.nodes(): 
        deg = g.out_degree(n) 
        if deg not in degs: 
            degs[deg] = 0 
        degs[deg] += 1 
    items = sorted(degs.items())
    items = sorted(degs.items()) 
    fig = plt.figure() 
    ax = fig.add_subplot(111) 
    ax.plot([k for (k,v) in items], [v for (k, v) in items]) 
    ax.set_xscale('log') 
    ax.set_yscale('log') 
    plt.title("twitter7 Network Following Out-Degree Distribution") 
    fig.savefig("OUTdegree_distribution_social_twitter7.png")

#plot_outdegree_distribution()
```
