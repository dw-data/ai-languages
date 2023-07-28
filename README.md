# How AI pioneers teach ChatGPT & Co. their languages

AI tools, from ChatGPT to Google Translate, are useless to billions of people in the Global South who don't work in western languages. Researchers and startups from Africa and other parts of the world are changing that.

*In this repository, you will find the methodology, data and code behind
the story that came out of this analysis.*

Story by [Kira Schacht](https://www.twitter.com/daten_drang), additional interviews by [Hanna Demissie](https://www.linkedin.com/in/hannademissie/). 

**Read the full article on DW.com:** [English](https://www.dw.com/a-66331763)


## Dataset

The file `Data.xslx` contains the data our analysis is based on. For further information about its structure please refer to the `metadata` sheet in the file.


## Data sources

### Common Crawl

The [Common Crawl](https://commoncrawl.org/) is an openly available dataset consisting of billions of web pages from the internet. It is an important data source for many AI models: For instance, [about 60% of the examples used to train ChatGPT's version 3.5 come from this collection](https://arxiv.org/pdf/2005.14165.pdf).

Starting in 2018, Common Crawl analyzed the language of each page in its dataset. An overview downloaded from their [GitHub](https://commoncrawl.github.io/cc-crawl-statistics/plots/languages) provides the 3-letter language code, number and share of pages for each language.

As of now, Common Crawl is able to identify 160 different languages and up to 3 languages per document. 51% of the total web pages in the dataset have an unkown language, mostly those scraped before 2018. After this time, only around 2-3% of pages have no specified language.

For this story, we analyzed all pages with known languages. Almost half of these are in English alone. Below are the top 10 languages in the Common Crawl:

| language   | pages          | share |
|------------|----------------|-------|
| English    | 56.562.220.256 | 46%   |
| Russian    | 9.075.940.671  | 7%    |
| Chinese    | 7.304.517.478  | 6%    |
| German     | 7.033.437.439  | 6%    |
| Japanese   | 6.127.097.773  | 5%    |
| French     | 5.752.349.660  | 5%    |
| Spanish    | 5.428.973.977  | 4%    |
| Italian    | 3.012.638.667  | 2%    |
| Portuguese | 2.520.457.387  | 2%    |
| Dutch      | 2.272.085.034  | 2%    |

Dataset downloaded on 14th July 2023.

### Ethnologue

[Ethnologue](https://www.ethnologue.com/) is a global database of languages operated by Christian NGO SIL International. It provides extensive data for over 7,000 languages worldwide.

We scraped the publicly available data on their database, which offers, among other things:

- Language code and name
- A rough categorization of how many native speakers a language has (> 1B, 1M-1B, 10K-1M, <10k, None)
- Vitality (institutional, stable, endangered, or extinct)
- Level of digital language support
- Language family
- Main country and region

Please refer to the Ethnologue language pages (e.g. [here](https://www.ethnologue.com/language/amh/)) for more information about their categorizations.

We used the Ethnologue database to assign language titles to the 3-letter language codes used in the Common Crawl dataset, as well as to get information about the Digital Support Level, Vitality and Geography of languages with more than 1 million speakers.


### Wikidata

To compare each language's representation in the Common Crawl with its speakers, we needed exact information on the number of native speakers for each language logged in the Common Crawl. Since official Databases like Ethnologue only offer a category, we sourced the figures from Wikidata. The exact query and its output can be found [here](https://query.wikidata.org/#SELECT%20%3FlanguageLabel%20%3FlanguageCode%20%3Fspeakers%20%3FpointInTime%20%3FappliesToPartLabel%20%3FrefStatedIn%20%3FrefURL%20%3FrefArchiveUrl%20WHERE%20%7B%0A%20%20%3Flanguage%20wdt%3AP220%20%3FlanguageCode%20%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20rdfs%3Alabel%20%3FlanguageLabel%20%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20p%3AP1098%20%3FspeakersStatement%20.%0A%20%20%3FspeakersStatement%20ps%3AP1098%20%3Fspeakers%20.%0A%20%20%3FspeakersStatement%20prov%3AwasDerivedFrom%20%3Fref.%0A%20%20%0A%20%20OPTIONAL%20%7B%20%3FspeakersStatement%20pq%3AP585%20%3FpointInTime%20.%20%7D%0A%20%20OPTIONAL%20%7B%0A%20%20%20%20%3FspeakersStatement%20pq%3AP518%20%3FappliesToPart%20.%0A%20%20%20%20%3FappliesToPart%20rdfs%3Alabel%20%3FappliesToPartLabel%20FILTER%28LANG%28%3FappliesToPartLabel%29%20%3D%20%22en%22%29.%20%7D%0A%20%20OPTIONAL%20%7B%20%20%20%20%0A%20%20%20%20%20%20%3Fref%20pr%3AP248%20%3Fsource.%0A%20%20%20%20%20%20%3Fsource%20rdfs%3Alabel%20%3FrefStatedIn%20filter%20%28lang%28%3FrefStatedIn%29%20%3D%20%22en%22%29.%0A%20%20%20%20%20%20%0A%20%20%20%20%20%20OPTIONAL%20%7B%20%3Fref%20pr%3AP1065%20%3FrefArchiveUrl.%20%7D%0A%20%20%20%20%20%20OPTIONAL%20%7B%20%3Fref%20pr%3AP854%20%3FrefURL.%20%7D%0A%20%20%7D%0A%20%20FILTER%20%28LANG%28%3FlanguageLabel%29%20%3D%20%22en%22%20%26%26%20STRLEN%28%3FlanguageCode%29%20%3D%203%20%26%26%20%3FlanguageCode%20IN%20%28%22eng%22%2C%22rus%22%2C%22zho%22%2C%22deu%22%2C%22jpn%22%2C%22fra%22%2C%22spa%22%2C%22ita%22%2C%22por%22%2C%22nld%22%2C%22pol%22%2C%22ces%22%2C%22tur%22%2C%22vie%22%2C%22ind%22%2C%22swe%22%2C%22kor%22%2C%22ara%22%2C%22fas%22%2C%22ron%22%2C%22ell%22%2C%22hun%22%2C%22dan%22%2C%22fin%22%2C%22ukr%22%2C%22tha%22%2C%22nor%22%2C%22slk%22%2C%22bul%22%2C%22cat%22%2C%22heb%22%2C%22srp%22%2C%22hrv%22%2C%22lit%22%2C%22slv%22%2C%22est%22%2C%22hin%22%2C%22lat%22%2C%22msa%22%2C%22lav%22%2C%22ben%22%2C%22aze%22%2C%22tam%22%2C%22bos%22%2C%22kat%22%2C%22sqi%22%2C%22isl%22%2C%22glg%22%2C%22hye%22%2C%22eus%22%2C%22mkd%22%2C%22urd%22%2C%22kaz%22%2C%22nep%22%2C%22mal%22%2C%22nno%22%2C%22bel%22%2C%22mon%22%2C%22tel%22%2C%22uzb%22%2C%22mar%22%2C%22afr%22%2C%22mya%22%2C%22cym%22%2C%22kan%22%2C%22guj%22%2C%22khm%22%2C%22epo%22%2C%22swa%22%2C%22sin%22%2C%22tat%22%2C%22tgl%22%2C%22kur%22%2C%22gle%22%2C%22pan%22%2C%22kir%22%2C%22tgk%22%2C%22som%22%2C%22fao%22%2C%22ltz%22%2C%22mlt%22%2C%22lao%22%2C%22ori%22%2C%22oci%22%2C%22pus%22%2C%22mlg%22%2C%22war%22%2C%22fry%22%2C%22san%22%2C%22amh%22%2C%22bre%22%2C%22cos%22%2C%22ceb%22%2C%22jav%22%2C%22hau%22%2C%22kin%22%2C%22tuk%22%2C%22yid%22%2C%22div%22%2C%22gla%22%2C%22bak%22%2C%22hat%22%2C%22roh%22%2C%22bod%22%2C%22asm%22%2C%22uig%22%2C%22zul%22%2C%22mri%22%2C%22kal%22%2C%22snd%22%2C%22xho%22%2C%22vol%22%2C%22sun%22%2C%22yor%22%2C%22grn%22%2C%22que%22%2C%22ina%22%2C%22tir%22%2C%22sco%22%2C%22bih%22%2C%22haw%22%2C%22syr%22%2C%22sot%22%2C%22sna%22%2C%22smo%22%2C%22nya%22%2C%22orm%22%2C%22ibo%22%2C%22glv%22%2C%22abk%22%2C%22ile%22%2C%22blu%22%2C%22hmn%22%2C%22lin%22%2C%22tsn%22%2C%22kha%22%2C%22iku%22%2C%22lug%22%2C%22aar%22%2C%22wol%22%2C%22bis%22%2C%22nso%22%2C%22ipk%22%2C%22mfe%22%2C%22run%22%2C%22fij%22%2C%22aym%22%2C%22dzo%22%2C%22ton%22%2C%22crs%22%2C%22aka%22%2C%22tso%22%2C%22zha%22%2C%22chr%22%2C%22ssw%22%2C%22nau%22%2C%22sag%22%2C%22ven%22%2C%22got%22%2C%22sux%22%2C%22kas%22%2C%22lif%22%2C%22new%22%29%29%20.%0A%7D%0A).

Where available, we manually picked the number of native speakers for each language from the query results. Languages that could not be found via Wikidata were supplemented manually, which is indicated in the column `manual` of the `wikidata` tab in the data file.

# Interview partners

- [Asmelash Teka Hadgu](https://twitter.com/asmelashteka),	founder of [Lesan.AI](https://lesan.ai/)
- Mekdes Gebrewold, founder of [Ashagari consultancy](https://ashagari.com/)