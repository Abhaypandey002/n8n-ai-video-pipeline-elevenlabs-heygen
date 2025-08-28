# n8n-ai-video-pipeline-elevenlabs-heygen
Google Sheets/Docs ➜ n8n ➜ ElevenLabs voice ➜ HeyGen avatar video ➜ Drive — fully automated AI clone video pipeline with status polling and sheet updates.

Stack

n8n (workflow engine)

Google Sheets, Docs, Drive

OpenAI (LLM)

HeyGen (avatar video)

What the workflow does

Trigger: Google Sheets rowAdded (Status = NEW).

Docs: Fetch the document by URL.

Script Writer: Summarize/condense text for spoken delivery.

HeyGen: Generate video (/v2/video/generate).

Status Polling: Check /v1/video_status.get until completed.

Drive: Upload MP4 and update the sheet (Status=DONE, VideoLink).

Prerequisites

Create credentials in n8n:

OpenAI → replace REPLACE_WITH_OPENAI_CREDENTIAL_*

HeyGen Header Auth (API key) → replace REPLACE_WITH_HEYGEN_HEADER_AUTH_*

Google Docs / Sheets / Drive → replace the REPLACE_WITH_GOOGLE_* placeholders

Update IDs/placeholders in the workflow:

YOUR_GOOGLE_SHEET_ID and sheet gid

YOUR_DRIVE_FOLDER_ID

YOUR_HEYGEN_AVATAR_ID and YOUR_HEYGEN_VOICE_ID (or keep the defaults)

Google Sheet (suggested columns)

Status | Title | GoogleDocURL | VideoLink

New row: set Status=NEW, fill Title and GoogleDocURL.

The workflow sets VideoLink and flips Status to DONE.

Import & Run

In n8n, Import the JSON workflow.

Open nodes with red credential badges and assign your creds.

In Generate Video node, confirm avatar/voice IDs.

Activate the workflow.

Add a row in the sheet (Status=NEW) and watch the link appear after render.

Notes / Gotchas

If Docs returns 404, share the file with the same Google account used by your n8n credential.

If HeyGen body errors with “valid JSON,” switch the Body → JSON field to Expression mode so n8n escapes quotes/newlines.

Inserted rows at the top may not fire rowAdded. Prefer appending at the bottom, or swap to a Cron + Read pattern to sweep all NEW rows.
