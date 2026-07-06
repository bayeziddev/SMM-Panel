# 🛠️ SMMSun Panel Dashboard - Implementation Guide

This guide explains the architecture and logic behind the **SMMSun SMM Panel User Dashboard**. It is designed to help developers (and AI assistants) understand, replicate, and customize the HTML, CSS (Tailwind), and JavaScript code.

---

## 1. Overview of the Page
This code represents the **"After Login" Home Page** for a user. It features a modern, mobile-friendly UI built with **Tailwind CSS**. 

**Core Features:**
* **User Details Profile:** Displays the logged-in user's name, total orders, active services, and account balance.
* **Platform Filtering:** Users can click on icons (Facebook, YouTube, TikTok, etc.) to filter services.
* **Dynamic Order Form:** The categories and services dropdowns automatically update based on the selected platform.
* **Real-time Pricing:** Automatically calculates the total charge based on the quantity entered.
* **Service Details:** Displays specific details (speed, drop ratio, guarantee) when a service is selected.

---

## 2. Structure Breakdown (HTML/UI)

The UI is divided into several clear sections:

### A. The Stat Cards (User Name & Details)
This is where the user's personal details are shown. In the final backend version, these hardcoded values will be replaced by dynamic database variables.
* **Username:** `<p id="val-username">syedshopan12</p>`
* **Total Orders:** `<p id="val-orders">3954841</p>`
* **Balance:** `<p id="val-balance">$0.52296</p>`

### B. Platform Grid
A set of buttons utilizing SVG icons from `api.iconify.design`. 
* **Class:** `.platform-btn`
* **Data Attribute:** `data-platform="Facebook"` (This attribute is crucial as the JavaScript uses it to filter the services).

### C. The Dynamic Order Form
Contains `<select>` dropdowns for Category and Service, and `<input>` fields for Link and Quantity. 
* **Charge Calculation:** The `<input id="charge-display">` is read-only and automatically filled by JavaScript.

---

## 3. How the JavaScript Logic Works

The JavaScript handles all the interactivity without needing to reload the page. Here is how it functions step-by-step:

### Step 1: The Database (`serviceData`)
All services are stored in an array of objects. Each object contains the details needed to populate the form:
```javascript
{
    cat: "YouTube Views | Cheapest", // Category name
    id: 970,                         // Unique Service ID
    name: "YouTube Views [Instant]...", // Display Name
    rate: 0.58,                      // Price per 1000 units
    min: 100, max: 25000,            // Min/Max order quantity
    platform: "YouTube",             // Used for filtering
    // ... other details (speed, guarantee, etc.)
}

```
### Step 2: Filtering by Platform
When a user clicks a platform icon (e.g., YouTube), the script updates the currentPlatform variable and runs populateCategories(). It filters the serviceData array so only YouTube services show up in the dropdown.
### Step 3: Cascading Dropdowns
 1. **populateCategories()**: Gets all unique categories for the chosen platform and fills the first dropdown.
 2. **populateServices()**: Looks at the chosen category and fills the second dropdown with matching services.
 3. **onServiceChange()**: When a specific service is picked, it unhides the #svc-details section and fills in the text (Start Time, Drop Ratio, Guarantee, etc.).
### Step 4: Price Calculation (updateCharge)
The math is simple: SMM panels price things per 1,000 units.
```javascript
// Formula used in the code:
let totalCharge = (selectedService.rate * quantity) / 1000;

```
This runs every time the user types a number into the Quantity box.
### Step 5: Submitting the Order
When the form is submitted, the code:
 1. Validates the input (checks if a link exists and if the quantity is within min/max limits).
 2. Prevents default page reload (e.preventDefault()).
 3. Sends the data to the backend via an SDK (window.dataSdk.create).
 4. Shows a success or error message.
## 4. How to Make it "Final Version" Ready (Dynamic User Data)
Right now, the username and balance are hardcoded. To make this functional for real users, you need to bind these IDs to your backend database (PHP, Node.js, Python, or Firebase).
**What to tell your AI/Developer for the next step:**
> *"I need to connect this HTML to my backend. When the user logs in, please fetch their data and inject it into id="val-username", id="val-balance", and id="val-orders"."*
> 
**Example of how dynamic injection looks in typical JavaScript:**
```javascript
// Fetch user data from your API
fetch('/api/user/details')
  .then(response => response.json())
  .then(userData => {
      document.getElementById('val-username').innerText = userData.username;
      document.getElementById('val-balance').innerText = '$' + userData.balance.toFixed(5);
      document.getElementById('val-orders').innerText = userData.total_orders;
  });

```
## 5. Summary of Tailwind Classes Used
If you want to change colors or styling, look for these utility classes in the code:
 * bg-orange-600 / text-orange-600: The primary brand color used for borders, buttons, and highlights.
 * rounded-xl / rounded-2xl: Gives the cards and inputs their modern, rounded corners.
 * shadow: Adds depth to the white cards against the gray background.
 * hidden: Used to hide elements (like the service details box or success messages) until JavaScript removes the class.
```

```
Manus Ai design add all service like this fromat 
<!doctype html>
<html lang="en">
 <head><script>window["__codeletBootstrap__"]=JSON.parse('{"A":"A","B":"20260706-05-21290da"}');</script><script src="/_sdk/f432b76fb310b513.telemetry_sdk.js" integrity="sha512-9FJQ55EKmmqhyyik8BhrMVfm0+iYYfb8uOlRsjtkxJMEI97qOVEI4PnHohDjQAtvAep0IOEpdQ64eki0sz62ug=="></script>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SMMSun Panel</title>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700;800&amp;display=swap" rel="stylesheet">
  <style>
        body { font-family: 'DM Sans', sans-serif; }
        .platform-btn { width: 44px; height: 44px; border-radius: 12px; display: flex; align-items: center; justify-content: center; cursor: pointer; background: white; border: 1px solid #f3f4f6; box-shadow: 0 1px 3px rgba(0,0,0,0.06); transition: all 0.15s; }
        .platform-btn:hover { box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
        .platform-btn.active { background: #6A8E23; border-color: transparent; box-shadow: 0 4px 12px rgba(106,142,35,0.3); }
        .platform-btn img { width: 24px; height: 24px; }
    </style>
  <script src="https://cdn.jsdelivr.net/npm/lucide@0.263.0/dist/umd/lucide.min.js" type="text/javascript"></script>
  <script src="/_sdk/0b3af1c300e720aa.resizing_sdk.js" type="text/javascript" integrity="sha512-nX1P6oXZmEY5wCI2lUVg3BwKu1cr0yi+ZpXgZ0Yn90vkSwvpmyzBMvOyD6VLJUtBwFI2dSrw1b6OvORxSggw3w=="></script>
 </head>
 <body data-template-id="__page-root" class="w-full min-h-screen pb-20"><!-- Welcome Banner -->
  <div data-template-id="welcome-banner" class="canva-section mx-4 mt-4 rounded-2xl p-5 text-white">
   <h2 data-template-id="welcome-title" class="canva-text font-bold text-xl mb-1"></h2>
   <p data-template-id="welcome-subtitle" class="canva-text text-sm opacity-90"></p>
  </div><!-- Giveaway Card -->
  <div data-template-id="giveaway-card" class="canva-card mx-4 mt-4 rounded-2xl p-4 text-white flex items-center justify-between">
   <div class="flex-1">
    <h3 data-template-id="giveaway-title" class="canva-text font-bold text-lg mb-1"></h3>
    <p data-template-id="giveaway-desc" class="canva-text text-xs opacity-90"></p>
   </div><img data-template-id="giveaway-icon" class="canva-image w-14 h-14 object-contain" loading="lazy">
  </div><!-- 2FA Notice -->
  <div class="mx-4 mt-4 bg-yellow-50 border border-yellow-200 rounded-xl p-3 flex items-center gap-2"><span class="text-lg">🔒</span>
   <p data-template-id="twofa-notice" class="canva-text text-sm font-semibold flex-1"></p>
  </div><!-- Stat Cards -->
  <div class="grid grid-cols-2 gap-3 mx-4 mt-4">
   <div class="bg-white rounded-xl shadow p-4 text-center border-l-4 border-purple-600"><span class="text-2xl">👤</span>
    <p data-template-id="stat-username-label" class="canva-text text-xs font-semibold text-gray-500 mt-1"></p>
    <p class="text-base font-bold text-gray-800" id="val-username">syedshopan12</p>
   </div>
   <div class="bg-white rounded-xl shadow p-4 text-center border-l-4 border-orange-600"><span class="text-2xl">📦</span>
    <p data-template-id="stat-orders-label" class="canva-text text-xs font-semibold text-gray-500 mt-1"></p>
    <p class="text-base font-bold text-gray-800" id="val-orders">3954841</p>
   </div>
   <div class="bg-white rounded-xl shadow p-4 text-center border-l-4 border-cyan-600"><span class="text-2xl">⚙️</span>
    <p data-template-id="stat-services-label" class="canva-text text-xs font-semibold text-gray-500 mt-1"></p>
    <p class="text-base font-bold text-gray-800" id="val-services">4217</p>
   </div>
   <div class="bg-white rounded-xl shadow p-4 text-center border-l-4 border-green-600"><span class="text-2xl">💰</span>
    <p data-template-id="stat-balance-label" class="canva-text text-xs font-semibold text-gray-500 mt-1"></p>
    <p class="text-base font-bold text-gray-800" id="val-balance">$0.52296</p>
   </div>
  </div><!-- Platform Icons -->
  <div class="mx-4 mt-4 bg-white rounded-2xl shadow p-3">
   <div id="platform-grid" class="flex flex-wrap gap-2 justify-center"><button type="button" class="platform-btn active" data-platform="Facebook" title="Facebook"><img src="https://api.iconify.design/logos:facebook.svg" alt="Facebook"></button> <button type="button" class="platform-btn" data-platform="YouTube" title="YouTube"><img src="https://api.iconify.design/logos:youtube-icon.svg" alt="YouTube"></button> <button type="button" class="platform-btn" data-platform="Instagram" title="Instagram"><img src="https://api.iconify.design/skill-icons:instagram.svg" alt="Instagram"></button> <button type="button" class="platform-btn" data-platform="TikTok" title="TikTok"><img src="https://api.iconify.design/logos:tiktok-icon.svg" alt="TikTok"></button> <button type="button" class="platform-btn" data-platform="Telegram" title="Telegram"><img src="https://api.iconify.design/logos:telegram.svg" alt="Telegram"></button> <button type="button" class="platform-btn" data-platform="Twitter" title="Twitter"><img src="https://api.iconify.design/logos:twitter.svg" alt="Twitter"></button> <button type="button" class="platform-btn" data-platform="LinkedIn" title="LinkedIn"><img src="https://api.iconify.design/logos:linkedin-icon.svg" alt="LinkedIn"></button> <button type="button" class="platform-btn" data-platform="Discord" title="Discord"><img src="https://api.iconify.design/logos:discord-icon.svg" alt="Discord"></button> <button type="button" class="platform-btn" data-platform="Traffic" title="Traffic"><img src="https://api.iconify.design/mdi:cloud.svg" alt="Traffic"></button>
   </div>
  </div><!-- New Order / Add Funds buttons -->
  <div class="flex gap-3 mx-4 mt-4"><button data-template-id="btn-new-order" class="canva-button flex-1 py-3 rounded-xl font-semibold text-sm border-2 border-orange-500 bg-white text-orange-600 flex items-center justify-center gap-2"></button> <button data-template-id="btn-add-funds" class="canva-button flex-1 py-3 rounded-xl font-semibold text-sm text-white flex items-center justify-center gap-2"></button>
  </div><!-- Search -->
  <div class="mx-4 mt-4"><input id="search-input" type="text" placeholder="🔍 Search" class="w-full border border-gray-200 rounded-xl p-3 text-sm focus:outline-none focus:ring-2 focus:ring-orange-400">
  </div><!-- Order Form -->
  <form id="order-form" class="mx-4 mt-4 space-y-4">
   <div><label data-template-id="label-category" class="canva-text block text-sm font-semibold mb-1" for="cat-select"></label> <select id="cat-select" class="w-full border border-gray-200 rounded-xl p-3 text-sm bg-white focus:outline-none focus:ring-2 focus:ring-orange-400"></select>
   </div>
   <div><label data-template-id="label-service" class="canva-text block text-sm font-semibold mb-1" for="svc-select"></label> <select id="svc-select" class="w-full border border-gray-200 rounded-xl p-3 text-sm bg-white focus:outline-none focus:ring-2 focus:ring-orange-400"></select>
   </div>
   <div><label data-template-id="label-link" class="canva-text block text-sm font-semibold mb-1" for="link-input"></label> <input id="link-input" type="url" class="w-full border border-gray-200 rounded-xl p-3 text-sm focus:outline-none focus:ring-2 focus:ring-orange-400" placeholder="https://...">
   </div>
   <div><label data-template-id="label-quantity" class="canva-text block text-sm font-semibold mb-1" for="qty-input"></label> <input id="qty-input" type="number" min="1" class="w-full border border-gray-200 rounded-xl p-3 text-sm focus:outline-none focus:ring-2 focus:ring-orange-400" placeholder="Enter quantity">
   </div>
   <div><label data-template-id="label-avg-time" class="canva-text block text-sm font-semibold mb-1"></label> <input id="avg-time" type="text" readonly class="w-full border rounded-xl p-3 text-sm bg-gray-50" value="—">
   </div>
   <div><label data-template-id="label-charge" class="canva-text block text-sm font-semibold mb-1"></label> <input id="charge-display" type="text" readonly class="w-full border rounded-xl p-3 text-sm bg-gray-50" value="$0.00">
   </div><button data-template-id="submit-btn" type="submit" class="canva-button w-full py-3 rounded-xl font-bold text-white text-base"></button>
  </form>
  <div id="form-msg" class="mx-4 mt-2 text-sm text-center hidden"></div><!-- Service Details -->
  <section id="svc-details" class="mx-4 mt-6 bg-white rounded-2xl shadow p-5 hidden">
   <h3 data-template-id="details-heading" class="canva-text font-bold text-lg mb-4 text-orange-600"></h3>
   <div class="space-y-3 text-sm">
    <div>
     <span class="font-semibold text-orange-600">Link Format</span>
     <p id="det-link-format" class="border rounded-lg p-2 mt-1 text-gray-700 bg-gray-50"></p>
    </div>
    <div class="grid grid-cols-2 gap-3">
     <div>
      <span class="font-semibold text-orange-600">Start Time</span>
      <p id="det-start" class="border rounded-lg p-2 mt-1 text-gray-700 bg-gray-50"></p>
     </div>
     <div>
      <span class="font-semibold text-orange-600">Daily Speed</span>
      <p id="det-speed" class="border rounded-lg p-2 mt-1 text-gray-700 bg-gray-50"></p>
     </div>
    </div>
    <div class="grid grid-cols-2 gap-3">
     <div>
      <span class="font-semibold text-orange-600">Drop Ratio</span>
      <p id="det-drop" class="border rounded-lg p-2 mt-1 text-gray-700 bg-gray-50"></p>
     </div>
     <div>
      <span class="font-semibold text-orange-600">Quality of Services</span>
      <p id="det-quality" class="border rounded-lg p-2 mt-1 text-gray-700 bg-gray-50"></p>
     </div>
    </div>
    <div>
     <span class="font-semibold text-orange-600">Guarantee</span>
     <p id="det-guarantee" class="border rounded-lg p-2 mt-1 text-gray-700 bg-gray-50"></p>
    </div>
    <div>
     <span class="font-semibold text-orange-600">Services Details</span>
     <p id="det-desc" class="border rounded-lg p-2 mt-1 text-gray-700 bg-gray-50"></p>
    </div>
   </div>
  </section><!-- WhatsApp FAB --> <a href="https://wa.me/message/TDYG575YENF6F1" target="_blank" rel="noopener noreferrer" class="fixed bottom-6 right-6 z-50 bg-[#25D366] hover:bg-[#1da851] text-white p-3 rounded-full shadow-lg"><img src="https://api.iconify.design/logos:whatsapp-icon.svg" class="w-7 h-7" alt="WhatsApp"></a>
  <script src="/_sdk/e063511df281944a.editing_sdk.js" integrity="sha512-Xa8p4aPfoMxVnzxQE2WAG0FfoaPLxdjOJMfUZGm1yZosJTYmnNDvgLkJ2EoN0i3zxjeQVAzUMSXNvTKQVqbXyQ=="></script>
  <script src="/_sdk/0179d719c431a79e.data_sdk.js" integrity="sha512-OsRvJkI5GkC3MJQcgotQYotpzrMSX415GiKKgvYdxQEEkzD/fdchShO2FMSy0PJr/tiINMXOk5Vl2zAgg6bUiA=="></script>
  <script>
const serviceData = [
{cat:"YouTube Views | Cheapest",id:970,name:"YouTube Views [Instant][500/Day][Non Drop] 30 Days Refill ♻️",rate:0.58,min:100,max:25000,time:"2h 55m",platform:"YouTube",linkFormat:"https://www.youtube.com/video_link",start:"Instant",speed:"300-500/Day",drop:"5-10%",quality:"Good Quality",guarantee:"30 Days Refill ♻️",desc:"Real YouTube views with non-drop guarantee."},
{cat:"YouTube Views | Cheapest",id:976,name:"YouTube Views [Instant][1K/Day][Non Drop] Lifetime 💚",rate:0.73,min:1000,max:10000000,time:"109h 37m",platform:"YouTube",linkFormat:"https://www.youtube.com/video_link",start:"Instant",speed:"1K/Day",drop:"0-5%",quality:"High Quality",guarantee:"Lifetime ❤️",desc:"Lifetime guaranteed YouTube views."},
{cat:"YouTube Views | Cheapest",id:973,name:"YouTube Views [Instant][20K/Day][Non Drop] Lifetime 💚",rate:0.88,min:100,max:150000,time:"8h 36m",platform:"YouTube",linkFormat:"https://www.youtube.com/video_link",start:"Instant",speed:"20K/Day",drop:"0-5%",quality:"High Quality",guarantee:"Lifetime ❤️",desc:"Fast delivery lifetime views."},
{cat:"YouTube Video Likes | Best 💎",id:202,name:"YouTube Likes [Instant][5K/Day][100% Non Drop] 30 Days Refill ♻️",rate:1.22,min:10,max:1000,time:"1h 50m",platform:"YouTube",linkFormat:"https://www.youtube.com/video_link",start:"Instant",speed:"5K/Day",drop:"0%",quality:"Premium",guarantee:"30 Days Refill ♻️",desc:"100% non-drop YouTube likes."},
{cat:"Facebook Followers | Best 💎",id:3156,name:"FB Page/Profile Followers [Instant][100K/Day][HQ] No Refill",rate:0.22,min:100,max:100000,time:"23m",platform:"Facebook",linkFormat:"https://www.facebook.com/page_or_profile",start:"Instant",speed:"100K/Day",drop:"10-20%",quality:"High Quality",guarantee:"No Refill",desc:"Fast Facebook followers."},
{cat:"Facebook Followers | Best 💎",id:1150,name:"FB Page/Profile Followers [Instant][100K/Day][HQ] Lifetime ❤️",rate:0.92,min:10,max:500000,time:"14m",platform:"Facebook",linkFormat:"https://www.facebook.com/page_or_profile",start:"Instant",speed:"100K/Day",drop:"0-5%",quality:"High Quality",guarantee:"Lifetime ❤️",desc:"Lifetime Facebook followers."},
{cat:"Facebook Post Likes 👍",id:1241,name:"FB Post Likes [👍][Instant][50K/Day] No Refill",rate:0.26,min:10,max:100000,time:"32m",platform:"Facebook",linkFormat:"https://www.facebook.com/post_link",start:"Instant",speed:"50K/Day",drop:"5-10%",quality:"Good Quality",guarantee:"No Refill",desc:"Facebook post likes."},
{cat:"Instagram Followers | Best 💎",id:1752,name:"IG Followers [Instant][50K/Day][Low Drop] No Refill",rate:0.33,min:100,max:10000,time:"2h 40m",platform:"Instagram",linkFormat:"https://www.instagram.com/username",start:"Instant",speed:"50K/Day",drop:"10-15%",quality:"Good Quality",guarantee:"No Refill",desc:"Instagram followers."},
{cat:"Instagram Likes | Best 💎",id:621,name:"IG Likes [Instant][100K/Day][100% Real] No Refill",rate:0.078,min:100,max:10000,time:"9m",platform:"Instagram",linkFormat:"https://www.instagram.com/p/post_link",start:"Instant",speed:"100K/Day",drop:"0%",quality:"Premium",guarantee:"No Refill",desc:"Real Instagram likes."},
{cat:"TikTok Views | Best 💎",id:3315,name:"TikTok Views [Instant][10M/Day][Non Drop] Unlimited",rate:0.0132,min:100,max:100000000,time:"3m",platform:"TikTok",linkFormat:"https://www.tiktok.com/@user/video/id",start:"Instant",speed:"10M/Day",drop:"0%",quality:"High Quality",guarantee:"Unlimited",desc:"TikTok views instant delivery."},
{cat:"TikTok Likes | Best 💎",id:2192,name:"TikTok Likes [Instant][50K/Day][HQ][Non-Drop] 30 Days ♻️",rate:0.28,min:10,max:1000000,time:"2h 1m",platform:"TikTok",linkFormat:"https://www.tiktok.com/@user/video/id",start:"Instant",speed:"50K/Day",drop:"0-5%",quality:"High Quality",guarantee:"30 Days ♻️",desc:"TikTok likes with refill."},
{cat:"Telegram Members | Best 💎",id:3602,name:"Telegram Members Channel/Group [Instant][50K/Days] 30 Days ♻️",rate:0.56,min:100,max:50000,time:"17m",platform:"Telegram",linkFormat:"https://t.me/channel_or_group",start:"Instant",speed:"50K/Day",drop:"5-10%",quality:"Good Quality",guarantee:"30 Days ♻️",desc:"Telegram members."},
{cat:"Twitter Followers | Best 💎",id:900,name:"Twitter Followers [Instant][20K/Day][HQ] No Refill",rate:3.65,min:10,max:1000000,time:"13m",platform:"Twitter",linkFormat:"https://twitter.com/username",start:"Instant",speed:"20K/Day",drop:"10-20%",quality:"High Quality",guarantee:"No Refill",desc:"Twitter followers."},
{cat:"LinkedIn Followers",id:2927,name:"LinkedIn Profile/Company Followers [200/Day] No Refill",rate:20.51,min:50,max:10000,time:"New 🌀",platform:"LinkedIn",linkFormat:"https://www.linkedin.com/in/username",start:"0-6H",speed:"200/Day",drop:"5-10%",quality:"Good Quality",guarantee:"No Refill",desc:"LinkedIn followers."},
{cat:"Discord Members",id:868,name:"Discord Members [5K/Day][0-24H]",rate:2.04,min:20,max:1500,time:"1h 25m",platform:"Discord",linkFormat:"https://discord.gg/invite_link",start:"0-24H",speed:"5K/Day",drop:"10-20%",quality:"Good Quality",guarantee:"No Refill",desc:"Discord server members."},
{cat:"Website Traffic | Direct",id:801,name:"Website Traffic [100K/Day][Direct Visits] No Referrer⚡",rate:0.26,min:100,max:10000000,time:"7m",platform:"Traffic",linkFormat:"https://yourwebsite.com",start:"Instant",speed:"100K/Day",drop:"0%",quality:"Good Quality",guarantee:"No Refill",desc:"Direct website traffic."}
];

let currentPlatform = 'Facebook';
let selectedService = null;
let allData = [];

// Platform buttons
document.querySelectorAll('#platform-grid .platform-btn').forEach(btn => {
    btn.addEventListener('click', function() {
        document.querySelectorAll('#platform-grid .platform-btn').forEach(b => b.classList.remove('active'));
        this.classList.add('active');
        currentPlatform = this.dataset.platform;
        populateCategories();
    });
});

function getFiltered(filter) {
    return serviceData.filter(s => {
        const pMatch = s.platform === currentPlatform;
        const sMatch = !filter || s.name.toLowerCase().includes(filter.toLowerCase()) || String(s.id).includes(filter);
        return pMatch && sMatch;
    });
}

function populateCategories() {
    const filter = document.getElementById('search-input').value.trim();
    const filtered = getFiltered(filter);
    const cats = [...new Set(filtered.map(s => s.cat))];
    const catSel = document.getElementById('cat-select');
    catSel.innerHTML = cats.map(c => `<option value="${c}">${c}</option>`).join('');
    populateServices();
}

function populateServices() {
    const filter = document.getElementById('search-input').value.trim();
    const filtered = getFiltered(filter);
    const cat = document.getElementById('cat-select').value;
    const svcs = filtered.filter(s => s.cat === cat);
    const svcSel = document.getElementById('svc-select');
    svcSel.innerHTML = svcs.map(s => `<option value="${s.id}">${s.id} - ${s.name}</option>`).join('');
    onServiceChange();
}

function onServiceChange() {
    const id = parseInt(document.getElementById('svc-select').value);
    selectedService = serviceData.find(s => s.id === id) || null;
    if (selectedService) {
        document.getElementById('avg-time').value = selectedService.time;
        document.getElementById('svc-details').classList.remove('hidden');
        document.getElementById('det-link-format').textContent = selectedService.linkFormat;
        document.getElementById('det-start').textContent = selectedService.start;
        document.getElementById('det-speed').textContent = selectedService.speed;
        document.getElementById('det-drop').textContent = selectedService.drop;
        document.getElementById('det-quality').textContent = selectedService.quality;
        document.getElementById('det-guarantee').textContent = selectedService.guarantee;
        document.getElementById('det-desc').textContent = selectedService.desc;
    } else {
        document.getElementById('avg-time').value = '—';
        document.getElementById('svc-details').classList.add('hidden');
    }
    updateCharge();
}

function updateCharge() {
    const qty = parseInt(document.getElementById('qty-input').value) || 0;
    if (selectedService && qty > 0) {
        document.getElementById('charge-display').value = '$' + ((selectedService.rate * qty) / 1000).toFixed(4);
    } else {
        document.getElementById('charge-display').value = '$0.00';
    }
}

document.getElementById('cat-select').addEventListener('change', populateServices);
document.getElementById('svc-select').addEventListener('change', onServiceChange);
document.getElementById('qty-input').addEventListener('input', updateCharge);
document.getElementById('search-input').addEventListener('input', () => { populateCategories(); });

// Order submit
document.getElementById('order-form').addEventListener('submit', async e => {
    e.preventDefault();
    if (!selectedService) return;
    if (allData.length >= 999) { showMsg('Max 999 records reached.', true); return; }
    const link = document.getElementById('link-input').value.trim();
    const qty = parseInt(document.getElementById('qty-input').value) || 0;
    if (!link) { showMsg('Enter a link.', true); return; }
    if (qty < selectedService.min || qty > selectedService.max) { showMsg(`Quantity must be ${selectedService.min}-${selectedService.max}`, true); return; }
    const btn = document.querySelector('#order-form button[type="submit"]');
    btn.disabled = true; btn.style.opacity = '0.6';
    const result = await window.dataSdk.create({
        order_id: 'ORD-' + Date.now(), service_category: currentPlatform, service_name: selectedService.id + ' - ' + selectedService.name,
        link, quantity: qty, charge: document.getElementById('charge-display').value, status: 'Pending', created_at: new Date().toISOString(),
        client_name: '', deposit_paid: false, start_count: 0, remains: qty, deposit_id: '', deposit_amount: '', deposit_method: '', deposit_trx_id: '', deposit_status: '',
        proof_id: '', proof_platform: '', proof_link: '', proof_name: '', proof_status: ''
    });
    btn.disabled = false; btn.style.opacity = '1';
    if (result.isOk) { showMsg('✅ Order submitted successfully!', false); document.getElementById('order-form').reset(); document.getElementById('charge-display').value = '$0.00'; }
    else showMsg('Failed to submit order.', true);
});

function showMsg(t, err) {
    const m = document.getElementById('form-msg');
    m.textContent = t; m.className = 'mx-4 mt-2 text-sm text-center ' + (err ? 'text-red-600' : 'text-green-600');
    m.classList.remove('hidden'); setTimeout(() => m.classList.add('hidden'), 4000);
}

const handler = { onDataChanged(data) { allData = data; } };
(async () => { await window.dataSdk.init(handler); })();

populateCategories();
</script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'a16e52dc027d1645',t:'MTc4MzMzNzkxMi4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
