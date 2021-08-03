# Replace-filterD

We are deprecating filterD() from the next version of FSA (v0.9.0). It will likely be removed by the start of 2022. filterD() was an attempt to streamline the process of using filter() (from dplyr) followed by droplevels() to remove levels of a factor variable that no longer existed in the filtered data frame.

For example, consider the very simple data frame below.

d <- data.frame(tl=runif(6,min=100,max=200),
                spec=factor(c("LMB","LMB","SMB","BG","BG","BG")))
d

##         tl spec
## 1 120.8437  LMB
## 2 112.9690  LMB
## 3 126.4658  SMB
## 4 118.9474   BG
## 5 140.0974   BG
## 6 135.2961   BG

Now suppose that this data frame is reduced to just Bluegill.

dbg <- dplyr::filter(d,spec=="BG")

A quick frequency table of species caught shows that levels for species that no longer exist in the data frame are maintained.

xtabs(~spec,data=dbg)

## spec
##  BG LMB SMB 
##   3   0   0

This same “problem” occurs when using subset() from base R.

dbg <- subset(d,spec=="BG")
xtabs(~spec,data=dbg)

## spec
##  BG LMB SMB 
##   3   0   0

These “problems” can be eliminated by submitting the new data frame to drop.levels().

dbg2 <- droplevels(dbg)
xtabs(~spec,data=dbg2)

## spec
## BG 
##  3

filterD() was a simple work-around that eliminated this second step and was useful for helping students who were just getting started with R.

dbg3 <- FSA::filterD(d,spec=="BG")
xtabs(~spec,data=dbg3)

## spec
## BG 
##  3

However, this is a hacky solution to a simple problem. Thus, we are deprecating filterD() from FSA with plans to remove it by the beginning of next year. Thus, please use droplevels() (or fct_drop() from forcats) after using filter() to accomplish the same task of the soon to be defunct filterD().
