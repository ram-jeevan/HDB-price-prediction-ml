# 🏠 HDB Resale Price Prediction Model

**Predicting Singapore HDB Resale Flat Prices using Machine Learning**

> A data-driven predictive model built to predict how 6 key factors impact HDB resale price estimates across Singapore.

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Key Features](#-key-features)
- [Model Performance](#-model-performance)
- [Installation](#-installation)
- [Usage](#-usage)
- [The 6 Key Price Drivers](#-the-6-key-price-drivers)
- [Project Structure](#-project-structure)
- [Methodology](#-methodology)
- [Results & Insights](#-results--insights)
- [Limitations](#-limitations)
- [Future Enhancements](#-future-enhancements)

---

## 🎯 Project Overview

This project analyses **150,000+ HDB resale transactions** to build a predictive model that helps real estate agents provide accurate, data-backed pricing recommendations to their clients.

### Business Problem

Singapore's HDB resale market has become increasingly competitive, with volatile pricing trends. Real estate agents need reliable tools to:
- Offer competitive pricing advice for buyers and sellers
- Accurately estimate HDB flat values across different locations and flat types
- Justify pricing recommendations with transparent, data-driven evidence

### Solution

We developed a **Linear Regression model** that explains **78% of price variation** and reduces prediction error by **53%** compared to baseline estimates.

---

## ✨ Key Features

- **78% Accuracy (R² = 0.78)** - Model explains 78% of what makes flats more or less expensive
- **$67,634 Average Error (RMSE)** - Typical prediction within ~$68k of actual price
- **53% Improvement** - Reduces error from $142k (baseline) to $67k
- **6 Core Predictors** - Clear, interpretable factors driving HDB prices
- **Regional Analysis** - Accounts for location premiums across 5 major regions
- **Production-Ready** - Clean, documented code ready for deployment

---

## 📊 Model Performance

| Model | RMSE | R² Score | Features |
|-------|------|----------|----------|
| **Baseline (Mean)** | $142,801 | 0.00 | - |
| **Linear Regression** | $67,634 | 0.78 | 9 features |
| **Ridge Regression** | $69,149 | 0.77 | 12 features |

**Translation:** For a flat worth $450,000, our model typically predicts between $382,000 and $518,000.

---

## 🚀 Installation

### Prerequisites

```bash
Python 3.8+
pandas
numpy
scikit-learn
matplotlib
seaborn
```

### Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/hdb-price-prediction.git
cd hdb-price-prediction

# Install dependencies
pip install -r requirements.txt

# Run the model
python hdb_model_clean.py
```

---

## 💻 Usage

### Basic Prediction

```python
from hdb_predictor import HDBPricePredictor

# Initialize the model
predictor = HDBPricePredictor()

# Make a prediction
flat_details = {
    'floor_area_sqm': 90,
    'hdb_age': 25,
    'mid_storey': 8,
    'flat_type': '4 ROOM',
    'region': 'CENTRAL',
    'mrt_nearest_distance': 400
}

predicted_price = predictor.predict(flat_details)
print(f"Estimated Price: ${predicted_price:,.2f}")
```

### Three Use Cases

#### 1. **Pricing a Listing**
```python
# Agent needs to price a Tampines flat near MRT
prediction = predictor.predict(tampines_flat)
# Output: $465,000 (with MRT premium)
```

#### 2. **Advising a Buyer**
```python
# Compare Bishan ($550k) vs Jurong ($420k)
bishan_prediction = predictor.predict(bishan_flat)  # $545k
jurong_prediction = predictor.predict(jurong_flat)  # $425k
# Both fairly priced; $125k difference due to central location premium
```

#### 3. **Negotiation Support**
```python
# Seller asking $500k, buyer thinks too high
prediction = predictor.predict(flat_details)  # $470k
# Overpriced by $30k - buyer is right!
```

---

## 🔑 The 6 Key Price Drivers

### 1. 🏗️ Floor Area (sqm)
**Impact:** Strongest predictor  
**Why:** Larger flats consistently command higher prices  
**Example:** 10 sqm difference = $20k-$30k price change

### 2. 📅 HDB Age
**Impact:** Depreciation factor  
**Why:** Newer flats have more lease years remaining, modern amenities  
**Note:** Location can offset age entirely (older central flats hold value)

### 3. 🏢 Floor Level (Storey)
**Impact:** Height premium  
**Why:** Better views, less noise, more privacy  
**Example:** High floor vs low floor = ~$270k difference

### 4. 🏘️ Flat Type
**Impact:** Major price determinant  
**Why:** Reflects family size needs and space requirements  
**Range:** 
- 2-room: $200k-$300k
- 3-room: $300k-$420k
- 4-room: $400k-$700k
- 5-room: $500k-$930k
- Executive: $500k-$950k

### 5. 📍 Regional Location
**Impact:** Up to $100k+ premium for Central  
**Ranking by Average Price:**
1. Central: $500,000
2. North-East: $460,000
3. East: $460,000
4. West: $420,000
5. North: $400,000

### 6. 🚇 MRT Proximity
**Impact:** Convenience premium  
**Why:** Better connectivity, rental potential  
**Example:** 300m vs 1km from MRT = ~$40k premium

---

## 📁 Project Structure

```
hdb-price-prediction/
│
├── data/
│   ├── train.csv              # Training dataset (150,000+ transactions)
│   └── test.csv               # Test dataset for predictions
│
├── notebooks/
│   ├── 01_EDA.ipynb          # Exploratory Data Analysis
│   ├── 02_Feature_Engineering.ipynb
│   └── 03_Model_Building.ipynb
│
├── src/
│   ├── hdb_model_clean.py    # Production-ready model script
│   ├── predictor.py          # Prediction wrapper class
│   └── utils.py              # Helper functions
│
├── models/
│   └── linear_regression_model.pkl  # Trained model
│
├── visualisations/
│   ├── feature_importance.png
│   ├── actual_vs_predicted.png
│   └── regional_price_map.png
│
├── docs/
│   ├── WOW_Presentation_Narrative.md
│   ├── WOW_Executive_Summary.md
│   └── Technical_Documentation.md
│
├── requirements.txt
├── README.md
└── LICENSE
```

---

## 🔬 Methodology

### 1. Data Collection & Preparation
- **Source:** 150,000+ HDB resale transactions
- **Features:** 77 original columns → 43 relevant features selected
- **Target:** Resale price in Singapore dollars

### 2. Feature Engineering

**Created Features:**
- **Regional Categories:** Grouped 27 towns into 5 regions (Central, East, West, North, North-East)
- **Flat Type Numeric:** Converted categorical flat types to ordinal scale (1-7)
- **Floor Mid-Point:** Calculated middle of storey range for standardisation

**Handled Missing Data:**
- Filled `Mall_Nearest_Distance` with median values
- Logical imputation for amenity proximity indicators

### 3. Model Selection

**Why Linear Regression?**
✅ **Transparency:** Every prediction is fully explainable  
✅ **Interpretability:** Clear coefficient for each feature  
✅ **Reliability:** Proven method in real estate valuation  
✅ **Speed:** Fast predictions for production use

### 4. Validation Strategy
- Train/Validation split: 80/20
- 5-fold cross-validation for robust performance estimates
- Comparison against baseline (predicting mean)

---

## 📈 Results & Insights

### Model Performance
- **RMSE:** $67,634 (53% better than baseline)
- **R² Score:** 0.78 (explains 78% of price variation)
- **MAE:** ~$50,000 average absolute error

### Key Business Insights

#### 1. Location Premium is Real
Central region flats command premiums of **$100,000+** over comparable outer-region properties.

#### 2. Size Always Matters
Floor area shows the **strongest correlation** with price. Even small size advantages (5-10 sqm) justify meaningful premiums.

#### 3. Height Commands Premium
High-floor flats (21+) average **$270,000 more** than low-floor flats (1-9), reflecting demand for views and privacy.

#### 4. MRT Accessibility Adds Value
Flats within 300m of MRT stations command **~$40,000 premium** over those 1km away.

#### 5. Age Can Be Offset
While newer flats typically command premiums, **well-located older flats in central areas hold strong value**.

---

## ⚠️ Limitations

### What the Model Doesn't Capture

1. **Unit-Specific Factors**
   - Renovation condition
   - Specific view quality (park vs highway)
   - Corner unit premiums
   - Unique layout features

2. **External Economic Factors**
   - Inflation over time
   - Economic recessions
   - Interest rate changes
   - Market sentiment

3. **Government Policies**
   - Cooling measures (post-2021)
   - MOP policy changes
   - Cash Over Valuation (COV) policies

4. **Subjective Elements**
   - Neighborhood character
   - Timing of purchase
   - Negotiation dynamics
   - Buyer urgency

### Data Timeframe
- Transactions: 2012-2021
- May not reflect post-COVID market dynamics
- Recent policy changes (2021+) not captured

---

## 🚀 Future Enhancements

### Model Improvements
- [ ] Incorporate time-series components for market trends
- [ ] Add interaction terms (e.g., region × flat type)
- [ ] Test ensemble methods (Random Forest, XGBoost)
- [ ] Implement prediction intervals for uncertainty quantification

### Feature Additions
- [ ] School quality metrics (PSLE rankings)
- [ ] Renovation cost proxies
- [ ] Distance to CBD/major employment hubs
- [ ] Nearby property development indicators

### Deployment
- [ ] REST API for real-time predictions
- [ ] Web interface for agent dashboard
- [ ] Mobile app integration
- [ ] Automated monthly model retraining

### Analysis Extensions
- [ ] Segment-specific models (by region or flat type)
- [ ] Price trend analysis over time
- [ ] Comparative market analysis tools
- [ ] ROI calculators for buyers

---

## 📚 Additional Resources

### Documentation
- [Full Presentation Narrative](docs/WOW_Presentation_Narrative.md)
- [Executive Summary](docs/WOW_Executive_Summary.md)
- [Technical Documentation](docs/Technical_Documentation.md)

### Related Links
- [HDB Resale Statistics](https://data.gov.sg/dataset/resale-flat-prices)
- [Singapore Planning Areas](https://en.wikipedia.org/wiki/Planning_Areas_of_Singapore)
- [Real Estate Valuation Methods](https://www.investopedia.com/terms/r/real-estate-valuation.asp)

---

## 🙏 Acknowledgements

- **Data.gov.sg** for providing comprehensive HDB transaction data
- **General Assembly** for guidance and bootcamp structure
- **Singapore HDB** for making resale data publicly available

---

<div align="center">

**Made with ❤️ by the DABPT03 Data Analytics Team**

*Empowering real estate agents with data-driven insights*

</div>
