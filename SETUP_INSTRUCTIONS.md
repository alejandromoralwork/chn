# Firebase Setup Instructions for Chinese Practice Websites

## Overview
This guide will help you set up Firebase Realtime Database for your two Chinese practice websites: Hanzi and Vocabulary.

---

## Part 1: Create Firebase Project

### Step 1: Go to Firebase Console
1. Visit [https://console.firebase.google.com/](https://console.firebase.google.com/)
2. Sign in with your Google account
3. Click **"Add project"** or **"Create a project"**

### Step 2: Configure Project
1. **Project name**: Enter a name (e.g., "chinese-practice" or "hanzi-study")
2. Click **Continue**
3. **Google Analytics**: You can disable this (optional for this project)
4. Click **Create project**
5. Wait for project creation to complete
6. Click **Continue**

---

## Part 2: Set Up Realtime Database

### Step 1: Create Database
1. In your Firebase project dashboard, click **"Realtime Database"** in the left sidebar (under "Build")
2. Click **"Create Database"**
3. **Database location**: Choose a location close to you (e.g., `us-central1`)
4. Click **Next**

### Step 2: Configure Security Rules
1. **Start in test mode** (allows public read/write)
2. Click **Enable**

⚠️ **Important**: Test mode allows anyone to read/write to your database. Since you mentioned this is fine for your use case, we'll use this. The website has password protection for editing.

Later, you can update rules in the "Rules" tab:
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

---

## Part 3: Get Firebase Configuration

### Step 1: Register Web App
1. In Firebase Console, go to **Project Overview** (home icon at top left)
2. Click the **Web icon** `</>` to add a web app
3. **App nickname**: Enter a name (e.g., "Hanzi Practice Web")
4. **Do NOT** check "Also set up Firebase Hosting" (we'll use GitHub Pages)
5. Click **Register app**

### Step 2: Copy Configuration
You'll see a code snippet like this:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "your-project-id.firebaseapp.com",
  databaseURL: "https://your-project-id-default-rtdb.firebaseio.com",
  projectId: "your-project-id",
  storageBucket: "your-project-id.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef1234567890"
};
```

**Copy these values** - you'll need them in the next step!

---

## Part 4: Update Your HTML Files

### Update hanzi.html
1. Open `hanzi.html` in a text editor
2. Find this section (around line 184):
```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "chngit-bb789.firebaseapp.comchngit-bb789.firebaseapp.com",
    ...
};
```
3. **Replace** with your actual Firebase config values
4. Save the file

### Update vocabulary.html
1. Open `vocabulary.html` in a text editor
2. Find the same section (around line 193)
3. **Replace** with your actual Firebase config values
4. Save the file

---

## Part 5: Convert Excel Files to JSON

### Step 1: Open the Converter
1. Open `excel-to-firebase.html` in your web browser
2. This is a local tool - it runs entirely in your browser

### Step 2: Convert Hanzi File
1. Click **"Choose File"** and select `Copy of Optimized Remembering the Hanzi.xlsx`
2. Click **"Convert to JSON"**
3. Review the preview to ensure data looks correct
4. Click **"Copy JSON"** or **"Download JSON"**
5. Save this as `hanzi-data.json`

### Step 3: Convert Vocabulary File
1. Refresh the page or click upload again
2. Select `Copy of 31k Chinese Vocabulary in Optimized Remembering the Hanzi Order.xlsx`
3. Click **"Convert to JSON"**
4. Click **"Copy JSON"** or **"Download JSON"**
5. Save this as `vocabulary-data.json`

---

## Part 6: Import Data to Firebase

### Method 1: Manual Add (Easiest - RECOMMENDED)
1. Go to Firebase Console → **Realtime Database**
2. You'll see your database root URL (something like `https://your-project-default-rtdb.firebaseio.com/`)
3. Click the **"+"** icon next to the database root URL
4. A dialog appears with two fields:
   - **Name**: Enter `hanzi`
   - **Value**: Leave empty for now, click **Add**
5. Now you'll see a new `hanzi` node in the database
6. Click the **three dots (⋮)** next to the `hanzi` node
7. Select **"Import JSON"**
8. Choose your `hanzi-data.json` file
9. Click **Import**
10. Repeat steps 3-9 for vocabulary:
    - Name: `vocabulary`
    - Import: `vocabulary-data.json`

### Method 2: Direct Import to Root (Then Move)
1. Go to Firebase Console → **Realtime Database**
2. Click the **three dots (⋮)** menu next to your database root URL
3. Select **"Import JSON"**
4. Choose `hanzi-data.json`
5. Click **Import** (this imports to root)
6. After import, you'll see numbered entries (0, 1, 2...) at root
7. Click the **three dots (⋮)** next to the root
8. Select **"Export JSON"** to backup
9. Delete the imported items
10. Create a new node called `hanzi` (use the "+" button)
11. Import into that `hanzi` node instead

### Method 3: Manual Copy-Paste (For Small Datasets)
1. Open `hanzi-data.json` in a text editor
2. Copy the ENTIRE JSON content
3. Go to Firebase Console → **Realtime Database**
4. Click the **"+"** icon next to your database URL
5. **Name**: Enter `hanzi`
6. **Value**: Paste the entire JSON content
7. Click **Add**
8. Repeat for vocabulary data

Your database structure should look like:
```
your-database
├── hanzi
│   ├── 0: {id: 1, ...columns..., comments: ""}
│   ├── 1: {id: 2, ...columns..., comments: ""}
│   └── ...
└── vocabulary
    ├── 0: {id: 1, ...columns..., comments: ""}
    ├── 1: {id: 2, ...columns..., comments: ""}
    └── ...
```

---

## Part 7: Change Default Password

Both websites use the same default password: **`hanzi`**

### To Change the Password:

1. Open `hanzi.html` in a text editor
2. Find this line (around line 117):
```javascript
const PASSWORD = "hanzi";
```
3. Change `"hanzi"` to your desired password
4. Save the file

5. Open `vocabulary.html` in a text editor
6. Find the same line (around line 126)
7. Change `"hanzi"` to the same password
8. Save the file

⚠️ **Security Warning**: The password is stored in plain text in the HTML files. Anyone can view the source code and see it. This is only suitable for personal use where convenience is more important than security.

---

## Part 8: Deploy to GitHub Pages

### Step 1: Create GitHub Repository
1. Go to [https://github.com/new](https://github.com/new)
2. **Repository name**: `chinese-practice` (or any name)
3. Make it **Public**
4. Click **Create repository**

### Step 2: Initialize Git and Push Files
Open PowerShell in your project folder (`C:\Users\pc\chn`) and run:

```powershell
# Initialize git repository
git init

# Add all files
git add hanzi.html vocabulary.html excel-to-firebase.html

# Commit
git commit -m "Initial commit: Chinese practice websites"

# Add remote (replace YOUR_USERNAME with your GitHub username)
git remote add origin https://github.com/YOUR_USERNAME/chinese-practice.git

# Push to GitHub
git branch -M main
git push -u origin main
```

### Step 3: Enable GitHub Pages
1. Go to your repository on GitHub
2. Click **Settings**
3. Click **Pages** in the left sidebar
4. Under "Source", select **main** branch
5. Click **Save**
6. Wait 1-2 minutes for deployment

### Step 4: Access Your Websites
Your websites will be available at:
- **Hanzi**: `https://YOUR_USERNAME.github.io/chinese-practice/hanzi.html`
- **Vocabulary**: `https://YOUR_USERNAME.github.io/chinese-practice/vocabulary.html`

---

## Part 9: Test Everything

### Test Hanzi Website
1. Visit your Hanzi page
2. You should see a login modal
3. Enter your password
4. The data should load from Firebase
5. Try editing a cell and saving
6. Refresh the page - changes should persist

### Test Vocabulary Website
1. Visit your Vocabulary page
2. Test login, editing, pagination, and search
3. Verify changes are saved to Firebase

---

## Troubleshooting

### Problem: "I accidentally imported JSON into the root - how do I delete it?"
**Solution - Fast Method (Delete Everything at Once):**
1. Go to Firebase Console → **Realtime Database**
2. You'll see numbered entries (0, 1, 2, 3...) directly under your database root
3. **Click directly on your database root URL** (the main URL at the top, like `https://your-project-default-rtdb.firebaseio.com/`)
4. On the right side panel, you'll see the full JSON data
5. Click the **three dots (⋮)** at the top of the right panel (next to the database name)
6. Select **"Delete"** or **"Remove"**
7. Confirm the deletion - this will clear EVERYTHING in your database
8. **After deletion:**
   - Your database is now empty
   - Follow Method 1 in Part 6 to set up correctly
   - Create `hanzi` node first, THEN import into it
   - Repeat for `vocabulary`

**Alternative Method (Using Rules):**
1. If the delete option doesn't appear, you can also:
2. Go to the **Rules** tab
3. Temporarily set: `{".write": true, ".read": true}`
4. Go back to **Data** tab
5. Try the delete method above again

### Problem: "Error loading data from Firebase"
- **Check**: Firebase config values are correct in HTML files
- **Check**: Database rules allow read/write access
- **Check**: Database URL ends with `.firebaseio.com`
- **Check**: Browser console (F12) for specific errors

### Problem: "No data found"
- **Check**: Data was imported to correct paths (`/hanzi` and `/vocabulary`)
- **Check**: Firebase Console shows data in Realtime Database
- **Verify**: JSON structure has `id` field and `comments` field

### Problem: "Changes not saving"
- **Check**: You're logged in (entered correct password)
- **Check**: Database rules allow write access
- **Check**: Network tab (F12) shows successful Firebase requests

### Problem: "Chinese characters not displaying"
- **Check**: File encoding is UTF-8
- **Check**: Excel file has correct character data
- **Try**: Re-export Excel to CSV with UTF-8 encoding

### Problem: "Login not working"
- **Check**: Password hash was generated correctly
- **Check**: You're entering the right password (case-sensitive)
- **Try**: Use browser console to verify hash generation

---

## Security Notes

⚠️ **Important Security Considerations:**

1. **Client-side password**: The password protection is client-side only. Anyone can view the source code and see the hash or remove the login check. This is OK for personal use but not secure for sensitive data.

2. **Public database**: With current rules, anyone with your database URL can read/write data directly via Firebase API (bypassing your website). This is fine if you're the only user.

3. **For better security** (optional):
   - Set up Firebase Authentication
   - Use Firebase Security Rules to require authentication
   - Move password hashing to a serverless function

4. **GitHub Pages**: Your HTML files are public. Don't store sensitive information in them.

---

## Optional Enhancements

### Add a Landing Page
Create `index.html` with links to both websites:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chinese Practice Hub</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center">
    <div class="text-center">
        <h1 class="text-4xl font-bold mb-8">🇨🇳 Chinese Practice</h1>
        <div class="space-y-4">
            <a href="hanzi.html" class="block bg-blue-500 hover:bg-blue-600 text-white font-bold py-4 px-8 rounded-lg text-xl">
                📚 Hanzi Practice
            </a>
            <a href="vocabulary.html" class="block bg-green-500 hover:bg-green-600 text-white font-bold py-4 px-8 rounded-lg text-xl">
                📖 Vocabulary Practice (31k)
            </a>
        </div>
    </div>
</body>
</html>
```

### Export Your Data
Add an export button to save your data locally as backup:
```javascript
function exportData() {
    const json = JSON.stringify(allData, null, 2);
    const blob = new Blob([json], { type: 'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `data-backup-${Date.now()}.json`;
    a.click();
}
```

---

## Quick Reference

### Default Password
- **Password**: `hanzi` (stored in plain text in HTML files)

### Firebase Paths
- **Hanzi data**: `/hanzi`
- **Vocabulary data**: `/vocabulary`

### Files
- **Hanzi website**: `hanzi.html`
- **Vocabulary website**: `vocabulary.html`
- **Excel converter**: `excel-to-firebase.html`
- **Setup guide**: `SETUP_INSTRUCTIONS.md` (this file)

---

## Support

If you encounter issues:
1. Check browser console (F12 → Console) for errors
2. Verify Firebase Console shows your data
3. Test database rules in Firebase Console
4. Ensure all files are UTF-8 encoded

---

**Good luck with your Chinese studies! 加油！🎉**
