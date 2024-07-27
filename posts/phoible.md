[Back](https://ycvogt.github.io/my_portfolio/)

---

### Exploring Linguistic Databases in R

Libraries: lingtypology, tidyverse

Datasets: Phoible, Glottolog

In this project I will use two linguistic datasets (_Phoible_[1], _Glottolog_[2]), join and explore them to generate insight into cross-linguistic phonetic questions. _Phoible_ is a detailed repository with phonetic inventories of 2186 distinct languages. While they point out that this is a convenience sample, it will still provide interesting tendencies. This repository does not list languages simply by name or ISO Code. Instead, it lists "doculects", i.e. how the phoneme inventory is described in a specific document. Therefore, multiple documents can describe one variety, there may be just one for another. 

What a “language” is might seem easy to identify in everyday contexts. However, socio-political aspects need to be considered aside from linguistic ones and this can complicate the status of a variety quickly. This is especially true when dealing with varieties of diverse status that are also documented differently. For that reason, I stick with the term “variety” throughout this post.

_Phoible_ already includes some information from _Glottolog_, which is a large catalogue of langauges and their status and families. _Glottolog_ also provides coordinates for mapping the languages and tracking feature distributions. Therefore, I decided to work with both datasets and the library of _lingtypology_[3]. This way, I could map the different phonetic features in Phoible together with the coordinates provided in _Glottolog_. It should be stressed that I specifically wanted to try it this way to challenge myself.

```
#loading phoible
url_ <- "https://github.com/phoible/dev/blob/master/data/phoible.csv?raw=true"
col_types <- cols(InventoryID='i', Marginal='l', .default='c')
phoible <- read_csv(url(url_), col_types=col_types)
tibble_phoible <- as_tibble(phoible)
view(tibble_phoible)

#loading glottolog
df_glotto <- read.csv(file="C:/Users/vogty/Downloads/languages_and_dialects_geo.csv", header=TRUE, stringsAsFactors=FALSE, encoding = "UTF-8")
df_glotto <- rename(df_glotto, Glottocode = glottocode)
view(as_tibble(df_glotto))
```

Next, I filtered the features of interest and saved them in a tibble. Grouping by _Glottocode_ rather than _LanguageName_, _ISO_ Codes or _InventoryID_ (in Phoible) was tricky. The one variable that both datasets shared was the _Glottocode_. Another way to link the two sources is provided on the website of [Phoible](https://phoible.org/faq#integrating-geographic-information). 

```
#filtering out the features of interest
specific_vowel <- filter(tibble_phoible, GlyphID==c("0079","0079+02D0"))%>% group_by(Glottocode)
view(specific_vowel)

#joining the two datasets at the key "Glottocode"
df_result <-right_join(df_glotto,specific_vowel, by="Glottocode")
tibble_results <- as_tibble(df_result)
view(tibble_results)
```

I wanted to start with some simple analyses:
<ol>
<li>Check if there are indeed as many varieties in the dataset as they say.</li>
<li>How many different segment types (consonants, vowels, tones) are there?</li>
<li>Which variety has the smallest/largest phoneme inventory?</li>
<li>Which vowel, consonant and tone are most frequently used among all languages?</li>
</ol>

```
number_langs <- nrow(by_lang) #check number of varieties/doculects
print(number_langs) #3020 correct!

by_segmentclass <- tibble_phoible %>% group_by(SegmentClass)%>% tally(sort=TRUE)
view(by_segmentclass) #segment types and their counts

by_lang <- tibble_phoible %>% group_by(InventoryID)%>% tally(sort=TRUE)
view(by_lang) #most
by_lang_asc <- arrange(by_lang, n)
view(by_lang_asc) #least

by_vowel_phoneme <- filter(tibble_phoible, SegmentClass=="vowel") %>% group_by(Phoneme)%>% tally(sort=TRUE)
view(by_vowel_phoneme) #most frequent vowel is /i/
by_cons_phoneme <- filter(tibble_phoible, SegmentClass=="consonant")%>% group_by(Phoneme)%>% tally(sort=TRUE)
view(by_cons_phoneme) #most frequent consonant is /m/
by_tone_phoneme <- filter(tibble_phoible, SegmentClass=="tone")%>% group_by(Phoneme)%>% tally(sort=TRUE)
view(by_tone_phoneme) #most frequent tone /˦/
```

<ol>
<li>There are indeed 3020 varieties/doculects documented.</li>
<li>The results:
<table>
<thead>
<tr>
<th align="left">Segment Type</th>
<th align="right">Count</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">consonant</td>
<td align="right">72282</td>
</tr>
<tr>
<td align="left">vowel</td>
<td align="right">31052</td>
</tr>
<tr>
<td align="left">tone</td>
<td align="right">2150</td>
</tr>
</tbody>
</table>
</li>
<li>The variety/doculect with the most phonemes is !Xóõ (East Taa) with 161 phonemes. The varieties/doculects with the fewest phonemes are Pirahã and Rotokas with only 11 phonemes each!</li>
<li>The most frequent vowel is /i/ (present in 2779 varieties), the most frequent consonant is /m/ (present in 2915 varieties) and the most frequent tone is a high tone /˦/ (present in 552 varieties).</li>
</ol>

Next, I decided to use the _GlyphID_ which is the ID given to the phonemes as it made filtering easier. IPA symbols which are used to represent phonemes would have been more problematic. In order to still render IPA symbols, I just used the column _Phoneme_ instad of _GlyphID_ as they represented the same item in the final filtered dataset. 

```
#mapping the features and languages on world map
map.feature(tibble_results$name,
            features = tibble_results$Phoneme,
            latitude = tibble_results$latitude,
            longitude = tibble_results$longitude,
            title = "vowels")
```

Feel free to explore by zooming in, and clicking on the data points for the language names.

<iframe src="images/distribution_y.html" width="100%" height="400px" style="border:none;"></iframe>


Similarly, I explored how many varieties have clicks:

<iframe src="images/clicks_languages.html" width="100%" height="400px" style="border:none;"></iframe>

```
click <- filter(tibble_phoible, click=="+")%>% count(LanguageName, sort=TRUE)
view(click)
nrow(click) #21
```

According to Phoible, 21 varieties have clicks. The varieties with the most clicks are: !Xóõ (n=45), !Xun (n=40), !XU (n=37), NAMA (n=20), Xhosa (n=18).

We can repeat the same analysis for tones:

<iframe src="images/tonal_languages.html" width="100%" height="400px" style="border:none;"></iframe>

Based on these datasets, varieties with clicks can only be found in Africa, while tones are present in many varieties in Africa, Asia and North America.

Tones are more frequent (n=635 varieties). The varieties with the most distinct tones are: Bafut (n=10), Buli (n=10), Ticuna (n=9), Babungo (n=9) and Nizaa (n=9).


References:

[1]Moran, Steven & McCloy, Daniel (eds.) 2019. _PHOIBLE 2.0_. Jena: Max Planck Institute for the Science of Human History. (Available online at <http://phoible.org>, Accessed on 2024-07-20.)

[2]Hammarström, Harald & Forkel, Robert & Haspelmath, Martin & Bank, Sebastian (eds.) 2024. _Glottolog 5.0_. Leipzig: Max Planck Institute for Evolutionary Anthropology. <https://doi.org/10.5281/zenodo.10804357> (Available online at <http://glottolog.org>, Accessed on 2024-07-20.)

[3]Moroz G (2017). _lingtypology: easy mapping for Linguistic Typology_.<https://CRAN.R-project.org/package=lingtypology>.

---
[Back](https://ycvogt.github.io/my_portfolio/)
