# Explaining the on_message_action Callback Function for MQTT

In IoT projects, you often want your Python program to react to messages received via MQTT. This is done using a **callback function**. Let’s break down how the provided `on_message_action` function works and how it is used to control an LED (or other light) via MQTT.

---

## What is a Callback Function?

A callback function is a special function that gets called automatically when a certain event happens. For MQTT, the `on_message` callback runs every time a message is received on a topic your client is subscribed to.

---

## Line-by-Line Explanation

```python
def on_message_action(client, userdata, message):  # This function will be called on receiving messages
    topic = message.topic  # Store topic
    msg = message.payload.decode("utf-8")  # Decode the message
```
- **client, userdata, message**: These parameters are provided by the MQTT library.
- `topic = message.topic`: Stores the name of the topic where the message was published.
- `msg = message.payload.decode("utf-8")`: The message arrives as bytes, so we decode it to a string.

---

### Light Control Using MQTT

```python
    # Light control
    if topic == 'LK/Light':
        if msg == 'ON':
            light.write(1)
            print('Topic:', topic)
            print('Message:', msg)
        elif msg == 'OFF':
            light.write(0)
            print('Topic:', topic)
            print('Message:', msg)
```
- This part checks if the incoming message’s topic is `'LK/Light'`.
- If the message is `'ON'`, it turns the light ON (`light.write(1)`).
- If the message is `'OFF'`, it turns the light OFF (`light.write(0)`).
- After changing the light, it prints the topic and message for feedback.

### What Does This Achieve?

- When a message is published to the `'LK/Light'` topic (for example, by another device or app), this function will:
  - Turn the light ON if the message is `'ON'`.
  - Turn the light OFF if the message is `'OFF'`.
- This allows remote control of your light through MQTT!

---

## How is this function used?

You must link this function to your MQTT client:

```python
mqtt_client.on_message = on_message_action
```

And make sure you subscribe to the correct topic:

```python
mqtt_client.subscribe('LK/Light')
```

Now, any MQTT message sent to `'LK/Light'` will trigger this function and control your hardware.

---

**Summary Table**

| MQTT Message Sent | Effect on Device           |
|-------------------|---------------------------|
| Topic: LK/Light, Message: ON  | Turns light ON  |
| Topic: LK/Light, Message: OFF | Turns light OFF |

---

With this approach, you can control devices from anywhere using MQTT!
