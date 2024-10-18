# Ticket Booking app

## Requirements
### Functional Requirements
- Book tickets
- View Events
- Search for events

### Non-functional Requirements
- Low latency search
- Strong consistency for booking tickets & high availability for search and view events.
- Read >> Write
- scalability to handle surges form popular events.

### Out of scope
- GDPR compliance
- Fault tolerance

## Core Entities
### Event 
| Column name  | datatype | Comment          |
|--------------|----------|------------------|
| id           | int      | PK               |
| venue_id     | int      | ref venue.id     |
| performer_id | int      | ref performer.id |
| tickets      | list     |
| name         | varchar  |
| description  | varchar  |
### Venue 
| Column name | datatype | Comment |
|-------------|----------|---------|
| id          | int      | PK      |
| location    | varchar  |
| seat_map    | varchar  |
### Performer
| Column name | datatype | Comment |
|-------------|----------|---------|
| id          | int      | PK      |
### Ticket
| Column name | datatype  | Comment                      |
|-------------|-----------|------------------------------|
| id          | int       | pk                           |
| event_id    | int       | ref event.id                 |
| seat        | int       |                              |
| price       | double    |                              |
| status      | varchar   | available, reserved, booked  |
| reserved    | timestamp |                              |
| user_id     | varchar   |



## API

``` View event
GET /event/:eventId 
-> Event & Venue & Performer & Ticket[]
```
``` Search Event
GET /search?term={term} & location={location} & type={type} & date={date} 
-> Partial<Event>[]
```

``` Reserve ticket
POST /booking/reserve
header: Auth
body: {
    ticketId
}
```
``` Confirm ticket
POST /booking/confirm
header: Auth
body: {
    ticketId,
    paymentDetails
}
```
## High Level Design
![Ticket Booking System](/assets/ticket-booking.svg)