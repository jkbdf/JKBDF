<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>যুব কল্যাণ রক্তদান ফাউন্ডেশন</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Hind Siliguri', sans-serif; background-color: #f1f5f9; }
        .hero-gradient { background: linear-gradient(135deg, #dc2626 0%, #991b1b 100%); }
        .container-custom { width: 100%; padding: 0 15px; max-width: 500px; margin: 0 auto; }
        .info-row { display: flex; border-bottom: 1px solid rgba(0,0,0,0.05); padding: 8px 0; align-items: center; }
        .info-label { font-weight: 800; color: #4b5563; min-width: 115px; font-size: 14px; }
        .info-value { font-weight: 700; font-size: 14px; flex: 1; }
        
        .card-0 { border-top-color: #dc2626; } .card-1 { border-top-color: #2563eb; }
        .card-2 { border-top-color: #059669; } .card-3 { border-top-color: #7c3aed; }
        .card-4 { border-top-color: #db2777; }
        
        .sl-0 { background-color: #fee2e2; color: #dc2626; }
        .sl-1 { background-color: #dbeafe; color: #2563eb; }
        .sl-2 { background-color: #d1fae5; color: #059669; }
        .sl-3 { background-color: #f3e8ff; color: #7c3aed; }
        .sl-4 { background-color: #fce7f3; color: #db2777; }
        .text-red-custom { color: #dc2626; }
    </style>
</head>
<body class="pb-10">

    <div class="bg-white p-5 shadow-md text-center flex flex-col items-center mb-4 border-b-2 border-red-50">
        <img src="https://i.ibb.co/C3m2X9Y/1000001730.png" class="w-20 h-20 mb-2 rounded-full border-4 border-red-50 shadow-lg">
        <h1 class="text-xl font-black text-red-600">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
    </div>

    <div id="loginPage" class="container-custom">
        <div class="bg-white p-6 rounded-[35px] shadow-xl text-center border border-gray-100">
            <h2 class="text-lg font-bold text-gray-800 mb-5">লগইন করুন</h2>
            
            <div class="flex gap-2 mb-4 p-1 bg-gray-100 rounded-xl">
                <button onclick="setRole('Member')" id="roleMember" class="flex-1 py-2 rounded-lg text-xs font-bold bg-white text-red-600 shadow-sm">সদস্য</button>
                <button onclick="setRole('Admin')" id="roleAdmin" class="flex-1 py-2 rounded-lg text-xs font-bold text-gray-500">এডমিন</button>
            </div>

            <div id="memberFields">
                <input type="tel" id="uPhone" placeholder="মোবাইল নম্বর" class="w-full p-4 mb-4 border rounded-2xl text-center font-bold outline-none focus:border-red-500 bg-gray-50">
            </div>
            <div id="adminFields" class="hidden">
                <input type="password" id="uPass" placeholder="এডমিন পাসওয়ার্ড" class="w-full p-4 mb-4 border rounded-2xl text-center font-bold outline-none focus:border-red-500 bg-gray-50">
            </div>

            <button onclick="handleLogin()" id="lBtn" class="w-full bg-red-600 text-white py-4 rounded-2xl font-bold shadow-lg active:scale-95 transition-transform">লগইন করুন</button>
            <p id="lErr" class="text-red-500 text-xs mt-3 hidden font-bold"></p>
            
            <div class="mt-6 pt-5 border-t border-gray-100">
                <p class="text-[11px] text-gray-400 font-bold mb-2">আপনি কি নতুন সদস্য?</p>
                <button onclick="showReg()" class="text-blue-600 font-bold text-sm underline">এখানে রেজিস্ট্রেশন করুন</button>
            </div>

            <div class="mt-8 pt-6 border-t-2 border-dashed border-gray-100">
                <p class="text-[10px] text-gray-400 font-black mb-3 uppercase tracking-widest">যোগাযোগ ও সোশ্যাল মিডিয়া</p>
                <div class="bg-red-50 p-4 rounded-2xl mb-4">
                    <h3 class="text-sm font-black text-gray-800">প্রতিষ্ঠাতা পরিচালক</h3>
                    <p class="text-base font-bold text-red-600 mt-1">মোঃ মেহেদী হাসান</p>
                    <div class="flex flex-wrap justify-center gap-2 mt-3">
                        <a href="https://wa.me/8801888354739" target="_blank" class="bg-white text-green-600 px-4 py-2 rounded-xl text-xs font-bold border border-green-100 shadow-sm">WhatsApp</a>
                        <a href="https://www.facebook.com/mdmehedihasan.mehedi.54922" target="_blank" class="bg-white text-blue-700 px-4 py-2 rounded-xl text-xs font-bold border border-blue-100 shadow-sm">Facebook</a>
                    </div>
                </div>
                <a href="https://facebook.com/groups/jubokolyan.bdf/" target="_blank" class="flex items-center justify-center gap-2 bg-blue-600 text-white py-3 rounded-2xl font-bold text-xs shadow-md mb-3">👥 আমাদের ফেসবুক গ্রুপে জয়েন করুন</a>
                <a href="tel:01888354739" class="flex items-center justify-center gap-2 bg-gray-800 text-white py-3 rounded-2xl font-bold text-sm w-full">📞 কল করুন: 01888354739</a>
            </div>
        </div>
    </div>

    <div id="regPage" class="container-custom hidden">
        <div class="bg-white p-6 rounded-[35px] shadow-xl border border-gray-100">
            <h2 class="text-lg font-bold text-gray-800 mb-5 text-center">মেম্বার রেজিস্ট্রেশন</h2>
            <input type="text" id="regName" placeholder="সম্পূর্ণ নাম" class="w-full p-3 mb-3 border rounded-xl outline-none font-bold bg-gray-50">
            <select id="regGroup" class="w-full p-3 mb-3 border rounded-xl outline-none font-bold bg-white">
                <option value="">রক্তের গ্রুপ</option>
                <option value="A+">A+</option><option value="A-">A-</option>
                <option value="B+">B+</option><option value="B-">B-</option>
                <option value="O+">O+</option><option value="O-">O-</option>
                <option value="AB+">AB+</option><option value="AB-">AB-</option>
            </select>
            <input type="text" id="regLoc" placeholder="ঠিকানা (গ্রাম, উপজেলা, জেলা। উদাঃ মহেশপুর, চাটখিল, নোয়াখালী)" class="w-full p-3 mb-3 border rounded-xl outline-none font-bold bg-gray-50 focus:border-red-500">
            
            <input type="tel" id="regPhone" placeholder="মোবাইল নম্বর" class="w-full p-3 mb-3 border rounded-xl outline-none font-bold bg-gray-50">
            <p class="text-[10px] text-gray-500 mb-1 px-1">সর্বশেষ রক্তদান (ঐচ্ছিক)</p>
            <input type="date" id="regLast" class="w-full p-3 mb-5 border rounded-xl outline-none font-bold bg-gray-50">
            <button onclick="handleRegister()" id="rBtn" class="w-full bg-green-600 text-white py-4 rounded-2xl font-bold shadow-lg">রেজিস্ট্রেশন সম্পন্ন করুন</button>
            <button onclick="location.reload()" class="w-full text-gray-500 mt-4 text-sm font-bold">ফিরে যান</button>
        </div>
    </div>

    <div id="mainPage" class="hidden">
        <div class="hero-gradient text-white p-5 rounded-b-[35px] text-center relative mb-4 shadow-lg">
            <button onclick="location.reload()" class="absolute top-4 right-4 text-[10px] bg-white/20 px-3 py-1 rounded-full border border-white/30">লগ আউট</button>
            <p id="welcome" class="text-xs font-bold text-yellow-200"></p>
        </div>
        <div class="container-custom mb-4">
            <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা গ্রুপ দিয়ে খুঁজুন..." class="w-full p-4 rounded-2xl border-none shadow-md outline-none focus:ring-2 focus:ring-red-500 font-bold">
        </div>
        <div id="donorList" class="container-custom grid gap-6"></div>
    </div>

    <script>
        const scriptURL = "https://script.google.com/macros/s/AKfycbwaIFFoE5Kzs9BoJa6JOADxdDtM-k62CgFD2phNOhQ6vat0d3a7s5w_TiXHMmfia2B3/exec"; 
        let allDonors = [], loggedUser = null, currentRole = 'Member', targetPhone = "";

        function showReg() { document.getElementById('loginPage').classList.add('hidden'); document.getElementById('regPage').classList.remove('hidden'); }
        function setRole(r) { currentRole = r; document.getElementById('memberFields').classList.toggle('hidden', r==='Admin'); document.getElementById('adminFields').classList.toggle('hidden', r==='Member'); document.getElementById('roleMember').classList.toggle('bg-white', r==='Member'); document.getElementById('roleAdmin').classList.toggle('bg-white', r==='Admin'); }

        async function handleLogin() {
            const phone = document.getElementById('uPhone').value.trim();
            const pass = document.getElementById('uPass').value.trim();
            const err = document.getElementById('lErr');
            err.innerText = "⏳ ডাটা যাচাই হচ্ছে..."; err.classList.remove('hidden');
            try {
                const res = await fetch(scriptURL); allDonors = await res.json();
                if(currentRole === 'Admin' && pass === 'Mehedi4739') { loggedUser = { n: "এডমিন", role: "Admin" }; showMain(); }
                else {
                    const user = allDonors.find(d => String(d.p).slice(-10) === phone.slice(-10));
                    if(user) { loggedUser = { ...user, role: "Member" }; showMain(); }
                    else { err.innerText = "❌ মেম্বার পাওয়া যায়নি!"; }
                }
            } catch (e) { err.innerText = "❌ সার্ভার এরর!"; }
        }

        async function handleRegister() {
            const n = document.getElementById('regName').value;
            const g = document.getElementById('regGroup').value;
            const l = document.getElementById('regLoc').value;
            const p = document.getElementById('regPhone').value;
            const last = document.getElementById('regLast').value;
            if(!n || !g || !l || !p) return alert("সব তথ্য দিন!");
            document.getElementById('rBtn').innerText = "⏳ প্রসেসিং...";
            try {
                await fetch(scriptURL, { method: 'POST', body: JSON.stringify({ action: "register", n, g, l, p, last }) });
                alert("রেজিস্ট্রেশন সফল!"); location.reload();
            } catch (e) { alert("ব্যর্থ!"); }
        }

        function showMain() {
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('mainPage').classList.remove('hidden');
            document.getElementById('welcome').innerText = "স্বাগতম: " + (loggedUser.n || "");
            renderDonors(allDonors);
        }

        function filterDonors() {
            const term = document.getElementById('searchInput').value.toLowerCase();
            renderDonors(allDonors.filter(d => d.n.toLowerCase().includes(term) || d.g.toLowerCase().includes(term) || d.l.toLowerCase().includes(term)));
        }

        function calculateStatus(dateStr) {
            if(!dateStr || dateStr == "" || dateStr == "undefined") return { last: "আপডেট প্রয়োজন", next: "আপডেট প্রয়োজন" };
            const lastDate = new Date(dateStr);
            if (isNaN(lastDate.getTime())) return { last: "আপডেট প্রয়োজন", next: "আপডেট প্রয়োজন" };
            const diffDays = Math.floor((new Date() - lastDate) / (1000 * 60 * 60 * 24));
            const fmt = lastDate.toLocaleDateString('en-GB', { day: 'numeric', month: 'long', year: 'numeric' });
            return diffDays >= 90 ? { last: fmt, next: "রক্ত দিতে পারবে ✅" } : { last: fmt, next: (90 - diffDays) + " দিন বাকী" };
        }

        function renderDonors(data) {
            const list = document.getElementById('donorList'); list.innerHTML = "";
            data.forEach((d, index) => {
                const s = calculateStatus(d.last); const cIdx = index % 5;
                const isMe = (loggedUser.role === 'Member' && String(d.p).slice(-10) === String(loggedUser.p).slice(-10));
                const isAdmin = (loggedUser.role === 'Admin');
                list.innerHTML += `
                <div class="bg-white p-5 rounded-[30px] shadow-sm border-t-[6px] card-${cIdx} relative overflow-hidden">
                    <div class="absolute top-0 right-0 bg-red-600 text-white px-4 py-1.5 rounded-bl-2xl font-black text-lg shadow-sm">${d.g}</div>
                    <div class="mt-2 space-y-1">
                        <div class="info-row"><span class="info-label text-xs">সিরিয়াল নংঃ</span><span class="px-2 py-0.5 rounded-full text-[10px] font-black sl-${cIdx}">সিরিয়াল নংঃ ${String(index+1).padStart(2,'0')}</span></div>
                        <div class="info-row"><span class="info-label">নামঃ</span><span class="text-xl font-black text-gray-900 leading-tight">${d.n}</span></div>
                        <div class="info-row"><span class="info-label text-xs">ঠিকানাঃ</span><span class="info-value text-gray-500">${d.l}</span></div>
                        <div class="info-row"><span class="info-label text-xs">সর্বশেষ রক্তদানঃ</span><span class="info-value text-red-custom">${s.last}</span></div>
                        <div class="info-row"><span class="info-label text-xs">পরবর্তী রক্তদানঃ</span><span class="info-value text-red-custom font-black">${s.next}</span></div>
                        <div class="info-row border-none pt-3">
                            <span class="info-label text-xs">মোবাইলঃ</span>
                            <div class="info-value flex items-center gap-3"><span class="text-blue-600 font-bold">${d.p}</span>
                                <a href="tel:${d.p}" class="bg-green-500 text-white p-2 rounded-full shadow-md"><svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z" /></svg></a>
                            </div>
                        </div>
                    </div>
                </div>`;
            });
        }
    </script>
</body>
</html>
