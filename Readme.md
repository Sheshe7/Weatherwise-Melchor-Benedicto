1.  Project introduction 
The weather app project is the prepared scenario for us to demonstrate and put into practice all the knowledge we acquired during our lectures. The project asks us to build a code that executes and meets the following instructions: 
	Retrieve weather data from a specified location. 
	Provide rain and forecast visualisation.
	Present a natural language interface.
	Intuitive menu system. 

2. Installation and setup 
Installation and setup are an important part of this. The application’s code starts with the PyInputPlus installation, which is a useful module for creating animated and interactive code. 
***** !pip install matplotlib pyinputplus

After installing the module, we must import requests. The module enables us to send requests to web APIs, which is a requirement for this code’s successful running. In the next row, we have the Import PyinputPlus, which allows the code to keep asking the user for input until a valid entry is inserted. 
*****import requests 
**** import pyinputplus as pyip

Following up, we found in the setup section that the user is required to provide an Api key. Furthermore, there is a URL that is the source from which the user retrieves weather data. 
******API_KEY = "ab4d2c405ef31fc026b5b5d99741fd14"
BASE_URL_CURRENT = "https://api.openweathermap.org/data/2.5/weather"  # Current weather endpoint
BASE_URL_FORECAST = "https://api.openweathermap.org/data/2.5/forecast"

3. Usage and functions
The application is meant to retrieve data from different locations input by the user, thus allowing access to weather and forecasts. Once the code is executed, a welcome message is displayed, and the user is prompted to input a city name and the unit of measurement of weather data.
========================================
🌦️  WELCOME TO MY WEATHER ASSISTANT 🌦️
========================================
📍 Enter city name: Malta 
Choose temperature units:
1. Metric (°C, m/s)
2. Imperial (°F, mph)
3. Standard (K, m/s)
When the user is done choosing, the application displays a menu.  The menu provides different choices, and the users, based on their preferences, will input their option, and then the code will execute the chosen option. 


 How to Run
python weather_assistant.py
Once running, you'll be prompted to:
1.	Enter your city
2.	Choose unit preferences (Metric, Imperial, or Standard)
3.	Select from a menu to:
o	View current weather
o	View 5-day weather forecast (charts)
o	Ask a question
o	Change city or units
o	Exit the app
________________________________________
 Example Questions You Can Ask
•	"Will it rain tomorrow?"
•	"What’s the temperature now?"
•	"Is it hot in Dubai?"
________________________________________
 Output Examples
•	Line chart of upcoming temperatures
•	 Bar chart of predicted rainfall
•	Console output of city weather conditions

📋 MAIN MENU
--------------------
Please select one of the following:
1. Current Weather
2. 5-Day Forecast Chart
3. Ask a Question
4. Change City
5. Change Units
6. Exit

If the user chooses option 1, the code will display a dashboard with the temperature level, weather condition, humidity and wind speed.  
📍 Miami, US
🌡️  Temperature: 26.61°C
🌤️  Conditions: Broken Clouds
💧 Humidity: 83%
💨 Wind: 3.09 m/s
If the user selects the option, 2 the code displays temperature trend and rain forecast for the next five days as it is shown in the next output design. 
 
One interesting and highly demanded option in this assignment was option 3, whereby users could ask questions related to weather conditions and forecasting
🧠 Ask your weather question: 
 
🌧️ Rain forecast (next 24h):
⏰ Thu 09:00: 🌧️ 0 mm
⏰ Thu 12:00: 🌧️ 0 mm
⏰ Thu 15:00: 🌧️ 0 mm
⏰ Thu 18:00: 🌧️ 0 mm
⏰ Thu 21:00: 🌧️ 0 mm
⏰ Fri 00:00: 🌧️ 0 mm
⏰ Fri 03:00: 🌧️ 0 mm
⏰ Fri 06:00: 🌧️ 0 mm


The next two options in the menu only allow users to modify the city and units of weather measurement data.  The last option terminates the loop only in case the break function within the code does not work. 
