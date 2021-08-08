from gpiozero import Buzzer
import Adafruit_DHT
from time import time, sleep
from urllib.request import urlopen
import sys

WRITE_API = "Your ThingSpeak Write API" # Replace your ThingSpeak API key here
BASE_URL = "https://api.thingspeak.com/update?api_key={}".format(WRITE_API)

buzzer = Buzzer(26)

SENSOR_PIN = 14
SENSOR_TYPE = Adafruit_DHT.DHT11

SensorPrevSec = 0
SensorInterval = 2 # 2 seconds
ThingSpeakPrevSec = 0
ThingSpeakInterval = 20 # 20 seconds

try:
    while True:
        
        if time() - SensorPrevSec > SensorInterval:
            SensorPrevSec = time()
            
            humidity, temperature = Adafruit_DHT.read_retry(SENSOR_TYPE, SENSOR_PIN)
            print("Humidity = {:.2f}%\tTemperature = {:.2f}C".format(humidity, temperature))
        
        if time() - ThingSpeakPrevSec > ThingSpeakInterval:
            ThingSpeakPrevSec = time()
            
            thingspeakHttp = BASE_URL + "&field1={:.2f}&field2={:.2f}".format(temperature, humidity)
            print(thingspeakHttp)
            
            conn = urlopen(thingspeakHttp)
            print("Response: {}".format(conn.read()))
            conn.close()
            
            buzzer.beep(0.05, 0.05, 1)
            sleep(1)
            
except KeyboardInterrupt:
    conn.close()
