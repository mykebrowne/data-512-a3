# data-512-a3 <br> 
# Final project plan <br> 

## To what extent is the opioid crisis driven by over-prescription? <br> 

### Background <br>
If you Google ‘opioid crisis’, you are likely to find many articles that highlight the increase in deaths associated with opioid usage in the US over the past two decades.   According to the CDC1, drug overdose deaths nearly tripled during the period 1999-2014 which some commentators2 have ascribed to the concurrent increase in opioid prescription rate, which quadrupled from 1999 to 2010.   One explanation provided is that overprescription of opioid pain relievers is a determinant of fatal overdose and abuse3.  In particular, individuals who are prescribed opioids for periods of over 90 days are unlikely to discontinue using and may switch to obtaining them illicitly when they can no longer obtain them through legitimate means4.   These illicit sources are more likely to include heroin and fentanyl which have been implicated in the significant increase in opioid death rates during 2014-20155. <br> 

The response to this crisis has varied.  Some states have enacted legislation designed to reduce opioid prescribing.  For example, Florida prohibited dispensing by prescribers in 2011 and subsequently experienced a 52% decline in oxycodone overdose death rate1.  The CDC advises local jurisdictions to target “high-prescribing areas for interventions such as...virtual physical therapy sessions with pain coping skills training...for chronic pain”2.  These approaches have, however, been criticized on the grounds that they penalize doctors for prescribing opioids to people who need them and for ignoring the wider socio-economic factors that contribute to drug addiction3. <br> 

This aim of this research is to further this debate by revisiting data on opioid prescribing and opioid overdose deaths and overlaying economic data to explore whether the relationship between opioid prescribing and opioid overdose is as straightforward as some of the Google headlines would suggest. <br> 

### Data sources <br> 

| Concept | Operational definition | Source |
| --- | ---| --- |
| Opioid prescription rate (at a year and state level) during 2006 to 2016 | Total number of retail opioid prescriptions dispensed in a given year and state per resident population | https://www.cdc.gov/drugoverdose/maps/rxstate2006.html <br><br> https://www.cdc.gov/drugoverdose/maps/rxstate2007.html | 
| Opioid overdose death rate (at a year and state level) | Total number of deaths due to:<br> ICD-10 underlying cause-of-death codes: X40-X44 (unintentional), X60-X64 (suicide), X85 (homicide), Y10-Y14 (undetermined intent). <br> <br>  And <br><br> ICD-10 multiple-cause-of-death codes:  T40.0 (Opium), T40.1 (Heroin), T40.2 (Other opioids), T40.3 (Methadone), T40.4 (Other synthetic narcotics), T40.6 (Other and unspecified narcotics).<br><br> In a given year and state per resident population. | Available at:  http://wonder.cdc.gov. | 
| Poverty rate (at a year and state level) during 2006 to 2016  | Population below poverty level in a given year and state per resident population | American Community Survey.  Poverty Status In The Past 12 Months. 2005 to 2016.  Available at: <br> <br> https://factfinder.census.gov/faces/nav/jsf/pages/searchresults.xhtml?refresh=t


### Questions to be investigated 

#### 1. What is the relationship between opioid prescription rate and opioid overdose rate? <br> 
If overprescription of opioids is a determinant of fatal overdose (as has been suggested by existing research), one would expect to see this reflected in the strength of the relationship between the opioid prescription rate and opioid overdose rate, both cross-sectionally (in other words, at a single point in time across geographies) and longitudinally (over time across a single geography).  
#### 2.  How does the relationship between opioid prescription rate and opioid overdose rate change given knowledge of the poverty rate?
If poverty somehow effects the relationship between opioid prescription and overdose rate, this would be manifested in a number of ways.   First the partial correlation between prescription and overdose rate given the poverty rate would be different to the overall correlation between the prescription and overdose rate.  Equivalently, when overdose rate is regressed against prescription rate and poverty rate together, one would expect to see either a main effect of poverty rate or an interaction between prescription rate and poverty rate.  

### Analytical approach 

#### 1. Create flat file unique at year, state level <br> 

| Column name | Type | Range | 
| --- | --- | --- | 
| Year | Categorical | {2006...2016} |
| State | Categorical | {AL...WY} |
| Prescription rate per population | Ratio | {0, 1} |
| Opioid overdose death rate per population | Ratio | {0, 1} |
| Poverty rate per population | Ratio | {0, 1} |
| Population | Integer |{0, ] |

#### 2. Visualize relationship between opioid prescription rates and overdose rates through scatterplots for: <br> 
- All states, all years 
- All states, split by year 
- Each state - all years (for selected states) 

#### 3. Calculate correlation coefficients <br> 
Between:  prescription and overdose rate;  partial correlation between prescription and overdose rate, controlling for poverty rate.    If the correlation between prescription and overdose rate is less strong when controlling for poverty rate, this would indicate that poverty has a role to play in influencing overdose rate.

#### 4. Perform Poisson regression with offset <br> 
Model the relationship between overdose rate, prescription rate and poverty rate.    More formally: <br> 
log(E[# overdoses]) = log(population) + β0 + (β1 * prescription rate)  +  (β2* poverty rate)  + ( β3* prescription rate : poverty rate) <br> 
If either the main effect of poverty,  β2, or the interaction effect,  β3, are significant this would also suggest that poverty influences overdose rate (independently) of prescription rate. <br> 
