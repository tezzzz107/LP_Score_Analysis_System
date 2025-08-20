# LP Score v4 Validation & Stress-Test Analysis

A comprehensive data analysis project to validate, explain, and pressure-test the LP (Liquidity Provider) Score v4 aggregation methodology. This project identifies anomalies, edge cases, and inconsistencies in the scoring system through rigorous statistical analysis.

## 📊 Project Overview

**Objective**: Validate how `aggregated_lp_score` is derived from per-pool behavior and score components, identify anomalies and inconsistencies where behavior suggests different scores than assigned.

**Dataset**: 7,532 wallets with 396 columns including LP scoring data, pool metadata, and behavioral metrics.

## 🔍 Key Findings

- **0% exact matches** between aggregated scores and sum of pool scores
- **41.7% of wallets** use category breakdown as primary scoring method
- **100% component coherence** within individual pools
- **5,314 negative liquidity values** affecting 70.6% of wallets (critical data quality issue)
- **173 high-volume, low-score anomalies** identified

## 🚀 Features

### Core Analysis Functions
- **Construction Validity**: Tests relationship between aggregated scores and pool components
- **Behavior-Score Alignment**: Analyzes correlations between activity metrics and scores
- **Component Coherence**: Validates internal scoring consistency
- **Anomaly Detection**: Identifies outliers and data quality issues
- **Cohort Analysis**: Volume-based monotonicity testing

### Visualization Dashboard
- Score distribution analysis
- Correlation matrices
- Category breakdown visualizations
- Volume vs score relationships
- Component contribution analysis

### Utility Functions
- Individual wallet deep-dive analysis
- Multi-wallet comparison tools
- Comprehensive data quality validation
- Anomaly export to CSV

## 📁 Repository Structure

```
├── LP_Score_v4_Analysis.ipynb    # Main analysis notebook
├── README.md                     # This file
├── requirements.txt              # Python dependencies
├── LP_Score_v4_Report.md         # Comprehensive analysis report
├── data/                         # Data directory (add your CSV files here)
│   └── dex-temp-db.score_v4.csv # Sample data file (not included)
└── outputs/                      # Generated outputs
    └── lp_score_anomalies.csv   # Anomaly detection results
```

## 🛠️ Installation & Setup

### Prerequisites
- Python 3.7+
- Jupyter Notebook or Google Colab
- Git

### Local Installation

1. **Clone the repository**
```bash
git clone https://github.com/tezzzz107/LP_Score_Analysis_System.git
cd lp-score-v4-analysis
```

2. **Install dependencies**
```bash
pip install -r requirements.txt
```

3. **Start Jupyter Notebook**
```bash
jupyter notebook LP_Score_v4_Analysis.ipynb
```

### Google Colab Usage

1. **Upload notebook to Google Colab**
   - Go to [Google Colab](https://colab.research.google.com/)
   - Upload `LP_Score_v4_Analysis.ipynb`

2. **Install requirements in Colab**
```python
!pip install -r requirements.txt
```

3. **Upload your data file**
   - Upload your CSV file to `/content/` directory
   - Update file path in the analysis functions

## 📈 Usage

### Quick Start

```python
# Run complete analysis pipeline
results = run_complete_analysis('/path/to/your/data.csv')
```

### Step-by-Step Analysis

```python
# Load and explore data
df = load_and_explore_data('/path/to/your/data.csv')

# Run individual analyses
construction_analysis = analyze_score_construction(df)
behavior_analysis = analyze_behavior_score_alignment(df)
anomalies = detect_anomalies(df)

# Create visualizations
create_visualizations(df)

# Generate summary report
generate_summary_report(df, analyses)
```

### Analyze Specific Wallets

```python
# Deep dive into specific wallet
explore_specific_wallet(df, 'wallet_id_here')

# Compare multiple wallets
compare_wallets(df, ['wallet1', 'wallet2', 'wallet3'])
```

## 📊 Expected Outputs

### Console Analysis
- Comprehensive dataset overview and statistics
- Construction validity analysis results
- Behavior-score alignment correlations
- Component coherence validation
- Detailed anomaly detection findings

### Visualizations
- Score distribution histograms
- Aggregated vs pool score scatter plots
- Category breakdown analysis charts
- Volume vs score correlation plots
- Component contribution bar charts

### Generated Files
- `lp_score_anomalies.csv`: Detailed anomaly list with wallet IDs, reasons, metrics, and thresholds
- Analysis summary report in console output

## 🎯 Key Analysis Components

### 1. Construction Validity
- **Aggregation Formula Testing**: Attempts to reproduce `aggregated_lp_score` from pool scores
- **Category Breakdown Audit**: Validates relationship between category fields and aggregated scores
- **Correlation Analysis**: Measures relationship strength between different scoring approaches

### 2. Behavior-Score Alignment
- **Volume Analysis**: Tests correlation between deposit/withdraw volumes and scores
- **Retention Analysis**: Validates liquidity retention impact on scores
- **Cohort Studies**: Volume-based decile analysis with monotonicity testing
- **Dust & Volatility Impact**: Analyzes penalty/bonus effects

### 3. Component Coherence
- **Within-Pool Validation**: Confirms score components sum to pool totals
- **Cross-Pool Consistency**: Tests component scoring consistency across pools
- **Attribution Analysis**: Ranks component contributions to high scores

### 4. Anomaly Detection
- **Outlier Detection**: IQR and MAD-based robust outlier identification
- **Behavior Misalignment**: High volume + low score anomalies
- **Data Quality Issues**: Negative values, duplicates, missing data
- **Temporal Anomalies**: Stale high scores, inactive wallet patterns

## 📋 Deliverables

### ✅ Completed Deliverables
1. **Comprehensive Analysis Report** (2-3 pages)
   - Executive summary with top 10 key findings
   - Clear tables and plots proving each claim
   - Formula inference and reconstruction attempts

2. **Reproducible Code** (Jupyter Notebook)
   - Modular analysis functions
   - Complete documentation and examples
   - Reusable for different datasets

3. **Anomalies CSV Export**
   - Format: `wallet_id, pool_id, reason, metric, value, threshold`
   - 5,488 anomalies identified and categorized

4. **Technical Assumptions & Open Questions**
   - Missing formula parameters identification
   - Data quality issues documentation
   - Recommendations for score system improvements

## 🔧 Customization

### Adding New Analysis Functions
```python
def your_custom_analysis(df):
    """
    Add your custom analysis here
    """
    # Your analysis code
    return results

# Add to main pipeline
results['custom'] = your_custom_analysis(df)
```

### Modifying Anomaly Detection
- Update thresholds in `detect_anomalies()` function
- Add new anomaly types by extending the detection logic
- Customize anomaly severity levels

### Extending Visualizations
- Add new plots to `create_visualizations()` function
- Customize plot styling and colors
- Create interactive visualizations with plotly

## 🐛 Troubleshooting

### Common Issues

**Issue**: `KeyError` for column names
**Solution**: Check your CSV column names match expected format (`lp_scores[0].pool_id`, etc.)

**Issue**: Memory errors with large datasets
**Solution**: Process data in chunks or increase system memory

**Issue**: Missing visualizations
**Solution**: Ensure matplotlib backend is properly configured

**Issue**: Empty analysis results
**Solution**: Verify data file path and CSV format

### Performance Optimization
- Use `dtype` specifications when loading large CSV files
- Consider sampling for initial analysis on very large datasets
- Use vectorized pandas operations instead of loops

## 📚 Dependencies

See `requirements.txt` for complete list. Key dependencies:
- `pandas` (≥1.3.0): Data manipulation and analysis
- `numpy` (≥1.20.0): Numerical computing
- `matplotlib` (≥3.3.0): Plotting and visualization
- `seaborn` (≥0.11.0): Statistical visualizations
- `scipy` (≥1.7.0): Statistical functions

## 🤝 Contributing

1. **Fork the repository**
2. **Create feature branch** (`git checkout -b feature/new-analysis`)
3. **Commit changes** (`git commit -am 'Add new analysis feature'`)
4. **Push to branch** (`git push origin feature/new-analysis`)
5. **Create Pull Request**

### Contribution Guidelines
- Follow existing code style and documentation patterns
- Add tests for new analysis functions
- Update README for significant feature additions
- Ensure compatibility with both local and Colab environments

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👥 Authors

- Tejas Singh(https://github.com/tezzzz107)

## 🙏 Acknowledgments

- LP Score v4 system developers for providing the dataset
- Open source data analysis community for tools and techniques
- Statistical analysis methodologies from academic research

## 📞 Support

For questions, issues, or contributions:
- **Create an issue** on GitHub
- **Email**: tejasparesh2003@gmail.com
- **Documentation**: Check inline code documentation and comments

---

**Last Updated**: August 2025  
**Version**: 1.0.0  
**Status**: Production Ready ✅
