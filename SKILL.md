---
name: ktmb-agent-skills
description: Look up KTM Malaysian train schedules — next train, full trip search, and station departure board.
metadata:
  homepage: https://ktmb-api-769756648802.us-central1.run.app
---

# KTMB Train Schedule

Answer questions about KTM Komuter, ETS, and Intercity train schedules in Malaysia. The script automatically uses the current Malaysia date and time when the user does not specify one.

## Usage Examples

- "Next train from KL Sentral to Ipoh"
- "Show me all trains from Seremban to KL Sentral today"
- "ETS trains from Butterworth to KL Sentral tomorrow"
- "Trains after 2pm from Gemas to JB Sentral on Friday"
- "Show departures from Subang Jaya"
- "What trains are leaving Ipoh towards KL soon?"

## Implementation

Call the `run_js` tool with script `index.html` and a JSON `data` parameter. Always include `action` to indicate the intent:

### `action: "next_train"` — single next departure between two stations
- **from**: Required. Departure station name (e.g. `"KL Sentral"`, `"Seremban"`).
- **to**: Required. Destination station name.
- **service_type**: Optional. `"Komuter"`, `"ETS"`, or `"Intercity"`.

### `action: "search_trains"` — all trips on a given date
- **from**: Required. Departure station name.
- **to**: Required. Destination station name.
- **date**: Optional. Travel date as `YYYY-MM-DD`. Omit when the user says "today" or gives no date — the script defaults to today's Malaysia date. For "tomorrow" or named days, resolve to an absolute date yourself.
- **time**: Optional. Earliest departure time as `HH:MM:SS` (e.g. `"14:00:00"`). Include when user says "after X pm".
- **service_type**: Optional.

### `action: "departure_board"` — upcoming departures from one station
- **station**: Required. Station name.
- **direction**: Optional. Headsign keyword if user wants a specific direction (e.g. `"Port Klang"`).
- **service_type**: Optional.

**Choosing the right action:**
- Use `next_train` when the user asks for "the next train" or "when is the next train".
- Use `search_trains` when the user asks to "show all trains", "list trains", or specifies a date/time.
- Use `departure_board` when only one station is mentioned (no destination).

**Response instructions:**
- Present results as a concise, friendly list. Include train number, departure time, arrival time, and service name.
- For `search_trains` with many results (>8), show the first 8 and note the total count.
- If no trains are found, say so and suggest alternatives (different date, check station names).
- Always respond in the same language as the user's original prompt.
