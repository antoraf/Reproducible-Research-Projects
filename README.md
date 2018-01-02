# Reproducible-Research-Projects
<p>Antonio Quintieri<br>
January, 2018<br>
###About
This was the first project for the <strong>Reproducible Research</strong> course in Coursera's Data Science specialization track. The purpose of the project was to answer a series of questions using data collected from a <a href="http://en.wikipedia.org/wiki/Fitbit" rel="nofollow">FitBit</a>.</p>
<p>##Synopsis
The purpose of this project was to practice:</p>
<ul>
<li>loading and preprocessing data</li>
<li>imputing missing values</li>
<li>interpreting data to answer research questions</li>
</ul>
<h2><a href="#data" aria-hidden="true" class="anchor" id="user-content-data"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Data</h2>
<p>The data for this assignment was downloaded from the course web
site:</p>
<ul>
<li>Dataset: <a href="https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip" rel="nofollow">Activity monitoring data</a> [52K]</li>
</ul>
<p>The variables included in this dataset are:</p>
<ul>
<li>
<p><strong>steps</strong>: Number of steps taking in a 5-minute interval (missing
values are coded as <code>NA</code>)</p>
</li>
<li>
<p><strong>date</strong>: The date on which the measurement was taken in YYYY-MM-DD
format</p>
</li>
<li>
<p><strong>interval</strong>: Identifier for the 5-minute interval in which
measurement was taken</p>
</li>
</ul>
<p>The dataset is stored in a comma-separated-value (CSV) file and there are a total of 17,568 observations in this dataset.</p>
<h2><a href="#loading-and-preprocessing-the-data" aria-hidden="true" class="anchor" id="user-content-loading-and-preprocessing-the-data"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Loading and preprocessing the data</h2>
<p>Download, unzip and load data into data frame <code>data</code>.</p>
<div class="highlight highlight-source-r"><pre><span class="pl-k">if</span>(<span class="pl-k">!</span>file.exists(<span class="pl-s"><span class="pl-pds">"</span>getdata-projectfiles-UCI HAR Dataset.zip<span class="pl-pds">"</span></span>)) {
        <span class="pl-smi">temp</span> <span class="pl-k">&lt;-</span> tempfile()
        download.file(<span class="pl-s"><span class="pl-pds">"</span>http://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip<span class="pl-pds">"</span></span>,<span class="pl-smi">temp</span>)
        unzip(<span class="pl-smi">temp</span>)
        unlink(<span class="pl-smi">temp</span>)
}

<span class="pl-smi">data</span> <span class="pl-k">&lt;-</span> read.csv(<span class="pl-s"><span class="pl-pds">"</span>activity.csv<span class="pl-pds">"</span></span>)</pre></div>
<h2><a href="#what-is-mean-total-number-of-steps-taken-per-day" aria-hidden="true" class="anchor" id="user-content-what-is-mean-total-number-of-steps-taken-per-day"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>What is mean total number of steps taken per day?</h2>
<p>Sum steps by day, create Histogram, and calculate mean and median.</p>
<div class="highlight highlight-source-r"><pre><span class="pl-smi">steps_by_day</span> <span class="pl-k">&lt;-</span> aggregate(<span class="pl-smi">steps</span> <span class="pl-k">~</span> <span class="pl-smi">date</span>, <span class="pl-smi">data</span>, <span class="pl-smi">sum</span>)
hist(<span class="pl-smi">steps_by_day</span><span class="pl-k">$</span><span class="pl-smi">steps</span>, <span class="pl-v">main</span> <span class="pl-k">=</span> paste(<span class="pl-s"><span class="pl-pds">"</span>Total Steps Each Day<span class="pl-pds">"</span></span>), <span class="pl-v">col</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>blue<span class="pl-pds">"</span></span>, <span class="pl-v">xlab</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>Number of Steps<span class="pl-pds">"</span></span>)</pre></div>
<p><a href="https://github.com/antoraf/Reproducible-Research-Projects/blob/Pictures/Picture1.png" target="_blank"><img src="https://github.com/antoraf/Reproducible-Research-Projects/blob/Pictures/Picture1.png" alt="plot of chunk unnamed-chunk-2" style="max-width:100%;"></a></p>
<div class="highlight highlight-source-r"><pre><span class="pl-smi">rmean</span> <span class="pl-k">&lt;-</span> mean(<span class="pl-smi">steps_by_day</span><span class="pl-k">$</span><span class="pl-smi">steps</span>)
<span class="pl-smi">rmedian</span> <span class="pl-k">&lt;-</span> median(<span class="pl-smi">steps_by_day</span><span class="pl-k">$</span><span class="pl-smi">steps</span>)</pre></div>
<p>The <code>mean</code> is 1.0766 × 10<sup>4</sup> and the <code>median</code> is 10765.</p>
<h2><a href="#what-is-the-average-daily-activity-pattern" aria-hidden="true" class="anchor" id="user-content-what-is-the-average-daily-activity-pattern"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>What is the average daily activity pattern?</h2>
<ul>
<li>Calculate average steps for each interval for all days.</li>
<li>Plot the Average Number Steps per Day by Interval.</li>
<li>Find interval with most average steps.</li>
</ul>
<div class="highlight highlight-source-r"><pre><span class="pl-smi">steps_by_interval</span> <span class="pl-k">&lt;-</span> aggregate(<span class="pl-smi">steps</span> <span class="pl-k">~</span> <span class="pl-smi">interval</span>, <span class="pl-smi">data</span>, <span class="pl-smi">mean</span>)

plot(<span class="pl-smi">steps_by_interval</span><span class="pl-k">$</span><span class="pl-smi">interval</span>,<span class="pl-smi">steps_by_interval</span><span class="pl-k">$</span><span class="pl-smi">steps</span>, <span class="pl-v">type</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>l<span class="pl-pds">"</span></span>, <span class="pl-v">xlab</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>Interval<span class="pl-pds">"</span></span>, <span class="pl-v">ylab</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>Number of Steps<span class="pl-pds">"</span></span>,<span class="pl-v">main</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>Average Number of Steps per Day by Interval<span class="pl-pds">"</span></span>)</pre></div>
<p><a href="https://github.com/antoraf/Reproducible-Research-Projects/blob/Pictures/Picture02.png" target="_blank"><img src="https://github.com/antoraf/Reproducible-Research-Projects/blob/Pictures/Picture02.png" alt="plot of chunk unnamed-chunk-3" style="max-width:100%;"></a></p>
<div class="highlight highlight-source-r"><pre><span class="pl-smi">max_interval</span> <span class="pl-k">&lt;-</span> <span class="pl-smi">steps_by_interval</span>[which.max(<span class="pl-smi">steps_by_interval</span><span class="pl-k">$</span><span class="pl-smi">steps</span>),<span class="pl-c1">1</span>]</pre></div>
<p>The 5-minute interval, on average across all the days in the data set, containing the maximum number of steps is 835.</p>
<h2><a href="#impute-missing-values-compare-imputed-to-non-imputed-data" aria-hidden="true" class="anchor" id="user-content-impute-missing-values-compare-imputed-to-non-imputed-data"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Impute missing values. Compare imputed to non-imputed data.</h2>
<p>Missing data needed to be imputed. Only a simple imputation approach was required for this assignment.
Missing values were imputed by inserting the average for each interval. Thus, if interval 10 was missing on 10-02-2012, the average for that interval for all days (0.1320755), replaced the NA.</p>
<div class="highlight highlight-source-r"><pre><span class="pl-smi">incomplete</span> <span class="pl-k">&lt;-</span> sum(<span class="pl-k">!</span>complete.cases(<span class="pl-smi">data</span>))
<span class="pl-smi">imputed_data</span> <span class="pl-k">&lt;-</span> transform(<span class="pl-smi">data</span>, <span class="pl-v">steps</span> <span class="pl-k">=</span> ifelse(is.na(<span class="pl-smi">data</span><span class="pl-k">$</span><span class="pl-smi">steps</span>), <span class="pl-smi">steps_by_interval</span><span class="pl-k">$</span><span class="pl-smi">steps</span>[match(<span class="pl-smi">data</span><span class="pl-k">$</span><span class="pl-smi">interval</span>, <span class="pl-smi">steps_by_interval</span><span class="pl-k">$</span><span class="pl-smi">interval</span>)], <span class="pl-smi">data</span><span class="pl-k">$</span><span class="pl-smi">steps</span>))</pre></div>
<p>Zeroes were imputed for 10-01-2012 because it was the first day and would have been over 9,000 steps higher than the following day, which had only 126 steps. NAs then were assumed to be zeros to fit the rising trend of the data.</p>
<div class="highlight highlight-source-r"><pre><span class="pl-smi">imputed_data</span>[as.character(<span class="pl-smi">imputed_data</span><span class="pl-k">$</span><span class="pl-smi">date</span>) <span class="pl-k">==</span> <span class="pl-s"><span class="pl-pds">"</span>2012-10-01<span class="pl-pds">"</span></span>, <span class="pl-c1">1</span>] <span class="pl-k">&lt;-</span> <span class="pl-c1">0</span></pre></div>
<p>Recount total steps by day and create Histogram.</p>
<div class="highlight highlight-source-r"><pre><span class="pl-smi">steps_by_day_i</span> <span class="pl-k">&lt;-</span> aggregate(<span class="pl-smi">steps</span> <span class="pl-k">~</span> <span class="pl-smi">date</span>, <span class="pl-smi">imputed_data</span>, <span class="pl-smi">sum</span>)
hist(<span class="pl-smi">steps_by_day_i</span><span class="pl-k">$</span><span class="pl-smi">steps</span>, <span class="pl-v">main</span> <span class="pl-k">=</span> paste(<span class="pl-s"><span class="pl-pds">"</span>Total Steps Each Day<span class="pl-pds">"</span></span>), <span class="pl-v">col</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>blue<span class="pl-pds">"</span></span>, <span class="pl-v">xlab</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>Number of Steps<span class="pl-pds">"</span></span>)

<span class="pl-c"><span class="pl-c">#</span>Create Histogram to show difference. </span>
hist(<span class="pl-smi">steps_by_day</span><span class="pl-k">$</span><span class="pl-smi">steps</span>, <span class="pl-v">main</span> <span class="pl-k">=</span> paste(<span class="pl-s"><span class="pl-pds">"</span>Total Steps Each Day<span class="pl-pds">"</span></span>), <span class="pl-v">col</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>red<span class="pl-pds">"</span></span>, <span class="pl-v">xlab</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>Number of Steps<span class="pl-pds">"</span></span>, <span class="pl-v">add</span><span class="pl-k">=</span><span class="pl-c1">T</span>)
legend(<span class="pl-s"><span class="pl-pds">"</span>topright<span class="pl-pds">"</span></span>, c(<span class="pl-s"><span class="pl-pds">"</span>Imputed<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>Non-imputed<span class="pl-pds">"</span></span>), <span class="pl-v">col</span><span class="pl-k">=</span>c(<span class="pl-s"><span class="pl-pds">"</span>blue<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>red<span class="pl-pds">"</span></span>), <span class="pl-v">lwd</span><span class="pl-k">=</span><span class="pl-c1">10</span>)</pre></div>
<p><a href="https://github.com/antoraf/Reproducible-Research-Projects/blob/Pictures/Picture3.png" target="_blank"><img src="https://github.com/antoraf/Reproducible-Research-Projects/blob/Pictures/Picture3.png" alt="plot of chunk unnamed-chunk-6" style="max-width:100%;"></a></p>
<p>Calculate new mean and median for imputed data.</p>
<div class="highlight highlight-source-r"><pre><span class="pl-smi">rmean.i</span> <span class="pl-k">&lt;-</span> mean(<span class="pl-smi">steps_by_day_i</span><span class="pl-k">$</span><span class="pl-smi">steps</span>)
<span class="pl-smi">rmedian.i</span> <span class="pl-k">&lt;-</span> median(<span class="pl-smi">steps_by_day_i</span><span class="pl-k">$</span><span class="pl-smi">steps</span>)</pre></div>
<p>Calculate difference between imputed and non-imputed data.</p>
<div class="highlight highlight-source-r"><pre><span class="pl-smi">mean_diff</span> <span class="pl-k">&lt;-</span> <span class="pl-smi">rmean.i</span> <span class="pl-k">-</span> <span class="pl-smi">rmean</span>
<span class="pl-smi">med_diff</span> <span class="pl-k">&lt;-</span> <span class="pl-smi">rmedian.i</span> <span class="pl-k">-</span> <span class="pl-smi">rmedian</span></pre></div>
<p>Calculate total difference.</p>
<div class="highlight highlight-source-r"><pre><span class="pl-smi">total_diff</span> <span class="pl-k">&lt;-</span> sum(<span class="pl-smi">steps_by_day_i</span><span class="pl-k">$</span><span class="pl-smi">steps</span>) <span class="pl-k">-</span> sum(<span class="pl-smi">steps_by_day</span><span class="pl-k">$</span><span class="pl-smi">steps</span>)</pre></div>
<ul>
<li>The imputed data mean is 1.059 × 10<sup>4</sup></li>
<li>The imputed data median is 1.0766 × 10<sup>4</sup></li>
<li>The difference between the non-imputed mean and imputed mean is -176.4949</li>
<li>The difference between the non-imputed mean and imputed mean is 1.1887</li>
<li>The difference between total number of steps between imputed and non-imputed data is 7.5363 × 10<sup>4</sup>. Thus, there were 7.5363 × 10<sup>4</sup> more steps in the imputed data.</li>
</ul>
<h2><a href="#are-there-differences-in-activity-patterns-between-weekdays-and-weekends" aria-hidden="true" class="anchor" id="user-content-are-there-differences-in-activity-patterns-between-weekdays-and-weekends"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Are there differences in activity patterns between weekdays and weekends?</h2>
<p>Created a plot to compare and contrast number of steps between the week and weekend. There is a higher peak earlier on weekdays, and more overall activity on weekends.</p>
<div class="highlight highlight-source-r"><pre><span class="pl-smi">weekdays</span> <span class="pl-k">&lt;-</span> c(<span class="pl-s"><span class="pl-pds">"</span>Monday<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>Tuesday<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>Wednesday<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>Thursday<span class="pl-pds">"</span></span>, 
              <span class="pl-s"><span class="pl-pds">"</span>Friday<span class="pl-pds">"</span></span>)
<span class="pl-smi">imputed_data</span><span class="pl-k">$</span><span class="pl-v">dow</span> <span class="pl-k">=</span> as.factor(ifelse(is.element(weekdays(as.Date(<span class="pl-smi">imputed_data</span><span class="pl-k">$</span><span class="pl-smi">date</span>)),<span class="pl-smi">weekdays</span>), <span class="pl-s"><span class="pl-pds">"</span>Weekday<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>Weekend<span class="pl-pds">"</span></span>))

<span class="pl-smi">steps_by_interval_i</span> <span class="pl-k">&lt;-</span> aggregate(<span class="pl-smi">steps</span> <span class="pl-k">~</span> <span class="pl-smi">interval</span> <span class="pl-k">+</span> <span class="pl-smi">dow</span>, <span class="pl-smi">imputed_data</span>, <span class="pl-smi">mean</span>)

library(<span class="pl-smi">lattice</span>)

xyplot(<span class="pl-smi">steps_by_interval_i</span><span class="pl-k">$</span><span class="pl-smi">steps</span> <span class="pl-k">~</span> <span class="pl-smi">steps_by_interval_i</span><span class="pl-k">$</span><span class="pl-smi">interval</span><span class="pl-k">|</span><span class="pl-smi">steps_by_interval_i</span><span class="pl-k">$</span><span class="pl-smi">dow</span>, <span class="pl-v">main</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>Average Steps per Day by Interval<span class="pl-pds">"</span></span>,<span class="pl-v">xlab</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>Interval<span class="pl-pds">"</span></span>, <span class="pl-v">ylab</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>Steps<span class="pl-pds">"</span></span>,<span class="pl-v">layout</span><span class="pl-k">=</span>c(<span class="pl-c1">1</span>,<span class="pl-c1">2</span>), <span class="pl-v">type</span><span class="pl-k">=</span><span class="pl-s"><span class="pl-pds">"</span>l<span class="pl-pds">"</span></span>)</pre></div>
<p><a href="https://github.com/antoraf/Reproducible-Research-Projects/blob/Pictures/Picture3.png" target="_blank"><img src="https://github.com/antoraf/Reproducible-Research-Projects/blob/Pictures/Picture3.png" alt="plot of chunk unnamed-chunk-10" style="max-width:100%;"></a></p>
</article>
  </div>
