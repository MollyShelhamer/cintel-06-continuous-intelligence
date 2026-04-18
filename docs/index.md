# Continuous Intelligence

This site provides documentation for this project.
Use the navigation to explore module-specific materials.

## How-To Guide

Many instructions are common to all our projects.

See
[⭐ **Workflow: Apply Example**](https://denisecase.github.io/pro-analytics-02/workflow-b-apply-example-project/)
to get these projects running on your machine.

## Project Documentation Pages (docs/)

- **Home** - this documentation landing page
- **Project Instructions** - instructions specific to this module
- **Glossary** - project terms and concepts

## Custom Project

### Dataset
- Provided dataset, system_metrics_case.csv
- requests, errors, total_latency_ms

### Signals
-  Created severity_index = error_rate * avg_latency_ms

### Experiments
- Adjusted anomaly threshold
- MAX_SEVERITY_INDEX = 1.0

### Results
- Pipeline ran as expected
- 23 anomalies detected

### Interpretation
- Rather than looking at error rates and latency seperately, the severity index provides further insight

## Additional Resources

- [Suggested Datasets](https://denisecase.github.io/pro-analytics-02/reference/datasets/cintel/)
