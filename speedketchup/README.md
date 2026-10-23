# Home Assistant App: SpeedKetchup

Periodic internet speed tests with charts, a full history and MQTT sensors.

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

![SpeedKetchup web page](https://raw.githubusercontent.com/gatispei/speedketchup-files/main/img/ketchup.png)

[SpeedKetchup](https://github.com/gatispei/speedketchup) tests your internet
connection every few minutes against speedtest.net servers and keeps every
result:

- speed and latency charts, each with a distribution of the shown results
- every result, or daily, monthly, time of day and time of week averages
- a sortable table of every result, failed tests included
- in the Home Assistant sidebar, no port needed
- a SpeedKetchup device in Home Assistant over MQTT, set up by itself with the
  Mosquitto broker app: download, upload, latency, last test, server, ISP and
  a problem sensor
- Prometheus metrics and InfluxDB line protocol output

A single small static binary, the results are a plain csv kept in Home
Assistant backups.

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
