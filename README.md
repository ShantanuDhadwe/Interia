

# Interia – AI Voice Assistant Workflows (n8n)

This repository contains automated workflows for **Interia**, a luxury interior design firm. The automation is built using [n8n](https://n8n.io) and integrates with [Vapi.ai](https://vapi.ai) and Airtable to automate outbound voice calls, qualify leads, and log call results.

**Live Repository:** [github.com/ShantanuDhadwe/Interia](https://github.com/ShantanuDhadwe/Interia

**Youtube Link:** [Youtube](https://youtu.be/KmNxSTa_690?si=erIuIZ4RK276pOnN)
---

## 📦 Overview

The system is powered by a conversational AI assistant called **Maya**, who:

- Calls leads from Airtable  
- Asks a series of qualifying questions  
- Updates lead statuses based on the outcome  
- Schedules consultations if qualified  
- Handles retries and failure gracefully  

---

## 🧩 Workflow 1: Outbound Calling Process

**File:** `Outbound_calling_Process.json`

This workflow fetches leads marked as `TBC` (To Be Called) from Airtable and initiates an outbound voice call through Vapi.ai.

### Flow Summary

- **Trigger:** Runs on a schedule (e.g., every few minutes)  
- **Airtable Search:** Fetches leads with `Status = 'TBC'`  
- **Set Fields:** Prepares fields like First Name, Mobile, ID  
- **Vapi API Call:** Sends POST request to initiate call  
- **Airtable Update:** Changes lead status to `In-Progress`  

### Airtable Table: `Lead Record`

- Fields used: `id`, `First Name`, `Mobile`, `Status`  
- Status values: `TBC`, `In-Progress`, `Called`, `Failed`  
- Tracks: Attempts, Summary, Assignee, Date/Time  

---

## 🧩 Workflow 2: Call Result Processing

**File:** `Call_Result_Processing.json`

This workflow handles POST webhooks from Vapi.ai and updates Airtable with call data, retry logic, or summaries.

### Flow Summary

- **Webhook:** Listens for Vapi `end-of-call-report` or `status-update`  
- **Call Record Save:** Logs full call data and cost breakdowns  
- **Check If Answered:** Filters out failed or missed calls  
- **Update Airtable:**  
  - If success: Sets `Status = Called`, `Attempt = #Success`, adds summary  
  - If failed once: Sets `Attempt = #1` and retries  
  - If failed twice: Marks as `Unreachable` or `Failed`  

### Airtable Tables

- `Lead Record`: Tracks lead lifecycle and status  
- `Call Records`: Stores detailed call metadata and transcripts  

---

## 🧠 Voice Assistant Logic – Maya

**File:** `System Prompt.txt`

Maya is a warm, high-end voice assistant representing Interia. Here's how she handles each call:

### Conversation Flow

1. **Greeting:**  
   “Hi, this is Maya from Interia. I’m calling regarding your recent interest in our interior design services. Is now a good time to chat for a couple of minutes?”

2. **Qualifying Questions:**  
   - Project scope: “What kind of space are you looking to renovate or design?”  
   - Budget: “Our services start at ₹30 lakhs. Does that fit your budget?”  
   - Property size, location, and timeline  
   - Service type: End-to-end or partial help?  

3. **Consultation Booking:**  
   If qualified, Maya checks calendar availability and books a call with a senior designer.

4. **If Not Qualified:**  
   Politely ends the call

5. **Closing Lines:**  
   “Thanks for your time today! We look forward to transforming your space with Interia.”

---

## 🔒 Business Rules

- Only continue if **budget ≥ ₹30L**  
- Ask **one question at a time**  
- If the lead is not ready, schedule a callback  
- End gracefully if unqualified  

---

## ⚙️ Tech Stack

- **n8n** – Workflow automation platform  
- **Vapi.ai** – Voice AI API  
- **Airtable** – Lead database and call logs  
- **Webhooks** – For real-time call result handling  
- **Custom JSON logic** – For dynamic field updates and API calls  

---

## 🚀 Setup Instructions

1. **Clone this repo**  
   `git clone https://github.com/ShantanuDhadwe/Interia.git`

2. **Import workflows into n8n**  
   - Open n8n Editor UI  
   - Import `Outbound_calling_Process.json` and `Call_Result_Processing.json`

3. **Create Airtable tables**  
   - Create `Lead Record` and `Call Records` tables matching the field names used in the workflows

4. **Set up credentials**  
   - Airtable API Token (for all Airtable nodes)  
   - Vapi API Key (for HTTP request nodes)

5. **Test**  
   - Add a sample lead with `Status = TBC` to Airtable  
   - Run the workflow manually or wait for scheduled trigger  

---

## 📝 Best Practices

- Run only during **business hours** (10AM–7PM IST, Mon–Sat)  
- Keep Airtable schema consistent  
- Use Vapi test mode for testing calls  
- Review transcripts and summaries to improve Maya's prompt  

---

## 🙋 About

**Author:** [Shantanu Dhadwe](https://github.com/ShantanuDhadwe)

---

## 📄 License

This project is open source and available under the MIT License.

---

✅ You can now copy and paste this entire content directly into your `README.md` file with zero formatting issues. Let me know if you want badges, a diagram, or contribution guidelines added!
