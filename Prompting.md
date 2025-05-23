---
title: "Sample Intentional Prompting Journey"
subtitle: "Weather Dashboard Project"
format:
  pdf:
    toc: false
    number-sections: false
    colorlinks: true
  docx:
    toc: false
    number-sections: false
    highlight-style: github
  html:
    toc: true
    toc-expand: 2
    embed-resources: true
---

This document demonstrates how a student might approach the Weather Dashboard project using intentional prompting techniques. It follows the 6-step programming methodology and showcases a variety of prompt types.

> **Important Note to Students**: This document demonstrates the *approach* to intentional prompting, not specific prompts to copy. Your project will have unique requirements and challenges that will require your own thoughtful prompts. Simply copying these example prompts will not result in the best solution for your specific implementation, and will not demonstrate your personal understanding and engagement with the AI. The assessment focuses on your ability to craft effective prompts specific to your project's needs - not on reproducing this example. Use this as inspiration for your own authentic conversations with AI, where you identify problems, ask clarifying questions, challenge initial solutions, and guide the AI toward better outcomes for your particular implementation.

---

## Step 1: Restate the Problem (Foundation Prompts)

### 🗣️ Initial Prompt
"What’s the temperature in New York today?"

### 🤖 AI Response (partial)
🌤️ Welcome to Weather Advisor! 🌤️
Get weather forecasts and ask questions in natural language.

Choose an option:
1. Set Location
2. Current Weather
3. Ask a Question
4. View Forecast
5. Visualizations
6. Exit

[User selects: Set Location]
Enter location (e.g., London, New York): New York
✅ Weather data loaded for New York

[User selects: Ask a Question]
Ask a weather question (e.g., 'Will it rain tomorrow?'): What’s the temperature in New York today?

🤖 Weather Advisor: The temperature in New York today is 22°C.




### 🗣️ Follow-up Prompt (Clarification)
“Can you clarify whether that’s the current temperature or the average for today?”


### 🤖 AI Response (partial)

“That’s the current temperature in New York as of now. If you'd like to know the average temperature for today, I can check that for you too!”
If you'd like to improve the system to automatically provide both current and average temperatures when a user mentions “today,” I can help you enhance the _generate_response logic. Would you like that?





## Step 2: Identify Input/Output Requirements

### 🗣️ Requirement Gathering Prompt
“I’m designing a weather assistant. What types of weather-related questions would you want it to answer, and what kind of features or visuals would be most useful to you?”
This prompt is great for gathering input from users, stakeholders, or test audiences. It encourages them to think beyond basic queries and suggest features like:
Real-time alerts or notifications
Umbrella or clothing advice based on weather
Hour-by-hour vs. day-by-day forecasts
Visual comparisons (e.g., temperature this week vs. last week)
Location-based recommendations (e.g., best day to go hiking)


### 🤖 AI Response
Sample AI Response to the Requirement Gathering Prompt:

"Thanks for asking! I'd like the weather assistant to answer questions like:

'Will it rain during my commute tomorrow morning?'

'What’s the weather like this weekend in Paris?'

'Should I wear a jacket today?'

As for features, it would be great to have:

Hourly breakdowns for today and tomorrow

Clear icons for sun, rain, wind, etc.

Visual charts showing temperature and rain trends

Clothing or umbrella recommendations based on the forecast

It’d also be helpful if I could save a few favorite locations and get quick updates with one click."

This response represents what a typical user might expect and request when engaging with your app. If you’d like, I can help you extract these needs into formal user stories or functional requirements.









## Step 3: Work Problem by Hand

### 🗣️ Manual Solution Prompt
```
“If you didn’t have access to a weather assistant app, how would you normally check the weather and plan your day? What tools or steps would you use manually?”

This prompt helps uncover the real-world, pre-AI behaviors of users—useful for understanding what tasks your app can automate or enhance.

🤖 AI Response (from a user perspective):

"Without a weather assistant, I’d usually:

Google ‘weather in [location]’

Check the weather app on my phone for the next few hours

Scroll through hourly forecasts to decide if I need an umbrella

Look at the precipitation graph to see when it might rain

Sometimes check multiple sources like AccuWeather or BBC Weather if I’m unsure"

This gives insight into what people value (quick access, visual data, reliability) and helps you design features that are faster, easier, or more insightful than the manual steps.

Would you like to turn this into a user scenario or map it to app features?
###

### Step 4: Request Pseudocode

pseudocode prompt
"Write a pseudocode algorithm for how a weather assistant should process a user's natural language question, retrieve the forecast, and generate a helpful response."

### AI Respons
START

DISPLAY welcome message

WHILE user has not exited:
    DISPLAY menu options:
        - Set location
        - Ask a question
        - View forecast
        - Exit

    IF user selects "Set location":
        PROMPT for location
        FETCH weather data from API
        STORE weather data

    IF user selects "Ask a question":
        PROMPT user for natural language input
        PARSE input to identify:
            - Location (if specified)
            - Time (today, tomorrow, weekend)
            - Attribute (temperature, rain, wind, etc.)
        IF location not provided:
            USE current stored location
        SELECT current or forecast data based on time
        EXTRACT requested attribute from data
        FORMAT a friendly natural language response
        DISPLAY the response

    IF user selects "View forecast":
        DISPLAY 5-day forecast from stored weather data

    IF user selects "Exit":
        DISPLAY goodbye message
        END program

END WHILE

END




### 🗣️ Follow-up Prompt (Challenge)
```
The pseudocode looks good overall. I noticed that error handling isn't explicitly included. Could you expand the pseudocode to include error handling for:

1. Invalid location input
2. Weather service connection failures
3. Handling missing data in the weather response

Also, how would you structure the functions to make testing easier?
```







### Step 5: Convert to Python Code

### 🗣️ Initial Implementation Prompt
```
“Write Python code to implement a simple weather assistant that takes a user’s location and displays the current temperature using the wttr.in API.”
- weather_api.get_current(location, units) - returns current weather
- weather_api.get_forecast(location, units, days=3) - returns forecast

I'll be using pyinputplus for user input validation and matplotlib for any visualisations.
```

### 🤖 AI Response (partial code)
```python

import requests

def get_current_temperature(location):
    url = f"https://wttr.in/{location}?format=j1"
    try:
        response = requests.get(url)
        data = response.json()
        current_temp = data["current_condition"][0]["temp_C"]
        print(f"The current temperature in {location} is {current_temp}°C.")
    except Exception as e:
        print("Could not fetch weather data:", e)

# Example usage
location = input("Enter a location: ")
get_current_temperature(location)







```

### 🗣️ Refinement Prompt

```“Improve the existing Python weather assistant by adding natural language question parsing, support for multiple weather attributes (like temperature, rain, and wind), and a simple user interaction loop with a menu.”

This refinement prompt helps expand your basic implementation into a more functional assistant. It leads to enhancements like:

Adding a WeatherParser class to interpret user questions

Handling different forecast time frames (today, tomorrow, weekend)

Supporting weather attributes beyond just temperature

Including a loop to interact with the user multiple times

Displaying structured responses and visualizations
```

### 🤖 AI Response (partial code)
```python
def fetch_weather_data(location, units):
    """
    Fetch weather data from API.
    
    Args:
        location: City name or location
        units: 'metric' or 'imperial'
        
    Returns:
        Dictionary containing current weather and forecast
        
    Raises:
        ConnectionError: If unable to connect to weather service
        ValueError: If location is invalid
    """
    try:
        # Attempt to get current weather
        current_weather = weather_api.get_current(location, units)
        
        # Attempt to get forecast
        forecast = weather_api.get_forecast(location, units, days=5)
        
        # Basic validation of returned data
        if not current_weather or 'temperature' not in current_weather:
            raise ValueError(f"Invalid data received for location: {location}")
        
        return {"current": current_weather, "forecast": forecast}
    
    except ConnectionError:
        raise ConnectionError("Failed to connect to weather service")
    except Exception as e:
        # Convert any other exceptions from the API to something we can handle
        raise Exception(f"Error fetching weather data: {str(e)}")

def display_dashboard(weather_data):
    """
    Display the main weather dashboard.
    
    Args:
        weather_data: Dictionary containing current weather and forecast
    """
    clear_screen()
    current = weather_data["current"]
    forecast = weather_data["forecast"]
    
    # Display header and current conditions
    print("=" * 60)
    print(f"  WEATHER DASHBOARD - {current['location'].upper()}")
    print("=" * 60)
    print(f"\nCurrent Conditions as of {current['timestamp']}")
    print(f"Temperature: {current['temperature']}°{'C' if current['units'] == 'metric' else 'F'}")
    print(f"Feels Like: {current['feels_like']}°{'C' if current['units'] == 'metric' else 'F'}")
    print(f"Condition: {current['condition']}")
    print(f"Humidity: {current['humidity']}%")
    print(f"Wind: {current['wind_speed']} {'km/h' if current['units'] == 'metric' else 'mph'} from {current['wind_direction']}")
    
    # Display simplified forecast
    print("\n3-DAY FORECAST")
    print("-" * 60)
    
    for i, day in enumerate(forecast[:3]):
        day_label = "Today" if i == 0 else "Tomorrow" if i == 1 else day['date']
        print(f"{day_label}: High {day['high']}° | Low {day['low']}° | {day['condition']}")
```

### 🗣️ Integration Prompt
```
“Combine the weather data retrieval, natural language question parsing, and user interface into a single cohesive Python application that allows users to interact with the assistant, ask weather-related questions, and receive structured answers.”

This prompt guides the system to merge components (fetching, parsing, responding) into a unified experience. It enables:

Seamless flow from user input → interpretation → data retrieval → output

Reuse of classes like WeatherFetcher, WeatherParser, and WeatherInterface

A central run() method to control the app lifecycle

Real-time, conversational-style interaction


...

## Step 6: Test with Data

### 🗣️ Testing Prompt
```
“Write test cases to verify that the weather assistant correctly parses user questions, fetches data from the API, and generates accurate responses for different time periods and weather attributes.”```

### 🤖 AI Response (partial)
import unittest
from unittest.mock import patch

class TestWeatherParser(unittest.TestCase):

    def setUp(self):
        self.parser = WeatherParser()

    def test_extract_location(self):
        question = "Will it rain tomorrow in London?"
        parsed = self.parser.parse_question(question)
        self.assertEqual(parsed["location"], "london")

    def test_extract_time_period(self):
        question = "What’s the temperature this weekend?"
        parsed = self.parser.parse_question(question)
        self.assertEqual(parsed["time"], "weekend")

    def test_extract_attribute(self):
        question = "Is it windy today?"
        parsed = self.parser.parse_question(question)
        self.assertEqual(parsed["attribute"], "wind")

class TestWeatherInterfaceResponse(unittest.TestCase):

    def setUp(self):
        self.interface = WeatherInterface()
        # Mock weather data for tests
        self.interface.weather_data = {
            "current": {
                "temp_C": "15",
                "weatherDesc": "Sunny",
                "precipMM": "0",
                "humidity": "40",
                "windspeedKmph": "10"
            },
            "forecast": [
                {
                    "date": "2025-05-24",
                    "maxtempC": "20",
                    "mintempC": "10",
                    "avgtempC": "15",
                    "precipMM": "0",
                    "weatherDesc": "Partly cloudy"
                }
            ]
        }
        self.interface.current_location = "London"

    def test_generate_temperature_response_today(self):
        parsed = {"location": "London", "time": "today", "attribute": "temperature"}
        response = self.interface._generate_response(parsed)
        self.assertIn("15°C", response)

    def test_generate_precipitation_response_no_rain(self):
        parsed = {"location": "London", "time": "today", "attribute": "precipitation"}
        response = self.interface._generate_response(parsed)
        self.assertIn("No precipitation expected", response)

if __name__ == "__main__":
    unittest.main()



### 🗣️ Edge Case Prompt
```
“Identify and handle edge cases in the weather assistant, such as invalid locations, ambiguous questions without specified location or time, missing data from the API, and unsupported weather attributes. Write code snippets to gracefully manage these cases and provide helpful user feedback.”
---


```

### 🗣️ Refinement Process Prompt
```
Describe a step-by-step refinement process for improving the weather assistant code, including adding error handling, enhancing natural language parsing, optimizing API calls, improving user interaction, and adding data visualizations. Provide suggestions for iterative testing and code organization.”```

### After: Improved Data Retrieval Function```
import requests
from requests.adapters import HTTPAdapter, Retry

class WeatherFetcher:
    BASE_URL = "https://wttr.in/{location}?format=j1"

    def __init__(self):
        self.session = requests.Session()
        self.session.headers.update({'User-Agent': 'Weather Advisor App'})

        retries = Retry(
            total=3,
            backoff_factor=1,
            status_forcelist=[429, 500, 502, 503, 504]
        )
        adapter = HTTPAdapter(max_retries=retries)
        self.session.mount("https://", adapter)
        self.session.mount("http://", adapter)

    def get_weather(self, location: str, forecast_days: int = 5) -> Optional[dict]:
        url = self.BASE_URL.format(location=location)
        try:
            response = self.session.get(url, timeout=10)
            response.raise_for_status()
            data = response.json()
            if "current_condition" not in data or "weather" not in data:
                print("Unexpected API response structure.")
                return None
            return self._structure_data(data, forecast_days)
        except requests.exceptions.RequestException as e:
            print(f"Network error occurred: {e}")
            return None
        except ValueError:
            print("Error decoding API response.")
            return None

### Explanation
The improved function demonstrates:

1. **Comprehensive error handling** with specific error types
2. **Data validation** to check for required fields
3. **Graceful degradation** by providing defaults for missing data
4. **Retry mechanism** for transient failures
5. **Defensive programming** by ensuring forecast is always a list
6. **Informative error messages** for better user experience
7. **Additional metadata** (timestamp, location) for context

This improvement resulted from intentional prompting that specifically challenged the AI to address potential failure modes and edge cases, rather than accepting the initial implementation.
