[Back](https://ycvogt.github.io/my_portfolio/)

##  Part II: Measuring and Visualizing Vowel Formant Frequency Alterations as Voice Disguise Strategy

A recent project of mine* explored the change of first and second formants in vowels as a means of voice disguise strategy, in addition to fundamental frequency alterations (see [Part I](/posts/praat_vowels1_2.md)). We conducted an experiment in our Phonetics Lab at university, recording every participant with a regular microphone and an electroglottograph (EGG). For this part of the analysis, I only utilized the microphone data. The data was then furhter analysed in _Praat_ [1] and visualized in _R_ [2]. The first part of the research question was as follows: In how far do adult women employ alteration in the first two formants (F1, F2) as a voice disguise strategy?

Based on this research question, I could formulate the following hypotheses:

* H0: There is no difference between normal and disguised vowels in the participants' F1 and F2 formant freuquencies in continuous speech based on acoustic measures.
* H1: There is a difference between normal and disguised vowels in the participants' F1 and F2 formant frequencies in continuous speech based on acoustic measures.

### Praat

Once the data was collected, I opened the audio files in _Praat_. After visually identifying the relevant segments with the aid of the spectrogram (this takes some knowledge of phonetics and how specific sounds look like in a spectrogram), I zoomed in to select the exact vowel segment. This segment was then measured for the average F1 and F2 values. According to Davenport and Hannahs [3], it is specifically the "relative positions of the first and second formants (F1 and F2) are characteristic of specific vowels". While the F1 formant represents tongue height, the F2 formant represents backness or frontness [4].

This amounted to 352 data points in total, based on which the mean and average values of F1 and F2 formants for each vowel were calculated. This data was then read into R and vowel charts were created. The data consists of 8 vowels (cardinal vowels: _tea_ /i/, _father_ /&#593;/, _fed_ /&#603;/, _food_ /u/, _bought_ /&#596;/, the closest approximant to CV4, _cat_ /&aelig;/, and two extra central vowels _the_ /&#601;/, _some_ /&#652;/.

Those vowels were measured for F1 and F2 values once for regular and once for disguised voice modes for all 10 participants. In Images 1-2 below, I selected the sound /i/ as in the word _tea_. 

<img src="images/praat/Praat.PNG">
Image 1: Selecting the relevant segment. My own voice recording disguised.

<img src="images/praat/Praat2.PNG">
Image 2: Measuring F1 and F2 values. My own voice recording disguised.

### Analysis in R and Visualizations

First, I import the libraries:

```
library(dplyr)
library(ggplot2)
```

Next, I load the data for the normal voice mode and plotted the vowel chart (a common way of displaying vowel frequencies):
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

<img src="images/praat/R1.png">
Image 3: Vowel Chart Normal Voice Mode.

The same was repeated for the disguised voice mode:

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

<img src="images/praat/R2.png">
Image 4: Vowel Chart Disguised Voice Mode.

### Findings 

What we can see here is that there is a change in F1 and F2 frequencies. How signficant (and how big these changes are, would require some statistical testing. Moreover, in order to conclusively link this to a change in vowel position in the mouth (i.e. a change in pronunciation) further measurement techniques would be necessary (e.g. MRI Imaging or electropalatography). I hope that this could still nicely illustrate how useful vowel charts can be and how one can measure F1 and F2 values in _Praat_.

---
### References and Footnotes

*In this article, I used partly altered excerpts from this paper (for the full paper, please contact me).

[1]Boersma, Paul & Weenink, David (2024). Praat: doing phonetics by computer [Computer program]. Version 6.3.17, retrieved 21 August 2024 from http://www.praat.org/. <br/>
[2]RStudio Team (2024). RStudio: Integrated Development for R. RStudio, PBC, Boston, MA URL http://www.rstudio.com/.<br/>
[3]Davenport, M., & Hannahs, S.J. (2010). Introducing Phonetics and Phonology (3rd ed.). Routledge. https://doi.org/10.4324/9780203785447.<br/>
[4]Hansen Edwards, J. G. (2023). The Sounds of English Around the World: An Introduction to Phonetics and Phonology. Cambridge: Cambridge University Press.<br/>

A great overview on how to make such vowel charts in _R_ is presented by Joey Stanley:<br/>
https://joeystanley.com/blog/making-vowel-plots-in-r-part-1/<br/>
https://joeystanley.com/blog/making-vowel-plots-in-r-part-2/

[Back](https://ycvogt.github.io/my_portfolio/)