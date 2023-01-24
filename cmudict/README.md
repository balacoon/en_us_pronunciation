# CMUDict pronunciations for en-US

Original CMUDict: https://github.com/cmusphinx/cmudict.
It uses [ARPABET](https://en.wikipedia.org/wiki/ARPABET) as phoneme set.
Wiki provides mapping of ARPABET phonemes to IPA, which are mapped
to XSAMPA used in Balacoon. Complete mapping, following wiki and other
convertors ([3p mapping](https://github.com/rossellhayes/ipa/blob/main/data-raw/phonemes.csv))

<details>
<summary>ARPABET to XSAMPA mapping</summary>
<pre style="font-size: 12px">
AA      vowel   A:
AE      vowel   {
AH      vowel   V for primary stressed (AH1), @ for unstressed (AH0) or secondary stress (AH2)
AO      vowel   O:
AW      vowel   a U
AY      vowel   a I
B       stop    b
CH      affricate       tS
D       stop    d
DH      fricative       D
EH      vowel   E
ER      vowel   3: if stressed or in the mid of word, @` if unstressed at the end
EY      vowel   e I
F       fricative       f
G       stop    g
HH      aspirate        h
IH      vowel   I
IY      vowel   i: but when unstressed at the end of word is just i
JH      affricate       dZ
K       stop    k
L       liquid  l
M       nasal   m
N       nasal   n
NG      nasal   N
OW      vowel   o U
OY      vowel   O I
P       stop    p
R       liquid  r\
S       fricative       s
SH      fricative       S
T       stop    t
TH      fricative       T
UH      vowel   U
UW      vowel   u:
V       fricative       v
W       semivowel       w
Y       semivowel       j
Z       fricative       z
ZH      fricative       Z
</pre>
</details>

Some phonemes are splited into two during mapping: `AW, AY, EY, OW, OY`.
In case ARPABET phoneme is stressed,
stress will be assigned only to first XSAMPA phoneme in a pair.

## CMUDict inconsitencies

There are some limitations spotted in CMUDict. Those should be taken
into account in multi-lingual trainings.

* English phonemeset should have two types of r-colored vowels: `r-coloured schwa`
  ([wiki](https://en.wikipedia.org/wiki/R-colored_vowel); example: "color")
  and `open-mid central unrounded vowel`
  ([wiki](https://en.wikipedia.org/wiki/Open-mid_central_unrounded_vowel); example: "nurse").
  CMUDict uses `ER` in both cases. Following [cmudict-ipa](https://github.com/menelik3/cmudict-ipa)
  we disambiguate based on stress and location in the word. 
* There is no schwa phoneme in CMUDict. In ARPABET `AX` stands for schwa, but it's not used
  in the dict. [cmudict-ipa](https://github.com/menelik3/cmudict-ipa) hints how to disambiguate
  `AH` into `V` and `@` depending on stress and location in the word.
 
## Train/test split

Train/test split mirrors default one from original [Phonetisaurus paper](https://www.gavo.t.u-tokyo.ac.jp/~mine/paper/PDF/2013/INTERSPEECH_p1821-1825_t2013-8.PDF).
Table 4 contains results for given split (although names are messed up, CMUDict is supposed to score 24.4% WER).
Split can be downloaded from [sourceforge](https://master.dl.sourceforge.net/project/cmusphinx/G2P%20Models/phonetisaurus-cmudict-split.tar.gz?viasf=1)
or [googlecode](https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/phonetisaurus/is2013-conversion.tgz).
Worth specifying how multiple pronunciations are treated in the evaluation: if generated pronunciation matches any of valid pronunciations of a word,
it is considered to be phonetisized correctly.

## Filtering

CMUDict contains pronunciations for abbreviations and unnormalized words (shortenings, etc). This introduced noise into FST training, thus such
occurences are excluded. Single-letter words are difficult to disambiguate with abbreviations at normalization, therefore they are kept intact.
