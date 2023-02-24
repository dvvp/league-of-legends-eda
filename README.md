# More Dragons = More Kills? (League of Legends EDA)
This is a project for DSC 80 at UCSD by Devin Pham and Minh Vo

---

### Code

#### Imports
```python
import pandas as pd
import numpy as np
import os
```

```python
import plotly.express as px
pd.options.plotting.backend = 'plotly'
```

#### Reading dataset
```python
path = os.path.join('data', '2022_LoL_esports_match_data_from_OraclesElixir.csv')
lol_2022 = pd.read_csv(path, low_memory=False) # low_memory=False allows pandas to process null values
```

---

### Introduction

Questions to ask:

- Does the amount of dragons killed correlate to more team kills?

- Does killing the first dragon mean you are more likely to win the game?

- Do winning teams have a higher proportion of gold spent?

- Which league has the most action-packed games?


Question we plan to investigate further:

- **Does the amount of dragons used correlate to more team kills?**

Killing a dragon in the game grants your team small buffs in the game that would give you an edge over your opponents. By having more dragons, theoretically, your team should accumulate a higher amount of kills because of the multiple buffs that are granted.

The dataset that we will be using is the 2022 [League of Legends](https://en.wikipedia.org/wiki/League_of_Legends) esports match data from the website [Oracle's Elixir](https://oracleselixir.com/tools/downloads) (OE) as it is the most recent complete dataset that they have. The first 5 rows of the DataFrame and the DataFrame's dimensions are displayed below: