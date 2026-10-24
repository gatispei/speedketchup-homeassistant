# Changelog

## 0.5.0

- SpeedKetchup 0.5.0, the page in the sidebar now shows the live test, its
  Start button and refreshes after every test
- The app listens on port 8288, the SpeedKetchup default, map 8288/tcp for
  direct access

## 0.4.0

- First release of the app, with SpeedKetchup 0.4.0
- The web page in the Home Assistant sidebar through ingress
- Publishes to the Mosquitto broker app by itself when no MQTT broker is set
- Options for the test interval, server, durations, connections, InfluxDB and
  MQTT
