# Home Assistant App: SpeedKetchup

SpeedKetchup tests the speed of your internet connection every few minutes
against [speedtest.net](https://www.speedtest.net) servers, keeps every result
and shows them on its own web page: speed and latency charts with a
distribution next to each, daily, monthly, time of day and time of week
averages, and a sortable table of every result, failed tests included.

With the Mosquitto broker app installed the results also turn up in Home
Assistant as a SpeedKetchup device, with no setup at all.

## Installation

1. Install the app and start it.
2. Open it with **Open Web UI**, or turn on **Show in sidebar** to have it a
   click away.

The first test runs right after the start and takes about half a minute.

## Sensors in Home Assistant

SpeedKetchup publishes every result over MQTT with
[MQTT discovery](https://www.home-assistant.io/integrations/mqtt/#mqtt-discovery),
so Home Assistant makes a **SpeedKetchup** device on its own with:

- download and upload speed
- latency, idle and while downloading and uploading
- the time of the last test
- a problem sensor that is on when the last test failed
- the speedtest server and your ISP

When the [Mosquitto broker](https://github.com/home-assistant/addons/blob/master/mosquitto/DOCS.md)
app is installed and the **MQTT broker** option is empty, SpeedKetchup uses it,
with the credentials the broker app hands out to apps. The MQTT integration has
to be set up in Home Assistant, which it offers to do itself once the broker
app runs. Restart SpeedKetchup after installing the broker app.

To use another broker, set **MQTT broker** (and the user and password). Only
plain MQTT is supported, no TLS. To publish nothing, turn off **Use the
Mosquitto broker app** and leave **MQTT broker** empty.

The state goes to `speedketchup/state` as json, the discovery configs under
`homeassistant/`, all retained. The speed and latency sensors go unavailable
when there was no successful test for two intervals.

## Options

| Option | Default | |
|-|-|-|
| Test interval | 10 | minutes between tests |
| Speedtest server | | `host[:port]` of a server to always use, empty picks the one with the lowest latency |
| Download duration | 10 | seconds, 0 skips the download test |
| Upload duration | 10 | seconds, 0 skips the upload test |
| Download connections | 8 | parallel connections for the download test |
| Upload connections | 8 | parallel connections for the upload test |
| InfluxDB write URL | | also write every result to InfluxDB, see below |
| InfluxDB token | | sent as `Authorization: Token <token>` |
| Use the Mosquitto broker app | on | publish to the Mosquitto broker app when no broker is set |
| MQTT broker | | `host[:port]` of a broker, 1883 by default |
| MQTT user, MQTT password | | for the broker above |

Every test moves real data, the speed times the duration in each direction:
on a 100 Mbit/s line some 125 MB down and as much up per test at the default
10 seconds, every 10 minutes. On a metered connection lengthen the interval or
shorten the durations.

## InfluxDB

**InfluxDB write URL** is the complete line protocol write url, http only, for
example with the InfluxDB app:

```
http://a0d7b954-influxdb:8086/write?db=speedketchup&u=user&p=password
```

or for InfluxDB 2 with a token in **InfluxDB token**:

```
http://influxdb:8086/api/v2/write?org=home&bucket=speedketchup
```

## Network port

The page in the sidebar needs no port. Map the **8288/tcp** port in the
**Network** section to also reach SpeedKetchup directly at
`http://homeassistant.local:8288`, to scrape its Prometheus metrics at `/metrics`,
or to use it as a speedtest server for other speedketchup instances. Anyone who
can reach that port can see the results and start a test, there is no login.

## Data

The results are kept in `speedketchup-results.csv` in the app's private data
folder, so they are part of Home Assistant backups and survive updates.
Uninstalling the app removes them.

## Logs

SpeedKetchup logs a line for every step of every test, and every connection to
its web page, so its log is busy. Errors are marked as such.

## Support

- Issues and questions: <https://github.com/gatispei/speedketchup/issues>
- SpeedKetchup itself: <https://github.com/gatispei/speedketchup>
