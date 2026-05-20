# Automated AI Quoting System

## Problem
Customers are asking for quotes from the mechanic, but not all of them convert. This occupies the mechanics' work time and reduces overall productivity.

## Solution
Using AI to automate the quoting system and only nudging the mechanic to approve the quotes before sending them out to the customer.

## Workflow
1. **Initial Request**: A customer sends a WhatsApp message including the make, model, and service required.
2. **Webhook & AI Parsing**: The message goes to the webhook on the server, which in turn sends that message to the AI.
3. **Information Validation**: The AI investigates the message and checks for any incomplete information.
   * *Missing Info*: If there is any incomplete information or extra details required, a message is sent back to the customer asking for the required information.
4. **VicRoads Verification**: Once all the information is consolidated, a payload including the car rego is sent to VicRoads to verify if the information collected by the AI is accurate.
5. **Database Check**: After verification from VicRoads, the accurate car information is checked against old car entries present in the database.
6. **Customer Confirmation**: Following the database verification, a follow-up message is sent to the customer confirming the car details.
7. **Quote Generation**: Once confirmed by the customer, the information is sent back to the AI (which has access to the database of parts, service lists, and their pricing). The AI creates a quote payload.
8. **Mechanic Approval**: The AI sends the quote payload to the mechanic to confirm.
9. **Final Delivery**: Once confirmed by the mechanic, the final quote is sent to the customer.

## Fallbacks
Every step has a fallback mechanism in case of failure:
* **WhatsApp** ➔ Falls back to **Email**
* **Server** ➔ Falls back to **Mechanic Takeover**
* **AI (Car & Service Recognition)** ➔ Falls back to **Webhook Handler**
* **VicRoads / Database** ➔ Falls back to **Cached Data**
* **AI (Creating Quote Payload)** ➔ Falls back to **Mechanic Nudged to Create Quote**

## Potential Improvements
* **Proactive Service Suggestions**: When a customer's message does not include specific service details, the AI (after verifying car details for accuracy) will check the database to see if they are a returning customer. If they are, the AI checks the last work done on the car and follows up with the next logical service or work that should be carried out.
* **Spam & Noise Reduction**: To filter out unserious inquiries or bargain hunters, a mandatory form could be sent to all new customers. This form bypasses the first AI system directly to the second AI while updating the database. Most unserious customers will not bother filling out the form, effectively filtering the noise.
