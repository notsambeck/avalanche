# Avalanche!
### Generating an automated forecast for avalanche danger at Mount Hood, OR using NWAC historical forecast data.

The goal is to take this information and start to build an automated avalanche forecast based on live weather data. While I would continue to trust NWAC's human-generated forecast over any simplistic model trained on those forecasts, there may be times where the two forecasts diverge; if the automated model performs well, this information could be relevant.

Professional avalanche forecasts are based on weather data and forecasts, but also observations of avalanche activity and the snowpack; the question is how much of that information can be derived from weather data alone.

### Strategy overview:
1. Scrape weather and forecast data from NWAC and NOAA.
2. Clean data
3. Generate X: stack of multi-dimensional time series of hourly weather conditions (wind, temp, precip)
4. Attempt to predict various useful avalanche danger metrics, including:

- Existence, scale, and location (i.e. slope aspect) of hazards: storm slab, wind slab, wet-loose, (more challenging: persistent slabs and glide avalanches)
- Overall hazard rating: 1-5
- Combinations of above data

### inital prediction: existence of storm slab on test set
![first round prediction](https://github.com/notsambeck/avalanche/blob/master/early_prediction.png)
_tl means data from Timberline Lodge, m means data from Mt. Hood Meadows base_.
This model uses entirely independent predictions across different weather variables i.e. if the snow is deep, there is lots of precip, and it's windy it should predict 66% chance of storm slab even if it's 80 degrees out. It appears there is hope, but I still prefer the professional forecast at this time...

### Problems:

- Simplest one: finding a good model to classify based on multi-dimensional time series, where relevant meta-features may take place at widely varying times relative to the time being classified.
- Overfitting on a relatively small dataset (4 years, 1000 avalanche forecasts) with relatively complex (i.e. high-dimensional) data.4
- Forecasts for '1 day' can be applicable to variable timespans (i.e. a storm rolls in at 3pm and immediately starts to produce storm slab danger, but the danger was 'forecast' throughout the 24 hour period). Some of this information is available in the detailed forecast discussion, but not in a standardized, easily machine-readable way.
