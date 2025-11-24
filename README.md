# wastemangement
wg
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Eco Club — Waste Management Portal</title>

  <style>
    * { margin:0; padding:0; box-sizing:border-box; }

    /* 🌿 Animated Gradient Background */
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(120deg, #d4f8e8, #a7f3d0, #bbf7d0, #86efac);
      background-size: 400% 400%;
      animation: bgMove 12s ease-in-out infinite;
      padding: 30px;
      color:#111827;
      overflow-x: hidden;
      transition: 0.3s;
    }

    @keyframes bgMove {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    /* Dark Mode */
    body.dark {
      background: #0f172a;
      color: white;
    }
    body.dark .container { background: rgba(255,255,255,0.08); color:white; }
    body.dark input, body.dark textarea, body.dark select { background:#1e293b; color:white; border:1px solid #475569; }
    body.dark #list { background:#1e293b; border:1px solid #334155; }
    body.dark .btn { background:#16a34a; }

    /* Header Logo */
    .header {
      text-align:center;
      margin-bottom:20px;
      display:flex;
      flex-direction:column;
      align-items:center;
    }

    .header img {
      width:110px;
      margin-bottom:10px;
    }

    .container {
      max-width: 900px;
      margin: 0 auto;
      background: rgba(255,255,255,0.92);
      padding: 30px;
      border-radius: 18px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.15);
      backdrop-filter: blur(6px);
      position: relative;
      z-index: 1;
    }

    h1 { font-size:34px; margin-bottom:8px; color: #059669; }
    p { color:#4b5563; margin-bottom:24px; }

    .form-group { margin-bottom:18px; }
    label { display:block; font-weight:700; margin-bottom:8px; }

    input, textarea, select {
      width:100%; padding:12px; border-radius:10px;
      border:1px solid #d1d5db;
      font-size:16px;
      background: #f9fafb;
    }

    .btn {
      background:#059669;
      color:white;
      padding:14px 22px;
      border:none;
      border-radius:10px;
      font-size:18px;
      font-weight:600;
      cursor:pointer;
      transition:0.2s;
    }
    .btn:hover { background:#047857; }

    .status-box {
      margin-top:24px;
      padding:18px;
      border-radius:12px;
      background:#d1fae5;
      border:1px solid #34d399;
      color: #065f46;
      display:none;
      font-weight:600;
    }

    #list {
      margin-top:12px;
      padding:16px;
      border-radius:12px;
      background:#ecfdf5;
      border:1px solid #bbf7d0;
    }

    /* Search Bar */
    #search {
      width:100%;
      padding:12px;
      margin:15px 0;
      border-radius:10px;
      border:1px solid #94a3b8;
      font-size:16px;
    }

    /* Dark Mode Toggle */
    .toggle {
      position: fixed;
      right: 20px;
      top: 20px;
      padding: 10px 18px;
      background:#10b981;
      color:white;
      border-radius:8px;
      cursor:pointer;
    }
  </style>
</head>

<body>
  <div class="toggle" onclick="toggleDark()">🌙 Dark Mode</div>

  <div class="header">
    <img src="https://cdn-icons-png.flaticon.com/512/7615/7615994.png" />
    <h1>Eco Club — Waste Complaint Portal</h1>
    <p>Help keep our campus clean & green 🌱</p>
  </div>

  <div class="container">

    <input type="text" id="search" placeholder="🔍 Search by name, area, category..." onkeyup="filterComplaints()" />

    <div class="form-group">
      <label>Your Name</label>
      <input type="text" id="name" placeholder="Enter your name" />
    </div>

    <div class="form-group">
      <label>Location / Area</label>
      <input type="text" id="location" placeholder="Enter your location" />
    </div>

    <div class="form-group">
      <label>Complaint Category</label>
      <select id="category">
        <option>Garbage not collected</option>
        <option>Overflowing dustbin</option>
        <option>Roadside dumping</option>
        <option>Recycling request</option>
      </select>
    </div>

    <div class="form-group">
      <label>Description</label>
      <textarea id="desc" placeholder="Describe the issue"></textarea>
    </div>

    <button class="btn" onclick="submitForm()">Submit Complaint</button>

    <div id="status" class="status-box"></div>

    <h2 style="margin-top:22px;font-size:24px; color:#059669;">Recent Complaints</h2>
    <div id="list"><div>No complaints yet.</div></div>
  </div>

  <script>
    // DARK MODE
    function toggleDark(){ document.body.classList.toggle('dark'); }

    // SUBMIT COMPLAINT
    function submitForm(){
      const n = name.value.trim(), loc = location.value.trim(), cat = category.value, d = desc.value.trim();

      if(!n || !loc || !d){ alert("Please fill all fields"); return; }

      const data = { name:n, location:loc, category:cat, desc:d, time: new Date().toLocaleString() };

      let arr = JSON.parse(localStorage.getItem("complaints") || "[]");
      arr.unshift(data);
      localStorage.setItem("complaints", JSON.stringify(arr));

      name.value = location.value = desc.value = "";

      status.innerHTML = `✔ Complaint submitted successfully!`;
      status.style.display = 'block';

      loadComplaints();
    }

    // LOAD SAVED COMPLAINTS
    function loadComplaints(){
      const list = document.getElementById("list");
      let arr = JSON.parse(localStorage.getItem("complaints") || "[]");

      if(arr.length === 0){ list.innerHTML = "<div>No complaints yet.</div>"; return; }

      list.innerHTML = arr.map(c => `
        <div style='padding:10px 0; border-bottom:1px solid #bbf7d0;'>
          <b>${c.name}</b> — ${c.category}<br>
          <span style='font-size:14px;color:#6b7280;'>${c.location}</span><br>
          <span style='font-size:14px;'>${c.desc}</span><br>
          <span style='font-size:12px;color:#94a3b8;'>${c.time}</span>
        </div>`).join('');
    }

    // SEARCH FILTER
    function filterComplaints(){
      const q = search.value.toLowerCase();
      const items = list.querySelectorAll('div');

      items.forEach(i => {
        i.style.display = i.innerText.toLowerCase().includes(q) ? 'block' : 'none';
      });
    }

    loadComplaints();
  </script>
</body>
</html>
