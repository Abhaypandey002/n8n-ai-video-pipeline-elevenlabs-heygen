# n8n-ai-video-pipeline-elevenlabs-heygen
Google Sheets/Docs ➜ n8n ➜ ElevenLabs voice ➜ HeyGen avatar video ➜ Drive — fully automated AI clone video pipeline with status polling and sheet updates.

## 🚀 What this workflow does

**Trigger**: Google Sheets rowAdded (when Status = NEW)

**Fetch**: Read the Google Doc content

**Condense**: LLM “Script Writer” prepares voice-ready text

**Generate**: Create avatar video via HeyGen /v2/video/generate

**Poll**: Check /v1/video_status.get until completed

**Store**: Upload MP4 to Drive

**Update**: Write VideoLink to the sheet and flip Status → DONE

## 🧱 Stack

**n8n** (workflow engine)

**Google Sheets, Docs, Drive**

**OpenAI** (LLM for the Script Writer)

**HeyGen** (avatar video)

**Elvenlabs** (Cloned Voice)

## 📋 Prerequisites

Create credentials in **n8n → Credentials** and plug them into the workflow:

  **OpenAI** → replace `REPLACE_WITH_OPENAI_CREDENTIAL_*`

  **HeyGen Header Auth** (API key) → replace `REPLACE_WITH_HEYGEN_HEADER_AUTH_*`

  **Google Docs / Sheets / Drive** → replace `REPLACE_WITH_GOOGLE_*`

Replace placeholders in node parameters:

  `YOUR_GOOGLE_SHEET_ID` (and the `gid` if needed)

  `YOUR_DRIVE_FOLDER_ID`

  `YOUR_HEYGEN_AVATAR_ID` and `YOUR_HEYGEN_VOICE_ID`

## 📑 Google Sheet structure (suggested)

<img width="813" height="122" alt="image" src="https://github.com/user-attachments/assets/eb515062-62ec-414b-907f-147320fa69f2" />


Add a new row with `Status = NEW`, `Title`, and a valid GoogleDocURL.

The workflow sets `VideoLink` and flips `Status → DONE`.

## 🔧 Import & Run

  1. In n8n, **Import** the provided JSON workflow.
  
  2. Connect your Heygen with ElvenLabs via `API`
  
  3. Open nodes with red credential badges and assign your creds.
  
  4. In **Generate Video** node, confirm your **avatar_id / voice_id**.
  
  5. **Activate** the workflow.

Append a new row to the sheet with `Status = NEW` → video link appears after render.

## 🗺️ Flow (overview)
`flowchart LR

  A[Google Sheets: add row (Status=NEW)] --> B[Get Google Doc]
  
  B --> C[Script Writer (LLM)]
  
  C --> D[HeyGen /v2/video/generate]
  
  D --> E[Poll /v1/video_status.get]
  
  E --> F[Download MP4]
  
  F --> G[Upload to Google Drive]
  
  G --> H[Update Sheet (VideoLink, Status=DONE)]`
  

## 🩺 Troubleshooting

  1. **Docs 404 / NOT_FOUND** → Share the Doc with the same Google account used by your **Docs credential**; or pass the **Document ID** instead of the full URL.
    
  2. “**JSON must be valid” (HTTP node)** → Set **Body** → **JSON** to **Expression mode** so n8n escapes quotes/newlines automatically.
    
  3. **rowAdded didn’t fire** → Inserted rows at the top may not trigger; append at bottom or switch to a **Cron + Read** sweeper pattern.
