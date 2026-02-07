<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>যুব কল্যাণ রক্তদান ফাউন্ডেশন - প্যানেল</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Hind Siliguri', sans-serif; background-color: #f1f5f9; }
        .hero-gradient { background: linear-gradient(135deg, #dc2626 0%, #991b1b 100%); }
        .status-box { border-radius: 20px; padding: 12px; transition: all 0.3s; }
    </style>
</head>
<body class="pb-20">

    <div id="loginPage" class="flex flex-col items-center justify-center min-h-screen px-6">
        <div class="bg-white p-8 rounded-[40px] shadow-2xl w-full max-w-sm text-center border border-gray-100">
            <img src="https://i.ibb.co/C3m2X9Y/1000001730.png" class="w-20 h-20 mx-auto mb-4 rounded-full border-4 border-red-50">
            <h2 class="text-2xl font-bold text-gray-800 mb-2">প্রবেশ করুন</h2>
            
            <div class="flex justify-center gap-4 mb-6 mt-4 p-1 bg-gray-100 rounded-2xl">
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
        </div>
    </div>

    <div id="mainPage" class="hidden">
        <div class="hero-gradient text-white p-8 rounded-b-[50px] shadow-lg text-center relative mb-6">
            <button onclick="location.reload()" class="absolute top-5 right-5 text-[10px] bg-white/20 px-3 py-1 rounded-full border border-white/30 font-bold">লগ আউট</button>
            <h1 class="text-lg font-bold uppercase tracking-tight">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
            <p id="welcome" class="text-yellow-300 text-[11px] mt-2 font-bold"></p>
        </div>

        <div class="px-6 mb-6">
            <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা রক্তের গ্রুপ দিয়ে খুঁজুন..." class="w-full p-4 rounded-2xl border-none shadow-md outline-none focus:ring-2 focus:ring-red-500 font-bold text-sm">
        </div>

        <div id="editModal" class="fixed inset-0 bg-black/60 hidden flex items-center justify-center p-6 z-50">
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

        <div class="px-8 mb-4 flex items-center justify-between">
            <h3 class="text-xs font-black text-gray-400 uppercase tracking-widest">রক্তদাতাদের তালিকা</h3>
            <span id="adminBadge" class="hidden bg-black text-white text-[8px] px-2 py-0.5 rounded font-bold uppercase tracking-widest">Admin Access</span>
        </div>

        <div id="donorList" class="px-6 grid gap-6"></div>
    </div>

    <script>
        const scriptURL = "https://script.google.com/macros/s/AKfycbx2AUrExnRv7h2cMRlBv6nqpf3oNy5s9Bh0iKzGG3-5YhGC1NGDEWN7lsRmuaEHo92o/exec"; 
        
        let allDonors = [];
        let loggedUser = null;
        let currentRole = 'Member';
        let targetPhone = "";

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
            if(loggedUser.role === 'Admin') document.getElementById('adminBadge').classList.remove('hidden');
            renderDonors(allDonors);
        }

        function filterDonors() {
            const term = document.getElementById('searchInput').value.toLowerCase();
            const filtered = allDonors.filter(d => 
                d.n.toLowerCase().includes(term) || d.g.toLowerCase().includes(term) || d.l.toLowerCase().includes(term)
            );
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
                <div class="bg-white p-6 rounded-[35px] shadow-md border-2 ${isMe ? 'border-green-400 bg-green-50/20' : 'border-white'} relative">
                    <span class="absolute -top-1 left-8 bg-gray-800 text-white text-[10px] px-3 py-1 rounded-b-lg font-bold">SL: ${d.sl || (index+1)}</span>
                    <div class="flex flex-col gap-4 mt-4">
                        <div class="flex items-center gap-4">
                            <div class="bg-red-600 text-white min-w-[60px] h-[60px] rounded-[22px] flex items-center justify-center font-black text-2xl shadow-lg">${d.g}</div>
                            <div class="flex-1">
                                <h4 class="font-bold text-gray-800 text-lg leading-tight">${d.n}</h4>
                                <p class="text-sm text-red-600 font-bold mt-1 tracking-wide">📍 ঠিকানা: ${d.l}</p>
                            </div>
                            ${(isMe || isAdmin) ? `<button onclick="openEdit('${d.p}', '${d.n}')" class="bg-blue-600 px-4 py-2 rounded-xl text-white shadow-md font-bold text-xs active:scale-95 transition-all">এডিট</button>` : ''}
                        </div>
                        <div class="grid grid-cols-2 gap-2 mt-2">
                            <div class="status-box bg-gray-50 border border-gray-100 text-center">
                                <p class="text-[9px] font-bold text-gray-400 uppercase mb-1 tracking-wider">সর্বশেষ রক্তদান</p>
                                <p class="text-sm font-black text-gray-700">${(d.last && d.last !== "undefined") ? d.last : "তারিখ নেই"}</p>
                            </div>
                            <div class="status-box ${status.color} border border-opacity-30 text-center">
                                <p class="text-[9px] font-bold opacity-60 uppercase mb-1 tracking-wider">রক্তদানের স্ট্যাটাস</p>
                                <p class="text-[13px] font-black leading-tight ${status.textColor}">
                                    ${status.txt}
                                </p>
                            </div>
                        </div>
                    </div>
                </div>`;
            });
        }

        function openEdit(phone, name) {
            targetPhone = phone;
            document.getElementById('editingName').innerText = name;
            document.getElementById('editModal').classList.remove('hidden');
        }

        function closeEdit() { document.getElementById('editModal').classList.add('hidden'); }

        async function submitUpdate() {
            const date = document.getElementById('newDate').value;
            if(!date) return alert("দয়া করে তারিখ সিলেক্ট করুন!");
            document.getElementById('sBtn').disabled = true;
            document.getElementById('sBtn').innerText = "সেভ হচ্ছে...";
            try {
                await fetch(scriptURL, { method: 'POST', body: JSON.stringify({ phone: targetPhone, newDate: date }) });
                alert("আপনার তথ্য সফলভাবে আপডেট করা হয়েছে!"); location.reload();
            } catch (e) { alert("দুঃখিত, আপডেট ব্যর্থ হয়েছে!"); document.getElementById('sBtn').disabled = false; }
        }

        function calculateStatus(dateStr) {
            // যদি তারিখ আপডেট করা না থাকে
            if(!dateStr || dateStr === "তারিখ নেই" || dateStr === "undefined" || dateStr === "") {
                return { 
                    txt: "দয়া করে তারিখ আপডেট করুন", 
                    can: false, 
                    color: "bg-orange-100 border-orange-200",
                    textColor: "text-orange-700" 
                };
            }

            const last = new Date(dateStr);
            const today = new Date();
            
            // অবৈধ তারিখ চেক
            if (isNaN(last.getTime())) {
                return { 
                    txt: "দয়া করে তারিখ আপডেট করুন", 
                    can: false, 
                    color: "bg-orange-100 border-orange-200",
                    textColor: "text-orange-700" 
                };
            }

            const diffTime = Math.abs(today - last);
            const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24));
            
            if (diffDays >= 90) {
                return { 
                    txt: "রক্ত দিতে পারবে ✅", 
                    can: true, 
                    color: "bg-green-100 border-green-200",
                    textColor: "text-green-700"
                };
            } else {
                const remaining = 90 - diffDays;
                return { 
                    txt: remaining + " দিন পর রক্ত দিতে পারবে", 
                    can: false, 
                    color: "bg-red-50 border-red-100",
                    textColor: "text-red-600"
                };
            }
        }
    </script>
</body>
</html>
