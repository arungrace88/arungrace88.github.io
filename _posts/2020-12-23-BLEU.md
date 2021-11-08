---
layout: post
title: NLP Metric: BLEU (BiLingual Evaluation Understudy) Score
---

![Photo by Waldemar Brandt on Unsplash](../images/BLEU/waldemar-brandt-U3Ptj3jafX8-unsplash.jpg "Photo by Waldemar Brandt on Unsplash")

In this article, we will look into a NLP metric which is commonly used in Machine Translation (MT) applications - BLEU, an acronym for BiLingual Evaluation Understudy. 

# BLEU Score

The idea behind the BLEU score is to check how close a machine translated text is to a human translation. Better the score, closer resemblance to human translation. Let us explore more about this by considering an example given below.

## Example

<span style="color:orange">Machine translation (a.k.a Candidate) = "the cat and the cat on the mat"</span>

<span style="color:green">Reference translation 1: "The cat is on the mat"</span>

<span style="color:green">Reference translation 2: "There is a cat on the mat"</span>

The BLEU score is calculated as:

<p align="center">BLEU = BP &times; exp(&sum;<sub>n=1...N</sub> w<sub>n</sub>&times; ln(p<sub>n</sub>)</p>

where,

pn is the modified precision for n-gram (will discuss this shortly)

wn is the weight (value between 0 and 1) such that 

<p align="center">&sum;<sub>n=1...N</sub> w<sub>n</sub> = 1</p>

BP is the brevity penalty to penalise short machine translations. It is calculated as:

<p align="center">BP = 1, when c&gt;r</p>
<p align="center">BP = e<sup>1-(r&divide;c)</sup>, when c&le;r</p>

where,

'c' is the length of candidate sentence

'r' is the effective reference corpus length. In other words, it is the closest reference sentence length to the candidate sentence.

## Modified Precision

The concept of modified precision can be best explained with an example of 1-gram (n=1) unique words from our above example. The list of unique 1-gram (unigram)words from our candidate sentence are:

<p align="center"> ( the, cat, and, on, mat)</p>

The list of unique 2-gram (bigram) words from our candidate sentence are:

<p align="center">(the cat, cat and, and the, cat on, on the, the mat)</p>

and so on for 3-gram, 4-gram, etc.

The Table 1 shows the unique unigram and its corresponding count in the candidate sentence. The third column is the clipped count which is defined as the maximum frequency of the unqiue unigram in the reference translations (either 1 or 2).

Table1:Unique Unigram Example

|Unique Unigram|Count|Clipped Count|
|--------------|-----|-------------|
|the           | 3   |    2        |
|cat           | 2   |    1        |
|and           | 1   |    0        |
|on            | 1   |    1        |
|mat           | 1   |    1        |

The total clipped counts for the unique unigram in reference sentence is 5 (2+1+0+1+1). The total number of counts of unique unigram in candidate sentence is 8 (3+2+1+1+1). The modified precision is then calculated as:

<p align="center">Modified Precision = Total Clipped Count &divide; Total Count</p>
<p align="center">= 5&divide;8</p>
<p align="center">= 0.625</p>

On a similar line, modified precision for unique bigram can be calculated. Give a try!

## BLEU Score Calculation

Usually BLEU takes N=4 and wn =1/N. For the above example we could find that

<p align="center">p<sub>1</sub> = 5 &divide;8</p>
<p align="center">p<sub>2</sub> = 4 &divide;7</p>
<p align="center">p<sub>3</sub> = 2 &divide;6</p>
<p align="center">p<sub>4</sub> = 1 &divide;5</p>
<p align="center">w<sub>1</sub> = w<sub>2</sub> = w<sub>3</sub> = w<sub>4</sub> = 1 &divide;4</p>
<p align="center">c = 8</p>
<p align="center">r = 7</p>
<p align="center">Hence, BP = 1</p>

When we substitute above values into the BLEU equation, we get a score of 0.3928. Please try from your side!.

Let us check whether our manual calculation is correct by comparing it with BLEU score calculated using 'nltk' python library.

    # Import the library
    import nltk
    # Reference translation 1
    ref_1 = "the cat is on the mat".split()
    # Reference translation 2
    ref_2 = "there is a cat on the mat".split()
    # Candidate translation a.k.a Machine translation
    candidate = "the cat and the cat on the mat".split()
    # Calculate BLEU Score
    bleu = nltk.translate.bleu_score.sentence_bleu(references=[ref_1,ref_2],hypothesis=candidate, weights=(0.25, 0.25, 0.25, 0.25))
    print("The BLEU score for the machine translated candidate sentence is:", bleu)


The above code outputs the BLEU score as 0.392814650900513. Our manual score match with the score returned from the python `nltk` library.

## Summary

In this post we discussed about a NLP metric used in machine translation - BLEU score. If you are interested in diving deep into more details of BLEU score, refer to the original research paper [BLEU: a Method for Automatic Evaluation of Machine Translation](https://www.aclweb.org/anthology/P02-1040.pdf).

Hope you learned something. Please do let me know your feedback or suggestions!

## Bibliography
[1]. [Lei Mao's Blog](https://leimao.github.io/blog/BLEU-Score/)

[2]. Papineni, K., Roukos, S., Ward, T. and Zhu, W.J., 2002, July. BLEU: a method for automatic evaluation of machine translation. In Proceedings of the 40th annual meeting of the Association for Computational Linguistics (pp. 311-318).

