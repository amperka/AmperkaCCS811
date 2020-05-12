# AmperkaCCS811 API

## `class CCS811`

Create an object of type `CCS811` to communicate with a particular [Air Quality Sensor](https://amperka.ru/product/sensor-co2-ccs811-with-case).

### `CCS811(uint8_t i2cAddress = 0x5A)`

Constructs a new object. The argument `i2cAddress` is the specifies the board I²C address in HEX value, which is `0x5A` factory-default but can be changed is `0x5B`. If omitted, the board I²C address is `0x5A`.

### `void begin(TwoWire* wire = &Wire)`

Initializes the given interface, prepares the board for communication. The argument `wire` is an index of the TwoWire interface.
If omitted, the connection started on default hardware I²C.

### `void setDriveMode(uint8_t mode)`

Sets the drive mode, which is responsible for the sample rate and power consumption of the sensor. The argument `mode` is the range from `0` to `3`:

- `0` – Idle. IAQ measurements are disabled, and consumption is 0,034 mW.
- `1` – Constant power mode. IAQ measurement every 1 second, consumption is 46 mW.
- `2` – Pulse heating mode. IAQ measurement every 10 seconds, consumption is 7 mW.
- `3` – Low power pulse mode. IAQ measurement every 60 seconds, consumption is 1,2 mW.

### `bool available()`

Сhecks if data TVOC and CO2 are available to be read using the function `readData`. The method returns `true` if data is available to be read, otherwise `false`.

### `void readData()`

Reads and stored the sensor data values TVOC and CO2. Use the `getTVOC` and`getCO2` methods to access the received data.

### `uint16_t getTVOC()`

Returns the stored total volatile organic compounds measurement in parts per billion — ranges from `0` to `1187` in `ppb`.
This method does not read data from the sensor. To do so, the call `readData`.

### `uint16_t getCO2()`

Returns the stored estimated carbon dioxide measurement in parts per million — ranges from `400` to `8192` in `ppm`.
This method does not read data from the sensor. To do so, the call `readData`.

### `void setEnvironmentData(uint8_t humidity, float temperature)`

Sets humidity and temperature data from an external sensor to compensate for data for CSS811.

- `humidity`: relative humidity from other sensors. Ranges from `0` to `100` %.
- `temperature`: ambient temperature from other sensors. Ranges from `−25` to `50` °C.

By default, the internal algorithm uses 50 % humidity and 25 °C temperature to compensate for environmental changes. Reads humidity and temperature from other Climatic Sensors, e.g., [Troyka Meteo Sensor](https://amperka.ru/product/troyka-meteo-sensor).

### `void reset()`

Triggers a software reset of the device, which alternative to Power-On reset or Hardware Reset. The call method `begin` to resume the board for communication.
