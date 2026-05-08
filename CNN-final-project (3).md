Flight Delays and Cancellations in the U.S.
================

# Background and Objectives

Flight delays and cancellations remain a persistent challenge in the
aviation industry, affecting millions of passengers annually and
creating significant financial burdens for airlines. While these
disruptions typically stem from routine factors—such as weather,
security processing, carrier logistics, or scheduling issues—2025 has
introduced unprecedented shocks that have exacerbated the situation.

Notably, the industry faced a record-breaking U.S. federal government
shutdown. This crisis forced air traffic controllers and security staff
to work without pay, resulting in widespread staffing shortages and
operational paralysis at major airports. Furthermore, media coverage has
intensified surrounding two catastrophic events: the June crash of an
Air India flight from Ahmedabad to London, which sparked global debates
on regulatory oversight, and the collision between an American Airlines
jet and an army helicopter over the Potomac River. Severe disruptions
like these have not only grounded flights but have shaken the public’s
trust in air travel safety.

Inspired by these events, our project aims to examine how major
disruptions can contribute to severe airline delays. For this project we
analyze a comprehensive sample dataset of U.S. commercial flights in
2024, containing information about airlines, routes, timings, delays,
cancellations and operational factors. The goal of our project is to
analyze these delay and cancellation patters through exploratory data
analysis in 3 different ways: an analysis of airline, airport and route
delays & cancellations, a temporal analysis of delays & cancellations,
and geographical patterns of these delays & cancellations. The goal is
to identify significant patterns in delay behavior and visualize how
delays and cancellations vary across airlines, airports, routes and
cities. Following this, we use boosting and linear regression to build a
model capable of predicting whether a flight is likely to be delayed.

Following our analysis using supervised learning, we found that our
predictive models achieved acceptable overall accuracy but exhibited
poor specificity, largely because the models tended to classify nearly
all flights as on-time. This behavior is driven by a pronounced class
imbalance in the dataset, with substantially more on-time flights than
delayed flights, making it difficult for the models to reliably identify
the occurrence of delays. Accordingly, we model two related but distinct
outcomes: the occurrence of flight delays and the length of departure
delays. For delay occurrence, we initially apply a boosting
classification model, but due to its limited discriminative performance,
we transitioned to unsupervised learning methods to better diagnose the
structural reasons underlying this poor prediction. In contrast, when
modeling delay length, a linear regression model reveals that travel
season and departure hour are strong and statistically significant
predictors, indicating that while supervised models struggle to identify
whether delays occur, they are more effective at explaining the severity
of delays once they arise.

Due to time constraints and our project being a pilot-level study, we
were unable to address variables that we thought would also affect
delays and cancellations. To improve future research, we would recommend
incorporating datasets and variables that relate to weather conditions,
airport congestion, number of active flights, etc. This could further
build on and improve the model that we have explored in this project,
providing a more comprehensive analysis of the cause of flight delays in
the United States.

# Methodology

**Data Used**

We used a sample dataset of all commercial flights at U.S. airports in
2024 because we wanted a complete, single-year snapshot of aviation
activity. The dataset, which we found on Kaggle, contains 10,000 rows of
detailed flight-level observations, including airline identifiers,
airport codes, cities, and both arrival and departure delay times,
indicators for whether the flight was cancelled or not, along with the
categorized reasons for those delays.

To visualize these patterns geographically, we required the latitude and
longitude of each arrival and departure airport. For this, we used a
second Kaggle dataset that provides geographic coordinates and metadata
for all commercial airports in the United States. We filtered this
dataset to retain only the airports appearing in our 2024 flight sample,
ensuring that each flight could be accurately plotted on a U.S. map.

We also used a dataset containing the the year, month, and \# of
passengers travelling domestically and internationally. This dataset was
used to track seasonal travelling patterns across years, only focusing
on the domestic numbers (since we are only looking at flights within the
U.S. This was helpful when creating seasonal buckets in our analysis,
differentiating flight congestion depending on the time of year
(season). We also differentiated Pre-Covid, Covid, and Post-Covid to see
if there were any differences in the three time periods.

# I. Distribution of Flight Delays and Cancellations Across Different Airlines, Airports, and Routes

## Visualisations of Airlines by Number of Flights

![](CNN-final-project_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

In the graph above, we can see that Southwest Airlines (WN) had the most
number of flights in our sample dataset, with a little over 2000,
followed by Delta Airlines (DL) and American Airlines (AA) with
approximately 1400 flights.

## Total Miles Travelled by Each Airline

![](CNN-final-project_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Next, we see that this graph, which ranks the airlines based on their
sum total of miles travelled, corroborates what we see in the first
graph. Southwest Airlines is at the top with the most amount of miles
travelled at more than 1.5 million, while Delta and American Airlines
approximately travelled 1.4 million miles.

## Average Arrival Delay by Airport

Moving onto arrival delay analysis, the table above shows that average
arrival delays (in minutes) vary widely across airports, with a small
number of locations experiencing exceptionally high delays. Victoria
Regional Airport (VCT) recorded the highest average arrival delay at 152
minutes, followed by Flagstaff Pulliam Airport (FLG) at 117 minutes, and
several others exceeding 100 minutes. While most airports fall below
these extremes, these particular airports are significantly higher than
the national average of 66 minutes, which suggests that weather patterns
or infrastructure limitations can significantly affect performance.
Overall, the distribution of delays indicates that chronic delays are
concentrated in a subset of airports rather than evenly spread across
the national aviation network.

## Average Departure Delay by Airport

The examination of average departure delays across U.S. airports reveals
a similar pattern to arrival delays, with a small group of airports
experiencing disproportionately high disruptions. Victoria Regional
Airport (VCT) again ranks highest, with an average departure delay of
154 minutes, followed by Kodiak Benny Benson State Aiport (116 minutes)
and Minot International Aiport (112 minutes). Several others report
averages exceeding 100 minutes, highlighting persistent congestion or
operational challenges at these locations. While the majority of
airports exhibit lower delay levels, these results suggest that there
may be some structural or environmental issues that may uniquely impact
specific airports.

## Arrival and Departure Delay by Airline

![](CNN-final-project_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

The bar chart illustrates the average delay times for major airline
carriers, segmented by departure (solid blue) and arrival (hatched)
delays. There is significant variance in operational performance across
the industry: American Airlines (AA) exhibits the highest cumulative
delay, approaching 40 minutes, followed closely by Frontier (F9) and PSA
Airlines (OH). In contrast, Republic Airways (YX) stands out as a
distinct anomaly; despite recording minor positive departure delays, it
shows negative arrival delays, indicating a consistent pattern of
arriving ahead of schedule. For the majority of other carriers, however,
departure delays serve as the dominant component, compounding into
substantial arrival delays.

## Route Level Delay Analysis

![](CNN-final-project_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

This bar chart details the top 15 most delayed flight routes in 2024.
The route from Newark (EWR) to Chicago (ORD) suffers the most severe
disruptions with average departure delays approaching 90 minutes and
arrival delays exceeding 75 minutes. A consistent trend across the data
is that departure delays (yellow) generally outpace arrival delays
(blue), indicating that while aircraft often recover some time
mid-flight, initial ground setbacks remain the primary driver of
lateness. While major hubs like Atlanta (ATL) and Dallas-Fort Worth
(DFW) feature prominently in the top rankings, the severity of delays
tapers off significantly after the top few routes, with the bottom half
of the list averaging between 25 and 35 minutes of delay.

## Route Level Cancellation Analysis

![](CNN-final-project_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

This bar chart details the top 10 most cancelled routes of 2024. The
route with the highest probability of cancellation is the Tampa (TPA) to
Atlanta (ATL) route, which faces a cancellation probability of nearly
17%. This rate is significantly higher than the rest of the field,
nearly doubling the roughly 10% cancellation rates seen on the next most
affected routes, Newark (EWR) to Fort Lauderdale (FLL) and Boston (BOS)
to Orlando (MCO). It is also interesting that while TPA to ATL is the
most cancelled route here, it did not appear in the most delayed routes
graph, while the reverse route of ATL to TPA was present. This suggests
that when operational issues hit this specific route, airlines are more
likely to cancel this flight entirely rather than delay it. This could
be due to the fact that when bad weather or congestion hits a major hub
like Atlanta, operational teams often sacrifice these shorter regional
flights first; because there are multiple daily departures, it is far
easier to rebook passengers on the next plane than it would be to cancel
a long-haul flight, essentially making the TPA-ATL route a strategic
casualty to keep the broader system moving.

# II. Temporal Patterns in Flight Cancellations and Delays

## Patterns by Day of Week

![](CNN-final-project_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->![](CNN-final-project_files/figure-gfm/unnamed-chunk-13-2.png)<!-- -->

Next, we move onto temporal pattern analysis for delays and
cancellations. This bar chart of average departure trends highlights a
midweek sweet spot for travelers, with Tuesday and Wednesday showing the
lowest average delays at roughly 10 minutes. In contrast, schedule
reliability worsens toward the weekend, peaking on Saturday with an
average delay exceeding 16 minutes. This pattern likely reflects the
cumulative impact of network congestion: midweek flights benefit from
lighter business travel, whereas weekend surges of leisure travelers,
combined with delays accumulated earlier in the week, create a
compounding effect. Notably, although many travelers assume Friday or
Sunday are the busiest days, the data indicate that Saturday experiences
the longest delays, emphasizing that peak passenger volume does not
always coincide with peak operational disruption.

This bar chart examining 2024 cancellation rates by day of the week
reveals a surprising paradox for travelers. While Wednesday and Thursday
generally offer better on-time performance in terms of delays, they are
ironically the riskiest days for outright cancellations, with rates
peaking near 1.45%. In contrast, Sunday, often assumed to be chaotic due
to high passenger volume, has the lowest cancellation probability, below
0.9%. This pattern suggests that airlines may strategically cancel or
consolidate flights on lower-demand midweek days to optimize operational
efficiency, while making extra effort to maintain schedules on
high-demand weekends to avoid stranding large numbers of passengers.

## Patterns by Hour of Day

![](CNN-final-project_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->![](CNN-final-project_files/figure-gfm/unnamed-chunk-14-2.png)<!-- -->

The first line graph shows average departure delays by hour. Most
flights have the shortest delays between 5:00 AM and 11:00 AM, staying
under 10 minutes on average. Delays gradually increase during the day,
peaking at over 25 minutes around 8:00 PM. An exception is 4:00 AM,
where delays jump to over 50 minutes. This spike likely happens because
there are very few flights at that hour, so one delay, such as a late
crew or maintenance issue, can make the average very high.

This second line graph shows cancellation rates by hour. While there are
almost no cancellations between midnight and 4:00 AM, rates jump at 5:00
AM to over 2%, likely due to overnight maintenance issues or
late-arriving planes. The highest risk happens late at night, around
10:00 PM, when cancellations exceed 3%. These late-night cancellations
usually occur because delays have built up during the day, crews have
reached their maximum work hours, or airports close for the night,
making the last flights of the day the riskiest for travelers.

## Patterns by Time-of-Day Buckets

![](CNN-final-project_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->![](CNN-final-project_files/figure-gfm/unnamed-chunk-15-2.png)<!-- -->

The first bar chart illustrating average departure delay by time of day
confirms that evening time (18-21) is the most disrupted travel window,
suffering the highest delays at nearly 20 minutes. In contrast, the AM
(5–9) block offers the most reliable performance, with average delays
significantly lower at roughly 7.5 minutes. While delays climb steadily
through the Late AM (10–13) and Afternoon (14–17) blocks, an interesting
anomaly appears in the Night (22–4) category; despite low passenger
volumes, delays here remain stubbornly high at around 17 minutes,
supporting the results seen in other graphs about night time delays.

The second bar chart categorizing cancellation rates by time of day
identifies a critical block for travelers: the Night (22–4) block.
Flights scheduled during these late hours face a cancellation rate
approaching 2.8%, nearly double the rate of any other time block. While
the Late AM (10–13) block appears to be the safest bet with cancellation
rates dipping below 1.0%, the risk climbs again in the Afternoon (14–17)
to roughly 1.4%. The data strongly suggests that flying very late is a
gamble; unlike daytime cancellations where rebooking options exist, a
cancellation in the 22:00–04:00 window often means being stranded
overnight as airline operations wind down for the day.

# III. Geographical Patterns in Flight Cancellations and Delays

![](CNN-final-project_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->![](CNN-final-project_files/figure-gfm/unnamed-chunk-18-2.png)<!-- -->

We now move onto geographic patterns of flights cancellation & analysis.
This geographic heatmap of cancellation rates reveals that flight
reliability is highly dependent on location, with risk concentrated in
specific regional pockets rather than evenly distributed across the
country. The bright yellow hotspots indicating cancellation rates
approaching 9% are visible in areas like the Northeast and parts of the
Southeast, contrasting sharply with the stable dark purple markers (near
0% cancellations) that dominate the West Coast. Interestingly, the
largest circles representing major hubs like Atlanta and Chicago are
generally not the brightest yellow, suggesting that while they handle
the most volume, it is often the smaller or secondary airports that
suffer the highest probability of having flights scrapped entirely,
likely serving as the first cuts airlines make during operational
stress.

The second map shows average arrival delays across the U.S. and
highlights a clear East vs. West pattern. Airports in the Eastern U.S.,
particularly in the Northeast corridor and Florida, experience the
longest delays, often 15–25 minutes on average. This is likely due to
higher traffic density, congested airspace, and frequent weather
disruptions in the region. In contrast, airports in the Western U.S.
mostly see shorter delays of 0–10 minutes, reflecting lower traffic
volumes and less congested routes. The map also shows that major hub
airports, such as those in New York, Atlanta, and Miami, tend to have
darker markers, confirming that large, busy airports contribute
disproportionately to national delays. Overall, the visualization
suggests that both geographic location and airport size strongly
influence arrival punctuality, and that travelers flying west of the
Mississippi generally have a higher chance of arriving on time.

![](CNN-final-project_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

This bar chart ranks airports by the total number of cancellations,
showing Dallas-Fort Worth (DFW) at the top with over 20 cancellations in
the sample dataset. The runner-up, Newark (EWR), has about 14, while
LaGuardia (LGA) and Atlanta (ATL) follow with around 10 each. DFW’s
cancellations are much higher than other major hubs like Chicago (ORD)
or Detroit (DTW), which have roughly 5 each, highlighting that
cancellations are heavily concentrated at DFW. Several factors could
contribute to this, including DFW’s high flight volume, its role as a
major connecting hub where delays span across the network, weather
disruptions in the region, and operational constraints such as runway
capacity or staffing shortages. It is also interesting to note that
while TPA -\> ATL had the highest cancellation rate on a previous chart,
DFW has the highest total number of cancellations. This suggests that
TPA’s issue is likely route-specific, whereas DFW faces broader
operational challenges affecting many flights.

![](CNN-final-project_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

This bar chart shows airports ranked by the number of flights delayed
more than 15 minutes, a common threshold to highlight delays that really
affect passengers. Dallas-Fort Worth (DFW) has the most delays, with
over 225 flights, followed by Atlanta (ATL) and Denver (DEN) with about
180–190 each. Big central hubs like Chicago (ORD) and Charlotte (CLT)
also have many delays because they handle a lot of connecting flights,
tight schedules, and delays that spread from earlier flights. Other
factors, like bad weather, crowded airspace, and limited runway or gate
space, make delays worse. In contrast, busy coastal airports like JFK
and Boston (BOS) have fewer major delays, showing that the biggest
problems happen at central hubs with complex flight networks.

![](CNN-final-project_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

This set of pie charts breaks down delay causes for four major hubs,
revealing a clear distinction between network problems and local
problems. We chose to focus on the top three airports with the most
delays in our dataset: Atlanta (ATL), Denver (DEN), and Dallas-Fort
Worth (DFW); as well as Los Angeles (LAX) because we live in Claremont
and often commute through LAX. The three busiest connecting hubs share a
similar profile dominated by late aircraft, indicating that their main
reasons for delays come from incoming flights arriving behind schedule.
In contrast, LAX stands apart with a dominant carrier delay share,
suggesting that disruptions there are more often caused by
airline-specific issues such as maintenance or crew logistics rather
than system-wide congestion. Additionally, NAS (National Aviation
System) delays, which include heavy air traffic volume and minor weather
impacts, play a noticeable role at DEN and DFW, while weather delays
remain visible at the interior hubs compared to the generally consistent
weather at LAX.

# IV. Reasons for Delays and Cancellations

![](CNN-final-project_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

This pie chart breaking down the main causes of flight delays in 2024
shows that the biggest factor is not weather, but the ripple effect of
earlier delays. Late aircrafts makes up the largest share of total delay
minutes, showing that when a plane is late, it causes delays for all
subsequent flights using that aircraft. The second-largest factor is
carrier delay, which includes airline-controlled issues like
maintenance, cleaning, or crew scheduling. Together, these two
operational causes have a much bigger impact than weather delays or
National Air System Delays, challenging the common idea that storms are
the main reason for getting stuck on the tarmac. This pattern aligns
with what we saw in the hub-specific pie charts, where late aircraft
delays dominated at major connecting airports like ATL, DEN, and DFW,
while carrier-related issues were particularly prominent at LAX.

Overall, our exploratory analysis reveals clear patterns in flight
delays and cancellations across U.S. airlines, airports, and routes.
Delays are concentrated at major connecting hubs, particularly DFW, ATL,
and DEN, with interior airports experiencing higher disruption than most
coastal airports. Operational factors like late aircraft and carrier
delays consistently outweigh weather and National Air System delays,
emphasizing the cascading nature of flight schedules. Temporal patterns
show that midweek and early morning flights are generally more reliable,
while weekends and late-night flights carry higher risks of delay or
cancellation. Geographically, the Eastern U.S. and heavily trafficked
hubs see the most significant delays, while Western airports benefit
from lighter congestion. These insights collectively paint a detailed
picture of when, where, and why flights experience operational
difficulties.

# V. Flight Delay Predictions

Building on these insights, the next step of our project focuses on
predicting flight delays using supervised learning. By leveraging the
patterns identified in the exploratory analysis, such as airline,
airport, route, day of the week, and time of day, we aim to construct a
model capable of estimating the likelihood of a flight being delayed.
This predictive approach will allow us to move beyond descriptive
insights, providing actionable information for travelers and airlines to
anticipate and mitigate disruptions.

## Feature Engineering: Day of Week × Time of Day

We believe that flight delays are not determined by the day of the week
or the time of day independently, but rather by the interaction between
the two. For example, a Monday morning departure may face very different
operational conditions than a Sunday morning departure, and weekday
afternoon flights are often subject to peak congestion that does not
occur at the same time on weekends.

By constructing an interaction term between day of week and time of day,
we want to capture these compound scheduling effects that would
otherwise be missed by treating the variables separately. This
interaction allows our model to distinguish between structurally
different flight environments, such as weekday peak-hour traffic versus
off-peak weekend operations, thereby providing a more realistic
representation of temporal congestion patterns in air travel.

## Feature Engineering: Travel Season

We hypothesize that domestic air passenger demand exhibits a strong
seasonal pattern driven by travel behavior such as holidays, school
schedules, and weather conditions. To explore this, we analyze monthly
domestic passenger data from the U.S. Bureau of Transportation
Statistics, aggregated across 2015-2025.

The data reveal a clear and recurring pattern: passenger volumes are
lowest in January and February, rise sharply beginning in March, and
remain elevated throughout the spring and summer months, peaking between
March and August. Demand then declines during the fall and early winter
period from September to December, though it remains higher than the
early-year trough.

![](CNN-final-project_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

Based on this empirical pattern, we construct a categorical Season
variable by grouping months into three demand regimes: shoulder season
(January–February), peak season (March–August), and moderate season
(September–December).

## Use of Boosting for Delay Prediction and Resulting Performance

We apply a gradient boosting model to predict whether a flight
experiences a departure delay of 15 minutes or more using airline, day
of week, departure hour, travel season, and the interaction between day
of week and time of day (dow_period) as predictors. Boosting is well
suited for this task because it can capture nonlinear effects and
complex temporal interactions that simpler models may overlook.

In practice, while the boosting model achieves high overall accuracy,
this performance is driven largely by the predominance of on-time
flights in the dataset. The model demonstrates strong sensitivity to
on-time flights but very limited ability to identify delayed flights,
reflecting the underlying class imbalance and the limited predictive
signal in the available scheduling variables. Adjusting probability
thresholds improves detection of delayed flights but highlights the
inherent difficulty of the prediction task rather than a deficiency in
the modeling approach.

![](CNN-final-project_files/figure-gfm/unnamed-chunk-31-1.png)<!-- -->

## Insights from Unsupervised Learning

Given the limited discriminative performance of supervised models, we
transition to unsupervised learning to better understand the structure
of the feature space and diagnose the source of model limitations. Using
Principal Component Analysis (PCA), we examine whether delayed flights
naturally separate from on-time flights based on airline and temporal
characteristics. The PCA results reveal substantial overlap between
delayed and on-time flights across all principal components, indicating
that these classes are not well separated in the feature space. This
finding suggests that flight delays are influenced by stochastic
operational factors, such as weather disruptions, aircraft availability,
and air traffic control decisions, that are not captured by scheduling
information alone.

![](CNN-final-project_files/figure-gfm/unnamed-chunk-32-1.png)<!-- -->

# VI. Predictions of Flight Delay Length

We estimate a linear regression model to predict departure delay length
(in minutes) using airline fixed effects, day-of-week indicators,
departure hour, and a seasonal travel indicator. Overall, the model is
statistically significant (F-statistic = 8.10, p \< 0.001), indicating
that the included predictors jointly explain variation in departure
delays. However, the model’s explanatory power is modest, with an
R-squared of 0.023, suggesting that only about 2.3% of the variation in
delay length is explained by these variables. This is expected given
that flight delays are influenced by many unobserved operational factors
such as weather, air traffic congestion, and aircraft turnaround
constraints.

## Key Predictors

Departure hour is a strong and highly significant predictor of delay
length. The coefficient of 1.01 implies that, holding other factors
constant, each additional hour later in the day is associated with
approximately one extra minute of departure delay. This result is
consistent with delay propagation throughout the day, where early
disruptions accumulate and affect later flights.

The seasonal indicators reveal meaningful differences in delay severity
across travel seasons. Relative to the shoulder season
(January–February), flights during the peak season (March–August)
experience delays that are on average 6.27 minutes longer, and this
effect is statistically significant (p \< 0.001). In contrast, the
moderate season (September–December) is associated with slightly shorter
delays (–2.37 minutes), though this effect is not statistically
significant. These findings align with higher demand, congestion, and
capacity constraints during peak travel months.

Airline effects are present but mixed. Some carriers exhibit
significantly longer delays relative to the reference airline, while
others show no statistically distinguishable differences. This suggests
heterogeneity in operational performance, though many airline
coefficients are imprecisely estimated once time and season controls are
included.

Day-of-week effects are generally small and statistically weak. While
weekend flights (especially Saturdays) show slightly longer delays on
average, these effects are not strongly significant, indicating that
departure time and seasonality play a larger role than the day of week
in explaining delay duration.

## Model Performance

Out-of-sample evaluation yields an RMSE of 42.3 minutes and a mean
absolute error (MAE) of 23.0 minutes. These values indicate substantial
prediction uncertainty, reflecting the inherently noisy nature of delay
length outcomes. While the model captures systematic patterns related to
time of day and seasonality, it is not designed to provide highly
precise individual flight delay predictions.

## Summary

Overall, the results suggest that temporal and seasonal factors
systematically affect the length of flight delays, with peak travel
periods and later departures being particularly prone to longer delays.
However, the low R-squared highlights the limitations of a linear model
that excludes key operational variables. This regression should
therefore be interpreted as capturing average structural effects rather
than serving as a high-accuracy predictive model.
