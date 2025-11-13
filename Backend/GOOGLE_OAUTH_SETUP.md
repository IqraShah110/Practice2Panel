# Google OAuth Setup Guide

## Step 1: Go to Google Cloud Console
1. Visit: https://console.cloud.google.com/
2. Select your project (or create a new one)
3. Go to **APIs & Services** → **Credentials**

## Step 2: Create OAuth 2.0 Client ID
1. Click **+ CREATE CREDENTIALS** → **OAuth client ID**
2. If prompted, configure the OAuth consent screen first:
   - User Type: **External** (for testing) or **Internal** (for organization)
   - App name: Your app name
   - User support email: Your email
   - Developer contact: Your email
   - Click **Save and Continue**
   - Scopes: Add `email`, `profile`, `openid`
   - Test users: Add your email for testing
   - Click **Save and Continue**

## Step 3: Configure OAuth Client
1. Application type: **Web application**
2. Name: e.g., "Interview App OAuth Client"
3. **Authorized JavaScript origins:**
   ```
   http://localhost:3000
   http://127.0.0.1:3000
   ```
   (Add your production URL when deploying)

4. **Authorized redirect URIs:**
   ```
   http://localhost:3000/auth/google/callback
   http://127.0.0.1:3000/auth/google/callback
   ```
   (Add your production callback URL when deploying)

5. Click **CREATE**

## Step 4: Copy Credentials
1. Copy the **Client ID** (looks like: `123456789-abc...apps.googleusercontent.com`)
2. Copy the **Client secret** (looks like: `GOCSPX-...`)
3. Update your `.env` file with these values

## Step 5: Update .env File
```env
GOOGLE_CLIENT_ID=your-new-client-id-here
GOOGLE_CLIENT_SECRET=your-new-client-secret-here
FRONTEND_URL=http://localhost:3000
```

## Important Notes:
- The redirect URI must match EXACTLY (including trailing slashes)
- For local development, use `http://localhost:3000/auth/google/callback`
- The OAuth client must be active (not deleted)
- Make sure the consent screen is published or you're added as a test user

