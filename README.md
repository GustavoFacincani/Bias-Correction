# Bias-Correction
Bias correction using the hyfo package in R (https://cran.r-project.org/web/packages/hyfo/hyfo.pdf)

The bias correction will use the getBiasFactor() function to get the bias factors for correcting the simulated data, it can be done in different
scales, in this case we are getting monthly bias factors. The inputs are observed and simulated dataframes, with the same lenght, a first column
with dates, and a second column with streamflow. Then, the bias factors are applied to get the whole simulated data using the applyBiasFactor() 
function, using as arguments, the bias factors and the simulated data only. Using these two functions can return random errors about the format
of the data, asking the columns to be read as date and numeric/dbl, even when they're already in this format, inputting them using as.data.frame(),
solves the problem.
The hyfo package offers different methods for bias correction, including:
- delta: This method adds to the observations the mean change signal. It should be avoided to bounded variables as it can produce values out of the 
variable range (e.g., negative streamflows).
- scaling: The data is corrected by scaling the simulation with the difference (additive) or quotient (multiplicative) between the observed and 
simulated means in the train period. The scaleType argument can be "multi" or "add", so that the bias factors can be derived for multiplying the 
simulated data or added to the simulated data. he problem identified with the "multi" option is that when flows are low in the simulated data, the 
bias factor can be 5-7 (increasing the flows in 5-7 times), and that causes higher flows in that period to be overescalated. The option "add" 
doesn't cause this problem.
- eqm (empirical quantile mapping): this method is applicable to any variable, as it's used to calibrate the simulated Cumulative Distribution Function 
(CDF) by adding to the observed quantiles, both the mean delta change and the individual delta changes, in the corresponding quantiles. The extrapolate 
argument can be set to "no", so that the simulated data doesn't surpass the limits found in the observed data, bouding it to the range of observed, not
producing biased extremes. It requires an extra argument ("obs") when applying the bias factor. The "preci" argument needs to be set to "FALSE" when using 
this method to variables other than precipitation.
- gqm (gama quantile mapping): used only for precipitation.


Bias correction is an active area of research; a variety of techniques have been examined, ranging from simple scaling to more complex distribution mapping 
methods (Cooper, 2019). Bias corrected results can vary by bias correction technique, model, climate output (Miralha et al., 2021), or even study area (Coper,
2019). Therefore, it is recommended that bias correction methods be fully documented and results from pre- and post- correction presented (Coper, 2019).
In this case, for correcting streamflow data, the eqm method provided the best results According to Mendez et al. (2020), the quantile mapping approach 
corrects the distribution of the simulated data, so that the variability of corrected data is more consistent with the observed. 
The authors used this approach to bias correct precipitation data, stating that it non-linearly corrects the mean, standard deviation (variance), quantiles, 
wet frequencies and intensities preserving the extremes, outperforming methods such as linear scaling, power transformation of precipitation, gamma quantile 
mapping and gamma-pareto quantile mapping. This method adjusts 99 percentiles and linearly interpolates inside this range every two consecutive percentiles
(Miralha et al., 2021). Mishra et al. (2020) also used the eqm method to bias correct historical and future simulations of precipitation, minimum and maximum
temperatures at the daily time scale.


Cooper, R. T. (2019). Projection of future precipitation extremes across the Bangkok Metropolitan Region. Heliyon, 5(5), e01678.
Mendez, M., Maathuis, B., Hein-Griggs, D., & Alvarado-Gamboa, L. F. (2020). Performance evaluation of bias correction methods for climate change monthly precipitation projections over Costa Rica. Water, 12(2), 482.
Miralha, L., Muenich, R. L., Scavia, D., Wells, K., Steiner, A. L., Kalcic, M., ... & Kirchhoff, C. J. (2021). Bias correction of climate model outputs influences watershed model nutrient load predictions. Science of The Total Environment, 759, 143039.
