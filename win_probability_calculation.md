---
title: "Calculating the probability of the team winning based on Leetify statistics"
author: [Pingviinituutti]
date: "2026-09-16"
keywords: [Counter Strike, Tim, Probability, Win]
---

# Calculating the probability of the team winning based on Leetify statistics

The probability $P$ of winning the next game $G$ is calculated as the [weighted arithmetic mean (link)](https://en.wikipedia.org/wiki/Weighted_arithmetic_mean#Mathematical_definition) of the past 15 games played by the team (with at least two players of the team involved). Using the weighted arithmetic mean, the probability becomes: 

\begin{equation*}
P(G) = \frac{\sum_{i=1}^{n} w_i x_i}{\sum_{i=1}^{n} w_i}
\end{equation*}

where:

* $n$ is the number of unique games the players have played (yes, sometimes players in the team play together);
* $i$ is a game in the pool of games each player in the prediction has played; and
* $w_i$ is the calculated weight of that game; and
* $x_i$ is the result of the game: $1$ for a win, $0.5$ for a tie, or $0$ for a loss;

The weight of the game, $w_i$, is calculated as:

\begin{equation*}
w_i = \frac{m}{5} \times w_{i,R_{tot}} \times w_{i,R_{diff}}
\end{equation*}

where:

* $m$ is the number of team players playing in the game, i.e., ${m \in [2, 5]}$ 
* $w_{i,R_{tot}}$ is the weight for the game based on how many rounds were played; and
* $w_{i,R_{diff}}$ is weight for the game based on the difference between the number of rounds won by the teams.

The idea is that games with fewer rounds should have a higher weight, i.e., closer to $1$, because the fewer number of rounds were played the harder one of the teams were dominating. Therefore, 13-0 is the best score and 15-15 is the worst. The result 15-15 means tie, motivating the minimum weight value of $w_{i,R_{tot}}$ to be $0.5$ so the prediction moves more towards the uncertain 50-50 probability.

\begin{equation*}
w_{i,R_{tot}} = 1 - \frac{1}{2} \times \frac{R_p - R_{min}}{R_{max} - R_{min}} = 1 - \frac{1}{2} \times \frac{R_p - 13}{30 - 13} = 1 - \frac{R_p - 13}{34}
\end{equation*}

where:

* $R_p$ is the total number of rounds played in the game;
* $R_{min}$ equals $13$; and
* $R_{max}$ equals $2 \times 15 = 30$.


The weight $w_{i,R_{tot}}$ should be higher if the difference in rounds won between the teams is high with the maximum difference being 13. A tie result 15-15 motivates the weight value of $w_{i,R_{diff}}$ to be $0.5$.

\begin{equation*}
w_{i,R_{diff}} = \frac{1}{2} \times \left(1 + \frac{|R_t - R_e|}{13}\right) = \frac{1}{2} + \frac{|R_t - R_e|}{26}
\end{equation*}

where:

* $R_t$ is the number of rounds the team won;
* $R_e$ is the number of rounds the enemy team won.

Finally, the complete calculation of the weight $w_i$ becomes:

\begin{align*}
w_i &= \frac{m}{5} \times w_{i,R_{tot}} \times w_{i,R_{diff}} \\ 
    &= \frac{m}{5} \times 
    \left(1 - \frac{R_p - 13}{34}\right) \times \left(\frac{1}{2} + \frac{|R_t - R_e|}{26}\right)
\end{align*}
