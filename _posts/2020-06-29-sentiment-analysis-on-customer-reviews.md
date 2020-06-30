---
title: "Sentiment Analysis on Customer Reviews"

author: "Aravind Karuturi"
date: "June 29, 2020"
layout: post
---


<section class="main-content">
<p>Businesses, nowadays are becoming more customer centric and customers love being heard. They are more than willing to share their opinions and experience.</p>
<p>Sentiment analysis paves the way to make this happen.If used properly, it can reveal gold mines inside the thoughts and opinions of your customers.</p>
<div id="sentiment-analysis" class="section level2">
<h2>Sentiment Analysis</h2>
<p>Sentiment Analysis is a process of extracting opinions that have different polarities (positive, negative or neutral). We identify the tone in which the customer speak about your product. So by able to track before hand the sentiment across the customers, that company can be well equipped to tackle what might coming their way.</p>
<p><em>Here, I would like to perform sentiment analysis on analyzing the customer reviews of a product launched by the company <strong>XYZ</strong>, a smart health tracker manufacturer.</em></p>
<p>When we have a few reviews, one can go through them directly. <strong>What if we have hundreds of reviews?</strong></p>
<p><strong>Sentiment Analysis</strong> allows us to analyze them automatically.</p>
<div id="web-scraping" class="section level3">
<h3>Web Scraping</h3>
<p>Web Scraping is used to extract information from websites.So here we collected reviews of the product written on the internet.</p>
<p>To do that, install <strong>package: rvest</strong> and provide the <em>URL</em> of the webpage from which the data is collected. Also use <strong>CSS Selector</strong> to select the parts of data that needs to be scraped. <br> <em>Note: CSS Selector extension can be added to the chrome</em> <br> <a href="https://selectorgadget.com/">click here for link</a></p>
<p>install.packages(“httr”) <br/> install.packages(“xml2”) <br/> install.packages(“rvest”) <br/> .huge[</p>
<div class="sourceCode" id="cb1"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb1-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb1-2" data-line-number="2"><span class="kw">library</span>(httr)</a>
<a class="sourceLine" id="cb1-3" data-line-number="3"><span class="kw">library</span>(xml2)</a>
<a class="sourceLine" id="cb1-4" data-line-number="4"><span class="kw">library</span>(rvest)</a></code></pre></div>
<p>Provide the css or xpath to parts of data selected through CSS Selector to <em>html_nodes()</em> function</p>
<p>url1 &lt;- <em>‘paste the url of the webpage’</em> <br/> <em>Note: url is masked</em></p>
<div class="sourceCode" id="cb2"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb2-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb2-2" data-line-number="2"><span class="co"># Assign url of webpage to url1</span></a>
<a class="sourceLine" id="cb2-3" data-line-number="3">webpage1 &lt;-<span class="st"> </span><span class="kw">read_html</span>(url1)</a>
<a class="sourceLine" id="cb2-4" data-line-number="4">data1 &lt;-<span class="st"> </span><span class="kw">html_nodes</span>(webpage1,<span class="st">&#39;.review-text-content span&#39;</span>)</a>
<a class="sourceLine" id="cb2-5" data-line-number="5">review_data1 &lt;-<span class="st"> </span><span class="kw">html_text</span>(data1)</a></code></pre></div>
<p>Similarly scrape data from other webpages. To minimize the complexity, I took only 2 webpages <br/></p>
<p>url2 &lt;- <em>‘paste the url of the webpage’</em> <br/> <em>Note: url is masked</em></p>
<div class="sourceCode" id="cb3"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb3-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb3-2" data-line-number="2"><span class="co"># Assign url of webpage to url2</span></a>
<a class="sourceLine" id="cb3-3" data-line-number="3">webpage2 &lt;-<span class="st"> </span><span class="kw">read_html</span>(url2)</a>
<a class="sourceLine" id="cb3-4" data-line-number="4">data2 &lt;-<span class="st"> </span><span class="kw">html_nodes</span>(webpage2,<span class="st">&#39;.review-text-content span&#39;</span>)</a>
<a class="sourceLine" id="cb3-5" data-line-number="5">review_data2 &lt;-<span class="st"> </span><span class="kw">html_text</span>(data2)</a></code></pre></div>
<p>The reviews are stored as character arrays</p>
<div class="sourceCode" id="cb4"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb4-1" data-line-number="1"><span class="kw">head</span>(review_data2,<span class="dv">2</span>)</a></code></pre></div>
<pre><code>## [1] &quot;Touch is slow and not that sensitive \U0001f623, Goqii basic black N white band touch is much better than this! You can&#39;t replace any medical equipment with this watch, not that trustworthy! But gives you result (temp, bp, hr) , that&#39;s not too far from accuracy!But in this price I guess these all with high performance can&#39;t be expected at this time!&quot;
## [2] &quot;The product and service team is really very good. Some connectivity issues we were facing, service team replace it within 2 days. Thanks for support. Body temprature is accurate . It Records the data of heart beat, temperature and steps also so that we can trace our health status. Thanks GOQII team&quot;</code></pre>
<div class="sourceCode" id="cb6"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb6-1" data-line-number="1"><span class="kw">class</span>(review_data2)</a></code></pre></div>
<pre><code>## [1] &quot;character&quot;</code></pre>
<p>Once the required data is scraped, merge all data to a single character vector</p>
<div class="sourceCode" id="cb8"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb8-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb8-2" data-line-number="2"><span class="co"># Merge the character arrays </span></a>
<a class="sourceLine" id="cb8-3" data-line-number="3">reviews &lt;-<span class="st"> </span><span class="kw">c</span>(review_data1,review_data2)</a></code></pre></div>
</div>
<div id="data-cleaning" class="section level3">
<h3>Data Cleaning</h3>
<p>As a part of it, remove numbers, punctuation and other non-ASCII characters which are not needed for sentiment analysis</p>
<p><strong>Install packages: NLP and tm</strong> <br/> install.packages(“NLP”) <br/> install.packages(“tm”)</p>
<div class="sourceCode" id="cb9"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb9-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb9-2" data-line-number="2"><span class="kw">library</span>(NLP)</a>
<a class="sourceLine" id="cb9-3" data-line-number="3"><span class="kw">library</span>(tm)</a>
<a class="sourceLine" id="cb9-4" data-line-number="4">reviews_clean &lt;-<span class="st"> </span><span class="kw">removeNumbers</span>(reviews)</a>
<a class="sourceLine" id="cb9-5" data-line-number="5">reviews_clean &lt;-<span class="st"> </span><span class="kw">removePunctuation</span>(reviews_clean)</a>
<a class="sourceLine" id="cb9-6" data-line-number="6"><span class="co"># to remove all non-ASCII characters</span></a>
<a class="sourceLine" id="cb9-7" data-line-number="7">reviews_clean &lt;-<span class="kw">gsub</span>(<span class="st">&quot;[^</span><span class="ch">\x01</span><span class="st">-</span><span class="ch">\x7F</span><span class="st">]&quot;</span>, <span class="st">&quot;&quot;</span>,reviews_clean)</a>
<a class="sourceLine" id="cb9-8" data-line-number="8"><span class="kw">head</span>(reviews_clean,<span class="dv">2</span>)</a></code></pre></div>
<pre><code>## [1] &quot;This is my second Goqii watch purchase Good upgrade over the first edition Always love Goqii products for their quality simplicity and reliability Wouldve loved to have more customisation for the display as didnt like the big temperature logo flashing on the home screen Hope to see an update sometime soon&quot;                                       
## [2] &quot;A compact and light weight tracker with a plethora of features blood pressure heart rate temperature have bought it for my father tomonitor his health while he traveles and works out in these testing times The touch takes time to get used too but is good to prevent accidental swipes  The app is great too  one can earn points to shop on the app&quot;</code></pre>
</div>
<div id="text-tidying" class="section level3">
<h3>Text Tidying</h3>
<p>The fundamental requirement of text mining is to get your text in a tidy format. To do that first convert the text into data frame or tibble. Then unnest the text into single words with the help of <em>unnest_tokens ()</em> function.</p>
<p><strong>Install packages: dplyr , tibble and tidytext</strong> <br/> install.packages(“dplyr”) <br/> install.packages(“tibble”) <br> install.packages(“tidytext”)</p>
<div class="sourceCode" id="cb11"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb11-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb11-2" data-line-number="2"><span class="kw">library</span>(dplyr) <span class="co"># for data manipulation</span></a>
<a class="sourceLine" id="cb11-3" data-line-number="3"><span class="kw">library</span>(tibble) <span class="co"># for simple data frames</span></a>
<a class="sourceLine" id="cb11-4" data-line-number="4"><span class="kw">library</span>(tidytext) <span class="co"># for text mining</span></a>
<a class="sourceLine" id="cb11-5" data-line-number="5"></a>
<a class="sourceLine" id="cb11-6" data-line-number="6">reviews_tidy &lt;-<span class="st"> </span><span class="kw">tibble</span>(<span class="dt">linenumber =</span> <span class="dv">1</span><span class="op">:</span><span class="dv">20</span>,<span class="dt">text =</span> reviews_clean)</a>
<a class="sourceLine" id="cb11-7" data-line-number="7">reviews_tidy &lt;-<span class="st"> </span>reviews_tidy <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">unnest_tokens</span>(word,text)</a>
<a class="sourceLine" id="cb11-8" data-line-number="8"><span class="kw">head</span>(reviews_tidy,<span class="dv">6</span>)</a></code></pre></div>
<pre><code>## # A tibble: 6 x 2
##   linenumber word  
##        &lt;int&gt; &lt;chr&gt; 
## 1          1 this  
## 2          1 is    
## 3          1 my    
## 4          1 second
## 5          1 goqii 
## 6          1 watch</code></pre>
</div>
<div id="word-frequency" class="section level3">
<h3>Word frequency</h3>
<p>Perform word frequency analysis to identify the most common words used in the reviews. For that use <em>count()</em> function to assess the words which are of high sensitivity</p>
<div class="sourceCode" id="cb13"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb13-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb13-2" data-line-number="2">reviews_tidy <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">count</span>(word,<span class="dt">sort =</span> <span class="ot">TRUE</span>)</a></code></pre></div>
<pre><code>## # A tibble: 571 x 2
##    word      n
##    &lt;chr&gt; &lt;int&gt;
##  1 and      38
##  2 the      35
##  3 to       34
##  4 is       29
##  5 for      22
##  6 this     21
##  7 a        20
##  8 i        20
##  9 of       20
## 10 good     17
## # … with 561 more rows</code></pre>
<p>As you see, a lot of common words are not informative (i.e. and,the,to,is,…). These words are called stop words. So, we can remove the stop words from the tibble with the help of <em>anti_join()</em> function and <em>stop_words</em> data set from tm package.</p>
<div class="sourceCode" id="cb15"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb15-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb15-2" data-line-number="2">common_words &lt;-<span class="st"> </span>reviews_tidy <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">anti_join</span>(stop_words) <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">count</span>(word,<span class="dt">sort =</span> <span class="ot">TRUE</span>)</a>
<a class="sourceLine" id="cb15-3" data-line-number="3"><span class="kw">head</span>(common_words)</a></code></pre></div>
<pre><code>## # A tibble: 6 x 2
##   word            n
##   &lt;chr&gt;       &lt;int&gt;
## 1 product        16
## 2 goqii          13
## 3 band           12
## 4 temperature    11
## 5 heart           9
## 6 rate            8</code></pre>
<p>To represent them in the form of word cloud,<br />
<strong>Install packages: RcolorBrewer and wordcloud</strong> <br> install.packages(“RcolorBrewer”) <br> install.packages(“wordcloud”)</p>
<div class="sourceCode" id="cb17"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb17-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb17-2" data-line-number="2"><span class="kw">library</span>(RColorBrewer)</a>
<a class="sourceLine" id="cb17-3" data-line-number="3"><span class="kw">library</span>(wordcloud) <span class="co"># to plot a word cloud</span></a>
<a class="sourceLine" id="cb17-4" data-line-number="4">common_words <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">with</span>(<span class="kw">wordcloud</span>(word,n,<span class="dt">max.words =</span> <span class="dv">100</span>))</a></code></pre></div>
<p><img src="{{ site.url }}{{ site.baseurl }}/knitr_files/Sentiment_analysis_customer_reviews_files/figure-html/unnamed-chunk-11-1.png" /><!-- --></p>
</div>
<div id="identifying-the-sentiment-of-the-reviews" class="section level3">
<h3>Identifying the sentiment of the reviews</h3>
<p>Once we convert the data to tidy format, we can use one of the <em>sentiment lexicons</em> from the <em>tidy text package</em> (i.e. bing,afinn,nrc) Get the lexicon using <em>get_sentiments()</em> function</p>
<div class="sourceCode" id="cb18"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb18-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb18-2" data-line-number="2"><span class="kw">library</span>(tidytext)</a>
<a class="sourceLine" id="cb18-3" data-line-number="3">bing &lt;-<span class="st"> </span><span class="kw">get_sentiments</span>(<span class="st">&quot;bing&quot;</span>)</a>
<a class="sourceLine" id="cb18-4" data-line-number="4">afinn &lt;-<span class="st"> </span><span class="kw">get_sentiments</span>(<span class="st">&quot;afinn&quot;</span>)</a>
<a class="sourceLine" id="cb18-5" data-line-number="5">nrc &lt;-<span class="st"> </span><span class="kw">get_sentiments</span>(<span class="st">&quot;nrc&quot;</span>)</a></code></pre></div>
<p>Choose the sentiment lexicon according to your requirement</p>
<div class="sourceCode" id="cb19"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb19-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb19-2" data-line-number="2"><span class="kw">head</span>(bing,<span class="dv">3</span>)</a></code></pre></div>
<pre><code>## # A tibble: 3 x 2
##   word     sentiment
##   &lt;chr&gt;    &lt;chr&gt;    
## 1 2-faces  negative 
## 2 abnormal negative 
## 3 abolish  negative</code></pre>
<div class="sourceCode" id="cb21"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb21-1" data-line-number="1"><span class="kw">head</span>(afinn,<span class="dv">3</span>)</a></code></pre></div>
<pre><code>## # A tibble: 3 x 2
##   word      value
##   &lt;chr&gt;     &lt;dbl&gt;
## 1 abandon      -2
## 2 abandoned    -2
## 3 abandons     -2</code></pre>
<div class="sourceCode" id="cb23"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb23-1" data-line-number="1"><span class="kw">head</span>(nrc,<span class="dv">3</span>)</a></code></pre></div>
<pre><code>## # A tibble: 3 x 2
##   word    sentiment
##   &lt;chr&gt;   &lt;chr&gt;    
## 1 abacus  trust    
## 2 abandon fear     
## 3 abandon negative</code></pre>
<p>In general people tend use negation in reviews (e.g. ‘not good’ in place to ‘bad’). So to consider affect of negation while finding sentiments of the review, we opt <strong>afinn</strong> lexicon as it contain words like not good, not working etc.</p>
<p><strong>First, we will identify the sentiment score of words used in the reviews</strong> <br> This can be done by using <em>inner_join()</em> function and <em>count()</em> function.</p>
<div class="sourceCode" id="cb25"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb25-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb25-2" data-line-number="2">word_count_afinn &lt;-<span class="st"> </span>reviews_tidy <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">inner_join</span>(afinn) <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">count</span>(word,value,<span class="dt">sort =</span> <span class="ot">TRUE</span>)</a>
<a class="sourceLine" id="cb25-3" data-line-number="3"><span class="kw">head</span>(word_count_afinn)</a></code></pre></div>
<pre><code>## # A tibble: 6 x 3
##   word    value     n
##   &lt;chr&gt;   &lt;dbl&gt; &lt;int&gt;
## 1 good        3    17
## 2 care        2     4
## 3 like        2     4
## 4 awesome     4     3
## 5 better      2     3
## 6 nice        3     3</code></pre>
<p><strong>Visualisation of sentiment score</strong></p>
<p><strong>Install packages: ggplot2</strong> <br> install.packages(“ggplot2”)</p>
<div class="sourceCode" id="cb27"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb27-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb27-2" data-line-number="2"><span class="kw">library</span>(ggplot2) <span class="co"># for data visualisation</span></a>
<a class="sourceLine" id="cb27-3" data-line-number="3">word_count_afinn  <span class="op">%&gt;%</span><span class="st">  </span><span class="kw">mutate</span>(<span class="dt">n =</span> <span class="kw">ifelse</span>(value <span class="op">&lt;</span><span class="st"> </span><span class="dv">0</span>, <span class="op">-</span>n,n))  <span class="op">%&gt;%</span><span class="st"> </span></a>
<a class="sourceLine" id="cb27-4" data-line-number="4"><span class="st">        </span><span class="kw">mutate</span>(<span class="dt">sentiment =</span> <span class="kw">ifelse</span>(value <span class="op">&lt;</span><span class="dv">0</span>,<span class="st">&quot;negative&quot;</span>,<span class="st">&quot;positive&quot;</span>))  <span class="op">%&gt;%</span><span class="st"> </span></a>
<a class="sourceLine" id="cb27-5" data-line-number="5"><span class="st">        </span><span class="kw">mutate</span>(<span class="dt">word =</span> <span class="kw">reorder</span>(word,n))  <span class="op">%&gt;%</span><span class="st">  </span></a>
<a class="sourceLine" id="cb27-6" data-line-number="6"><span class="st">        </span><span class="kw">ggplot</span>(<span class="dt">mapping =</span> <span class="kw">aes</span>(word,n,<span class="dt">fill=</span>sentiment)) <span class="op">+</span><span class="st"> </span><span class="kw">geom_col</span>() <span class="op">+</span><span class="st"> </span><span class="kw">coord_flip</span>() <span class="op">+</span><span class="st"> </span><span class="kw">labs</span>(<span class="dt">y =</span> <span class="st">&quot;sentiment score&quot;</span>)</a></code></pre></div>
<p><img src="{{ site.url }}{{ site.baseurl }}/knitr_files/Sentiment_analysis_customer_reviews_files/figure-html/unnamed-chunk-15-1.png" style="display: block; margin: auto;" /></p>
<p><strong>Now, identify the sentiment of each review</strong> <br> By using <em>inner_join()</em> and <em>summarise()</em> function, we can calculate the total sentiment of the review.</p>
<div class="sourceCode" id="cb28"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb28-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb28-2" data-line-number="2">review_sentiment_afinn &lt;-<span class="st"> </span>reviews_tidy <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">inner_join</span>(afinn) <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">group_by</span>(<span class="dt">Review =</span> linenumber) <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">summarise</span>(<span class="dt">value =</span> <span class="kw">sum</span>(value))</a></code></pre></div>
<p><strong>Visualisation of sentiment of the reviews</strong> With the help of <em>mutate()</em> function and <em>sentiment value</em> of each review, identify whether it is a positive or negative review.</p>
<div class="sourceCode" id="cb29"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb29-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb29-2" data-line-number="2">review_sentiment_afinn <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">mutate</span>(<span class="dt">sentiment =</span> <span class="kw">ifelse</span>(value <span class="op">&lt;</span><span class="dv">0</span>,<span class="st">&quot;negative&quot;</span>,<span class="st">&quot;positive&quot;</span>)) <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">ggplot</span>(<span class="kw">aes</span>(Review,value,<span class="dt">fill=</span>sentiment)) <span class="op">+</span><span class="st"> </span><span class="kw">geom_col</span>() <span class="op">+</span><span class="st"> </span><span class="kw">labs</span>(<span class="dt">title =</span> <span class="st">&quot;Sentiment of Reviews&quot;</span>)</a></code></pre></div>
<p><img src="{{ site.url }}{{ site.baseurl }}/knitr_files/Sentiment_analysis_customer_reviews_files/figure-html/unnamed-chunk-17-1.png" style="display: block; margin: auto;" /></p>
</div>
</div>
<div id="inference" class="section level2">
<h2>Inference <br></h2>
<ul>
<li><strong>From the plot, we observe that the customer reviews received for the product of XYZ are mostly positive (&gt; 80%). This indicates that the most of the customers are satisfied with the product.</strong></li>
</ul>
<p>All represent positive and negative words as word cloud: <br> <strong>Install packages: RcolorBrewer, wordcloud and reshape2</strong> <br> install.packages(“reshape2”)</p>
<div class="sourceCode" id="cb30"><pre class="sourceCode r"><code class="sourceCode r"><a class="sourceLine" id="cb30-1" data-line-number="1"><span class="co"># Code</span></a>
<a class="sourceLine" id="cb30-2" data-line-number="2"><span class="kw">library</span>(reshape2)</a>
<a class="sourceLine" id="cb30-3" data-line-number="3">reviews_tidy <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">inner_join</span>(afinn) <span class="op">%&gt;%</span><span class="st"> </span><span class="kw">count</span>(word,value,<span class="dt">sort =</span> <span class="ot">TRUE</span>) <span class="op">%&gt;%</span><span class="st"> </span></a>
<a class="sourceLine" id="cb30-4" data-line-number="4"><span class="st">         </span><span class="kw">mutate</span>(<span class="dt">sentiment =</span> <span class="kw">ifelse</span>(value <span class="op">&lt;</span><span class="dv">0</span>,<span class="st">&quot;negative&quot;</span>,<span class="st">&quot;positive&quot;</span>)) <span class="op">%&gt;%</span></a>
<a class="sourceLine" id="cb30-5" data-line-number="5"><span class="st">         </span><span class="kw">acast</span>(word<span class="op">~</span>sentiment,<span class="dt">value.var =</span> <span class="st">&quot;n&quot;</span>,<span class="dt">fill =</span> <span class="dv">0</span>) <span class="op">%&gt;%</span></a>
<a class="sourceLine" id="cb30-6" data-line-number="6"><span class="st">         </span><span class="kw">comparison.cloud</span>(<span class="dt">colors =</span> <span class="kw">c</span>(<span class="st">&quot;red&quot;</span>,<span class="st">&quot;green&quot;</span>),<span class="dt">max.words =</span> <span class="dv">100</span>)</a></code></pre></div>
<p><img src="{{ site.url }}{{ site.baseurl }}/knitr_files/Sentiment_analysis_customer_reviews_files/figure-html/unnamed-chunk-18-1.png" style="display: block; margin: auto;" /></p>
<ul>
<li><p><strong>Negative words gives us an understanding of things that the company should carefully analyse to improve the product.</strong></p></li>
<li><p><strong>By increasing the number of reviews considered for the sentiment analysis, we can increase the accuracy of the model.</strong></p></li>
</ul>
</div>
</section>
