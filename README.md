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
