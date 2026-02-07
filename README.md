<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>যুব কল্যাণ রক্তদান ফাউন্ডেশন</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Hind Siliguri', sans-serif; background-color: #f8fafc; }
        .hero-gradient { background: linear-gradient(135deg, #dc2626 0%, #991b1b 100%); }
        .status-box { border-radius: 18px; padding: 10px; }
    </style>
</head>
<body class="pb-20">

    <div class="bg-white p-6 shadow-sm border-b-2 border-red-50 text-center flex flex-col items-center">
        <img src="https://i.ibb.co/C3m2X9Y/1000001730.png" class="w-24 h-24 mb-3 rounded-full border-4 border-red-100 shadow-md">
        <h1 class="text-2xl font-black text-red-600 tracking-tight leading-tight">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-[10px] text-gray-500 font-bold uppercase tracking-[3px] mt-1">মানবতার তরে আমরা সর্বদা প্রস্তুত</p>
        
        <a href="https://facebook.com/groups/jubokolyan.bdf/" target="_blank" class="mt-4 inline-flex items-center gap-2 bg-blue-600 text-white px-5 py-2 rounded-full text-xs font-bold shadow-md active:scale-95 transition-all">
            <img src="https://upload.wikimedia.org/wikipedia/commons/b/b8/2021_Facebook_icon.svg" class="w-4 h-4 brightness-200">
            আমাদের ফেসবুক গ্রুপে জয়েন করুন
        </a>
    </div>

    <div id="loginPage" class="flex flex-col items-center justify-start min-h-screen px-6 pt-10">
        <div class="bg-white p-8 rounded-[40px] shadow-xl w-full max-w-sm text-center border border-gray-100">
            <h2 class="text-xl font-bold text-gray-800 mb-6">প্যানেলে প্রবেশ করুন</h2>
            
            <div class="flex justify-center gap-4 mb-6 p-1 bg-gray-100 rounded-2xl">
                <button onclick="setRole('Member')" id="roleMember" class="flex-1 py-3 rounded-xl text-xs font-bold bg-white text-red-600 shadow-sm transition-all">সদস্য</button>
                <button onclick="setRole('Admin')" id="roleAdmin" class="flex-1 py-3 rounded-xl text-xs font-bold text-gray-500 transition-all">এডমিন</button>
            </div>

            <div id="memberFields">
                <input type="tel" id="uPhone" placeholder="মোবাইল নম্বর লিখুন" class="w-full p-4 mb-4 border-2 border-gray-50 rounded-2xl outline-none focus:border-red-500 bg-gray-50 text-center font-bold">
            </div>

            <div id="adminFields" class="hidden">
                <input type="password" id="uPass" placeholder="এডমিন পাসওয়ার্ড দিন" class="w-full p-4 mb-4 border-2 border-gray-50 rounded-2xl outline-none focus:border-red-500 bg-gray-50 text-center font-bold">
            </div>
            
            <button onclick="handleLogin()" id="lBtn" class="w-full bg-red-600 text-white py-4 rounded-2xl font-bold shadow-lg active:scale-95 transition-all">লগইন করুন</button>
            <p id="lErr" class="text-red-500 text-[10px] mt-4 font-bold hidden"></p>

            <div class="mt-8 pt-6 border-t border-gray-50">
                <p class="text-[10px] text-gray-400 font-bold mb-3 uppercase">প্রয়োজনে যোগাযোগ করুন</p>
                <h3 class="text-[11px] font-black text-gray-700">প্রতিষ্ঠাতা পরিচালকঃ মোঃ মেহেদী হাসান</h3>
                
                <div class="flex flex-col gap-2 mt-4">
                    <a href="tel:01888354739" class="flex items-center justify-center gap-2 bg-blue-50 text-blue-700 py-2.5 rounded-xl font-bold text-sm border border-blue-100">📞 01888354739</a>
                    <div class="grid grid-cols-2 gap-2">
                        <a href="https://wa.me/8801888354739" class="flex items-center justify-center gap-2 bg-green-50 text-green-700 py-2.5 rounded-xl text-[10px] font-bold border border-green-100">
                            <img src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" class="w-4 h-4"> হোয়াটসঅ্যাপ
                        </a>
                        <a href="https://www.facebook.com/Mehedi.1316" class="flex items-center justify-center gap-2 bg-indigo-50 text-indigo-700 py-2.5 rounded-xl text-[10px] font-bold border border-indigo-100">
                            <img src="https://upload.wikimedia.org/wikipedia/commons/b/b8/2021_Facebook_icon.svg" class="w-4 h-4"> ফেসবুক
                        </a>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div id="mainPage" class="hidden">
        <div class="hero-gradient text-white p-6 rounded-b-[40px] shadow-lg text-center relative mb-6">
            <button onclick="location.reload()" class="absolute top-4 right-4 text-[9px] bg-white/20 px-3 py-1 rounded-full border border-white/30 font-bold">লগ আউট</button>
            <p id="welcome" class="text-yellow-300 text-xs font-bold"></p>
        </div>

        <div class="px-6 mb-6">
            <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা রক্তের গ্রুপ দিয়ে খুঁজুন..." class="w-full p-4 rounded-2xl border-none shadow-md outline-none focus:ring-2 focus:ring-red-500 font-bold text-sm">
        </div>

        <div id="donorList" class="px-6 grid gap-6"></div>
    </div>

    <div id="editModal" class="fixed inset-0 bg-black/60 hidden flex items-center justify-center p-6 z-[100]">
        <div class="bg-white p-6 rounded-[35px] w-full max-w-sm shadow-2xl">
            <h3 id="editingName" class="font-bold text-gray-800 text-lg"></h3>
            <p class="text-[10px] text-gray-400 mb-4 font-bold uppercase tracking-widest">রক্তদানের তারিখ আপডেট</p>
            <input type="date" id="newDate" class="w-full p-4 border rounded-2xl mb-4 bg-gray-50 outline-none font-bold text-center">
            <div class="flex gap-2">
                <button onclick="closeEdit()" class="flex-1 bg-gray-100 py-3 rounded-xl font-bold text-xs">বাতিল</button>
                <button onclick="submitUpdate()" id="sBtn" class="flex-1 bg-green-600 text-white py-3 rounded-xl font-bold text-xs">সেভ করুন</button>
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
                        loggedUser = { n: "অ্যাডমিন প্যানেল", role: "Admin" }; showMain();
                    } else { err.innerText = "❌ ভুল এডমিন পাসওয়ার্ড!"; }
                } else {
                    const member = allDonors.find(d => String(d.p).slice(-10) === phone.slice(-10));
                    if(member) {
                        loggedUser = { ...member, role: "Member" }; showMain();
                    } else { err.innerText = "❌ এই নম্বরটি তালিকায় নেই!"; }
                }
            } catch (e) { err.innerText = "❌ কানেকশন এরর!"; }
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

        function renderDonors(data) {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            data.forEach((d, index) => {
                const isMe = (loggedUser.role === 'Member' && String(d.p).slice(-10) === String(loggedUser.p).slice(-10));
                const isAdmin = (loggedUser.role === 'Admin');
                const status = calculateStatus(d.last);
                list.innerHTML += `
                <div class="bg-white p-6 rounded-[35px] shadow-sm border-2 ${isMe ? 'border-green-400 bg-green-50/20' : 'border-white'} relative">
                    <span class="absolute -top-1 left-8 bg-gray-800 text-white text-[10px] px-3 py-1 rounded-b-lg font-bold">SL: ${d.sl || (index+1)}</span>
                    <div class="flex flex-col gap-4 mt-4">
                        <div class="flex items-center gap-4">
                            <div class="bg-red-600 text-white min-w-[60px] h-[60px] rounded-[22px] flex items-center justify-center font-black text-2xl shadow-lg">${d.g}</div>
                            <div class="flex-1">
                                <h4 class="font-bold text-gray-800 text-lg leading-tight">${d.n}</h4>
                                <p class="text-sm text-red-600 font-bold mt-1 tracking-wide">📍 ${d.l}</p>
                            </div>
                            ${(isMe || isAdmin) ? `<button onclick="openEdit('${d.p}', '${d.n}')" class="bg-blue-600 px-4 py-2 rounded-xl text-white shadow-md font-bold text-xs">এডিট</button>` : ''}
                        </div>
                        <div class="grid grid-cols-2 gap-2 mt-2">
                            <div class="status-box bg-gray-50 border border-gray-100 text-center">
                                <p class="text-[9px] font-bold text-gray-400 uppercase mb-1">সর্বশেষ রক্তদান</p>
                                <p class="text-sm font-black text-gray-700">${(d.last && d.last !== "undefined") ? d.last : "তারিখ নেই"}</p>
                            </div>
                            <div class="status-box ${status.color} border border-opacity-30 text-center">
                                <p class="text-[9px] font-bold opacity-60 uppercase mb-1">স্ট্যাটাস</p>
                                <p class="text-[12px] font-black leading-tight ${status.textColor}">${status.txt}</p>
                            </div>
                        </div>
                    </div>
                </div>`;
            });
        }

        function openEdit(phone, name) { targetPhone = phone; document.getElementById('editingName').innerText = name; document.getElementById('editModal').classList.remove('hidden'); }
        function closeEdit() { document.getElementById('editModal').classList.add('hidden'); }

        async function submitUpdate() {
            const date = document.getElementById('newDate').value;
            if(!date) return alert("তারিখ দিন!");
            document.getElementById('sBtn').disabled = true;
            try {
                await fetch(scriptURL, { method: 'POST', body: JSON.stringify({ phone: targetPhone, newDate: date }) });
                alert("সফলভাবে আপডেট হয়েছে!"); location.reload();
            } catch (e) { alert("ব্যর্থ হয়েছে!"); document.getElementById('sBtn').disabled = false; }
        }

        function calculateStatus(dateStr) {
            if(!dateStr || dateStr === "তারিখ নেই" || dateStr === "undefined" || dateStr === "") {
                return { txt: "দয়া করে তারিখ আপডেট করুন", can: false, color: "bg-orange-100 border-orange-200", textColor: "text-orange-700" };
            }
            const last = new Date(dateStr);
            const diffDays = Math.floor((new Date() - last) / (1000 * 60 * 60 * 24));
            if (diffDays >= 90) {
                return { txt: "রক্ত দিতে পারবে ✅", can: true, color: "bg-green-100 border-green-200", textColor: "text-green-700" };
            } else {
                return { txt: (90 - diffDays) + " দিন পর রক্ত দিতে পারবে", can: false, color: "bg-red-50 border-red-100", textColor: "text-red-600" };
            }
        }
    </script>
</body>
</html>
