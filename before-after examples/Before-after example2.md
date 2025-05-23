---
title: "Before/After Example: Weather Data Retrieval Function"
author: "Tekrajsingh Gujadhur"



  Initial Prompt:
"Create a Python command-line weather application that:

Fetches real-time weather data and 5-day forecasts from wttr.in API
Supports natural language questions like 'Will it rain in Paris this weekend?'
Displays temperature trends and precipitation forecasts using matplotlib
Uses OOP principles with separate classes for data fetching, parsing, visualization
Features an interactive menu system with pyinputplus
Shows formatted current conditions with temperature, humidity, and wind speed
Handles API errors gracefully and validates user input"
Improved AI-Generated Code (Key Improvements Shown):

python
# %% Improved error handling and type hints
class WeatherFetcher:
    def get_weather(self, location: str, forecast_days: int = 5) -> Optional[Dict]:
        """Fetch weather data with enhanced error handling"""
        try:
            response = self.session.get(self.BASE_URL.format(location=location))
            response.raise_for_status()
            try:
                return self._structure_data(response.json(), forecast_days)
            except (KeyError, IndexError) as e:
                print(f"Error parsing weather data: {e}")
                return None
        except requests.exceptions.HTTPError as e:
            print(f"Location not found: {e.response.status_code}")
            return None
        except requests.exceptions.ConnectionError:
            print("Connection error - please check your internet connection")
            return None

# %% Multi-word location support
class WeatherParser:
    def _extract_location(self, question: str) -> Optional[str]:
        """Improved location extraction with regex for multi-word cities"""
        pattern = r"(?:in|at|for)\s+([\w\s-]+?)(?:\?|$|today|tomorrow|weekend)"
        match = re.search(pattern, question, re.IGNORECASE)
        return match.group(1).strip() if match else None

# %% Enhanced weekend detection
class WeatherInterface:
    def _get_forecast_data(self, time_period: str) -> Optional[Dict]:
        """Improved weekend handling with date range check"""
        if time_period == "weekend":
            today = datetime.today()
            weekend_days = [
                day for day in self.weather_data["forecast"]
                if self._is_weekend(day["date"]) and 
                datetime.strptime(day["date"], "%Y-%m-%d") >= today
            ]
            return weekend_days[0] if weekend_days else None
        # Rest of method remains...

# %% Precipitation data aggregation
class WeatherFetcher:
    def _structure_forecast_day(self, day: Dict) -> Dict:
        """Calculate total daily precipitation"""
        total_precip = sum(
            float(hour["precipMM"]) 
            for hour in day["hourly"] 
            if hour["precipMM"].isdigit()
        )
        return {
            "date": day["date"],
            "maxtempC": day["maxtempC"],
            "mintempC": day["mintempC"],
            "avgtempC": day["avgtempC"],
            "precipMM": total_precip,
            "weatherDesc": max(
                (desc["value"] for desc in day["hourly"][0]["weatherDesc"]),
                key=len
            )
        }

# %% Enhanced visualization labels
class WeatherVisualizer:
    def _format_plot(self, title: str, x_label: str, y_label: str) -> None:
        """Improved plot formatting with dynamic sizing"""
        plt.title(title, fontsize=14, pad=20)
        plt.xlabel(x_label, fontsize=12)
        plt.ylabel(y_label, fontsize=12)
        plt.xticks(rotation=45, ha='right')
        plt.grid(True, alpha=0.3)
        plt.tight_layout()
        plt.legend(frameon=True, facecolor='white')
