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

“Create a simple Python weather advisor application that fetches data from wttr.in using the requests library. The app should let the user enter a location and return the current weather conditions including temperature, humidity, and weather description. It should also support asking basic natural language questions like ‘Is it raining in Paris?’ and display appropriate responses.”

## Initial AI-Generated Code (Before)

import requests

class SimpleWeatherAdvisor:
    BASE_URL = "https://wttr.in/{}?format=j1"

    def get_weather(self, location):
        try:
            response = requests.get(self.BASE_URL.format(location))
            data = response.json()
            current = data['current_condition'][0]
            return {
                'temperature': current['temp_C'],
                'humidity': current['humidity'],
                'description': current['weatherDesc'][0]['value']
            }
        except Exception as e:
            print("Error retrieving weather data:", e)
            return None

    def answer_question(self, question):
        # Basic keyword check
        question = question.lower()
        if "rain" in question or "raining" in question:
            location = self.extract_location(question)
            weather = self.get_weather(location)
            if weather:
                if "rain" in weather['description'].lower():
                    print(f"Yes, it's raining in {location}.")
                else:
                    print(f"No, it's not raining in {location}.")
        else:
            print("Sorry, I can only answer questions about rain right now.")

    def extract_location(self, text):
        # Simple extraction: assume last word is the location
        words = text.strip("?.").split(" ")
        return words[-1]

# Sample usage
app = SimpleWeatherAdvisor()
loc = input("Enter location: ")
weather = app.get_weather(loc)
if weather:
    print(f"Current weather in {loc}:\n"
          f"- Temperature: {weather['temperature']}°C\n"
          f"- Humidity: {weather['humidity']}%\n"
          f"- Condition: {weather['description']}")
else:
    print("Weather data could not be retrieved.")

question = input("\nAsk a question (e.g., Is it raining in Paris?): ")
app.answer_question(question)


## My Analysis of the Initial Code

Limitations Identified
Weak Location Extraction
The extract_location method assumes the last word in the sentence is the location, which is overly simplistic. For example, “Will it rain in New York today?” would incorrectly extract “today” as the location.

Limited Question Scope
The answer_question method can only answer yes/no questions about rain. It lacks extensibility for other weather attributes like temperature, wind, or humidity.

Lack of Error Handling and Feedback
No validation for user input or fallback messages if weather data retrieval fails beyond a print statement.
No graceful handling of cases where API data is missing or malformed.

No Visualization or Forecasting
There are no graphs, trends, or multi-day forecasts — only current conditions are shown.
The lack of visualization limits the user’s ability to interpret weather trends or plan ahead.

Minimal Modularity and Reusability
While structured as a class, the logic is still quite linear and limited in scalability.
No separation of responsibilities for parsing, fetching, and visualizing.

🛠️ What I Learned
Clear prompts result in functioning prototypes, but refinement is necessary for usability and extensibility.
Even basic AI-generated code can provide a useful scaffold for a more advanced app.
Modular design, better NLP handling, and user-focused feedback mechanisms are crucial for improving the user experience.



## My Follow-up Prompt

Prompt:

“Improve the weather advisor app by implementing the following:
Add support for more types of weather questions (e.g., temperature, humidity, wind, and general condition).
Include support for ‘today’, ‘tomorrow’, and ‘weekend’ forecasts.
Implement simple natural language parsing to extract the location, date, and attribute from the user's question.
Organize the code into separate classes for data fetching, question parsing, and visual output.
Add visualizations for temperature trends and precipitation using Matplotlib.
Make the program interactive using a menu system.”

## Improved AI-Generated Code (After)

import requests
import matplotlib.pyplot as plt
from datetime import datetime
import pyinputplus as pyip

# --- Weather Data Fetcher ---

class WeatherFetcher:
    BASE_URL = "https://wttr.in/{location}?format=j1"

    def __init__(self):
        self.session = requests.Session()
        self.session.headers.update({'User-Agent': 'WeatherAdvisorApp'})

    def get_weather(self, location):
        try:
            response = self.session.get(self.BASE_URL.format(location=location



## Why My Prompting Strategy Was Effective

✅ Why My Prompting Strategy Was Effective
My prompting strategy was effective because it transformed a simple, limited script into a robust, user-friendly, and modular weather advisor application. The effectiveness of my approach is evident in the following key aspects:

🎯 1. Clear, Specific Instructions
I gave explicit goals in my prompt:

Expand the types of weather attributes (temperature, humidity, wind, etc.).

Support for multiple time frames (today, tomorrow, weekend).

Modular design using classes.

Integration of visualization and interactive features.

By clearly listing each feature I wanted, the AI was able to generate more complex and organized code tailored to those expectations.

🧠 2. Progressive Enhancement
Rather than asking for a full solution from the start, I iterated step-by-step:

I started with a basic version.

Then, I analyzed its limitations.

Finally, I crafted a follow-up prompt to build upon it.

This layered prompting mirrors a real development workflow and allowed the AI to incrementally improve its output in a meaningful way.

⚙️ 3. Human-Like Clarity in Prompting
My follow-up prompt read like a mini software spec. It included:

What to do (e.g., add forecast visualization)

How to do it (e.g., use matplotlib, separate classes)

Why it’s needed (to support natural language interaction and usability)

This helped guide the AI with context, not just instructions — which led to a more intelligent and comprehensive result.

🧩 4. Encouraged Modularity and Reusability
I explicitly requested class separation (fetching, parsing, visualizing), which encouraged:

Clean code structure

Easier debugging

Future extensibility

This strategic modularity meant that the AI didn’t just tack on features—it integrated them meaningfully.

📈 5. Prompted Real-World Usefulness
By asking for interactivity, visual charts, and natural language support, I nudged the AI to think like a product designer — not just a coder. This ensured the output wasn’t just functional, but also user-centered
