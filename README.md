<h1 align="center">
  <br>
  <!-- LOGO PLACEHOLDER -->
  <img src="logo.png" alt="Project Logo" height="80">
  <br><br>
  GDG Certificate Verification System
</h1>
  
<p align="center">
    An automated, serverless certificate verification system built for the <strong>Google Cloud Study Jam</strong>.
  </p>
  
<p align="center">
    <a href="https://gdgjistverifycertificates.netlify.app/">View Live Demo</a> •
    <a href="#how-it-works">How it Works</a> •
    <a href="#setup-guide">Build Your Own</a>
  </p>


<p align="center">
  <img src="https://img.shields.io/badge/Status-Live-success?style=for-the-badge" alt="Status" />
  <img src="https://img.shields.io/badge/Stack-HTML%20%2B%20Google%20Apps%20Script-4285F4?style=for-the-badge&logo=google" alt="Stack" />
  <img src="https://img.shields.io/badge/Database-Google%20Sheets-34A853?style=for-the-badge&logo=googlesheets" alt="DB" />
  <img src="https://img.shields.io/badge/Hosting-Netlify-00C7B7?style=for-the-badge&logo=netlify" alt="Hosting" />
</p>

</div>

<br />

## ⚡ About The Project

This project solves a major problem for community organizers: **How to issue verifiable certificates without paying for expensive servers or hosting.**

It allows students to:
1.  **Verify** their completion status using a unique Certificate ID.
2.  **Preview** their certificate instantly in the browser.
3.  **Add to LinkedIn** with one click (using official organization IDs).
4.  **Download** the PDF copy directly from Google Drive.

The entire system runs for **free** using Google's ecosystem as a backend.

## 🏗 Architecture

This project uses a **Serverless Architecture** where Google Sheets acts as the database and Google Drive acts as the file storage.

```mermaid
graph LR
    A[User] -- Enters ID --> B(Netlify Website)
    B -- Fetch Request --> C{Google Apps Script API}
    C -- Query --> D[(Google Sheet Database)]
    D -- Return Data (Name, Date, FileID) --> C
    C -- JSON Response --> B
    B -- Displays Data --> A
````

  * **Frontend:** HTML5, CSS3 (Glassmorphism UI), Vanilla JavaScript.
  * **Backend API:** Google Apps Script (doGet).
  * **Database:** Google Sheets.
  * **Storage:** Google Drive (PDFs).

-----
<h2 id="setup-guide">🚀 How to Build This Yourself</h2>

Want to use this for your own GDG or Club? Follow these steps.

### Phase 1: The Database (Google Sheet)

1.  Create a new Google Sheet.
2.  Create columns in this exact order:
      * `A` Certificate ID
      * `B` Student Name
      * `C` Profile URL (optional)
      * `D` Issue Date
      * `E` File ID (This is the Google Drive ID of the PDF)

### Phase 2: The API (Google Apps Script)

1.  Open your Google Sheet, go to **Extensions \> Apps Script**.
2.  Paste the following code into `Code.gs`:

\<details\>
\<summary\>Click to view the Backend API Code\</summary\>

```javascript
function doGet(e) {
  var idToCheck = e.parameter.id;
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Sheet1'); // Change to your tab name
  var data = sheet.getDataRange().getValues();
  
  var result = { valid: false, error: "Certificate not found" };

  for (var i = 1; i < data.length; i++) {
    if (data[i][0] == idToCheck) {
      result = {
        valid: true,
        certId: data[i][0],
        name: data[i][1],
        profileUrl: data[i][2],
        date: data[i][3],
        fileId: data[i][4]
      };
      break; 
    }
  }

  return ContentService.createTextOutput(JSON.stringify(result))
    .setMimeType(ContentService.MimeType.JSON);
}
```

\</details\>

3.  Click **Deploy** \> **New Deployment** \> **Web App**.
4.  **Important:** Set *Who has access* to **"Anyone"**.
5.  Copy the **Web App URL**.

### Phase 3: The Frontend

1.  Clone this repository.
2.  Open `index.html`.
3.  Find the `API_URL` variable in the `<script>` section.
4.  Replace it with your **Web App URL** from Phase 2.
5.  Upload `index.html`, `logo.png`, and `google.svg` to Netlify or GitHub Pages.

-----

## 🎨 Features & UI

  * **Glassmorphism Design:** Modern, frosted glass card UI.
  * **Google Branding:** Uses official Google colors and fonts (Inter).
  * **Smart Loader:** Google-colored loading spinner.
  * **Dark Mode Safe:** Optimized logos and contrasts.
  * **LinkedIn Integration:** Generates a pre-filled "Add to Profile" link.
  * **Visitor Counter:** Tracks page hits using `counterapi.dev`.

## 📂 Directory Structure

```text
/
├── index.html        # The main verification portal
├── logo.png          # Your community logo
├── google.svg        # Google G icon for buttons
└── README.md         # Documentation
```

## 🤝 Contributing

Contributions are welcome\! If you have ideas to make this better, feel free to fork the repo and submit a pull request.

1.  Fork the Project
2.  Create your Feature Branch
3.  Commit your Changes
4.  Push to the Branch
5.  Open a Pull Request

## 👤 Author

**Ezaz Ahmed**

  * GDG On Campus Lead, JIST
  * [LinkedIn](https://www.linkedin.com/in/ezeehere/)

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

-----

<div align="center"\>
<p>Made with ❤️ by the GDG On Campus Organizer JIST </p>
</div>
