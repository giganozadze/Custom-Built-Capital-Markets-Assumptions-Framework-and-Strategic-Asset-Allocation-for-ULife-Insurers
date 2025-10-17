# Custom-Built Capital Market Assumptions Framework and Strategic Asset Allocation for U.S. Life Insurers

**Author:** Giga Nozadze  
**Institution:** New York University  
**Program:** Capstone Project - Fall 2025  
**Focus:** Quantitative Portfolio Management & Insurance Asset Allocation

---

## 📋 Project Overview

This capstone project develops a comprehensive **Strategic Asset Allocation (SAA)** framework tailored specifically for U.S. life insurance companies. The project combines custom-built **Capital Market Assumptions (CMAs)**, **liability-driven investment (LDI)** strategies, and **risk-based capital (RBC)** constraints to construct optimal portfolios that balance return generation with regulatory requirements and liability matching.

### Key Objectives

1. **Build Custom CMAs**: Develop forward-looking return, risk, and correlation assumptions across multiple asset classes (Fixed Income, Public Equity, Private Equity, Real Estate, Direct Lending, etc.)

2. **Analyze Current Allocations**: Examine and compare the asset allocation strategies of six major U.S. life insurers:
   - Brighthouse Financial
   - Corebridge Financial
   - MetLife
   - Principal Financial Group
   - Prudential Financial
   - Voya Financial

3. **Liability Modeling**: Create company-specific liability duration profiles based on product mix and business characteristics

4. **Portfolio Optimization**: Implement multi-constraint quadratic optimization with:
   - Surplus optimization (asset-liability management)
   - Duration matching constraints
   - Credit quality requirements
   - Key Rate Duration (KRD) bucket matching
   - RBC capital efficiency

5. **Robust Optimization**: Employ resampled efficient frontier techniques to address parameter uncertainty and produce portfolios robust to estimation error

---

## 📁 Project Structure

```
Capstone/
│
├── CMA framework/                          # Capital Market Assumptions Development
│   ├── Capital Market Assumptions Framework_Giga Nozadze.docx
│   ├── Capital Market Assumptions Framework_Giga Nozadze.pdf
│   ├── Capital Market Assumptions Framework_Giga Nozadze (presentation).pptx
│   ├── Capital Market Assumptions Framework_Giga Nozadze (presentation).pdf
│   │
│   ├── Excel files/
│   │   ├── Fixed Income.xlsx              # Treasury, IG/HY Corporate, MBS, ABS, Loans
│   │   ├── Equity.xlsm                    # Public equity - sector-by-sector bottom-up analysis
│   │   ├── Alternatives.xlsx              # PE, RE, Direct Lending, Special Situations
│   │   ├── Covariances.xlsx               # Cross-asset correlation matrix
│   │   ├── correl.ipynb                   # Correlation analysis & regime dynamics
│   │   └── Bottom-up sector equity data/  # GICS sector-level analysis (11 sectors)
│   │
│   └── versions/                           # Historical iterations & methodology evolution
│
├── Current allocations/                    # Empirical Analysis of Life Insurer Portfolios
│   ├── Comparative Analysis of Asset Allocations of Six U.S. Life Insurers.pdf
│   │
│   ├── 10-Q reports/                      # Source regulatory filings (Q3 2024)
│   │   ├── Brighthouse.pdf
│   │   ├── Corebridge.pdf
│   │   ├── MetLife.pdf
│   │   ├── Principal.pdf
│   │   ├── Prudential.pdf
│   │   └── Voya.pdf
│   │
│   └── Aggregated/                        # Cleaned & normalized allocation data
│       ├── Aggregated.xlsx                # Consolidated view across all companies
│       ├── Brighthouse.xlsx
│       ├── Corebridge.xlsx
│       ├── MetLife.xlsx
│       ├── Principal.xlsx
│       ├── Prudential.xlsx
│       └── Voya.xlsx
│
├── Optimization/                           # Portfolio Construction & Analysis
│   ├── Optimization.ipynb                 # Main optimization notebook (detailed implementation)
│   ├── final_inputs.xlsx                  # Master input file: assets, returns, risks, correlations, RBC
│   ├── Liability_Profiles.xlsx            # Company-specific liability term structures
│   ├── optimal_portfolios.xlsx            # Output: optimized allocations for all six companies
│   ├── Brighthouse_Portfolio_Analysis.xlsx
│   ├── Risk, return, regulation.xlsx      # RBC charge analysis
│   └── versions/                          # Development iterations
│
├── Resources/                             # Academic & Industry Research
│   ├── Literature/                        # 21 academic papers on SAA, factor investing, robust optimization
│   ├── Materials/
│   │   ├── Alternatives/                  # Private markets benchmarks (NCREIF, Cambridge, AQR)
│   │   ├── Correlation/                   # Stock-bond correlation regime analysis
│   │   ├── Equity/                        # Equity risk premium (Damodaran)
│   │   └── Fixed Income/                  # Yield curve & duration research
│   ├── Regulations/                       # NAIC RBC guidelines & insurance investment regulations
│   └── SAA course/                        # Foundational SAA course materials & case studies
│
├── Capstone Project Proposal_Giga Nozadze.docx
├── Capstone Project Proposal_Giga Nozadze.pdf
├── .gitignore
└── README.md

```

---

## 🔬 Methodology

### 1. Capital Market Assumptions (CMA) Framework

#### Asset Classes Covered
- **Fixed Income**: U.S. Treasury (5Y, 10Y, 30Y), Investment-Grade Corporate, High-Yield Corporate, Leveraged Loans, Residential & Commercial MBS, ABS, CLOs
- **Public Equity**: U.S. Large-Cap, Mid-Cap, Small-Cap (sector decomposition using GICS), Emerging Markets
- **Private Markets**: Private Equity, Real Estate (Core, Value-Add, Opportunistic), Direct Lending, Private Credit
- **Alternatives**: Special Situations, Infrastructure

#### CMA Development Approach
- **Building Block Method**: Risk-free rate + term premium + credit premium + equity risk premium
- **Bottom-Up Sector Analysis**: For public equity, aggregation from 11 GICS sectors with individual ERP estimates
- **Historical Calibration**: Volatility and correlation estimates from 10-20 year lookback with regime adjustments
- **Forward-Looking Adjustments**: Incorporating current yield levels, credit spreads, and valuation metrics
- **Regime Considerations**: Stock-bond correlation dynamics and diversification benefits across market environments

### 2. Liability Modeling

Each company's liability profile is modeled using a **three-bucket exponential decay framework**:
- **Short Duration (3-4 years)**: Group benefits, stable value, general account products
- **Medium Duration (7-8 years)**: Variable annuities (VA), retirement products requiring dynamic hedging
- **Long Duration (15-20 years)**: ULSG (Universal Life with Secondary Guarantees), payout annuities, pension risk transfer

Company-specific calibration based on product mix:
- **Brighthouse & Voya**: VA-heavy → higher medium-duration weighting
- **Prudential**: ULSG & annuities → significant long-duration exposure
- **Corebridge**: Balanced across buckets
- **MetLife & Principal**: Diversified mix

**Output Metrics**:
- Effective liability duration (5-12 years range across companies)
- Convexity proxies
- Liquidity needs (1Y, 5Y cash flow shares)

### 3. Portfolio Optimization Framework

#### Objective Function: Surplus Optimization
$$
\max_{w} \quad \mu^T w - \lambda \cdot w^T \Sigma w - \phi \cdot (c_{RBC}^T w)
$$

Where:
- $\mu$ = vector of expected returns (CMAs)
- $\Sigma$ = covariance matrix
- $\lambda$ = risk aversion parameter (default: 8)
- $c_{RBC}$ = vector of RBC capital charges per asset
- $\phi$ = capital penalty coefficient (0.1)

#### Constraints

**1. Budget Constraint**
$$
\sum_{i=1}^{n} w_i = 1
$$

**2. Duration Matching**
$$
D_{liab} - 1 \leq \sum_{i=1}^{n} D_i w_i \leq D_{liab} + 1
$$

**3. Credit Quality Constraints**
- Average fixed income rating ≤ A+ (numeric score ≤ 5)
- BBB-rated bonds ≤ 15%
- Below-investment-grade ≤ 5%

**4. Asset Class Limits**
- Public Equity ≤ 5%
- Leveraged Loans ≤ 5%
- Emerging Markets Debt (IG) ≤ 10%
- Real Estate ≤ 5%
- Commercial/Residential Whole Loans ≤ 20% (each)
- Direct Lending ≤ 20%
- Alternatives/Special Situations ≤ 5%
- **Total Strategic Assets ≤ 35%** (regulatory prudence)

**5. Key Rate Duration (KRD) Bucket Matching** (Optional)
$$
(1 - \tau) \cdot L_k \leq \sum_{i \in \text{bucket } k} w_i \leq (1 + \tau) \cdot L_k \quad \forall k \in \{KR5, KR10, KR20\}
$$
Where $\tau$ = tolerance (30%), $L_k$ = liability weight in bucket $k$

**6. Position Limits**
- Individual asset: 0% ≤ $w_i$ ≤ 10%

#### Solver
- **CVXPY** with ECOS (Embedded Conic Solver)
- Convex quadratic programming (QP)
- Computational efficiency: ~200 portfolios in 15-30 seconds

### 4. Robust Optimization (Resampled Efficient Frontier)

#### Motivation
Point-estimate CMAs are subject to **estimation error**. Small changes in expected returns can produce dramatically different optimal portfolios (optimizer's "error-maximization" property).

#### Approach: Michaud Resampling
1. **Simulate CMA Scenarios** (N=100):
   - Perturb expected returns: $\tilde{\mu} = \mu + g_{market} + \epsilon_{idio}$
     - Market shock: common factor, scaled to ~4% of median vol
     - Idiosyncratic: asset-specific, ~5% of individual vol
   - Perturb covariance: jitter correlations (±1%) and volatilities (±2%), then shrink back toward base $\Sigma$ (α=0.4 for stability)

2. **Optimize per Scenario**: Solve QP for each $(\tilde{\mu}, \tilde{\Sigma})$ pair → 100 frontiers

3. **Aggregate**:
   - Interpolate all frontiers onto a common volatility grid (120 points)
   - Keep only grid points with ≥80% scenario coverage (robustness filter)
   - Compute **mean weights** at each risk level across scenarios
   - Calculate percentile bands (25th, 75th) to visualize uncertainty

4. **Select Portfolios**:
   - 6 evenly-spaced portfolios along mean frontier
   - 1 max-utility portfolio: $\arg\max (\bar{\mu}^T w - \lambda \bar{\sigma}^2(w))$

#### Benefits
- Reduces concentration in estimation-error-driven "corner solutions"
- Produces well-diversified, stable allocations
- Explicitly accounts for forecasting uncertainty
- Mean-frontier portfolios are **out-of-sample robust**

---

## 💻 Technical Implementation

### Requirements

**Python Packages:**
```python
numpy>=1.21.0
pandas>=1.3.0
cvxpy>=1.1.0        # Convex optimization
scipy>=1.7.0
matplotlib>=3.4.0
openpyxl>=3.0.0     # Excel I/O
xlsxwriter>=3.0.0
ecos>=2.0.0         # QP solver backend
scs>=2.1.0          # Alternative solver
```

**Development Environment:**
- Python 3.8+
- Jupyter Notebook / JupyterLab
- Excel (Microsoft 365 or equivalent for .xlsm macros)

### Key Functions

#### `load_inputs()`
Reads `final_inputs.xlsx` containing:
- Asset names, credit ratings
- Expected returns (μ), volatilities (σ)
- Covariance matrix (Σ)
- RBC capital charges
- Effective durations
- Current company allocations

#### `solve_surplus_qp()`
Core optimization engine:
```python
solve_surplus_qp(mu, Sigma, duration, cap_charge, wmin, wmax,
                 assets, ratings, rating_scores, company_liability_duration,
                 krd_targets=None, B=None, krd_names=None,
                 surplus_lambda=8.0, cap_penalty_phi=0.1)
```
Returns: optimal weights, portfolio variance, portfolio return

#### `optimizer(company_name="Brighthouse")`
High-level interface for single-scenario optimization:
- Loads company-specific liabilities
- Builds efficient frontier (200 portfolios via λ-sweep)
- Selects representative portfolios (6 spaced + 1 max-utility)
- Generates formatted output tables and plots
- Exports to Excel

#### `simulate_cma_scenarios(mu, Sigma, n_sim=100)`
CMA perturbation engine for robust optimization:
- Market + idiosyncratic return shocks
- Correlation and volatility jittering
- Covariance matrix shrinkage for positive-definiteness

#### `simulate_efficient_frontiers()`
Builds N efficient frontiers under perturbed CMAs

#### `aggregate_frontiers()`
Interpolates, filters (coverage), and aggregates simulated frontiers:
- Returns: mean frontier, IQR bands, coverage mask

#### `robust_optimizer(company_name="Brighthouse", n_scenarios=100)`
End-to-end robust optimization:
- Simulates CMAs
- Builds 100 frontiers
- Aggregates to mean frontier
- Selects robust portfolios
- Plots baseline vs. resampled frontiers

---

## 📊 Key Results & Insights

### Current Allocation Patterns (Q3 2024)

| Company | Fixed Income | Mortgage Loans | Direct Lending | Alts/PE/RE | Public Equity | Eff. Duration |
|---------|-------------|----------------|----------------|------------|---------------|---------------|
| **Brighthouse** | ~60% | ~15% | ~10% | ~10% | ~5% | 6.5 years |
| **Corebridge** | ~65% | ~12% | ~8% | ~10% | ~5% | 7.2 years |
| **MetLife** | ~58% | ~18% | ~12% | ~8% | ~4% | 6.8 years |
| **Principal** | ~62% | ~10% | ~15% | ~9% | ~4% | 7.0 years |
| **Prudential** | ~55% | ~20% | ~10% | ~12% | ~3% | 8.5 years |
| **Voya** | ~63% | ~14% | ~8% | ~11% | ~4% | 6.3 years |

**Common Themes:**
- **Fixed Income Dominance**: Core holdings in IG Corporate, Treasuries, and Structured Products
- **Private Markets Shift**: Significant allocations to Commercial Mortgage Whole Loans and Direct Lending (higher yield + illiquidity premium)
- **Limited Public Equity**: Regulatory capital charges + volatility → minimal public equity exposure
- **Alternatives Diversification**: Private Equity, Real Estate, and Special Situations for return enhancement

### Optimization Insights

#### Efficient Frontier Characteristics
- **Return Range**: 4.5% - 6.5% (expected)
- **Risk Range**: 3.0% - 8.0% (volatility)
- **Sharpe Ratios**: 0.15 - 0.35 (relative to 4% risk-free rate)

#### Optimal vs. Current Allocations

**Typical Adjustments for Max-Utility Portfolio**:
1. **Reduce**: Low-yielding Treasuries (~5% → ~2%)
2. **Increase**: Direct Lending (~10% → ~18-20%, near upper bound)
3. **Increase**: IG Corporate Bonds with higher spread (~25% → ~30%)
4. **Maintain**: Mortgage Loans (~15-20%, strong risk-adjusted returns)
5. **Strategic Tilt**: Core Real Estate (~3% → ~5%), Private Equity (~2% → ~4%)
6. **Credit Quality**: Shift down the IG spectrum (more A/BBB, less AAA/AA) within constraints

**Portfolio Improvements**:
- **Expected Return**: +40-60 bps increase
- **Risk**: Slight increase (+20-30 bps vol) or neutral depending on risk aversion
- **Sharpe Ratio**: +0.05-0.10 improvement
- **Duration Match**: Improved (net duration closer to 0)
- **Capital Efficiency**: Similar or improved RBC utilization

#### Robust vs. Baseline Frontiers

**Resampled Frontier Characteristics**:
- **Smoother**: Eliminates estimation-driven corner solutions
- **Well-Diversified**: Mean weights are more evenly distributed (no extreme concentrations)
- **Stable**: Less sensitive to small CMA changes
- **IQR Bands**: ±30-50 bps return uncertainty at each risk level (shows parameter risk explicitly)

**Trade-offs**:
- Slightly lower expected returns on mean frontier vs. baseline (~10-20 bps) 
- Rationale: sacrifices "error-maximization" for robustness
- **Out-of-sample performance**: Mean-frontier portfolios historically outperform baseline in backtests (Michaud, 1998)

---

## 📈 Usage Instructions

### Running the Analysis

#### 1. Setup Environment
```bash
# Clone the repository
git clone https://github.com/giganozadze/Custom-Built-Capital-Markets-Assumptions-Framework-and-Strategic-Asset-Allocation-for-ULife-Insurers.git

cd Capstone

# Install dependencies
pip install numpy pandas cvxpy scipy matplotlib openpyxl xlsxwriter ecos scs
```

#### 2. Review CMAs
- Open `CMA framework/Capital Market Assumptions Framework_Giga Nozadze.pdf` for methodology
- Review `CMA framework/Excel files/`:
  - `Fixed Income.xlsx` - FI asset classes
  - `Equity.xlsm` - Equity sector analysis (enable macros)
  - `Alternatives.xlsx` - Private markets assumptions
  - `Covariances.xlsx` - Full correlation matrix

#### 3. Run Optimization
```python
# Open Optimization/Optimization.ipynb in Jupyter

# For single-scenario optimization (baseline):
results_df = optimizer(company_name="Brighthouse", show_plots=True, export_excel=True)

# For robust optimization (resampled frontier):
robust_results = robust_optimizer(
    company_name="Brighthouse",
    n_scenarios=100,
    n_points=80,
    show_plots=True,
    export_excel=True
)
```

#### 4. Analyze Results

**Baseline Output** (`Brighthouse_Portfolio_Analysis.xlsx`):
- `Full_Results`: Complete allocation weights and metrics for 7 portfolios (Current + Port A-F + Max Utility)
- `Summary_Metrics`: Return, Risk, Sharpe, Duration, Credit Quality, RBC usage
- `Formatted_Display`: Styled table with color gradient for weights

**Robust Output** (`Brighthouse_Robust_Portfolio_Analysis.xlsx`):
- Same structure, but portfolios are mean weights from resampled frontier
- More stable, diversified allocations

**Plots**:
- Efficient frontier with current portfolio and optimal portfolios
- Baseline vs. Resampled frontiers with uncertainty bands

#### 5. Repeat for Other Companies
```python
companies = ["Corebridge", "MetLife", "Principal", "Prudential", "Voya"]
for company in companies:
    optimizer(company_name=company, export_excel=True)
    robust_optimizer(company_name=company, export_excel=True)
```

### Customization Options

#### Adjust Parameters (in notebook):
```python
RISK_FREE = 0.04                  # Risk-free rate (4%)
SURPLUS_LAMBDA = 8.0              # Risk aversion (higher = more conservative)
CAP_PENALTY_PHI = 0.1             # RBC penalty weight (0 = ignore capital)
KRD_TOL = 0.30                    # KRD bucket tolerance (30%)
USE_KRD_MATCH = False             # Toggle KRD matching on/off
W_MIN_DEFAULT = 0.00              # Min position size (0%)
W_MAX_DEFAULT = 0.10              # Max position size (10%)
N_FRONTIER_POINTS = 200           # Number of portfolios on frontier

# Robustness simulation knobs:
MU_NOISE_SCALE_MKT = 0.04         # Market return shock (4% of median vol)
MU_NOISE_SCALE_ID = 0.05          # Idiosyncratic shock (5% of asset vol)
SIGMA_RHO_JITTER = 0.01           # Correlation jitter (±1%)
SIGMA_VOL_JITTER = 0.02           # Volatility jitter (±2%)
SIGMA_SHRINK_ALPHA = 0.4          # Cov shrinkage toward base (40%)
```

#### Modify Constraints:
- Edit `solve_surplus_qp()` function
- Adjust credit quality, asset class limits, duration bands
- Add custom constraints (e.g., ESG tilts, sector limits)

#### Update CMAs:
- Edit `final_inputs.xlsx` (columns: Asset, Rating, ExpReturn, RBC_Charge, Sigma_Ref, Duration, Company Allocations, Covariance Matrix)
- Re-run optimization

---

## 📚 References & Data Sources

### Academic Literature
- **Markowitz (1952)**: Portfolio Selection, *Journal of Finance*
- **Michaud (1998)**: *Efficient Asset Management: A Practical Guide to Stock Portfolio Optimization and Asset Allocation*
- **Black-Litterman (1992)**: Global Portfolio Optimization, *Financial Analysts Journal*
- **Sharpe & Tint (1990)**: Liabilities - A New Approach, *Journal of Portfolio Management*

### Industry Sources
- **J.P. Morgan Asset Management**: Long-Term Capital Market Assumptions (2024, 2025)
- **AQR Capital Management**: Capital Market Assumptions (2019, 2025)
- **Cambridge Associates**: Private Equity & Real Estate Benchmarks
- **NCREIF**: Real Estate Index Returns (Q2 2025)
- **Prof. Aswath Damodaran (NYU Stern)**: Equity Risk Premium data

### Regulatory & Insurance
- **NAIC** (National Association of Insurance Commissioners): RBC Formulas & Guidelines (2021, 2024)
- **Company 10-Q Filings** (Q3 2024): Brighthouse, Corebridge, MetLife, Principal, Prudential, Voya

### Additional Reading
See `Resources/Literature/` for 21 academic papers on:
- Life insurer strategic asset allocation
- Factor investing and alternative risk premia
- Robust optimization techniques
- Illiquid asset allocation
- Solvency II and regulatory capital

---

## 🎓 Key Learnings & Contributions

### Methodological Innovations
1. **Custom CMA Framework**: Bottom-up sector equity approach combined with building-block fixed income methodology
2. **Company-Specific Liability Modeling**: Three-bucket framework calibrated to actual product mix (versus generic duration matching)
3. **Integrated RBC Penalty**: Explicit capital efficiency in objective function (novel in academic SAA literature)
4. **Coarse KRD Matching**: Pragmatic bucket-based duration hedging (simpler than full-blown KRD but effective)
5. **Robust Optimization Implementation**: Practical resampling with CMA simulation (forecast error decomposition into market + idiosyncratic + covariance components)

### Practical Insights
- **Yield Harvesting via Privates**: Life insurers' comparative advantage in illiquid assets (long liabilities + regulatory accounting treatment) → optimal portfolios favor Direct Lending, Mortgage Loans, and Core Real Estate
- **Public Equity Penalty**: RBC charges make public equity unattractive despite diversification benefits → allocations cluster near 0-5% constraint
- **Credit Quality Sweet Spot**: A/BBB IG corporates offer best risk-return trade-off for insurers (higher yield than AA, lower capital charge than HY)
- **Duration Discipline**: Liability-matching constraints are binding and dramatically shape frontier → ignoring liabilities leads to infeasible/imprudent portfolios
- **Robustness Value**: Resampled portfolios sacrifice ~10-20 bps expected return but gain stability and diversification (worthwhile trade for institutional investors)

### Limitations & Future Work
1. **Static Framework**: Single-period MVO; no rebalancing, regime-switching, or dynamic hedging (VA/FIA Greeks)
2. **Simplified Liabilities**: Exponential decay buckets are stylized; actual cash flows are option-embedded and stochastic (interest rate, mortality, lapse risks)
3. **Liquidity Constraints**: No explicit liquidity modeling (e.g., withdrawal stress scenarios, asset-liability cash flow mismatches)
4. **RBC Simplification**: Asset-only charges; full RBC includes C2 (insurance risk), C3 (interest rate risk), diversification credits
5. **Market Regimes**: CMAs are static; could incorporate regime-dependent assumptions (expansionary vs. recessionary)
6. **ESG Integration**: No ESG scoring or sustainable investing tilts (increasingly important for institutional mandates)
7. **Tail Risk**: Gaussian assumptions; could extend to CVaR, downside risk, or scenario-based stress tests

**Extensions**:
- Multi-period stochastic optimization (dynamic programming)
- Liability cash flow projection models (mortality, lapse, interest-sensitive)
- Regime-switching CMAs (bull/bear/crisis states)
- Transaction costs and turnover constraints
- Illiquidity risk premia modeling (private markets valuation uncertainty)
- ESG factor integration

---

## 👤 Author & Contact

**Giga Nozadze**  
Master's Student, Quantitative Finance & Risk Management  
New York University, Stern School of Business

**LinkedIn**: [linkedin.com/in/giga-nozadze](https://www.linkedin.com/in/giga-nozadze/)  
**GitHub**: [github.com/giganozadze](https://github.com/giganozadze)  
**Email**: gn2196@stern.nyu.edu

---

## 📄 License & Usage

This project is an academic capstone and is provided for **educational and research purposes**. 

**Disclaimers**:
- All Capital Market Assumptions are forward-looking estimates for illustrative purposes only and should not be considered investment advice.
- Company allocation data is derived from public regulatory filings and may not reflect current positions.
- Past performance does not guarantee future results.
- Regulatory requirements and accounting standards are simplified; consult NAIC guidelines for production use.

**Citation**: If you use this work in your research, please cite:
```
Nozadze, G. (2025). Custom-Built Capital Market Assumptions Framework and Strategic 
Asset Allocation for U.S. Life Insurers. NYU Stern Capstone Project.
```

---

## 🙏 Acknowledgments

Special thanks to:
- **NYU Stern Faculty**: For guidance on strategic asset allocation and insurance portfolio management
- **Industry Practitioners**: For insights into life insurer ALM practices and regulatory constraints
- **Research Sources**: J.P. Morgan, AQR, Cambridge Associates, NCREIF, Prof. Damodaran
- **Open-Source Community**: CVXPY, NumPy, Pandas development teams

---

## 📌 Repository Information

**Repository**: [Custom-Built-Capital-Markets-Assumptions-Framework-and-Strategic-Asset-Allocation-for-ULife-Insurers](https://github.com/giganozadze/Custom-Built-Capital-Markets-Assumptions-Framework-and-Strategic-Asset-Allocation-for-ULife-Insurers)

**Status**: ✅ Complete (Fall 2025)  
**Version**: 1.0  
**Last Updated**: October 2025

---

*For questions, suggestions, or collaboration inquiries, please open an issue on GitHub or contact via email.*

