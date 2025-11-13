# Create New OAuth Client - Step by Step

## Step 1: Create New OAuth Client
1. Go to: https://console.cloud.google.com/apis/credentials
2. Click **+ CREATE CREDENTIALS** → **OAuth client ID**
3. If you see "OAuth consent screen" warning:
   - Click **CONFIGURE CONSENT SCREEN**
   - User Type: **External** (for testing)
   - App name: **Practice2Panel** (or your app name)
   - User support email: Your email
   - Developer contact: Your email
   - Click **SAVE AND CONTINUE**
   - Scopes: Click **ADD OR REMOVE SCOPES**
     - Select: `openid`, `email`, `profile`
     - Click **UPDATE**
   - Click **SAVE AND CONTINUE**
   - Test users: Click **+ ADD USERS**
     - Add: `iqrashah.bscsf22dd@iba-suk.edu.pk`
     - Click **ADD**
   - Click **SAVE AND CONTINUE**
   - Click **BACK TO DASHBOARD**

## Step 2: Configure OAuth Client
1. Application type: **Web application**
2. Name: **Practice2Panel** (or any name)
3. **Authorized JavaScript origins** (click + ADD URI for each):
   ```
   http://localhost:3000
   http://127.0.0.1:3000
   ```
4. **Authorized redirect URIs** (click + ADD URI for each):
   ```
   http://localhost:3000/auth/google/callback
   http://127.0.0.1:3000/auth/google/callback
   ```
5. Click **CREATE**

## Step 3: Copy Credentials
1. A popup will show your new credentials
2. **Copy the Client ID** (looks like: `123456789-abc...apps.googleusercontent.com`)
3. **Copy the Client secret** (looks like: `GOCSPX-...`)
4. **IMPORTANT**: Copy these NOW - you won't see the secret again!

## Step 4: Update .env File
Replace the old credentials with the new ones in your `.env` file:

```env
GOOGLE_CLIENT_ID=<paste-new-client-id-here>
GOOGLE_CLIENT_SECRET=<paste-new-client-secret-here>
FRONTEND_URL=http://localhost:3000
```

## Step 5: Delete Old Client (Optional)
1. Go back to Credentials page
2. Find the old "Practice2Panel" client
3. Click the **Delete** icon (trash can)
4. Confirm deletion

## Step 6: Restart Flask Server
1. Stop your Flask server (Ctrl+C)
2. Start it again: `python app.py`
3. Wait for it to fully start

## Step 7: Test
1. Open your frontend in **incognito/private mode**
2. Try "Continue with Google" button
3. Make sure you're logged into Google with: `iqrashah.bscsf22dd@iba-suk.edu.pk`

## Important Notes:
- The redirect URIs must match EXACTLY (no trailing slashes)
- Wait 1-2 minutes after creating for Google to propagate changes
- Use incognito mode to avoid cached OAuth tokens
- Make sure your email is added as a test user in OAuth consent screen

