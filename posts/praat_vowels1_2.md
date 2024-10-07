[Back](https://ycvogt.github.io/my_portfolio/)

##  Hypothesis & Significance Testing: Fundamental Frequency Alterations as Voice Disguise Strategy

In the following project* I explored inhowfar people change their fundamental frequency (F0, informally known as pitch) to disguise their voice. We conducted an experiment in our Phonetics Lab at university, recording every participant with a regular microphone and an electroglottograph (EGG). The participants (n=11) were prompted to read a text consisting of twelve sentences once in a ’normal’ voice and once in a ’disguised voice’ that should render them unrecognizable, even for people who know them well. They were not given any input as to what the strategy might entail to avoid any interference with their ad hoc solution to disguise their voice. In the following, I will only report on the acoustic data analysis. My research question was thus: Inhowfar do the participants employ alteration in fundamental frequency (F0)? The null hypothesis and alternative hypothesis are therefore:
* H0: There is no statistically significant difference between normal and disguised average fundamental frequencies among participants in continuous speech based on acoustic.
* H1: There is a statistically significant difference between normal and disguised average fundamental frequencies among participants in continuous speech based on acoustic.

The data was measured in _Praat_, by selecting the entire sentence of a participant and measuring the average F0 for the selected segment. This was repeated for all sentences of each participant, once for the regular and once for the disguised voice dataset. For more, see [Measuring & Visualizing Vowel Formants in Praat & R](/my_portfolio/posts/praat_vowels2_2.html).

### 1. Preprocessing and Exploring the Data
First, we want to reorder the datastructure and get an overview of the collected data. The data was collected with one column for each student, the rows represent each the average pitch measured for the entire sentence. This is not very useful, so let's restructure it:

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

<iframe src="images/praat/pitch.html" width="100%" height="400px" style="border:none;"></iframe>


What we can see from these boxplots, is that there is a difference in the average pitches chosen in disguised mode, compared to normal mode.

### 2. Significance Testing
#ToDo

### 3. Conclusion
#ToDo
The null hypothesis...alternative hypothesis...
In [Measuring & Visualizing Vowel Formants in Praat & R](/my_portfolio/posts/praat_vowels2_2.html), I will explore alterations in the first and second vowel formant frequencies and how one can visualize this in vowel charts using _R_. 

### References
*In this article, I used partly altered excerpts, data and ideas from my paper (for the full paper, please contact me).

[Back](https://ycvogt.github.io/my_portfolio/)
