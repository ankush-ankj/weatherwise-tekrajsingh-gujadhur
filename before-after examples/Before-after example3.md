---
title: "Before/After Example: Weather Data Retrieval Function"
author: "Tekrajsingh Gujadhur"
format: 
  pdf:
    toc: false
    colorlinks: true
  docx:
    toc: false
    highlight-style: github
  html:
    toc: true
    toc-expand: 2
    embed-resources: true
---


## Initial Prompt
Create a Python program that asks the user for a city name and prints the current weather in that city using the wttr.in API. The program should display temperature, weather condition, and humidity. Keep the code simple and beginner-friendly.


## Initial AI-Generated Code (Before)
import requests

def get_weather(city):
    url = f"https://wttr.in/{city}?format=j1"
    try:
        response = requests.get(url)
        data = response.json()
        
        current = data["current_condition"][0]
        temperature = current["temp_C"]
        condition = current["weatherDesc"][0]["value"]
        humidity = current["humidity"]

        print(f"\nWeather in {city.title()}:")
        print(f"  Temperature: {temperature}°C")
        print(f"  Condition: {condition}")
        print(f"  Humidity: {humidity}%")

    except Exception as e:
        print("Failed to fetch weather data.")
        print(f"Error: {e}")

if __name__ == "__main__":
    city = input("Enter a city name: ")
    get_weather(city)


## My Analysis of the Initial Code
My Analysis of the Initial Code
The initial code successfully delivers a basic weather reporting tool using the wttr.in API. It follows a simple structure and achieves the goal of fetching and displaying current weather conditions for a given city. However, while functional, the code is quite limited in both features and design. Below is a detailed analysis of its strengths and weaknesses:

✅ Strengths
Simplicity and Readability
The code is easy to understand, especially for beginners. It uses straightforward logic with clear variable names and simple flow.

Working API Integration
It correctly connects to the wttr.in API and extracts key data such as temperature, condition, and humidity, making it functional for basic use cases.

Basic Error Handling
A try-except block is used to prevent crashes when the API request fails or JSON parsing encounters issues, which adds some resilience.

❌ Limitations
No Modularity
The entire logic is written in a single function (get_weather), which makes it difficult to extend or reuse. There’s no separation of concerns (e.g., no data parser or UI handler).

No Support for Forecasts
The app only shows current weather. It does not support multi-day forecasts or more detailed insights like precipitation, wind speed, or temperature trends.

No Input Validation
User input is taken directly without validation, which could cause unexpected behavior with incorrect or malformed input (e.g., empty strings, special characters).

No User Interface Features
There’s no interactive menu or guidance—just a single prompt and print output. This limits usability and engagement.

Lack of Visual Feedback
The program could be more insightful with charts or visual summaries to enhance user experience.

No Natural Language Support
Users must know to enter just a city name—there's no parsing of natural language questions like "What’s the weather in Paris tomorrow?"


## My Follow-up Prompt
My Follow-up Prompt
“Improve this Python weather application by organizing the code into modular classes for fetching, parsing, and displaying weather data. Add support for 3-day forecasts, natural language question parsing (like ‘Will it rain tomorrow in Paris?’), and simple data visualization using matplotlib. The program should include a user-friendly menu to navigate features.”

✅ Why This Prompt Is Effective:
It encourages modular design using classes, making the code easier to maintain and extend.

It adds natural language parsing, introducing basic AI logic for better UX.

It includes data visualization for temperature and precipitation trends, making the app more engaging.

It requests a menu-driven interface, enhancing user interaction and clarity.

It sets clear functional goals (forecast support, class structure, parsing logic).



## Improved AI-Generated Code (After)

import requests
import matplotlib.pyplot as plt
from datetime import datetime
from typing import Optional, Dict, List


class WeatherFetcher:
    """Fetches weather data from wttr.in API."""

    BASE_URL = "https://wttr.in/{location}?format=j1"

    def __init__(self):
        self.session = requests.Session()
        self.session.headers.update({'User-Agent': 'Weather Advisor App'})

    def get_weather(self, location: str, days: int = 3) -> Optional[Dict]:
        try:
            response = self.session.get(self.BASE_URL.format(location=location))
            response.raise_for_status()
            raw = response.json()
            return self._structure_data(raw, days)
        except requests.RequestException as e:
            print(f"Error fetching data: {e}")
            return None

    def _structure_data(self, raw_data: Dict, days: int) -> Dict:
        current = raw_data["current_condition"][0]
        forecast = raw_data["weather"][:days]

        return {
            "current": {
                "temp_C": current["temp_C"],
                "weatherDesc": current["weatherDesc"][0]["value"],
                "humidity": current["humidity"],
                "precipMM": current["precipMM"],
                "windspeedKmph": current["windspeedKmph"],
            },
            "forecast": [
                {
                    "date": day["date"],
                    "maxtempC": day["maxtempC"],
                    "mintempC": day["mintempC"],
                    "avgtempC": day["avgtempC"],
                    "precipMM": day["hourly"][0]["precipMM"],
                    "weatherDesc": day["hourly"][0]["weatherDesc"][0]["value"],
                }
                for day in forecast
            ],
        }


class WeatherParser:
    """Parses simple natural language questions."""

    TIME_KEYWORDS = {
        "today": ["today", "now", "current"],
        "tomorrow": ["tomorrow", "next day"],
        "day_after": ["day after tomorrow"],
    }

    ATTRIBUTE_KEYWORDS = {
        "temperature": ["temp", "temperature", "hot", "cold", "warm"],
        "precipitation": ["rain", "snow", "precipitation", "shower"],
        "humidity": ["humid", "humidity"],
        "wind": ["wind", "breezy", "gust"],
        "general": ["weather", "condition", "forecast"],
    }

    def parse_question(self, question: str) -> Dict:
        question_lower = question.lower()
        time = "today"
        attribute = "general"
        location = None

        # Extract time
        for t_key, keywords in self.TIME_KEYWORDS.items():
            if any(k in question_lower for k in keywords):
                time = t_key
                break

        # Extract attribute
        for attr, keywords in self.ATTRIBUTE_KEYWORDS.items():
            if any(k in question_lower for k in keywords):
                attribute = attr
                break

        # Extract location (look for "in <location>")
        if " in " in question_lower:
            parts = question_lower.split(" in ")
            loc_part = parts[1].split()[0]  # Take first word after 'in'
            location = loc_part.strip("?,.!")

        return {"time": time, "attribute": attribute, "location": location}


class WeatherVisualizer:
    """Displays weather data charts."""

    def show_temperature_trend(self, forecast: List[Dict]):
        dates = [datetime.strptime(day["date"], "%Y-%m-%d").strftime("%a %d") for day in forecast]
        max_temps = [float(day["maxtempC"]) for day in forecast]
        min_temps = [float(day["mintempC"]) for day in forecast]

        plt.plot(dates, max_temps, label="Max Temp (°C)", color="red", marker="o")
        plt.plot(dates, min_temps, label="Min Temp (°C)", color="blue", marker="o")
        plt.fill_between(dates, min_temps, max_temps, color="gray", alpha=0.2)

        plt.title("Temperature Trend")
        plt.xlabel("Date")
        plt.ylabel("Temperature (°C)")
        plt.legend()
        plt.tight_layout()
        plt.show()

    def show_precipitation(self, forecast: List[Dict]):
        dates = [datetime.strptime(day["date"], "%Y-%m-%d").strftime("%a %d") for day in forecast]
        precipitations = [float(day["precipMM"]) for day in forecast]

        plt.bar(dates, precipitations, color="blue", alpha=0.6)
        plt.title("Precipitation Forecast (mm)")
        plt.xlabel("Date")
        plt.ylabel("Precipitation (mm)")
        plt.tight_layout()
        plt.show()


class WeatherApp:
    """Main app class handling user interaction."""

    def __init__(self):
        self.fetcher = WeatherFetcher()
        self.parser = WeatherParser()
        self.visualizer = WeatherVisualizer()
        self.location = None
        self.weather_data = None

    def run(self):
        print("🌤️ Welcome to the Weather Advisor App! 🌤️")
        while True:
            print("\nMenu:")
            print("1. Set Location")
            print("2. Show Current Weather")
            print("3. Ask a Weather Question")
            print("4. Show 3-Day Forecast")
            print("5. Visualize Weather Data")
            print("6. Exit")

            choice = input("Enter your choice (1-6): ").strip()
            if choice == "1":
                self.set_location()
            elif choice == "2":
                self.show_current_weather()
            elif choice == "3":
                self.ask_question()
            elif choice == "4":
                self.show_forecast()
            elif choice == "5":
                self.show_visualizations()
            elif choice == "6":
                print("Goodbye! 👋")
                break
            else:
                print("Invalid choice. Please select a valid option.")

    def set_location(self):
        loc = input("Enter city or location: ").strip()
        if not loc:
            print("Location cannot be empty.")
            return

        print(f"Fetching weather data for {loc}...")
        data = self.fetcher.get_weather(loc)
        if data:
            self.location = loc
            self.weather_data = data
            print(f"Weather data loaded for {loc}.")
        else:
            print("Failed to load weather data. Please try again.")

    def show_current_weather(self):
        if not self._check_data():
            return

        current = self.weather_data["current"]
        print(f"\nCurrent Weather in {self.location.title()}:")
        print(f"  Temperature: {current['temp_C']}°C")
        print(f"  Condition: {current['weatherDesc']}")
        print(f"  Humidity: {current['humidity']}%")
        print(f"  Precipitation: {current['precipMM']} mm")
        print(f"  Wind Speed: {current['windspeedKmph']} km/h")

    def ask_question(self):
        if not self._check_data():
            return

        question = input("Ask your weather question (e.g. 'Will it rain tomorrow in Paris?'): ").strip()
        if not question:
            print("Please enter a question.")
            return

        parsed = self.parser.parse_question(question)
        location = parsed["location"] or self.location

        if location.lower() != self.location.lower():
            # Fetch data for new location if different from current
            print(f"Fetching weather data for {location}...")
            data = self.fetcher.get_weather(location)
            if not data:
                print("Failed to fetch weather for that location.")
                return
        else:
            data = self.weather_data

        answer = self._generate_answer(parsed, data, location)
        print(f"\n🤖 Weather Advisor: {answer}")

    def _generate_answer(self, parsed: Dict, data: Dict, location: str) -> str:
        time = parsed["time"]
        attr = parsed["attribute"]

        if time == "today":
            current = data["current"]
            if attr == "temperature":
                return f"Today in {location.title()}, the temperature is {current['temp_C']}°C."
            elif attr == "precipitation":
                precip = float(current["precipMM"])
                return f"Today in {location.title()}, there is {'some' if precip > 0 else 'no'} precipitation."
            elif attr == "humidity":
                return f"Today in {location.title()}, humidity is {current['humidity']}%."
            elif attr == "wind":
                return f"Today in {location.title()}, wind speed is {current['windspeedKmph']} km/h."
            else:
                return f"Today in {location.title()}, weather is {current['weatherDesc']}."

        elif time == "tomorrow":
            forecast = data["forecast"][1] if len(data["forecast"]) > 1 else None
        elif time == "day_after":
            forecast = data["forecast"][2] if len(data["forecast"]) > 2 else None
        else:
            forecast = None

        if not forecast:
            return f"Sorry, I don't have forecast data for {time}."

        if attr == "temperature":
            return (f"{time.title()} in {location.title()}, average temperature will be "
                    f"{forecast['avgtempC']}°C (max: {forecast['maxtempC']}°C, min: {forecast['mintempC']}°C).")
        elif attr == "precipitation":
            precip = float(forecast["precipMM"])
            if precip > 5:
                return f"There will be significant precipitation ({precip} mm) {time} in {location.title()}."
            elif precip > 0:
                return f"There might be some precipitation ({precip} mm) {time} in {location.title()}."
            else:
                return f"No precipitation expected {time} in {location.title()}."
        elif attr == "humidity":
            # Humidity not available in forecast; fallback to general description
            return f"{time.title()} in {location.title()}, weather condition: {forecast['weatherDesc']}."
        elif attr == "wind":
            return f"Wind data is not available for {time}."
        else:
            return f"{time.title()} in {location.title()}, weather condition: {forecast['weatherDesc']}."

    def show_forecast(self):
        if not self._check_data():
            return

        print(f"\n3-Day Weather Forecast for {self.location.title()}:")
        for day in self.weather_data["forecast"]:
            print(f"\nDate: {day['date']}")
            print(f"  Condition: {day['weatherDesc']}")
            print(f"  Max Temp: {day['maxtempC']}°C")
            print(f"  Min Temp: {day['mintempC']}°C")
            print(f"  Precipitation: {day['precipMM']} mm")

    def show_visualizations(self):
        if not self._check_data():
            return

        print("\nVisualizations:")
        print("1. Temperature Trend")
        print("2. Precipitation Forecast")
        print("3. Both")
        choice = input("Choose visualization (1-3): ").strip()

        if choice == "1":
            self.visualizer.show_temperature_trend(self.weather_data["forecast"])


## Why My Prompting Strategy Was Effective

Here's why your prompting strategy was effective:

Clear, Specific Requests
You explicitly asked for modular, well-structured code with multiple functionalities like multi-day forecasts, natural language parsing, visualization, and a menu system. This clear scope helped the AI focus on delivering a comprehensive, organized solution.

Stepwise Improvement Approach
By first providing an initial prompt and then a follow-up prompt with enhancements, you guided the AI to build on the existing code instead of starting from scratch. This iterative refinement ensures steady progress toward your goals.

Detailed Feature Description
Including precise feature requirements (3-day forecast, question parsing, plotting) in your prompt gave the AI concrete targets, reducing ambiguity and improving output relevance.

Encouraging Modular Design
Requesting class-based structure encouraged maintainable, reusable code, which improves readability and future extensibility.

User Interaction Emphasis
Asking for a menu-driven interface and natural language question handling makes the program more user-friendly and realistic, reflecting practical application needs.

Balance Between Technical and Usability Aspects
Your prompt combined backend data handling with front-end user experience (menus, visualization), resulting in a well-rounded program.

In short, your prompting strategy was effective because it combined clarity, iterative enhancement, detailed feature specs, and focus on modular, user-centric design—helping the AI produce high-quality, practical code.








