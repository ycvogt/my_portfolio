[Back](https://ycvogt.github.io/my_portfolio/)

---

### Exploring Linguistic Databases in R

Libraries: lingtypology, tidyverse
Datasets: Phoible, Glottolog, APiCS

In this project I will use two linguistic datasets (Phoible, Glottolog, APiCS), join and explore them to generate insight into cross-linguistic phonetic questions. 

Phoible is a detailed repository with phonetic inventories of 2186 distinct languages. While they point out that this is a convenience sample, it will still provide interesting tendencies. This repository does not list languages simply by name or ISO Code. Instead, it lists "doculects", i.e. how the phoneme inventory is described in a specific document. Therefore, multiple documents can describe one variety, there may be just one for another. 

Phoible already includes some information from Glottolog, which is a large catalogue of langauges and their status and families. Glottolog also provides coordinates for mapping the languages and tracking feature distributions. Phoible, however, only provides information on language families and the Glottocode (i.e. same ID as used on Glottolog), but not coordinates. 

Therefore, I decided to work with both datasets and the library of lingtypology. This way, I could map the different phonetic features in Phoible together with the coordinates provided in Glottolog.

What a "language" is, is seemingly easy to identify in everyday contexts. However, socio-political aspects need to be considered aside from linguistic ones that can complicate the status of a variety quickly. This is especially true when dealing with varieties of diverse status that are also documented differently. For that reason, I stick with the term "variety" throughout this post.

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

```
#filtering out the features of interest
specific_vowel <- filter(tibble_phoible, GlyphID==c("0079","0079+02D0"))%>% group_by(Glottocode)
view(specific_vowel)

#joining the two datasets at the key "Glottocode"
df_result <-right_join(df_glotto,specific_vowel, by="Glottocode")
tibble_results <- as_tibble(df_result)
view(tibble_results)

#mappling the features and languages on world map
map.feature(tibble_results$name,
            features = tibble_results$Phoneme,
            latitude = tibble_results$latitude,
            longitude = tibble_results$longitude,
            title = "vowels")
```

Grouping by _Glottocode_ rather than _Language Name_, ISO Codes or Inventory ID (in Phoible) was tricky. The one variable that both datasets shared was the Glottocode.
I decided to use the GlyphID which is the ID given to the phonemes as it made filtering easier. IPA symbols which are used to represent phonemes would have been more problematic. In order to still render IPA symbols, I just used the column _Phoneme_ instad of _GlyphID_ as they represented the same item in the final filtered dataset. 

Another way to link the two sources is provided on the page of Phoible: https://phoible.org/faq#integrating-geographic-information.  

<iframe src="images/distribution_y.html" width="100%" height="400px" style="border:none;"></iframe>

Similarly, I explored how many languages have clicks:

<iframe src="images/clicks_languages.html" width="100%" height="400px" style="border:none;"></iframe>

...and tones:

<iframe src="images/tonal_languages.html" width="100%" height="400px" style="border:none;"></iframe>

References:

Hammarström, Harald & Forkel, Robert & Haspelmath, Martin & Bank, Sebastian (eds.) 2024. _Glottolog 5.0_. Leipzig: Max Planck Institute for Evolutionary Anthropology. https://doi.org/10.5281/zenodo.10804357 (Available online at http://glottolog.org, Accessed on 2024-07-20.)

Michaelis, Susanne Maria & Maurer, Philippe & Haspelmath, Martin & Huber, Magnus (eds.) 2013. _Atlas of Pidgin and Creole Language Structures Online_. Leipzig: Max Planck Institute for Evolutionary Anthropology. (Available online at http://apics-online.info, Accessed on 2024-07-21.)

Moran, Steven & McCloy, Daniel (eds.) 2019. _PHOIBLE 2.0_. Jena: Max Planck Institute for the Science of Human History. (Available online at http://phoible.org, Accessed on 2024-07-20.)

Moroz G (2017). _lingtypology: easy mapping for Linguistic Typology_.<https://CRAN.R-project.org/package=lingtypology>.

---
[Back](https://ycvogt.github.io/my_portfolio/)