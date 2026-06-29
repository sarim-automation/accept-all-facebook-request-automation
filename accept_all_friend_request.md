
# Accept All Facebook Friend Requests Automation

A lightweight JavaScript automation script that demonstrates browser automation techniques by automatically accepting pending Facebook friend requests using modern DOM APIs.

> **Educational Purpose Only**
>
> This project is intended solely for educational, research, and learning purposes. It demonstrates browser automation, DOM manipulation, asynchronous JavaScript, and UI interaction. This repository is **not affiliated with, endorsed by, or supported by Meta or Facebook.**

---

# Overview

This script automates the repetitive task of accepting pending Facebook friend requests by locating visible **Confirm** or **Accept** buttons, handling confirmation dialogs when necessary, and continuing until all visible requests have been processed.

The project is designed to demonstrate practical browser automation concepts and modern JavaScript techniques.

---

# Features

* Pure JavaScript
* No external libraries or dependencies
* Runs directly from the browser console
* Automatically detects pending friend requests
* Handles confirmation dialogs when required
* Automatic scrolling to load additional requests
* Randomized delays between actions
* Console progress logging
* Stops automatically when all visible requests have been processed
* Easy to customize and extend

---

# Requirements

* Google Chrome
* Microsoft Edge
* Brave Browser
* Opera
* Any modern Chromium-based browser

No installation required.

---

# Usage

1. Open your Facebook **Friend Requests** page:

   ```
   https://www.facebook.com/friends/requests
   ```

2. Press **F12** or **Ctrl + Shift + I** to open Developer Tools.

3. Open the **Console** tab.

4. Paste the script below.

5. Press **Enter**.

The automation will begin processing immediately.

---

# Script

```javascript
// Paste the acceptAllFriendRequests() script here.
async function acceptAllFriendRequests() {
  const delay = (ms) => new Promise((res) => setTimeout(res, ms));
  const randomDelay = (min, max) => delay(Math.floor(Math.random() * (max - min + 1)) + min);

  let processed = 0;
  let consecutiveFails = 0;

  console.log("🚀 Starting friend request acceptance...");
  console.log("📌 Make sure you're on: https://www.facebook.com/friends/requests");

  while (true) {
    // Find all buttons on the page
    const allButtons = Array.from(document.querySelectorAll('div[role="button"], button, a'));
    
    // Find confirm/accept buttons specifically
    const confirmButtons = allButtons.filter(btn => {
      const text = btn.textContent.trim().toLowerCase();
      const aria = (btn.getAttribute('aria-label') || '').toLowerCase();
      const rect = btn.getBoundingClientRect();
      
      // Look for confirm/accept buttons that are visible and reasonably sized
      const isConfirmButton = text.includes('confirm') || text.includes('accept') || 
                             text.includes('add friend') || text.includes('add as friend') ||
                             aria.includes('confirm') || aria.includes('accept') || 
                             aria.includes('add friend');
      
      // Exclude "Delete" or "Remove" buttons
      const isNotDelete = !text.includes('delete') && !text.includes('remove') && 
                         !aria.includes('delete') && !aria.includes('remove');
      
      const isSmallButton = rect.width > 0 && rect.width < 150 && rect.height > 0 && rect.height < 60;
      const isVisible = rect.top >= 0 && rect.top < window.innerHeight;
      
      return isConfirmButton && isNotDelete && isSmallButton && isVisible;
    });

    if (confirmButtons.length === 0) {
      // Try scrolling to load more requests
      window.scrollBy(0, 500);
      await randomDelay(1500, 2500);
      
      consecutiveFails++;
      if (consecutiveFails > 5) {
        console.log(`✅ All done! Accepted ${processed} friend requests.`);
        break;
      }
      continue;
    }

    consecutiveFails = 0;

    // Process each confirm button
    for (const btn of confirmButtons) {
      const btnText = btn.textContent.trim() || btn.getAttribute('aria-label');
      console.log(`✅ Accepting request: "${btnText}"`);
      
      btn.scrollIntoView({ block: "center" });
      await randomDelay(300, 600);
      
      btn.click();
      await randomDelay(1000, 1500);

      // Look for confirmation dialog (some requests might need confirmation)
      const allButtonsAfterClick = Array.from(document.querySelectorAll(
        'div[role="button"], button, a'
      ));
      
      const confirmDialogBtn = allButtonsAfterClick.find(el => {
        const modal = el.closest('div[role="dialog"]') || 
                     el.closest('div[aria-modal="true"]') || 
                     el.closest('div[data-type="modal"]');
        
        if (modal) {
          const text = el.textContent.trim().toLowerCase();
          const aria = (el.getAttribute('aria-label') || '').toLowerCase();
          return text.includes('confirm') || text.includes('accept') || 
                 aria.includes('confirm') || aria.includes('accept');
        }
        return false;
      });

      if (confirmDialogBtn) {
        confirmDialogBtn.click();
        processed++;
        console.log(`✅ Accepted request #${processed} (confirmed in dialog)`);
        await randomDelay(1000, 1500);
      } else {
        // No confirmation dialog needed
        processed++;
        console.log(`✅ Accepted request #${processed}`);
      }

      await randomDelay(1500, 2500);
    }

    // Scroll to load more
    window.scrollBy(0, 800);
    await randomDelay(1000, 2000);
  }
}

// Run the script
acceptAllFriendRequests();
```

---

# How It Works

The script performs the following steps:

1. Scans the Friend Requests page.
2. Locates visible **Confirm** or **Accept** buttons.
3. Clicks the appropriate button.
4. Handles confirmation dialogs if they appear.
5. Waits for a randomized delay.
6. Scrolls to load additional requests.
7. Repeats until no more pending requests remain.

---

# Project Structure

```text
Accept-All-Facebook-Friend-Requests/
│
├── README.md
├── script.js
└── LICENSE
```

---

# Disclaimer

This project is provided **"AS IS"**, without warranty of any kind.

By using this software, you acknowledge that:

* You are solely responsible for how you use this project.
* The author accepts no responsibility for account restrictions, suspensions, bans, data loss, or any damages resulting from its use.
* You are responsible for complying with all applicable laws and the Terms of Service of any platform on which this software is used.
* This repository is intended exclusively for educational and research purposes.
* The author does not encourage or endorse the misuse of automation tools or the violation of third-party platform policies.

---

# Warning

Websites frequently update their interfaces.

As a result:

* The script may require updates over time.
* Automation may trigger security checks.
* Your account could be temporarily or permanently restricted if platform policies are violated.

Use this software entirely at your own risk.

---

# Contributing

Contributions are welcome.

Bug reports, feature requests, documentation improvements, and pull requests are appreciated.

Please open an issue before submitting major changes.

---

# License

This project is licensed under the **MIT License**.

You are free to use, modify, and distribute this software under the terms of the MIT License.

See the `LICENSE` file for complete details.
