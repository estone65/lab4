Lab 04 - Visualizing spatial data”
================
Eric Stone
2024-02-01

### Load packages and data

``` r
library(tidyverse) 
library(dsbox) 
data ("dennys")
data ("laquinta")
```

``` r
states <- read_csv("data/states.csv")
```

### Exercise 1

What are the dimensions of the Denny’s dataset? (Hint: Use inline R code
and functions like nrow and ncol to compose your answer.) What does each
row in the dataset represent? What are the variables?

``` r
number_of_rows <- nrow(dennys)
number_of_columns <- ncol(dennys)
print(paste("Number of rows:", number_of_rows))
```

    ## [1] "Number of rows: 1643"

``` r
print(paste("Number of columns:", number_of_columns))
```

    ## [1] "Number of columns: 6"

``` r
colnames(dennys)
```

    ## [1] "address"   "city"      "state"     "zip"       "longitude" "latitude"

There are 1643 rows and 6 columns.

The rows indicate the specific dennys establishment; the columns are the
address, city, state, zip, longitude, and latitude.

### Exercise 2

``` r
number_of_rows <- nrow(laquinta)
number_of_columns <- ncol(laquinta)
print(paste("Number of rows:", number_of_rows))
```

    ## [1] "Number of rows: 909"

``` r
print(paste("Number of columns:", number_of_columns))
```

    ## [1] "Number of columns: 6"

``` r
colnames(laquinta)
```

    ## [1] "address"   "city"      "state"     "zip"       "longitude" "latitude"

What are the dimensions of the La Quinta’s dataset? What does each row
in the dataset represent? What are the variables?

There are 909 rows and 6 columns.

The rows indicate the specific laquita establishment; the columns are
the address, city, state, zip, longitude, and latitude.

### Exercise 3

``` r
# examined website and data sets
```

Take a look at the websites that the data come from (linked above). Are
there any La Quinta’s locations outside of the US? If so, which
countries? What about Denny’s?

I saw no locations outside the US listed in the website
<http://njgeo.org/2014/01/30/mitch-hedberg-and-gis/>

I also saw no Denny’s that were outside the United States.

However, there are La Quinta locations outside the US. The problem seems
to be that states are listed that aren’t really states. The ones I
detected were:

Aguascalientes (State = AG) Medellin Colombia (State = ANT) Richmond
(State = BC) Col Partido Iglesias Juarez (State = CH) contiguo Mall Las
Cascadas Tegucigalpa (State = FM) Parque Industrial Interamerican
Apodaca (State = NL) Col. Centro Monterrey (State = NL) Monterrey (State
= NL) Oshawa (State = ON) San Jose Chiapa (State = PU) Col.
ReservaTerritorial Atlixcayotl San Puebla (State = PU) Cancun (State =
QR) San Luis Potosi (State = SL) Poza Rica (State = VE)

It’s possible I missed one or two, but the point is that there are a
bunch of these. The person who constructed this data set should be
reprimanded.

### Exercise 4

``` r
# describe possible approaches
```

Now take a look at the data. What would be some ways of determining
whether or not either establishment has any locations outside the US
using just the data (and not the websites). Don’t worry about whether
you know how to implement this, just brainstorm some ideas. Write down
at least one as your answer, but you’re welcomed to write down a few
options too.

Well, I “brute forced” it, using my knowledge of state abbreviations. I
just scrolled down until I got to a state abbreviation I wasn’t familiar
with. Typically there was only 1 instance of that abbreviation (three at
most).

In SPSS I would enter each of the 51 abbreviations as part of an if
command, giving me a 1 if it matches and a 0 otherwise. The command
would look something like:

Do if state eq (al or aq or az ….) Compute US = 1 Else Compute US = 0
End if

I expect R could do something similar. That first command is tedious,
though, with 51 states. Since these abbreviations are all listed in the
‘states’ data set, there is probably a way to use that data set in
conjunction with the laquinta data set and simpler code.

### Exercise 5

Find the Denny’s locations that are outside the US, if any. To do so,
filter the Denny’s locations for observations where state is not in
states$abbreviation. The code for this is given below. Note that the %in% operator matches the states listed in the state variable to those listed in states$abbreviation.
The ! operator means not. Are there any Denny’s locations outside the
US?

``` r
dennys  %>%
  filter(!(state %in% states$abbreviation))
```

    ## # A tibble: 0 × 6
    ## # ℹ 6 variables: address <chr>, city <chr>, state <chr>, zip <chr>,
    ## #   longitude <dbl>, latitude <dbl>

Nope, no dennys outside the US – at least not in this data set! (And
yes, this is much easier than what I did :) )

### Exercise 6

Add a country variable to the Denny’s dataset and set all observations
equal to “United States”. Remember, you can use the mutate function for
adding a variable. Make sure to save the result of this as dn again so
that the stored data frame contains the new variable going forward.

``` r
dennys_us <- dennys %>%
  mutate(country = "United States")
colnames(dennys_us)
```

    ## [1] "address"   "city"      "state"     "zip"       "longitude" "latitude" 
    ## [7] "country"

It worked!

### Exercise 7

Find the La Quinta locations that are outside the US, and figure out
which country they are in. This might require some googling. Take notes,
you will need to use this information in the next exercise.

``` r
laquinta_no_us <- laquinta %>% 
  filter(!(state %in% states$abbreviation))
laquinta_no_us %>%
  select(address, city, state) %>%
  print()
```

    ## # A tibble: 14 × 3
    ##    address                                                           city  state
    ##    <chr>                                                             <chr> <chr>
    ##  1 Carretera Panamericana Sur KM 12                                  "\nA… AG   
    ##  2 Av. Tulum Mza. 14 S.M. 4 Lote 2                                   "\nC… QR   
    ##  3 Ejercito Nacional 8211                                            "Col… CH   
    ##  4 Blvd. Aeropuerto 4001                                             "Par… NL   
    ##  5 Carrera 38 # 26-13 Avenida las Palmas con Loma de San Julian El … "\nM… ANT  
    ##  6 AV. PINO SUAREZ No. 1001                                          "Col… NL   
    ##  7 Av. Fidel Velazquez #3000 Col. Central                            "\nM… NL   
    ##  8 63 King Street East                                               "\nO… ON   
    ##  9 Calle Las Torres-1 Colonia Reforma                                "\nP… VE   
    ## 10 Blvd. Audi N. 3 Ciudad Modelo                                     "\nS… PU   
    ## 11 Ave. Zeta del Cochero No 407                                      "Col… PU   
    ## 12 Av. Benito Juarez 1230 B (Carretera 57) Col. Valle Dorado Zona H… "\nS… SL   
    ## 13 Blvd. Fuerza Armadas                                              "con… FM   
    ## 14 8640 Alexandra Rd                                                 "\nR… BC

This seems to match perfectly what I came up with previously!

AG = Mexico (AGUASCALIENTES) QR = Mexico (Cancun) CH = Mexico
(Coahuila?) NL = Mexico (NUEVO LEON) ANT = Columnbia (Antioquia) ON =
Canada (Ontario) VE = Mexico (Veracruz) PU = Mexico (Pueblo) SL = Mexico
(SAN LUIS POTOSI) FM = Honduras (Francisco Morazán) BC = Canada (British
Columnbia)

### Exercise 8

Add a country variable to the La Quinta dataset. Use the case_when
function to populate this variable. You’ll need to refer to your notes
from Exercise 7 about which country the non-US locations are in. Here is
some starter code to get you going:

``` r
laquinta_country <- laquinta %>% 
  mutate(country = case_when(
    state %in% state.abb ~ "United States",
    state %in% c("ON", "BC") ~ "Canada",
    state == "ANT" ~ "Colombia",
    state == "FM" ~ "Honduras",
    state %in% c("AG", "QR", "CH", "NL","VE", "PU","SL") ~ "Mexico"
  ))
```

This command worked well.

### Exercise 9

``` r
laquinta_us <- laquinta_country %>% 
 filter(country == "United States")
state_freq_denny <- dennys_us %>%
  count(state)
print(state_freq_denny,  n = Inf)
```

    ## # A tibble: 51 × 2
    ##    state     n
    ##    <chr> <int>
    ##  1 AK        3
    ##  2 AL        7
    ##  3 AR        9
    ##  4 AZ       83
    ##  5 CA      403
    ##  6 CO       29
    ##  7 CT       12
    ##  8 DC        2
    ##  9 DE        1
    ## 10 FL      140
    ## 11 GA       22
    ## 12 HI        6
    ## 13 IA        3
    ## 14 ID       11
    ## 15 IL       56
    ## 16 IN       37
    ## 17 KS        8
    ## 18 KY       16
    ## 19 LA        4
    ## 20 MA        8
    ## 21 MD       26
    ## 22 ME        7
    ## 23 MI       22
    ## 24 MN       15
    ## 25 MO       42
    ## 26 MS        5
    ## 27 MT        4
    ## 28 NC       28
    ## 29 ND        4
    ## 30 NE        5
    ## 31 NH        3
    ## 32 NJ       10
    ## 33 NM       28
    ## 34 NV       35
    ## 35 NY       56
    ## 36 OH       44
    ## 37 OK       15
    ## 38 OR       24
    ## 39 PA       40
    ## 40 RI        5
    ## 41 SC       17
    ## 42 SD        3
    ## 43 TN        7
    ## 44 TX      200
    ## 45 UT       27
    ## 46 VA       28
    ## 47 VT        2
    ## 48 WA       49
    ## 49 WI       25
    ## 50 WV        3
    ## 51 WY        4

``` r
state_freq_laquinta <- laquinta_us %>%
    count(state)
print(state_freq_laquinta,  n = Inf)
```

    ## # A tibble: 48 × 2
    ##    state     n
    ##    <chr> <int>
    ##  1 AK        2
    ##  2 AL       16
    ##  3 AR       13
    ##  4 AZ       18
    ##  5 CA       56
    ##  6 CO       27
    ##  7 CT        6
    ##  8 FL       74
    ##  9 GA       41
    ## 10 IA        4
    ## 11 ID       10
    ## 12 IL       17
    ## 13 IN       17
    ## 14 KS        9
    ## 15 KY       10
    ## 16 LA       28
    ## 17 MA        6
    ## 18 MD       13
    ## 19 ME        1
    ## 20 MI        4
    ## 21 MN        7
    ## 22 MO       12
    ## 23 MS       12
    ## 24 MT        9
    ## 25 NC       12
    ## 26 ND        5
    ## 27 NE        5
    ## 28 NH        2
    ## 29 NJ        5
    ## 30 NM       19
    ## 31 NV        8
    ## 32 NY       19
    ## 33 OH       17
    ## 34 OK       29
    ## 35 OR       10
    ## 36 PA       10
    ## 37 RI        2
    ## 38 SC        8
    ## 39 SD        2
    ## 40 TN       30
    ## 41 TX      237
    ## 42 UT       12
    ## 43 VA       14
    ## 44 VT        2
    ## 45 WA       16
    ## 46 WI       13
    ## 47 WV        3
    ## 48 WY        3

``` r
#the following was from chatgpt, as it was more helpful to see them together
state_freq_denny <- dennys_us %>%
  count(state) %>%
  rename(dennys_freq = n)

state_freq_laquinta <- laquinta_us %>%
  count(state) %>%
  rename(laquinta_freq = n)

combined_freq <- full_join(state_freq_denny, state_freq_laquinta, by = "state")

print(combined_freq, n = Inf)
```

    ## # A tibble: 51 × 3
    ##    state dennys_freq laquinta_freq
    ##    <chr>       <int>         <int>
    ##  1 AK              3             2
    ##  2 AL              7            16
    ##  3 AR              9            13
    ##  4 AZ             83            18
    ##  5 CA            403            56
    ##  6 CO             29            27
    ##  7 CT             12             6
    ##  8 DC              2            NA
    ##  9 DE              1            NA
    ## 10 FL            140            74
    ## 11 GA             22            41
    ## 12 HI              6            NA
    ## 13 IA              3             4
    ## 14 ID             11            10
    ## 15 IL             56            17
    ## 16 IN             37            17
    ## 17 KS              8             9
    ## 18 KY             16            10
    ## 19 LA              4            28
    ## 20 MA              8             6
    ## 21 MD             26            13
    ## 22 ME              7             1
    ## 23 MI             22             4
    ## 24 MN             15             7
    ## 25 MO             42            12
    ## 26 MS              5            12
    ## 27 MT              4             9
    ## 28 NC             28            12
    ## 29 ND              4             5
    ## 30 NE              5             5
    ## 31 NH              3             2
    ## 32 NJ             10             5
    ## 33 NM             28            19
    ## 34 NV             35             8
    ## 35 NY             56            19
    ## 36 OH             44            17
    ## 37 OK             15            29
    ## 38 OR             24            10
    ## 39 PA             40            10
    ## 40 RI              5             2
    ## 41 SC             17             8
    ## 42 SD              3             2
    ## 43 TN              7            30
    ## 44 TX            200           237
    ## 45 UT             27            12
    ## 46 VA             28            14
    ## 47 VT              2             2
    ## 48 WA             49            16
    ## 49 WI             25            13
    ## 50 WV              3             3
    ## 51 WY              4             3

Which states have the most and fewest Denny’s locations? What about La
Quinta? Is this surprising? Why or why not?

California has by far the most Denny’s locations. Delaware has the
fewest, but all small states had only a small number. All states had at
least one.

Texas has the most La Quinta locations. Again, size of state is an
important factor, but another important factor is whether the state is
southern or not. DC, Delaware, and Hawaii have no La Quintas. In
general, the key difference is that there’s a clear tendency for
southern states to have more La Quinta locations, but not more Dennys
locations. In retrospect, this isn’t surprising at all given that states
further south are more Spanish speaking, which is consistent with the
name La Quinta. Furthermore, most of the non-US La Quintas are in
Mexico, which is also consistent with this finding.

### Exercise 10

Next, let’s calculate which states have the most Denny’s locations per
thousand square miles. This requires joining information from the
frequency tables you created in the previous set with information from
the states data frame.

First, we count how many observations are in each state, which will give
us a data frame with two variables: state and n. Then, we join this data
frame with the states data frame. However note that the variables in the
states data frame that has the two-letter abbreviations is called
abbreviation. So when we’re joining the two data frames we specify that
the state variable from the Denny’s data should be matched by the
abbreviation variable from the states data:

``` r
laquinta_us_with_area <- laquinta_us %>% 
  count(state) %>%
  inner_join(states, by = c("state" = "abbreviation")) %>%  
  mutate(location_per_thousand_miles = 1000 * n / area)
laquinta_us_with_area %>%
  select(state, location_per_thousand_miles) %>%
  print(n = Inf)
```

    ## # A tibble: 48 × 2
    ##    state location_per_thousand_miles
    ##    <chr>                       <dbl>
    ##  1 AK                        0.00301
    ##  2 AL                        0.305  
    ##  3 AR                        0.244  
    ##  4 AZ                        0.158  
    ##  5 CA                        0.342  
    ##  6 CO                        0.259  
    ##  7 CT                        1.08   
    ##  8 FL                        1.13   
    ##  9 GA                        0.690  
    ## 10 IA                        0.0711 
    ## 11 ID                        0.120  
    ## 12 IL                        0.294  
    ## 13 IN                        0.467  
    ## 14 KS                        0.109  
    ## 15 KY                        0.247  
    ## 16 LA                        0.535  
    ## 17 MA                        0.568  
    ## 18 MD                        1.05   
    ## 19 ME                        0.0283 
    ## 20 MI                        0.0414 
    ## 21 MN                        0.0805 
    ## 22 MO                        0.172  
    ## 23 MS                        0.248  
    ## 24 MT                        0.0612 
    ## 25 NC                        0.223  
    ## 26 ND                        0.0707 
    ## 27 NE                        0.0646 
    ## 28 NH                        0.214  
    ## 29 NJ                        0.573  
    ## 30 NM                        0.156  
    ## 31 NV                        0.0724 
    ## 32 NY                        0.348  
    ## 33 OH                        0.379  
    ## 34 OK                        0.415  
    ## 35 OR                        0.102  
    ## 36 PA                        0.217  
    ## 37 RI                        1.29   
    ## 38 SC                        0.250  
    ## 39 SD                        0.0259 
    ## 40 TN                        0.712  
    ## 41 TX                        0.882  
    ## 42 UT                        0.141  
    ## 43 VA                        0.327  
    ## 44 VT                        0.208  
    ## 45 WA                        0.224  
    ## 46 WI                        0.198  
    ## 47 WV                        0.124  
    ## 48 WY                        0.0307

``` r
dennys_us_with_area <- dennys_us %>% 
  count(state) %>%
  inner_join(states, by = c("state" = "abbreviation")) %>% 
  mutate(location_per_thousand_miles = 1000 * n / area)
dennys_us_with_area %>%
  select(state, location_per_thousand_miles) %>%
  print(n = Inf)
```

    ## # A tibble: 51 × 2
    ##    state location_per_thousand_miles
    ##    <chr>                       <dbl>
    ##  1 AK                        0.00451
    ##  2 AL                        0.134  
    ##  3 AR                        0.169  
    ##  4 AZ                        0.728  
    ##  5 CA                        2.46   
    ##  6 CO                        0.279  
    ##  7 CT                        2.16   
    ##  8 DC                       29.3    
    ##  9 DE                        0.402  
    ## 10 FL                        2.13   
    ## 11 GA                        0.370  
    ## 12 HI                        0.549  
    ## 13 IA                        0.0533 
    ## 14 ID                        0.132  
    ## 15 IL                        0.967  
    ## 16 IN                        1.02   
    ## 17 KS                        0.0972 
    ## 18 KY                        0.396  
    ## 19 LA                        0.0764 
    ## 20 MA                        0.758  
    ## 21 MD                        2.10   
    ## 22 ME                        0.198  
    ## 23 MI                        0.227  
    ## 24 MN                        0.173  
    ## 25 MO                        0.603  
    ## 26 MS                        0.103  
    ## 27 MT                        0.0272 
    ## 28 NC                        0.520  
    ## 29 ND                        0.0566 
    ## 30 NE                        0.0646 
    ## 31 NH                        0.321  
    ## 32 NJ                        1.15   
    ## 33 NM                        0.230  
    ## 34 NV                        0.317  
    ## 35 NY                        1.03   
    ## 36 OH                        0.982  
    ## 37 OK                        0.215  
    ## 38 OR                        0.244  
    ## 39 PA                        0.869  
    ## 40 RI                        3.24   
    ## 41 SC                        0.531  
    ## 42 SD                        0.0389 
    ## 43 TN                        0.166  
    ## 44 TX                        0.745  
    ## 45 UT                        0.318  
    ## 46 VA                        0.655  
    ## 47 VT                        0.208  
    ## 48 WA                        0.687  
    ## 49 WI                        0.382  
    ## 50 WV                        0.124  
    ## 51 WY                        0.0409

Before you move on the the next question, run the code above and take a
look at the output. In the next exercise, you will need to build on this
pipe.

Which states have the most Denny’s locations per thousand square miles?
What about La Quinta?

For Denny’s it is by far and away DC.

For La Quinta, Rhode Island is tops, followed by Florida.

Note that in general, the smallest states had the largest ratios.

### Exercise 11

Next, we put the two datasets together into a single data frame. However
before we do so, we need to add an identifier variable. We’ll call this
establishment and set the value to “Denny’s” and “La Quinta” for the dn
and lq data frames, respectively.

``` r
dennys_to_combine <- dennys_us_with_area %>%
  mutate(establishment = "Denny's")
laquinta_to_combine <- laquinta_us_with_area %>%
  mutate(establishment = "La Quinta")

dn_lq <- bind_rows(dennys_to_combine, laquinta_to_combine)
```
