# Subscribing to an MQTT Topic in Python (with paho-mqtt)

To receive messages in your Python IoT project using MQTT, you need to **subscribe** to a topic. This means your program tells the MQTT broker, "I want to get all messages sent to this topic!" — and the broker will deliver them to your client.

---

## What Is a Topic?

- Topics are like channels or addresses where messages are published and received.
- They look like strings, for example: `"home/livingroom/temperature"` or `"test/topic"`.
- IoT devices can **publish** (send) or **subscribe** (listen) to topics.

---

## How to Subscribe to a Topic Using paho-mqtt

### 1. Make Sure You Have an MQTT Client

Set up your client and connect to the broker first:

```python
import paho.mqtt.client as paho

mqtt_client = paho.Client()
mqtt_client.connect('192.168.248.70', 1883)
mqtt_client.loop_start()
```

---

### 2. Define an on_message Callback Function

This function will run every time a new message arrives on a topic you are subscribed to.

```python
def on_message_action(client, userdata, message):
    print("Received message on topic:", message.topic)
    print("Message payload:", message.payload.decode())
```

Then, link it to your client:

```python
mqtt_client.on_message = on_message_action
```

---

### 3. Subscribe to a Topic

Use the `subscribe()` method:

```python
mqtt_client.subscribe("test/topic")
```

- `"test/topic"` is the topic name. Replace it with your actual topic.
- You can subscribe to more than one topic by calling `subscribe()` multiple times.

#### Wildcards

- Use `+` as a wildcard for one level:  
  `"home/+/temperature"` matches `"home/livingroom/temperature"` and `"home/bedroom/temperature"`
- Use `#` as a wildcard for multiple levels:  
  `"home/#"` matches `"home/livingroom/temperature"`, `"home/kitchen/humidity"`, etc.

---

### 4. Full Example

```python
import paho.mqtt.client as paho

def on_message_action(client, userdata, message):
    print("Received message on topic:", message.topic)
    print("Message payload:", message.payload.decode())

mqtt_client = paho.Client()
mqtt_client.connect('192.168.248.70', 1883)
mqtt_client.on_message = on_message_action
mqtt_client.loop_start()

mqtt_client.subscribe("test/topic")

import time
while True:
    time.sleep(1)
```

---

### 5. Quality of Service (QoS)

You can set a QoS (Quality of Service) level when subscribing:

```python
mqtt_client.subscribe("test/topic", qos=1)
```
- `qos=0`: At most once (default, fastest, least reliable)
- `qos=1`: At least once (more reliable)
- `qos=2`: Exactly once (most reliable, slowest)

---

### 6. Important Notes

- Subscribing must be done **after** connecting to the broker.
- You must keep your program running (e.g., with a `while True:` loop) to continue receiving messages.
- You can unsubscribe from a topic with `mqtt_client.unsubscribe("test/topic")`.

---

**Summary Table**

| Step                | Code Example                                  |
|---------------------|-----------------------------------------------|
| Connect to Broker   | `mqtt_client.connect(...)`                    |
| Start Loop          | `mqtt_client.loop_start()`                    |
| Set Callback        | `mqtt_client.on_message = on_message_action`  |
| Subscribe           | `mqtt_client.subscribe("topic/name")`         |

---

With these steps, your Python program can listen for MQTT messages from any device publishing to the topic!
