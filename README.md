# Gmail Email Automation

An n8n workflow that reads records from Google Sheets and sends a formatted
summary by Gmail. The workflow is manually triggered and skips the email step
when the spreadsheet contains no rows.

## Workflow

![n8n Gmail automation workflow](Email-automation-n8n-workflow.png)

The workflow runs as follows:

1. **Manual Trigger** starts the workflow when **Execute workflow** is clicked.
2. **Get rows in sheet** reads all records from the configured Google Sheet.
3. **IF** checks whether at least one row was returned.
4. **Code in JavaScript** creates the subject and plain-text email body.
5. **Send a message** sends the summary through Gmail.

If the spreadsheet is empty, the workflow takes no further action.

## Files

- `Gmail Email Automation.json` - importable n8n workflow
- `n8n Assignment Data.xlsx` - source workbook saved for reference
- `Email-automation-n8n-workflow.png` - screenshot of the workflow in n8n

The workflow is configured to read a Google Sheet. The local `.xlsx` file is
not read directly by n8n.

## Spreadsheet format

The JavaScript node expects rows with the following fields:

| Field | Description |
|---|---|
| `name` | Person or record name |
| `course` | Course or item associated with the record |
| `status` | Current status |

Example:

| name | course | status |
|---|---|---|
| Alice | Python | Active |
| Bob | JavaScript | Completed |

The email body assigns each row a number and prints:

```text
1. Name: Alice | Course: Python | Status: Active
```

## Requirements

- An n8n instance
- A Google Sheets account
- A Gmail account
- Permission to read the source spreadsheet
- Permission to send email from the configured Gmail account

## Google setup

1. Upload or create the source spreadsheet in Google Sheets.
2. Make sure the first row contains column names such as `name`, `course`, and
   `status`.
3. In n8n, create a Google Sheets OAuth2 credential.
4. Open **Get rows in sheet** and select the spreadsheet and worksheet.

The exported workflow references an existing Google Drive spreadsheet. Importing
it into another n8n account requires re-selecting that spreadsheet.

## Gmail setup

1. In n8n, create a Gmail OAuth2 credential.
2. Open **Send a message** and select the Gmail credential.
3. Change `sendTo` to the intended recipient address.

Do not commit OAuth credentials or private recipient data to source control.

## Import and configure

1. Open n8n and select **Import from File**.
2. Import `Gmail Email Automation.json`.
3. Add and select your Google Sheets OAuth2 credential.
4. Select the source spreadsheet and worksheet.
5. Add and select your Gmail OAuth2 credential.
6. Update the recipient email address.
7. Click **Execute workflow** to test it.

The workflow is inactive by default because it uses a manual trigger.

## Email output

Subject:

```text
Data Summary from Google Sheet
```

The plain-text body contains the number of records and one summary line per
row.

## Test and limitations

- Run the workflow with at least one spreadsheet row to verify Gmail delivery.
- Test with an empty sheet to confirm that no email is sent.
- Keep column headers lowercase and consistent with `name`, `course`, and
  `status`.
- The workflow sends only one summary email and does not send per-row emails.
- The spreadsheet data is sent to Gmail, so use suitable credentials and
  access controls for sensitive information.
