<img width="1810" height="521" alt="image" src="https://github.com/user-attachments/assets/015a8b8f-f654-4dba-931d-d7b810ee9495" />
Step by Step Flow
1️⃣ Vehicle Arrives (Entry)

Car shows up at the gate
Parking Service is the receptionist — it receives the car's request
Vehicle Types Service checks: "Is this a bike, car, or Ev?" — because different vehicles need different spot sizes

2️⃣ Finding a Spot

Spot Allocation Service is like a spot finder — it searches the Parking Spot Database for a free spot
Two outcomes:

✅ Spot Available → goes to Reservation Service
❌ Not Available → goes to Waitlist



3️⃣ Reservation

Reservation Service shows the user the nearest available spot
User picks their spot
This triggers Ticket Service which creates a parking ticket

4️⃣ Payment

Payment Service charges the user upfront
Two outcomes:

✅ Success → Spot is officially reserved, details saved to Vehicle Parking Details DB
❌ Failure → Spot gets released back to the pool



5️⃣ Spot Reserved

Spot Reserved confirms everything is locked in
Reservation Expiry Service starts a 30-minute countdown timer
If the user never arrives in 30 mins → spot is automatically released

6️⃣ Vehicle Exits

Vehicle scans at exit gate → triggers Exit Service
Exit Service publishes to Event Broker 
Event Broker broadcasts "Spot Released!"
Waitlist Service Trigger picks this up and notifies the next person waiting

7️⃣ Waitlist Flow (When No Spot Available)

Waitlist Service adds the user to a queue
Notification Service sends them an alert — "A spot is available!"
User gets 5 minutes to confirm
If confirmed → goes back to Spot Allocation to assign the spot
