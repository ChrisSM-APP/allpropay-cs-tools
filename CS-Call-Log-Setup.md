# Setting up call logging for the CS Call Guide

A 3-minute, one-time setup that wires the CS Call Guide to log every call to a Google Sheet.

## What you'll have when this is done

- **Google Sheet** (already created): https://docs.google.com/spreadsheets/d/1aneeb69wneeJ0lZfSDqd1HttRNKgtRk1ywau2ZzKpKE
- Every time a CS rep wraps up a call in the guide, a new row appears in this sheet within ~5 seconds.

---

## Step 1. Open the sheet and add the Apps Script

1. Open the sheet linked above (titled **AllProPay CS Call Log**).
2. In the top menu: **Extensions → Apps Script**. A new tab opens.
3. Delete any default code in the editor.
4. Paste the script below in full.
5. Click the **Save** disk icon (or `Ctrl+S`).

```javascript
function doPost(e) {
  try {
    const data = JSON.parse(e.postData.contents);
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheets()[0];
    sheet.appendRow([
      new Date(),
      data.rep || '',
      data.client || '',
      data.path || '',
      data.reviewSent ? 'Yes' : '',
      data.referralCaptured ? 'Yes' : '',
      data.referralName || '',
      data.referralContact || '',
      data.callbackTime || '',
      data.issue || '',
      data.actionCommitted || '',
      data.lukewarmFeedback || '',
      data.notes || ''
    ]);
    return ContentService.createTextOutput(JSON.stringify({ok:true}))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService.createTextOutput(JSON.stringify({ok:false, error: String(err)}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

## Step 2. Deploy as a web app

1. Top right of the Apps Script editor: click **Deploy → New deployment**.
2. Click the gear icon next to "Select type" → choose **Web app**.
3. Fill in:
   - **Description**: `CS Call Log v1` (or leave blank)
   - **Execute as**: **Me (chris@allpropay.com)**
   - **Who has access**: **Anyone**
4. Click **Deploy**.
5. Google will ask you to authorize. Click **Authorize access** → pick your account → "Advanced" → "Go to (project name)" → **Allow**. (This is normal for personal Apps Scripts.)
6. After deploy, you'll see a **Web app URL** ending in `/exec`. **Copy that URL.**

## Step 3. Send me the URL

Paste the web app URL back to me and I'll bake it into the call guide. After that, every call wrap-up is logged.

---

## What gets logged

| Column | Always | When |
|---|---|---|
| Timestamp | Yes | Auto |
| Rep | Yes | Auto |
| Client | Yes | Auto |
| Path | Yes | `busy`, `lukewarm`, `problem`, `no-review`, `normal`, `referral` |
| Review Sent | Yes | `Yes` only on the review-sent path |
| Referral Captured | Yes | `Yes` only when a referral was captured |
| Referral Name | If applicable | Free text |
| Referral Contact | If applicable | Phone / email |
| Callback Time | If applicable | Busy path |
| Issue | If applicable | Problem path |
| Action Committed | If applicable | Problem path |
| Lukewarm Feedback | If applicable | Lukewarm path |
| Notes | If applicable | Free text |

## Headers (row 1)

If row 1 is missing labels when you first open the sheet, paste this into A1:

```
Timestamp	Rep	Client	Path	Review Sent	Referral Captured	Referral Name	Referral Contact	Callback Time	Issue	Action Committed	Lukewarm Feedback	Notes
```

## If something breaks

- The page caches failed sends in localStorage and retries on the next page load, so transient network issues won't drop logs.
- If you redeploy the script (e.g. you change something), you get a **new URL**. The page would need to be updated with the new URL.
- To view logs of the script itself (errors, etc.): in Apps Script editor, **Executions** in the left sidebar.
