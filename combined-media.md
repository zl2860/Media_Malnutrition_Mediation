combined\_media
================
kai Wang, Shutong Huo, Yuao Yang, Zongchao Liu
11/15/2020

# Import data

This part followed the previous modifications. To run the codes you have
to have `chwmhh18`.

# Combine Media Variables

Combination Rules:

Newly added variables:

  - t\_1 : magazine

  - t\_2 : maximum value for the usage of `computer & internet & phone`

  - t\_3 : maximum value for the usage of `radio & tele`

This combination rule is a simple trial and can be changed later based
on other domain knowledge.

``` r
### UB2年龄 welevel1母亲教育  wealthp家庭财富 HL4性别 HH6城乡 HH7省份
reg_data = chwmhh %>%
  dplyr::select(stunting, magazine, radio, tele, computer, internet, phone, age, welevel1, wealthp, sex, urbanrural, province,WS9,WB4, UB2, MN36,MN39A,MN39C,MN39G,MN39H,MN39I) %>% # 想加变量在这加！
  # combine media⬇
  mutate(t_1 = factor(magazine),
         t_2 = factor(pmax(computer, internet, phone)),
         t_3 = factor(pmax(radio, tele)))
```

# Analysis

  - use `reg_data` for future analysis. It has combined `media` usage.

<!-- end list -->

``` r
skimr::skim(reg_data)
```

    ## Skim summary statistics
    ##  n obs: 20002 
    ##  n variables: 25 
    ##  group variables:  
    ## 
    ## ── Variable type:character ───────────────────────────────────────────────────────────
    ##  variable missing complete     n min max empty n_unique
    ##     MN39A      76    19926 20002   1   1     0        3
    ##     MN39C      76    19926 20002   1   1     0        3
    ##     MN39G      76    19926 20002   1   1     0        3
    ##     MN39H      76    19926 20002   1   1     0        3
    ##     MN39I      76    19926 20002   1   1     0        3
    ## 
    ## ── Variable type:factor ──────────────────────────────────────────────────────────────
    ##    variable missing complete     n n_unique
    ##    province      76    19926 20002       26
    ##         sex      76    19926 20002        2
    ##    stunting     584    19418 20002        2
    ##         t_1      76    19926 20002        4
    ##         t_2      76    19926 20002        2
    ##         t_3      76    19926 20002        4
    ##  urbanrural      76    19926 20002        2
    ##                           top_counts ordered
    ##  16: 1008, 20: 954, 22: 893, 24: 889   FALSE
    ##            2: 10075, 1: 9851, NA: 76   FALSE
    ##           0: 10788, 1: 8630, NA: 584   FALSE
    ##      0: 18837, 1: 689, 2: 312, 3: 88   FALSE
    ##             2: 19768, 1: 158, NA: 76   FALSE
    ##  0: 14952, 3: 2188, 1: 1536, 2: 1250   FALSE
    ##            2: 14616, 1: 5310, NA: 76   FALSE
    ## 
    ## ── Variable type:integer ─────────────────────────────────────────────────────────────
    ##  variable missing complete     n  mean   sd p0 p25 p50 p75 p100     hist
    ##       age      76    19926 20002 1.93  1.42  0   1   2   3    4 ▇▇▁▇▁▇▁▇
    ##  computer      76    19926 20002 1.97  0.17  1   2   2   2    2 ▁▁▁▁▁▁▁▇
    ##  internet      76    19926 20002 1.98  0.13  1   2   2   2    2 ▁▁▁▁▁▁▁▇
    ##  magazine      76    19926 20002 0.079 0.36  0   0   0   0    3 ▇▁▁▁▁▁▁▁
    ##     phone      76    19926 20002 1.83  0.37  1   2   2   2    2 ▂▁▁▁▁▁▁▇
    ##     radio      76    19926 20002 0.38  0.85  0   0   0   0    3 ▇▁▁▁▁▁▁▁
    ##      tele      76    19926 20002 0.27  0.79  0   0   0   0    3 ▇▁▁▁▁▁▁▁
    ##   wealthp      76    19926 20002 2.57  1.38  1   1   2   4    5 ▇▆▁▅▁▅▁▃
    ##  welevel1      76    19926 20002 1.14  0.76  0   1   1   2    2 ▅▁▁▇▁▁▁▇
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────
    ##  variable missing complete     n  mean    sd p0 p25 p50 p75 p100     hist
    ##      MN36    5364    14638 20002  1.01 0.096  1   1   1   1    2 ▇▁▁▁▁▁▁▁
    ##       UB2      76    19926 20002  1.93 1.42   0   1   2   3    4 ▇▇▁▇▁▇▁▇
    ##       WB4      76    19926 20002 29.98 7.16  15  25  30  35   49 ▂▅▆▇▅▃▂▁
    ##       WS9      76    19926 20002  1.98 0.3    1   2   2   2    8 ▁▇▁▁▁▁▁▁
