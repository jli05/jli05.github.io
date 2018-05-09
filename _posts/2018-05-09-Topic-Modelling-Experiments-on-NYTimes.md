---
title: Topic Modelling Experiments on NYTimes Corpus 
layout: post
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

# Introduction
On a full scale corpus, simple formulation for Topic Modelling as Non-negative Matrix Decomposition no longer renders satisfactory results; we have to use more flexible formulation as the Latent Dirichlet Allocation. We compare the results from two main algorithms for LDA:
* Spectral LDA <https://github.com/Mega-DatA-Lab/SpectralLDA-Spark>
* Online Variational "Online Learning for Latent Dirichlet Allocation", NIPS 2010

# Dataset
The data is from the [UCI Bag of Words NYTimes dataset](https://archive.ics.uci.edu/ml/datasets/bag+of+words).

Statistics | Value
--- | ---
Vocabulary Size    |  200000
Number of Articles |  299650

Quantiles | Levels (.05, .25, .5, .75, .95)
--- | ---
Number of Distinct Tokens per Article  |  (1.0, 181.0, 237.0, 1117.0, 1778.0)
Document Length | (1.0, 217.0, 308.0, 1554.0, 3586.0)

# Model Parameters
We use non-informative priors.

Parameter | Value
--- | ---
Number of Topics (k)  | 10
All $$\alpha_i$$ for prior Topic Distribution      | 1.0
All $$\beta_{ij}$$ for prior Topic Word Distribution | 1.0

For the Online Variational algorithm, the minibatch size is as large as possible to fully utilise 8-cores on an AWS Instance. It runs 10 full iterations over the corpus.

# Results
We run on the entire corpus, then on economy-related, China-related, music-related sub-collection of documents. We can see that Spectral LDA gennerally discovers pertinent and consistent topics, esp. on collections with known theme.

## Entire Corpus
* Spectral LDA
    Topic #0: company, million, percent, companies, stock, market, billion, business, shares, analyst, quarter, sales, deal, firm, chief, share, customer, investor, executive, cost
    Topic #1: team, game, season, play, player, games, point, coach, run, win, hit, played, won, left, yard, shot, ball, goal, guy, playing
    Topic #2: zzz_al_gore, zzz_bush, campaign, zzz_george_bush, president, percent, 
    election, republican, tax, political, vote, voter, democratic, zzz_white_house, presidential, zzz_republican, zzz_clinton, school, bill, zzz_senate
    Topic #3: percent, stock, market, million, quarter, company, billion, companies,
     point, economy, fund, analyst, tax, rate, sales, cut, growth, investor, earning, zzz_al_gore
    Topic #4: com, web, www, information, question, newspaper, site, company, busine
    ss, zzz_eastern, daily, sport, commentary, mail, separate, online, spot, need, computer, copy
    Topic #5: zzz_held, guard, premature, publication, released, advisory, send, advise, zzz_boston_globe, zzz_istanbul, zzz_johannesburg, nyt, held, advance, zzz_jakarta, zzz_lexington, vacation, error, zzz_mesa, zzz_seoul
    Topic #6: file, zzz_boston_globe, tonight, spot, zzz_los_angeles_daily_new, slugged, zzz_new_york, zzz_xxx, earlier, article, zzz_x_x_x, today, incorrectly, zzz_holland, zzz_washington, sport, advise, com, error, misstated
    Topic #7: question, newspaper, copy, fall, zzz_diane, com, percent, kill, daily, zzz_eastern, information, sport, mandatory, commentary, palestinian, zzz_u_s, a
    ttack, zzz_israel, marked, separate
    Topic #8: test, zzz_houston_chronicle, school, zzz_seattle_post_intelligencer, s
    tudent, ignore, program, system, drug, teacher, point, testing, official, patien
    t, zzz_kansas_city, cancer, anthrax, doctor, result, wire
    Topic #9: file, zzz_new_york, sport, zzz_los_angeles, school, read, internet, no
    tebook, email, zzz_chuck, output, zzz_calif, wrote, book, student, copy, zzz_ana
    heim_angel, zzz_los_angeles_dodger, fall, zzz_diane

* Variational



