# 🍽️ Canteen Menu Web App

A dynamic, foodie-themed canteen menu website created using **Vue 3**. This project fetches live menu data (including names, categories, images, prices, and availability) from a **Google Sheet backend** using a **Google Apps Script API**. It uses bright **orange and chocolate themes**, and features a **responsive layout**.

---

## 🎯 Task Objective

Build a fully responsive canteen web application with the following specifications:

- Pull menu items dynamically from a Google Sheet (using Apps Script).
- Display items with **images** stored locally in the `public/menu_images` folder.
- Visually engaging **foodie UI theme** using orange and chocolate tones.
- Make the layout mobile-first and fully responsive for all screen sizes.

---

## 🛠️ Setup Instructions

### 1. 📦 Prerequisites

Ensure you have the following installed:

- Node.js (v16 or higher)
- npm (comes with Node.js)
- Vue CLI or Vite (if not, we’ll install it below)

---

### 2. ⬇️ Clone the Project

git clone https://github.com/your-username/canteen-menu-app.git
cd canteen-menu-app


### 3. 📥 Install Dependencies
npm install

### 4. 📁 Add Menu Item Images
Place all food images inside the public/menu_images/ folder.

✅ Image file names must match the item_image field in your Google Sheet (e.g., lemon.jpg).

### 5. 🔗 Set Up Google Sheets as Backend
Create a Google Sheet with this structure:
```
item	item_image	category	price	availability
Lemon	lemon.jpg	Juice	20	Yes
Milk Tea	milktea.jpg	Tea	10	Yes


Go to Extensions > Apps Script and paste:

function doGet() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Sheet1");
  const data = sheet.getDataRange().getValues();
  const headers = data.shift();
  const result = data.map(row => {
    let item = {};
    row.forEach((value, index) => {
      item[headers[index]] = value;
    });
    return item;
  });
  return ContentService.createTextOutput(JSON.stringify(result)).setMimeType(ContentService.MimeType.JSON);
}
Deploy as a Web App:

Go to Deploy > New Deployment

Select doGet as function to run

Access: Anyone

Click Deploy, then copy the web app URL

Replace the fetch URL in Menu.vue:


const response = await fetch('https://script.google.com/macros/s/PASTE_YOUR_DEPLOYMENT_URL_HERE/exec');\

```
### 6. 🚀 Run the App Locally
npm run dev

Then visit:
📍 http://localhost:5173

### 7. 🧊 Build for Production
npm run build

### ✨ Features
✅ Dynamic Menu – Fetched live from Google Sheet
🖼️ Images – Displayed from local public/menu_images/ folder
🎨 Foodie Theme – Stylish UI with orange & chocolate tones
📱 Responsive – Mobile-first design; adapts to all screens
📊 Sorted by Category – Like Juice, Tea, Snacks, etc.

### 🔧 Tech Stack
Vue 3
Vite
Google Sheets API (Apps Script)
HTML + CSS (with animations)
JSON data structure


### 🙌 Author
Ayush
 -Student • Developer • Innovator
📬 Open for collaboration and improvements!

### 📜 License
This project is open source and free to use. Modify it as per your college, café, or small business needs.
