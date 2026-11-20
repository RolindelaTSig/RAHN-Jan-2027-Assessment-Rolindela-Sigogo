FitDesk Enquiry Assistant (RAHN Jan 2027 assessment)

Hours actually spent: 4 hours

A command-line assistant for a Hermanus fitness studio. It reads a customer enquiry, asks Gemini to turn it into structured JSON, looks up the approved prices and Saturday slots in a local data file, drafts a reply from that data only, and waits for staff to approve it before anything is recorded as sent. It never takes bookings or payments, and never invents prices or availability.

How it works
You type a customer message.
Gemini returns JSON (intent, details, requires_human_approval), checked against a Pydantic model.
The reply is built from data/fitdesk_data.json only. Anything not in the data gets the exact phrase: I'll check with the owner and come back to you.
The draft is printed and you answer Send? (y/n).
The outcome is appended to status_log.json. sent is recorded only if the AI output was valid and staff typed y.
Setup
Install Python 3.10 or newer.
Open a terminal in the project folder.
Create a virtual environment:
Mac/Linux: python -m venv .venv then source .venv/bin/activate
Windows: python -m venv .venv then .venv\Scripts\activate
Install the dependencies: pip install -r requirements.txt
Get a free key from https://aistudio.google.com/ .
Copy .env.example to .env and paste your key after GEMINI_API_KEY=. Optionally set GEMINI_MODEL (I used Gemini 3.5 Flash-Lite). Never commit .env.
Run
python main.py

Type a message, for example: How much is the 12-session pack and do you have Saturday slots? Read the draft, then answer y or n. Results are appended to status_log.json (created on first run).

Test (no API key needed)
pytest

Gemini is mocked, so the tests make no API calls. Expected result: 8 passed. Test descriptions are in docs/REGRESSION_TESTS.md.

Project layout
main.py: command-line program and the y/n prompt
fitdesk/: models.py (Pydantic model), llm.py (Gemini call), drafting.py (reply from data), agent.py (pipeline and safety rules), status_log.py (log file)
data/fitdesk_data.json: sample prices and Saturday slots
tests/test_agent.py: automated regression tests
docs/: SRS.md, REGRESSION_TESTS.md, RAPLH_LOG.md
AI use
Claude (chat): wrote the first drafts of docs/SRS.md, the code, the tests, docs/REGRESSION_TESTS.md and docs/RAPLH_LOG.md. I read and ran the code myself and corrected it (see docs/RAPLH_LOG.md).
GitHub Copilot Chat: helped me install the dependencies.
Gemini API: used inside the program only to classify the customer message into JSON. It never writes prices or availability.
The Working style answers below were written by me without AI.
What doesn't work yet
Package matching is exact. If Gemini returns "12 sessions" instead of "12", the reply escalates instead of giving the price (safe, but unhelpful).
Only Saturday slots are supported. Questions about other days, discounts or bookings escalate to the owner.
status_log.json is not safe if two people run the program at once.
sent only means staff approved the text. Nothing is delivered to the customer; staff send it themselves.
The live Gemini call is only checked by manual runs. The automated tests use a fake Gemini reply.
Behaviour depends on the model following the prompt, so a different model may classify some messages differently.
Working style

1. It is 22:00 and you have been stuck on the same error for three hours. What do you do next, step by step?

i usually decide to take a step back for roughly 30 minutes to an hour. i will comeback and break down the issue that im having step by step. then ask for Ai to help me debug and correct the smallest parts of the code then i restructure it and put it back together.

2. It is Wednesday. Your teammate has not replied since Monday and you present together on Friday. What do you do?

i would try to contact him one last time. if im still unable to get a hold of him i would then start with his part of the project and notify the organization of the challenge and the issue that i would be facing. i would likey ask for a little more time and then present the work without him

3. Your mentor tells you the approach you spent two days on is wrong. How do you react, and what do you do next?

i would obiviously be upset but i would go back home and review where i could have possibly gone wrong and reflect on my error and then ask for assistance from my mentor aswell as ask him which approach would make more sense without limiting too much of my creativety whilst benefitting the company

4. From January to March 2027 you must attend sessions every Monday, Wednesday and Friday. What in your life could get in the way, and what is your plan for each thing?

the only thiing that could get in the way would be me trying to complete my honours studies but how i would work around this is trying to attend the classes during my lunch breaks or after work
