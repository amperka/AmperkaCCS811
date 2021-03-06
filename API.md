# AmperkaCCS811 API

## `class AmperkaCCS811`

Create an object of type `CCS811` to communicate with a particular [Air Quality Sensor](https://amperka.ru/product/sensor-co2-ccs811-with-case).

### `AmperkaCCS811(uint8_t i2cAddress = 0x5A)`

Constructs a new object. The argument `i2cAddress` specifies the board I²C address in HEX value, which is `0x5A` factory-default but can be changed to `0x5B`. If omitted, the board I²C address is `0x5A`.

### `bool begin(TwoWire* wire = &Wire)`

Initializes the given interface, prepares the board for communication. Returns `true` if device is set up, otherwise `false`. The argument `wire` is a pointer to the TwoWire interface. If omitted, the connection starts with default hardware I²C.

### `void setDriveMode(DriveMode mode)`

Sets the drive mode, which is responsible for the sample rate and power consumption of the sensor. Valid argument `mode` values are:

- `DriveMode::SLEEP`: Idle. Measurements are disabled, and consumption is 0,034 mW.
- `DriveMode::PERIOD_60S`: low power pulse mode. Measurement every 60 seconds, consumption is 1,2 mW.
- `DriveMode::PERIOD_10S`: pulse heating mode. Measurement every 10 seconds, consumption is 7 mW.
- `DriveMode::PERIOD_1S`: constant power mode. Measurement every 1 second, consumption is 46 mW. This mode is set by default.
- `DriveMode::PERIOD_250MS`: constant ultra power mode. Measurement every 250 milliseconds, consumption more then 46 mW.

### `bool available()`

Сhecks if new TVOC and CO2 data is available. Returns `true` if data is readable, otherwise `false`. Use the `read` method to read and store the sensor data.

### `bool read()`

Reads and stores the sensor data values TVOC and CO2. Returns value:

- `true`: if data is read successfully. Use the `getTVOC` and `geteCO2` methods to access the received data.
- `false`: if data is read with an error. Use the `getErrorID` method to access the error ID.

### `String getErrorID()`

Returns the stored Error ID, when method `read` returned `false`.

- `WRITE_REG_INVALID`: the CCS811 received an I²C write request addressed to this station but with invalid register address ID.
- `READ_REG_INVALID`: the CCS811 received an I²C read request to a mailbox ID that is invalid.
- `MEASMODE_INVALID`: the CCS811 received an I²C request to write an unsupported mode to MEAS_MODE.
- `MAX_RESISTANCE`: the sensor resistance measurement has reached or exceeded the maximum range.
- `HEATER_FAULT`: the Heater current in the CCS811 is not in range.
- `HEATER_SUPPLY`: the Heater voltage is not being applied correctly.

### `uint16_t getTVOC()`

Returns the stored total volatile organic compounds (TVOC), when method `read` returned `true`. Measurement TVOC in parts per billion — ranges from `0` to `1187` in `ppb`.

### `uint16_t geteCO2()`

Returns the stored estimated equivalent carbon dioxide (eCO2), when method `read` returned `true`. Measurement eCO2 in parts per million — ranges from `400` to `8192` in `ppm`.

### `void setEnvironmentData(float humidity, float temperature)`

Sets humidity and temperature data from other climatic sensors to compensate CSS811 data.

- `humidity`: relative humidity from other sensors. Ranges from `0` to `100` %.
- `temperature`: ambient temperature from other sensors. Ranges from `−25` to `50` °C.

By default, the internal algorithm uses 50 % humidity and 25 °C temperature to compensate for environmental changes. Read humidity and temperature from other climatic sensors, e.g., [Troyka Meteo Sensor](https://amperka.ru/product/troyka-meteo-sensor).

### `void reset()`

Triggers a software reset of the device, which is an alternative to power-on reset and hardware reset.
