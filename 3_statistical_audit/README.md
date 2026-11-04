# Maji Ndogo Agricultural Survey Data Audit & Pipeline Modernization
**Author:** Victor Kagai Njoroge  
**Status:** Audit Completed & Production Pipeline Validated  
**Target Audience:** Audit Board & Ministry Leadership  

---

## Executive Summary

Following regional discrepancies and operational uncertainty regarding state agricultural funding in Maji Ndogo, this technical audit evaluated the integrity, reliability, and analytical value of the statewide agricultural survey ($N = 5,654$ fields across 5 IoT weather stations). 

Initial analyses revealed severe statistical rejections when comparing field data against static reference values, raising initial concerns over data corruption. However, refactoring the analytical pipeline to utilize independent-samples **Welch’s $t$-tests** against raw IoT sensor distributions successfully resolved these alarms, demonstrating full statistical consistency ($p > 0.05$) and proving that field surveys and sensors capture the exact same physical reality.

Key analytical findings establish that agricultural productivity is driven by crop-specific environmental suitability and normalized per-unit yields rather than cumulative acreage volume (with specialized crops like Rice leading median per-unit performance). Furthermore, environmental pollution emerged as the strongest measurable inverse correlate to yield ($r = -0.285$).

While acknowledging structural limitations—specifically spatial autocorrelation and potential socio-economic confounding of the pollution signal—this audit concludes that **the Maji Ndogo agricultural dataset is fully trustworthy for official government planning**. I recommend shifting ministry subsidies from raw volume targets to per-hectare productivity zoning, modernizing environmental monitoring infrastructure to capture rolling real-time distributions, and embedding spatial controls in future survey designs.

---

## 1. Background

In response to conflicting regional crop reports and operational uncertainty surrounding state agricultural funding, an independent technical audit was commissioned on the Maji Ndogo agricultural survey dataset. The survey encompasses **5,654 field records** and **5 regional IoT weather stations**, capturing geographic location, weather telemetry, soil characteristics, crop choices, and reported yields across five major provinces.

The primary objective of this audit is to evaluate the integrity and reliability of the survey data, identify true drivers of agricultural productivity, and resolve discrepancies between field observations and automated environmental monitoring systems before committing public resources.

---

## 2. Method

The audit was executed in three progressive phases using a modular Python analysis architecture:

1. **Exploratory Data Analysis & Normalization:** Descriptive statistics were calculated across provincial boundaries and crop categories. To eliminate sample-size bias across regions, raw production metrics were normalized to **`Standard_yield`** (yield per unit area).
2. **Parametric & Distributional Hypothesis Testing:**
   * **Bessel's Correction ($n-1$):** Applied to eliminate sample variance estimation bias.
   * **Central Limit Theorem (CLT) Validation:** Verified via repeated sampling ($k=1,000, n=30$) to confirm normal sampling distribution properties prior to parametric testing.
   * **One-Sample $t$-Tests:** Evaluated mean field rainfall readings against static published reference numbers for each weather station plaque ($H_0: \mu_{field} = \mu_{reference}$).
   * **Welch’s Two-Sample Independent $t$-Tests:** Applied using unequal variance settings ($equal\_var=False$) to compare full continuous field distributions against raw IoT sensor logs ($H_0: \mu_{field} = \mu_{sensor}$).
3. **Correlation & Risk Assessment:** Environmental correlates, specifically `Pollution_level`, were evaluated across fields to isolate potential yield bottlenecks.

---

## 3. Findings

### F-1: Specialized Crop Yields Excel in Normalized Metrics Over Raw Acreage Volume
Initial aggregations by raw volume artificially favored high-acreage provinces. Normalizing crop yield per unit area  via standard yield demonstrates that rarer crops—most notably **Rice**, with a median yield of **1.73 tonnes**—achieve the highest per-unit productivity despite representing a lower total field count ($N=265$).

![crop yield distribution](exhibits/exhibit_f1_crop_yield_distributions.png)
![crop median yield](exhibits/crop_median_yield.png)

*Interpretation:* Evaluating agricultural output purely by cumulative harvest volume creates a false impression of regional efficiency. Allocations based on raw counts mask high-performing specialized crops and misguide regional funding priorities.

---

### F-2: Static Reference Plaques Trigger False Alarm Rejections in One-Sample Tests
Comparing field rainfall data against static single-point weather station references via one-sample $t$-tests resulted in statistical rejections ($p < 0.05$) across 4 out of 5 weather stations (e.g., Station 0 yielded $p = 0.00001$).

![false alarm(one sample t-test)](exhibits/exhibit_f2_onesample_static_discrepancy.png)

*Interpretation:* Single-value benchmarks fail to account for continuous natural variance, seasonal shifts, or localized micro-climates. The initial statistical alarm was an artifact of comparing dynamic field data against a rigid, unyielding static constant.

---

### F-3: Two-Sample Welch’s $t$-Tests Confirm Distributional Alignment and Data Integrity
When field observations were re-tested against raw continuous IoT sensor logs using independent-samples Welch's $t$-tests ($equal\_var=False$), all 15 station-to-measurement comparisons passed without statistical rejection ($p > 0.05$).
![two sample t-test](exhibits/t-test.png)
![two sample distributions](exhibits/exhibit_f3_welch_two_sample_distributions.png)

*Interpretation:* The field survey team and the IoT weather sensors recorded the exact same physical reality. Natural environmental variation accounts for individual measurement differences, proving that the survey dataset is authentic and uncorrupted.

---

### F-4: Environmental Pollution Exhibits the Strongest Inverse Correlation with Standard Yield
Across all measured environmental parameters, `Pollution_level` demonstrated the strongest negative correlation with standard yield ($r = -0.285$).

![pollution level correlation](exhibits/exhibit_f4_pollution_yield_correlation.png)

*Interpretation:* Higher pollution readings consistently track with reduced agricultural output. While correlation alone does not establish biological toxicity, pollution represents the single largest measurable environmental bottleneck to productivity in the survey data.

---

## 4. Limitations

While the refactored pipeline and distributional testing establish a defensible baseline for Maji Ndogo's agricultural data, two structural limitations must be noted:

1. **Spatial Autocorrelation (Observation Independence):** Standard parametric hypothesis tests assume spatial independence across observations. In agricultural field surveys, neighboring plots share continuous geographic features such as soil structure, micro-climates, and local water tables. Consequently, the effective sample size may be lower than the raw $N = 5,654$ field count due to spatial clustering. Future iterations will incorporate spatial autoregressive models ($SAR$) and cluster-robust standard errors grouped by coordinate grids to explicitly isolate geographic dependencies.
2. **Confounding Variables in Environmental Correlates:** The negative correlation between `Pollution_level` and `Standard_yield` ($r = -0.285$) represents the strongest environmental signal in the dataset. However, this metric indicates statistical association rather than direct biological causality. Pollution severity may act as a proxy variable for underlying socio-economic or infrastructural disparities, such as reduced access to capital, lower-grade fertilizers, or degraded irrigation networks in industrial-adjacent farming zones. Multi-variable regression modeling is required to decouple direct environmental toxicity from economic confounding factors.

---

## 5. Verdict


**Yes, the Maji Ndogo agricultural survey dataset can be trusted for official government planning and funding allocation.** 

The apparent data discrepancies flagged during initial one-sample audits were methodological artifacts caused by testing dynamic field observations against static reference constants. Once evaluated using Welch’s independent-samples distributional comparisons against raw IoT logs, the survey data demonstrated full statistical consistency ($p > 0.05$). 

### Recommendations
1. **Shift Subsidies from Volume to Productivity:** The ministry should move away from rewarding raw cumulative acreage and opt to place funding and resources into per-hectare productivity and climate-suitability zoning.
2. **Modernize Environmental Monitoring Infrastructure:** Static reference values are prone to triggering false alarms. The ministry should allocate funding for the modernization of weather station sensors to capture continuous, rolling distributions of real-time weather data, reducing reliance on brittle fixed constants and improving overall data accuracy.
3. **Incorporate Spatial and Economic Controls:** Future data collection frameworks must account for spatial autocorrelation and socio-economic confounding factors (such as infrastructure access in industrial-adjacent farming zones) to ensure policy interventions target true biological root causes rather than proxy variables.

---
