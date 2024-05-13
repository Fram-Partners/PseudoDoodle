# PseudoDoodle Application

(Borrowed and modified, with love, from https://gist.github.com/anttti/2b69aebc63687ebf05ec)

PseudoDoodle is an application to help scheduling events with friends, quite like http://doodle.com/ but in a much simplified way. An event is created by posting a name and suitable dates to the backend, events can be queried from the backend and participants can submit dates suitable for them.

The only requirements: the application needs to persist data between launches. The backend must be written in C#, and the frontend must be written in Vue. You can choose the architectural design and patterns freely.

The deliverable should be a publicly accessible repository containing the application and a README file containing at least a guide on how to run the application.

Note: in the following example the IDs are `long`s that start from 0. That's not a strict requirement. The identifier can be whatever you want to use, such as GUID/UUIDs.

Some guidelines:

- Your solution should be able to scale for a larger purpose. Imagine the app being extended to cover a larger API and/or a bigger feature set.
- Showcase your abilities, and use the task to demonstrate your idea of best practices, regarding coding style, project structure, frameworks, patterns, design, testing etc.
- There is no right or wrong solution, as long as it is your solution.
- Be prepared to describe your work in detail.

***

# Frontend
Should be written in Vue. There should be UI for the following actions:
- Creating a new event
- Viewing a list of all events
- Viewing an existing event's details
- Viewing the results of an event (e.g. only the dates that are acceptable for all attendees).
- Adding votes for an existing event.

These don't all need to be separate pages--several of these functions can exist on the same page.

# Backend
To be written in C#, API design below.

## List all events
Endpoint: `/api/v1/event/list`

### Request
Method: `GET`

### Response
Body:

```
{
  "events": [
    {
      "id": 0,
      "name": "Jake's secret party"
    },
    {
      "id": 1,
      "name": "Bowling night"
    },
    {
      "id": 2,
      "name": "Tabletop gaming"
    }
  ]
}
```

## Create an event
Endpoint: `/api/v1/event`

### Request
Method: `POST`

Body:

```
{
  "name": "Jake's secret party",
  "dates": [
    "2014-01-01",
    "2014-01-05",
    "2014-01-12"
  ]
}
```

### Response
Body:

```
{
  "id": 0
}
```

## Show an event
Endpoint: `/api/v1/event/{id}`

### Request
Method: `GET`

Parameters: `id: long`

### Response
Body:

```
{
  "id": 0,
  "name": "Jake's secret party",
  "dates": [
    "2014-01-01",
    "2014-01-05",
    "2014-01-12"
  ],
  "votes": [
    {
      "date": "2014-01-01",
      "people": [
        "John",
        "Julia",
        "Paul",
        "Daisy"
      ]
    }
  ]
}
```

## Add votes to an event
Endpoint: `/api/v1/event/{id}/vote`

### Request
Method: `POST`

Parameters: `id: long`

Body:

```
{
  "name": "Dick",
  "votes": [
    "2014-01-01",
    "2014-01-05"
  ]
}
```

### Response

```
{
  "id": 0,
  "name": "Jake's secret party",
  "dates": [
    "2014-01-01",
    "2014-01-05",
    "2014-01-12"
  ],
  "votes": [
    {
      "date": "2014-01-01",
      "people": [
        "John",
        "Julia",
        "Paul",
        "Daisy",
        "Dick"
      ]
    },
    {
      "date": "2014-01-05",
      "people": [
        "Dick"
      ]
    }
  ]
}
```

## Show the results of an event
Endpoint: `/api/v1/event/{id}/results`
Responds with dates that are **suitable for all participants**.

### Request
Method: `GET`

Parameters: `id: long`

### Response

```
{
  "id": 0,
  "name": "Jake's secret party",
  "suitableDates": [
    {
      "date": "2014-01-01",
      "people": [
        "John",
        "Julia",
        "Paul",
        "Daisy",
        "Dick"
      ]
    }
  ]
}
```
