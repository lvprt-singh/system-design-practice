# Booking System Specification Sheet

## Overview
The Booking System is designed to help customers make workshop bookings online, drastically reducing the time required by administrative staff and mechanics to assist customers with their appointments.

## Core Requirements (Day One MVP)
To successfully assist mechanics and drastically reduce manual input, the initial system requires:
* **Customer Access:** Customers must be able to access the booking system and provide job details.
* **Mechanic Notifications:** The system must successfully send a notification to the mechanic.
* **Critical Infrastructure:**
  * **Customer-facing online portal** for appointment entry.
  * **Webhook** to receive incoming customer information.
  * **Database** to update and verify appointments against mechanic availability.
  * **Email system** to handle communication between the customer and the mechanic.

## Additional Features
* **Arrival / Absence Check:** A feature to verify if the customer has arrived for their scheduled job. This utilizes existing infrastructure via a simple condition check, triggering an extra nudge to the mechanic for final confirmation, further alleviating admin stress.

## Fallbacks & Outage Handling
All components have assigned backups to ensure the system remains operational during an outage:
* **Entire Booking System** ➔ Falls back to manual **email communication**.
* **Absence / Arrival Check** ➔ Falls back to a **manual check** by the mechanics.

## Pre-Implementation Verifications
Due to the complex nature of the system, strict manual checks and verifications must be completed prior to implementation:
* **Database Real-Time Sync:** Verify the database can successfully read/write and display output to the mechanic-facing dashboard in real-time (failure here defeats the system's core purpose).
* **Local Caching (Offline Mode):** Cached data for the dashboard must support local read and write operations. If the database or dashboard goes offline, edits made by mechanics locally must be queued and synced to the database once the connection is restored.
* **Notification Visibility (Windows):** If the dashboard runs on Windows, notifications must be forced to **High Priority**, as standard Windows notifications are easily missed.
* **Cross-Platform Notifications:** Ensure nudges and alerts work reliably across various mobile operating systems (Android, iOS) used by mechanics.
* **Scope Constraint:** The system explicitly **will not** handle or process payments.

## Validation Order
| Component Failure | System Impact |
| :--- | :--- |
| **Customer-facing dashboard** | System **degrades** ➔ Falls back to email |
| **Database** | System **degrades** ➔ Falls back to cached data |
| **Canned email absence** | System **breaks** ➔ Reverts to manual checks |
