# LP Score v4 Validation & Stress-Test Report

**Analysis Date:** August 2025  
**Dataset:** 7,532 wallets, 396 columns  
**Objective:** Validate aggregated_lp_score derivation and identify anomalies

---

## Executive Summary: Top 10 Key Findings

### 🔍 **Construction & Formula Analysis**

1. **Complete Aggregation Mismatch**: **0% exact matches** between `aggregated_lp_score` and sum of per-pool `total_score` values, indicating a fundamentally different aggregation methodology than simple summation.

2. **Moderate Aggregation Correlation**: Correlation of 0.629 between aggregated scores and pool score sums suggests some relationship exists, but with significant systematic transformation.

3. **Category Breakdown Partial Match**: 41.7% (3,141 wallets) show exact matches between category breakdown sum and aggregated score, indicating categories may be the primary scoring mechanism for nearly half the dataset.

### 📊 **Behavior-Score Alignment Findings**

4. **Strong Component Correlations**: Deposit volume scores show moderate-to-strong correlations (0.41-0.60) with aggregated scores, particularly in primary pools, indicating volume does matter contrary to initial hypothesis.

5. **Retention Score Effectiveness**: Liquidity retention scores demonstrate strong correlation (0.63) in primary pools, validating the retention scoring mechanism.

6. **Volume-Score Monotonicity Failure**: Despite good correlations, cohort analysis reveals non-monotonic relationship in volume deciles, with decile 0 (lowest volume) having significantly lower median scores (102.0) than other deciles (285-352).

### 🚨 **Data Quality & Anomaly Issues**

7. **Perfect Component Coherence**: **100% exact matches** between score components and total scores within individual pools, indicating excellent internal scoring consistency.

8. **Massive Retained Liquidity Data Issues**: 5,314 negative values in `retained_liquidity` fields across multiple pool slots, representing a critical data quality problem affecting 70.6% of wallets.

9. **High-Volume Low-Score Anomalies**: 173 wallets (2.3%) show high deposit volumes but unexpectedly low scores, indicating potential scoring algorithm issues or data quality problems.

10. **Minimal Extreme Outliers**: Only 1 extreme score outlier detected, suggesting the scoring system has effective bounds and consistency mechanisms.

---

## Detailed Analysis & Evidence

### Construction Validity Analysis

#### Score Aggregation Investigation

| Metric | Value | Interpretation |
|--------|-------|----------------|
| Exact Matches (Sum = Aggregated) | 0 (0.0%) | **No simple summation relationship** |
| Correlation (Aggregated vs Sum) | 0.629 | Moderate positive relationship |
| Mean Difference | -476.7 | Massive systematic underestimation |
| Median Difference | -377.0 | Consistent large negative gap |
| Standard Deviation | 415.2 | High variability in differences |

**Critical Finding**: The aggregation formula is **NOT** based on pool score summation. The large negative differences (mean -476.7) suggest either:
- Pool scores represent different units/scales than aggregated scores
- Aggregated scores use entirely different calculation methodology
- Data collection inconsistencies between pool and aggregated scoring

#### Category Breakdown Analysis

| Category Field | Correlation with Aggregated | Mean Value | Median Value |
|----------------|----------------------------|------------|--------------|
| stable-stable | 0.074 | - | 0.0 |
| stable-volatile | 0.527 | - | 0.0 |
| volatile-volatile | 0.225 | - | 102.0 |

**Key Insight**: 
- **41.7% exact matches** between category sum and aggregated score
- **Stable-volatile** shows strongest correlation (0.527)
- **Volatile-volatile** is the only category with non-zero median (102.0)

### Behavior-Score Alignment Analysis

#### Component Score Correlations with Aggregated Score

| Component | Pool 0 | Pool 1 | Pool 2 | Pool 3 | Pool 4 |
|-----------|--------|--------|--------|--------|--------|
| **Deposit Volume Score** | 0.508 | 0.605 | 0.409 | 0.461 | 0.091 |
| **Liquidity Retention Score** | 0.626 | 0.370 | 0.263 | 0.192 | NaN |
| **LP Volatility Score** | 0.379 | 0.367 | 0.252 | - | - |

**Performance Pattern**: 
- **Primary pool (Pool 0)** shows strongest correlations
- **Secondary pools** show declining correlation strength
- **Pool 4+** shows very weak relationships

#### Volume Cohort Analysis: Critical Monotonicity Break

| Volume Decile | Count | Mean Score | Median Score | Min Score | Max Score |
|---------------|-------|------------|---------------|-----------|-----------|
| **0 (Lowest)** | 1,507 | **170.2** | **102.0** | 45.0 | 724.5 |
| 1 | 753 | 327.8 | 302.0 | 52.0 | 736.0 |
| 2 | 753 | 318.9 | 285.6 | 45.1 | 724.5 |
| 3 | 753 | 320.1 | 292.0 | 65.6 | 660.0 |
| 4 | 753 | 322.9 | 299.0 | 48.2 | 659.0 |
| 5 | 753 | 325.4 | 309.4 | 55.8 | 800.4 |
| 6 | 753 | 350.2 | 325.5 | 113.3 | 920.0 |
| 7 | 753 | 355.9 | 329.8 | 46.5 | 802.0 |
| 8 (Highest) | 753 | 362.9 | 352.8 | 116.5 | 732.0 |

**Critical Issue**: Massive gap between decile 0 (median 102.0) and all others (285-352), indicating either:
- Zero-volume wallets receive penalty scoring
- Different scoring algorithm for inactive wallets
- Data quality issue in volume calculations

### Component Coherence Analysis

#### Perfect Internal Consistency

| Pool Slot | Active Pools | Component Match Rate | Status |
|-----------|--------------|---------------------|---------|
| Pool 0 | 7,531 | 100.0% | ✅ Perfect |
| Pool 1 | 1,398 | 100.0% | ✅ Perfect |
| Pool 2 | 307 | 100.0% | ✅ Perfect |
| Pool 3 | 80 | 100.0% | ✅ Perfect |
| Pool 4+ | 39 total | 100.0% | ✅ Perfect |

**Exceptional Finding**: Unlike the aggregation issues, individual pool component scoring shows perfect mathematical consistency.

### Anomaly Detection Results

#### Critical Data Quality Issues

| Issue Type | Count | % of Dataset | Severity |
|------------|-------|--------------|----------|
| **Negative Retained Liquidity** | 5,314 | 70.6% | **Critical** |
| High Volume, Low Score | 173 | 2.3% | High |
| Extreme Score Outliers | 1 | 0.01% | Low |

#### Negative Liquidity Distribution by Pool

| Pool Slot | Negative Values | % of Pool Users |
|-----------|----------------|-----------------|
| Pool 0 | 4,145 | 55.0% |
| Pool 1 | 891 | 63.7% |
| Pool 2 | 195 | 63.5% |
| Pool 3 | 50 | 62.5% |
| Pool 4+ | 33 | Various |

**Critical Data Issue**: Negative retained liquidity values are mathematically impossible and indicate serious data collection or calculation errors.

---

## Formula Inference & Reconstruction

### Aggregation Formula: Cannot Be Determined

**Conclusion**: The aggregated LP score does **NOT** follow a simple mathematical relationship with pool scores.

**Evidence Against Simple Aggregation**:
1. **0% exact matches** with any summation approach
2. **Large systematic differences** (-476.7 mean gap)
3. **Moderate correlation only** (0.629)

### Most Likely Scoring Architecture

Based on the 41.7% category breakdown matches, the probable architecture is:

```
aggregated_lp_score = f(category_breakdown) for 41.7% of wallets
aggregated_lp_score = g(alternative_method) for 58.3% of wallets
```

Where category breakdown calculation:
```
category_sum = stable_stable + stable_volatile + volatile_volatile
IF category_sum > 0: aggregated_lp_score = category_sum
ELSE: aggregated_lp_score = alternative_calculation()
```

### Why Formula Reconstruction is Impossible

1. **Dual Scoring Systems**: Evidence suggests two different scoring methodologies
2. **Missing Category Logic**: No clear rule for when categories vs alternatives are used
3. **Scale Mismatch**: Pool scores appear to use different units than aggregated scores
4. **Data Quality Issues**: Negative values compromise analysis reliability

---

## Recommendations

### Immediate Critical Actions

1. **Fix Negative Liquidity Data**: Address 5,314 impossible negative retained_liquidity values affecting 70.6% of wallets
2. **Document Dual Scoring**: Clarify why 41.7% use category breakdown while 58.3% use alternative methods
3. **Scale Reconciliation**: Explain unit differences between pool scores (sum ~773) and aggregated scores (median 296)
4. **Volume Decile Investigation**: Examine why lowest volume decile has dramatically different scoring pattern

### Strategic Scoring Improvements

1. **Unified Methodology**: Establish single, consistent aggregation approach
2. **Monotonicity Enforcement**: Ensure higher activity generally correlates with higher scores
3. **Component Transparency**: Document exact relationship between pool components and aggregated scores
4. **Quality Validation**: Implement real-time checks for impossible values

### Data Architecture Questions

1. **Why two scoring systems?** What determines category vs alternative calculation?
2. **What is the alternative scoring method** for the 58.3% not using categories?
3. **Are pool scores and aggregated scores measuring the same thing?**
4. **How should negative retained liquidity be interpreted or corrected?**

### Validation Framework

1. **Daily Anomaly Monitoring**: Track negative values, extreme outliers, and scoring inconsistencies
2. **Cohort Validation**: Regular monotonicity testing across volume/activity bands
3. **Component Auditing**: Verify component summation remains at 100% accuracy
4. **Cross-Reference Testing**: Validate category vs alternative scoring decision logic

---

## Conclusions

The LP Score v4 system demonstrates **excellent internal component consistency** (100% accuracy) but **fundamental aggregation methodology issues**. The scoring system appears to use two distinct approaches - category-based for 41.7% of wallets and an undetermined alternative for the remaining 58.3%.

**Critical data quality issues**, particularly 5,314 negative liquidity values affecting 70.6% of wallets, must be resolved before the scoring system can be considered reliable.

The **complete absence of aggregation-to-pool-sum relationship** (0% matches) indicates either sophisticated non-linear transformations or entirely separate calculation methodologies that require documentation and validation.

---

**Report Prepared By**: LP Score Analysis System  
**Methodology**: Statistical analysis of 7,532 wallet records using correlation analysis and cohort studies  
**Reproducibility**: All findings verified through executable notebook analysis with full dataset coverage