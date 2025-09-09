# Task-2
# The Developers Arena

import requests

# Get your free API key from https://openweathermap.org/api
API_KEY = "your_api_key_here"
BASE_URL = "http://api.openweathermap.org/data/2.5/weather"

def get_weather(city):
    try:
        params = {
            "q": city,
            "appid": API_KEY,
            "units": "metric"  # for Celsius
        }
        response = requests.get(BASE_URL, params=params)
        response.raise_for_status()  # raises HTTPError for bad requests

        data = response.json()

        # Extract info
        city_name = data["name"]
        country = data["sys"]["country"]
        temp = data["main"]["temp"]
        humidity = data["main"]["humidity"]
        description = data["weather"][0]["description"]

        print(f"\n📍 Weather in {city_name}, {country}:")
        print(f"🌡️ Temperature: {temp}°C")
        print(f"💧 Humidity: {humidity}%")
        print(f"☁️ Condition: {description.capitalize()}\n")

    except requests.exceptions.HTTPError:
        print("❌ City not found. Please try again.")
    except requests.exceptions.RequestException as e:
        print("⚠️ Network error:", e)
    except KeyError:
        print("⚠️ Unexpected response format.")

def main():
    while True:
        city = input("Enter city name (or 'exit' to quit): ")
        if city.lower() == "exit":
            print("👋 Exiting Weather Client. Goodbye!")
            break
        get_weather(city)

if __name__ == "__main__":
    main()
