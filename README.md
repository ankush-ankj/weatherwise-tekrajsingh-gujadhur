How to Run the Weather Advisor App

1. 🔧 Preparation

Before you can use the Weather Advisor app, make sure you:
	•	Have Python installed (version 3.7 or higher).
	•	Are using an environment that supports Jupyter Notebooks (e.g., Google Colab, VS Code with Jupyter extension, or JupyterLab).
	•	Have an internet connection (the app fetches live weather data from the wttr.in API).

⸻

2. 📁 Open the Notebook
	1.	Locate the starter notebook file that has been renamed (as per your course instructions).
	•	For example, weather_advisor_ankush.ipynb.
	2.	Open the notebook using:
	•	Google Colab: Right-click the file > “Open with” > “Google Colab”
	•	VS Code: Open VS Code > Open Folder > Select the notebook file
	•	JupyterLab: Launch JupyterLab > Navigate to the file > Double click to open

⸻

3. ▶️ Run the Notebook

Once the notebook is open:
	1.	Run All Cells (Top to Bottom):
	•	In Google Colab: Click Runtime > Run all
	•	In JupyterLab/VS Code: Click Run or press Shift + Enter on each code cell
🔹 The first cell contains !pip install pyinputplus. This will install the required library. Allow it to finish before moving on.


🌤️ Start the App

After running all cells, the final block of code contains:
if __name__ == "__main__":
    app = WeatherInterface()
    app.run()

  This launches your Weather Advisor App in the notebook interface.

⸻

5. 🧭 Using the App – Menu Options

You’ll be presented with a menu like:## 📓 Submission Checklist
Choose an option:
1. Set Location
2. Current Weather
3. Ask a Question
4. View Forecast
5. Visualizations
6. Exit

   Example Workflow:
	1.	Set Location: Choose this to enter a city name (e.g., Paris, New York). This fetches weather data for the location.
	2.	Current Weather: Displays live weather stats for your chosen city.
	3.	Ask a Question: Type questions like:
	•	“Will it rain tomorrow?”
	•	“What’s the temperature today in London?”
	4.	View Forecast: Shows a 5-day forecast for your location.
	5.	Visualizations:
	•	Choose Temperature Trends, Precipitation Forecast, or Both to see charts.
	6.	Exit: Closes the app gracefully.

6. ❗ Tips for Success
	•	Use meaningful location names (city or town).
	•	Type questions naturally for the best AI-generated answers.
	•	If weather data doesn’t load, try a different city or check your internet connection.





# Submiision Checklist


# My Project consists of a files named:
1 weatherwise_Tekrajsingh_starternotebook.ipynb
2 WeatherWiseAppTekrajsingh.ipynb                 
3 README.md                                                     
4 reflection.md  
5 PROMPTING.md


# My project consist of folders named:
1 ai-conversations/                                          
    conversation1.txt
    conversation2.txt
    conversation3.txt
    conversation4.txt
    conversation5.txt


2 before-after examples/                               
    before-after-example1.md
    before-after-example2.md
    before-after-example3.md
