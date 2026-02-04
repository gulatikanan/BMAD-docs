# API Reference — Concrete Endpoints & Examples

> **Purpose**: Concrete API documentation for BMAD agents and developers. For the full OpenAPI spec and all endpoints, see [docs/api-reference/v2/openapi.json](/docs/api-reference/v2/openapi.json) and the [Cal.com API v2 docs](https://cal.com/docs/api-reference/v2/introduction).

## Base URL & Authentication

- **Base URL**: `https://api.cal.com/v2` (or your self-hosted API URL).
- **Authentication**: `Authorization: Bearer <API_KEY>`. API keys have prefix `cal_` (test) or `cal_live_` (live).
- **Version header**: Many endpoints require `cal-api-version` (e.g. `2024-08-13` for bookings).

---

## 1. Create a Booking

**Endpoint**: `POST /v2/bookings`

**Headers**:
- `Authorization: Bearer <API_KEY>` (optional for public event types)
- `cal-api-version: 2024-08-13` (required)

**Request body example** (regular booking by event type ID):

```json
{
  "eventTypeId": 123456,
  "start": "2026-02-15T14:00:00.000Z",
  "attendee": {
    "name": "Jane Doe",
    "email": "jane@example.com"
  },
  "metadata": {}
}
```

**Alternative**: Book by slug and username (no auth required for public types):

```json
{
  "eventTypeSlug": "30min",
  "username": "johndoe",
  "start": "2026-02-15T14:00:00.000Z",
  "attendee": {
    "name": "Jane Doe",
    "email": "jane@example.com"
  }
}
```

**Response** (201 Created):

```json
{
  "booking": {
    "id": 789,
    "uid": "abc123xyz",
    "title": "30 Minute Meeting",
    "description": null,
    "startTime": "2026-02-15T14:00:00.000Z",
    "endTime": "2026-02-15T14:30:00.000Z",
    "status": "ACCEPTED",
    "attendees": [
      {
        "name": "Jane Doe",
        "email": "jane@example.com"
      }
    ],
    "metadata": {}
  }
}
```

**Notes**: `start` must be in UTC. For recurring event types use the same endpoint with recurring payload; for instant meetings pass `"instant": true` (team event types).

---

## 2. Get All Event Types

**Endpoint**: `GET /v2/event-types`

**Headers**:
- `Authorization: Bearer <API_KEY>` (required for authenticated user's event types)
- `cal-api-version: 2024-06-14`

**Query parameters** (optional):
- `username` — Filter by user's event types.
- `eventSlug` — Specific event type slug (requires `username`).
- `orgSlug` / `orgId` — Organization context.
- `sortCreatedAt` — `asc` or `desc`.

**Example request**:

```
GET /v2/event-types?username=johndoe
Authorization: Bearer cal_xxxxx
cal-api-version: 2024-06-14
```

**Response** (200 OK):

```json
{
  "eventTypes": [
    {
      "id": 123456,
      "slug": "30min",
      "title": "30 Minute Meeting",
      "length": 30,
      "hidden": false,
      "locations": [
        {
          "type": "integrations:google:meet",
          "link": "https://meet.google.com/..."
        }
      ]
    }
  ]
}
```

---

## 3. Get All Bookings

**Endpoint**: `GET /v2/bookings`

**Headers**:
- `Authorization: Bearer <API_KEY>` or managed user access token
- `cal-api-version: 2024-08-13`

**Query parameters** (optional):
- `status` — `upcoming`, `recurring`, `past`, `cancelled`, `unconfirmed` (comma-separated for multiple).
- `attendeeEmail` — Filter by attendee email.

**Example request**:

```
GET /v2/bookings?status=upcoming,past
Authorization: Bearer cal_xxxxx
cal-api-version: 2024-08-13
```

**Response** (200 OK): Paginated list of bookings with structure similar to the create-booking response.

---

## Full Reference

- **OpenAPI (v2)**: [docs/api-reference/v2/openapi.json](/docs/api-reference/v2/openapi.json)
- **Published docs**: [Cal.com API v2 Introduction](https://cal.com/docs/api-reference/v2/introduction)
- **Rate limits**: 120 requests/minute (API key); see [docs](https://cal.com/docs/api-reference/v2/introduction#rate-limits).
