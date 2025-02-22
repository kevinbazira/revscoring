[![Build Status](https://travis-ci.org/wikimedia/revscoring.svg?branch=master)](https://travis-ci.org/wikimedia/revscoring)
[![Test coverage](https://codecov.io/gh/wikimedia/revscoring/branch/master/graph/badge.svg)](https://codecov.io/gh/wikimedia/revscoring)
# Revision Scoring

A generic, machine learning-based revision scoring system designed to help automate critical wiki-work — for example, vandalism detection and removal. This library powers [ORES](https://ores.wikimedia.org).

## Example


Using a scorer_model to score a revision::
```
  import mwapi
  from revscoring import Model
  from revscoring.extractors.api.extractor import Extractor

  with open("models/enwiki.damaging.linear_svc.model") as f:
       scorer_model = Model.load(f)

  extractor = Extractor(mwapi.Session(host="https://en.wikipedia.org",
                                          user_agent="revscoring demo"))

  feature_values = list(extractor.extract(123456789, scorer_model.features))

  print(scorer_model.score(feature_values))
  {'prediction': True, 'probability': {False: 0.4694409344514984, True: 0.5305590655485017}}
  ```


# Installation

The easiest way to install is via the Python package installer
(pip).

``pip install revscoring``

You may find that some of the dependencies fail to compile (namely
`scipy`, `numpy` and `sklearn`).  In that case, you'll need to install some
dependencies in your operating system.

### Ubuntu & Debian:
  *  Run ``sudo apt-get install python3-dev g++ gfortran liblapack-dev libopenblas-dev``
  *  Run ``apt-get install aspell-ar aspell-bn aspell-is myspell-cs myspell-nl myspell-en-us myspell-en-gb myspell-en-au myspell-et voikko-fi myspell-fr myspell-de-at myspell-de-ch myspell-de-de myspell-he myspell-hr myspell-hu aspell-id myspell-it myspell-nb myspell-fa aspell-pl myspell-pt myspell-es hunspell-sr aspell-sv aspell-ta myspell-ru myspell-uk hunspell-vi aspell-el myspell-lv aspell-ro myspell-ca hunspell-gl hunspell-bs aspell-hi hunspell-eu``
<!-- ### Windows:
<i>TODO</i>
-->
### MacOS:
  Using Homebrew and pip, installing `revscoring` and `enchant` can be accomplished
  as follows::

* brew install aspell --with-all-languages
* brew install enchant
* pip install --no-binary pyenchant revscoring

#### Adding languages in aspell (MacOS only)
```
cd /tmp
wget http://ftp.gnu.org/gnu/aspell/dict/pt/aspell-pt-0.50-2.tar.bz2
bzip2 -dc aspell-pt-0.50-2.tar.bz2 | tar xvf -
cd aspell-pt-0.50-2
./configure
make
sudo make install
 ```
 Caveats: <br>
  <b><u> The differences between the `aspell` and `myspell` dictionaries can cause </b>
    <b> <u>some of the tests to fail </b>


Finally, in order to make use of language features, you'll need to download
some NLTK data.  The following command will get the necessary corpora.

``python -m nltk.downloader omw sentiwordnet stopwords wordnet``

You'll also need to install [enchant](https://en.wikipedia.org/wiki/Enchant_(software))-compatible
dictionaries of the languages you'd like to use.  We recommend the following:

* languages.arabic: aspell-ar
* languages.basque: hunspell-eu
* languages.bengali: aspell-bn
* languages.bosnian: hunspell-bs
* languages.catalan: myspell-ca
* languages.czech: myspell-cs
* languages.croatian: myspell-hr
* languages.dutch: myspell-nl
* languages.english: myspell-en-us myspell-en-gb myspell-en-au
* languages.estonian: myspell-et
* languages.finnish: voikko-fi
* languages.french: myspell-fr
* languages.galician: hunspell-gl
* languages.german: myspell-de-at myspell-de-ch myspell-de-de
* languages.greek: aspell-el
* languages.hebrew: myspell-he
* languages.hindi: aspell-hi
* languages.hungarian: myspell-hu
* languages.icelandic: aspell-is
* languages.indonesian: aspell-id
* languages.italian: myspell-it
* languages.latvian: myspell-lv
* languages.norwegian: myspell-nb
* languages.persian: myspell-fa
* languages.polish: aspell-pl
* languages.portuguese: myspell-pt
* languages.serbian: hunspell-sr
* languages.spanish: myspell-es
* languages.swedish: aspell-sv
* languages.tamil: aspell-ta
* languages.russian: myspell-ru
* languages.ukrainian: aspell-uk
* languages.vietnamese: hunspell-vi
# Running tests
Make sure you installed test dependencies:

``
$ pip install -r test-requirements.txt
``

Then run:

``
$ pytest . -vv
``

# Authors
  *   [Aaron Halfaker](http://halfaker.info)
  *   [Helder](https://github.com/he7d3r)
  *   [Adam Roses Wight](https://mediawiki.org/wiki/User:Adamw)
  *   [Amir Sarabadani](https://github.com/Ladsgroup)
