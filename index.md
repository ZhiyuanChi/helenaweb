## Portfolio
---
### [Where Should Philly Build New bike Share Stations?: Predicting and Evaluating for Philadelphia's Bike Share Expansion]( https://indegoexpansion.web.app/)

[Indego](https://www.rideindego.com/), Philadelphia's bike share system, wants to expand their network to increase ridership (revenue), accessibility, and equity. To help Indegeo evaluate different expansion plans, we created [Station Planner](https://indegoexpansion.web.app/). As our team's machine learning lead, I built a model that predicts areas of high and low ridership across Philadelphia.  Powered by these predictions, our interactive tool lets planners place new bike stations on the map to see what expected future ridership, accessibility, and equity looks like for the system. 

<img src="images/practicum/app_screenshot_2.png?raw=true" alt="Station Planner App main map page for planning future bike stations."/>

Check out our [interactive app](https://indegoexpansion.web.app/), our [summary presentation](https://indegoexpansion.web.app/about), or follow along with our [R code and report](https://indegoexpansion.web.app/html/Final_Presentation.html).

This project was produced as part of the [University of Pennsylvania’s Master of Urban Spatial Analytics (MUSA) Spring 2023 Practicum course](https://pennmusa.github.io/MUSA_801.io/) with my colleagues [Minwook Kang](https://mintheworld.com/) and Aidan Rhianne.

---
### [Where is the city growing?: Identifying Urban Land in Indonesia Using Convolutional Neural Networks](https://rebekahadams.com/pdf/adams-tran-urban-expansion-pres.pdf)
Cities across the Global South are expanding rapidly. Yet few datasets include up-to-date information on land cover in many of these countries, making it difficult to track this urban growth. To address this dearth of information, I worked with Tiffany Tran to pilot three deep learning algorithms to identify urban land in the city of Cirebon, Indonesia.

For this project, I created a novel dataset of satellite images of Cirebon and its surrounding area, based on the city's most recently mapped area (2014). I then used these images to train and test three different types of convolutional neural networks using Tensorflow in Python. Our best model accurately identified urban vs. non-urban images 82% of the time. 

<img src="images/remote_sensing/data_creation.png?raw=true" alt="Presentation slide of the steps to create labelled image tiles from a satellite image of Cirebon, Indonesia."/>

But what we were really intersted in were the images that our models *thought* were urban areas, but according to the most recent (2014) mapped boundary were *not* urban. These "false positives" give us a feasible way to identify potential areas of new urban expansion since 2014. 

<img src="images/remote_sensing/false_positives.png?raw=true" alt="Presentation slide of examples of false positives: images predicted as urban but labelled as non-urban. Some examples show areas that appear to be urbanized."/>

Check out our [summary presentation](https://rebekahadams.com/pdf/adams-tran-urban-expansion-pres.pdf), follow along with [our Python code](https://github.com/rradams/MUSA650_RemoteSensing_Final), or [read our report](https://rebekahadams.com/pdf/adams-tran-urban-expansion-report.pdf).

---
### Estimating Causality: Estimating the Impact of NC-540 on Home Values
Wake County, North Carolina is in the process of building NC-540, a tolled beltway through Raleigh’s outer suburbs. I wanted to know: what impact has the highway had on nearby home values? 

Using a Differences-in-Differences approach, I estimate that single-family homes within two miles of NC-540 have seen their values increase by more than $36,000 thanks to the new highway. When home features are taken into account, the effect is even larger: homes near the highway have seen an average $49,000 boost in value.

<img src="images/exp_design/control_v_treatment_tiled.png?raw=true" alt="Map of treatment and control groups. Homes within the treated group are homes within a 2-mile redius of existing NC-540 entrances and exits. Homes within the control group are homes within a 2-mile radius of estimated future NC-540 intersections."/>

This result is robust to a placebo test, indicating that these results can be attributed to the opening of the highway in 2012. Examining the parallel trends of treatment and control groups, we see that after 2012, homes within the treatment group (i.e. homes with access of NC-540) saw their home values increase at a greater pace than those in the control group. It also appears that home values in the treatment group were more resilient to the 2008 financial crisis despite not yet having access to NC-540. It is possible that anticipation of the future highway helped buoy these homes' values at that time.

<img src="images/exp_design/NC540_Parallel_Trends.png?raw=true" alt="Graph of the parallel trends of average home values (in 2019 dollars) of treatment and control groups from 2000-2019. From 2000-2008, the control group's home values were slightly higher than the treatment group. In 2008, this trend reversed, and treatment group's home values depreciated less quickly than the control group's. After 2012, there is steeper increase in home values of the treatment group compared to the control group."/>

For deeper discussion into my methods, check out [my report](https://rebekahadams.com/pdf/Adams_DID_report.pdf). This report was prepared for my class on Quasi-Experimental Design Methods. I used R for the analysis.

---
### [When will the train arrive?: Predicting NJ Transit Delays](https://rradams.github.io/adams_rummler_MUSA508_final/Adams_Rummler_508_Final.html)
Commuters need to get to their destinations on time. To provide commuters with greater insight into their commute, my colleague [Jack Rummler](https://jtrummler.xyz/) and I created a fictional app that predicts NJ Transit train delays in advance. I led our model's development and built our predictive model in R.

Our app's predictive models were highly accurate - on average, our predictions were off by just 26 seconds. Our combination of models provided increasingly accurate delay predictions up to a week in advance.

<img src="images/njtransit/models_mae_line.png?raw=true" alt="Graph of increasingly accurate predictions of train delays over time."/>

Follow along with our code [here](https://rradams.github.io/adams_rummler_MUSA508_final/Adams_Rummler_508_Final.html), or watch our presentation [here](https://www.youtube.com/watch?v=vrF7Rini-4M).

---
### [How much will that home sell for?: Predicting home prices in Mecklenburg County, NC](https://rebekahadams.com/htmls/charlotte_home_prices.html)
Mecklenburg County, NC is home to more than a million residents and the city of Charlotte. Using Python, I created a simple machine learning model to predict home prices across the county.

I used a spatial analysis of Local Indicators of Spatial Autocorrelation (LISA) to hone in on statistically significant clusters of high and low home prices.

<img src="images/charlotte/mecklenburg_LISA.png?raw=true" alt="Four maps of LISA's I statistics across Mecklenburg County."/>

Follow along with my Python code [here](https://rebekahadams.com/htmls/charlotte_home_prices.html).

---
### [Solving an Operational Puzzle: Forecasting Citibike trips for system rebalancing in Brooklyn, NY](https://rebekahadams.com/htmls/Adams_BikeshareHW5_v2.html)
Citibike lets New Yorkers hop on a bike when they want, to go wherever they want. No more worrying about where to lock up a bike - Citibike’s stations have dedicated slots to securely return your bikes. And no more having to schlep your bike home - bike share means that you can take one-way trips and not have to worry about your bike when you’re done. It’s convenient, easy, and affordable.

This convenience of on-demand bikes creates a puzzle for Citibike: how to ensure that bikes are available where and when they’re needed? If bikes aren’t available at the right place and the right time, ridership and revenue will fall. It’s therefore very important to rebalance bikes in the system: i.e. move bikes from areas of low demand to areas of high demand, before the high demand begins.

To help Citibike rebalance bikes, I created a model to forecast bikeshare demand in Brooklyn. Follow along with my [R code here](https://rebekahadams.com/htmls/Adams_BikeshareHW5_v2.html). This project was originally an assignment for the University of Pennsylvania's Public Policy Analytics course.

<img src="images/Citibike/bk_citibike_trips.gif?raw=true" alt="Gif map of Citibike trips by station across Brooklyn on a Sunday in September 2022. Trips rise over the course of the day, and appear to concentrate in DUMBO, Williamsburg, and Prospect Park."/>

---
### [Are New Yorkers Willing to Pay More for Subway Proximity?: An Exploratory Analysis of Transit Oriented Development (TOD) in New York City](https://rebekahadams.com/htmls/NYC_TOD_study.html)

New York is famous for its iconic subway and sky-high rents. But even in the face of such high rents, are New Yorkers still willing to pay more to live close to the subway? If they are, the City could consider zoning changes to allow more development close to transit. Increased density could mean increased tax revenue, potentially allowing the City to reinvest that income into expanding the subway and other city services.

In this policy brief, I investigate whether rents are higher in areas close to the subway, compared to areas without subway access, and how that has changed over time between 2011 and 2020. [I include my R code](https://rebekahadams.com/htmls/NYC_TOD_study.html) to demonstrate how I performed the analysis.

<img src="images/NYC_TOD/NYC_TOD.png?raw=true" alt="Map of median rent prices in NYC in 2020."/>

This brief was completed as an assignment in my Public Policy Analytics course at the University of Pennsylvania.

---
### [Predicting Churn: Who will enroll in a housing subsidy program?](https://rebekahadams.com/htmls/RAdams_churn.html)
The fictional Emil City Department of Housing and Community Development wants to figure out how to increase enrollees in a housing subsidy program. I used their campaign data to understand who was more likely to enroll in or drop out (churn) from the program.

I completed this assignment for the University of Pennsylvania's Public Policy Analyitics course. While the housing subsidy is fictional, the data set used is from a real loan program. This data turned out to be very challenging to build a model to correctly predict for, but I ultimately built a model that correctly predicted enrollment 12.6% of the time -- a modest but positive improvement from the program's previous success rate of 11%. I built this model using R.

<img src="images/churn/thresholds.png?raw=true" alt="Graphs of the total net benefit and total credit provided versus the selected threshold. As the threshold rises, so does the net benefit."/>

Follow along with my [R code here](https://rebekahadams.com/htmls/RAdams_churn.html).

---
<p style="font-size:11px">Page template forked from <a href="https://github.com/evanca/quick-portfolio">evanca</a></p>
<!-- Remove above link if you don't want to attibute -->
