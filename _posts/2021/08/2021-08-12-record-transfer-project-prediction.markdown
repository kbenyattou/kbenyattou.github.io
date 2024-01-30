---
layout: post
title: "Record Football Transfers (Part II - Prediction)"
date: 2021-08-12 14:39:00 +0100
categories: ['regression', 'football', 'transfers']
permalink: /portfolio/2021/08/record-transfers-part-2/
# author: 'you'
---

<p>Continuing on from my <a href="/portfolio/2021/01/record-transfers-part-1/">last project</a>, I would like to apply some of the ML techniques I've discovered (by watching the last few lectures of <a href="http://ocw.mit.edu/6-0002F16">MIT 6.0002</a>) so that I can predict the average price of next year's high-profile transfers.</p>

<p>I'll be using the idea of polynomial linear regression to fit a curve to the data I've collected and make a prediction using said curve.</p>

<p>The lack of information on the Wikipedia table between 2002 and 2008 would certainly affect the results so I added the top transfers from each year using the same sources as the Wikipedia table (and of course cross-checking my sources). Then I worked out the average price of the (at least 5 or more) highest transfers for each year and plotted them in red:
<img src="/assets/portfolio/2021/08/record-transfers-part-2/transfersav.svg" class="center"></p>

<p>Using my dataset <code>fee_dataset</code>, I used the following code to perform and plot polynomial linear regression to find the line (polynomial of degree 1) that best fits the data.

<pre><code class="language-python">linear_model = pylab.polyfit(fee_dataset.year, fee_dataset.fee, 1)
linear_predictions = pylab.polyval(linear_model, fee_dataset.year)
pylab.plot(fee_dataset.year, linear_predictions, 'g--', linewidth=1, label="Linear")</code></pre>
</p>

<p>
    <ul>
        <li><p>The <code>.polyfit(x, y, n)</code> method of the <code>pyplot</code> class returns a list of coefficients corresponding to the polynomial of degree <code>n</code> that best approximates the data (independent variable, dependent variable) given by (<code>x</code>,<code>y</code>).</p></li>
        <li><p><code>.polyval</code> returns a set of predictions given the data.</p></li>
    </ul>
</p>

<img src="/assets/portfolio/2021/08/record-transfers-part-2/polynomialfit.svg" class="center">

<p>The quartic model shows the most significant downturn beginning around 2019. The cubic model that's been fit to the data shows more of a plateau. There is some circumstantial evidence that contextualises the data:</p>
        
<ul>
    <li><p>Despite the pandemic, FFP (Financial Fair Play) regulations have been relaxed in the most recent transfer window (2021) and the data that I've collected concerns the upper echelon of football spending. <del>Their spending power defies the current global climate.</del></p></li>
    <li><p>Neymar's transfer for £198M also greatly skews the results for 2017. This transfer was a singularity amongst singularities. Without his contribution, the average would've been £70.5M. <del>Thanks, PSG.</del></p></li>
</ul>

<p>The linear model fails to fit early (2000-2008) transfer data very well and doesn't account for the slowing down of spending in recent years. The quadratic model is slightly better than the linear model for early years but also fails to capture the current spending climate reasonably.</p>

<p>To quantify how well the models fit the data, I've chosen to calculate a quantity, \(R^{2}\), known as the coefficient of determination. It's defined as:</p>

<p>$$ R^{2} = 1 - \dfrac{ \sum_{i} (y_{i} - p_{i})^{2}}{\sum_{i} (y_{i} - \mu)^{2}} $$</p>

<ul>
    <li><p><code>y</code> \(= (y_{i})_{i \in I}\) is the sequence of dependent variable values (average fees in our case),</p></li>
    <li><p><code>p</code> \(= (p_{i})_{i \in I}\) is the sequence of predicted values from our model, and</p></li>
    <li><p>\(\mu\) is the mean of <code>y</code> calculated in the ordinary way: \(\mu = \frac{1}{\#I} \left( \sum_{i \in I} y_{i} \right)\) where \(\#I\) is the number of terms in <code>y</code>.</p></li>
</ul>
        
<p>The calculation for \(R^{2}\) is simple to implement:</p>

<pre><code>def coeff_determination(y,p):
    ymean = sum(y)/len(y)
    comp = sum([(yi - pi)**2 for yi,pi in zip(y,p)])/sum([(yi - ymean)**2 for yi in y])
    return 1 - comp</code></pre>

<p>The results are as follows:</p>

<pre>    Linear model:       0.7562801560992423
    Quadratic model:    0.8048401397741687
    Cubic model:        0.8355798702015268
    Quartic model:      0.8380772813567788</pre>

<p>The \(R^{2}\) values are all relatively high and the graphs don't seem to be over-fitting the data. They all reasonably capture the increasing average transfer fees as time progresses since the beginning of the century.</p>

<p>Taking into account all of the above, I'm inclined to side with the cubic model over the others.</p>

<p>To predict the value of the cubic model for the year 2022, I can define a <code>Polynomial</code> class that I'll feed the cubic model coefficients (found with <code>.polyfit(x,y,3)</code>) to:</p>

<pre><code>class Polynomial:
    def __init__(self, coeffs):
        self.coeffs = coeffs

    def evaluate(self, a):
        output = 0
        for i in range(len(self.coeffs)):
            output += self.coeffs[i]*(a**(len(self.coeffs) - 1 - i))
        return output

prediction = Polynomial(cubic_model).evaluate(22)</code></pre>

<p>Thus, the final prediction of this analysis is roughly <strong>£77.89M</strong>!</p>

<h2>Postgame Analysis</h2>

<p style="text-align: center" class="date">1st September 2022</p>

<p>The chosen polynomial linear regression model was quite close to the observed record fee. Antony was the record transfer in the 2022 summer transfer window for a fee of £82.2M which is £4.31M over the model's prediction of £77.89M!</p>

<p>Football inflation is truly incredible (and unsustainable...?)</p>