<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>যুব কল্যাণ রক্তদান ফাউন্ডেশন - লগইন</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Hind Siliguri', sans-serif; background-color: #f1f5f9; }
        .hero-gradient { background: linear-gradient(135deg, #dc2626 0%, #991b1b 100%); }
        .custom-radio:checked + label { border-color: #dc2626; background-color: #fef2f2; color: #dc2626; }
    </style>
</head>
<body class="pb-10">

    <div id="loginPage" class="flex flex-col items-center justify-center min-h-screen px-6">
        <div class="bg-white p-8 rounded-[40px] shadow-2xl w-full max-w-sm border border-gray-100">
            <div class="text-center mb-6">
                <img src="logo.png" onerror="this.src='https://i.ibb.co/C3m2X9Y/1000001730.png'" class="w-20 h-20 mx-auto mb-4 rounded-full border-4 border-red-50 shadow-md">
                <h2 class="text-2xl font-bold text-gray-800">লগইন প্যানেল</h2>
            </div>

            <div class="flex gap-4 mb-6">
                <div class="flex-1">
                    <input type="radio" id="roleMember" name="userRole" value="Member" class="hidden custom-radio" checked>
                    <label for="roleMember" class="block text-center p-3 border-2 rounded-2xl cursor-pointer font-bold text-gray-500 transition-all">সদস্য</label>
                </div>
                <div class="flex-1">
                    <input type="radio" id="roleAdmin" name="userRole" value="Admin" class="hidden custom-radio">
                    <label for="roleAdmin" class="block text-center p-3 border-2 rounded-2xl cursor-pointer font-bold text-gray-500 transition-all">এডমিন</label>
                </div>
            </div>

            <div class="space-y-4">
                <div>
                    <label class="text-xs font-bold text-gray-400 ml-2">মোবাইল নম্বর</label>
                    <input type="tel" id="loginPhone" placeholder="01XXX-XXXXXX" class="w-full p-4 border rounded-2xl outline-none focus:border-red-500 bg-gray-50 text-sm">
                </div>
                <div>
                    <label class="text-xs font-bold text-gray-400 ml-2">পাসওয়ার্ড</label>
                    <input type="password" id="loginPass" placeholder="••••••••" class="w-full p-4 border rounded-2xl outline-none focus:border-red-500 bg-gray-50 text-sm">
                </div>
                <button onclick="handleLogin()" class="w-full bg-red-600 text-white py-4 rounded-2xl font-bold shadow-lg hover:bg-red-700 active:scale-95 transition-all mt-2">প্রবেশ করুন</button>
            </div>
            <p id="errorMsg" class="text-red-500 text-xs mt-4 text-center font-bold hidden">❌ ভুল নম্বর বা পাসওয়ার্ড!</p>
        </div>
    </div>

    <div id="mainPage" class="hidden">
        <div class="hero-gradient text-white p-8 rounded-b-[50px] shadow-lg text-center relative">
            <button onclick="location.reload()" class="absolute top-5 right-5 text-[10px] bg-white/20 px-3 py-1 rounded-full font-bold uppercase">লগ আউট</button>
            <h1 class="text-lg font-bold">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
            <p id="userWelcome" class="text-yellow-300 text-sm mt-2 font-bold"></p>
        </div>

        <div id="updateSection" class="mx-6 -mt-8 bg-white p-6 rounded-[35px] shadow-2xl border border-red-50 relative z-10 hidden">
            <h3 class="text-sm font-bold text-gray-700 mb-4 flex items-center gap-2">📅 রক্তদানের তারিখ আপডেট</h3>
            <div class="flex flex-col gap-3">
                <input type="date" id="dateInput" class="w-full p-4 border rounded-2xl outline-none bg-gray-50 font-bold text-gray-700">
                <button onclick="submitUpdate()" id="submitBtn" class="bg-green-600 text-white py-4 rounded-2xl font-bold shadow-md active:scale-95 transition-all">সেভ করুন</button>
            </div>
        </div>

        <div class="px-6 mt-10">
            <input type="text" id="search" onkeyup="renderDonors()" placeholder="নাম বা এলাকা লিখুন..." class="w-full p-4 border rounded-2xl shadow-sm outline-none focus:border-red-400 text-sm">
        </div>

        <div id="listContainer" class="p-6 grid gap-4"></div>
    </div>

    <script>
        // আপনার Google Apps Script এর Web App URL এখানে দিন
        const apiURL = "আপনার_নতুন_DEPLOY_URL_এখানে"; 
        
        let allData = [];
        let activeUser = null;

        // লগইন হ্যান্ডলার
        async function handleLogin() {
            const phone = document.getElementById('loginPhone').value;
            const pass = document.getElementById('loginPass').value;
            const role = document.querySelector('input[name="userRole"]:checked').value;
            const error = document.getElementById('errorMsg');

            // পাসওয়ার্ড ভ্যালিডেশন
            const isMemberValid = (role === 'Member' && pass === 'JKBDF');
            const isAdminValid = (role === 'Admin' && pass === 'Mehedi4739');

            if (!isMemberValid && !isAdminValid) {
                error.innerText = "❌ ভুল পাসওয়ার্ড!";
                error.classList.remove('hidden');
                return;
            }

            try {
                const response = await fetch(apiURL);
                allData = await response.json();
                activeUser = allData.find(u => u.p == phone);

                if (activeUser) {
                    // যদি মেম্বার হয় তবে চেক করা সে কি মেম্বার কি না (শিটে Role কলাম অনুযায়ী)
                    document.getElementById('loginPage').classList.add('hidden');
                    document.getElementById('mainPage').classList.remove('hidden');
                    document.getElementById('userWelcome').innerText = (role === 'Admin' ? "👑 এডমিন: " : "👋 সদস্য: ") + activeUser.n;
                    
                    // সদস্য এবং এডমিন উভয়েই তারিখ আপডেট করতে পারবে
                    document.getElementById('updateSection').classList.remove('hidden');
                    renderDonors();
                } else {
                    error.innerText = "❌ এই নম্বরটি আমাদের ডাটাবেজে নেই!";
                    error.classList.remove('hidden');
                }
            } catch (e) {
                alert("সার্ভার ত্রুটি! ইন্টারনাল কানেকশন চেক করুন।");
            }
        }

        // তারিখ আপডেট সাবমিট
        async function submitUpdate() {
            const newDate = document.getElementById('dateInput').value;
            const btn = document.getElementById('submitBtn');
            if(!newDate) return alert("অনুগ্রহ করে তারিখ সিলেক্ট করুন");

            btn.disabled = true;
            btn.innerText = "আপডেট হচ্ছে...";

            const payload = { phone: activeUser.p, newDate: newDate };

            try {
                await fetch(apiURL, { method: 'POST', body: JSON.stringify(payload) });
                alert("সফলভাবে আপডেট করা হয়েছে!");
                location.reload();
            } catch (e) {
                alert("আপডেট ব্যর্থ হয়েছে!");
                btn.disabled = false;
                btn.innerText = "সেভ করুন";
            }
        }

        // ডোনার লিস্ট রেন্ডার
        function renderDonors() {
            const query = document.getElementById('search').value.toLowerCase();
            const list = document.getElementById('listContainer');
            list.innerHTML = "";

            const filtered = allData.filter(d => d.n.toLowerCase().includes(query) || d.l.toLowerCase().includes(query));
