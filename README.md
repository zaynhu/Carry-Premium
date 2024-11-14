```
from AlgorithmImports import *
from datetime import timedelta

class BondCarryOptimization(QCAlgorithm):
    def Initialize(self):
        self.SetStartDate(2015, 1, 1)
        self.SetEndDate(2024, 11, 8)
        self.SetCash(100000)
        
        # Add bond ETFs
        self.bond_etfs = {
            'TLT': self.AddEquity('TLT', Resolution.Daily),
            'LQD': self.AddEquity('LQD', Resolution.Daily),
            'JNK': self.AddEquity('JNK', Resolution.Daily),
            'EMB': self.AddEquity('EMB', Resolution.Daily)
        }
        
        # Monthly rebalance
        self.Schedule.On(self.DateRules.MonthStart('TLT'), self.TimeRules.AfterMarketOpen('TLT'), self.Rebalance)

    def Rebalance(self):
        # Get yields for each ETF (approximation using dividend yield or price changes as a proxy)
        yields = {symbol: self.GetYield(symbol) for symbol in self.bond_etfs.keys()}
        
        # Calculate weights based on optimization logic
        # Here, we prioritize high yield (carry) while keeping diversification
        total_yield = sum(yields.values())
        weights = {symbol: yield_val / total_yield for symbol, yield_val in yields.items()}

        # Set portfolio weights
        for symbol, weight in weights.items():
            self.SetHoldings(symbol, weight)
            self.Debug(f"Setting {symbol} weight to {weight:.2%}")

    def GetYield(self, symbol):
        # Retrieve the dividend history for the past year
        dividend_history = self.History(Dividend, symbol, timedelta(days=365), Resolution.Daily)
        
        # Check if dividend history has data and contains a 'dividends' column
        if not dividend_history.empty and 'dividends' in dividend_history.columns:
            total_dividends = dividend_history['dividends'].sum()
            current_price = self.Securities[symbol].Price
            return total_dividends / current_price if current_price > 0 else 0
        else:
            # Return a placeholder yield if no dividend data is available
            return 0.03  # Placeholder default yield

    def OnData(self, data):
        # Handle data events, check for new rebalance, etc.
        pass
```
