# How to Verify Your OAuth Client ID

## You're Already on the Right Page!

Looking at your Google Cloud Console, you can see:
- **OAuth 2.0 Client IDs** section
- Client named: **"Practice2Panel"**
- Client ID starts with: `731124056099-ehod...`

## Steps to Verify:

### 1. Click on the Client Name
- Click on **"Practice2Panel"** (the clickable link in the Name column)
- This will open the client details page

### 2. View Full Client ID
- On the details page, you'll see the full **Client ID**
- It should match: `731124056099-ehod57r9sc72ldr7lht40i99p7fmoh6s.apps.googleusercontent.com`
- You can also copy it using the copy icon (📋) next to it

### 3. Check Client Status
- Make sure it shows **"Active"** status (not "Deleted" or "Disabled")
- If it's deleted, you'll need to create a new one

### 4. Verify Configuration
On the client details page, check:

**Authorized JavaScript origins:**
```
http://localhost:3000
http://127.0.0.1:3000
```

**Authorized redirect URIs:**
```
http://localhost:3000/auth/google/callback
http://127.0.0.1:3000/auth/google/callback
```

### 5. Edit if Needed
- Click the **Edit** button (pencil icon) in the Actions column
- Or click "Practice2Panel" and then click "EDIT" on the details page
- Make sure the redirect URIs match EXACTLY (no trailing slashes)

## Quick Check:
1. Click **"Practice2Panel"** → See full Client ID
2. Verify it matches your `.env` file
3. Check redirect URIs are correct
4. Make sure status is "Active"

