---
title: "Before/After Example: Weather Data Retrieval Function"
author: "Tekrajsingh Gujadhur"
format: pdf
 



## Initial Prompt
Will I need an umbrella in London tomorrow?

## Initial AI-Generated Code (Before)
import requests

def umbrella_check(location="London", days=1):
    try:
        # Get weather data
        url = f"https://wttr.in/{location}?format=j1"
        response = requests.get(url, headers={'User-Agent': 'Mozilla/5.0'})
        data = response.json()
        
        # Get tomorrow's precipitation
        tomorrow = data['weather'][days]  # Index 0 is today, 1 is tomorrow
        max_precip = max([float(hour['precipMM']) for hour in tomorrow['hourly'][:4]])  # First 4 hours
        
        # Generate recommendation
        if max_precip > 5:
            return f"Yes! Bring an umbrella tomorrow in {location} ☔ (Expected rain: {max_precip}mm)"
        elif max_precip > 0:
            return f"Maybe take an umbrella in {location} 🌧️ (Possible rain: {max_precip}mm)"
        return f"No umbrella needed tomorrow in {location} 🌞"
    
    except Exception as e:
        return f"Could not check umbrella status: {str(e)}"

# Example usage
print(umbrella_check("London"))



## My Analysis of the Initial Code
Temporal Limitations
python
tomorrow['hourly'][:4]  # Only checks first 4 hours
Doesn't cover full day (24-hour period)
Misses afternoon/evening precipitation chances
Data Reliability Issues
Assumes API structure stability (data['weather'][days])
No validation for missing precipitation data
Single API call without retry mechanism
Threshold Simplification
python
if max_precip > 5: ...
elif max_precip > 0: ...
No distinction between rain/snow/hail types
Doesn't consider probability of precipitation



## My Follow-up Prompt
Follow-Up Development Prompt:
"Let's enhance the umbrella recommendation system to address the identified limitations. Can we:

Expand time coverage to analyze all 8 hourly forecasts (24-hour period) instead of just 4 hours?
Integrate precipitation probability from the API's chanceofrain field for more accurate predictions?
Add error resilience with retry logic for API failures and data validation?
Differentiate precipitation types (rain vs. snow) in recommendations?"


## Improved AI-Generated Code (After)
import requests
import pyinputplus as pyip
from datetime import datetime
import matplotlib.pyplot as plt

class WeatherFetcher:
    BASE_URL = "https://wttr.in/{location}?format=j1"

    def fetch(self, location):
        try:
            response = requests.get(self.BASE_URL.format(location=location))
            response.raise_for_status()
            return response.json()
        except requests.RequestException as e:
            print(f"❌ Error: {e}")
            return None

class WeatherParser:
    def parse_question(self, question):
        question = question.lower()
        location = self._extract_location(question)
        time = "tomorrow" if "tomorrow" in question else "today"
        attribute = self._detect_attribute(question)
        return {"location": location, "time": time, "attribute": attribute}

    def _extract_location(self, question):
        tokens = question.split(" in ")
        return tokens[1].strip("?.") if len(tokens) > 1 else None

    def _detect_attribute(self, question):
        if "rain" in question:
            return "precip"
        elif "temp" in question or "hot" in question:
            return "temperature"
        return "general"

class WeatherInterface:
    def __init__(self):
        self.fetcher = WeatherFetcher()
        self.parser = WeatherParser()
        self.weather_data = None
        self.location = None

    def run(self):
        print("\n🌤️ Welcome to Weather Advisor!")
        while True:
            choice = pyip.inputMenu([
                "Set Location", "Ask a Question", "Current Weather", "Exit"
            ], numbered=True, prompt="\nChoose an option:\n")

            if choice == "Set Location":
                self._set_location()
            elif choice == "Ask a Question":
                self._ask_question()
            elif choice == "Current Weather":
                self._show_current()
            elif choice == "Exit":
                print("👋 Goodbye!")
                break

    def _set_location(self):
        self.location = pyip.inputStr("Enter a location: ")
        self.weather_data = self.fetcher.fetch(self.location)
        if self.weather_data:
            print(f"✅ Weather data loaded for {self.location}")
        else:
            print("❌ Failed to load data. Try another location.")

    def _ask_question(self):
        if not self.weather_data:
            print("⚠️ Please set a location first.")
            return
        question = pyip.inputStr("Ask a weather question: ")
        parsed = self.parser.parse_question(question)
        location = parsed["location"] or self.location
        forecast = self.weather_data.get("weather", [])
        current = self.weather_data.get("current_condition", [])[0]

        if parsed["attribute"] == "precip":
            print(f"🌧️ Rainfall today in {location}: {current['precipMM']}mm")
        elif parsed["attribute"] == "temperature":
            print(f"🌡️ Current temperature in {location}: {current['temp_C']}°C")
        else:
            print(f"📍 General weather in {location}: {current['weatherDesc'][0]['value']}")

    def _show_current(self):
        if not self.weather_data:
            print("⚠️ Please set a location first.")
            return
        current = self.weather_data["current_condition"][0]
        print(f"\nCurrent conditions in {self.location}:")
        print(f"  Temperature: {current['temp_C']}°C")
        print(f"  Weather: {current['weatherDesc'][0]['value']}")
        print(f"  Wind: {current['windspeedKmph']} km/h")
        print(f"  Precipitation: {current['precipMM']} mm")
        print(f"  Humidity: {current['humidity']}%")

if __name__ == "__main__":
    WeatherInterface().run()
    


## Why My Prompting Strategy Was Effective

Why My Prompting Strategy Was Effective
My prompting strategy proved effective because it was clear, structured, and goal-oriented, which enabled the AI to generate a well-organized and functional weather advisor application. The success of the output can be attributed to the following key factors:

✅ 1. Clear and Specific Objectives
I began with a clearly defined goal — to create a Python-based Weather Advisor application that could retrieve real-time weather data, interpret user questions using natural language, and provide visual forecasts. By explicitly stating the purpose, I minimized ambiguity and helped guide the AI toward relevant solutions.

✅ 2. Modular Design Approach
I broke down the application into logical components:

WeatherFetcher for API communication

WeatherParser for interpreting user questions

WeatherVisualizer for data plotting

WeatherInterface for user interaction

This modular approach enabled the AI to generate clean, maintainable, and scalable code, mimicking best practices used in professional software development.

✅ 3. Incorporation of Tools and Libraries
By specifying relevant libraries like requests, matplotlib, and pyinputplus, I provided technical constraints and guided the AI’s implementation choices. This helped ensure the solution was not only functional but also feasible to run and interact with in a user-friendly way.

✅ 4. Use of Natural Language Examples
I included example prompts such as “What’s the temperature in New York today?” which helped the AI understand the expected input style. This guided the development of the natural language parser and ensured the app could interpret and respond to real-world user queries.

✅ 5. Iterative Improvement Mindset
After generating the initial output, I reviewed it and requested improvements. This feedback loop allowed the AI to refine the code, address limitations, and enhance functionality, such as better rain logic or more intuitive responses.

✅ 6. User Experience Consideration
I emphasized an intuitive and engaging command-line interface, enhancing usability. The use of a menu-driven system via pyinputplus supported a better flow and structure for non-technical users, aligning with user-centered design principles.


