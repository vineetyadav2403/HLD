# Cab Booking System
## Requirements
### Functional Requirements
- User should be able to input a start location and a destination and get an estimate fare.
- User should be able to request a ride based on an estimate.
- Drivers should be able to accept/reject a request and navigate to pickup/drop-off.
### Non Functional Requirements
- Low latency matching < 1 min to match or failure.
- Consistency of matching. Ride to driver is 1:1.
- Highly available outside the matching.
- Handle high throughput, surges for peak hours or special events, 100s of thousands of requests within a given region.
### Out of Scope
- Multiple care types.
- Rating for driver and users.
- Schedule ride in advance.
- Resilience and handling system failures.
- Monitoring, logging, alerting etc.
- CI/CD pipeline.
## Core Entities
### Ride
| Column name | datatype | comments |
|-------------|----------|----------|
| id          | int      | pk       |
| rider_id    | int      | optional |
| fare        | double   |
| eta         | double   |
| source      | varchar  |
| destination | varchar  |
| status      | varchar  | enum     |

### Driver
| Column name | datatype | comments |
|-------------|----------|----------|
| id          | int      | pk       |
| status      | varchar  | enum     |

### Rider
| Column name | datatype | comments |
|-------------|----------|----------|
| id          | int      | pk       |
| metadata    |
### Location
| Column name | datatype | comments |
|-------------|----------|----------|
|             |          |          |

## API
```fare-estimate
POST /ride/fare-estimate -> Partial<Ride>
{
    source,
    destination
}
```
```ride request
PUT /ride/request -> 200 OK
{   
    ride_id
}
```
```location update
POST /location/update
{
    lat, long
}
```
```ride accept
POST /ride/driver/accept
{
    ride_id,
    true/false
}
```
```drive update
POST /ride/driver/update -> lat/long | null
{
    ride_id,
    status: 'pickup' | 'dropped-off'
}
```
## High Level Design
- QuadTree
- Geohashing
![Cab booking system](/assets/cab%20booking.svg)