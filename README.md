
# 🔧 Arduino IoT Dashboard

A **real-time IoT monitoring dashboard** built with **React, TypeScript, and Vite**. Monitor temperature, humidity, and gas levels from Arduino-compatible devices with interactive charts, live updates, and a responsive user interface.

## 🚀 Features

- ⚡ **Real-Time Monitoring** — Live sensor data updates from Arduino devices.
- 📊 **Interactive Visualizations** — Beautiful charts powered by Recharts.
- 🌡️ **Multi-Sensor Support** — Monitor temperature, humidity, and gas levels.
- 📱 **Responsive Design** — Works seamlessly on desktop, tablet, and mobile.
- ⚙️ **Easy Configuration** — Configure the Arduino IP address, port, endpoint, and update interval.
- 💎 **Modern UI** — Built using shadcn/ui and Tailwind CSS.
- 🔌 **Connection Status** — Monitor the connection between the dashboard and Arduino.

## Dashboard Workflow Diagram
  <img width="1536" height="1024" alt="Arduino IoT Dashboard Workflow" src="https://github.com/user-attachments/assets/29a6e35c-17a4-4262-8403-0537766de549" />


## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| React 18.3 | User interface development |
| TypeScript | Type-safe development |
| Vite | Fast development server and build tool |
| Tailwind CSS | Responsive styling |
| shadcn/ui | Reusable UI components |
| Recharts | Data visualization |
| Lucide React | Icons |
| Arduino / ESP32 | Sensor data collection |

## 📋 Prerequisites

- Node.js 18 or later
- npm
- Arduino-compatible board with Wi-Fi support, such as ESP32
- Temperature, humidity, and gas sensors
- Wi-Fi network connectivity
- Arduino HTTP endpoint returning JSON sensor data

## 📦 Installation

### 1. Clone the repository

```bash
git clone <YOUR_GIT_URL>
cd <YOUR_PROJECT_NAME>
```

### 2. Install dependencies

```bash
npm install
```

### 3. Start the development server

```bash
npm run dev
```

Open the local URL displayed in your terminal. The default port depends on your Vite configuration.

## 🔌 Arduino Configuration

### Expected JSON Response

The Arduino endpoint should return data in the following format:

```json
{
  "temperature": 23.5,
  "humidity": 55.2,
  "gas": 350,
  "timestamp": 1699564800000
}
```

| Field | Description |
|---|---|
| `temperature` | Temperature in degrees Celsius |
| `humidity` | Relative humidity percentage |
| `gas` | Gas sensor reading |
| `timestamp` | Timestamp in milliseconds |

### Connection Steps

1. Open the Arduino IoT Dashboard.
2. Click **Setup Arduino**.
3. Enter the Arduino IP address.
4. Configure the port and endpoint path, such as `/sensors`.
5. Set the refresh interval in milliseconds.
6. Click **Connect** to start monitoring sensor data.

## 💻 Example Arduino Code

This example demonstrates an ESP32 web server that returns temperature, humidity, and gas sensor readings.

**Required libraries:**
- WiFi
- WebServer
- DHT sensor library

```cpp
#include <WiFi.h>
#include <WebServer.h>
#include <DHT.h>

#define DHTPIN 4
#define DHTTYPE DHT22

const char* ssid = "YOUR_SSID";
const char* password = "YOUR_PASSWORD";

DHT dht(DHTPIN, DHTTYPE);
WebServer server(80);

void handleSensors() {
    float temperature = dht.readTemperature();
    float humidity = dht.readHumidity();
    int gas = analogRead(34);

    if (isnan(temperature) || isnan(humidity)) {
        server.send(
            500,
            "application/json",
            "{\"error\":\"Sensor read failed\"}"
        );
        return;
    }

    String json = "{";
    json += "\"temperature\":" + String(temperature, 1) + ",";
    json += "\"humidity\":" + String(humidity, 1) + ",";
    json += "\"gas\":" + String(gas) + ",";
    json += "\"timestamp\":" + String(millis());
    json += "}";

    server.sendHeader("Access-Control-Allow-Origin", "*");
    server.send(200, "application/json", json);
}

void setup() {
    Serial.begin(115200);
    dht.begin();

    WiFi.begin(ssid, password);

    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.println("Connecting to Wi-Fi...");
    }

    Serial.print("ESP32 IP Address: ");
    Serial.println(WiFi.localIP());

    server.on("/sensors", HTTP_GET, handleSensors);
    server.begin();
}

void loop() {
    server.handleClient();
}
```

**Note:** This example assumes an ESP32, a DHT22 sensor connected to GPIO 4, and a compatible analog gas sensor connected to GPIO 34. Check your hardware pin assignments before use. The timestamp uses `millis()`, which measures elapsed time since startup.

## 📁 Project Structure

```text
src/
├── components/
│   ├── ui/                  # shadcn/ui components
│   ├── ArduinoSetup.tsx     # Arduino setup wizard
│   ├── Dashboard.tsx        # Main dashboard
│   ├── SensorCard.tsx       # Individual sensor readings
│   ├── ChartWidget.tsx      # Sensor charts
│   └── StatusPanel.tsx      # Connection status
├── services/
│   └── arduinoService.ts    # Arduino data fetching
├── pages/
│   └── Index.tsx            # Home page
└── main.tsx                 # Application entry point
```

## 🔒 Troubleshooting

- **Connection failed:** Check the Arduino IP address, port, and endpoint.
- **No sensor readings:** Verify sensor wiring and power supply.
- **CORS errors:** Configure the Arduino or backend to allow requests from the dashboard.
- **Invalid JSON:** Ensure the endpoint returns valid JSON with the expected field names.
- **Dashboard not updating:** Check the refresh interval and browser console.

For public deployments, use HTTPS, appropriate authentication, and a secure backend rather than exposing the Arduino web server directly to the internet.

## 💡 Future Enhancements

- 🚨 **Threshold Alerts** — Notify users when sensor readings exceed safe limits.
- 📈 **Data Logging** — Store sensor readings in a database or cloud service.
- 🌍 **Additional Sensors** — Add support for light, motion, and pressure sensors.
- 🔐 **Authentication** — Implement secure login and access control.
- 🧾 **Automated Reports** — Generate daily and weekly sensor reports.
- 📥 **Data Export** — Export sensor history to CSV.
- 📡 **Multiple Devices** — Monitor multiple Arduino devices from one dashboard.

## 🤝 Contributing

Contributions, suggestions, and bug reports are welcome. Fork the repository, create a feature branch, and submit a pull request.

## 📄 License

Add a `LICENSE` file to specify the license under which this project is distributed.

---

**Built with ❤️ using React, TypeScript, and Arduino-compatible hardware.**
