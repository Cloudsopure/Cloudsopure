//@version=5
strategy("Bollinger Bands Strategy", overlay=true)

// Input Parameters
length = input.int(20, title="BB Length")
mult = input.float(2.0, title="BB Multiplier")
target_points = input.int(100, title="Target Points")
stop_loss_points = input.int(50, title="Stop Loss Points")

// Calculate Bollinger Bands
basis = ta.sma(close, length)
dev = mult * ta.stdev(close, length)
upper_band = basis + dev
lower_band = basis - dev

// Strategy Logic
long_condition = ta.crossover(close, lower_band)
short_condition = ta.crossunder(close, upper_band)

// Plot Bollinger Bands
plot(upper_band, color=color.blue, title="Upper Band")
plot(lower_band, color=color.red, title="Lower Band")

// Strategy Entry
if long_condition
    strategy.entry("Long", strategy.long)
if short_condition
    strategy.entry("Short", strategy.short)

// Calculate Target and Stop Loss Levels
long_target = strategy.position_avg_price + target_points
long_stop_loss = strategy.position_avg_price - stop_loss_points
short_target = strategy.position_avg_price - target_points
short_stop_loss = strategy.position_avg_price + stop_loss_points

// Strategy Exit
strategy.exit("Long Exit", "Long", limit=long_target, stop=long_stop_loss)
strategy.exit("Short Exit", "Short", limit
