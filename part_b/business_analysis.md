# B1. Problem Formulation

---

## (a) Machine Learning Problem

**Target Variable:**
items_sold

**Input Features:**

* transaction_date
* store_id
* store_size
* location_type
* promotion_type
* is_weekend
* is_festival
* competition_density

**Type of Problem:**
Regression

**Justification:**
We are predicting a numeric value (items_sold), so this is a regression problem.

---

## (b) Why Items Sold is Better than Revenue

Revenue is affected by pricing, discounts, and product cost, so it is not stable.

Items_sold directly represents customer demand and is more reliable.

**Conclusion:**
Items_sold is a better target variable.

**Principle:**
Always choose a target variable that is stable and directly related to the business goal.

---

## (c) Better Modeling Strategy

Using one global model for all stores is not ideal because different stores behave differently.

**Better Approach:**

* Use store features like location_type and store_size
* Or create separate models for different store types

**Conclusion:**
Model should consider store differences to improve accuracy.

# B2. Data and EDA Strategy

---

## (a) Data Joining and Dataset Design

The data comes from multiple tables:

* Transactions
* Store attributes
* Promotion details
* Calendar data

**How to join:**

* Join transactions with store attributes using `store_id`
* Join with promotions using `promotion_type`
* Join with calendar using `transaction_date`

**Final dataset (grain):**
One row = one store per day

**Aggregations:**

* Total items_sold per store per day
* Count of promotions
* Average competition density

---

## (b) EDA (Exploratory Data Analysis)

1. **Sales Distribution Plot**

   * To check how items_sold is distributed
   * Helps identify skewness or outliers

2. **Sales by Promotion Type**

   * Compare which promotion performs best
   * Helps in feature importance

3. **Sales by Store Type**

   * Compare urban vs rural stores
   * Helps understand store behavior

4. **Correlation Heatmap**

   * Check relationship between features
   * Helps remove unnecessary features

---

## (c) Handling Imbalance in Promotions

**Problem:**
80% transactions have no promotion → data imbalance

**Impact:**

* Model may ignore promotion effect
* Bias towards "no promotion"

**Solution:**

* Use balanced sampling
* Give weight to promotion data
* Analyze promotion separately

---

# B3. Model Evaluation and Deployment

---

## (a) Train-Test Split and Metrics

**Split Strategy:**

* Use time-based split (not random)
* Train on past data, test on recent data

**Why not random split?**

* Data is time-based
* Random split causes data leakage

**Metrics:**

* RMSE → measures error size
* MAE → measures average error

---

## (b) Explaining Model Decisions

Use feature importance to understand:

* Why different promotions are suggested

Example:

* December → festival → loyalty bonus works better
* March → normal month → flat discount works better

---

## (c) Deployment Strategy

**Steps:**

1. Save model using pickle
2. Load model for predictions
3. Input new monthly data
4. Generate recommendations

**Monitoring:**

* Track model accuracy
* Check prediction errors
* Retrain if performance drops
