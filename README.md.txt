🚀 Automated Firmographic Lead Scorer (Python + Latenode/No-Code)
This repository contains the Python custom code used to automate B2B lead scoring based on firmographic data (Employee Count, Industry, Location). This solution is designed to run seamlessly within a modern automation platform like Latenode, eliminating manual qualification and ensuring high-intent leads are prioritized instantly.
🎯 The Business Problem Solved
Traditional manual lead scoring is slow and inconsistent, leading to delayed follow-up on high-value leads. This script solves that by:
1. Reducing qualification time from hours to seconds.
2. Applying objective scoring logic to prioritize high-revenue potential accounts.
3. Eliminating human error in the scoring and routing process.
⚙️ How the Workflow Functions
The Python script (lead_scorer.py) serves as the core logic block in a larger automation workflow:
1. Trigger: New lead submits a form (e.g., via HubSpot).
2. Data Pass: The lead's domain name is passed to the Latenode custom code node.
3. Execution (This Script): The score_lead(domain) function executes:
   * It fetches company data via the Clearbit API.
   * It calculates a weighted score (0-100) based on size, industry, and location.
4. Action: The resulting score is written back to the CRM, instantly triggering a task assignment or a high-priority Slack notification for the Sales team.
💻 Technical Setup
Prerequisites
* Python 3.x
* requests library (pip install requests)
* A Clearbit API Key
Configuration
The script attempts to load the API key from an environment variable for security:
CLEARBIT_API_KEY = os.environ.get('CLEARBIT_API_KEY', 'YOUR_API_KEY_PLACEHOLDER')

For Latenode Integration: You would set the CLEARBIT_API_KEY as a secure secret/environment variable within the Latenode execution environment.
⭐ Key Logic Highlights
The scoring is tiered to maximize priority for large, strategic accounts:
Criterion
	Points
	Logic in lead_scorer.py
	Employee Count
	40 or 30
	High-value threshold (>500 or >100 employees)
	Target Industry
	30
	Matches strategic industries (Software, FinTech, B2B Services)
	Geo-Targeting
	30
	Matches target markets (United States, Canada)
	This simple structure allows for rapid customization to match any specific Ideal Customer Profile (ICP).