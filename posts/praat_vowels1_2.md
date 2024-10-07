[Back](https://ycvogt.github.io/my_portfolio/)

##  Hypothesis & Significance Testing: Fundamental Frequency Alterations as Voice Disguise Strategy

In the following project* I explored inhowfar people change their fundamental frequency (F0, informally known as pitch) to disguise their voice. We conducted an experiment in our Phonetics Lab at university, recording every participant with a regular microphone and an electroglottograph (EGG). The participants (n=11) were prompted to read a text consisting of twelve sentences once in a 'normal' voice and once in a 'disguised voice' that should render them unrecognizable, even for people who know them well. They were not given any input as to what the strategy might entail to avoid any interference with their ad hoc solution to disguise their voice. In the following, I will only report on the acoustic data analysis. My research question was thus: Inhowfar do the participants employ alteration in fundamental frequency (F0)? The null hypothesis and alternative hypothesis are therefore:
* H0: There is no statistically significant difference between normal and disguised average fundamental frequencies among participants in continuous speech based on acoustic data.
* H1: There is a statistically significant difference between normal and disguised average fundamental frequencies among participants in continuous speech based on acoustic data.

The data was measured in _Praat_[1], by selecting the entire sentence of a participant and measuring the average F0 for the selected segment. This was repeated for all sentences of each participant, once for the regular and once for the disguised voice dataset. In the follwoing, I will analyze the data in _R_[2]. For more, see [Measuring & Visualizing Vowel Formants in Praat & R](/my_portfolio/posts/praat_vowels2_2.html).

### 1. Preprocessing and Exploring the Data
Next, we want to reorder the datastructure and get an overview of the collected data. The data was collected with one column for each student, the rows represent each the average pitch measured for the entire sentence. This is not very useful, so let's restructure it:

```
# Load your data
# Reshaping the data input to a more fitting format 
result <- data.frame() #empty dataframe to populate
for (i in colnames(df_normal))
  {
  Pitch <- df_normal[[i]]
  Student <- i
  Manner <- "normal"
  df<- data.frame(Pitch = Pitch, Student = Student, Manner = Manner)
  #print(df)
  result <- result %>% rbind(i, df)
  }
#remove lines with heads and a few N/A ones
#print(result)
result <- result[-c(1, 14, 27, 40, 53, 63, 66, 79, 92, 105, 118, 131, 138), ]
rownames(result) <- 1:nrow(result) #reset the index
print(result)
```
After repeating this for the disguised data, we merge the two datasets together: ```df_all <- rbind(result, result_disg)```.

Finally, we can plot it with ```plotly```:

<iframe src="images/praat/pitch.html" width="100%" height="400px" style="border:none;"></iframe><br/>


What we can see from these boxplots, is that there is a difference in the average pitches chosen in disguised mode, compared to normal mode.

### 2. Significance Testing

Now, we will do a paired, two-sided t-test to check across all datapoints of all students whether there is a difference between normal and disguised:
```
t.test(x=as.numeric(result$Pitch), y=as.numeric(result_disg$Pitch),
       alternative = c("two.sided", "less", "greater"),
       mu = 0, paired = TRUE, var.equal = FALSE,
       conf.level = 0.95)
```

The output is:
```
t = -5.4708, df = 129, p-value = 2.24e-07
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval: -35.25809 -16.52923
sample estimates: mean of the differences -25.89366
```

It is evident, that there is a clear, significant difference between the normal and disguised voice modes in terms of pitch. In other words, there is a difference in pitch between normal and disguised voice modes. The null hypothesis can therefore be rejected in favour of the alternative hypothesis, that there is a significant difference between the normal pitch and the pitch used in disguised voices. The negative t-value indicates that the sample mean of the normal voice mode data was significantly smaller than the one of the disguised voice mode - in other words, the pitch was lower in normal voice mode than in disguised voice mode. The first data exploration can show us a similar trend of the data, i.e. that many participants chose to raise their pitch, while only a few lowered it, and one would not alter it at all. 

### 3. Conclusion
What this means, is that we can definitively say that there is an overall difference in terms of pitch across all participants, when they were prompted to disguise their voice. What we do not know is how "strong" the change was. 

In [Measuring & Visualizing Vowel Formants in Praat & R](/my_portfolio/posts/praat_vowels2_2.html), I will explore alterations in the first and second vowel formant frequencies and how one can visualize this in vowel charts using _R_. 

### References
*In this article, I used partly altered excerpts, data and ideas from my paper (for the full paper, please contact me).

[1] Boersma, Paul & Weenink, David (2024). Praat: doing phonetics by computer [Computer program]. Version 6.3.17, retrieved 21 August 2024 from [http://www.praat.org/](http://www.praat.org/).<br/>
[2] RStudio Team (2024). RStudio: Integrated Development for R. RStudio, PBC, Boston. Available at [http://www.rstudio.com/](http://www.rstudio.com/).<br/>

[Back](https://ycvogt.github.io/my_portfolio/)
