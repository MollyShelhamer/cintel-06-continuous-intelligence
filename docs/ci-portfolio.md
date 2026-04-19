# Continuous Intelligence Portfolio

Molly Shelhamer

2026-04

This page summarizes my work on **continuous intelligence** projects.

## 1. Professional Project

### Repository Link

https://github.com/MollyShelhamer/cintel-01-getting-started

### Brief Overview of Project Tools and Choices

- The first project establishes the format for the following projects with an emphasis on an organized structure and repeatable workflows.
- Project environment managed by uv
- Git to track changes
- pyproject.toml: packages, dependencies, metadata
- Pertinent documentation in README.md & zensical.toml
- Folders
  - src/cintel: .py files
  - docs: project instructions and additional information
  - data: holds input data
  - artifacts: holds results

## 2. Anomaly Detection

### Repository Link

https://github.com/MollyShelhamer/cintel-02-static-anomalies

### Techniques

- Anomalies were detected by first reading the csv data file into a polars dataframe, then inspecting the data to define reasonable thresholds
- In this case, the sample data is for an adult clinic, with columns age (years) and height (inches.)
  - Therefore, a minimum value of 18 years is the defined threshold
  - 60 inches (5 feet) is a minimum height
- Any anomalies detected (under 18 years and/or under 60 inches) are saved in an output file within the artifacts folder.

### Artifacts

https://github.com/MollyShelhamer/cintel-02-static-anomalies/tree/main/artifacts
- 3 anomalies were detected, 2 were lower than the minimum age threshold, and 1 was lower than the minimum height threshold.

### Insights

- 12.5 % of the dataset were detected as anomalies
  - The age anomalies could be special-case patients or data entry errors, but the height anomaly (19 inches) should be investigated.
- Upper bounds may be a logical next step- as some patient ages are unusually high (but not impossible.)

## 3. Signal Design

### Repository Link

https://github.com/MollyShelhamer/cintel-03-signal-design

### Signals

- The data analyzed is annual AQI (air quality index) by county in 2025.
  - unhealthy_ratio: measure of unhealthy days out of total days
  - good_day_ratio: measure of good (healthy) air quality days out of total days
  - pollution_severity_index: weighted pollution severity score created by assigning increasing values to types of days (Moderate * 1, Unhealthy * 3, etc.)
  - aqi_spread: gap between median and macx AQI to identify pollution events
  - pm_pollution_ratio: fraction of pm pollution (particulate matter) days relative to total AQI

### Artifacts

https://github.com/MollyShelhamer/cintel-03-signal-design/tree/main/artifacts

A row for each county in the dataset (most in the US) with the signals above.

### Insights

- Pollution data varies greatly across the US. Further studies would have to be conducted to reveal patterns or causes.

## 4. Rolling Monitoring

### Repository Link

https://github.com/MollyShelhamer/cintel-04-rolling-monitoring

### Techniques

- Data was read from a csv containing a timeseries of system metrics
  - timestamp, requests, errors, total_latency_ms

- Sorted by timestamp, a rolling window with a size of 4 (most recent 4 observations) computes the following statistics:
  - Requests: Rolling mean
  - Errors: Rolling mean & median
  - Latency: Rolling mean

### Artifacts

https://github.com/MollyShelhamer/cintel-04-rolling-monitoring/tree/main/artifacts

- The output artifact file contains the original dataset, along with the rolling statistics. The first three records do not have statistics due to the window size of 4.

### Insights

- Short term variation is smoothed, long term trends are revealed

## 5. Drift Detection

### Repository Link

https://github.com/MollyShelhamer/cintel-05-drift-detection

### Techniques

- Two csv files were used for drift detection: reference metrics and current metrics. Both sets of metrics were averages (avg requests, avg errors, etc.)
- Difference in mean indicates results
  - A positive value indicates that the current period is higher than the reference period
  - A negative value indicates that the current period is lower than the reference
- Drift is flagged when the difference exceeds thresholds
  - Requests: > 20.0
  - Errors: > 1.0
  - Error Rate: > 0.01
  - Latency: > 1000.0 ms

### Artifacts

https://github.com/MollyShelhamer/cintel-05-drift-detection/tree/main/artifacts

- A drift summary table is produced which details the averages for both the reference and current metrics, the difference, and a T/F flag for drift.

### Insights

- Drift is present in all metrics.
- There is significant difference in requests, errors, and latency. Error rate is only slightly elevated.

## 6. Continuous Intelligence Pipeline

### Repository Link

https://github.com/MollyShelhamer/cintel-06-continuous-intelligence

### Techniques

- Raw metrics included requests, errors, and total_latency_ms
  - Signals created:
    - error_rate = errors/requests
    - avg_latency_ms = total_latency_ms / requests
    - severity_index = error_rate * avg_latency_ms

- Anomaly Detection
  - Error rate > 3%
  - Average Latency > 35ms
  - Severity Index > 1.0

- System Assessment
  - Threshold logic applied to averages of all signals
    - System classified as "STABLE" or "DEGRADED"

### Artifacts

https://github.com/MollyShelhamer/cintel-06-continuous-intelligence/tree/main/artifacts

- Averages of all metrics and signals are present in the output file.
- System was classified as Stable.

### Assessment

- 23 Anomalies detected
- Error rate is low, Latency is on the higher end but still normal, and the severity index is below 1.0.
- The system is stable.
