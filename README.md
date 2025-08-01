# Smart-Hydroponics-System
A smart hydroponics system is a soil-free farming setup that uses sensors, automation, and IoT technology to precisely control plant-growing conditions. 🌱 It monitors and adjusts parameters like pH, nutrient levels, temperature, humidity, and water flow in real time to optimize plant health and yield. 









hydroponics system that consists of three chambers: one for storing water, one for storing a mixture of water and nutrients, and another for holding concentrated nutrients. Each chamber is equipped with a water level sensor to monitor the liquid levels. In the nutrient mixture chamber, a pH sensor is used to measure the acidity or alkalinity of the solution, and a DHT sensor is included to track environmental parameters like temperature and humidity. Each chamber has a dedicated pump, and all three pumps are connected to individual relays, allowing the circuit to manage fluid flow and nutrient balance automatically based on sensor input.



This project automates the nutrient and water delivery to plants grown hydroponically. It uses:

Sensors to monitor tank levels, temperature, humidity, pH, and TDS.

Relays to control water, nutrient, and mixing pumps.

LEDs to indicate tank statuses.

Wi-Fi & ThingSpeak for data logging and cloud monitoring.

Web Server for real-time manual control and status view.

⚡ Circuit Diagram Explanation
The uploaded image shows these key components:

1. Power Supply
LiPo Battery → connected to charging module (TP4056).

Output of TP4056 powers the ESP32 and sensors via 5V & GND.

2. Microcontroller
ESP32 – the brain of the system, managing all inputs/outputs and cloud communication.

3. Sensors
Sensor	GPIO Pin	Purpose
DHT11	GPIO 27	Reads air temperature & humidity
Water Level Sensor	GPIO 12	Checks water tank level
Nutrient Level Sensor	GPIO 13	Checks nutrient tank level
Mix Level Sensor	GPIO 14	Checks mixed tank level
pH Sensor (Analog)	GPIO 34	Measures pH level of water
TDS Sensor (Analog)	GPIO 35	Measures nutrient concentration
pH Temp Sensor	GPIO 33	Measures water temperature

4. Relays (Pump Control)
Pump	GPIO Pin	Function
Water Pump	GPIO 18	Fills mix tank
Nutrient Pump	GPIO 19	Adds nutrients to mix
Mix Pump	GPIO 21	Delivers mix to plants

These relays switch 5V motors (shown in the image) that pump water/nutrient/mix.

5. LED Indicators
LED Color	GPIO Pin	Status Shown
Red	GPIO 2	Mix tank LOW
White	GPIO 4	Nutrient tank LOW
Blue	GPIO 15	Water tank LOW

🔄 Working Principle
🟢 Startup (setup())
Initializes GPIOs, sensors, Wi-Fi, and ThingSpeak.

Reads saved Wi-Fi and config from flash (Preferences).

Starts web server for dashboard control.

📡 Loop Function
Runs updateSystem() every 20 seconds:

Sensor Reading

Collects water levels, pH, TDS, temperature, and humidity.

Indicator Update

LEDs turn ON when a tank is LOW.

Pump Control

If water/nutrient tanks are full:

Fills the mix tank with water and nutrient.

Activates the mix pump when mix tank is full.

Data Logging & Cloud Sync

Logs readings on Serial Monitor.

Sends sensor data to ThingSpeak fields.

Web Dashboard Update

Updates the web page with latest sensor readings.

SMS Alert

Sends alert via REST API if any tank is low.

🌐 Web Dashboard Features
Served at ESP32's IP, this dashboard allows:

Manual pump ON/OFF control (via /pump_name/on and /off)

Live sensor data refresh every 3 seconds via JavaScript and /data

☁️ ThingSpeak Integration
Uses field mapping:

yaml
Copy
Edit
Field 1: Temperature
Field 2: Humidity
Field 3: pH
Field 4: TDS
Field 5: Water Level
Field 6: Nutrient Level
Field 7: Mix Level
Field 8: pH Temp
Helps you view trends and set triggers.

🛠️ Additional Notes
Calibration factors (ph_calibration, tds_calibration) help tune analog readings.

You’re averaging analog reads over 10 samples to reduce noise.

sendSMSAlert() sends a GET request to a local server at 192.168.184.52:5000 to trigger alerts (you must run a Flask/Python backend there).

🔁 Full System Operation Flow
pgsql
Copy
Edit
Startup → Wi-Fi Connect → Read Sensors →
Update LEDs → Check Tanks →
If Mix Tank LOW → Pump Water & Nutrient →
When Mix Tank Full → Mix Pump ON →
Log & Send to Cloud → Serve Web Dashboard →
Trigger SMS Alerts if Needed





✅ Advantages
Efficient Resource Management: The use of water level sensors ensures that each tank maintains optimal levels, minimizing waste and preventing overflow.

Automated Nutrient Control: With the pH sensor, the system can automatically adjust the acidity or alkalinity by adding water or nutrients, promoting healthy plant growth.

Environmental Monitoring: The DHT sensor adds another layer of control by tracking temperature and humidity, which are crucial for plant development.

Modular Setup: Having separate tanks allows for better mixing and maintenance, as each solution is handled independently before delivery to the plants.

Scalability: Relays and pumps make the system adaptable for larger setups by integrating more chambers or sensors.
