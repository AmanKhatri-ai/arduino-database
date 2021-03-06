# Arduino Database

A key-value database implementation for Arduino boards

**The library currently only supports _ESP8266 MCU_ from Espressif, support for other boards will be added in future releases.**




## Tested Devices

* NodeMCU




## Features

* **Supports EEPROM memory**
* **Supports SPIFFS memory**
* **Easy access to data using keys**




## Dependencies

This library required **ESP8266 Core SDK**

For Arduino IDE, ESP8266 Core SDK [installation instructions](https://github.com/esp8266/Arduino#installing-with-boards-manager).




## Installation

For Arduino IDE, download zip file from the repository by selecting **Code** dropdown at the top of repository, select **Download ZIP**

From Arduino IDE, select menu **Sketch** -> **Include Library** -> **Add .ZIP Library...**

Choose **arduino-database-master.zip** that previously downloaded.

Go to menu **Files** -> **Examples** -> **ArduinoDatabase** and choose one from examples.




## Usage

See [Examples](/examples)

### Initialization

```C++
// Include Arduino Database library
#include "ArduinoDb.h"

// For SPIFFS- Declare the ArduinoDb object in global scope
// To use SPIFFS memory for storing database
ArduinoDb arduinoDb;

// For EEPROM- Declare the ArduinoDb object in global scope
// To use EEPROM memory for storing the database
ArduinoDb arduinoDbEEPROM(1024);	// max is 4096

setup()
{
	// Initiate the database
	arduinoDb.begin();
	arduinoDbEEPROM.begin();

	// Format the memory before using(only required for first time use of library)
	arduinoDb.format();
	arduinoDbEEPROM.format();
}
```


### Initiate Database

The sketch must initiate the database before starting to use it.

To initiate the database use `begin()` function.

Returns `true` if the database initialization operation is successful.

Refer the following example

```C++
if (arduinoDb.begin())
{
	Serial.println("Initialization successful");
}
else
{
	Serial.println("Initialization failed");
}

// OR / AND

if (arduinoDbEEPROM.begin())
{
	Serial.println("Initialization successful");
}
else
{
	Serial.println("Initialization failed");
}
```


### Format Database

All the data stored in the database can be formatted using this `format()` function.

**`format()` should always be used before initialting the database for the very first time. After formatting make sure to remove/comment the `format()` line and re-upload the code.**

It returns `true` if the format operation was successful.

Refer the following example

```C++
if (arduinoDb.format())
{
	Serial.println("Format successful");
}
else
{
	Serial.println("Format failed");
}

// OR / AND

if (arduinoDbEEPROM.format())
{
	Serial.println("Format successful");
}
else
{
	Serial.println("Format failed");
}
```


### Insert Values

To insert values into the database use `insert(String key, String value)` function.

Pass **key** and **value** in string form when calling `insert`.

Returns `SUCCESS` if value inserted successfully
		`FAILURE` if value insertion failed
		`MEM_FULL` if memory is full and their is no space for new data

In case of `MEM_FULL`, you need to remove some data from database using `remove(String key)` function.

Refer the following example

```C++
int result = arduinoDb.insert("key1", "value1");

if (result == SUCCESS)
{
	Serial.println("Value inserted into database successfully");
}
else if (result == FAILURE)
{
	Serial.println("Value insertion failed");
}
else if (result == MEM_FULL)
{
	Serial.println("Memory full, please remove some data");
}

// OR / AND

int result = arduinoDbEEPROM.insert("key1", "value1");

if (result == SUCCESS)
{
	Serial.println("Value inserted into database successfully");
}
else if (result == FAILURE)
{
	Serial.println("Value insertion failed");
}
else if (result == MEM_FULL)
{
	Serial.println("Memory full, please remove some data");
}
```


### Remove Values

To remove key-value pairs from the database use `remove(String key)` function.

Pass **key** in string form as argument when calling `remove`.

Returns `true` if removal successful
		`false` if removal failed

Refer the following example

```C++
if (arduinoDb.remove("key1"))
{
	Serial.println("Value removed successfully");
}
else
{
	Serial.println("Value remove failed");
}

// OR / AND

if (arduinoDbEEPROM.remove("key1"))
{
	Serial.println("Value removed successfully");
}
else
{
	Serial.println("Value remove failed");
}
```


### Get / Obtain Stored Values

To obtain the values stored in the database use their associated keys by calling `get(String key, String defaultValue)` function.

Pass **key** in string form whose value is to be obtained
	 **defaultValue** in string form which will be retured in-case key is not found in database.

Returns value associated with key in string form, if key not found return `defaultValue`.

Refer the following example

```C++
String val = arduinoDb.get("Key1", "Key not found");

Serial.println(val);

// OR / AND

String val = arduinoDbEEPROM.get("Key1", "Key not found");

Serial.println(val);
```


### Checking for Key existence

Use `exists(String key)` to check weather a key exists in database or not.

Pass **key** in string form to check for it's existence.

Returns `true` if key exists
		`false` if key not exists

Refer the following example

```C++
if (arduinoDb.exists("key1"))
{
	Serial.println("Key exists");
}
else
{
	Serial.println("Key not exists");
}

// OR / AND

if (arduinoDbEEPROM.exists("key1"))
{
	Serial.println("Key exists");
}
else
{
	Serial.println("Key not exists");
}
```


## Key points

* Max size supported for EEPROM memory is 4096 bytes
* Max size supported for SPIFFS memory is 10240 bytes