# parmkumar93-XAUUSD-HF-Scalper-MT5
README — bot details
What it is
High-frequency XAUUSD scalper for MetaTrader 5. Default chart/timeframe is M1. It trades gold using a simple filter stack:

Fast EMA vs slow EMA (direction)
RSI (avoid extremes)
ADX (minimum trend strength)
ATR (stop loss / take profit distance)
Spread cap + per-bar / per-minute / cooldown limits
Magic number: 777001 (hardcoded)
Version: 1.04
Language: MQL5
File: XAUUSD_HF_Scalper_fixed.mq5

Strategy logic (exact)
Long when all are true:

EMA fast > EMA slow
RSI < overbought (76)
ADX ≥ threshold (12)
Short when all are true:

EMA fast < EMA slow
RSI > oversold (24)
ADX ≥ threshold (12)
This is not an EMA crossover. It is a state filter. While the condition stays true, it can keep trying to enter (limited by bar/minute/cooldown).

Stops (ATR-based):

SL = ATR × 0.45
TP = ATR × 0.35
That is R:R ≈ 1 : 0.78 (TP smaller than SL). It needs a high win rate or it will bleed.

Indicators (current bar, shift 0):

Indicator	Period	Notes
EMA fast	5	Close
EMA slow	13	Close
RSI	7	OB 76 / OS 24
ADX	14	Main line only (no +DI/−DI)
ATR	14	SL/TP
Features
XAUUSD default, any symbol/TF via inputs
Fixed lot or % risk
Spread filter (skip if spread too wide)
Max 1 trade per bar
Max 3 trades per minute
500 ms cooldown between attempts
Trailing stop after 60% of the way to TP
Combined floating-profit target: close all EA positions at $10,000 (default)
Magic isolation (777001)
Optional “trade only chart symbol”
Not implemented in v1.04:

Partial close is an input, but the close call is commented out
No filling-mode auto-detect (FOK/IOC)
No max-open-trades cap (only bar/minute limits)
No global loss limit
No opposite-position close on reverse signal
Inputs (defaults)
Input	Default	Meaning
InpSymbol	XAUUSD	Symbol
InpTF	M1	Signal timeframe
UsePercentRisk	false	false = fixed lot
FixedLot	0.10	Lot if % risk off
RiskPercent	0.5	% of balance if on
EMA_fast_period	5	Fast EMA
EMA_slow_period	13	Slow EMA
RSI_period	7	RSI
RSI_overbought	76	Block longs above this
RSI_oversold	24	Block shorts below this
ADX_period	14	ADX
ADX_threshold	12	Min ADX
ATR_period	14	ATR
ATR_SL_mult	0.45	SL distance
ATR_TP_mult	0.35	TP distance
MaxTradesPerBar	1	Rate limit
MaxTradesPerMinute	3	Rate limit
CooldownMs	500	Min ms between attempts
SlippagePoints	12	Deviation
MaxSpreadPoints	25	Skip if spread wider
UseTrailing	true	Trail SL
TrailingStartPct	0.6	Start trail at 60% of TP path
TrailStepPoints	5	Min SL move
PartialCloseEnable	false	Does nothing in v1.04
PartialClosePercent	0.5	Unused
OnlyOnChartSymbol	false	Chart-only mode
ProfitTarget	10000	Close all EA positions at this $ profit
