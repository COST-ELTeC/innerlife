# Data and code for: Towards a computational history of modernism in European literary history: Mapping the Inner Lives of Characters in the European Novel, 1840-1920

[![DOI](https://zenodo.org/badge/660108194.svg)](https://zenodo.org/badge/latestdoi/660108194)

This folder contains data, code and results described in the following paper: Tamara Radak, Lou Burnard, Pieter Francois, Agnes Hilger, Fotis Jannidis, Gábor Palkó, Roxana Patras, Michael Preminger, Diana Santos, Christof Schöch (2023): "Towards a computational history of modernism in European literary history: Mapping the Inner Lives of Characters in the European Novel (1840–1920)", _Open Research Europe: Digital Humanities_, DOI: https://doi.org/10.12688/openreseurope.16290.1. 

## Licence 

All raw data in the repository is made available with a [Creative Commons Zero (CC0)](https://creativecommons.org/share-your-work/public-domain/cc0/) licence. 

All figures and code in the repository are made available with a [Creative Commons Attribution 4.0 International CC BY)](https://creativecommons.org/licenses/by/4.0/%0A) licence. 

Please use the citation suggestion below, or cite the paper linked to above, if you use the data or figures in your research.  

## Citation suggestion 

Tamara Radak et al.: "Data and code for: Towards a computational history of modernism in European literary history: Mapping the Inner Lives of Characters in the European Novel, 1840-1920", v1. Github/Zenodo, June 2023. URL: https://github.com/COST-ELTeC/innerlife/, DOI: https://doi.org/10.5281/zenodo.8094222. 

## Data sources 

The following is a list of repositories that were used as data sources for the experiments conducted in the present project. They are all repositories that fall under the umbrella of the '[European Literary Text Collection' (ELTeC)](https://github.com/COST-ELTeC/ELTeC), DOI: [10.5281/zenodo.4271467](https://doi.org/10.5281/zenodo.4271467). 

- [ELTeC-deu](https://github.com/COST-ELTeC/ELTeC-deu), DOI: [10.5281/zenodo.3543243](https://doi.org/10.5281/zenodo.3543243) 
- [ELTeC-eng](https://github.com/COST-ELTeC/ELTeC-eng), DOI: [10.5281/zenodo.3462536](https://doi.org/10.5281/zenodo.3462536) 
- [ELTeC-fra](https://github.com/COST-ELTeC/ELTeC-fra), DOI: [10.5281/zenodo.3462535](https://doi.org/10.5281/zenodo.3462535) 
- [ELTeC-hun](https://github.com/COST-ELTeC/ELTeC-hun), DOI: [10.5281/zenodo.42716375](https://doi.org/10.5281/zenodo.42716375) 
- [ELTeC-nor](https://github.com/COST-ELTeC/ELTeC-nor), DOI: [10.5281/zenodo.3521571](https://doi.org/10.5281/zenodo.3521571) 
- [ELTeC-pol](https://github.com/COST-ELTeC/ELTeC-pol), DOI: [10.5281/zenodo.4662596](https://doi.org/10.5281/zenodo.4662596) 
- [ELTeC-por](https://github.com/COST-ELTeC/ELTeC-por), DOI: [10.5281/zenodo.3492067](https://doi.org/10.5281/zenodo.3492067) 
- [ELTeC-rom](https://github.com/COST-ELTeC/ELTeC-rom), DOI: [10.5281/zenodo.3543111](https://doi.org/10.5281/zenodo.3543111) 
- [ELTeC-slv](https://github.com/COST-ELTeC/ELTeC-slv), DOI: [10.5281/zenodo.3518108](https://doi.org/10.5281/zenodo.3518108) 
- [ELTeC-spa](https://github.com/COST-ELTeC/ELTeC-spa), DOI: [10.5281/zenodo.3662758](https://doi.org/10.5281/zenodo.3662758) 


## Organization

This repository contains the following main folders:

- lists of inner life verbs for various languages (`0_verbs`)
- data regarding verb frequency statistics in novels (`1_data`)
- scripts written in XSLT, Perl and Python for generating, analysing and visualizing the data (`2_scripts`)
- results visualized in various forms (`3_results`). 

Each of the main folders is further separated into various kinds of aspects of our analysis. Finally, each data or results folder contains one folder for each language or dataset. 


## Workflow

The workflow is as follows:

- first, each ELTeC level2 file is simplified by the `filter.py` script, which outputs a version of each file containing a bag of blank-delimited tokens ;
- next, we define a list of the tokens for which frequency over time is to be calculated. For the innerlife experiment, subject-experts were asked to select from a list of verbs in descending frequency order. The resulting list is saved in the file `innerVerbs.xml` ;
- next, we produce a data file showing the frequency of each such token in each text, for each language for which data is available: this is done by the perl script `quickCount.pl`; for language xxx, the frequency table for method zzz is saved in a file called `xxx/zzz_verbFreqs.dat`
- finally, we run scripts to visualise and analyse the datasets. Scripts both in R and in python are included, thanks to contributions from ELTeC partners, producing boxplots, regression plots, density plots, etc.


## Details

### Step 1: filter the corpora

The filter.py script reduces each text file to a bag of tokens containing either the content of each `<w>` element selected from  the file, or the value of its @lemma attribute. 

The `<w>` elements to be processed are selected according to their POS code, as given by the @pos attribute.

The special value `CONTENT` may be used to select <w> elememts with POS codes NOUN ADJ ADV or VERB

These parameters are supplied on the command line, like this:

~~~~
python Scripts/filter.py [lang] [pos] [wot]
~~~~
where
   `lang` identifies the language and the repo to be processed : the script expects to find text data at [repoRoot][lang]/level-2
    `pos` identifies the filter to be applied  is either a single pos value e.g. 'NOUN' or 'CONTENT' which means 'NOUN|ADV|ADJ|VERB"  
    `wot` indicates the output required and is either 'lemma' or 'form'
    
for example:
~~~~
python Scripts/filter.py deu VERB lemma
#extract just the lemmas for each word tagged as a VERB in  each German text

python Scripts/filter.py eng NOUN content
#extract just the word forms for each word tagged as a NOUN in  each English text
~~~~

By "lemmas" we mean strictly the content of the @lemma attribute, as it appears in the level2 file. No lemmatisation or pattern matching is carried out by these scripts.

The script writes a new folder called `[pos]/[wot]` in the appropriate language folder, and creates within it a file for each text, listing each [wot] it contains delimited by spaces and in text order. It selects from the input file only `<w>` elements which are contained by a `<div type='chapter'>`. 

The filename used for this dataset comprises the XML identifier of the source text, its publication date, and  a code derived from its balance criteria. For example 'HU00662_1901T4MMH.txt' is derived from the text with identifier `HU00662`, published in 1901, which is from timeslot 4, has a male author, is of medium length, and has a high reprint count.


### Step 2: decide what to count

This is an iterative process. 

For the inner life verbs experiment, we first run the XSLT script `verbFreq.xsl` to produce a list of all distinct lemmas marked with pos code `VERB` and their frequencies in a given corpus. Only lemmas with frequencies above 100 are listed,  in descending frequency order.  

To run it for corpus [xxx] use the following command line:
~~~~
saxon -xi [path]ELTeC-[xxx]/driver-2.tei verbFreq.xsl
~~~~
This will generate an XML file containing lines like this
~~~
<lemma form="beszél" freq="6890"/>
~~~
Output from this script is stored in a file called `verbFreqs.xml` in each language folder. 

Language experts were invited to scan this list and to choose from it verbs indicative of inner life. They also supplied an English translation and a categorisation for each lemma selected. 

The results of this process are held in a simple XML file called `innerVerbs.xml` It combines a number of `<list>` elements, one for each language corpus analysed. Each list contains `<lemma>` elements, whose attributes specify the form of the lemma to be counted, its frequency in the whole corpus, and its categorization. For non-English corpora, a translation into English is also provided. Here for exmple is the start of the German list:
~~~~
 <list type="manual" xml:lang="deu">
  <lemma form="sehen" cat="perception" freq="50350"  trans="to see"/>
  <lemma form="wollen" cat="volition" freq="47800"  trans="to want"/>
  <lemma form="müssen" cat="moral" freq="40047"  trans="to have to"/>
~~~~

###  Step 3: do the counting

The perl script `quickCount.pl` produces counts for each token identified in the lists produced by the previous step, one for each language. The results are in tabular form, with one row for each text in the corpus, and one column for each token counted. The token may be either the form of the lemma, or the category assigned to it. 

The script is run at the command line as follows:
~~~~
perl Scripts/quickCount.pl [lang] [where] [which]
~~~~

- [lang] identifies the input repository, e.g. 'fra'
- [where] gives the location of the input data, e.g. `VERB/lemma` . The script will look for input files at the location `repoRoot-[lang]/[where]` (where `repoRoot` is defined in the perl script)
- [which] specifies which type of frequency list is to be produced. Currently two types are recognised :
    - `manual` for verbs selected manually
    - `w2v` for verbs selected on the basis of a word2vec procedure 

It produces a file called `ELTeC-data/[lang]/[which]Counts.dat`, in which each row contains 

- the text identifier
- the year of publication
- the total number of verbs in the text
- the total number of "inner verbs" in the text
- counts for each specified item

Here for example is the start of the output produced by the commnd
~~~~
perl Scripts/quickCounts.pl eng VERB/lemma manual | sort

0textId year verbs innerVerbs know:perception see:perception think:cognition seem:perception feel:affect hear:perception mean:cognition believe:cognition want:volition like:affect love:affect try:volition wish:volition suffer:physiology sleep:physiology blush:physiology 
ENG18400 1840 37878 2951 593 443 487 245 265 268 74 117 92 121 49 13 118 33 27 6 
ENG18410 1841 31727 2255 263 312 295 441 241 191 48 123 15 49 60 26 138 34 7 12 
ENG18411 1844 5536 402 75 81 43 30 19 53 7 12 2 4 36 6 20 7 6 1 
~~~~
This shows that in the text ENG18400, from year 1840, there are 37,878 verb occurrences, of which 2951 are classed as indicative of inner life; 593 of these are occurrences of "know", which we categorise as "perception", and so on.
    
## Step 4 : Visualise the data

The Python script `visualize.py` written by Christof Schoech produces visualisations for this data. To run it, ensure that there is a copy of the datafiles output in the preceding step in a folder called `data` which is a sibling of the folder containing the script. The resulting plots are placed in a folder called `results/xxx' for each language `xxx`.

