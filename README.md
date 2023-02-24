# More Dragons = More Kills? (League of Legends EDA)
Created by Devin Pham and Minh Vo

---

### Code

Importing the necessary Python libraries...
```python
import pandas as pd
import numpy as np
import os
```

```python
import plotly.express as px
pd.options.plotting.backend = 'plotly'
```

Reading in our dataset...
```python
path = os.path.join('data', '2022_LoL_esports_match_data_from_OraclesElixir.csv')
lol_2022 = pd.read_csv(path, low_memory=False)  # low_memory=False allows pandas to process null values
```

---

### Introduction

Questions to ask:

- Does the amount of dragons killed correlate to more team kills?

- Does killing the first dragon mean you are more likely to win the game?

- Do winning teams have a higher proportion of gold spent?

- Which league has the most action-packed games?


Question we plan to investigate further:

- **Does the amount of dragons kills correlate to more team kills?**

Killing a dragon in the game grants your team small buffs in the game that would give you an edge over your opponents. By having more dragons, theoretically, your team should accumulate a higher amount of kills because of the multiple buffs that are granted Answering this question will provide professional League of Legends teams insight on one of the many factors that may influence their success in gaming tournaments.

The dataset that we will be using is the 2022 [League of Legends](https://en.wikipedia.org/wiki/League_of_Legends) esports match data from the website [Oracle's Elixir](https://oracleselixir.com/tools/downloads) (OE) as it is the most recent complete dataset that they have. The first 5 rows of the DataFrame and the DataFrame's dimensions are displayed below:

```python
lol_2022.head()
```

| gameid                | datacompleteness   |   url | league   |   year | split   |   playoffs | date                |   game |   patch |   participantid | side   | position   | playername   | playerid                                  | teamname                 | teamid                                  | champion   | ban1   | ban2    | ban3   | ban4   | ban5   |   gamelength |   result |   kills |   deaths |   assists |   teamkills |   teamdeaths |   doublekills |   triplekills |   quadrakills |   pentakills |   firstblood |   firstbloodkill |   firstbloodassist |   firstbloodvictim |   team kpm |   ckpm |   firstdragon |   dragons |   opp_dragons |   elementaldrakes |   opp_elementaldrakes |   infernals |   mountains |   clouds |   oceans |   chemtechs |   hextechs |   dragons (type unknown) |   elders |   opp_elders |   firstherald |   heralds |   opp_heralds |   firstbaron |   barons |   opp_barons |   firsttower |   towers |   opp_towers |   firstmidtower |   firsttothreetowers |   turretplates |   opp_turretplates |   inhibitors |   opp_inhibitors |   damagetochampions |     dpm |   damageshare |   damagetakenperminute |   damagemitigatedperminute |   wardsplaced |    wpm |   wardskilled |   wcpm |   controlwardsbought |   visionscore |   vspm |   totalgold |   earnedgold |   earned gpm |   earnedgoldshare |   goldspent |   gspd |   total cs |   minionkills |   monsterkills |   monsterkillsownjungle |   monsterkillsenemyjungle |   cspm |   goldat10 |   xpat10 |   csat10 |   opp_goldat10 |   opp_xpat10 |   opp_csat10 |   golddiffat10 |   xpdiffat10 |   csdiffat10 |   killsat10 |   assistsat10 |   deathsat10 |   opp_killsat10 |   opp_assistsat10 |   opp_deathsat10 |   goldat15 |   xpat15 |   csat15 |   opp_goldat15 |   opp_xpat15 |   opp_csat15 |   golddiffat15 |   xpdiffat15 |   csdiffat15 |   killsat15 |   assistsat15 |   deathsat15 |   opp_killsat15 |   opp_assistsat15 |   opp_deathsat15 |
|:----------------------|:-------------------|------:|:---------|-------:|:--------|-----------:|:--------------------|-------:|--------:|----------------:|:-------|:-----------|:-------------|:------------------------------------------|:-------------------------|:----------------------------------------|:-----------|:-------|:--------|:-------|:-------|:-------|-------------:|---------:|--------:|---------:|----------:|------------:|-------------:|--------------:|--------------:|--------------:|-------------:|-------------:|-----------------:|-------------------:|-------------------:|-----------:|-------:|--------------:|----------:|--------------:|------------------:|----------------------:|------------:|------------:|---------:|---------:|------------:|-----------:|-------------------------:|---------:|-------------:|--------------:|----------:|--------------:|-------------:|---------:|-------------:|-------------:|---------:|-------------:|----------------:|---------------------:|---------------:|-------------------:|-------------:|-----------------:|--------------------:|--------:|--------------:|-----------------------:|---------------------------:|--------------:|-------:|--------------:|-------:|---------------------:|--------------:|-------:|------------:|-------------:|-------------:|------------------:|------------:|-------:|-----------:|--------------:|---------------:|------------------------:|--------------------------:|-------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|
| ESPORTSTMNT01_2690210 | complete           |   nan | LCK CL   |   2022 | Spring  |          0 | 2022-01-10 07:44:08 |      1 |   12.01 |               1 | Blue   | top        | Soboro       | oe:player:38e0af7278d6769d0c81d7c4b47ac1e | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | Renekton   | Karma  | Caitlyn | Syndra | Thresh | Lulu   |         1713 |        0 |       2 |        3 |         2 |           9 |           19 |             0 |             0 |             0 |            0 |            0 |                0 |                  0 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |               15768 | 552.294 |     0.278784  |               1072.4   |                    777.793 |             8 | 0.2802 |             6 | 0.2102 |                    5 |            26 | 0.9107 |       10934 |         7164 |      250.928 |          0.253859 |       10275 |    nan |        231 |           220 |             11 |                     nan |                       nan | 8.0911 |       3228 |     4909 |       89 |           3176 |         4953 |           81 |             52 |          -44 |            8 |           0 |             0 |            0 |               0 |                 0 |                0 |       5025 |     7560 |      135 |           4634 |         7215 |          121 |            391 |          345 |           14 |           0 |             1 |            0 |               0 |                 1 |                0 |
| ESPORTSTMNT01_2690210 | complete           |   nan | LCK CL   |   2022 | Spring  |          0 | 2022-01-10 07:44:08 |      1 |   12.01 |               2 | Blue   | jng        | Raptor       | oe:player:637ed20b1e41be1c51bd1a4cb211357 | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | Xin Zhao   | Karma  | Caitlyn | Syndra | Thresh | Lulu   |         1713 |        0 |       2 |        5 |         6 |           9 |           19 |             0 |             0 |             0 |            0 |            1 |                0 |                  1 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                1 |               11765 | 412.084 |     0.208009  |                944.273 |                    650.158 |             6 | 0.2102 |            18 | 0.6305 |                    6 |            48 | 1.6813 |        9138 |         5368 |      188.021 |          0.19022  |        8750 |    nan |        148 |            33 |            115 |                     nan |                       nan | 5.1839 |       3429 |     3484 |       58 |           2944 |         3052 |           63 |            485 |          432 |           -5 |           1 |             2 |            0 |               0 |                 0 |                1 |       5366 |     5320 |       89 |           4825 |         5595 |          100 |            541 |         -275 |          -11 |           2 |             3 |            2 |               0 |                 5 |                1 |
| ESPORTSTMNT01_2690210 | complete           |   nan | LCK CL   |   2022 | Spring  |          0 | 2022-01-10 07:44:08 |      1 |   12.01 |               3 | Blue   | mid        | Feisty       | oe:player:d1ae0e2f9f3ac1e0e0cdcb86504ca77 | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | LeBlanc    | Karma  | Caitlyn | Syndra | Thresh | Lulu   |         1713 |        0 |       2 |        2 |         3 |           9 |           19 |             0 |             0 |             0 |            0 |            0 |                0 |                  0 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |               14258 | 499.405 |     0.252086  |                581.646 |                    227.776 |            19 | 0.6655 |             7 | 0.2452 |                    7 |            29 | 1.0158 |        9715 |         5945 |      208.231 |          0.210665 |        8725 |    nan |        193 |           177 |             16 |                     nan |                       nan | 6.7601 |       3283 |     4556 |       81 |           3121 |         4485 |           81 |            162 |           71 |            0 |           0 |             1 |            0 |               0 |                 0 |                1 |       5118 |     6942 |      120 |           5593 |         6789 |          119 |           -475 |          153 |            1 |           0 |             3 |            0 |               3 |                 3 |                2 |
| ESPORTSTMNT01_2690210 | complete           |   nan | LCK CL   |   2022 | Spring  |          0 | 2022-01-10 07:44:08 |      1 |   12.01 |               4 | Blue   | bot        | Gamin        | oe:player:998b3e49b01ecc41eacc392477a98cf | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | Samira     | Karma  | Caitlyn | Syndra | Thresh | Lulu   |         1713 |        0 |       2 |        4 |         2 |           9 |           19 |             0 |             0 |             0 |            0 |            1 |                0 |                  1 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |               11106 | 389.002 |     0.196358  |                463.853 |                    218.879 |            12 | 0.4203 |             6 | 0.2102 |                    4 |            25 | 0.8757 |       10605 |         6835 |      239.405 |          0.242201 |       10425 |    nan |        226 |           208 |             18 |                     nan |                       nan | 7.9159 |       3600 |     3103 |       78 |           3304 |         2838 |           90 |            296 |          265 |          -12 |           1 |             1 |            0 |               0 |                 0 |                0 |       5461 |     4591 |      115 |           6254 |         5934 |          149 |           -793 |        -1343 |          -34 |           2 |             1 |            2 |               3 |                 3 |                0 |
| ESPORTSTMNT01_2690210 | complete           |   nan | LCK CL   |   2022 | Spring  |          0 | 2022-01-10 07:44:08 |      1 |   12.01 |               5 | Blue   | sup        | Loopy        | oe:player:e9741b3a238723ea6380ef2113fae63 | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | Leona      | Karma  | Caitlyn | Syndra | Thresh | Lulu   |         1713 |        0 |       1 |        5 |         6 |           9 |           19 |             0 |             0 |             0 |            0 |            1 |                1 |                  0 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |                3663 | 128.301 |     0.0647631 |                475.026 |                    490.123 |            29 | 1.0158 |            14 | 0.4904 |                   11 |            69 | 2.4168 |        6678 |         2908 |      101.856 |          0.103054 |        6395 |    nan |         42 |            42 |              0 |                     nan |                       nan | 1.4711 |       2678 |     2161 |       16 |           2150 |         2748 |           15 |            528 |         -587 |            1 |           1 |             1 |            0 |               0 |                 0 |                1 |       3836 |     3588 |       28 |           3393 |         4085 |           21 |            443 |         -497 |            7 |           1 |             2 |            2 |               0 |                 6 |                2 |

```python
lol_2022.shape  # (rows, columns)
```
(149232, 123)

The original dataset contains 149232 rows and 123 columns. We will be using 3 columns. Using OE's [definitions page](https://oracleselixir.com/tools/downloads) along with our background on the game, we defined these columns below:

| Column           | Type   | Description                    |
|:-----------------|:-------|:-------------------------------|
| gameid           | object | Unique identifier to each game |
| datacompleteness | object | Whether the data is complete   |
| url              | object | Webpage address to game        |


---

### Cleaning and EDA
#### Data Cleaning
Since many of these columns should be of type `bool`, but they are not, we will convert these columns to booleans.

```python
for col in lol_2022.columns:
    if ((lol_2022[col] == 0) | (lol_2022[col] == 1)).sum() == lol_2022.shape[0]:
        lol_2022[col] = lol_2022[col].astype(bool)

lol_2022.head()
```

| gameid                | datacompleteness   |   url | league   |   year | split   | playoffs   | date                |   game |   patch |   participantid | side   | position   | playername   | playerid                                  | teamname                 | teamid                                  | champion   | ban1   | ban2    | ban3   | ban4   | ban5   |   gamelength | result   |   kills |   deaths |   assists |   teamkills |   teamdeaths |   doublekills |   triplekills |   quadrakills |   pentakills |   firstblood |   firstbloodkill |   firstbloodassist |   firstbloodvictim |   team kpm |   ckpm |   firstdragon |   dragons |   opp_dragons |   elementaldrakes |   opp_elementaldrakes |   infernals |   mountains |   clouds |   oceans |   chemtechs |   hextechs |   dragons (type unknown) |   elders |   opp_elders |   firstherald |   heralds |   opp_heralds |   firstbaron |   barons |   opp_barons |   firsttower |   towers |   opp_towers |   firstmidtower |   firsttothreetowers |   turretplates |   opp_turretplates |   inhibitors |   opp_inhibitors |   damagetochampions |     dpm |   damageshare |   damagetakenperminute |   damagemitigatedperminute |   wardsplaced |    wpm |   wardskilled |   wcpm |   controlwardsbought |   visionscore |   vspm |   totalgold |   earnedgold |   earned gpm |   earnedgoldshare |   goldspent |   gspd |   total cs |   minionkills |   monsterkills |   monsterkillsownjungle |   monsterkillsenemyjungle |   cspm |   goldat10 |   xpat10 |   csat10 |   opp_goldat10 |   opp_xpat10 |   opp_csat10 |   golddiffat10 |   xpdiffat10 |   csdiffat10 |   killsat10 |   assistsat10 |   deathsat10 |   opp_killsat10 |   opp_assistsat10 |   opp_deathsat10 |   goldat15 |   xpat15 |   csat15 |   opp_goldat15 |   opp_xpat15 |   opp_csat15 |   golddiffat15 |   xpdiffat15 |   csdiffat15 |   killsat15 |   assistsat15 |   deathsat15 |   opp_killsat15 |   opp_assistsat15 |   opp_deathsat15 |
|:----------------------|:-------------------|------:|:---------|-------:|:--------|:-----------|:--------------------|-------:|--------:|----------------:|:-------|:-----------|:-------------|:------------------------------------------|:-------------------------|:----------------------------------------|:-----------|:-------|:--------|:-------|:-------|:-------|-------------:|:---------|--------:|---------:|----------:|------------:|-------------:|--------------:|--------------:|--------------:|-------------:|-------------:|-----------------:|-------------------:|-------------------:|-----------:|-------:|--------------:|----------:|--------------:|------------------:|----------------------:|------------:|------------:|---------:|---------:|------------:|-----------:|-------------------------:|---------:|-------------:|--------------:|----------:|--------------:|-------------:|---------:|-------------:|-------------:|---------:|-------------:|----------------:|---------------------:|---------------:|-------------------:|-------------:|-----------------:|--------------------:|--------:|--------------:|-----------------------:|---------------------------:|--------------:|-------:|--------------:|-------:|---------------------:|--------------:|-------:|------------:|-------------:|-------------:|------------------:|------------:|-------:|-----------:|--------------:|---------------:|------------------------:|--------------------------:|-------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|
| ESPORTSTMNT01_2690210 | complete           |   nan | LCK CL   |   2022 | Spring  | False      | 2022-01-10 07:44:08 |      1 |   12.01 |               1 | Blue   | top        | Soboro       | oe:player:38e0af7278d6769d0c81d7c4b47ac1e | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | Renekton   | Karma  | Caitlyn | Syndra | Thresh | Lulu   |         1713 | False    |       2 |        3 |         2 |           9 |           19 |             0 |             0 |             0 |            0 |            0 |                0 |                  0 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |               15768 | 552.294 |     0.278784  |               1072.4   |                    777.793 |             8 | 0.2802 |             6 | 0.2102 |                    5 |            26 | 0.9107 |       10934 |         7164 |      250.928 |          0.253859 |       10275 |    nan |        231 |           220 |             11 |                     nan |                       nan | 8.0911 |       3228 |     4909 |       89 |           3176 |         4953 |           81 |             52 |          -44 |            8 |           0 |             0 |            0 |               0 |                 0 |                0 |       5025 |     7560 |      135 |           4634 |         7215 |          121 |            391 |          345 |           14 |           0 |             1 |            0 |               0 |                 1 |                0 |
| ESPORTSTMNT01_2690210 | complete           |   nan | LCK CL   |   2022 | Spring  | False      | 2022-01-10 07:44:08 |      1 |   12.01 |               2 | Blue   | jng        | Raptor       | oe:player:637ed20b1e41be1c51bd1a4cb211357 | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | Xin Zhao   | Karma  | Caitlyn | Syndra | Thresh | Lulu   |         1713 | False    |       2 |        5 |         6 |           9 |           19 |             0 |             0 |             0 |            0 |            1 |                0 |                  1 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                1 |               11765 | 412.084 |     0.208009  |                944.273 |                    650.158 |             6 | 0.2102 |            18 | 0.6305 |                    6 |            48 | 1.6813 |        9138 |         5368 |      188.021 |          0.19022  |        8750 |    nan |        148 |            33 |            115 |                     nan |                       nan | 5.1839 |       3429 |     3484 |       58 |           2944 |         3052 |           63 |            485 |          432 |           -5 |           1 |             2 |            0 |               0 |                 0 |                1 |       5366 |     5320 |       89 |           4825 |         5595 |          100 |            541 |         -275 |          -11 |           2 |             3 |            2 |               0 |                 5 |                1 |
| ESPORTSTMNT01_2690210 | complete           |   nan | LCK CL   |   2022 | Spring  | False      | 2022-01-10 07:44:08 |      1 |   12.01 |               3 | Blue   | mid        | Feisty       | oe:player:d1ae0e2f9f3ac1e0e0cdcb86504ca77 | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | LeBlanc    | Karma  | Caitlyn | Syndra | Thresh | Lulu   |         1713 | False    |       2 |        2 |         3 |           9 |           19 |             0 |             0 |             0 |            0 |            0 |                0 |                  0 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |               14258 | 499.405 |     0.252086  |                581.646 |                    227.776 |            19 | 0.6655 |             7 | 0.2452 |                    7 |            29 | 1.0158 |        9715 |         5945 |      208.231 |          0.210665 |        8725 |    nan |        193 |           177 |             16 |                     nan |                       nan | 6.7601 |       3283 |     4556 |       81 |           3121 |         4485 |           81 |            162 |           71 |            0 |           0 |             1 |            0 |               0 |                 0 |                1 |       5118 |     6942 |      120 |           5593 |         6789 |          119 |           -475 |          153 |            1 |           0 |             3 |            0 |               3 |                 3 |                2 |
| ESPORTSTMNT01_2690210 | complete           |   nan | LCK CL   |   2022 | Spring  | False      | 2022-01-10 07:44:08 |      1 |   12.01 |               4 | Blue   | bot        | Gamin        | oe:player:998b3e49b01ecc41eacc392477a98cf | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | Samira     | Karma  | Caitlyn | Syndra | Thresh | Lulu   |         1713 | False    |       2 |        4 |         2 |           9 |           19 |             0 |             0 |             0 |            0 |            1 |                0 |                  1 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |               11106 | 389.002 |     0.196358  |                463.853 |                    218.879 |            12 | 0.4203 |             6 | 0.2102 |                    4 |            25 | 0.8757 |       10605 |         6835 |      239.405 |          0.242201 |       10425 |    nan |        226 |           208 |             18 |                     nan |                       nan | 7.9159 |       3600 |     3103 |       78 |           3304 |         2838 |           90 |            296 |          265 |          -12 |           1 |             1 |            0 |               0 |                 0 |                0 |       5461 |     4591 |      115 |           6254 |         5934 |          149 |           -793 |        -1343 |          -34 |           2 |             1 |            2 |               3 |                 3 |                0 |
| ESPORTSTMNT01_2690210 | complete           |   nan | LCK CL   |   2022 | Spring  | False      | 2022-01-10 07:44:08 |      1 |   12.01 |               5 | Blue   | sup        | Loopy        | oe:player:e9741b3a238723ea6380ef2113fae63 | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | Leona      | Karma  | Caitlyn | Syndra | Thresh | Lulu   |         1713 | False    |       1 |        5 |         6 |           9 |           19 |             0 |             0 |             0 |            0 |            1 |                1 |                  0 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |                3663 | 128.301 |     0.0647631 |                475.026 |                    490.123 |            29 | 1.0158 |            14 | 0.4904 |                   11 |            69 | 2.4168 |        6678 |         2908 |      101.856 |          0.103054 |        6395 |    nan |         42 |            42 |              0 |                     nan |                       nan | 1.4711 |       2678 |     2161 |       16 |           2150 |         2748 |           15 |            528 |         -587 |            1 |           1 |             1 |            0 |               0 |                 0 |                1 |       3836 |     3588 |       28 |           3393 |         4085 |           21 |            443 |         -497 |            7 |           1 |             2 |            2 |               0 |                 6 |                2 |

This dataset combines game, player, and team data together. We found that each 'gameid' corresponds to up to 12 rows (5 rows for players in one team, 5 rows for players in another team, and 2 rows containing summary data for the two teams). We can separate each dataset into three DataFrames to distinguish each type of data. The first five columns of each DataFrame are displayed below:

{: .note }
In each of the following DataFrames, we also dropped all of the columns that have all missing values since these values are missing by design (MD).

```python
games = lol_2022[lol_2022['gameid'].str.contains('game')]
games.drop(columns=games.columns[games.isna().all()])
games.head()
```