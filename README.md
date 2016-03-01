MGB Challenge 2016 Arabic Track
====

## Introduction ##

This repository is for additional resources for Arabic Broadcast Transcription Challenge 2016. For introduction and dataset released for the challenge, please refer to the challenge website http://alt.qcri.org/mgb-ar-2016.

## Resources ##

* SCLITE

  `wget  ftp://jaguar.ncsl.nist.gov/pub/sctk-2.4.10-20151007-1312Z.tar.bz2`

* DDSCORE

## Transcript Generation ##

This section explains the xml transcript generation process that we followed to generate training transcripts (xml) and the dev transcripts (xml) from the original transcripts provided by AL Jazeera

* Aligning data:

First, we perform a lightly supervised alignment between the ctm transcription file generated by the ASR system and the manual transcription provided by AL Jazeera for each program (which does not have any timing information). For more details read this paper : `http://alt.qcri.org/MGB_challenge_Arabic_Track_2016/MGB_Arabic_description_2016.pdf`

The code for the alignment is at the location: `/transcriptgeneration/src/mgbmain/ArabicASRChallenge`

When this code is run giving it the appropriate arguments, it generates a transcript file: 'ALL_MOD.tra' which a tab delimited file, whose each line is a speech utterance and its details:

`01E4B05D-E8C2-4F92-819F-140B607ABA50.xml_mtHdv-bAsm-dAr-Al<ftA'-AlflsTynyp_00:00:34,65_00:00:37,15 الأقصى المبارك فإنه قد ثبت بالوجه الشرعي     Words:7 Correct:3      Correct:42      Ins:0   Del:4   WMER:57.0       PMER:50.0       AWD:2.8 Start:1 End:0`

Semantics of the above line are as follows:
`<program_ID>.xml_<Speaker_name (in Buckwalter Format)><space><utterance_text><\t><some statistics from the alignment><\t><correct%><\t><insertions and deletions (alignments)><\t><word error rate%><\t><graheme match error rate%><\t><Average Word Duration>`

The word error rate is calculated using levenshtein distance (No Substitutions, Insertions and Deletions cost 1)

The transcript file 'ALL_MOD.tra' takes considerable time to generate but it is ok, as it is a one time process. We are currently looking into making the code faster

* Generating transcript xmls

Once we have the 'ALL_MOD.tra', we now parse this file to generate the trancript xml files. XML files are generated using the java class `/transcriptgeneration/src/mgbmain/XMLGeneration.java`.

We generate both the bukwalter and the utf8 formats. The flags that need to be set for the program are:

 -a <arg>           Yes/No: annotate latin words in the corpus with the
                    tag @@LAT@@
 -b <arg>           Yes/No: remove segments from the dev set that have
                    overlap speech
 -bw <arg>          Yes/No: Convert to bukwalter or not
 -d <arg>           transcript xml files destination folder
 -h <arg>           Yes/No: remove hesitation mark or not
 -hh <arg>          Yes/No: remove double ## at the start of a word
 -norm <arg>        Yes/No: normalize the arabic text or not, remove
                    punctuations
 -num <arg>         Yes/No: flag for mapping the arabic indic numbers to
                    Arabic numberals
 -p <arg>           Absolute Path: program tdf file path
 -rd <arg>          Yes/No: remove diacritics
 -rmEnglish <arg>   Yes/No Remove the english words
 -rmNum <arg>       Yes/No Remove numbers
 -t <arg>           Train/Dev: Generate xml for training or dev set
 -tra <arg>         Absolute Path: Give the ALL.tra file

The settings that we use for generating the train xml trancripts for bukwalter are:

`-a yes 
-b no 
-bw yes 
-d /Users/alt-sameerk/Documents/mgb/dev_xml_2016_03_1/bw/ 
-h yes 
-hh no 
-norm yes 
-num yes 
-rmNum no 
-rmEnglish no 
-p /Users/alt-sameerk/Documents/mgb/exp-2015-11-10/aja_tdf.txt 
-rd yes 
-t train 
-tra /Users/alt-sameerk/Documents/mgb/ALL_MOD.tra`

and for the utf8 xml transcripts:
`-a no 
-b no 
-bw no 
-d /Users/alt-sameerk/Documents/mgb/dev_xml_2016_03_1/utf8/ 
-h no 
-hh no 
-norm no 
-num no 
-rmNum no 
-rmEnglish no 
-p /Users/alt-sameerk/Documents/mgb/exp-2015-11-10/aja_tdf.txt 
-rd no 
-t train 
-tra /Users/alt-sameerk/Documents/mgb/ALL_MOD.tra`

For the dev xml transcript generation we use the same class, but just changing the `-t` flag to `dev` and also generating the dev transcript file using the python script `/extras/trs2xml.py`

## Language Modelling Text ##

We provide the normalized and unnormalized language modelling text by parsing the articles archive provided by AL Jazeera. The normalized bukwalter LM text is generated using the java class `/transcriptgeneration/src/mgbmain/ExtractLMText.java`

## Kaldi Recipe ##

To get you started with an ASR system, we have provided a recipe (under progress) which can be used as a starting point.
The recipe is at `/baseline/recipe`. The recipe scripts are well commented and you can get all the information by looking at the scripts

## Lexicon ##

Lexicon is under progress and has been put in github and participants will be notified when any changes are made to the lexicon
