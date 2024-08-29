## Vowel Formant Frequency Alterations as Voice Disguise Strategy 

A recent project of mine for university explored the change of first and second formants in vowels as a means of voice disguise strategy. For the analysis part I utilized _Praat_ and _R_. For the full paper, please contact me.


### Praat

(My own voice recording disguised, versino 6.3.17)
Spectogram at the bottom
This collected data was then read into R and vowel charts were created.


### Analysis in R and Visualizations

```library(dplyr)
library(ggplot2)
```

```
##load data
my_data <- read.table(file="D:/Uni/Master/Semester_8/VA/Results.txt", header=TRUE, sep="\t", encoding="UTF-8")
print(my_data)

#get mean of value of each vowel
means <- my_data%>%group_by(VOWEL)%>%summarise(mean_F1 = mean(F1),mean_F2 = mean(F2))

#plot the data points as vowel chart and reverse the axes
ggplot(means, aes(x = mean_F2, y = mean_F1, label= VOWEL)) + 
  geom_label() + 
  expand_limits(x=c(200,2500), y=c(200, 1500)) +
  labs(y = "F1 (Hz)", x = "F2 (Hz)") +
  scale_y_reverse(position = "right")+
  scale_x_reverse(position = "top") +
  theme_classic()
```





```
##load data
my_data_disguised <- read.table(file="D:/Uni/Master/Semester_8/VA/Results_disguised.txt", header=TRUE, sep="\t", encoding="UTF-8")
print(my_data_disguised)

#get mean of value of each vowel
means_d <- my_data_disguised%>%group_by(VOWEL)%>%summarise(mean_F1_d = mean(F1),mean_F2_d = mean(F2))

#plot the data points as vowel chart and reverse the axes
ggplot(means_d, aes(x = mean_F2_d, y = mean_F1_d, label= VOWEL)) + 
  geom_label() + 
  expand_limits(x=c(200,2500), y=c(200, 1500)) +
  labs(y = "F1 (Hz)", x = "F2 (Hz)") +
  scale_y_reverse(position = "right")+
  scale_x_reverse(position = "top") +
  theme_classic()
```




### Findings 


### References

A great overview on how to make such vowel charts in R are shown by Joey Stanley:<br/>
https://joeystanley.com/blog/making-vowel-plots-in-r-part-1/<br/>
https://joeystanley.com/blog/making-vowel-plots-in-r-part-2/
