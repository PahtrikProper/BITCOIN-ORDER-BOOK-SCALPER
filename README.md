"""
## Overview

This trading bot is designed to perform live trading on the Binance exchange for the `BTC/USDT` trading pair. The bot uses the `ccxt` library to interact with the Binance API and bases its trading decisions purely on order book analysis. The primary goal is to buy and sell based on detected market conditions, volume imbalances, and the presence of large orders (whales).

## Strategy

The bot makes trading decisions based on the following strategy:

1. **Order Book Analysis**: Analyzes the top 10 levels of the order book to detect market conditions, volume imbalances, and large orders.
2. **Market Conditions**:
    - **Bullish**: More buy volume than sell volume, detected large bid orders, no significant sell walls.
    - **Bearish**: More sell volume than buy volume, detected large ask orders, or significant sell walls.
    - **Neutral**: Neither bullish nor bearish conditions detected.
3. **Trading Logic**:
    - **Buy**: Place a buy order when there is an opportunity to make at least 0.44% profit based on the order book analysis.
    - **Sell**: Place a sell order when a profit target is met or if the market condition changes to bearish during an active trade. Do not sell if the equivalent balance of BTC is less than 50 USDT.

## Configuration Parameters

- `SYMBOL`: The trading pair (default is `BTC/USDT`).
- `ORDER_BOOK_DEPTH`: Number of levels in the order book to analyze (default is 20).
- `TRADE_AMOUNT`: Fixed amount in USDT to trade each time (default is 300 USDT).
- `TRADE_INTERVAL_SECONDS`: Interval between trade checks (default is 5 seconds).
- `PROFIT_PERCENTAGE`: Minimum profit target for selling (default is 0.44%).
- `VOLUME_IMBALANCE_THRESHOLD`: Threshold for detecting volume imbalances (default is 1.1).
- `SELL_WALL_THRESHOLD`: Threshold for detecting large sell walls (default is 2.0).
- `LARGE_ORDER_THRESHOLD`: Threshold for detecting large orders (default is 5% of total volume).
- `MIN_SELL_BALANCE_USDT`: Minimum balance in USDT equivalent to initiate a sell (default is 50 USDT).

## Functions

### `fetch_order_book(symbol, limit=ORDER_BOOK_DEPTH)`

Fetches the order book for the specified symbol and depth. Handles network, exchange, and rate limit errors.

### `analyze_order_book(order_book)`

Analyzes the top 10 levels of the order book to determine:
- Volume imbalance between bids and asks.
- Presence of large sell walls.
- Large orders in bids and asks.
- Ideal exit price for a profitable sell.
- Market condition (bullish, bearish, or neutral).

### `place_order(symbol, side, amount, price=None)`

Places a limit buy or sell order on the exchange. Logs the order details and handles errors.

### `get_current_balance(asset)`

Fetches the current available balance of the specified asset. Handles errors.

### `check_open_orders(symbol)`

Checks for any open orders for the specified symbol. Returns the list of open orders.

### `cancel_order(order_id, symbol)`

Cancels the specified order for the symbol. Handles errors and logs the cancellations.

### `trading_bot(symbol)`

The main trading bot function:
- Checks the order book and market conditions at regular intervals.
- Places buy orders when there is an opportunity to make at least 0.44% profit.
- Places sell orders when profit targets are met or market conditions change to bearish. Ensures sufficient balance to sell.
- Cancels and replaces orders based on real-time analysis of the order book.

## How to Run

1. **Install Dependencies**: Ensure you have `ccxt`, `python-dotenv`, and other required libraries installed.
    ```bash
    pip install ccxt python-dotenv
    ```

2. **Set Up Environment Variables**: Create a `.env` file with your Binance API credentials.
    ```
    BINANCE_API_KEY=your_api_key
    BINANCE_API_SECRET=your_api_secret
    ```

3. **Run the Bot**: Execute the script.
    ```bash
    python your_script_name.py
    ```

## Logging

The bot uses Python's built-in logging module to log information, warnings, and errors. Logs include details about placed orders, detected market conditions, balance updates, and any errors encountered during execution.

## Disclaimer

This bot is for educational purposes only. Trading cryptocurrencies involves significant risk, and you should not trade with money you cannot afford to lose. The author is not responsible for any financial losses incurred while using this bot.

---

Feel free to modify the parameters and improve the strategy according to your needs. Happy trading!
"""
