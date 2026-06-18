<img width="2898" height="918" alt="image" src="https://github.com/user-attachments/assets/d3233701-7d60-4746-81ad-ef36df87a1c5" />


Step by Step Flow


Parking Lot System Flow

1️⃣ Vehicle Arrives and Requests Parking

User opens the parking application.

Vehicle information is submitted to the Parking Service.

Parking Service forwards the request to Vehicle Type Service.

Vehicle Type Service determines whether the vehicle is:

Car

Bike

EV

SUV

This helps identify the appropriate parking spot type.

2️⃣ Finding an Available Spot
Spot Allocation Service searches the Parking Spot Database for available spots matching the vehicle type.

Two possible outcomes:

Spot Available

Request moves to Reservation Service.

Spot Not Available

User is added to the Waitlist Service.

User waits until a spot becomes available.



3️⃣ Spot Reservation
Reservation Service retrieves all available spots.

User selects a preferred parking spot.

Reservation Service temporarily locks the selected spot to prevent double booking.


4️⃣ Payment Processing
Reservation Service sends the request to Payment Service.

Payment Success

Spot status changes to Reserved.

Reservation details are stored in the database.

Vehicle details are saved.

Parking ticket is generated.

Payment Failure

Reserved spot is released immediately.

Spot becomes available for other users.



5️⃣ Spot Reserved

Ticket Service generates a parking ticket containing:

Ticket ID

Spot Number

Reservation Time

Vehicle Details

Reservation Expiry Service starts a 30-minute countdown timer.

6️⃣ Reservation Expiry Service
User Arrives Within 30 Minutes

Vehicle reaches the parking lot.

User scans the ticket at Entry Scan.

Reservation Expiry Service stops the timer.

Vehicle status changes to Vehicle Parked.

User Does Not Arrive Within 30 Minutes

Reservation Expiry Service automatically releases the spot.

Spot status becomes Available.

Spot Released Event is published.

Waitlist Service Trigger picks this up and notifies the next person waiting.



8️⃣ Parking Duration Reminder
While the vehicle is parked, Notification Service monitors the parking duration.

A reminder notification is sent to the user 10 minutes before the booked parking duration ends.
Example:

Your parking session will expire in 10 minutes.

This allows the user to extend the booking or prepare to exit.

9️⃣ Vehicle Exit
User scans the ticket at Exit Scan.

Exit Scan validates the ticket and completes the parking session.

The occupied spot is released and marked Available.

A Spot Released Event is published to the Event Broker.

 Notify Waiting Users
 
Waitlist Service subscribes to Spot Released Events.

When a spot becomes available:

Waitlist Service selects the next eligible user.

Notification Service sends:

A parking spot is now available. Please confirm within 5 minutes.

If the user confirms, the flow returns to Spot Allocation Service.

Otherwise, the next user in the queue is notified.
