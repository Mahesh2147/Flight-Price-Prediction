 # Flight-Price-Prediction

## 🧭 Project Overview

The goal of this project is to predict flight prices based on various flight-related factors such as airline, source, destination, duration, number of stops, and departure/arrival times.
The problem is treated as a supervised regression task using machine learning algorithms.

## 📂 Dataset Details

The dataset used is typically named Flight_Fare.xlsx, containing 10,683 rows and 11 columns:
```
ColumnName	                      Description

Airline		             Name of the airline (e.g., Jet Airways, IndiGo, Air India)

Date_of_Journey          Date when the flight takes off

Source	                 City of origin
      
Destination              City of arrival

Route                    Full route of the flight (e.g., DEL → BOM → BLR)

Dep_Time                 Departure time

Arrival_Time             Arrival time

Duration	             Total travel time

Total_Stops	             Number of stops (e.g., 0, 1, 2+)

Additional_Info	         Other info such as meal, baggage, etc.

Price	                 Target variable (ticket price)
```
## 🧹 Data Preprocessing
1️⃣ Handling Missing Values

* Only the Total_Stops column had 1 missing value.

* It was replaced with the most frequent value (‘1’)

* All other columns were complete after that.

2️⃣ Date Conversion

* The Date_of_Journey column was split into:

    * Journey_date

    * Journey_month

    * (Year was dropped because all records were from 2019)

3️⃣ Time Conversion

* Dep_Time → Extracted into Dep_hour and Dep_min

* Arrival_Time → Extracted into Arrival_hour and Arrival_min

* Original string columns (Dep_Time, Arrival_Time) were dropped.

4️⃣ Duration Conversion

* Duration values like “2h 50m” or “5h” were converted to total minutes.

   * Duration_hr and Duration_min were extracted, then combined:
     Duration = Duration_hr*60 + Duration_min

* The temporary columns were deleted.

5️⃣ Categorical Encoding

* Label encoding was used for:

  * Airline

  * Source

  * Destination

* No one-hot encoding or scaling was applied since tree-based models can handle integer-coded categories.

6️⃣ Dropping Unnecessary Columns

* Route, Additional_Info, and Date_of_Journey were dropped (low or redundant info).

After cleaning, the final dataset contained only numeric columns suitable for modeling.

## 🧠 Feature Engineering Summary

Final feature list used for modeling:
```
   ['Airline', 'Source', 'Destination', 'Total_Stops', 
    'Journey_date', 'Journey_month', 'Dep_hour', 'Dep_min', 
    'Arrival_hour', 'Arrival_min', 'Duration']
```
## ⚙️ Model Building

The dataset was split into:

* X (features) → All columns except Price

* y (target) → Price

* Train-test split: 80% training, 20% testing (random_state=42)

 **Models Trained**

* Linear Regression

* Decision Tree Regressor

* Random Forest Regressor

Each model was trained and evaluated using standard regression metrics.

## 📊 Model Evaluation (Before Tuning)
```
Model	              R² Score	    MAE	       MSE

Linear Regression	  0.43	       2472.6	   12,069,000

Decision Tree	      0.60       	1416.8	   8,396,700

Random Forest	      0.79       	1198.5	   4,274,800
```
➡️ Random Forest performed best with the **highest R² (0.79)**  and lowest errors.

## 🧩 Hyperparameter Tuning

The Random Forest model was further tuned using RandomizedSearchCV.

Key parameters optimized:

* n_estimators (number of trees)

* max_depth

* min_samples_split

* min_samples_leaf

* max_features

* bootstrap

**Best Parameters Found**

Example best configuration:
```
  n_estimators = 400
 
  max_depth = 30
  
  min_samples_split = 5
  
  max_features = 'sqrt'
```
**Tuned Model Performance**
```
Metric	        Score

R² Score	   0.821

MAE	           1212.4

MSE	           3,783,600

RMSE	       1945.6
```
## 📈 Observations

* Random Forest Regressor gave the best overall performance with R² ≈ 0.82.

* The model successfully captured non-linear relationships between features and price.

* The features most strongly influencing price include:

  * Airline

  * Duration

  * Total Stops

  * Departure/Arrival times

## 🧾 Final Conclusion

✅ The project effectively predicts flight ticket prices using machine learning.

✅ Random Forest model achieved ~82% accuracy (R² = 0.821).

✅ Proper data preprocessing (feature extraction + encoding) greatly improved model performance.


