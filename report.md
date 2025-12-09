# Q9: Writeup

**Phase 9:** Written Report  
**Points: 40 points**

**Focus:** Create a comprehensive written report documenting your analysis.

**Lecture Reference:** Lecture 11, Notebook 4 ([`11/demo/04_modeling_results.ipynb`](https://github.com/christopherseaman/datasci_217/blob/main/11/demo/04_modeling_results.ipynb)), Phase 9. Also see `example_report/report.md` for structure and level of detail.

---

## Objective

Create a comprehensive written report documenting your complete 9-phase data science workflow analysis.

---

## Deliverable

**File:** `report.md` - A comprehensive markdown report

**Location:** Save in the assignment root directory (same level as `q1_setup_exploration.md`, `q2_data_cleaning.md`, etc.)

**Note:** Focus on including all required sections (see below) and providing clear, comprehensive documentation. See the example report in `example_report/report.md` for structure and level of detail.

---

## Required Sections

Your report must include all of the following sections:

### 1. Executive Summary (1 paragraph)
- What dataset was analyzed
- Main goal/question
- Key finding in one sentence

### 2. Phase-by-Phase Findings
Document findings for each of the 9 phases:
- **Phase 1-2 (Q1):** Exploration findings, data quality issues
- **Phase 3 (Q2):** What was cleaned, how missing data/outliers were handled
- **Phase 4 (Q3):** Datetime parsing, temporal features extracted
- **Phase 5 (Q4):** Derived features created, rolling windows calculated
- **Phase 6 (Q5):** Trends identified, seasonal patterns, correlations
- **Phase 7 (Q6):** Train/test split approach, features selected
- **Phase 8 (Q7):** Models trained, performance metrics, feature importance
- **Phase 9 (Q8):** Final visualizations, summary of key findings

### 3. Visualizations (at least 5 figures with captions)
- Embed visualizations from your analysis
- Each figure must have:
  - Image embedded using: `![Figure N: Description](output/filename.png)`
  - Caption explaining what the figure shows
- Required visualizations:
  - At least 2 time series plots (from Q1, Q5, or Q8)
  - At least 3 additional plots (distributions, correlations, model performance, etc.)

### 4. Model Results
- Performance metrics table (use markdown table format)
- Feature importance discussion
- Model interpretation (what do R², RMSE, MAE mean in context?)
- Model comparison

### 5. Time Series Patterns
- Trends over time (increasing/decreasing/stable)
- Seasonal patterns (daily, weekly, monthly cycles)
- Temporal relationships between variables
- Any anomalies or interesting temporal features

### 6. Limitations & Next Steps
- Data quality issues that couldn't be fully addressed
- Model limitations
- Additional features that could be created
- Additional analysis that would be valuable
- How results could be validated or extended

---

## Format Requirements

### File Format
- Markdown (`.md`) with embedded images
- Professional presentation
- Error-free writing

### Image Embedding
- Save visualizations to `output/` directory
- Embed using: `![Figure 1: Description](output/figure1.png)`
- All images must have captions (either in alt text or as separate text)

### Tables
- Use markdown table format (recommended for model results)
- Example:
  ```markdown
  | Model | R² | RMSE | MAE |
  |-------|----|----|----|
  | Linear Regression | 0.XX | X.XX | X.XX |
  | Random Forest | 0.XX | X.XX | X.XX |
  ```

### Structure
- Include all required sections (see Required Sections above)
- Focus on quality over quantity
- See `example_report/report.md` for structure and level of detail

---

## Requirements Checklist

- [ ] Executive summary written (1 paragraph)
- [ ] Phase-by-phase findings documented (all 9 phases)
- [ ] At least 5 visualizations included with captions
- [ ] Model results presented (metrics, feature importance, interpretation)
- [ ] Time series patterns identified and explained
- [ ] Limitations and next steps discussed
- [ ] Professional formatting and presentation
- [ ] File saved as `report.md` in assignment root directory

---

## Grading Rubric

Your writeup will be evaluated on:

**Documentation Quality (12 points)**
- Process Explanation (4 points): Clear, step-by-step description of entire workflow
- Decision Rationale (4 points): All major decisions explained with reasoning
- Professional Presentation (4 points): Well-formatted markdown, error-free

**Visualizations & Tables (14 points)**
- Time Series Visualizations (5 points): At least 2 time series plots with clear labels
- Other Visualizations (5 points): At least 3 additional plots with appropriate choices
- Tables (2 points): Model results and key findings in well-formatted tables
- Best Practices (2 points): All visualizations have titles, axis labels, legends, captions

**Interpretation & Insights (14 points)**
- EDA Findings (5 points): Key patterns from exploration phase clearly summarized
- Time Series Patterns (5 points): Trends, seasonality, temporal relationships identified
- Model Interpretation (2 points): Model performance metrics interpreted correctly
- Limitations & Conclusions (2 points): Honest assessment of limitations and conclusions

---

## Template

See `README.md` for a detailed writeup template with example structure.

---

## Checkpoint

After Q9, you should have:
- [ ] Complete written report (`report.md`)
- [ ] All required sections included
- [ ] At least 5 visualizations with captions
- [ ] Professional formatting
- [ ] Report saved in assignment root directory

---

**Congratulations!** You've completed the full 9-phase data science workflow. Review the submission checklist in `README.md` before submitting.


## Executive Summary

This analysis examines weather sensor data from Chicago beaches along Lake Michigan, covering 196,321 hourly measurements from April 2015 to December 2025 across three weather stations. The project follows a complete 9-phase data science workflow to understand temporal patterns in beach weather conditions and build predictive models for air temperature. Key findings include strong seasonal temperature patterns, significant daily cycles, and successful prediction models. The XGBoost model emerged as the best performer, with a test R² of 0.9154 and RMSE of 2.911°C, demonstrating that air temperature can be predicted with good accuracy from temporal features, rolling windows of predictor variables, and weather variables.

## Phase-by-Phase Findings

### Phase 1-2: Exploration

Initial exploration revealed a dataset of **196,321 records** with 18 columns including temperature measurements (air and wet bulb), wind speed and direction, humidity, precipitation, barometric pressure, solar radiation, and sensor metadata. The data spans from April 25, 2015 to December 03, 2025, with measurements from three different weather stations: 63rd Street Weather Station, Foster Weather Station, and Oak Street Weather Station.

**Key Data Quality Issues Identified:**
- Approximately 75 missing values in Air Temperature (0.04%)
- Approximately 146 missing values in Barometric Pressure (0.07%)
- Approximately 75,951 missing values in Wet Bulb Temperature (38.7%) - significant portion of data
- Missing values in Rain Intensity, Total Rain, Precipitation Type, and Heading (same 75,951 records)
- The most outlier detected in Air Temperature, Wet Bulb Temperature, and Barometric Pressure 
- Data collected at hourly intervals with some gaps

Initial visualizations showed:
- Air temperature generally ranging from approximately -10°C to 35°C
- Clear seasonal patterns visible in temperature data, hourly
- Wind direction following a distribution with most data showing winds going Southeast (~70 degrees) and West (270 degrees)

![Figure 1: Initial Data Exploration](output/q1_visualizations.png)
*Figure 1: Initial exploration visualizations showing distributions of air temperature, air temperature time series and wind direction distribution.*

### Phase 3: Data Cleaning

Data cleaning addressed missing values, outliers, and data type validation. Missing values in numeric columns were handled using forward-fill (appropriate for time series data) followed by backward fill and then median imputation for any remaining gaps. This approach preserved temporal continuity while ensuring complete datasets for modeling.

**Cleaning Results:**
- Rows before cleaning: **196,321**
- Missing values: Forward-filled and median-imputed
  - Air Temperature: 75 missing → 0 missing
  - Wet Bulb Temperature: 75,626 missing → 0 missing (large gap, likely sensor-specific)
  - Barometric Pressure: 146 missing → 0 missing
- Outliers: Capped using percentile method (Percentile 0.1th and 99th bounds)
  - Wet Bulb Temperature: 390 outliers capped (bounds: [-19.47, 26.90])
- Duplicates: Removed (0 duplicates found)
- Data types: Validated and converted as needed
- Rows after cleaning: **196,321** (no rows removed, only values cleaned)

The cleaning process maintained the full dataset size while improving data quality. The large number of missing values in Wet Bulb Temperature (38.7%) suggests that this sensor may not be available at all stations or during certain periods, but forward-fill and median imputation ensured we could still use this feature in analysis.

### Phase 4: Data Wrangling

Datetime parsing and temporal feature extraction were critical for time series analysis. The `Measurement Timestamp` column was parsed from the format "MM/DD/YYYY HH:MM:SS AM/PM" and set as the DataFrame index, enabling time-based operations.

**Temporal Features Extracted:**
- `hour`: Hour of day (0-23)
- `day_of_week`: Day of week (0=Monday, 6=Sunday)
- `month`: Month of year (1-12)
- `year`: Year
- `day_name`: Day name (Monday-Sunday)
- `is_weekend`: Binary indicator (1 if Saturday/Sunday)

The dataset covers approximately 10.6 years of hourly measurements (April 2015 to December 2025), providing substantial data for robust temporal analysis. After removing rows with invalid datetime values, **196,321 records** remained with valid temporal features.

### Phase 5: Feature Engineering

Feature engineering created derived variables and rolling window statistics to capture relationships and temporal dependencies.

**Derived Features:**
- `wind_speed_squared`: Non-linear wind effect
- Note: Features derived from the target variable (e.g., `air_temp_diff_1h`, `humidity_temp_ratio`) were created during feature engineering but excluded from modeling to avoid data leakage.

**Rolling Window Features:**
- `humidity_rolling_24h`: 24-hour rolling mean of humidity
- `solar_rolling_24h`: 24-hour rolling mean of solar radiation

**Important:** Only rolling windows of predictor variables were created, not the target variable. Creating rolling windows of the target variable (e.g., `air_temp_rolling_7h` when predicting Air Temperature) would cause data leakage. The rolling window features of predictor variables capture temporal dependencies essential for time series prediction.

### Phase 6: Pattern Analysis

Pattern analysis revealed several important temporal and correlational patterns:

**Temporal Trends:**
- Clear seasonal patterns: Air temperatures peak in summer months and reach minima in winter
- Monthly air temperature range: -2.6°C to 23.6°C
- Strong seasonal variation typical of Chicago's climate

**Daily Patterns:**
- Strong diurnal cycle in air temperature (warmer during day, cooler at night)
- Peak air temperature typically occurs around hour 16 (4 PM)
- Minimum air temperature typically occurs around hour 6 (6 AM)
- This pattern reflects solar heating and cooling cycles

**Correlations:**
 - Air temp vs Wet Bulb Temp: 0.830 (strong positive correlation, air temp may be affecting the temperature of the wet bulb consistently) 
 - Maximum wind speed vs wind speed squared: (moderate positive correlation)

![Figure 2: Pattern Analysis](output/q5_patterns.png)
*Figure 2: Advanced pattern analysis showing monthly temperature trends, seasonal patterns by month, daily patterns by hour, and correlation heatmap of key variables.*

### Phase 7: Modeling Preparation

Modeling preparation involved selecting a target variable, performing temporal train/test splitting, and preparing features. Air temperature was chosen as the target variable, as it's a key indicator of beach conditions and shows predictable patterns.

**Temporal Train/Test Split:**
- Split method: Temporal (80/20 split by time, NOT random)
- Training set: **157,055 samples** (earlier data: April 2015 to ~July 2023)
- Test set: **39,266 samples** (later data: ~July 2023 to December 2025)
- Rationale: Time series data requires temporal splitting to avoid data leakage and ensure realistic evaluation

**Feature Preparation:**
- Features selected (excluding target, non-numeric columns, and features derived from target)
- **Critical:** Excluded features derived from target variable:
  - `air_temp_diff_1h` (uses Air Temperature)
- Categorical variables (Station Name, wind_category) dropped
- All features standardized and missing values handled
- Infinite values replaced with NaN then filled with median
- No data leakage: future data excluded from training set, and features derived from target excluded
- Total dataset: **196,321 rows** before split

### Phase 8: Modeling

Two models were trained and evaluated: Linear Regression and XGBoost (as suggested in the assignment).

**Model Performance:**

| Model | R² Score | RMSE | MAE |
|-------|----------|------|-----|
| Linear Regression | 0.917 | 2.918°C | 1.419°C |
| XGBoost | 0.923 | 2.815°C | 1.228°C |

**Key Findings:**
- Linear Regression achieved good performance (R² = 0.917), indicating that linear relationships alone are sufficient for accurate temperature prediction
- XGBoost achieved strong performance (R² = 0.923), demonstrating the importance of non-linear modeling and gradient boosting methods
- XGBoost significantly outperforms Linear Regression, with RMSE of 2.815°C compared to 2.918°C

**Feature Importance (XGBoost):**
Top features by importance:
1. `Wet Bulb Temperature` (80.94% importance) - by far the most important, capturing seasonal patterns
2. `Solar Radiation` (8.61% importance)
3. `Wind Speed` (5.53% importance)

The Wet Bulb Temperatre feature dominates feature importance, accounting for 80.94% of total importance. This makes intuitive sense - something directly affected by air temperature strongest predictor of air temperature. Temporal features (month, year) and weather variables (rain, pressure, humidity) are more important than rolling windows of predictor variables. The top 3 features account for 95.08% of total importance.

![Figure 3: Model Performance](output/q8_final_visualizations.png)
*Figure 3: Final visualizations showing model performance comparison, predictions vs actual values, feature importance, and residuals plot for the best-performing XGBoost model.*

### Phase 9: Results

The final results demonstrate successful prediction of air temperature with good accuracy. The XGBoost model achieves strong performance on the test set, with predictions within 2.815°C on average.

**Summary of Key Findings:**
1. **Model Performance:** XGBoost achieves R² = 0.923, indicating that 92.3% of variance in air temperature can be explained by the features
2. **Feature Importance:** The Wet Bulb Temperature feature is overwhelmingly the most important predictor (80.94% importance), highlighting the critical role of water temperature patterns
3. **Temporal Patterns:** Strong seasonal and daily patterns are critical for accurate prediction
4. **Data Quality:** Cleaning process maintained full dataset while improving reliability
5. **Data Leakage Avoidance:** By excluding features derived from the target variable, we achieved realistic and generalizable model performance

The residuals plot shows relatively uniform distribution around zero, suggesting the model performs reasonably well across the full temperature range. The predictions vs actual scatter plot shows points distributed around the perfect prediction line with some scatter, indicating good but not perfect accuracy - which is realistic for weather prediction.

## Visualizations

![Figure 1: Initial Data Exploration](output/q1_visualizations.png)
*Figure 1: Initial exploration showing distributions and time series of key variables.*

![Figure 2: Pattern Analysis](output/q5_patterns.png)
*Figure 2: Advanced pattern analysis revealing temporal trends, seasonal patterns, daily cycles, and correlations.*

![Figure 3-6: Pattern Analysis](output/q8_temporal_patterns.png)
*Figure 3-6: Basic pattern analysis revealing daily/hourly temporal trends, monthly/yearly patterns, daily cycles, and daily rain patterns.*

![Figure 5: Model Performance](output/q8_final_visualizations.png)
*Figure 5: Final results showing model comparison, prediction accuracy, feature importance, and residual analysis.*

## Model Results

The modeling phase successfully built predictive models for air temperature. The performance metrics demonstrate that XGBoost performs well, while Linear Regression shows that linear relationships alone are insufficient for this task.

**Performance Interpretation:**
- **R² Score:** Measures proportion of variance explained. XGBoost's R² of 0.923 means the model explains 80.94% of variance in air temperature - a strong but realistic result.
- **RMSE (Root Mean Squared Error):** Average prediction error in original units. XGBoost's RMSE of 2.815°C means predictions are typically within 2.815°C of actual values - reasonable for weather prediction.
- **MAE (Mean Absolute Error):** Average absolute prediction error. XGBoost's MAE of 1.228°C indicates good predictive accuracy.

**Model Selection:** XGBoost is selected as the best model due to:
1. Highest R² score (0.923)
2. Lowest RMSE (2.815°C)
3. Lowest MAE (1.228°C)
4. Good generalization (train R² = 0.8164, test R² = 0.9227 - some overfitting but reasonable)

**Feature Importance Insights:**
The feature importance analysis reveals that:
- The Wet Bulb Temperature feature is overwhelmingly the most important predictor (80.94% importance)
- This suggests that physical indicators are the strongest predictor of air temperature
- Weather variables (Total Rain, Barometric Pressure, Humidity) are important but secondary to temporal patterns
- Rolling windows of predictor variables (humidity, pressure, wind speed) contribute but are less important than physical features
- Temporal features (month, year) are more important than static weather variables
- Station location has minimal impact (encoded station features have very low importance)

**Note on Data Leakage Avoidance:** By excluding features derived from the target variable (air_temp_diff_1h, humidity_temp_ratio, wind_speed_squared), we achieved realistic model performance. This demonstrates the importance of careful feature selection to avoid circular logic.

## Time Series Patterns

The analysis revealed several important temporal patterns:

**Long-term Trends:**
- Stable long-term trends over the 10.62-year period
- No significant increasing or decreasing trends (data appears stationary after accounting for seasonality)
- Consistent seasonal cycles year over year

**Seasonal Patterns:**
- **Monthly:** Clear seasonal cycle with temperatures peaking in summer months (June-August) and reaching minima in winter months (December-February)
- Monthly air temperature range: -2.6°C to 23.6°C
- **Daily:** Strong diurnal cycle with temperatures peaking in afternoon (4 PM, hour 16) and reaching minima in early morning (6 AM, hour 6)
- Daily patterns are consistent across seasons, though amplitude varies

**Temporal Relationships:**
- Air temperature shows strong seasonal patterns (month is the most important predictor)
- Wind speed shows moderate negative correlation with temperature (-0.230)
- Humidity shows very weak correlation with temperature (0.01)
- Rolling windows of predictor variables (wind speed, humidity, pressure) capture temporal dependencies

**Anomalies:**
- Large gap in Wet Bulb Temperature data (75,951 missing values, 38.6% of dataset)
- This likely represents periods when certain sensors were not operational
- Some sensor dropouts identified (gaps in time series)
- No major anomalies in temporal patterns beyond expected seasonal variation

These temporal patterns are critical for accurate prediction, as evidenced by the high importance of temporal features (especially rolling windows) in the model.

## Limitations & Next Steps

**Limitations:**

1. **Data Quality:**
   - Large number of missing values in Wet Bulb Temperature (38.6%) required imputation, which may introduce bias
   - Sensor dropouts create gaps in time series that could affect pattern detection
   - Outlier capping may have removed some valid extreme events
   - Only 3 weather stations - limited spatial coverage

2. **Model Limitations:**
   - Linear Regression's moderate performance (R² = 0.917) indicates that linear relationships are insufficient for this task
   - XGBoost shows some overfitting (train R² = 0.8164 vs test R² = 0.923), though this is reasonable
   - Model relies heavily on physical features (wet bulb temperature = 80.94% importance), which limits predictive power for same-season predictions
   - Model trained on historical data may not generalize to future climate conditions
   - RMSE of 2.815°C, while reasonable, may not be sufficient for applications requiring high precision

3. **Feature Engineering:**
   - Some potentially useful features may not have been created (e.g., lag features, interaction terms)
   - Rolling window sizes (7h, 24h) were chosen somewhat arbitrarily
   - Features derived from target variable were correctly excluded to avoid data leakage
   - External data (e.g., weather forecasts, lake conditions) not incorporated

4. **Scope:**
   - Analysis focused on air temperature prediction; other targets (e.g., wind speed, precipitation) not explored
   - Only one target variable analyzed; multi-target modeling could provide additional insights
   - Spatial relationships between stations not analyzed

**Next Steps:**

1. **Model Improvement:**
   - Experiment with different rolling window sizes and lag features
   - Try additional models (e.g., XGBoost, Gradient Boosting) to potentially improve performance
   - Incorporate external data sources (weather forecasts, lake level data)
   - Try ensemble methods combining multiple models
   - Validate model on truly out-of-sample data (future dates)
   - Address overfitting in XGBoost (train/test gap suggests some overfitting)

2. **Feature Engineering:**
   - Create interaction features between key variables
   - Add lag features (previous hour/day values) explicitly
   - Incorporate spatial features (distance between stations, station-specific effects)
   - Create weather condition categories

3. **Analysis Extension:**
   - Predict other targets (wind speed, precipitation, humidity)
   - Analyze station-specific patterns and differences
   - Investigate sensor reliability and data quality by location
   - Build forecasting models for future predictions
   - Analyze spatial relationships between stations

4. **Validation:**
   - Cross-validation with temporal splits
   - Validation on additional time periods
   - Comparison with physical models (if available)
   - Sensitivity analysis on feature importance
   - Further investigation of feature engineering to improve Linear Regression performance

5. **Deployment:**
   - Real-time prediction system
   - Alert system for extreme conditions
   - Dashboard for beach managers
   - Integration with weather forecasting systems

## Conclusion

This analysis successfully applied a complete 9-phase data science workflow to Chicago Beach Weather Sensors data, achieving good air temperature predictions (R² = 0.923, RMSE = 2.815°C). The project demonstrated the importance of temporal feature engineering, particularly seasonal features (month), which dominated feature importance. Key insights include strong seasonal and daily patterns, the critical role of temporal features in prediction, and the superior performance of ensemble tree-based models over linear models. The analysis demonstrates proper data leakage avoidance by excluding features derived from the target variable, resulting in realistic and generalizable model performance. This provides a solid foundation for beach condition monitoring and prediction systems.


