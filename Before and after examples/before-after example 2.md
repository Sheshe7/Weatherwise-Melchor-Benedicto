"Before/After Example: API and data handling
author: "Melchor Benedicto "
format: 
 

## Initial Prompt

I need a code that retrieves data from a web site using scrapping method or Api key, the code should also be able to provide a visualization and forecasting. 

## Initial AI-Generated Code (Before)
def get_weather_data(api_key, city_id):
    """Retrieve weather data from OpenWeatherMap API for a specific city ID"""
    base_url = "https://openweathermap.org/city/934219"

    try:
        params = {
            "id": city_id,
            "appid": api_key,
            "units": "metric" # Use "imperial" for Fahrenheit
        }

        response = requests.get(base_url, params=params)
        response.raise_for_status()  # Raise an exception for HTTP errors

        return response.json()

    except requests.exceptions.RequestException as e:
        print(f"Error fetching weather data: {e}")
        return None

def display_weather(data):
    """Display current weather conditions in a readable format"""
    if not data:
        print("No weather data available.")
        return

    # Extract relevant data
    city = data['name']
    country = data['sys']['country']
    weather_desc = data['weather'][0]['description'].title()
    temp = data['main']['temp']
    feels_like = data['main']['feels_like']
    humidity = data['main']['humidity']
    wind_speed = data['wind']['speed']
    wind_deg = data['wind'].get('deg', 'N/A')
    sunrise = datetime.fromtimestamp(data['sys']['sunrise']).strftime('%H:%M:%S')
    sunset = datetime.fromtimestamp(data['sys']['sunset']).strftime('%H:%M:%S')

    # Convert wind degrees to direction
    directions = ['N', 'NNE', 'NE', 'ENE', 'E', 'ESE', 'SE', 'SSE',
                  'S', 'SSW', 'SW', 'WSW', 'W', 'WNW', 'NW', 'NNW']
    if wind_deg != 'N/A':
        index = round(wind_deg / (360. / len(directions))) % len(directions)
        wind_dir = directions[index]
    else:
        wind_dir = 'N/A'

    # Determine temperature unit based on units parameter
    temp_unit = '°C'  # We're using metric units
    speed_unit = 'm/s'  # We're using metric units

    # Display the weather information
    print("\n" + "="*50)
    print(f"Current Weather in {city}, {country} (City ID: 934277)")
    print("="*50)
    print(f"Conditions: {weather_desc}")
    print(f"Temperature: {temp}{temp_unit} (Feels like {feels_like}{temp_unit})")
    print(f"Humidity: {humidity}%")
    print(f"Wind: {wind_speed} {speed_unit} {wind_dir}")
    print(f"Sunrise: {sunrise}")
    print(f"Sunset: {sunset}")

    # Additional weather data if available
    if 'rain' in data:
        print(f"Rain (last hour): {data['rain'].get('1h', 0)} mm")
    if 'snow' in data:
        print(f"Snow (last hour): {data['snow'].get('1h', 0)} mm")

    print("="*50)
    print(f"\nData corresponds to the web page: https://openweathermap.org/city/934219")

def main():
    # API configuration
    API_KEY = "ab4d2c405ef31fc026b5b5d99741fd14"
    CITY_ID = 934277  # Quatre Bornes, Mauritius

    print("\nWeather Data Retrieval for Quatre Bornes, Mauritius")
    print("================================================")

    # Get weather data
    weather_data = get_weather_data(API_KEY, CITY_ID)

    if weather_data:
        display_weather(weather_data)
    else:
        print("Could not retrieve weather data. Please check your API key and internet connection.")

if __name__ == "__main__":
    main()

## My Analysis of the Initial Code

The initial code has several issues:

1. API endpoint error. The base_url provided cannot connect to the resources that API provides because it is hardcoded. 
2. Timeout Error. The code privided does not have a timeout. The absence of a timeout leads the program to wait undefinitely until it hang due to no response.   
3. No API response validation. The API should have a format or the data it contains might be incorrect. No API validation means lack of verification on API responses. 
4. hardcoded city ID. The code provided will always print city ID 934277 regardless of the city we input.  


## My Follow-up Prompt
How can i fix all the mistakes and errors highlighted in the previous code?
I'd like to improve this function with:

1. Correcting the API URL and set it as web page endpoint. 
2. Adding timeout will prevent udefinitely and long waiting time. 
3. Using the actual city ID i input instead of the default. 
4. Enhancing error handling, which will allow users to customise their errors and make it more understandable  
```

## Improved AI-Generated Code (After)

```python
qimport requests
from datetime import datetime

def get_weather_data(api_key, city_id, units="metric"):
    """Retrieve weather data from OpenWeatherMap API for a specific city ID"""
    base_url = "https://api.openweathermap.org/data/2.5/weather"

    try:
        params = {
            "id": city_id,
            "appid": api_key,
            "units": units
        }

        response = requests.get(base_url, params=params, timeout=10)
        response.raise_for_status()  # Raises HTTPError for bad responses (4xx, 5xx)

        data = response.json()

        # Basic validation
        required_keys = ['name', 'sys', 'main', 'weather', 'wind']
        if not all(key in data for key in required_keys):
            print("Incomplete data received from API.")
            return None

        return data

    except requests.exceptions.Timeout:
        print("The request timed out.")
    except requests.exceptions.RequestException as e:
        print(f"Error fetching weather data: {e}")
    return None


def display_weather(data, units="metric"):
    """Display current weather conditions in a readable format"""
    if not data:
        print("No weather data available.")
        return

    try:
        # Extract relevant data
        city = data['name']
        country = data['sys'].get('country', 'N/A')
        city_id = data.get('id', 'N/A')
        weather_desc = data['weather'][0].get('description', 'N/A').title()
        temp = data['main'].get('temp', 'N/A')
        feels_like = data['main'].get('feels_like', 'N/A')
        humidity = data['main'].get('humidity', 'N/A')
        wind_speed = data['wind'].get('speed', 'N/A')
        wind_deg = data['wind'].get('deg', 'N/A')
        sunrise = datetime.fromtimestamp(data['sys'].get('sunrise')).strftime('%H:%M:%S') if data['sys'].get('sunrise') else 'N/A'
        sunset = datetime.fromtimestamp(data['sys'].get('sunset')).strftime('%H:%M:%S') if data['sys'].get('sunset') else 'N/A'

        # Wind direction
        directions = ['N', 'NNE', 'NE', 'ENE', 'E', 'ESE', 'SE', 'SSE',
                      'S', 'SSW', 'SW', 'WSW', 'W', 'WNW', 'NW', 'NNW']
        if isinstance(wind_deg, (int, float)):
            index = round(wind_deg / (360. / len(directions))) % len(directions)
            wind_dir = directions[index]
        else:
            wind_dir = 'N/A'

        # Units
        temp_unit = '°C' if units == 'metric' else '°F' if units == 'imperial' else 'K'
        speed_unit = 'm/s' if units == 'metric' else 'mph' if units == 'imperial' else 'm/s'

        # Display output
        print("\n" + "="*50)
        print(f"Current Weather in {city}, {country} (City ID: {city_id})")
        print("="*50)
        print(f"Conditions: {weather_desc}")
        print(f"Temperature: {temp}{temp_unit} (Feels like {feels_like}{temp_unit})")
        print(f"Humidity: {humidity}%")
        print(f"Wind: {wind_speed} {speed_unit} {wind_dir}")
        print(f"Sunrise: {sunrise}")
        print(f"Sunset: {sunset}")

        # Optional: Rain/Snow
        if 'rain' in data:
            print(f"Rain (last hour): {data['rain'].get('1h', 0)} mm")
        if 'snow' in data:
            print(f"Snow (last hour): {data['snow'].get('1h', 0)} mm")

        print("="*50)
        print(f"\nData fetched from OpenWeatherMap API.")

    except Exception as e:
        print(f"Error while displaying data: {e}")


def main():
    # API configuration
    API_KEY = "ab4d2c405ef31fc026b5b5d99741fd14"  # Replace with a secure source
    CITY_ID = 934277  # Quatre Bornes, Mauritius
    UNITS = "metric"  # "imperial" for Fahrenheit, "standard" for Kelvin

    print("\nWeather Data Retrieval for Quatre Bornes, Mauritius")
    print("="*48)

    weather_data = get_weather_data(API_KEY, CITY_ID, units=UNITS)

    if weather_data:
        display_weather(weather_data, units=UNITS)
    else:
        print("Could not retrieve weather data. Please check your API key, city ID, and internet connection.")


if __name__ == "__main__":
    main()

        return None
```

## Why My Prompting Strategy Was Effective

My follow-up prompt was effective because:

1. **My final code is effective because it enables API data retrieval rather than an HTML page. 
2. ** The timeout handling improves user experience and prevents apps from freezing.  
3. **The data validation in the final code reduces the chance of getting errors and crashes because it checks the fields before running the code. 
4. ** I add robust error handling, which will make troubleshooting easier and improve error responses. 

This approach guided the AI to generate a much more robust function that:
      
- Effective 
- Validates inputs
- Has comprehensive error handling
- Returns structured, processed data instead of raw API response
- Reliable and secured. 

The key to effective prompting was being specific about what needed improvement and why, rather than making vague requests for "better" code.
