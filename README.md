Overview:
This n8n workflow automates a 109‑day LinkedIn content schedule. It pulls project data from Google Sheets, creates AI‑generated posts, uploads screenshots, schedules via Buffer, and emails status updates.

How It Works:
1. Trigger – Runs every Monday and Friday at 9 AM.
2. Fetch – Reads all rows from the LinkedIn Automation sheet.
3. Completion Check – If every row is marked "Completed", sends an alert email and stops.
4. Process – Otherwise, pulls the first row with Status = Enabled.
5. Extract Fields – Pulls project info, screenshots, complexity, etc.
6. Image Handling – Uploads screenshots to ImgBB (if present).
7. Image Analysis – Uses Gemini to produce short summaries of each screenshot.
8. AI Agent – Combines project data, image summaries, and complexity to craft a LinkedIn post following 2026 algorithm best practices.
9. Post Cleaning – Strips markdown, leaving plain‑text ready for LinkedIn.
10. Buffer – Sends the post (with image URLs) to Buffer for scheduling.
11. Notification – Sends a success email with post details.
12. Update Sheet – Marks the processed row as "Completed" in Google Sheets.

Nodes & Tools Used:
| Node | Purpose |
|------|---------|
| Schedule Trigger | Starts workflow on Mon/Fri 9 AM |
| Google Sheets (Get All Sheet Rows) | Reads the master sheet |
| Code (Check All Completed) | Determines if all rows are done |
| IF (All Rows Completed?) | Branches to alert or processing |
| Gmail (Sheet Completed Alert) | Notifies when sheet is finished |
| Google Sheets (Get Rows For Processing) | Pulls rows for action |
| Filter (Status Enabled Only) | Keeps only enabled rows |
| Limit (1 Entry Only) | Processes one row per run |
| Set (Extract Project Fields) | Maps sheet columns to workflow variables |
| IF (Has Images?) | Splits path for image handling |
| HTTP Request (ImgBB – Upload Screenshot 1/2) | Uploads images to ImgBB |
| IF (Has Screenshot 2?) | Determines if second image exists |
| LangChain Google Gemini (Analyze Images) | Generates image descriptions |
| Set (Set Image Summaries) | Stores summaries for later use |
| LangChain Agent (AI Agent1) | Crafts the LinkedIn post |
| LangChain Chat Model (Qwen Cloud) | Powers the agent |
| Code (Extract Post Content) | Cleans the AI output |
| HTTP Request (Buffer) | Sends post to Buffer |
| Gmail (Post Published) | Sends success notification |
| Google Sheets (Update Row – Mark Completed) | Marks the row as done |

Prerequisites:
- An n8n instance (self‑hosted or n8n.cloud)
- Google Sheets API access (OAuth2) with a sheet named "LINKEDIN AUTOMATION 109 Days Plan" and a tab "Project Dashboard"
- Buffer account with API credentials
- ImgBB account for image hosting (API key)
- Google Gemini API key (via Google Palm credentials)
- Alibaba Cloud Qwen API key (optional, used as language model)
- Gmail account (OAuth2) for notifications
- Optional: Set environment variables IMGBB_API_KEY, etc.

Setup & Usage:
1. Clone this repository (or copy the JSON) into your n8n instance via Import → Workflow.
2. Replace all placeholder credential IDs with your own (or create new credentials for each service).
3. Set the Google Sheet document ID and tab ID in the two Google Sheets nodes.
4. Add your ImgBB API key as an environment variable or workflow variable.
5. Activate the workflow; it will run every Monday and Friday at 9 AM.
6. Monitor the Gmail alerts for completion or success notifications.

Use Cases:
- Marketing teams maintaining a consistent LinkedIn presence without manual copywriting.
- Founders or solopreneurs who want to showcase project progress on a fixed schedule.
- Agencies managing multiple client pages and needing a repeatable, AI‑assisted posting pipeline.
- Anyone who wants to turn a spreadsheet of project details into automated, algorithm‑optimized LinkedIn content.