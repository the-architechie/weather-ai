

## Summary
The idea behind this solution is to leverage the Weather AI APIs to give customers an interactive experience through WhatsApp. As such, a user does not need to log in anywhere to understand how to get weather insights or crop insights. All of that happens within WhatsApp natively.

## Architecture Diagram
![](/Users/muchami/Desktop/Screenshot 2026-06-05 at 16.03.58.png)

## Tools and Technologies
- n8n - Workflow orchestration
- Open-Meteo Geocoding API - to translate city names into coordinates
- Weather Ai apis (Weather and Tree Analysis api)
- Whatsapp Messaging API

## Languages Used
- JavaScript
## How to Interact with the solution
- Simply chat with the bot in WhatsApp
- The bot will guide you through the process of getting started
- The bot will provide you with weather insights and crop insights(if any)
A user can interact with the bot in 4 distinct ways. They can send:

- A greeting
- A name of a city
- A pin location
- A picture of their crop
- Greeting
The bot detects the user's greeting (e.g., hi, hello) and triggers the Welcome flow. This flow guides the user through the process of getting started, explaining exactly what they need to provide to get meaningful weather or crop insights.

#### City Name
When a user sends the name of a city, the workflow leverages the free Open-Meteo Geocoding API to resolve the city name into exact coordinates. These coordinates are then passed to the Weather AI API. In return, the user receives a summary of the current weather and a prediction for the next 2 days.
Example response:
```json
Weather at your location 
🌡️ Currently: 25°C
🌤️ Conditions: Overcast ☁️
💨 Wind: 9.2 km/h
📅 Forecast:
* Fri, Jun 5: 13°C - 26°C | Light drizzle 🌦️ (Rain: 20%)
* Sat, Jun 6: 16°C - 26°C | Moderate drizzle 🌧️ (Rain: 45%)
* Sun, Jun 7: 16°C - 25°C | Light drizzle 🌦️ (Rain: 35%)
  Pin Location
```
#### Pin Location
  When a user sends a live or static pin location, the webhook payload extracts the exact coordinates of the pin and passes them directly to the Weather AI API.
Example response:
```json
Weather at your location 📍
🌡️ Currently: 22°C
🌤️ Conditions: Overcast ☁️
💨 Wind: 12.3 km/h
📅 Forecast:
* Fri, Jun 5: 12°C - 24°C | Light drizzle 🌦️ (Rain: 20%)
* Sat, Jun 6: 15°C - 24°C | Light drizzle 🌦️ (Rain: 45%)
* Sun, Jun 7: 13°C - 23°C | Light drizzle 🌦️ (Rain: 35%)
```
#### Media (Tree Analysis)
  When a user sends a picture of their crop, the bot automatically downloads the binary file from the WAHA server. It then extracts the user's caption from the Webhook payload and POSTs it alongside the image to the Tree Analysis API. The user's original caption is dynamically injected back into the final response for a conversational experience.

```text
🌳 Tree Analysis complete
_You asked: "Are these brown spots dangerous?"_
We couldn't identify any trees clearly from the image you provided. This can happen if the photo is blurry, taken from far away, or doesn't show trees prominently.
Tip: Try taking a clearer photo of your trees from a closer angle, and make sure the area is well-lit.
```
