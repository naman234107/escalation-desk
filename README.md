# Escalation Desk (v0)

An internal tool for L1 support agents at RideOps, built for a case study. It sits at the moment an agent decides whether to escalate a ticket to engineering. For each ticket it shows:

- what happened to similar tickets
- whether engineering may already be working on the problem

Escalate stays locked until a short checklist and the details engineering needs are done.

It runs on the supplied ticket export (`tickets.csv`, 1,444 tickets from 2 Jul to 30 Sep 2026). There is no server, database or AI; everything is computed in the browser.

**Live app:** https://naman234107.github.io/escalation-desk/

Author: Naman Chugh, Roll no. 2023UIC4153

## Run it locally

The page loads `tickets.csv` from its own folder, so serve the folder rather than double-clicking the file:

```bash
git clone https://github.com/naman234107/escalation-desk.git
cd escalation-desk
python3 -m http.server 8000
# open http://localhost:8000
```

Any static server works. To use newer data, click **Load newer export** and pick a CSV with the same columns. The file stays in the browser.

## Try this first (2 minutes)

1. In **Working as**, pick **Rahul T**. He escalates 62% of his tickets, against 34% expected for his mix.
2. Open the first ticket, which engineering sent back for missing info. You'll see:
   - the recommendation
   - a likely known issue, with its ENG ticket IDs
   - similar-ticket numbers
   - the checklist, with **Escalate** locked in the bottom bar
3. Tick the checks and fill the fields until the meter is full, then press **Re-escalate** to see the packet it writes for engineering.
4. Open **My stats** and switch between **Rahul T** and **Karan B**. They show the two opposite problems: escalating too much, and closing too fast so tickets come back.

## Data format

The CSV needs these columns (extra columns are ignored):

`ticket_id, created_at, channel, client_name, client_tier, category, priority, description, assigned_agent, first_response_at, escalated_to_eng, eng_ticket_id, eng_handoffs, eng_outcome, status, first_closed_at, final_closed_at, reopen_count, csat_score`

Timestamps look like `YYYY-MM-DD HH:MM`.

## Limits

- **No write-back.** It doesn't write to a ticketing system. Actions last for the session, and the text is copied into the real ticket by hand.
- **Draft checklists.** The checklists, in `PLAYBOOK` in `index.html`, are drafts built from engineering's verdicts in the data, to be agreed with engineering.
- **Templated descriptions.** Ticket descriptions in this data come from 48 templates. Real client text will need fuzzier matching.
