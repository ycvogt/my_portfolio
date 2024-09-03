# Unsupervised Sentiment Analysis of Bridgerton YouTube Trailer Comments

The popular Netflix Series "Bridgerton" (based on the novel series by Julia Quinn) seems to cause quite many emotions whenever new season trailers are released. What do people think about the different seasons based on their trailers? Let’s find out!

**Skills**: webscraping, data cleaning, social media opinion mining, NLP, unsupervised sentiment analysis<br/> 
**Libraries**: youtube-comment-downloader, pandas, spacy, emoji, transformers, wordcloud, plotly

Almacks.jpg
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

Caveat: 
not always accurate categorization (gossip as negative, but what a joke correctly identified, hit and miss with sarcasm)
only looked at 200 most popular comments (high reply count and likes), while this may give a good picture of the grand scheme of opinions, one must not forget that many smaller comments may shift some of the categorizations 
this is just based on youtube, and does not account for the overall impression in all social media

References:

[1] "Almack's Assembly Rooms". Wikimedia Commons. https://commons.wikimedia.org/wiki/File:Almack%27s_Assembly_Rooms_inside.jpg (last accessed Sept 9, 2024).
[2] https://pypi.org/project/youtube-comment-downloader
[3] https://huggingface.co/bhadresh-savani/distilbert-base-uncased-emotion 


