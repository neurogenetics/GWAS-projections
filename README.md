## GWAS-projections

Cornelis + Andy + Mike 

February 2021

Goal: Prediction of the number of GWAS loci in future neurodegenerative disease GWASes (mainly focussing on the larger ones AD-PD)

### Overview of this repo

1) getting data...

2) code...

3) figures...

#### 1) getting data...


##### Alzheimer:

```
Notes: 
- Proxy cases are counted in cases_corrected as 1/2 case
- Of course always the discussion AD vs dementia vs proxy dementia etc. that is ignored here

```

| First_Author       | PMID | year | cases  | proxies | cases_corrected | controls | loci |
|--------------|------|------|--------|---------|-----------------|----------|------|
| Lambert      | 19734903   | 2009 | 6010   | 0       | 6010            | 8625     | 3    |
| Harold       | 19734902   | 2009 | 5964   | 0       | 5964            | 10188    | 3    |
| Hollingworth | 22005930   | 2011 | 19870  | 0       | 19870           | 39846    | 10   |
| Naj          | 21460841   | 2011 | 19490  | 0       | 19490           | 36770    | 10   |
| Lambert      | 24162737   | 2013 | 25580  | 0       | 25580           | 48466    | 19   |
| Jansen       | 30617256   | 2019 | 24087 | 47793   | 47983           | 383378  | 29   |
| Kunkle       | 30820047   | 2019 | 35274 | 0       | 35274           | 59163   | 25   |
| Bellenguez   | preprint*   | 2020 | 64498 | 46828   | 87912          | 677663   | 75   |

* => medrxiv preprint... https://www.medrxiv.org/content/10.1101/2020.10.01.20200659v2

		

##### Parkinson:

```
Notes: 
- Proxy cases are counted in cases_corrected as 1/2 case

```

| First_Author       | PMID | year | cases  | proxies | cases_corrected | controls | loci |
|--------------|------|------|--------|---------|-----------------|----------|------|
| Simón-Sánchez| 19915575   | 2009 | 5074   | 0       | 5074            | 8551     | 3    |
| Nalls       | 21292315   | 2011 | 12386  | 0      | 12386            | 21026    | 11    |
| IPDGC | 21738488   | 2011 | 50650  | 0       | 15812           | 21026    | 16   |
| Nalls          | 25064009   | 2014 | 19061  | 0       | 19490           | 100833    | 28   |
| Chang      | 28892059   | 2017 | 26035  | 0       | 25580           | 403190    | 44   |
| Nalls       | 30617256   | 2019 | 37688 | 18618   | 46997           | 1400000  | 90   |


#### 2) code...


```
library("data.table")
library("ggplot2")
require("ggrepel")

## read in data and predict

data <- read.table("lociPRojection_PD.txt", header = T)
# OR
data <- read.table("lociPRojection_AD.txt", header = T)

### prediciton based on cases only
modelCases <- lm(loci ~ cases_corrected, data = data)

### add predictions
data$casePredictedLoci <- predict(modelCases, data)

## look at the data
data

## make the plot for PD
basicPlot <- ggplot(data, aes(cases_corrected, loci, label=year)) + geom_point() + geom_smooth(method = "lm", se = T, fullrange = T)
formattedPlot <- basicPlot + theme_bw() + ylab("Number of PD Loci") + xlab("Number of PD Cases") + xlim(0,102000) 

formattedPlotanno <- formattedPlot + geom_text(hjust=-1, vjust=1)

formattedPlotanno <- formattedPlot + geom_text(aes(label = rownames(df)),
              size = 3.5)


## save plot
ggsave(filename = "lociPlots_PD.png", plot = exportPlot)




## figure for twitter...
formattedPlot <- basicPlot + theme_bw() + ylab("Number of Loci") + xlab("Number of Cases") + xlim(0,182000) + ylim(0,180)

ggsave(filename = "lociPlots_for_TWITTER_AD.png", plot = formattedPlot)

```


#### 3) figures...

Figures without annotation




Figures with annotation (using powerpoint cause is easier)




## Done....

![myImage](https://media.giphy.com/media/XRB1uf2F9bGOA/giphy.gif)
