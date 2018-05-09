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

Topic | Words
--- | ---
 #0 | company, million, percent, companies, stock, market, billion, business, shares, analyst, quarter, sales, deal, firm, chief, share, customer, investor, executive, cost
 #1 | team, game, season, play, player, games, point, coach, run, win, hit, played, won, left, yard, shot, ball, goal, guy, playing
 #2 | zzz_al_gore, zzz_bush, campaign, zzz_george_bush, president, percent, 
election, republican, tax, political, vote, voter, democratic, zzz_white_house, presidential, zzz_republican, zzz_clinton, school, bill, zzz_senate
 #3 | percent, stock, market, million, quarter, company, billion, companies,
     point, economy, fund, analyst, tax, rate, sales, cut, growth, investor, earning, zzz_al_gore
 #4 | com, web, www, information, question, newspaper, site, company, busine
    ss, zzz_eastern, daily, sport, commentary, mail, separate, online, spot, need, computer, copy
 #5 | zzz_held, guard, premature, publication, released, advisory, send, advise, zzz_boston_globe, zzz_istanbul, zzz_johannesburg, nyt, held, advance, zzz_jakarta, zzz_lexington, vacation, error, zzz_mesa, zzz_seoul
 #6 | file, zzz_boston_globe, tonight, spot, zzz_los_angeles_daily_new, slugged, zzz_new_york, zzz_xxx, earlier, article, zzz_x_x_x, today, incorrectly, zzz_holland, zzz_washington, sport, advise, com, error, misstated
 #7 | question, newspaper, copy, fall, zzz_diane, com, percent, kill, daily, zzz_eastern, information, sport, mandatory, commentary, palestinian, zzz_u_s, a
    ttack, zzz_israel, marked, separate
 #8 | test, zzz_houston_chronicle, school, zzz_seattle_post_intelligencer, s
    tudent, ignore, program, system, drug, teacher, point, testing, official, patien
    t, zzz_kansas_city, cancer, anthrax, doctor, result, wire
 #9 | file, zzz_new_york, sport, zzz_los_angeles, school, read, internet, no
    tebook, email, zzz_chuck, output, zzz_calif, wrote, book, student, copy, zzz_ana
    heim_angel, zzz_los_angeles_dodger, fall, zzz_diane

* Variational

Topic | Words
--- | ---
 #0 | com, web, computer, information, site, mail, question, www, zzz_enron, online, daily, newspaper, sites, sport, user, internet, palm, game, zzz_internet, zzz_nfl
 #1 | team, game, season, player, play, games, point, run, win, won, coach, hit, right, left, guy, ball, shot, night, minutes, played
 #2 | zzz_united_states, zzz_u_s, government, official, military, country, war, zzz_american, zzz_afghanistan, zzz_china, zzz_taliban, attack, terrorist, nation, zzz_bush, leader, group, foreign, countries, forces
 #3 | campaign, zzz_bush, president, zzz_george_bush, zzz_al_gore, election, law, school, court, political, republican, vote, zzz_white_house, right, federal, democratic, bill, public, voter, zzz_senate
 #4 | film, show, look, movie, car, room, hour, home, water, night, house, set, place, part, big, high, town, find, small, zzz_new_york
 #5 | percent, company, million, companies, market, business, stock, billion, cost, money, industry, plan, firm, economy, customer, sales, analyst, pay, consumer, tax
 #6 | palestinian, zzz_israel, leader, peace, official, zzz_israeli, attack, government, israeli, zzz_yasser_arafat, violence, minister, war, israelis, talk, meeting, killed, group, zzz_bush, zzz_iraq
 #7 | family, book, women, school, father, children, friend, son, mother, wife, home, boy, parent, told, word, young, student, daughter, right, child
 #8 | music, show, book, art, human, artist, program, research, scientist, director, million, song, part, history, network, group, zzz_new_york, magazine, century, project
 #9 | drug, official, patient, doctor, found, case, police, death, problem, attack, medical, disease, officer, health, test, cell, study, hospital, trial, zzz_fbi

## Economy-related
Collection of documents with words fitting regex "^econ"

* Spectral LDA

Topic | Words
--- | ---
 #0 | government, zzz_united_states, zzz_china, official, zzz_u_s, country, 
zzz_bush, leader, countries, military, political, administration, foreign, natio
n, zzz_american, trade, zzz_russia, war, economic, president
 #1 | percent, job, school, worker, economy, business, com, student, rate, c
ompanies, number, high, program, economist, women, growth, information, unemploy
ment, employees, rates
 #2 | company, percent, million, companies, quarter, billion, analyst, stock
, market, sales, share, business, earning, cent, industry, cost, profit, revenue
, executive, chief
 #3 | percent, stock, market, economy, investor, point, companies, rates, zz
z_fed, rate, index, prices, quarter, growth, earning, analyst, interest, zzz_nas
daq, fund, fell
 #4 | zzz_bush, president, administration, tax, zzz_white_house, cut, zzz_un
ited_states, zzz_u_s, zzz_congress, official, leader, zzz_washington, attack, zz
z_clinton, plan, military, nation, zzz_al_gore, campaign, terrorist
 #5 | percent, quarter, sales, rate, economy, stock, growth, point, market, 
million, fell, analyst, rose, index, earning, rates, increase, job, average, num
ber
 #6 | zzz_al_gore, zzz_george_bush, campaign, president, election, voter, zz
z_clinton, democratic, republican, political, presidential, zzz_bill_clinton, vo
te, poll, zzz_republican, democrat, zzz_party, vice, candidate, candidates
 #7 | tax, cut, zzz_bush, billion, plan, spending, taxes, economy, bill, zzz_congress, income, money, zzz_social_security, surplus, zzz_white_house, federal, trillion, proposal, zzz_senate, zzz_democrat
 #8 | las, como, los, zzz_latin_trade, telefono, articulo, espanol, transmiten, fax, paises, del, articulos, una, sobre, zzz_america_latina, economia, notas, tiene, mundo, categorias
 #9 | sales, indicator, economic, claim, jobless, scheduled, dates, listed, major, weekly, consumer, order, leading, home, prices, construction, retail, price, spending, producer

hgjhgjhg.

* Variational

Topic | Words
--- | ---
#0 | zzz_enron, bill, prices, oil, million, percent, market, company, progr
am, price, farmer, farm, industry, federal, law, companies, barrel, zzz_congress
, production, states
#1 | las, con, los, como, una, zzz_latin_trade, fax, por, mas, telefono, di
ce, articulo, million, espanol, paises, sobre, transmiten, economia, articulos, 
financial
#2 | percent, tax, economy, stock, market, cut, economic, billion, rate, sp
ending, rates, growth, quarter, economist, companies, point, interest, fund, mon
ey, investor
#3 | palestinian, zzz_israel, drug, percent, official, government, group, m
illion, plan, patient, peace, doctor, israeli, care, zzz_yasser_arafat, black, h
ealth, right, zzz_clinton, zzz_israeli
#4 | car, company, zzz_microsoft, system, industry, percent, market, power,
 airline, cost, passenger, flight, companies, million, economy, business, seat, 
airlines, sales, price
#5 | com, school, book, home, show, look, american, family, women, student,
 question, children, zzz_washington, beach, palm, daily, zzz_new_york, high, pla
ce, room
#6 | government, zzz_united_states, country, zzz_u_s, official, zzz_china, 
economic, countries, zzz_bush, zzz_american, military, nation, leader, war, fore
ign, political, attack, president, zzz_russia, power
#7 | company, companies, million, percent, business, market, industry, job,
 firm, zzz_internet, money, customer, high, sales, technology, computer, executi
ve, web, deal, team
#8 | energy, plant, water, gas, million, power, fuel, official, oil, plan, 
percent, industry, worker, problem, zzz_bush, environmental, project, cost, admi
nistration, companies
#9 | zzz_george_bush, zzz_al_gore, president, campaign, election, political
, zzz_bush, democratic, republican, vote, voter, zzz_white_house, leader, zzz_cl
inton, presidential, government, public, school, zzz_republican, right



