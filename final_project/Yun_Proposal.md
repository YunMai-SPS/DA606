DA606 final project
================
Yun Mai
March 23, 2017

``` r
knitr::opts_chunk$set(echo = TRUE)
```

### Data Preparation

Load packages

``` r
library(RCurl)
```

    ## Loading required package: bitops

``` r
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 3.3.3

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tidyr)
```

    ## 
    ## Attaching package: 'tidyr'

    ## The following object is masked from 'package:RCurl':
    ## 
    ##     complete

``` r
library(stringr)
```

    ## Warning: package 'stringr' was built under R version 3.3.3

``` r
library(knitr)
library(ggplot2)
```

    ## Warning: package 'ggplot2' was built under R version 3.3.3

``` r
# Load data from Github.
url <- getURL("https://raw.githubusercontent.com/YunMai-SPS/DA606_homework/master/final_project/ECLS_2011_K2.csv")
 
library(data.table)
```

    ## -------------------------------------------------------------------------

    ## data.table + dplyr code now lives in dtplyr.
    ## Please library(dtplyr)!

    ## -------------------------------------------------------------------------

    ## 
    ## Attaching package: 'data.table'

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     between, first, last

``` r
earlyedu <- fread(url, header = T, sep = ',')
head(earlyedu)
```

    ##     CHILDID  X6AGE X5AGE X4AGE X3AGE X2KAGE_R X1KAGE_R X_CHSEX_R X1RTHETK2
    ## 1: 10000001 103.66    NA 91.66    NA    79.76    72.39         1    0.1930
    ## 2: 10000002     NA    NA 89.56    NA    77.52    71.41         2   -0.7870
    ## 3: 10000003  96.26    NA 84.33    NA    73.51       NA         1        NA
    ## 4: 10000004 101.19    NA 89.23    NA    78.84    72.39         1   -1.7087
    ## 5: 10000005     NA    NA    NA    NA    65.06    59.51         2   -0.0734
    ## 6: 10000006 107.51    NA 94.82    NA    82.62    75.78         2   -1.4601
    ##    X2RTHETK2 X3RTHETK2 X4RTHETK2 X5RTHETK2 X6RTHETK2 X1MTHETK2 X2MTHETK2
    ## 1:    1.7614        NA    3.1523        NA    3.3637    0.5827    1.5384
    ## 2:    0.7452        NA    2.2023        NA        NA   -0.3473    0.8800
    ## 3:    0.3323        NA    1.1861        NA    2.0689        NA    1.0112
    ## 4:   -0.2922        NA    0.8632        NA    1.5117   -2.1245   -0.2834
    ## 5:    0.8013        NA        NA        NA        NA   -0.5545    0.6163
    ## 6:   -0.6792        NA    1.0132        NA    1.9750   -1.0616   -0.5379
    ##    X3MTHETK2 X4MTHETK2 X5MTHETK2 X6MTHETK2 X2STHETK2 X3STHETK2 X4STHETK2
    ## 1:        NA    2.7286        NA    3.2950    1.3738        NA    3.3354
    ## 2:        NA    2.0825        NA        NA    0.8811        NA    1.3122
    ## 3:        NA    2.2080        NA    2.8384    0.4244        NA    2.2479
    ## 4:        NA    1.1082        NA    2.8392    0.6509        NA    0.2933
    ## 5:        NA        NA        NA        NA    0.3996        NA        NA
    ## 6:        NA    0.4484        NA    1.0869   -0.6320        NA   -0.7748
    ##    X5STHETK2 X6STHETK2 X1DCCSTOT X2DCCSTOT X3DCCSTOT X4DCCSTOT X5DCCSSCR
    ## 1:        NA    2.5071        16        17        NA        16        NA
    ## 2:        NA        NA        17        16        NA        16        NA
    ## 3:        NA    2.3529        NA        14        NA        16        NA
    ## 4:        NA    1.7845        15        14        NA        17        NA
    ## 5:        NA        NA        14        17        NA        NA        NA
    ## 6:        NA   -0.2110        16        14        NA        18        NA
    ##    X6DCCSSCR X6REGION X5REGION X4REGION X3REGION X2REGION X1REGION
    ## 1:    7.3280       -2       -2       -2       -2       -2       -2
    ## 2:        NA       -2       -2       -2       -2       -2       -2
    ## 3:    8.2395       -2       -2       -2       -2       -2       -2
    ## 4:    7.2868       -2       -2       -2       -2       -2       -2
    ## 5:        NA       -2       -2       -2       -2       -2       -2
    ## 6:    5.3872       -2       -2       -2       -2       -2       -2
    ##    X6LOCALE X5LOCALE X4LOCALE X3LOCALE X2LOCALE X1LOCALE X6PAR1EMP_I
    ## 1:        4       NA        4       NA        4        4           4
    ## 2:       NA       NA        2       NA        2        2          NA
    ## 3:        2       NA        2       NA        2       NA           1
    ## 4:        4       NA        4       NA        4        4          NA
    ## 5:       NA       NA       NA       NA        2        2          NA
    ## 6:        4       NA        4       NA        4        4           3
    ##    X4PAR1EMP_I X1PAR1EMP X4PAR1ED_I X6PAR1OCC_I X4PAR1OCC_I X1PAR1OCC_I
    ## 1:           4         4          5          -1          -1          -1
    ## 2:           4         4          5          NA          -1          -1
    ## 3:           1        NA          9           7           7          NA
    ## 4:           1         1          5          NA          13          13
    ## 5:          NA         1         NA          NA          NA           4
    ## 6:           1         1          3          20          19          19
    ##    X6PAR2OCC_I X4PAR2OCC_I X1PAR2OCC_I X6PAR1SCR_I X4PAR1SCR_I X1PAR1SCR_I
    ## 1:          19          19          19       -1.00       -1.00       -1.00
    ## 2:          NA           1           1          NA       -1.00       -1.00
    ## 3:          -1          -1          NA       77.50       77.50          NA
    ## 4:          NA          16          16          NA       38.18       38.18
    ## 5:          NA          NA           1          NA          NA       59.00
    ## 6:          -1          -1          -1       35.92       33.42       33.42
    ##    X6DISTPOV X4DISTPOV X_DISTPOV S2GIFNOG S2GIFNO S4GIFNO S4GIFNOG
    ## 1:        38        39        45       -9      -9       1        0
    ## 2:        NA         7         7        1       1       1        0
    ## 3:        12        11         9        2       1       1        0
    ## 4:        20        20        17       -9      -9       0        0
    ## 5:        NA        NA        -1       NA      NA      NA       NA
    ## 6:        10        12        11       -9      -9       0        0
    ##    S6GIFNOG S6GIFNO A1ATNDPR S6TTLPRE S4TTLPRE A1INKNDR A1VSTK A1SHRTN
    ## 1:        0       0        3        1        2        1      1       1
    ## 2:       NA      NA        3       NA       -1        1      1       2
    ## 3:        0       1       NA       -1       -1       NA     NA      NA
    ## 4:        0       0        5        1        1        2      1       2
    ## 5:       NA      NA        5       NA       NA        1      1       2
    ## 6:        0       0        4        2        1        2      1       1
    ##    A1STAGGR A1PRNTOR A1HMEVST A1COMM A1IDCOLO A1FOLWDR A1ALPHBT A1SITSTI
    ## 1:        2        1        2      3        2        3        2        3
    ## 2:        2        1        2      4        3        4        3        3
    ## 3:       NA       NA       NA     NA       NA       NA       NA       NA
    ## 4:        2        1        2      5        4        4        5        5
    ## 5:        2        1        2      5        4        5        5        5
    ## 6:        1        1        2      4        4        4        3        4
    ##    A1SENSTI A1ENGLAN A1NOTDSR A1PENCIL A1PRBLMS A1SHARE A1CNT20 A1FNSHT
    ## 1:        3        2        3        3        2       3       2       3
    ## 2:        3        4        4        2        4       5       3       3
    ## 3:       NA       NA       NA       NA       NA      NA      NA      NA
    ## 4:        3       -9        5        5        3       4       4       4
    ## 5:        4        4        5        4        3       5       4       4
    ## 6:        4        3        3        4        3       4       3       4
    ##    S6CCLSDE S6TT1CLA S4TT1CLA S2TT1CLA A1FRMLIN A1ALPHBF A1LRNREA A1TCHPRN
    ## 1:        2        1        1        2        3        2        4        4
    ## 2:       NA       NA       -1        2        2        3        4        2
    ## 3:        1       -1       -1       -1       NA       NA       NA       NA
    ## 4:        1        1        2        2        5        5        5        4
    ## 5:       NA       NA       NA       NA        5        5        5        5
    ## 6:        1        1        1        2        3        3        4        4
    ##    A1PRCTWR A1HMWRK A1READAT A1CNTRLC A1ENJOY A1MKDIFF A1TEACH A1CLSSIZ
    ## 1:        3       2        4        4       5        5       4        5
    ## 2:        2       1        5        5       5        5       5        2
    ## 3:       NA      NA       NA       NA      NA       NA      NA       NA
    ## 4:        4       2        5        5       5        5       5        5
    ## 5:        5       5        5        4       5        5       5        5
    ## 6:        3       2        4        4       5        4       4        5
    ##    A1NATEXM A1EARLY A1ELEM A1DEVLP A1MTHDRD A1MTHDMA A1MTHDSC A1RSPINT
    ## 1:        1       1      1       1        1        1        1        1
    ## 2:        1       1      1       1        1        1        1        2
    ## 3:       NA      NA     NA      NA       NA       NA       NA       NA
    ## 4:        2       2      2       1        2        2        2        2
    ## 5:        1       1      2       1        1        1        1        2
    ## 6:        2       2      1       1        1        1        1        2
    ##    A1INTSRV A1STATCT A1HIGHQL A1YRBORN A1HGHSTD A1HGHPAR A1YRSCH P3DOMATH
    ## 1:        2        1        3        7        5        6       5       NA
    ## 2:        2        1        1        2        5        5      27       NA
    ## 3:       NA       NA       NA       NA       NA       NA      NA       NA
    ## 4:        2        1        1        5        5        5       6       NA
    ## 5:        2        1        2        5        4        3       7       NA
    ## 6:        2        1        1        8        5        2       8       NA
    ##    P3DOWRIT P3RDBKTC P3HWLGRD P3RDALON P3COMEDU P3OUTACT P3TVHR P3TVMIN
    ## 1:       NA       NA       NA       NA       NA       NA     NA      NA
    ## 2:       NA       NA       NA       NA       NA       NA     NA      NA
    ## 3:       NA       NA       NA       NA       NA       NA     NA      NA
    ## 4:       NA       NA       NA       NA       NA       NA     NA      NA
    ## 5:       NA       NA       NA       NA       NA       NA     NA      NA
    ## 6:       NA       NA       NA       NA       NA       NA     NA      NA
    ##    P3VIDHR P3VIDMIN P3VISLIB P3STHLIB P3SUMBK P3SUMRD P3ARTMUS P3ZOOS
    ## 1:      NA       NA       NA       NA      NA      NA       NA     NA
    ## 2:      NA       NA       NA       NA      NA      NA       NA     NA
    ## 3:      NA       NA       NA       NA      NA      NA       NA     NA
    ## 4:      NA       NA       NA       NA      NA      NA       NA     NA
    ## 5:      NA       NA       NA       NA      NA      NA       NA     NA
    ## 6:      NA       NA       NA       NA      NA      NA       NA     NA
    ##    P3AMUSPK P3BEACHS P3PLYCRT P3LRGCTY P3SUMSCH P3NDYPRM P3NHRPRM P3SMREAD
    ## 1:       NA       NA       NA       NA       NA       NA       NA       NA
    ## 2:       NA       NA       NA       NA       NA       NA       NA       NA
    ## 3:       NA       NA       NA       NA       NA       NA       NA       NA
    ## 4:       NA       NA       NA       NA       NA       NA       NA       NA
    ## 5:       NA       NA       NA       NA       NA       NA       NA       NA
    ## 6:       NA       NA       NA       NA       NA       NA       NA       NA
    ##    P3SMMATH P3SMSCI P3SMART P3SMMUSI P3SMCMPT P3SMREQ P3DONCMP P3NUMCMP
    ## 1:       NA      NA      NA       NA       NA      NA       NA       NA
    ## 2:       NA      NA      NA       NA       NA      NA       NA       NA
    ## 3:       NA      NA      NA       NA       NA      NA       NA       NA
    ## 4:       NA      NA      NA       NA       NA      NA       NA       NA
    ## 5:       NA      NA      NA       NA       NA      NA       NA       NA
    ## 6:       NA      NA      NA       NA       NA      NA       NA       NA
    ##    P3NMDCMP P3NMHCMP P3NMWCMP P3CMPSPT P3CMPART P3CMPCPT P3CMPACA P3CMPMPA
    ## 1:       NA       NA       NA       NA       NA       NA       NA       NA
    ## 2:       NA       NA       NA       NA       NA       NA       NA       NA
    ## 3:       NA       NA       NA       NA       NA       NA       NA       NA
    ## 4:       NA       NA       NA       NA       NA       NA       NA       NA
    ## 5:       NA       NA       NA       NA       NA       NA       NA       NA
    ## 6:       NA       NA       NA       NA       NA       NA       NA       NA
    ##    P3CMPSUP P3TUTOR A2REGHLP A4REGHLP A6REGHLP A2TPCONF A4TPCONF A6TPCONF
    ## 1:       NA      NA        3        2        2        5        5        5
    ## 2:       NA      NA        2        4       NA        5        5       NA
    ## 3:       NA      NA        2        2        4        5        5        5
    ## 4:       NA      NA        2        2        3        3        3        3
    ## 5:       NA      NA        2       NA       NA        5       NA       NA
    ## 6:       NA      NA        2        2        2        5        5        5
    ##    A2ATTOPN A4ATTOPN A6ATTOPN VAR00001 A4ATTART A6ATTART
    ## 1:        3        5        3        3        5        5
    ## 2:        5        4       NA       NA        4       NA
    ## 3:        5        5        5        5        5        4
    ## 4:        3        4        3        3        5        3
    ## 5:        5       NA       NA       NA       NA       NA
    ## 6:        2        5        2        2        5        5

``` r
# Load value lables from Github.
lable <- read.csv("https://raw.githubusercontent.com/YunMai-SPS/DA606_homework/master/final_project/variablelable.csv")
head(lable)
```

    ##   Variable Position                              Label Measurement.Level
    ## 1  CHILDID        1        CHILD IDENTIFICATION NUMBER           Nominal
    ## 2    X6AGE        2     X6 CHILD ASSESSMENT AGE(MNTHS)             Scale
    ## 3    X5AGE        3     X5 CHILD ASSESSMENT AGE(MNTHS)             Scale
    ## 4    X4AGE        4     X4 CHILD ASSESSMENT AGE(MNTHS)             Scale
    ## 5    X3AGE        5     X3 CHILD ASSESSMENT AGE(MNTHS)             Scale
    ## 6 X2KAGE_R        6 X2 CHILD ASSESSMENT AGE(MNTHS)-REV             Scale
    ##    Role Column.Width Alignment Print.Format Write.Format
    ## 1 Input           10      Left           A8           A8
    ## 2 Input            9     Right         F7.2         F7.2
    ## 3 Input            9     Right         F7.2         F7.2
    ## 4 Input            8     Right         F6.2         F6.2
    ## 5 Input            8     Right         F6.2         F6.2
    ## 6 Input           10     Right         F6.2         F6.2

``` r
value <- read.csv("https://raw.githubusercontent.com/YunMai-SPS/DA606_homework/master/final_project/variablevalue.csv")
head(value)
```

    ##      Value               Label
    ## 1    X6AGE -9: NOT ASCERTAINED
    ## 2    X5AGE -9: NOT ASCERTAINED
    ## 3    X4AGE -9: NOT ASCERTAINED
    ## 4    X3AGE -9: NOT ASCERTAINED
    ## 5 X2KAGE_R -9: NOT ASCERTAINED
    ## 6 X1KAGE_R -9: NOT ASCERTAINED

Select the semester for anlysis.The prefix of the variables name could be infered from the table below. The prefix are 1:2010 fall(K1): 2011 spring(K2); 3, 2011 fall(11f); 4: 2012 sprinsg(G1\_2); 5: 2012 fall(12f): 6: 2013 spring(G2\_2).

``` r
# There are a lot of columns in these data frames. This will subset the dataframes to include only the variables we are interested in. We will also rename the columns to be more descriptive.
# According the data completeness, I will subset the data to get the survey results from Kindergarten 1st semester(1), Kindergarten 2nd semester(2), first grade 2nd semester(4), second grade 2nd semester(6). 

# reading scores
myreading <- c("CHILDID","X1RTHETK2",   "X2RTHETK2",    "X4RTHETK2",    "X6RTHETK2") 
reading <- subset(earlyedu,,myreading)
colnames(reading) <- c("CHILDID", "K1_READ", "K2_READ", "G1_2_READ", "G2_2_READ")

# math scores
mymath <- c("CHILDID","X1MTHETK2",  "X2MTHETK2",    "X4MTHETK2",    "X6MTHETK2") 
math <- subset(earlyedu,,mymath)
colnames(math) <- c("CHILDID", "K1_Math", "K2_Math", "G1_2_Math", "G2_2_Math")

# science scores
myscience <- c("CHILDID", "X2STHETK2",  "X4STHETK2",    "X6STHETK2") 
science <- subset(earlyedu,,myscience)
colnames(science) <- c("CHILDID", "K2_SCI", "G1_2_SCI", "G2_2_SCI")

# The Dimensional Change Card Sort (DCCS): A Method of Assessing Executive Function in Children
myDCCSTOT <- c("CHILDID", "X1DCCSTOT",  "X2DCCSTOT",    "X4DCCSTOT") 
DCCSTOT <- subset(earlyedu,,myDCCSTOT)
colnames(DCCSTOT) <- c("CHILDID", "K1_DCCSTOT", "K2_DCCSTOT", "G1_2_DCCSTOT")

# DCCS composite score by summing the post-switch score and the Border Game score. Relative completed survay results are only available at 6th semester.
myDCCSSCR <- c("CHILDID", "X6DCCSSCR") 
DCCSSCR <- subset(earlyedu,,myDCCSSCR)
colnames(DCCSSCR) <- c("CHILDID", "G2_2_DCCSSCR")

# GIFTED-TALENT not offered in K. Survay results are only available at 2nd and 4th semester.
GTKlevels <- c("yes", "no", "NA")
myGTK <- c("CHILDID", "S2GIFNO","S4GIFNO") 
GTK <- subset(earlyedu,,myGTK)
GTK$S2GIFNO <- str_replace_all(GTK$S2GIFNO,"(^\\-9)|(^\\-1)","NA") %>% 
  str_replace_all("1","yes") %>% 
  str_replace_all("2","no")
GTK$S4GIFNO <- str_replace_all(GTK$S4GIFNO,"(^\\-9)|(^\\-1)","NA") %>% 
  str_replace_all("0","yes") %>%
  str_replace_all("1","no")
colnames(GTK) <- c("CHILDID", "K2_GIFK", "G1_2_GIFK")

# GIFTED-TALENT offered at some grades or not offered at the school.Survay results are only available at 2nd and 4th semester.
GTSlevels <- c("yes", "no", "NA")
myGTS <- c("CHILDID", "S2GIFNOG","S4GIFNOG") 
GTS <- subset(earlyedu,,myGTS)
GTS$S2GIFNOG <- str_replace_all(GTS$S2GIFNOG,"(^\\-9)|(^\\-1)","NA") %>% 
   str_replace_all("1","yes") %>% 
  str_replace_all("2","no")
GTS$S4GIFNOG <- str_replace_all(GTS$S4GIFNOG,"(^\\-9)|(^\\-1)","NA") %>%
  str_replace_all("0","yes") %>% 
  str_replace_all("1","no")
colnames(GTS) <- c("CHILDID", "K2_GIFS", "G1_2_GIFS")


# A CUTOFF DATE FOR CHILD TO TURN FIVE TO ENTER KINDERGARTEN
Sep1cutofflevels <- c(1,2,NA)
mycutoff <- c("CHILDID", "S6GIFNO") 
Sep1cutoff <- subset(earlyedu,,mycutoff)
Sep1cutoff$S6GIFNOt <- str_replace_all(Sep1cutoff$S6GIFNO,"0","yes") %>% 
  str_replace_all("1","no")
colnames(Sep1cutoff) <- c("CHILDID", "G2_2_Sep1Cut", "G2_2_Sep1Cut_t")

# Whether PreK is helpful for prepare children for Kindergarten.
PreKlevles <- c(1, 2, 3, 4, 5)
myprek <- c("CHILDID", "A1ATNDPR") 
PreK <- subset(earlyedu,,myprek)
PreK$A1ATNDPR  <- str_replace_all(PreK$A1ATNDPR,"^\\-9","NA") 
PreK$A1ATNDPRt  <- str_replace_all(PreK$A1ATNDPR, "1","STRONGLY DISAGREE") %>% 
  str_replace_all("2","DISAGREE") %>% 
  str_replace_all("3","NEITHER AGREE NOR DISAGREE") %>% 
  str_replace_all("4","AGREE") %>% 
  str_replace_all("5","STRONGLY AGREE") 
colnames(PreK) <- c("CHILDID", "PreK", "PreKt")

agelevels <- c(4, 5, 6, 7, 8)
myage <- c("CHILDID", "X1KAGE_R", "X2KAGE_R", "X4AGE", "X6AGE") 
Age <- subset(earlyedu,,myage)
Age[,2]<- Age[,2]/12
Age[,3]<- Age[,3]/12
Age[,4]<- Age[,4]/12
Age[,5]<- Age[,5]/12
colnames(Age) <- c("CHILDID", "K1_AGE", "K2_AGE", "G1_2_AGE", "G2_2_AGE")

genderLevels <- c('Male', 'Female')
mygender <- c("CHILDID", "X_CHSEX_R") 
Gender <- subset(earlyedu,,mygender)
Gender$X_CHSEX_R <- str_replace_all(Gender$X_CHSEX_R,"1","Male") %>% 
  str_replace_all("2","Female")
colnames(Gender) <- c("CHILDID", "CHSEX")

decreasesizelevel <- c("yes", "no", "NA")
mydecreasesize <- c("CHILDID", "S2TT1CLA", "S4TT1CLA", "S6TT1CLA") 
Classsize <- subset(earlyedu,,mydecreasesize)
Classsize$S2TT1CLA <- str_replace_all(Classsize$S2TT1CLA,"(^\\-9)|(^\\-1)","NA") %>% 
  str_replace_all("1","yes") %>% 
  str_replace_all("2","no")
Classsize$S4TT1CLA <- str_replace_all(Classsize$S4TT1CLA,"(^\\-9)|(^\\-1)","NA") %>% 
  str_replace_all("1","yes") %>%
  str_replace_all("2","no")
Classsize$S6TT1CLA <- str_replace_all(Classsize$S6TT1CLA,"(^\\-9)|(^\\-1)","NA") %>% 
  str_replace_all("1","yes") %>% 
  str_replace_all("2","no")
colnames(Classsize) <- c("CHILDID", "K2_CLSI", "G1_2_CLSI", "G2_2_CLSI")

regionlevels <-c(1,2,3,4,NA)
myregion <- c("CHILDID", "X1LOCALE", "X2LOCALE", "X4LOCALE", "X6LOCALE") 
Region <- subset(earlyedu,,myregion)
Region$X1LOCALE <- str_replace_all(Region$X1LOCALE,"(^\\-9)|(^\\-1)","NA") 
Region$X1LOCALt <- str_replace_all(Region$X1LOCALE, "1", "CITY") %>%
  str_replace_all("2", "SUBURB") %>% 
  str_replace_all("3", "TOWN") %>% 
  str_replace_all("4", "RURAL") 
Region$X2LOCALE <- str_replace_all(Region$X2LOCALE,"(^\\-9)|(^\\-1)","NA") 
Region$X2LOCALt <- str_replace_all(Region$X2LOCALE, "1", "CITY") %>%
  str_replace_all("2", "SUBURB") %>% 
  str_replace_all("3", "TOWN") %>% 
  str_replace_all("4", "RURAL") 
Region$X4LOCALE <- str_replace_all(Region$X4LOCALE,"(^\\-9)|(^\\-1)","NA")
Region$X4LOCALt <- str_replace_all(Region$X4LOCALE, "1", "CITY") %>%
  str_replace_all("2", "SUBURB") %>% 
  str_replace_all("3", "TOWN") %>% 
  str_replace_all("4", "RURAL")
Region$X6LOCALE <- str_replace_all(Region$X6LOCALE,"(^\\-9)|(^\\-1)","NA")
Region$X6LOCALt <- str_replace_all(Region$X6LOCALE, "1", "CITY") %>%
  str_replace_all("2", "SUBURB") %>% 
  str_replace_all("3", "TOWN") %>% 
  str_replace_all("4", "RURAL")
colnames(Region) <- c("CHILDID", "K1_LOC","K2_LOC", "G1_2_LOC", "G2_2_LOC", "K1_LOCt","K2_LOCt", "G1_2_LOCt", "G2_2_LOCt")

# Parents getting involve in school volunteer 
parentschoolhlplevels <- c(0, 1, 2, 3, 4,5,NA)
myhlp <- c("CHILDID", "A2REGHLP", "A4REGHLP", "A6REGHLP") 
Prthlp <- subset(earlyedu,,myhlp)
Prthlp$A2REGHLP  <- str_replace_all(Prthlp$A2REGHLP,"^\\-9","NA")
Prthlp$A4REGHLP  <- str_replace_all(Prthlp$A4REGHLP,"^\\-9","NA")
Prthlp$A6REGHLP  <- str_replace_all(Prthlp$A6REGHLP,"^\\-9","NA")
colnames(Prthlp) <- c("CHILDID", "K2_Prthlp", "G1_2_Prthlp", "G2_2_Prthlp")

parentworklevels <- c("b35h", "a35h", "NA")
myprtwork <- c("CHILDID", "X1PAR1EMP","X4PAR1EMP_I", "X6PAR1EMP_I") 
Prtwork <- subset(earlyedu,,myprtwork)
Prtwork$X1PAR1EMP <- str_replace_all(Prtwork$X1PAR1EMP,"(^\\-9)|(^3)|(^4)","NA") %>% 
  str_replace_all("1","a35h") %>% 
  str_replace_all("2","blw35h")
Prtwork$X4PAR1EMP_I <- str_replace_all(Prtwork$X4PAR1EMP_I,"(^\\-9)|(^3)|(^4)","NA") %>% 
  str_replace_all("1","a35h") %>% 
  str_replace_all("2","blw35h")
Prtwork$X6PAR1EMP_I <- str_replace_all(Prtwork$X6PAR1EMP_I,"(^\\-9)|(^3)|(^4)","NA") %>% 
  str_replace_all("1","a35h") %>% 
  str_replace_all("2","blw35h")
colnames(Prtwork) <- c("CHILDID", "K1_PrtW", "G1_2_PrtW", "G2_2_PrtW")

# HIGHEST EDUCATION LEVEL PARENTS ACHIEVED
Parentedulevles <- c(1, 2, 3, 4, 5, 6, 7) 
myedu <- c("CHILDID", "A1HGHPAR") 
Parentedu <- subset(earlyedu,,myedu)
Parentedu$A1HGHPAR <- str_replace_all(Parentedu$A1HGHPAR, "(\\-9)|(8)", "NA")
Parentedu$A1HGHPARt <- str_replace_all(Parentedu$A1HGHPAR,"1","DID NOT COMPLETE HIGH SCHOOL") %>%
  str_replace_all("2","HIGH SCHOOL") %>% 
  str_replace_all("3","SOME COLLEGE") %>% 
  str_replace_all("4","ASSOCIATE'S DEGREE") %>% 
  str_replace_all("5","BACHELOR") %>% 
  str_replace_all("6","MASTER") %>% 
  str_replace_all("7","BEYOND A MASTER")
colnames(Parentedu) <- c("CHILDID", "PrtEDU", "PrtEDUt")

earlychildhood <- inner_join(Age, Gender, by = "CHILDID") %>%
  inner_join(reading, by = "CHILDID") %>% 
  inner_join(math, by = "CHILDID") %>% 
  inner_join(science, by = "CHILDID") %>% 
  inner_join(DCCSTOT, by = "CHILDID") %>% 
  inner_join(DCCSSCR, by = "CHILDID") %>%  
  inner_join(GTK, by = "CHILDID") %>% 
  inner_join(GTS, by = "CHILDID") %>% 
  inner_join(Sep1cutoff, by = "CHILDID") %>%
  inner_join(Classsize, by = "CHILDID") %>%
  inner_join(PreK, by = "CHILDID") %>% 
  inner_join(Region, by = "CHILDID") %>% 
  inner_join(Prthlp, by = "CHILDID") %>% 
  inner_join(Prtwork, by = "CHILDID") %>% 
  inner_join(Parentedu, by = "CHILDID")

head(earlychildhood)
```

    ##    CHILDID   K1_AGE   K2_AGE G1_2_AGE G2_2_AGE  CHSEX K1_READ K2_READ
    ## 1 10000001 6.032500 6.646667 7.638333 8.638333   Male  0.1930  1.7614
    ## 2 10000002 5.950833 6.460000 7.463333       NA Female -0.7870  0.7452
    ## 3 10000003       NA 6.125833 7.027500 8.021667   Male      NA  0.3323
    ## 4 10000004 6.032500 6.570000 7.435833 8.432500   Male -1.7087 -0.2922
    ## 5 10000005 4.959167 5.421667       NA       NA Female -0.0734  0.8013
    ## 6 10000006 6.315000 6.885000 7.901667 8.959167 Female -1.4601 -0.6792
    ##   G1_2_READ G2_2_READ K1_Math K2_Math G1_2_Math G2_2_Math  K2_SCI G1_2_SCI
    ## 1    3.1523    3.3637  0.5827  1.5384    2.7286    3.2950  1.3738   3.3354
    ## 2    2.2023        NA -0.3473  0.8800    2.0825        NA  0.8811   1.3122
    ## 3    1.1861    2.0689      NA  1.0112    2.2080    2.8384  0.4244   2.2479
    ## 4    0.8632    1.5117 -2.1245 -0.2834    1.1082    2.8392  0.6509   0.2933
    ## 5        NA        NA -0.5545  0.6163        NA        NA  0.3996       NA
    ## 6    1.0132    1.9750 -1.0616 -0.5379    0.4484    1.0869 -0.6320  -0.7748
    ##   G2_2_SCI K1_DCCSTOT K2_DCCSTOT G1_2_DCCSTOT G2_2_DCCSSCR K2_GIFK
    ## 1   2.5071         16         17           16       7.3280      NA
    ## 2       NA         17         16           16           NA     yes
    ## 3   2.3529         NA         14           16       8.2395     yes
    ## 4   1.7845         15         14           17       7.2868      NA
    ## 5       NA         14         17           NA           NA    <NA>
    ## 6  -0.2110         16         14           18       5.3872      NA
    ##   G1_2_GIFK K2_GIFS G1_2_GIFS G2_2_Sep1Cut G2_2_Sep1Cut_t K2_CLSI
    ## 1        no      NA       yes            0            yes      no
    ## 2        no     yes       yes           NA           <NA>      no
    ## 3        no      no       yes            1             no      NA
    ## 4       yes      NA       yes            0            yes      no
    ## 5      <NA>    <NA>      <NA>           NA           <NA>    <NA>
    ## 6       yes      NA       yes            0            yes      no
    ##   G1_2_CLSI G2_2_CLSI PreK                      PreKt K1_LOC K2_LOC
    ## 1       yes       yes    3 NEITHER AGREE NOR DISAGREE      4      4
    ## 2        NA      <NA>    3 NEITHER AGREE NOR DISAGREE      2      2
    ## 3        NA        NA <NA>                       <NA>   <NA>      2
    ## 4        no       yes    5             STRONGLY AGREE      4      4
    ## 5      <NA>      <NA>    5             STRONGLY AGREE      2      2
    ## 6       yes       yes    4                      AGREE      4      4
    ##   G1_2_LOC G2_2_LOC K1_LOCt K2_LOCt G1_2_LOCt G2_2_LOCt K2_Prthlp
    ## 1        4        4   RURAL   RURAL     RURAL     RURAL         3
    ## 2        2     <NA>  SUBURB  SUBURB    SUBURB      <NA>         2
    ## 3        2        2    <NA>  SUBURB    SUBURB    SUBURB         2
    ## 4        4        4   RURAL   RURAL     RURAL     RURAL         2
    ## 5     <NA>     <NA>  SUBURB  SUBURB      <NA>      <NA>         2
    ## 6        4        4   RURAL   RURAL     RURAL     RURAL         2
    ##   G1_2_Prthlp G2_2_Prthlp K1_PrtW G1_2_PrtW G2_2_PrtW PrtEDU      PrtEDUt
    ## 1           2           2      NA        NA        NA      6       MASTER
    ## 2           4        <NA>      NA        NA      <NA>      5     BACHELOR
    ## 3           2           4    <NA>      a35h      a35h   <NA>         <NA>
    ## 4           2           3    a35h      a35h      <NA>      5     BACHELOR
    ## 5        <NA>        <NA>    a35h      <NA>      <NA>      3 SOME COLLEGE
    ## 6           2           2    a35h      a35h        NA      2  HIGH SCHOOL

``` r
# In order to investigate the correlation between age and reading skill. the reading score data from first semester(K1), 2nd semester(K2), 4th semester(grade 1_2), 6th semester(grade2_2), and the data about September cutoff will be further cleaned. filter cases that don't have "NA".

# remove rows with NA in reading score
earlychildhood1 <- earlychildhood[!is.na(earlychildhood$K1_READ),]
earlychildhood2 <- earlychildhood[!is.na(earlychildhood$K2_READ),]
earlychildhood3 <- earlychildhood[!is.na(earlychildhood$G1_2_READ),]
earlychildhood4 <- earlychildhood[!is.na(earlychildhood$G2_2_READ),]

# remove rows with NA in September 1st cutoff  
earlychildhood5 <- earlychildhood1[!is.na(earlychildhood1$G2_2_Sep1Cut_t),]
earlychildhood6 <- earlychildhood2[!is.na(earlychildhood2$G2_2_Sep1Cut_t),]
earlychildhood7 <- earlychildhood3[!is.na(earlychildhood3$G2_2_Sep1Cut_t),]
earlychildhood8 <- earlychildhood4[!is.na(earlychildhood4$G2_2_Sep1Cut_t),]

# remove rows with NA in PrtEDU
earlychildhood9 <- filter(earlychildhood1, PrtEDU != "NA")
earlychildhood10 <- filter(earlychildhood2, PrtEDU != "NA")
earlychildhood11 <- filter(earlychildhood3, PrtEDU != "NA")
earlychildhood12 <- filter(earlychildhood4, PrtEDU != "NA")

length(earlychildhood5$G2_2_Sep1Cut)
```

    ## [1] 11278

``` r
# 12992
length(earlychildhood6$G2_2_Sep1Cut)
```

    ## [1] 12487

``` r
# 13184
length(earlychildhood7$G2_2_Sep1Cut)
```

    ## [1] 12495

``` r
# 15007
length(earlychildhood8$G2_2_Sep1Cut)
```

    ## [1] 12467

``` r
# 16374
length(earlychildhood10$PrtEDU)
```

    ## [1] 14412

``` r
# 17215
length(earlychildhood10$PrtEDU)
```

    ## [1] 14412

``` r
# 17215
length(earlychildhood11$PrtEDU)
```

    ## [1] 12591

``` r
# 15132
length(earlychildhood12$PrtEDU)
```

    ## [1] 11540

``` r
# 13850
```

### Research question

**You should phrase your research question in a way that matches up with the scope of inference your dataset allows for.**

Q1.1 Are the students from the schools who use the 5-year-old cutoff for Kindergarten entrance have better academic performance? Q1.2 Does parents' education level have effects on children's academic performance in the early childhood?

### Cases

**What are the cases, and how many are there?**

Each case represents children who participated, or whose parent participated, in at least one of the two kindergarten data collections (Fall 2010 or Spring 2011). For question 1.1, there are 12992 observations in the data set for 1st semester reading score, 13184 observations in the data set for 2nd semester reading score, 15007 observations in the data set for 4th semester reading score, and 16374 observations in the data set for 6th semester reading score.

For question 1.2, there are 17215 observations in the data set for 1st semester reading score, 17215 observations in the data set for 2nd semester reading score, 15132 observations in the data set for 4th semester reading score, and 13850 observations in the data set for 6th semester reading score.

### Data collection

**Describe the method of data collection.**

The ECLS-K:2011 is the third and latest study in the Early Childhood Longitudinal Study (ECLS) program collected by the [National Center for Educational Statistics](http://nces.ed.gov//) (NCES) from 2010 fall to 2013 spring. Survy were conduct every semester and data from the 2010 fall, 2011 spring, 2012 spring, and 2013 spring are more completed.

### Type of study

**What type of study is this (observational/experiment)?**

This is an observational study.

### Data Source

**If you collected the data, state self-collected. If not, provide a citation/link.**

Data is collected by NCES and is available online here: <https://nces.ed.gov/ecls/dataproducts.asp>. For this project, data were download and loaded into SPSS as the website suggests, cleaned up (remove most of the repressed variables) and converted to CSV files.

The manual "ECLS-K:2011 Kindergarten User's Manual, Public Version PDF File" and "ECLS-K:2011 Kindergarten-Second Grade User's Manual, Public Version PDF File" is available here: <https://nces.ed.gov/ecls/dataproducts.asp>. The Eclectronic Code Book could be downloaded from the website too.

### Response

**What is the response variable, and what type is it (numerical/categorical)?**

For both of the questions, the response variables are the scores from the five academic assessments. They are numerical variables.

### Explanatory

**What is the explanatory variable, and what type is it (numerical/categorival)?**

For Q1.1, the explanatory variable is September 1st cutoff. It is categorical variable. For Q1.2, the explanatory variable is parents' highest education level. I would take this variable as numerical variable. American education can be convert as following: a high school level of education is equivalent to 12 years'; an Associate's Degree is equivalent to 14 years', a B.S./B.A. is equivalent to 16 years', etc. a Master's Degree is equivalent to 17-18 years', and Master beyondis equivalent to &gt;= 20 years'.

### Relevant summary statistics

**Provide summary statistics relevant to your research question. For example, if you're comparing means across groups provide means, SDs, sample sizes of each group. This step requires the use of R, hence a code chunk is provided below. Insert more code chunks as needed.**

``` r
# statistic for explanatory variables
summary(earlychildhood5$G2_2_Sep1Cut)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##  0.0000  0.0000  0.0000  0.3837  1.0000  1.0000

``` r
summary(earlychildhood6$G2_2_Sep1Cut)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##  0.0000  0.0000  0.0000  0.3828  1.0000  1.0000

``` r
summary(earlychildhood7$G2_2_Sep1Cut)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##  0.0000  0.0000  0.0000  0.3826  1.0000  1.0000

``` r
summary(earlychildhood8$G2_2_Sep1Cut)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##  0.0000  0.0000  0.0000  0.3816  1.0000  1.0000

``` r
summary(earlychildhood9$PrtEDU)
```

    ##    Length     Class      Mode 
    ##     14611 character character

``` r
summary(earlychildhood10$PrtEDU)
```

    ##    Length     Class      Mode 
    ##     14412 character character

``` r
summary(earlychildhood11$PrtEDU)
```

    ##    Length     Class      Mode 
    ##     12591 character character

``` r
summary(earlychildhood12$PrtEDU)
```

    ##    Length     Class      Mode 
    ##     11540 character character

``` r
# statistic for response variable
summary(earlychildhood2$K2_READ)
```

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -9.00000 -0.00405  0.49390  0.43090  0.93440  2.97800

``` r
summary(earlychildhood2$K2_Math)
```

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -9.00000  0.01305  0.52170  0.40770  0.94840  2.88200

``` r
summary(earlychildhood2$K2_SCI)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -9.0000 -0.6074  0.0928 -0.1527  0.6492  1.8900

``` r
summary(earlychildhood2$K2_DCCSTOT)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   -9.00   15.00   15.00   15.05   17.00   18.00

``` r
summary(earlychildhood1$K1_READ)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -9.0000 -1.1510 -0.5752 -0.6038 -0.0115  2.9780

``` r
summary(earlychildhood1$K1_Math)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -9.0000 -1.0760 -0.4074 -0.5966  0.1273  5.3200

``` r
summary(earlychildhood1$K1_DCCSTOT)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   -9.00   14.00   15.00   13.94   16.00   18.00

``` r
# example for summary of 1st semester reading score
```

``` r
# subest 2nd semester reading score and september 1st cutoff for statistic analysis
k2read <- data.frame("CHILDID"=earlychildhood5$CHILDID,"K2_READ"=earlychildhood5$K2_READ,"G2_2_Sep1Cut_t"=earlychildhood5$G2_2_Sep1Cut_t)

k2read <- k2read %>% mutate_if(is.factor, as.character)  

# do the statistic on 2nd semester science score 
stat <- k2read %>%  group_by(G2_2_Sep1Cut_t) %>% summarise (mean=mean(K2_READ,na.rm=T),sd(K2_READ,na.rm=T), median=median(K2_READ,na.rm=T), min=min(K2_READ,na.rm=T),max=max(K2_READ,na.rm=T))
```

``` r
ggplot(data=subset(k2read,!is.na(k2read$K2_READ)),aes(factor(G2_2_Sep1Cut_t),K2_READ))+
  geom_boxplot()+
  xlab("September 1st Cut Off for Kindergarten") + 
  ylab("2011 Spring Kindergarten Reading Score")
```

![](Yun_Proposal_files/figure-markdown_github/unnamed-chunk-9-1.png)

``` r
# subest 6th semester reading score and parents eduction level for statistic analysis
g2edu <- data.frame("CHILDID"=earlychildhood12$CHILDID,"G2_2_READ"=earlychildhood12$G2_2_READ,"PrtEDU"=earlychildhood12$PrtEDU)

g2edu  %>% mutate_if(is.factor, as.character) -> g2edu 
```

``` r
ggplot(data=subset(g2edu,!is.na(g2edu$G2_2_READ)),aes(factor(PrtEDU),G2_2_READ))+
  geom_boxplot(aes(fill=factor(PrtEDU)))+
  xlab("Parent Education Level")+
  ylab("2013 Spring 2nd Grade Reading Score")
```

![](Yun_Proposal_files/figure-markdown_github/unnamed-chunk-11-1.png)
