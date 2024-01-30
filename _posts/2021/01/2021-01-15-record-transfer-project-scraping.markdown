---
layout: post
title: "Record Football Transfers (Part I - Data Scraping)"
date: 2021-01-15 11:12:00 +0000
categories: ['scraping', 'data analysis']
permalink: /portfolio/2021/01/record-transfers-part-1/
# author: 'you'
# toc: true
---

<!-- {% toc %} -->

<p>The beautiful game has undergone a drastic shift. Nations now own clubs. Players are being exchanged for hundreds of millions of currency... and the Premier League won't let Newcastle join in on the fun.</p>

<p>I thought it would be interesting to graph the highest transfer fees over the past 20 years.</p>

<h2>Importing the Data</h2>

<p>First of all, I chose to scrape a table off Wikipedia that contained the most expensive football transfers. I did this by using several modules:</p>

<ul>
  <li><p>I began by using the requests module to make a HTTP requests and retrieve the HTML from the url of a Wikipedia site stored in <code>wiki_url</code>. The module isn't designed to parse the information.
  <pre><code>data = requests.get(wiki_url)</code></pre>
  </p></li>
  
  <li>
    <p>To parse the HTML, I passed it into <code>bs4</code>'s <code>BeautifulSoup</code> constructor to return a tree-like object that we can interact with.</p>
    <p style="text-align:right; "><span class="warning">This part requires the <code>lxml</code> parser to be installed. </span></p>
    <p><pre><code>soup = BeautifulSoup(data.text, 'lxml')</code></pre></p>
    <p>This <code>BeautifulSoup</code> object supports methods like <code>.find()</code> and <code>.find_all()</code> to search the nested tag structure of a HTML document.</p>
  </li>
</ul>

<h2>Cleaning the Data</h2>

<p>Data scraped from the internet is usually not in a presentable format. My end goal is to produce a table that's easy to read (and access information from). The first course of action was to deal with row elements that span multiple cells. Next up was to remove extra fluff from the cells. Wikipedia, for example, has loads of citations like[1] this. I defined a regular expression to get rid of them.
<pre><code>def remove_citation(n):
    return re.sub(r'[\[].*?[\]]', '', n)</code></pre>
There was a fair bit of uninteresting code to fix up the table that I'll leave out. It was all essentially looking at the tag structure of the table and making changes where necessary.</p>

<p>Now I have a list of lists <code>[row1, ..., rown]</code>.</p>

<ul>
  <li><p>For an easy to way to build and sort a table, I used the <code>pandas</code> module. I converted my list of lists to what is called a <code>DataFrame</code> object in pandas. According to Google, a <code>DataFrame</code> is a 2-dimensional labelled data structure with columns of potentially different types.</p>

<pre><code>df = pd.DataFrame(listofrows)

df.columns = ['name','from','to','position','feeeuro','fee','year','born']
df.year = df.year.astype(int)
df.fee = df.fee.astype(float)

df = df.sort_values(by=['year','fee'])</code></pre>

<p>The <code>columns</code> attribute labels the columns and the two lines that follow change the type of the column entries to <code>int</code>egers and <code>float</code>s respectively. A very useful method of the <code>DataFrame</code> structure is the ability to sort the table by columns. I first sorted by the <code>'year'</code> of the transfer (which is my independent variable) and the transfer <code>'fee'</code> in GBP (my dependent variable).</p>
  </li>
</ul>

<p>Without posting the entirety of the table, it came out looking something like this:</p>

<pre>
<!-- <code> -->
                   name                from            to     position  feeeuro     fee  year  born
47            Luís Figo           Barcelona   Real Madrid   Midfielder       62   37.00     0  1972
24      Zinedine Zidane            Juventus   Real Madrid   Midfielder       76   46.60     1  1972
34   Zlatan Ibrahimović         Inter Milan     Barcelona      Forward     69.5   56.00     9  1981
37                 Kaká               Milan   Real Madrid   Midfielder       67   56.00     9  1982
12    Cristiano Ronaldo   Manchester United   Real Madrid      Forward       94   80.00     9  1985
<!-- </code> -->
</pre>

<h2>Visualising the Data</h2>

<p>Plotting the information was a simple case of using <code>pyplot</code> from the <code>matplotlib</code> module.</p>

<pre><code>pyplot.title('The Highest Football Transfers')
pyplot.xlabel('Year')
pyplot.ylabel(r'''Transfer Fee (£M)''')

pyplot.plot(df.year, df.fee, 'o')
pyplot.show()</code></pre>

<img src="/assets/portfolio/2021/01/record-transfers-part-1/wikitransfers.svg" class="center">

<p>Maybe I'll try some machine learning stuff to predict what the average high-profile 2022 signing will look like given the information I have.</p>

<p><strong>Edit:</strong> I did end up doing this. Follow <a href="/portfolio/2021/08/record-transfers-part-2/">this link</a> to find the follow-up!</p>