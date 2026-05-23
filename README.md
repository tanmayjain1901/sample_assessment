# Event Management System API

## Introduction
You have been tasked with building a backend API for an event management platform. The platform allows users to create events, view event details, register attendees, and explore upcoming events with various filtering and sorting options.

## Problem Statement
1. Make all tests pass by implementing the missing features.
2. Write your code in these files: `event_manager/event_manager.py` and `event_manager/schemas.py`.
3. The functions have HTTP method decorators but lack implementation. You may remove or modify the default code.
4. Use an in-memory dictionary called `events` to store event records (no external database).

## Task List

### Task 1
**Implement the `list_events` path operation function.**

It should return a list of all events in the following format:
```json
[
  {
    "id": 1,
    "title": "Python Conference 2026",
    "description": "Annual Python conference",
    "category": "Conference",
    "date": "2026-06-15",
    "location": "San Francisco",
    "capacity": 500,
    "registered_count": 125,
    "status": "open"
  },
  ...
]
```

### Task 2
**Configure path routes for `event_details`, `update_event`, and `delete_event` path operation functions.**

The route decorators are commented out. Uncomment and configure them according to these requirements:
- Each function should accept the event ID as a path parameter
- The routes should accept a trailing slash, e.g., both `/1` and `/1/` are valid

### Task 3
**Implement the `event_details` path operation function.**

It should return details of a single event specified by ID. The response format should match the list format above.

If the event does not exist, return a 404 status code with the response: `{"detail": "Not Found"}`.

### Task 4
**Implement the `delete_event` path operation function.**

It should remove an event with the specified ID and respond with a 204 status code (no content).

If the event does not exist, return a 404 status code with: `{"detail": "Not Found"}`.

### Task 5
**Implement the `create_event` path operation function.**

This function should create a new event. Requirements:
- Automatically assign an ID starting from 1
- Accept only these fields: `title`, `description`, `category`, `date`, `location`, `capacity`
- Validate that `capacity` is a positive integer (1-10000)
- Return status code 201 on success
- Return validation errors with 422 status code on invalid data
- `registered_count` should default to 0 and `status` should default to "open"

### Task 6
**Implement the `update_event` path operation function.**

This function should update an existing event with a PUT request. Requirements:
- Allow partial updates (user can send one or more properties)
- Prevent ID changes
- Return 200 status code on success
- Return 404 with `{"detail": "Not Found"}` if the event doesn't exist
- Return 422 with validation errors if data is invalid or no fields are provided

### Task 7
**Implement the `register_attendee` path operation function.**

This function should register an attendee for an event. Requirements:
- Accept event ID as path parameter
- Accept `name` and `email` fields in the request body
- Return 201 status code on successful registration
- Return 400 with `{"detail": "Event is at full capacity"}` if capacity is reached
- Return 404 with `{"detail": "Not Found"}` if the event doesn't exist
- Registration should increment `registered_count` but not permanently store attendee records

### Task 8
**Make the `list_events` function filterable, sortable, and pageable.**

Add query parameters for:
- **Filtering**: 
  - `category` - filter by event category (exact match)
  - `status` - filter by event status: "open" or "closed"
- **Sorting**: 
  - `sort_by` - sort by: "id", "title", "date", or "capacity" (ascending order only)
- **Pagination**: 
  - `page` - page number (default: 1), with page size of 10 records
  - Return an empty list if the requested page doesn't exist

Users should be able to combine filters and sorting, e.g., `/?category=Conference&status=open&sort_by=date&page=2`.

## Hints
1. Do not modify code outside of `event_manager/` and `tests/` directories.
2. You can import dependencies from FastAPI and Python's standard library.
3. Consider using Pydantic's `Field` for validation constraints.
4. The `HTTPException` from FastAPI is helpful for returning error responses with specific status codes.
