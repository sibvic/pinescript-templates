# Templates

## MTF Heatmap

Shows indicator values for several timeframes as a heatmap. Insert your logic between "User defined section" block. By assigning 1/-1 to direction you can define the color of the heatmap (green/red).

## Divergence

Divergence indicator. Insert your logic between "User defined section" block and assign you indicator to the indi variable. 

It searches for peaks on the indicator (not on the price!):

* when the indicator values greater than two values before and after that value it's a peak
* when the indicator values lesser than two values before and after that value it's a trough

NOTE: The peak/trough could be detected only after two new bars are formed! 

The indicator remembers high/low price on the bar with the peak/trough. When the indicator value at the peak is lower than the indicator value at the previous peak and the low price at the current peak is higher than the low price at last peak then it's a hidden bullish . Vice versa for the trough.

So, this indicator can detect the next cases:

* indicator fall + high rise = regular bearish
* indicator rise + high fall = hidden bearish
* indicator rise + low fall = regular bullish
* indicator fall + low rise = hidden bullish

## Study

Study with alerts

* End of turn Alerts

  Backtester in the tradingview works on candle close and the forwardtesting can work on candle close or on each tick (depending on the Recalculate on every tick parameter of the strategy). When you add the strategy to the chart the strategy work on all historical data up till the moment you add it in the backtesting mode (on candle close only). And all new bars/candle executed in forwardtesting mode (can be on candle close or on every tick). But after you refresh the chart/reload the page or change the chart parameters (instrument, period etc) the strategy will recalculate all data (including the candles calculated in the forwardtesting mode) in the backtesting mode. This can case the repainting issue. According to my research the "Recalculate On Every Tick" doesn't work and the strategy forwardtested in every tick (it looks like a bug in TradingView).
  The study works the same - on candle close for the historical data and on every tick for the new candles. The study doesn't have a built-in option to calculate the values on candle close only. This parameter designed to do exactly that. Turn it on to get the study calculated on candle close. This helps to avoid the repainting issue for the study.

* Show stop (Debug)
  
  This option designed to help you understand where the stops are placed. 

Also, the study/indicator have more outputs to mimic the strategy. All trading events do have alerts.

* Background is red.
  This is how the study highlights the stop hit.

* Background is yellow.
  This is how the study highlights the stop hit after moving stop into profit (using breakeven or tailing SL).

* Background is green.
  This is how the study highlights the limit hit.

* The lime color "E" with the line color arrow below the candle
  Exit buy/long signal

* The red color "E" with the red arrow above the candle
  Exit Sell/short signal

* The lime color "B" with the line color arrow below the candle
  Buy/long signal

* The red "S" with the red arrow above the candle
  Sell/short signal

## Strategy

Strategy

Here how the parameters are working:
* Stop loss/Stop

  Stop loss depends on the three parameres: Stop loss type, Stop loss and Stop ATR Length
  Stop loss do have several modes:
  - Do not use
    There will be no stop for the positions
  - Fixed rate shift
    This is the fixed rate stop loss specified in the "Stop loss" parameters. The value should be always positive. It a simple shift.
    Ex.: "Stop loss" 0.001 for the EUR/USD 1.20 buy position will give you 1.199 stop. For the sell position it will be 1.201
  - ATR
    Dynamic stop based on Average true range. You can specify the ATR length in the "Stop ATR Length" parameter. The strategy will calculate ATR for each bar and set it as a stop (lower that the open price for buy and higher for sell).
  - Pips
    Fixed stop specified in pips.
    Ex.: "Stop loss" 10 for the EUR/USD 1.20 buy position will give you 1.199 stop. For the sell position it will be 1.201

* SL/TP Trailing

  Start trailing stop loss on "Trailing distance" pips from the most profitable point after "Trailing start after" value is hit.

* Breakeven

  This logic moves stop to the breakeven level after the the price goes to the favour direction on x pips. 
  The "Use breakeven" parameter turns on/off this logic. The stop will be moved to x pips in profit which you can specify in the "Move stop to, pips" parameter after the position goes to y pip in profit specified in the "Move stop on profit, pips" parameter.

* Take Profit/Limit

  Take Profit depends on the three parameres: Take Profit type, Take Profit and Limit ATR Length
  Take Profit do have several modes:
  - Do not use
    There will be no take profit for the positions
  - Fixed rate shift
    This is the fixed rate take profit specified in the "Take profit" parameters. The value should be always positive. It a simple shift.
    Ex.: "Take profit" 0.001 for the EUR/USD 1.20 buy position will give you 1.121 stop. For the sell position it will be 1.199
  - ATR
    Dynamic take profit based on Average true range. You can specify the ATR length in the "Limit ATR Length" parameter. The strategy will calculate ATR for each bar and set it as a take profit (lower that the open price for buy and higher for sell).
  - Pips
    Take profit specified in pips.
    Ex.: "Take profit" 10 for the EUR/USD 1.20 buy position will give you 1.121 stop. For the sell position it will be 1.199

Known issues:
 - The "Recalculate on every tick" parameter of the strategy doesn't work. It looks like a TradingView bug and I don't have a workaround for it. It cases the repainting issue for the strategy.
 - Backtester in some cases leaves the trade open despite the close command. It even puts an event on the chart signaling that the position is closed, but Pine Script commands still thinks the position is open. It's a TradingView bug. This cases the strategy to stop trading for some number of bars. Backtesting results will be invalid. In some strategies you can use a workaround - turn the pyramiding on with the "Allow Up To" parameter > 1
 - Pine Script doesn't give enough language features to work with multiple positions. It makes impossible to properly implement working stop/limit logic for the multiple positions.