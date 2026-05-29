## Problem
The receptionist’s time is tied up doing repetitive, less productive tasks rather than investing that time into bringing real value to the business.

## Solution
Implementing an automated booking system. Instead of the front desk staff handling the entire process, customers place bookings online. With just a quick human check to confirm, this reduces the time spent on a booking by the staff or mechanic from 15 minutes down to just 2 minutes.

## Workflow
1. **Customer Booking:** The customer accesses an online portal to select a date, time, vehicle, and service type.
2. **Webhook & Database Check:** Once details are entered, a webhook receives the booking and updates the database. The system immediately checks against existing bookings for any collisions or overlaps.
3. **Handling Overlaps:** 
   * *If there is an overlap:* The customer is sent a warning and prompted to choose a different time slot.
   * *If the slot is clear:* The mechanic is nudged to confirm the booking.
4. **Mechanic Confirmation:**
   * *If declined:* The customer is asked to choose an alternative time.
   * *If confirmed:* A confirmation email is sent to the customer, and the database is updated.
5. **System Updates:** The database update syncs a cron timer to match the booking time. All of this information is updated live on the internal dashboard.
6. **Day of Booking (No-Show Protection):** 
   * If a job hasn't started and the cron timer hits the 10-minute mark past the scheduled start time, the mechanic is nudged to check if they simply forgot to start the job on the system.
   * If they forgot, they start it right away. 
   * If the customer hasn't arrived, it is marked as a "no-show," the database is updated, and a canned absence email is automatically triggered to the customer.

## Fallbacks
* **Communication Failure:** The entire system defaults to standard email notifications if the primary communication method fails.
* **Database Failure:** All data will be cached locally to keep the workflow moving even if the database temporarily goes down.

## Potential Improvements
* **Live Calendar System:** Instead of checking the database for overlapping appointments *after* the customer selects a time, a live calendar system could be implemented upfront. Customers would only be able to select from currently available dates and times, immediately followed by an email asking them to wait for final confirmation from the mechanic.
