import requests
import matplotlib.pyplot as plt
import seaborn as sns
import datetime

API_KEY = "afa0064d30e5a8526682766fcf284875"   


CITY = input("Enter city name: ")


URL = f"http://api.openweathermap.org/data/2.5/forecast?q={CITY}&appid={API_KEY}&units=metric"


response = requests.get(URL)
data = response.json()

if data.get("cod") != "200":
    print("Error fetching data:", data.get("message", "Unknown error"))
    exit()


dates = []
temps = []
humidity = []
wind_speed = []

for entry in data["list"]:
    dt = datetime.datetime.fromtimestamp(entry["dt"])
    temp = entry["main"]["temp"]
    hum = entry["main"]["humidity"]
    wind = entry["wind"]["speed"]

    dates.append(dt)
    temps.append(temp)
    humidity.append(hum)
    wind_speed.append(wind)


plt.figure(figsize=(14,10))

# Chart 1: Temperature
plt.subplot(3,1,1)
sns.lineplot(x=dates, y=temps, color="red")
plt.title(f"Temperature Trend in {CITY}")
plt.ylabel("Temp (°C)")
plt.xticks(rotation=45)

# Chart 2: Humidity
plt.subplot(3,1,2)
sns.lineplot(x=dates, y=humidity, color="blue")
plt.title(f"Humidity Trend in {CITY}")
plt.ylabel("Humidity (%)")
plt.xticks(rotation=45)

# Chart 3: Wind Speed
plt.subplot(3,1,3)
sns.lineplot(x=dates, y=wind_speed, color="green")
plt.title(f"Wind Speed Trend in {CITY}")
plt.ylabel("Wind Speed (m/s)")
plt.xlabel("Date & Time")
plt.xticks(rotation=45)

plt.tight_layout()
plt.show()
