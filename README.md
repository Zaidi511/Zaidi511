import requests
from telegram import Bot
import schedule
import time

# Your Telegram bot token
TOKEN = 'your-telegram-bot-token'
# Your chat ID to receive messages (get it from your Telegram app)
CHAT_ID = 'your-chat-id'

bot = Bot(token=TOKEN)

def fetch_meme_coin_data():
    url = "https://api.example.com/meme-coins"  # Replace with the actual API or scraping URL
    response = requests.get(url)
    data = response.json()
    return data

def filter_coins(data):
    filtered_coins = []
    for coin in data:
        market_cap = coin.get('market_cap', 0)
        # Check if market cap is around 250k
        if 240000 <= market_cap <= 260000:
            filtered_coins.append(coin)
    return filtered_coins

def send_message(message):
    bot.send_message(chat_id=CHAT_ID, text=message)

def job():
    data = fetch_meme_coin_data()
    filtered_coins = filter_coins(data)
    for coin in filtered_coins:
        message = f"New Meme Coin: {coin['name']} - Market Cap: {coin['market_cap']}"
        send_message(message)

# Schedule the job to run every hour
schedule.every(1).hours.do(job)

while True:
    schedule.run_pending()
    time.sleep(1)
