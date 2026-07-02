# Event-Driven-Notification-Dispatcher
Event-Driven Notification Dispatcher
Project Overview
This project is a lightweight asynchronous notification system built using Node.js, Express.js, and SQLite.

When a business event such as order_placed is received:

The event is stored in the SQLite database.
A notification record is created with pending status.
The notification is pushed into an in-memory queue.
The API immediately returns 202 Accepted without waiting.
A background worker processes the notification asynchronously.
The notification status is updated to completed or failed.
Tech Stack
Node.js
Express.js
SQLite
sqlite3
JavaScript
Project Structure
project-root/

src/
│
├── app.js
├── server.js
├── controllers/
│      eventController.js
├── services/
│      eventService.js
│      notificationService.js
│      queueWorker.js
├── routes/
│      eventRoutes.js
└── db/
       database.js
       schema.sql

README.md
package.json
.env.example
architecture-diagram.png
Installation
Clone the repository

git clone <repository-url>
Install dependencies

npm install
Running the Project
node src/server.js
Server runs on

http://localhost:3000
Database
SQLite database is automatically created when the application starts.

Tables:

events
notifications
API Endpoint
POST /api/v1/events
Request
{
    "event_type":"order_placed",
    "recipient":"user@example.com",
    "data":{
        "order_id":101
    }
}
Response
{
    "message":"Event accepted for processing",
    "tracking_id":1,
    "notification_id":1,
    "status":"pending"
}
HTTP Status

202 Accepted
Queue Processing
The application uses an in-memory JavaScript queue.

Flow:

Notification added to queue
Background worker picks notification
Waits random 500–1000 ms
Simulates notification sending
10% failure probability
Updates SQLite database
Error Handling
400 Bad Request

{
    "error":"event_type and recipient are required"
}
500 Internal Server Error

{
    "error":"Internal server error"
}
Assumptions
Default notification channel is email.
Queue is stored in memory.
Queue resets if server restarts.
Limitations
No authentication
No persistent queue
No external messaging service
No automatic retries beyond retry_count increment
