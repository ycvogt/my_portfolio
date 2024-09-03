[Back](https://ycvogt.github.io/my_portfolio/)

# Unsupervised Sentiment Analysis of Bridgerton YouTube Trailer Comments

The popular Netflix Series "Bridgerton" (based on the novel series by Julia Quinn) seems to cause quite many emotions whenever new season trailers are released. What do people think about the different seasons based on their trailers? Let’s find out!

**Skills**: webscraping, data cleaning, social media opinion mining, NLP, unsupervised sentiment analysis<br/> 
**Libraries**: youtube-comment-downloader, pandas, spacy, emoji, transformers, wordcloud, plotly

<img src="images/movies/Almacks.jpg" width="300"/>
[Almack's Assembly Rooms [1]]

For this project, I first scraped the comments with the library youtube-comment-downloader [2].

#code

Next, preprocessing was necessary: removing emojis and emoticons, punctuation, usernames, tokenizing, lowercasing, and shaping the comments into useful dataframes and as input to the model:

#code

The dataset was now ready for the model to be predicted. I decided to use disilbert-base-uncased-emotion [3], as it offered a more fine-grained display of emotional categories ('sadness', 'joy', 'love', 'anger', 'fear', 'surprise') compared to the popular three-way distinction (positve, negative, neutral). The model assigned to every post a percentage of how likely each of these emotions are. I decided to keep all of them instead of choosing the most likely one (i.e. the one with the highest score), as this might help in some situations to represent a more nuanced picture of the user's sentiment. In addition, this model is trained on twitter data, which is fairly close to YouTube comments, as both are part of the wider social media register.

#code

Finally, I made some visualizations based on the results of the model:

#images

#interpretation/findings

### Caveats and Shortcomings

*  The model has a reported accuracy of 93.8	and F1-score of 93.79. However, the model does of course not always assign the most fitting category, e.g. "Gossip girl + pride and prejudice + 50 shades[...]" are classified as "angry", but "what a joke" is correctly identified as "angry". It is a bit hit and miss with sarcasm as well. This could be improved with some fine-tuning of the model.
*  I only looked at the 200 most popular comments (high reply count and likes) of each trailer. While this may give a good picture of the most prominent views, it is still excluding many smaller opinions that could shift some of the total counts/percentages. 
* The insights gained from this project are just based on the platform YouTube and do not account for the overall impression of these trailers in all social media. This is something to consider.

### References:

[1] "Almack's Assembly Rooms". Wikimedia Commons. https://commons.wikimedia.org/wiki/File:Almack%27s_Assembly_Rooms_inside.jpg (last accessed Sept 9, 2024).<br/> 
[2] https://pypi.org/project/youtube-comment-downloader<br/> 
[3] https://huggingface.co/bhadresh-savani/distilbert-base-uncased-emotion<br/> 

[Back](https://ycvogt.github.io/my_portfolio/)
