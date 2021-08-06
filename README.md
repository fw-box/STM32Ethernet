# STM32 Ethernet Library for Arduino

The library is for 32F746GDISCOVERY Discovery kit.

## Dependency

This library is based on LwIP.

https://github.com/stm32duino/LwIP

## Example

	#include <LwIP.h>
	#include <STM32Ethernet.h>

	EthernetClient client;

	// random MAC address
	byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };

	void setup() {
	    Serial.begin(115200);

	    Serial.println("Connecting to Ethernet");

	    if (!Ethernet.begin(mac)) {
		Serial.println("ERROR: Could not connect to ethernet!");
		while(1);
	    }

	    Serial.println("Connecting to example.com");

	    if (!client.connect("example.com", 80)) {
		Serial.println("ERROR: Could not connect to example.com!");
		while(1);
	    }

	    Serial.println("Connected, sending request");

	    client.println("GET / HTTP/1.1");
	    client.println("Host: example.com");
	    client.println();
	    client.flush();

	    Serial.println("Request sent, printing response");
	}

	void loop() {
	    if (client.available()) {
		char c = client.read();
		Serial.print(c);
	    }

	    if (!client.connected()) {
		Serial.println();
		Serial.println("disconnecting.");
		client.stop();
		while (1);
	    }
	}

