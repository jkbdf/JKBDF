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
        .container-custom { width: 100%; padding-left: 10px; padding-right: 10px; max-width: 500px; margin: 0 auto; }
        .info-row { display: flex; border-bottom: 1px solid rgba(0,0,0,0.05); padding: 8px 0; align-items: center; }
        .info-label { font-weight: 800; color: #4b5563; min-width: 115px; font-size: 14px; }
        .info-value { font-weight: 700; font-size: 14px; flex: 1; }
        
        .card-0 { border-top-color: #dc2626; }
        .card-1 { border-top-color: #2563eb; }
        .card-2 { border-top-color: #059669; }
        .card-3 { border-top-color: #7c3aed; }
        .card-4 { border-top-color: #db2777; }
        
        .sl-0 { background-color: #fee2e2; color: #dc2626; }
        .sl-1 { background-color: #dbeafe; color: #2563eb; }
        .sl-2 { background-color: #d1fae5; color: #059669; }
        .sl-3 { background-color: #f3e8ff; color: #7c3aed; }
        .sl-4 { background-color: #fce7f3; color: #db2777; }

        .text-red-custom { color: #dc2626; }
    </style>
</head>
<body class="pb-20">

    <div class="bg-white p-5 shadow-md border-b-2 border-red-100 text-center flex flex-col items-center mb-4">
        <img src="https://i.ibb.co/C3m2X9Y/1000001730.png" class="w-24 h-24 mb-3 rounded-full border-4 border-red-50 shadow-lg">
        <h1 class="text-2xl font-black text-red-600 leading-tight">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <a href="https://facebook.com/groups/jubokolyan.bdf/" target="_blank" class="mt-3 inline-flex items-center gap-2 bg-blue-600 text-white px-6 py-2 rounded-full text-xs font-bold shadow-md">আমাদের ফেসবুক গ্রুপ</a>
    </div>

    <div id="loginPage" class="container-custom flex flex-col items-center pt-4">
        <div class="bg-white p-6 rounded-[40px] shadow-xl w-full text-center border border-gray-100">
            <h2 class="text-xl font-bold text-gray-800 mb-6">লগইন করুন</h2>
            
            <div class="flex justify-center gap-2 mb-6 p-1 bg-gray-100 rounded-2xl">
                <button onclick="setRole('Member')" id="roleMember" class="flex-1 py-3 rounded-xl text-xs font-bold bg-white text-red-600 shadow-sm">সদস্য</button>
                <button onclick="setRole('Admin')" id="roleAdmin" class="flex-1 py-3 rounded-xl text-xs font-bold text-gray-500">এডমিন</button>
            </div>

            <div id="memberFields">
                <input type="tel" id="uPhone" placeholder="মোবাইল নম্বর লিখুন" class="w-full p-4 mb-4 border-2 border-gray-100 rounded-2xl outline-none focus:border-red-500 bg-gray-50 text-center font-bold">
            </div>

            <div id="adminFields" class="hidden">
                <input type="password" id="uPass" placeholder="এডমিন পাসওয়ার্ড দিন" class="w-full p-4 mb-4 border-2 border-gray-100 rounded-2xl outline-none focus:border-red-500 bg-gray-50 text-center font-bold">
            </div>
            
            <button onclick="handleLogin()" id="lBtn" class="w-full bg-red-600 text-white py-4 rounded-2xl font-bold shadow-lg active:scale-95 transition-all">লগইন করুন</button>
            <p id="lErr" class="text-red-500 text-[10px] mt-4 font-bold hidden"></p>

            <div class="mt-8 pt-6 border-t border-gray-100">
                <p class="text-[11px] text-gray-400 font-bold mb-3 uppercase tracking-wider">যেকোনো প্রয়োজনে যোগাযোগ করুন</p>
                <h3 class="text-sm font-black text-gray-700">প্রতিষ্ঠাতা পরিচালকঃ মোঃ মেহেদী হাসান</h3>
                <div class="grid grid-cols-1 gap-2 mt-4">
                    <a href="tel:01888354739" class="flex items-center justify-center gap-2 bg-blue-50 text-blue-700 py-3 rounded-2xl font-bold text-sm border border-blue-100">📞 01888354739</a>
                </div>
            </div>
        </div>
    </div>

    <div id="mainPage" class="hidden">
        <div class="hero-gradient text-white p-5 rounded-b-[40px] shadow-lg text-center relative mb-6">
            <button onclick="location.reload()" class="absolute top-4 right-4 text-[10px] bg-white/20 px-3 py-1 rounded-full border border-white/30 font-bold">লগ আউট</button>
            <p id="welcome" class="text-yellow-300 text-xs font-bold"></p>
        </div>
        <div class="container-custom mb-6">
            <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা রক্তের গ্রুপ দিয়ে খুঁজুন..." class="w-full p-4 rounded-2xl border-none shadow-md outline-none focus:ring-2 focus:ring-red-500 font-bold text-sm">
        </div>
        <div id="donorList" class="container-custom grid gap-6"></div>
    </div>

    <div id="editModal" class="fixed inset-0 bg-black/60 hidden flex items-center justify-center p-4 z-[100]">
        <div class="bg-white p-8 rounded-[40px] w-full max-w-sm shadow-2xl text-center">
            <h3 id="editingName" class="font-bold text-gray-800 text-lg mb-4"></h3>
            <input type="date" id="newDate" class="w-full p-4 border-2 border-gray-100 rounded-2xl mb-6 bg-gray-50 outline-none font-bold text-center">
            <div class="flex gap-3">
                <button onclick="closeEdit()" class="flex-1 bg-gray-100 py-3 rounded-2xl font-bold text-sm">বাতিল</button>
                <button onclick="submitUpdate()" id="sBtn" class="flex-1 bg-green-600 text-white py-3 rounded-2xl font-bold text-sm">সেভ করুন</button>
            </div>
        </div>
    </div>

    <script>
        const scriptURL = "https://script.google.com/macros/s/AKfycbx2AUrExnRv7h2cMRlBv6nqpf3oNy5s9Bh0iKzGG3-5YhGC1NGDEWN7lsRmuaEHo92o/exec"; 
        let allDonors = [], loggedUser = null, currentRole = 'Member', targetPhone = "";

        function setRole(role) {
            currentRole = role;
            const mFields = document.getElementById('memberFields'), aFields = document.getElementById('adminFields');
            const mBtn = document.getElementById('roleMember'), aBtn = document.getElementById('roleAdmin');
            if(role === 'Admin') {
                aFields.classList.remove('hidden'); mFields.classList.add('hidden');
                aBtn.className = "flex-1 py-3 rounded-xl text-xs font-bold bg-white text-red-600 shadow-sm";
                mBtn.className = "flex-1 py-3 rounded-xl text-xs font-bold text-gray-500";
            } else {
                aFields.classList.add('hidden'); mFields.classList.remove('hidden');
                mBtn.className = "flex-1 py-3 rounded-xl text-xs font-bold bg-white text-red-600 shadow-sm";
                aBtn.className = "flex-1 py-3 rounded-xl text-xs font-bold text-gray-500";
            }
        }

        async function handleLogin() {
            const phone = document.getElementById('uPhone').value.trim();
            const pass = document.getElementById('uPass').value.trim();
            const err = document.getElementById('lErr');
            err.innerText = "⏳ ডাটা যাচাই হচ্ছে..."; err.classList.remove('hidden');
            try {
                const res = await fetch(scriptURL);
                allDonors = await res.json();
                if(currentRole === 'Admin') {
                    if(pass === 'Mehedi4739') {
                        loggedUser = { n: "এডমিন প্যানেল", role: "Admin" }; showMain();
                    } else { err.innerText = "❌ ভুল পাসওয়ার্ড!"; }
                } else {
                    const member = allDonors.find(d => String(d.p).slice(-10) === phone.slice(-10));
                    if(member) {
                        loggedUser = { ...member, role: "Member" }; showMain();
                    } else { err.innerText = "❌ নম্বরটি সঠিক নয়!"; }
                }
            } catch (e) { err.innerText = "❌ সার্ভার এরর!"; }
        }

        function showMain() {
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('mainPage').classList.remove('hidden');
            document.getElementById('welcome').innerText = "স্বাগতম: " + loggedUser.n;
            renderDonors(allDonors);
        }

        function filterDonors() {
            const term = document.getElementById('searchInput').value.toLowerCase();
            const filtered = allDonors.filter(d => d.n.toLowerCase().includes(term) || d.g.toLowerCase().includes(term) || d.l.toLowerCase().includes(term));
            renderDonors(filtered);
        }

        function calculateStatus(dateStr) {
            if(!dateStr || dateStr == "" || dateStr == "undefined") {
                return { last: "দয়া করে আপডেট করে নিন", next: "আপডেট প্রয়োজন" };
            }
            const lastDate = new Date(dateStr);
            if (isNaN(lastDate.getTime())) {
                return { last: "দয়া করে আপডেট করে নিন", next: "আপডেট প্রয়োজন" };
            }
            const today = new Date();
            const diffDays = Math.floor((today - lastDate) / (1000 * 60 * 60 * 24));
            const formattedDate = lastDate.toLocaleDateString('en-GB', { day: 'numeric', month: 'long', year: 'numeric' });
            if (diffDays >= 90) return { last: formattedDate, next: "রক্ত দিতে পারবে ✅" };
            return { last: formattedDate, next: (90 - diffDays) + " দিন বাকী" };
        }

        function renderDonors(data) {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            data.forEach((d, index) => {
                const status = calculateStatus(d.last);
                const isMe = (loggedUser.role === 'Member' && String(d.p).slice(-10) === String(loggedUser.p).slice(-10));
                const isAdmin = (loggedUser.role === 'Admin');
                const colorIdx = index % 5;
                const slNo = String(index + 1).padStart(2, '0');

                list.innerHTML += `
                <div class="bg-white p-5 rounded-[30px] shadow-sm border-t-[6px] card-${colorIdx} relative overflow-hidden">
                    <div class="absolute top-0 right-0 bg-red-600 text-white px-5 py-2 rounded-bl-[20px] font-black text-xl shadow-md">${d.g}</div>
                    <div class="mt-2 space-y-1">
                        <div class="info-row"><span class="info-label">সিরিয়াল নংঃ</span><span class="px-3 py-0.5 rounded-full text-[11px] font-black sl-${colorIdx}">সিরিয়াল নংঃ ${slNo}</span></div>
                        <div class="info-row"><span class="info-label">নামঃ</span><span class="text-xl font-black text-gray-900 flex-1 leading-tight">${d.n}</span></div>
                        <div class="info-row"><span class="info-label">ঠিকানাঃ</span><span class="info-value text-gray-600">${d.l}</span></div>
                        <div class="info-row"><span class="info-label">সর্বশেষ রক্তদানঃ</span><span class="info-value text-red-custom">${status.last}</span></div>
                        <div class="info-row"><span class="info-label">পরবর্তী রক্তদানঃ</span><span class="info-value text-red-custom">${status.next}</span></div>
                        <div class="info-row border-none">
                            <span class="info-label">মোবাইলঃ</span>
                            <div class="info-value flex items-center gap-3">
                                <span class="text-blue-600 font-bold text-base">${d.p}</span>
                                <a href="tel:${d.p}" class="bg-green-500 text-white p-2 rounded-full shadow-md active:scale-90 transition-all"><svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z" /></svg></a>
                            </div>
                        </div>
                    </div>
                    ${(isMe || isAdmin) ? `<button onclick="openEdit('${d.p}', '${d.n}')" class="mt-4 w-full bg-blue-600 text-white py-2 rounded-xl font-bold text-xs shadow-md">তথ্য আপডেট করুন</button>` : ''}
                </div>`;
            });
        }

        function openEdit(phone, name) { targetPhone = phone; document.getElementById('editingName').innerText = name; document.getElementById('editModal').classList.remove('hidden'); }
        function closeEdit() { document.getElementById('editModal').classList.add('hidden'); }
        async function submitUpdate() {
            const date = document.getElementById('newDate').value;
            if(!date) return alert("তারিখ দিন!");
            document.getElementById('sBtn').innerText = "হচ্ছে..";
            try {
                await fetch(scriptURL, { method: 'POST', body: JSON.stringify({ phone: targetPhone, newDate: date }) });
                alert("সফলভাবে আপডেট হয়েছে!"); location.reload();
            } catch (e) { alert("ব্যর্থ হয়েছে!"); }
        }
    </script>
</body>
</html>
