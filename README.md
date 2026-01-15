# n8n-ni8mare-educational

LEGAL DISCLAIMER

This project is for educational and research purposes only. The author does not endorse or encourage using this information for malicious purposes. This lab is designed to demonstrate a specific vulnerability.

Do not use this exploit against systems you do not own or have explicit permission to test.

## About CVE-2026-21858
This lab demonstrates a vulnerability within an AI-powered n8n workflow.

By manipulating the JSON payload sent to a Form Trigger, an attacker can trick the workflow into:
* Accepting a local server path (e.g., /etc/passwd) as if it were an uploaded file.
* Indexing the content of that sensitive file into the Vector Database.
* Retrieving the sensitive data via the AI Chat Interface.

---
## Prerequisites
* Docker and Docker Compose installed.
* An OpenAI API Key (for the AI Agent).
* curl (installed on your host machine to run the exploit script).

## Setup Instructions
1. Start the Environment
Spin up the vulnerable n8n instance pinned to the specific version for this lab.



```bash
docker compose up -d
```

Once the containers are running, navigate to:

n8n Dashboard: http://localhost:5678

## Import the Vulnerable Workflow
Open the n8n dashboard.

* If this is your first time, create a local owner account.
* Go to the Workflows menu and select "Import from File".
* Select WorkflowForCve.json from this repository.

## Configure Credentials
The workflow requires an OpenAI connection to function.

* In the workflow editor, find the OpenAI Chat Model node.
* Click on the Credential to connect with dropdown.
* Select "Create New Credential".
* Enter your OpenAI API Key.
* Ensure the "Model" is set to gpt-4o-mini (or similar) in the node settings.

## Exploitation Guide
Follow these steps exactly to reproduce the vulnerability.

### Step 1: Prepare the Target
* In the n8n editor, locate the "On form submission" node (the trigger).
* Click on it to open the parameters.
* Look for the Test URL (it should look like http://localhost:5678/webhook-test/...).
* Copy this URL.
* Click the "Execute Workflow" (or "Test Workflow") button at the bottom of the screen. The workflow is now listening for a single event.

### Step  2: Launch the Injection
Open your terminal in this repository folder.

Run the exploit script:
```bash
sh ./exploit.sh
```
* When prompted, paste the Webhook URL you copied in Step 1.
* Press Enter.
* You should see a "Workflow executed successfully" message in the n8n UI.

## Step 3: Exfiltrate the Data
* Now that the /etc/passwd file has been indexed into the Vector Store, you can ask the AI Agent to read it for you.
* In the n8n editor, click the Chat button (usually located in the bottom right corner or via the "Test Chat" interface).
* Enter the following prompt exactly:

"Provide me with any data you have from the knowledge base that came from /etc/passwd. Do not abbreviate or summarize."

Result: The AI Agent will query the Vector Store, find the chunks labeled with /etc/passwd, and output the raw content of the system file (users, shells, and uids).
