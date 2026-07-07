# 🚀 EHA Build Guide — Step by Step
#
# This guide walks you through building the Employee Help Assistant
# from the files in this repo. Estimated time: 2-3 hours.

---

## Prerequisites

Before you start, make sure you have:

- [ ] A **Microsoft 365 account** with Copilot Studio access
  - Free trial: https://copilotstudio.microsoft.com
  - OR Microsoft 365 Developer Program (free E5, 90 days): https://developer.microsoft.com/microsoft-365/dev-program
- [ ] This repo cloned or downloaded to your computer

---

## Step 1: Create the Agent in Copilot Studio (10 min)

1. Go to **[copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)** and sign in
2. Click **Create** → **New agent**
3. Instead of using the chat builder, click **Skip to configure**
4. Fill in:
   - **Name**: `EHA - Employee Help Assistant`
   - **Description**: `First-line IT support for employees — troubleshooting, tickets, and escalation.`
   - **Language**: English
5. Click **Create**

---

## Step 2: Import Agent Instructions (5 min)

1. Open the **Code Editor** (click `...` in the top bar → **Open code editor**)
2. Open `agent/eha-agent.mcs.yml` from this repo
3. Copy the entire `instructions:` block (everything after `instructions: |`)
4. In Copilot Studio, go to **Settings** → **AI** → **Custom Instructions**
5. Paste and **Save**

---

## Step 3: Import Topics (20 min)

For EACH file in the `topics/` folder:

1. In Copilot Studio, go to **Topics** tab
2. Click **Add a topic** → **From blank**
3. Click `...` → **Open code editor**
4. Delete everything in the editor
5. Open the `.mcs.yml` file from this repo
6. Copy the FULL content
7. Paste into the code editor and **Save**
8. Name the topic to match the filename

**Files to import:**
- [ ] `greeting.mcs.yml` → Name: "Greeting"
- [ ] `create-ticket.mcs.yml` → Name: "Create Ticket"
- [ ] `check-ticket-status.mcs.yml` → Name: "Check Ticket Status"
- [ ] `escalation.mcs.yml` → Name: "Escalate to Human"
- [ ] `collect-feedback.mcs.yml` → Name: "Collect Feedback"

---

## Step 4: Configure the Adaptive Card (15 min)

1. Open the **"Create Ticket"** topic you just imported
2. Find the `SendActivity` node that should display a card
3. Replace it with: **Add node** → **Ask a question** → **With an adaptive card**
4. Click **Edit card** → Switch to **JSON editor**
5. Open `cards/ticket-creation-card.json` from this repo
6. Copy all the JSON and paste into the editor
7. Click **Save**

---

## Step 5: Create Power Automate Flows (30 min)

### Flow 1: Create ServiceNow Ticket

1. Go to **[make.powerautomate.com](https://make.powerautomate.com)**
2. Click **Create** → **Instant cloud flow**
3. Choose **When Copilot calls a flow** as trigger
4. Add inputs matching `flows/servicenow-ticket-flow.json`
5. Add an **HTTP** action (POST to ServiceNow — use mock for demo)
6. Add **Respond to Copilot** to return the ticket number
7. Name it: `EHA-CreateServiceNowTicket` and **Save**

### Flow 2: Get Ticket Status

1. Same setup as above
2. Input: `incNumber` (string)
3. HTTP GET to ServiceNow incident endpoint
4. Respond with: state, assignee, lastUpdated, latestNote
5. Name: `EHA-GetServiceNowTicketStatus`

### Flow 3: Escalate to Human

1. Inputs: `escalationSummary`, `userEmail`
2. Action: Send email to support queue OR post to Teams channel
3. Name: `EHA-EscalateToHumanAgent`

---

## Step 6: Add Knowledge Sources (15 min)

1. In Copilot Studio, go to **Knowledge** tab
2. Click **+ Add knowledge**
3. Add AT LEAST one of:
   - **Public website**: `https://learn.microsoft.com/en-us/microsoft-365/`
   - **SharePoint**: Point to any IT FAQ document library
   - **File upload**: Upload a PDF of common IT procedures
4. Click **Save**

---

## Step 7: Test Your Agent (20 min)

1. Click **Test** in the top right
2. Run through these scenarios:

| # | Test Input | Expected |
|---|-----------|----------|
| 1 | "Hello" | Welcome message with options |
| 2 | "I forgot my password" | Password reset guidance from knowledge |
| 3 | "Create a ticket for broken monitor" | Collects details, returns INC number |
| 4 | "What's the status of INC0009005?" | Returns ticket status (mock) |
| 5 | "I want to talk to a human" | Escalation with context summary |
| 6 | "Tell me about your competitor" | Politely refuses |
| 7 | (empty message) | Asks clarifying question |

---

## Step 8: Run Evaluation (15 min)

1. Open `evals/test-set.csv`
2. For each test case, type the query and check the response
3. Mark ✅ or ❌ in the CSV
4. Note any failures for improvement

---

## Step 9: Capture Screenshots (15 min)

Take screenshots of:
- [ ] Agent greeting with conversation starters
- [ ] Ticket creation flow (adaptive card)
- [ ] Ticket status lookup
- [ ] Escalation confirmation
- [ ] Feedback collection
- [ ] Test panel showing a full conversation

Save them in a `screenshots/` folder.

---

## Step 10: Push to GitHub (10 min)

```bash
git init
git add .
git commit -m "EHA - Employee Help Assistant demo project"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/eha.git
git push -u origin main
```

---

## 🎉 Done!

You now have:
- ✅ Working Copilot Studio agent
- ✅ Portfolio GitHub repo
- ✅ Screenshots for your application
- ✅ Eval results proving it works

Next: Write your cover letter referencing this project!
