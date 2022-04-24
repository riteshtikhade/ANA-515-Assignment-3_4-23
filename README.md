# ANA-515-Assignment-3_4-23
getwd()

#import CSV
library(tidyverse)
my_data <- read.csv("StormEvents.csv")
View(my_data)

# select variables v1, v2, v3,v4, v5, v6,v7, v8, v9,v10, v11, v12,v13,v14, v15,v16, v17, v18, v19
myvars1 <- c("BEGIN_YEARMONTH", "BEGIN_DAY", "BEGIN_TIME","END_YEARMONTH","END_DAY","END_TIME","EPISODE_ID","EVENT_ID","STATE","STATE_FIPS","CZ_NAME","CZ_TYPE","CZ_FIPS","EVENT_TYPE","SOURCE","BEGIN_LAT","BEGIN_LON","END_LAT","END_LON")
newdata1 <- my_data[myvars1]
head(newdata1)

#Arranging the data
library(dplyr)
Oder_data <- arrange(newdata1, BEGIN_YEARMONTH)

#Title Name
library(stringr)
view(Oder_data)
str_to_title(string = Oder_data$STATE)
str_to_title(string = Oder_data$CZ_NAME)

#Limit the column and remove column.
library(dplyr)
R_data<-filter(Oder_data, CZ_TYPE=='C')
R_data <- select(Oder_data, - c(CZ_TYPE))
view(R_data)

#pading and unite

newdata2 <- str_pad(newdata1$STATE_FIPS, width=3, side= "left", pad= "0")
view(newdata2)
str_pad(newdata1$CZ_FIPS, width=3, side= "left", pad= "0")
view(newdata2)
library(tidyr)
unite(R_data, flips, STATE_FIPS, CZ_FIPS, sep ="00") 
rename_all(R_data, tolower)

#New dataframe
data("state")
us_state_info <- data.frame(state=state.name, region=state.region, area=state.area)
Newset <- data.frame(table(newdata1$STATE))
head(Newset)
Newset1<-rename(Newset, c("state"="Var1"))
merged <- merge(x=Newset1, y=us_state_info, by.x = "state", by.y = "state")
head(merged)

library(dplyr)
us_state_info2  <- mutate_all(us_state_info, toupper)
merged <- merge(x=Newset1, y=us_state_info2, by.x = "state", by.y = "state")
head(merged)   

library(ggplot2)
Storm_plot <- ggplot(merged, aes(x = area, y = Freq)) +
  geom_point(aes(color = region)) +
  labs(x = "Land area(square miles)" , y = "# of storm events in 2017") +
  theme(axis.text.x = element_text(angle = 90, size = 8))
Storm_plot
  
