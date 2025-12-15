# Binance Futures Trading Bot

A simplified Python trading bot for Binance Futures Testnet supporting market, limit, and stop-limit orders with comprehensive logging and CLI interface.

## Features

 **Market Orders** - Instant execution at current market price
 **Limit Orders** - Buy/sell at specified price with GTC (Good Till Cancel) support
 **Stop-Limit Orders** - Advanced order type with trigger price and limit price
 **Comprehensive Logging** - All API requests, responses, and errors logged to file
 **CLI Interface** - User-friendly command-line interface with input validation
 **Error Handling** - Robust exception handling for Binance API errors
 **Testnet Support** - Configured for Binance Futures Testnet (USDT-M Futures)

## Project Structure

```
binance-futures-trading-bot/
├── config.py              # Configuration management
├── logger_setup.py        # Logging configuration
├── basic_bot.py           # Core Bot class implementation
├── cli.py                 # CLI interface and argument parsing
├── requirements.txt       # Python dependencies
├── .env.example           # Example environment variables
├── .gitignore            # Git ignore rules
└── README.md             # This file
```

## Installation

### 1. Clone the Repository
```bash
git clone https://github.com/KUBENDHIRAN-V-B/binance-futures-trading-bot
cd binance-futures-trading-bot
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Setup Environment Variables

1. Go to [Binance Futures Testnet](https://testnet.binancefuture.com)
2. Register and create an account
3. Navigate to API Management (Account → Settings → API Key)
4. Create API key and secret
5. Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```
6. Edit `.env` and add your Binance Testnet credentials:
   ```
   BINANCE_API_KEY=your_testnet_api_key
   BINANCE_API_SECRET=your_testnet_api_secret
   TESTNET=True
   ```

## Usage

### Command-Line Interface Examples

#### Place a Market BUY Order
```bash
python cli.py --symbol BTCUSDT --type market --side buy --qty 0.001
```

#### Place a Limit SELL Order
```bash
python cli.py --symbol ETHUSDT --type limit --side sell --qty 0.01 --price 2500
```

#### Place a Stop-Limit Order
```bash
python cli.py --symbol BTCUSDT --type stop-limit --side buy --qty 0.001 --price 40000 --stop-price 39500
```

#### Interactive Mode
```bash
python cli.py
```

The bot will prompt you for order details interactively.

## Code Examples

### Using the BasicBot Class Directly

```python
from basic_bot import BasicBot
from config import BINANCE_API_KEY, BINANCE_API_SECRET, TESTNET

# Initialize bot
bot = BasicBot(BINANCE_API_KEY, BINANCE_API_SECRET, testnet=TESTNET)

# Place market order
order = bot.place_market_order(
    symbol='BTCUSDT',
    side='BUY',
    quantity=0.001
)

if order:
    print(f"Order placed: {order['orderId']}")

# Place limit order
limit_order = bot.place_limit_order(
    symbol='ETHUSDT',
    side='SELL',
    quantity=0.01,
    price=2500
)

# Place stop-limit order (bonus feature)
stop_order = bot.place_stop_limit_order(
    symbol='BTCUSDT',
    side='BUY',
    quantity=0.001,
    price=40000,
    stop_price=39500
)
```

## Logging

All bot activity is logged to `logs/bot.log` with the following information:

- **API Requests**: Symbol, order type, side, quantity, price
- **API Responses**: Order ID, status, execution details
- **Errors**: Validation errors, API errors, exceptions with full stack traces
- **Timestamps**: All events are timestamped

### Sample Log Output
```
2024-12-15 10:30:45 - basic_bot - INFO - Bot initialized. Testnet mode: True
2024-12-15 10:31:20 - basic_bot - INFO - Placing MARKET order: BUY 0.001 BTCUSDT
2024-12-15 10:31:21 - basic_bot - INFO - Market order placed successfully: {...}
2024-12-15 10:32:05 - basic_bot - INFO - Placing LIMIT order: SELL 0.01 ETHUSDT @ 2500
2024-12-15 10:32:06 - basic_bot - INFO - Limit order placed successfully: {...}
```

## API Methods

### BasicBot Class Methods

#### `place_market_order(symbol, side, quantity)`
Places an immediate market order.

**Parameters:**
- `symbol` (str): Trading pair, e.g., 'BTCUSDT'
- `side` (str): 'BUY' or 'SELL'
- `quantity` (float): Order quantity

**Returns:** Order details dict or None if failed

#### `place_limit_order(symbol, side, quantity, price, time_in_force="GTC")`
Places a limit order with specified price.

**Parameters:**
- `symbol` (str): Trading pair
- `side` (str): 'BUY' or 'SELL'
- `quantity` (float): Order quantity
- `price` (float): Limit price
- `time_in_force` (str): 'GTC' or 'IOC'

**Returns:** Order details dict or None if failed

#### `place_stop_limit_order(symbol, side, quantity, price, stop_price)`
Places a stop-limit order with trigger price.

**Parameters:**
- `symbol` (str): Trading pair
- `side` (str): 'BUY' or 'SELL'
- `quantity` (float): Order quantity
- `price` (float): Limit price (execute at this price)
- `stop_price` (float): Stop/trigger price

**Returns:** Order details dict or None if failed

#### `get_order_status(symbol, order_id)`
Fetches the status of an existing order.

#### `cancel_order(symbol, order_id)`
Cancels an existing open order.

## Error Handling

The bot includes comprehensive error handling for:

- **API Errors**: Binance API exceptions with error codes and messages
- **Validation Errors**: Input validation for symbols, sides, quantities, and prices
- **Network Errors**: Connection issues with detailed logging
- **Order Errors**: Invalid order parameters or market conditions

## Requirements

- Python 3.8+
- python-binance 1.0.17
- python-dotenv 1.0.0
- requests 2.31.0

## Bonus Features Implemented

 **Stop-Limit Orders** - Advanced order type supporting trigger prices
 **Comprehensive Logging** - File-based logging with detailed error tracking
 **Input Validation** - Robust validation of all user inputs
 **Order Status Tracking** - Methods to check and cancel orders

## Testing

The bot has been tested with:
- Binance Futures Testnet API
- Market orders execution
- Limit orders with various price points
- Stop-limit orders with trigger conditions
- Error scenarios and edge cases

## Future Enhancements

- [ ] WebSocket support for real-time order updates
- [ ] Grid trading strategy
- [ ] TWAP (Time-Weighted Average Price) orders
- [ ] Portfolio management features
- [ ] Dashboard/UI interface
- [ ] Backtesting framework

## Troubleshooting

### "ModuleNotFoundError: No module named 'binance'"
```bash
pip install python-binance
```

### "BinanceAPIException: APIError(code=-1022): Signature for this request invalid"
- Check that API key and secret are correct in `.env`
- Ensure your system time is synchronized

### "Cannot place order on Testnet"
- Verify you're using testnet credentials from `testnet.binancefuture.com`
- Ensure `TESTNET=True` in `.env`

## License

MIT License - Feel free to use this project for educational purposes.

## Author

Developed as a Junior Python Developer assignment for Bajaj Angs Crypto Trading Bot Position.

