# Setting up Dino Days on GitHub + iPhone

## Step 1 — Create the GitHub repository

1. Go to **github.com** and sign in
2. Click the **+** icon top right → **New repository**
3. Name it `dino-days` (or whatever you like)
4. Set it to **Public** (required for free GitHub Pages)
5. Leave everything else unchecked → click **Create repository**

---

## Step 2 — Initialize git in your local folder

Open Terminal and navigate to your project folder:

```bash
cd ~/path/to/dino-tasks
```

Then run these commands one by one:

```bash
# Initialize git
git init

# Add all files
git add .

# Make your first commit
git commit -m "Initial commit — Dino Days"

# Rename the branch to main
git branch -M main

# Link to your GitHub repo (replace YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/dino-days.git

# Push to GitHub
git push -u origin main
```

---

## Step 3 — Enable GitHub Pages

1. Go to your repo on GitHub
2. Click **Settings** (top tab)
3. Click **Pages** in the left sidebar
4. Under **Source**, select **Deploy from a branch**
5. Select **main** branch, **/ (root)** folder
6. Click **Save**

After ~60 seconds, your site will be live at:
```
https://YOUR_USERNAME.github.io/dino-days/
```

---

## Step 4 — Add to iPhone home screen

1. Open **Safari** on your iPhone (must be Safari, not Chrome)
2. Go to `https://YOUR_USERNAME.github.io/dino-days/`
3. Tap the **Share** button (box with arrow pointing up)
4. Scroll down and tap **Add to Home Screen**
5. Name it `Dino Days` → tap **Add**

It now lives on your home screen as a full-screen app with no browser chrome.

---

## Making updates later

Whenever you edit the code, push your changes:

```bash
git add .
git commit -m "Updated tasks"
git push
```

GitHub Pages will rebuild automatically within ~30 seconds.

---

## Running locally (without GitHub)

```bash
cd ~/path/to/dino-tasks
python3 -m http.server 8080
```

Open `http://localhost:8080` in your browser.
To test on iPhone over Wi-Fi: find your Mac's local IP (`System Preferences → Wi-Fi → Details`) and open `http://YOUR_MAC_IP:8080` in Safari on your phone.
