import requests
import matplotlib.pyplot as plt
from datetime import datetime

API_BASE = "https://api.coingecko.com/api/v3"

def get_price(coin="bitcoin", currency="usd"):
    url = f"{API_BASE}/simple/price"
    params = {"ids": coin, "vs_currencies": currency}
    r = requests.get(url, params=params)
    data = r.json()
    return data[coin][currency]

def get_history(coin="bitcoin", currency="usd", days=7):
    url = f"{API_BASE}/coins/{coin}/market_chart"
    params = {"vs_currency": currency, "days": days}
    r = requests.get(url, params=params)
    data = r.json()
    return data["prices"]

def plot_chart(prices, coin="bitcoin"):
    dates = [datetime.fromtimestamp(p[0] / 1000) for p in prices]
    values = [p[1] for p in prices]

    plt.figure(figsize=(10, 5))
    plt.plot(dates, values, label=f"{coin.capitalize()} Price", linewidth=2)
    plt.title(f"{coin.capitalize()} - Last 7 Days")
    plt.xlabel("Date")
    plt.ylabel("Price (USD)")
    plt.legend()
    plt.grid(True)
    plt.show()

def main():
    coin = input("Enter coin (e.g. bitcoin, ethereum, dogecoin): ").lower()
    currency = "usd"

    # Live price
    price = get_price(coin, currency)
    print(f"💰 Current {coin.capitalize()} price: {price} {currency.upper()}")

    # History chart
    prices = get_history(coin, currency)
    plot_chart(prices, coin)

if __name__ == "__main__":
    main()

