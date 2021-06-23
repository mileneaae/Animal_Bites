###########################################################################
#
# Animal Bites Analyzes
# Dataset available in https://www.kaggle.com/rtatman/animal-bites
# Milene Alves Eigenheer <mileneaae@gmail.com>
#
# Feel free to use, modify, and share
###########################################################################

# Loading packages
if(!require(data.table)) install.packages("data.table ", dep=T); library(data.table)
if(!require(tidyverse)) install.packages("tidyverse", dep=T); library(tidyverse)
if(!require(lubridate)) install.packages("lubridate", dep=T); library(lubridate)
if(!require(ggplot2)) install.packages("ggplot2", dep=T); library(ggplot2)
if(!require(RColorBrewer)) install.packages("RColorBrewer", dep=T); library(RColorBrewer)

# Creating my directories 
dir.create("datadir")
dir.create("mapdir")
dir.create("resultsdir")

# Setting working directory
datadir<-("_\\setyourdirectoryhere\\datadir")
mapdir<-("_\\setyourdirectoryhere\\mapdir")
resultsdir<-("_\\setyourdirectoryhere\\resultsdir")

# I saved my data in datadir
setwd(datadir)

# READING ANIMAL BITES DATA
bites <- read.table("AnimalBites.csv", header = T, sep = ",", na.string="")
head(bites)
sum(is.na(bites))
str(bites) # bites is a data frame with 9003 observations, 15 variables 
#and a lot (67.903) of missing values

# Removing duplicates
b_clean<-bites[!duplicated(bites),]
nrow(b_clean) # after removing duplicated registers - 8954 observations

# Considering missing values
colSums(is.na(b_clean))
# using this information is easy to see that some columns have really few information
# However, we can possibly solve part of the 295 missing values in bite_date column 
# using the head_sent_date column as a proxy. To assess if it is trustable, I calculated
# the difference between head_sent_date and bite_date on the registers that contain
# both columns

# Converting date columns
b_clean$bite_date<-ymd(substr(b_clean$bite_date, 1, 10))
b_clean$head_sent_date<-ymd(substr(b_clean$head_sent_date, 1, 10))
b_clean$datadif<-(b_clean$head_sent_date - b_clean$bite_date)
plot(b_clean$datadif, ylab="Difference between dates", 
     xlab="Occurrence of bite accidents", xlim=c(0,2000)) 
# There are only 2 outliers

tail(sort(unlist(b_clean$datadif, use.names = FALSE)), 5) 
# The outliers are 369 and 1096. all the other difference are < =10 days
(2*100)/295 
# the outliers are only 0.68%, so I feel safe to replace the missing values in 
# bite_date column by the head_sent_date column 

b_clean <- b_clean %>% 
  mutate (bite_date = fifelse (is.na(bite_date), head_sent_date, bite_date))
head(b_clean)  
str(b_clean)
colSums(is.na(b_clean))

# I also included information about year, month and season
b_clean$year<-as.numeric(substr(b_clean$bite_date, 1, 4))
b_clean$month<-as.numeric(substr(b_clean$bite_date, 6, 7))
b_clean$season<-ifelse(b_clean$month %in% 3:5, "spring",
                       ifelse(b_clean$month %in% 6:8, "summer",
                              ifelse(b_clean$month %in%9:11, "fall", "winter")))
head(b_clean)
str(b_clean)
nrow(b_clean)

### ANALYSES PER SPECIES
species<-as.data.frame(b_clean %>% group_by(SpeciesIDDesc) %>% 
                         count (SpeciesIDDesc))
ordersp<-c("DOG","CAT","BAT","RACCOON", "HORSE","FERRET","RABBIT",
           "SKUNK","OTHER","NA")
species <- species [order(match(species[,"SpeciesIDDesc"], ordersp)),]

setwd(resultsdir)
write.csv (species, "_\\resultsdir\\bites_sp.csv", row.names=FALSE)

# NUMBER OF BITES PER SPECIES
tiff(file = "_\\resultsdir\\bites_per_species.tiff", width = 6400, height = 3200, 
     units = "px", res = 300)
p <- ggplot(species, aes(x=factor(SpeciesIDDesc, level = ordersp), y=n, fill=factor(n)))
p + geom_bar(stat="identity") + ylim(0,8000)+
  scale_fill_brewer(palette="BrBG")+
  labs (x="", y="Number of registered events", 
        title="Number of registered bites by animal species at Louisville (1985-2017)") +
  theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank(),
        panel.background = element_blank())+
  theme(legend.position="none",
        plot.title = element_text(hjust = 0.5),
        text = element_text(size = 28),
        axis.title = element_text(face="bold"),
        axis.text.x=element_text(size = 28, margin = margin(t = 20, r = 20, b = 20, l = 20)),
        axis.text.y = element_blank(), axis.ticks = element_blank())+
  geom_text(aes(label=n), vjust=-1, size=14)
dev.off()

# Merging this information with the b_clean dataset - it will be useful to statistics
b_clean<-merge(b_clean, species, by.x="SpeciesIDDesc")
colnames(b_clean)[which(names(b_clean)=="n")] <- "total"
head(b_clean)

### Considering time information, I had to subset to remove the NA values in 
# bite_date column
summary(b_clean$bite_date) # 123NAs, besides past and future dates 
# Keeping only data between 1985 and 2017:
b_date<- b_clean %>% filter (b_clean$year >= 1985 & b_clean$year <=2017)
summary(b_date) 

# PER YEAR
bitesperyear <- as.data.frame(b_date %>% group_by(year) %>% count (year))
write.csv(bitesperyear, "_\\resultsdir\\bites_year.csv", row.names=FALSE)

# As before 2010 there were almost none registers, I subset my data
b_date2 <- b_date %>% filter (year>= 2010)
str(b_date2)
sp_date<-as.data.frame(b_date2 %>% group_by(year,SpeciesIDDesc) %>% tally())

yearorder<-1985:2017
tiff(file = "_\\resultsdir\\bites_per_year.tiff", width = 6400, height = 3200, 
     units = "px", res = 300)
g<- ggplot(sp_date, aes(x=factor(year, levels=yearorder), y=n, 
                        fill=factor(SpeciesIDDesc, levels=ordersp)))
g + geom_bar(stat="identity")+
  scale_fill_brewer(palette="BrBG", direction=-1)+
  labs (x="Year", y="Number of registered events", fill="Species",
        title="Number of registered bites by 
  	animals at Louisville (1985-2017)") +
  theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank(),
        panel.background = element_blank(),
        plot.title = element_text(hjust = 0.5),
        text = element_text(size = 28),
        axis.title = element_text(face="bold"),
        axis.text.x=element_text(size = 20, margin = margin(t = 20, r = 20, b = 20, l = 20)),
        axis.text.y=element_text(size = 20,  margin = margin(t = 20, r = 20, b = 20, l = 20)))
dev.off()

# Considering that actions to mitigate rabies has to be different to domestic 
# animals and wildlife, I split my data. I am ignoring "NA" and "OTHER" values.
# I am also considering all the period between 1985 and 2017.
domestic<- c("DOG","CAT","HORSE","FERRET","RABBIT")
wildlife<-c("BAT","RACCOON","SKUNK")
b_date$class<-ifelse (b_date$SpeciesIDDesc %in% domestic, "domestic", "wildlife")
b_domest <- subset (b_date, b_date$SpeciesIDDesc %in% domestic)
head(b_domest)

# DOMESTIC ANIMAL BITES PER SEASON 
b_domest_season <- as.data.frame(b_domest %>% 
                                   group_by(season, SpeciesIDDesc, total) %>% tally())
tiff(file = "_\\resultsdir\\bites_per_season2.tiff", width = 1600, height = 3200, 
     units = "px", res = 300)
s <- ggplot(b_domest_season, aes(x=season, y=n, 
                                 fill=factor(SpeciesIDDesc, levels=ordersp))) 
s + geom_bar(stat="identity")+
  scale_fill_brewer(palette="BrBG", direction=-1)+
  labs (x="", y="Number of registered events", fill="Species",
        title="Number of registered bites by domestic 
        	animals at Louisville (1985-2017)") +
  theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank(),
        panel.background = element_blank(),
        plot.title = element_text(hjust = 0.5),
        text = element_text(size = 28),
        axis.title = element_text(face="bold"),
        axis.text.x=element_text(size = 28, margin = margin(t = 20, r = 20, b = 20, l = 20)),
        axis.text.y=element_text(size = 28, margin = margin(t = 20, r = 20, b = 20, l = 20)))
dev.off()

shapiro.test(b_domest_season$n) # not normal distribution 
kruskal.test(n ~ season, data=b_domest_season) #none difference

# WILDLIFE BITES PER SEASON
b_wild <- subset (b_date, b_date$SpeciesIDDesc %in% wildlife)
b_wild_season <- as.data.frame(b_wild %>% 
                                 group_by(season, SpeciesIDDesc,total) %>% tally())
tiff(file = "_\\resultsdir\\WILD_bites_per_season.tiff", width = 3200, height = 3200, 
     units = "px", res = 300)
s <- ggplot(b_wild_season, aes(x=season, y=n, fill=factor(SpeciesIDDesc, levels=ordersp))) 
s + geom_bar(stat="identity")+
  scale_fill_brewer(palette="BrBG", direction=-1)+
  labs (x="", y="Number of registered events", fill="Species",
        title="Number of registered bites by wild 
        	animals at Louisville (1985-2017)") +
  theme(plot.title = element_text(hjust = 0.5),
        text = element_text(size = 28),
        axis.title = element_text(face="bold"),
        axis.text.x=element_text(size = 28, margin = margin(t = 20, r = 20, b = 20, l = 20)),
        axis.text.y=element_text(size = 28, margin = margin(t = 20, r = 20, b = 20, l = 20)),
        panel.grid.major = element_blank(), panel.grid.minor = element_blank(),
        panel.background = element_blank())
dev.off()

shapiro.test(b_wild_season$n) # not normal distribution 
kruskal.test(n ~ season, data=b_wild_season) #none difference

# RABIES
rabies<-as.data.frame(b_clean %>% group_by(SpeciesIDDesc, ResultsIDDesc) %>% tally())
sum(rabies$n) #8954 cases

rabies_all<-as.data.frame(b_clean %>%
                            filter(!is.na(b_clean$SpeciesIDDesc),!is.na(b_clean$ResultsIDDesc)) %>% 
                            group_by(SpeciesIDDesc, ResultsIDDesc) %>% 
                            summarise(count = n()))
sum(rabies_all$count) #1538 total
sum(rabies_all$count)/sum(rabies$n)*100 #proportion of tests/number of incidents
3/(3+158)*100 #1.86% bat rabies
1/59*100 #1.69% dog rabies
write.csv (rabies_all, "_\\resultsdir\\rabies.csv", row.names = FALSE)

# LOCATION INFORMATION
# I downloaded information about zip codes and their latitude and longitude
ZipCodeSourceFile = "http://download.geonames.org/export/zip/US.zip"
temp <- tempfile()
download.file(ZipCodeSourceFile , temp)
ZipCodes <- read.table(unz(temp, "US.txt"), sep="\t")
unlink(temp)
names(ZipCodes) = c("CountryCode", "zip", "PlaceName", 
                    "AdminName1", "AdminCode1", "AdminName2", "AdminCode2", 
                    "AdminName3", "AdminCode3", "latitude", "longitude", "accuracy")
head(ZipCodes)

# Change to map dir
setwd(mapdir)

#Filtering my data by victim zip code
zc<-as.data.frame(b_date %>% filter(!is.na(b_date$victim_zip)))

#merging my information with the previous downloaded dataset
spatial<-merge(zc, ZipCodes, by.x=c("victim_zip"), by.y=c("zip"))
head(spatial)
write.csv (spatial,"_\\mapdir\\spatial.csv", row.names = FALSE)

# grouping by count to show where things are happening - same points, + intensity
spatial2 <- as.data.frame(spatial %>% 
                            group_by(latitude, longitude) %>% 
                            summarise(count = n()))
head(spatial2)
write.csv (spatial2, "_\\mapdir\\spatial2.csv", row.names = FALSE)
