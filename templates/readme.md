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