---
layout: post
title: "A Look Into International Goalscoring"
categories: ['football', 'data analysis', 'pandas']
permalink: /portfolio/2024/01/26/international-goalscorer-analysis/
# author: 'you'
---

<p>Competitive play is the meat and potatoes of international football. Who are the best players to score on a consistent basis when it really counts? "Back in my day, goals were few and far between!" Join me in this brief analysis where we'll be taking information on 43189 goals over 100 years of football to piece together the cold, hard <del>facts</del> interpretations!</p>

<p>The dataset in question describes each goal with 8 variables:</p>
<pre>date        home_team	away_team   team        scorer              minute	own_goal    penalty
1916-07-02  Chile       Uruguay     Uruguay     José Piendibene     44.0	False       False
</pre>

<p><ul>
    <li>Importing and cleaning the data was done with <code>pandas</code> as usual.</li>
    <li>Plotting this time around will be taken care of by a new addition to the family — <code>plotly.express</code> (imported under the alias <code>px</code>). The plots are interactive!</li>
</ul></p>

<h2>Who are the top scorers?</h2>

<p>The code below aggregates (<code>.agg()</code>) the data by the <code>'scorer'</code> and counts how many goals each has scored:

<pre><code class="language-python">record_goalscorers = goals.groupby(
        by=['team','scorer']
    )[['scorer']].agg(
        {'scorer':'count'}
    ).rename(
        columns={'scorer': 'goals_scored'}
    ).reset_index().sort_values(by=['goals_scored'],ascending=False)</code></pre>

{% include /2024/01/26/international-goalscorer-analysis/highestgoalscorers.html %}

<p>It's interesting to see that Lionel Messi is \(5^{\text{th}}\) in this list. His goals in friendly matches push him up to \(3^{\text{rd}}\) place (with \(106\) goals) in the overall international tally as can be found <a href="https://en.wikipedia.org/wiki/List_of_top_international_men%27s_football_goal_scorers_by_country">here</a> (accurate as of the date of this blogpost).</p> <p>Similarly, Ali Daei from Iran shoots all the way up from \(7^{\text{th}}\) with \(49\) goals to a whopping \(108\) goals at \(2^{\text{nd}}\) place — friendly matches really skew the perception!</p>

<h2>Is there any general trend in the number of goals scored in competitive matches over time?</h2>

As usual, a <del>picture</del> plot is worth a thousand words:

<p>{% include /2024/01/26/international-goalscorer-analysis/goalsperyearscatter.html %}</p>

<p>Overall, there's been a general upturn in the number of goals. The upturn in goals is likely positively correlated with the expansion of international competitions, continental championships and qualifiers. There also seems to be some level of periodicity that could be explained by there being more games in the qualifying period roughly a year before each major competition.</p>

<ul>
    <li>The UEFA European Championships began in 1960 and occur every 4 years
        <ul>
            <li>Euro 2020 was postponed to 2021 (due to the COVID pandemic)</li>
        </ul>
    </li>
    <li> The FIFA World Cup began in 1930 and occurs every 4 years.</li>
    <li>The UEFA Nations League began in 2018 — the first final was in 2019 and it's a biennial competition.</li>
</ul>

<p>Instead of a scatter plot, a  bar-chart colour-coded by competitions may help to surface a pattern. To do so, I need to create another column that will serve as a reference point to colour-code the bars:</p>

<pre><code class="language-python">def which_tournament(year):
    if year == 2021:
        return 'Euro'
    if (2020 > year >= 1960) and ((year - 1960)%4 == 0):
        return 'Euro'
    elif (year >= 1930) and ((year - 1930)%4 == 0):
        return 'World Cup'
    return 'No Tournament'

goals_by_year['tournament'] = goals_by_year['year'].apply(which_tournament)</code></pre>

{% include /2024/01/26/international-goalscorer-analysis/goalsperyearcoloured.html %}

To go even more granular, a similar process can highlight every year that comes before, for example, a World Cup competition:

<p>{% include /2024/01/26/international-goalscorer-analysis/wcqualgoals.html %}</p>

<p style="text-align:right;"><span class="warning">This seems like something that could be investigated more rigorously with the theory of time series.</span></p><br>

<ul>
    <li>The uncharacteristically high number of goals in the WC Qualifying period of 2001 is interesting too.</li>
    <ul>
        <li>In the 2002 World Cup qualification round, Australia broke the record for the largest win in an international match with a 31–0 win over American Samoa.</li>
    </ul>
    <li>The FIFA Confederations Cup was also held every 2 years from 1992 to 2017 which also factors into the increase in qualifying period goals after the 1990 WC.</li>
</ul>

<p>Maybe goals aren't as hard to come by these days as previously thought!</p>