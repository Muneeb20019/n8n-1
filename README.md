# Email Classifier and Summarizer with n8n & OpenAI

This project demonstrates an automated workflow built with n8n that classifies incoming Gmail emails using Artificial Intelligence (OpenAI), applies relevant labels, summarizes their content, logs key details to a Google Sheet, and can even draft AI-generated replies for specific categories.

---

## Problem Statement

Managing a high volume of incoming emails can be a time-consuming and often overwhelming task. Without proper automation, individuals and businesses can struggle with:
* **Manual Classification:** Categorizing emails by hand into "Promotions," "Sales," "Recruitment," "Health," etc., is repetitive and prone to human error.
* **Delayed Responses:** Important emails (e.g., sales inquiries, recruitment applications) might get buried, leading to missed opportunities or poor customer/candidate experience.
* **Information Overload:** Quickly grasping the core content of many emails requires reading through them all, which is inefficient.
* **Inconsistent Logging:** Manually logging email details to a spreadsheet for tracking or analysis is often neglected.

This workflow aims to streamline email management, ensuring that critical emails are identified, processed, and responded to efficiently.

---

## Solution Overview

This n8n workflow automates the entire process of email management. It functions as an intelligent email assistant that:
1.  **Monitors incoming Gmail emails.**
2.  **Analyzes email content** (subject and body snippet) using a powerful AI model (OpenAI's GPT-4o-mini).
3.  **Classifies emails** into predefined categories.
4.  **Applies corresponding labels** in Gmail.
5.  **Summarizes email content** for quick overview.
6.  **Logs classified and summarized data** to a Google Sheet.
7.  **Generates AI-powered draft replies** for specific email types (e.g., Recruitment, Health inquiries) to facilitate rapid communication.
8.  **Marks emails as read** once processed.

**Workflow:**
*(Once you've uploaded your workflow screenshot to `assets/`, uncomment and update the path below)*
*(Example: `![Workflow Diagram](assets/Screenshot_2025-06-01_9.04.50_PM.png)` if that's your filename)*

---

## Features

* **Automatic Email Triggering:** Polls Gmail for new, unread emails at regular intervals (e.g., every hour).
* **AI-Powered Classification:** Utilizes OpenAI's GPT-4o-mini to intelligently categorize emails based on their subject and snippet into custom-defined categories such as:
    * **Promotions:** Sales, discounts, marketing campaigns, special offers.
    * **Sales:** Product/service inquiries, follow-ups, proposals, lead generation.
    * **Recruitment:** Requests for documents, information, approvals, job applications.
    * **Health:** Medical appointments, prescriptions, lab results, health insurance, wellness programs.
* **Email Labeling:** Automatically applies the determined category as a label to the email in Gmail for easy filtering and organization.
* **Email Summarization:** Generates a concise summary of the email content using OpenAI, providing a quick overview without needing to read the full email.
* **Google Sheet Logging:** Appends email ID, date, subject, a short snippet, and the AI-generated summary to a designated Google Sheet, creating a searchable log.
* **AI-Generated Replies (Conditional):** For specific email types (e.g., "Recruitment" and "Health"), the workflow drafts an intelligent and contextually relevant reply using OpenAI, saving significant time.
* **Mark as Read:** Automatically marks processed emails as read in Gmail, helping maintain an organized inbox.

---

## Technologies Used

* **Automation Platform:** [n8n](https://n8n.io/)
* **Email Service:** Gmail
* **AI/LLM Provider:** [OpenAI](https://openai.com/) (specifically GPT-4o-mini model)
* **Data Storage:** Google Sheets

---

## Setup and Installation

To set up and run this workflow, you will need the following prerequisites and follow these steps:

### Prerequisites:

* An active **n8n instance** (self-hosted, desktop app, or cloud).
* A **Gmail Account** with access to create/manage labels.
* An **OpenAI API Key** with access to the GPT-4o-mini model.
* A **Google Account** with Google Sheets enabled.

### Installation Steps:

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/YOUR_USERNAME/n8n-1.git](https://github.com/YOUR_USERNAME/n8n-1.git)
    cd n8n-1
    ```
2.  **Download the Workflow JSON:**
    The workflow JSON is located in the `workflows/` directory.
    `workflows/email_classifier.json`
3.  **Import into n8n:**
    * Open your n8n instance in your web browser.
    * Navigate to the "Workflows" section in the left sidebar.
    * Click on the "New" button (or `+` icon) and select "Import from JSON" -> "Upload JSON File".
    * Choose the `email_classifier.json` file from the `workflows/` directory of your cloned repository.
4.  **Configure Credentials:**
    After importing, n8n will prompt you to link credentials. You will need to link or create the following:
    * **`Gmail OAuth2 API`**: Connect your Gmail account. Follow the prompts to authorize n8n to access your Gmail.
    * **`OpenAI API`**: Provide your OpenAI API key.
    * **`Google Sheets OAuth2 API`**: Connect your Google account. Authorize n8n to access your Google Sheets.
5.  **Configure Google Sheet Node:**
    * In the n8n workflow editor, locate the **"Google Sheets"** node.
    * In its parameters, find the `documentId` field.
    * **Replace the current value (`15zPhKFZf59pT4GFWpVn-nkl4iu3N4_1IW8wVN18ag9M`)** with the actual ID of your target Google Sheet where you want to log data.
    * **Ensure your Google Sheet has the following header columns in the first row (case-sensitive):** `ID`, `DATE `, `SUBJECT LINE`, `SUMMARY`, `SNIPPET`.
6.  **Configure Gmail Label Nodes:**
    * Locate the Gmail nodes named **"Promotions"**, **"Sales"**, **"Recruitment"**, and **"Health"**.
    * For each of these nodes, you will need to replace the placeholder `labelIds` with the actual **Label ID** from your Gmail account.
        * To find a Gmail Label ID: Go to Gmail in your browser, click on the label in the sidebar. In the URL, you'll see something like `/#label/Label_123456789`. Copy the `Label_123456789` part.
        * Example: Replace `Label_3898474445476321875` with your actual Promotions label ID.
7.  **Activate Workflow:**
    * Once all credentials are linked and node-specific configurations (Google Sheet ID, Gmail Label IDs) are updated, click the **"Save"** button at the top of the workflow editor.
    * Then, click the **"Activate"** toggle switch (usually in the top right corner) to set the workflow live.

---

## How to Use / Demo

Once the workflow is activated, it will run automatically based on the schedule defined in the "Gmail Trigger" node (defaulting to every hour).

* **Email Classification:** Send an email to your configured Gmail account that fits one of the defined categories (e.g., a "promotional" email, a "sales inquiry," a "job application," or a "health appointment reminder").
* **Gmail Labels:** Observe that the email automatically gets the corresponding label (Promotions, Sales, Recruitment, or Health) applied in your Gmail inbox.
* **Google Sheet Logging:** Check your designated Google Sheet. A new row should be added with the email's ID, the current date, its subject line, a summarized version, and the original snippet.
* **AI-Generated Replies (Drafts):** For emails classified as "Recruitment" or "Health", a draft reply will be created in your Gmail account. Review and send these as needed.
* **Marked as Read:** The processed email will be marked as read in your Gmail inbox.

**Example Outputs:**
*(Once you've uploaded your screenshots to `assets/`, uncomment and update the paths below)*
*(Example: `![Gmail Labels Applied](assets/Screenshot_2025-06-01_9.09.23_PM.png)`)*
*(Example: `![Google Sheet Output](assets/image_739bf4.png)`)*
*(Example: `![Example AI-Generated Reply](assets/Screenshot_2025-06-02_8.05.19_PM.png)`)*

---

## Future Enhancements

* **Notifications:** Add integrations to send notifications (e.g., to Slack, Discord, Microsoft Teams) for specific email classifications (e.g., "Sales" leads or "Recruitment" applications).
* **CRM Integration:** Directly create leads or contacts in a CRM system (e.g., Salesforce, HubSpot) based on "Sales" email classifications.
* **Sentiment Analysis:** Incorporate sentiment analysis to gauge the tone of incoming emails and prioritize urgent or negative correspondence.
* **Dynamic Prompts:** Allow for more dynamic AI prompt customization directly within n8n.
* **Advanced Filtering:** Implement more complex filtering logic within the Gmail Trigger or subsequent nodes.

---

## Author

* [Your Name Here] ([LinkedIn Profile URL](https://www.linkedin.com/in/YOUR_LINKEDIN_PROFILE/))
* [Your Website/Portfolio URL (Optional)]

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
