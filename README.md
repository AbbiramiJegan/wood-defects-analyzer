# 🪵 AI Wood Defect Analyzer (n8n Workflow)

This project is an automated workflow built on **n8n** that detects defects in wood surfaces using Computer Vision (OpenAI GPT-4o-mini). It streamlines the quality control process by allowing users to upload images, automatically cataloging defects into a database, and presenting the data on a web-based dashboard.

## 🚀 Features

* **Image Ingestion:** Simple form interface for uploading wood sample images.
* **Automated Storage:** Automatically saves uploaded images to a specific **Google Drive** folder.
* **AI Analysis:** Uses an AI Agent to inspect images and identify:
    * Defect Type (e.g., discoloration, knots, cracks)
    * Severity
    * Location on the board
    * Confidence Level
* **Database Logging:** Appends analysis results and image links directly to **Google Sheets**.
* **Live Dashboard:** Includes a secondary flow that generates a responsive **HTML Table** (Bootstrap styled) to view all analyzed items and their thumbnails.

## 🛠️ Architecture

The workflow consists of two main branches:

1.  **The Analysis Pipeline:**
    `Form Trigger` → `Google Drive Upload` → `AI Agent (LangChain)` → `Data Formatting` → `Google Sheets Append`
2.  **The Dashboard Pipeline:**
    `Webhook` → `Fetch Sheet Data` → `HTML Generation (JS)` → `Respond to Webhook`

## 📋 Prerequisites

To run this workflow, you need:

1.  **n8n** (Self-hosted or Cloud).
2.  **OpenAI API Key** (Must support Vision models, e.g., GPT-4o-mini).
3.  **Google Cloud Platform Project** with credentials for:
    * Google Drive API
    * Google Sheets API

## ⚙️ Setup Guide

### 1. Google Sheets Preparation
Create a new Google Sheet. You **must** create a header row (Row 1) with the exact following columns for the code to work:

| A | B | C | D | E |
| :--- | :--- | :--- | :--- | :--- |
| **No.** | **Defect Type** | **Severity** | **Confidence** | **Image** |

*Note: The "Image" column will be populated with a thumbnail link automatically.*

### 2. Import Workflow
1.  Download the `wood_defect_analyzer.json` file.
2.  Open your n8n instance.
3.  Go to **Workflows** > **Import from File**.

### 3. Configure Credentials
You will need to re-connect the nodes to your own accounts:
* **Google Drive Node:** Select your credential and choose the specific Folder ID where images should be saved.
* **Google Sheets Node:** Select your credential and choose the Sheet created in Step 1.
* **OpenAI Node:** Select your OpenAI credential.

### 4. Activate
1.  Toggle the workflow to **Active**.
2.  Use the **Form Trigger** URL to upload an image.
3.  Use the **Webhook** URL to view the Dashboard.

## 🖥️ Usage

### Analyzing an Image
1.  Open the **Production URL** of the `Form Trigger` node.
2.  Upload a photo of a wood surface.
3.  Wait for the process to complete.

### Viewing Results
1.  Open the **Production URL** of the `Webhook` node.
2.  A webpage will load displaying a table of all processed logs, including a thumbnail of the wood and the AI's diagnosis.

## 🧩 Key Nodes Explained

* **AI Agent:** This node uses a system prompt: *"Analyze the image for wood defects... Return a comma-separated list..."*. It acts as the brain of the operation.
* **Code in JavaScript (Formatting):** Parses the text output from the AI and constructs a valid Google Drive thumbnail link.
* **Code in JavaScript (HTML):** Dynamically builds an HTML page using template literals and maps the Google Sheet rows into table `<tr>` elements.

## 🤝 Contributing
Feel free to fork this workflow. Future improvements could include:
* Adding email notifications for "High Severity" defects.
* Implementing a "Retry" loop for low-confidence results.
* Expanding the HTML dashboard to include charts/graphs.

**License:** MIT
