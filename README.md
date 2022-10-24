# Sherlock Crypto Bot
Sherlock Crypto is a comprehensive tool that allows users to find and analyze crypto arbitrage opportunities. The tool is based on Crypto Arbitrage API from rapidapi.com.

## The tool is composed of:

- [**Telegram bot**](https://t.me/ArbitrageScannerBot) - searches for arbitrage opportunities and sends them to users on a fixed interval (30 minutes)
- PostgreSQL database - collects all events
- Scanner dashboard - helps users manually search arbitrage opportunities 
- Analytical dashboard - gives a thorough overview on both current and historical arbitrage market state

[**Dashboard link**](https://sherlock-crypto-arbitrage-bot-dashboard-homepage-fmy8hj.streamlitapp.com/)

    
## Technologies used
- Bot general:
    - `Aiogram` module for asynchronous operation
    - Webhook
- API requests
    - `Aiohhtp` module for asynchronous requests (over 50 requests asynchronously)
- Database management
    - `Psycopg2` for PostgreSQL
- Dashboard
    - `Streamlit` for general dashboard creation
    - `Plotly` for creating graphs
    
## Tool structure

``` bash                  
│   main.py
│   Procfile
│   requirements.txt
│
├───dashboard
│   │   config.py
│   │   🏠_Homepage.py
│   │
│   ├───data
│   │       exchanges.txt
│   │       pairs.txt
│   │
│   └───pages
│           📊_Analytics.py
│           🔍_Scanner.py
│           🔝_Top-exchanges.py
│
└───utils
        db.py
        parser.py
``` 

### Bot related:
- **_main.py_** - bot’s main file. Creates webhook to telegram API, connects to database using **_utils.db_** module, schedules parsing task on set interval, parses data using **_utils.parser_** module. At the same time instantly reacts to subscribe/unsubscribe callbacks. Implements aiogram module, which is the best asynchronous telegram bot framework in the industry. 
- **_utils/db.py_** - `BotDB` class containing methods for connecting to PostgreSQL database, inserting and fetching arb events data, inserting and updating telegram user’s subscription statuses.
- **_utils/parser.py_** - `Parser` class that inherits from `BotDB`. Contains method for asynchronous RapidAPI parsing using asyncio and aiohttp libraries.

### Dashboard related:
- **_dashboard/🏠_Homepage.py_** - dashboard’s main page containing project description
- **_dashboard/pages/📊_Analytics.py_** - analytics page. Connects directly to PosgreSQL DB and implements 8+ methods for grouping and plotting data using plotly. All changes in DB will be shown on the page after refresh.
- **_dashboard/pages/🔍_Scanner.py_** - contains get_symbols function to parse pairs and exchanges selected by user. Also prints all orders nicely on the page. Collects EXCHANGES and PAIRS from dashboard/config.py. Actually, this module should use `Parser` class from **_utils/parser.py_** to parse API. This will be changed in upcoming updates. 

### Other:
- **_dashboard/config.py_** - config file which is related both to the bot and the dashboard. Situated in a strange place, but from here it is easy to import it into all other modules. The path will be changed in upcoming updates. 

## Deployment logic
- Heroku:
    - Telegram bot
    - PostgreSQL database
- Streamlit cloud:
    - Dashboard

***
**P.S.:** currently telegram bot is not working because Heroku changed its free plan policy. However, the dashboard remains operational
