---
layout: page
title: xwMOOC 기계학습
subtitle: 텍스트 데이터 전처리 -- `qdap`
output:
  html_document: 
    keep_md: yes
  pdf_document:
    latex_engine: xelatex
mainfont: NanumGothic
---
 
> ## 학습목표 {.objectives}
>
> * 텍스트 데이터 전처리 과정에 대해 이해한다.
> * `qdap` 팩키지를 통해 다양한 전처리 과정을 이해한다.




### 1. `qdap`, `tm` 텍스트 자료구조 비교

[텍스트 마이닝(Text Mining)](https://en.wikipedia.org/wiki/Text_mining)을 위한 R의 대표적인  팩키지가 `qdap` `tm` 이다.

| 팩키지명 |       원 텍스트         |       단어 빈도수(word counts)                          |
|----------|-------------------------|---------------------------------------------------------|
| **qdap** | 데이터프레임(Dataframe) |   단어 빈도 행렬(Word Frequency Matrix)                 |
| **tm**   | 말뭉치 (Corpus)         | 단어 문서행렬(Term Document Matrix)/문서 단어행렬(Document Term Matrix) |


[qdap](https://cran.r-project.org/web/packages/qdap/)은 원 텍스트 데이터를 데이터프레임 형태로 저장하는 반면에,
[tm](https://cran.r-project.org/web/packages/tm/) 팩키지는 `Corpus` 말뭉치 형태로 원 텍스트 데이터를 저장한다는 점에서 차이가 난다.

두 팩키지 모두 공통으로 사용하는 단어/용어 빈도수에는 행렬(`matrix`)을 사용한다. 이를 그림을 표현하면 다음과 같다.

<img src="fig/ml-text-qdap-tm-comp.png" alt="qdap, tm 비교" width="77%" />

* `qdap` 텍스트 원문 `qdap_dat` &rarr; `qview(qdap_dat)`
* `tm` 텍스트 원문 `tm_dat` &rarr; inspect(tm_dat)
* `qdap` 단어 빈도수 `qdap_wfm` &rarr; summary(qdap_wfm)
* `tm` 단어 빈도수 `tm_tdm` &rarr; inspect(tm_tdm)

### 2. `tm`, `qdap` 데이터 정제 기능

<img src="fig/ml-text-cleaning.png" alt="tm, qdap 전처리 기능" width="70%" />


단어 주머니 기법을 활용하여 텍스트를 분석할 때, 데이터 정제를 통해 단어를 합산하는데 큰 도움이 된다.
영어 단어 예를 들어, statistics, statistical, stats 등은 모두 통계라는 한 단어로 정리되면 좋다.

`tm` 팩키지 및 `base` 팩키지에 내장된 데이터 정제 기능은 다음과 같다.

* tolower(): `base`에 포함된 함수로 모든 문자를 소문자로 변환.
* removePunctuation(): `tm`에 포함된 함수로 모든 구두점을 제거.
* removeNumbers(): `tm`에 포함된 함수로 숫자를 제거
* stripWhitespace(): `tm`에 포함된 함수로 공백(whitespace)을 제거 

`qdap`에는 좀더 다양한 텍스트 정제 함수가 지원된다.

* bracketX(): 괄호 내 모든 텍스트 제거 
    * "It's (very) nice" &rarr; "It's nice"
* replace_number(): 아라비아 숫자를 대응되는 영어문자로 변환
    * "7" &rarr; "seven")
* replace_abbreviation(): 축약어를 대응되는 전체 문자로 풀어냄
    * "Jan" &rarr; "Janunary")
* replace_contraction(): 단어 축약을 원래 상태로 되돌림
    * "can't" &rarr; "can not")
* replace_symbol(): 일반 기호를 대응되는 단어로 교체
     * "$" &rarr; "dollar"

텍스트가 너무 자주 출현하여 거의 정보를 제공하지 않는 단어를 **불용어(stop words)** 라고 부른다.
`tm` 팩키지에는 영어기준으로 174개 불용어가 등재되어 있다. 또한, 관심있는 주제로 문서를 모았다면 
수집된 거의 모든 문서에 특정 단어가 포함되어 있어 이것도 도움이 되지 않아 불용어에 등록하여 텍스트 
분석을 수행한다.

~~~ {.r}
removeWords(text, stopwords("english"))
stop_words_lst <- c("rstudio", "statistics", stopwords("english"))
removeWords(text, stop_words_lst)
~~~

`stopwords("english")` 영어불용어 사전에 "rstudio", "statistics" 단어를 더해서 불용어 사전을 완성하고 나서 
removeWords() 함수로 새로 갱신된 사전에 맞춰 불용어를 정리한다.


### 참고문헌

* [Tyler W. Rinker, qdap-tm Package Compatibility](https://cran.r-project.org/web/packages/qdap/vignettes/tm_package_compatibility.pdf)
* [Basic Text Mining in R](https://rstudio-pubs-static.s3.amazonaws.com/31867_8236987cf0a8444e962ccd2aec46d9c3.html)
* [Hands-On Data Science with R Text Mining, Graham.Williams](http://onepager.togaware.com/TextMiningO.pdf)
* [Natural Language Processing Tutorial](http://www.vikparuchuri.com/blog/natural-language-processing-tutorial/)

* [Statistics meets rhetoric: A text analysis of "I Have a Dream" in R](http://anythingbutrbitrary.blogspot.kr/2014/01/statistics-meets-rhetoric-text-analysis.html)
* [How to Create WordCloud of Twitter Data using R Programming](http://technokarak.com/how-to-create-wordcloud-of-twitter-data-using-r-programming.html)
* [How to Clean the Twitter Data using R – Twitter Mining Tutorial](http://technokarak.com/how-to-clean-the-twitter-data-using-r-twitter-mining-tutorial.html)
* [How to Load Twitter Tweets in R Environment](http://technokarak.com/how-to-load-twitter-tweets-in-r-environment.html)
