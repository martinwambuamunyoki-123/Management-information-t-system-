<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>VETI ICT - Student Management</title>
<style>
  :root{--blue:#0b5ed7;--nav:#0a3b70;--muted:#f1f5f9;}
  body{font-family:Inter,Arial,Helvetica,sans-serif;margin:0;background:#f7fafc;color:#111}
  /* top bar */
  .topbar{background:var(--nav);color:#fff;padding:12px 16px;display:flex;align-items:center;gap:12px;position:relative}
  .hamburger{font-size:22px;cursor:pointer;user-select:none}
  .brand{font-weight:700;font-size:18px}
  /* sidebar (sliding) */
  .sidebar{position:fixed;left:-260px;top:0;height:100%;width:260px;background:#0f1724;color:#fff;padding-top:64px;transition:left .26s ease;z-index:40}
  .sidebar a{display:block;padding:12px 18px;text-decoration:none;color:#fff;border-bottom:1px solid rgba(255,255,255,.03)}
  .sidebar a:hover{background:#0b1220}
  /* main area */
  .wrap{padding:18px;max-width:1200px;margin:16px auto}
  .welcome{margin:12px 0;font-weight:600}
  .section{display:none;background:#fff;padding:14px;border-radius:8px;box-shadow:0 6px 18px rgba(2,6,23,.06)}
  .section.active{display:block}
  .form-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}
  @media(max-width:920px){.form-grid{grid-template-columns:1fr}}
  input,select{padding:8px;border:1px solid #d1d5db;border-radius:6px;width:100%;box-sizing:border-box}
  label{font-size:13px;color:#374151}
  .controls{margin-top:10px;display:flex;gap:8px;flex-wrap:wrap}
  .btn{background:var(--blue);color:#fff;border:none;padding:8px 12px;border-radius:6px;cursor:pointer}
  .btn.secondary{background:#6b7280}
  .search{margin:10px 0}
  .search input{width:50%;padding:8px;border-radius:6px;border:1px solid #cbd5e1}
  table{width:100%;border-collapse:collapse;margin-top:12px;background:#fff}
  th,td{border:1px solid #e6edf3;padding:8px;text-align:left;font-size:14px}
  th{background:#f1f5f9}
  .actions button{margin-right:6px}
  .top-actions{display:flex;gap:8px;flex-wrap:wrap;align-items:center;justify-content:space-between}
  .count{font-weight:700}
  /* small helpers */
  .muted{color:#6b7280}
  .center{text-align:center}
  .settings-box{max-width:520px;padding:12px;background:#fff;border-radius:8px;box-shadow:0 6px 18px rgba(2,6,23,.06)}
  footer{margin:24px;text-align:center;color:#6b7280;font-size:13px}
</style>
</head>
<body>

<!-- topbar -->
<div class="topbar">
  <div class="hamburger" id="hamburgerBtn" aria-label="menu">☰</div>
  <div class="brand">VETI ICT Department — Student Management</div>
</div>

<!-- sidebar -->
<nav id="sidebar" class="sidebar" aria-hidden="true">
  <a href="#" data-section="diploma">Diploma</a>
  <a href="#" data-section="certificate">Certificate</a>
  <a href="#" data-section="artisan">Artisan</a>
  <a href="#" data-section="settings">Settings</a>
  <a href="#" id="logoutLink">Logout</a>
</nav>

<!-- main wrapper -->
<div class="wrap" id="appWrap">

  <!-- login view -->
  <section id="loginSection" class="section active">
    <h2>Login</h2>
    <p class="muted"><strong>You are logging into VETI ICT DEPARTMENT</strong></p>
    <div style="max-width:520px;margin-top:8px">
      <label>Username</label>
      <input id="loginUser" placeholder="Username" value="">
      <label style="margin-top:8px">Password</label>
      <input id="loginPass" type="password" placeholder="Password">
      <div style="margin-top:8px;display:flex;align-items:center;gap:8px">
        <button class="btn" id="loginBtn">Login</button>
        <label class="muted"><input type="checkbox" id="showLoginPass"> Show</label>
      </div>
      <p id="loginMsg" class="muted"></p>
    </div>
  </section>

  <!-- diploma -->
  <section id="diploma" class="section">
    <h3>Diploma Students</h3>
    <div class="top-actions" style="margin-bottom:8px">
      <div>
        <button class="btn" onclick="focusAdd('diploma')">Add New</button>
        <button class="btn secondary" onclick="exportCSV('diploma')">Export CSV</button>
        <button class="btn secondary" onclick="printTable('diploma')">Print</button>
      </div>
      <div class="count">Total: <span id="diplomaCount">0</span></div>
    </div>

    <div class="form-grid" id="diplomaFormArea">
      <div>
        <label>Surname</label><input id="d_surname">
        <label>Date of Birth</label><input id="d_dob" type="date">
        <label>Year Joined</label><input id="d_yearJoined" type="number" min="1900" max="2100">
      </div>
      <div>
        <label>First Name</label><input id="d_firstname">
        <label>Semester</label>
        <select id="d_semester"></select>
        <label>Year of Study</label>
        <select id="d_yearStudy">
          <option>First Year</option><option>Second Year</option><option>Third Year</option>
        </select>
      </div>
      <div>
        <label>Other Name</label><input id="d_othername">
        <label>Admission No.</label><input id="d_admin">
        <label>Student Phone</label><input id="d_studentPhone">
        <label>Parent Phone</label><input id="d_parentPhone">
      </div>
    </div>
    <div style="margin-top:8px">
      <button class="btn" onclick="saveStudent('diploma')">Save</button>
      <button class="btn secondary" onclick="clearForm('diploma')">Clear</button>
      <input class="search" id="searchDiploma" placeholder="Search Diploma..." oninput="filterTable('diploma')" />
    </div>

    <table id="diplomaTable"><thead>
      <tr><th>#</th><th>Surname</th><th>First</th><th>Other</th><th>Adm No</th>
      <th>DOB</th><th>Semester</th><th>Year Joined</th><th>Year Study</th><th>Student Phone</th><th>Parent Phone</th><th>Action</th></tr>
    </thead><tbody></tbody></table>
  </section>

  <!-- certificate -->
  <section id="certificate" class="section">
    <h3>Certificate Students</h3>
    <div class="top-actions" style="margin-bottom:8px">
      <div>
        <button class="btn" onclick="focusAdd('certificate')">Add New</button>
        <button class="btn secondary" onclick="exportCSV('certificate')">Export CSV</button>
        <button class="btn secondary" onclick="printTable('certificate')">Print</button>
      </div>
      <div class="count">Total: <span id="certificateCount">0</span></div>
    </div>

    <div class="form-grid" id="certificateFormArea">
      <div>
        <label>Surname</label><input id="c_surname">
        <label>Date of Birth</label><input id="c_dob" type="date">
        <label>Year Joined</label><input id="c_yearJoined" type="number" min="1900" max="2100">
      </div>
      <div>
        <label>First Name</label><input id="c_firstname">
        <label>Semester</label><select id="c_semester"></select>
        <label>Year of Study</label>
        <select id="c_yearStudy"><option>First Year</option><option>Second Year</option><option>Third Year</option></select>
      </div>
      <div>
        <label>Other Name</label><input id="c_othername">
        <label>Admission No.</label><input id="c_admin">
        <label>Student Phone</label><input id="c_studentPhone">
        <label>Parent Phone</label><input id="c_parentPhone">
      </div>
    </div>
    <div style="margin-top:8px">
      <button class="btn" onclick="saveStudent('certificate')">Save</button>
      <button class="btn secondary" onclick="clearForm('certificate')">Clear</button>
      <input class="search" id="searchCertificate" placeholder="Search Certificate..." oninput="filterTable('certificate')" />
    </div>

    <table id="certificateTable"><thead>
      <tr><th>#</th><th>Surname</th><th>First</th><th>Other</th><th>Adm No</th>
      <th>DOB</th><th>Semester</th><th>Year Joined</th><th>Year Study</th><th>Student Phone</th><th>Parent Phone</th><th>Action</th></tr>
    </thead><tbody></tbody></table>
  </section>

  <!-- artisan -->
  <section id="artisan" class="section">
    <h3>Artisan Students</h3>
    <div class="top-actions" style="margin-bottom:8px">
      <div>
        <button class="btn" onclick="focusAdd('artisan')">Add New</button>
        <button class="btn secondary" onclick="exportCSV('artisan')">Export CSV</button>
        <button class="btn secondary" onclick="printTable('artisan')">Print</button>
      </div>
      <div class="count">Total: <span id="artisanCount">0</span></div>
    </div>

    <div class="form-grid" id="artisanFormArea">
      <div>
        <label>Surname</label><input id="a_surname">
        <label>Date of Birth</label><input id="a_dob" type="date">
        <label>Year Joined</label><input id="a_yearJoined" type="number" min="1900" max="2100">
      </div>
      <div>
        <label>First Name</label><input id="a_firstname">
        <label>Semester</label><select id="a_semester"></select>
        <label>Year of Study</label>
        <select id="a_yearStudy"><option>First Year</option><option>Second Year</option><option>Third Year</option></select>
      </div>
      <div>
        <label>Other Name</label><input id="a_othername">
        <label>Admission No.</label><input id="a_admin">
        <label>Student Phone</label><input id="a_studentPhone">
        <label>Parent Phone</label><input id="a_parentPhone">
      </div>
    </div>
    <div style="margin-top:8px">
      <button class="btn" onclick="saveStudent('artisan')">Save</button>
      <button class="btn secondary" onclick="clearForm('artisan')">Clear</button>
      <input class="search" id="searchArtisan" placeholder="Search Artisan..." oninput="filterTable('artisan')" />
    </div>

    <table id="artisanTable"><thead>
      <tr><th>#</th><th>Surname</th><th>First</th><th>Other</th><th>Adm No</th>
      <th>DOB</th><th>Semester</th><th>Year Joined</th><th>Year Study</th><th>Student Phone</th><th>Parent Phone</th><th>Action</th></tr>
    </thead><tbody></tbody></table>
  </section>

  <!-- settings -->
  <section id="settings" class="section">
    <h3>Settings</h3>
    <div class="settings-box">
      <p class="muted">Change password for the demo admin account (VETI).</p>
      <label>Current password (confirm):</label>
      <input id="currentPasswordConfirm" type="password">
      <label style="margin-top:8px">New password:</label>
      <input id="newPasswordSet" type="password">
      <div style="margin-top:8px">
        <button class="btn" onclick="setNewPassword()">Update Password</button>
        <button class="btn secondary" onclick="revealPassword()">View Current Password</button>
      </div>
      <p id="settingsMsg" class="muted"></p>
    </div>
  </section>

  <footer>VETI ICT Department • Demo portal (client-side storage)</footer>
</div>

<script>
/* ---------- Utilities & data model (localStorage) ---------- */
const STORAGE_KEY = "veti_student_data_v1";
const AUTH_KEY = "veti_auth_v1";

let data = {
  diploma: [],
  certificate: [],
  artisan: []
};
let auth = { username: "VETI", password: "ICT" };
let editing = { type: null, index: null };

/* load from localStorage if present */
function loadStorage(){
  try{
    const raw = localStorage.getItem(STORAGE_KEY);
    if(raw) data = JSON.parse(raw);
    const au = localStorage.getItem(AUTH_KEY);
    if(au) auth = JSON.parse(au);
  }catch(e){ console.error(e) }
}
function saveStorage(){
  localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
  localStorage.setItem(AUTH_KEY, JSON.stringify(auth));
}

/* ---------- UI init ---------- */
loadStorage();

/* populate semester selects (1..10) */
function populateSemesters(){
  ["d_semester","c_semester","a_semester"].forEach(id=>{
    const sel = document.getElementById(id);
    sel.innerHTML = "";
    for(let i=1;i<=10;i++){
      let o = document.createElement("option"); o.value = `Semester ${i}`; o.text = `Semester ${i}`;
      sel.appendChild(o);
    }
  });
}
populateSemesters();

/* show/hide sidebar (sliding) */
const sidebar = document.getElementById("sidebar");
document.getElementById("hamburgerBtn").addEventListener("click", ()=> {
  sidebar.style.left = sidebar.style.left === "0px" ? "-260px" : "0px";
});
/* close sidebar when clicking a link and show section */
sidebar.querySelectorAll("a[data-section]").forEach(a=>{
  a.addEventListener("click", e=>{
    e.preventDefault();
    const sec = a.getAttribute("data-section");
    showSection(sec);
    sidebar.style.left = "-260px";
  });
});
document.getElementById("logoutLink").addEventListener("click", e=>{
  e.preventDefault();
  logout();
});

/* ---------- Login ---------- */
function showSection(id){
  // hide login
  document.getElementById("loginSection").classList.remove("active");
  document.getElementById("loginSection").classList.add("hidden");
  // hide all sections
  ["diploma","certificate","artisan","settings"].forEach(s=>{
    document.getElementById(s).classList.remove("active");
    document.getElementById(s).classList.add("hidden");
  });
  // show target
  document.getElementById(id).classList.remove("hidden");
  document.getElementById(id).classList.add("active");
  updateAllTables();
}
function login(){
  const u = document.getElementById("loginUser").value.trim();
  const p = document.getElementById("loginPass").value;
  if(u === auth.username && p === auth.password){
    // hide login; show dashboard
    document.getElementById("loginSection").classList.add("hidden");
    document.getElementById("dashboard").classList.remove("hidden");
    // welcome text shown earlier always
    showSection("diploma");
  } else {
    document.getElementById("loginMsg").innerText = "Invalid username or password";
  }
}
document.getElementById("loginBtn").addEventListener("click", login);
document.getElementById("showLoginPass").addEventListener("change", function(){
  const p = document.getElementById("loginPass"); p.type = this.checked ? "text" : "password";
});

/* ---------- CRUD & rendering ---------- */
function getFormFields(prefix){
  return {
    surname: document.getElementById(prefix + "_surname").value.trim(),
    firstname: document.getElementById(prefix + "_firstname").value.trim(),
    othername: document.getElementById(prefix + "_othername").value.trim(),
    admin: document.getElementById(prefix + "_admin").value.trim(),
    dob: document.getElementById(prefix + "_dob") ? document.getElementById(prefix + "_dob").value : "",
    semester: document.getElementById(prefix + "_semester").value,
    yearJoined: document.getElementById(prefix + "_yearJoined").value,
    yearStudy: document.getElementById(prefix + "_yearStudy").value,
    studentPhone: document.getElementById(prefix + "_studentPhone").value.trim(),
    parentPhone: document.getElementById(prefix + "_parentPhone").value.trim()
  };
}
function setFormFields(prefix, obj){
  document.getElementById(prefix + "_surname").value = obj.surname || "";
  document.getElementById(prefix + "_firstname").value = obj.firstname || "";
  document.getElementById(prefix + "_othername").value = obj.othername || "";
  document.getElementById(prefix + "_admin").value = obj.admin || "";
  if(document.getElementById(prefix + "_dob")) document.getElementById(prefix + "_dob").value = obj.dob || "";
  document.getElementById(prefix + "_semester").value = obj.semester || `Semester 1`;
  document.getElementById(prefix + "_yearJoined").value = obj.yearJoined || "";
  document.getElementById(prefix + "_yearStudy").value = obj.yearStudy || "First Year";
  document.getElementById(prefix + "_studentPhone").value = obj.studentPhone || "";
  document.getElementById(prefix + "_parentPhone").value = obj.parentPhone || "";
}
function clearForm(type){
  const prefix = type[0];
  ["surname","firstname","othername","admin","dob","semester","yearJoined","yearStudy","studentPhone","parentPhone"].forEach(f=>{
    const el = document.getElementById(prefix + (f==="dob"?"_dob": (f==="firstname"? "_firstname": (f==="othername"?"_othername": (f==="studentPhone"?"_studentPhone": (f==="parentPhone"?"_parentPhone": (f==="yearJoined"? "_yearJoined": (f==="yearStudy"?"_yearStudy": (f==="admin"?"_admin": "_surname"))))))));
    if(el) {
      if(el.tagName === "SELECT") el.selectedIndex = 0;
      else el.value = "";
    }
  });
  editing.type = null; editing.index = null;
}

/* save student (add or update) */
function saveStudent(type){
  const prefix = type[0];
  const obj = {
    surname: document.getElementById(prefix + "_surname").value.trim(),
    firstname: document.getElementById(prefix + "_firstname").value.trim(),
    othername: document.getElementById(prefix + "_othername").value.trim(),
    admin: document.getElementById(prefix + "_admin").value.trim(),
    dob: document.getElementById(prefix + "_dob").value || "",
    semester: document.getElementById(prefix + "_semester").value,
    yearJoined: document.getElementById(prefix + "_yearJoined").value.trim(),
    yearStudy: document.getElementById(prefix + "_yearStudy").value,
    studentPhone: document.getElementById(prefix + "_studentPhone").value.trim(),
    parentPhone: document.getElementById(prefix + "_parentPhone").value.trim()
  };
  // validation minimal
  if(!obj.surname || !obj.firstname || !obj.admin || !obj.semester || !obj.yearJoined || !obj.yearStudy){
    alert("Please fill Surname, First name, Admission No., Semester, Year Joined and Year of Study.");
    return;
  }

  if(editing.type === type && editing.index !== null){
    data[type][editing.index] = obj;
    editing.type = null; editing.index = null;
  } else {
    data[type].push(obj);
  }
  saveStorage();
  renderTable(type);
  clearForm(type);
}

/* render table for a type */
function renderTable(type){
  const tbody = document.querySelector(`#${type}Table tbody`);
  tbody.innerHTML = "";
  data[type].forEach((s,i)=>{
    const tr = document.createElement("tr");
    tr.innerHTML = `<td>${i+1}</td>
      <td>${escapeHtml(s.surname)}</td>
      <td>${escapeHtml(s.firstname)}</td>
      <td>${escapeHtml(s.othername)}</td>
      <td>${escapeHtml(s.admin)}</td>
      <td>${escapeHtml(s.dob)}</td>
      <td>${escapeHtml(s.semester)}</td>
      <td>${escapeHtml(s.yearJoined)}</td>
      <td>${escapeHtml(s.yearStudy)}</td>
      <td>${escapeHtml(s.studentPhone)}</td>
      <td>${escapeHtml(s.parentPhone)}</td>
      <td class="actions">
        <button onclick="startEdit('${type}',${i})">Edit</button>
        <button onclick="deleteRow('${type}',${i})">Delete</button>
      </td>`;
    tbody.appendChild(tr);
  });
  document.getElementById(type + "Count").innerText = data[type].length;
  // keep any active search applied
  filterTable(type);
}

/* global helper to update all */
function updateAllTables(){ ["diploma","certificate","artisan"].forEach(renderTable) }

/* edit / delete */
function startEdit(type, index){
  const prefix = type[0];
  setFormFields(prefix, data[type][index]);
  editing.type = type; editing.index = index;
  window.scrollTo({ top: 0, behavior: "smooth" });
}
function deleteRow(type,index){
  if(!confirm("Delete this record?")) return;
  data[type].splice(index,1);
  saveStorage();
  renderTable(type);
}

/* filter table */
function filterTable(type){
  const query = document.getElementById("search" + capitalize(type)).value.toLowerCase().trim();
  const rows = document.querySelectorAll(`#${type}Table tbody tr`);
  rows.forEach(r=>{
    r.style.display = query === "" ? "" : (r.innerText.toLowerCase().includes(query) ? "" : "none");
  });
}

/* utilities */
function escapeHtml(s){ return s? String(s).replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;") : ""; }
function capitalize(s){ return s.charAt(0).toUpperCase()+s.slice(1) }

/* clear form helper for external buttons */
function focusAdd(type){ clearForm(type); window.scrollTo({top:0,behavior:"smooth"}); }

/* export CSV */
function exportCSV(type){
  if(data[type].length === 0){ alert("No records to export."); return; }
  const headers = ["No","Surname","First","Other","AdmNo","DOB","Semester","YearJoined","YearStudy","StudentPhone","ParentPhone"];
  const rows = data[type].map((r,i)=>[
    i+1, r.surname, r.firstname, r.othername, r.admin, r.dob, r.semester, r.yearJoined, r.yearStudy, r.studentPhone, r.parentPhone
  ]);
  const csv = [headers, ...rows].map(r => r.map(c => `"${String(c).replace(/"/g,'""')}"`).join(",")).join("\n");
  const blob = new Blob([csv], {type:"text/csv;charset=utf-8;"});
  const url = URL.createObjectURL(blob);
  const a = document.createElement("a");
  a.href = url; a.download = `${type}_students.csv`; document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
}

/* print table (open new window with table HTML) */
function printTable(type){
  const tableHtml = document.getElementById(type + "Table").outerHTML;
  const w = window.open("", "_blank");
  w.document.write(`<html><head><title>${capitalize(type)} Students</title></head><body><h2>${capitalize(type)} Students</h2>${tableHtml}</body></html>`);
  w.document.close(); w.focus(); setTimeout(()=>w.print(),300);
}

/* settings: change password */
function setNewPassword(){
  const confirmCur = document.getElementById("currentPasswordConfirm").value;
  const np = document.getElementById("newPasswordSet").value;
  if(confirmCur !== auth.password){ document.getElementById("settingsMsg").innerText = "Current password incorrect."; return; }
  if(!np){ document.getElementById("settingsMsg").innerText = "Enter new password."; return; }
  auth.password = np; saveStorage(); document.getElementById("settingsMsg").innerText = "Password updated."; document.getElementById("newPasswordSet").value=""; document.getElementById("currentPasswordConfirm").value="";
}
function revealPassword(){ alert("Current password: " + auth.password); }

/* delete helper */
function logout(){ document.getElementById("dashboard").classList.add("hidden"); document.getElementById("loginSection").classList.remove("hidden"); sidebar.style.left = "-260px"; }

/* initial render on page load */
(function init(){
  // if auth saved, keep it
  loadStorage();
  // show login view
  document.getElementById("loginSection").classList.remove("hidden");
  // init form id mapping convenience for save/clear helpers
  // map DOM ids used above for convenience: already present
  // render tables if data exists
  updateAllTables();
})();

</script>
</body>
</html>
