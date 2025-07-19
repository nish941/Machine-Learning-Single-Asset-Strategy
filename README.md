# Machine-Learning-Single-Asset-Strategy
Design a Machine Learning Single Asset Strategy using Pfizer stock [PFE] from 20100101 [YYYYMMDD] to 20250416 [YYYYMMDD].

Every day, you can decide to either buy [Long] or sell [Short] USD 1 Million worth of the stock. The model must take only one position/decision [Long/Short] on any given day.

You may use any publicly available data for creating features/models.

Research Directions:
  - This submission expects creativity at all stages
  - Design and test interesting features beyond standard ones like stock_return, change_in_volume, etc.
  - The strategy is expected to outperform a baseline strategy where USD 1 Million is held long every trading day.
  - Example: Sentiment around Pfizer stock, market share of most popular drug from Pfizer etc. 
 - Candidates are free to use other stock information as well, with the only constraint being no lookahead bias.

⚠️ Key points to remember:

  - [No Lookahead Bias] Ensure there is no forward bias in your data extraction, feature engineering, or modelling.
  - [No Lookahead Bias] The Buy/Sell [Long/Short] decision on date "di" must not be influenced in any way by any data from dates > di.
  - [Normalized Features] When using technical features like close or volume, normalize them using a rolling window to avoid adjustment biases.


📁 The submission should be a [.zip file] containing:

    A: .ipynb file demonstrating:
     - Data extraction
     - Data cleaning
     - Proper handling of non-numeric features
     - Feature engineering
     - Modelling: The target of the model can be the next day return of PFE stock,   i.e., using all features on date di, predict the return on di+1
     - Training vs Validation vs Out-of-Sample splits
     - Data from 20220101 to 20250416 should be treated as pure out-of-sample

    B: Detailed pdf report explaining the same

PnL Calculation:
  - PnL should be based on changes in daily closing prices.
  - PnL can be marked as the difference between yesterday's close and today’s close, multiplied by the position.
  - Ignore corporate actions, dividends, and other adjustments.

Data Representation:
  - Data should be structured as a 2D NumPy array with shape (1 x num_dates).
  - Each row corresponds to the stock, and each column represents the closing price/portfolio position on a specific date.

Avoid Forward Bias:
  - Future data must not be used in decision-making.
  - If a position is taken on date di, ensure only data available up to di is used for signal generation.
  - Strategies using rolling statistics (e.g., moving averages, z-scores) must rely on past data only.

PnL Visualization:
  - Generate a PnL plot showing cumulative returns over time.
  - The plot should clearly depict the performance trend of the strategy.
