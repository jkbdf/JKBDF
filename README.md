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
    </style>
</head>
<body class="bg-gray-50 pb-20">

    <div class="bg-red-600 text-white p-6 text-center shadow-lg">
        <h1 class="text-2xl font-bold uppercase tracking-tight">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-sm opacity-90 mt-1 italic">মানবতার কল্যাণে আমাদের রক্তদান</p>
        
        <div class="mt-4 pt-3 border-t border-red-400">
            <p class="text-md font-bold text-yellow-300">প্রতিষ্ঠাতা পরিচালক: মোঃ মেহেদী হাসান</p>
            <a href="tel:01888354739" class="inline-block mt-1 bg-white text-red-600 px-4 py-1 rounded-full text-xs font-bold shadow-md">📞 01888354739</a>
        </div>

        <div class="flex justify-center gap-3 mt-5">
            <button onclick="toggleForm('memberPanel')" class="bg-white text-green-700 px-4 py-2 rounded-xl text-[11px] font-bold shadow-lg">➕ ডোনার হিসেবে যোগ দিন</button>
            <button onclick="toggleForm('adminModal')" class="bg-red-900 text-white px-4 py-2 rounded-xl text-[10px] opacity-90">🔑 অ্যাডমিন লগইন</button>
        </div>
    </div>

    <div id="memberPanel" class="hidden mx-4 my-6 p-6 bg-white border-t-8 border-green-500 rounded-3xl shadow-xl">
        <h2 class="font-bold text-green-700 mb-4 text-center text-lg">রক্তদাতা নিবন্ধন ফরম</h2>
        <div class="space-y-3">
            <input type="text" id="mName" placeholder="আপনার নাম" class="w-full p-3 border rounded-xl text-sm outline-none border-gray-200">
            <input type="text" id="mLoc" placeholder="আপনার এলাকা / ঠিকানা" class="w-full p-3 border rounded-xl text-sm outline-none border-gray-200">
            <select id="mGroup" class="w-full p-3 border rounded-xl text-sm font-bold text-red-600 border-gray-200 text-center">
                <option value="A+">A+</option><option value="B+">B+</option><option value="O+">O+</option><option value="AB+">AB+</option>
                <option value="A-">A-</option><option value="B-">B-</option><option value="O-">O-</option><option value="AB-">AB-</option>
            </select>
            <input type="text" id="mPhone" placeholder="মোবাইল নম্বর" class="w-full p-3 border rounded-xl text-sm outline-none border-gray-200">
            <div>
                <label class="text-[10px] text-gray-400 ml-2 font-bold uppercase">শেষ রক্তদানের তারিখ</label>
                <input type="date" id="mDate" class="w-full p-3 border rounded-xl text-sm outline-none border-gray-200">
            </div>
            <button onclick="saveDonor()" id="mSaveBtn" class="w-full bg-green-600 text-white py-3 rounded-xl font-bold shadow-lg mt-2">জমা দিন</button>
            <button onclick="toggleForm('memberPanel')" class="w-full text-gray-400 text-xs py-1 mt-2 underline text-center block">বন্ধ করুন</button>
        </div>
    </div>

    <div id="adminModal" class="fixed inset-0 bg-black/70 hidden z-50 flex items-center justify-center p-4">
        <div class="bg-white p-6 rounded-3xl w-full max-w-sm text-center shadow-2xl">
            <h2 class="font-bold text-lg mb-4 text-gray-800">অ্যাডমিন প্রবেশ</h2>
            <input type="password" id="adminPass" placeholder="পাসওয়ার্ড দিন" class="w-full p-3 border rounded-xl mb-4 text-center outline-none border-gray-200">
            <div class="flex gap-2">
                <button onclick="loginAdmin()" class="flex-1 bg-red-600 text-white py-2 rounded-xl font-bold">প্রবেশ</button>
                <button onclick="toggleForm('adminModal')" class="flex-1 bg-gray-100 py-2 rounded-xl text-gray-500">বাতিল</button>
            </div>
        </div>
    </div>

    <div class="m-4 p-4 bg-white shadow-md rounded-2xl border border-gray-100">
        <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা এলাকা লিখে খুঁজুন..." class="w-full p-3 border border-gray-200 rounded-xl mb-3 outline-none focus:ring-2 focus:ring-red-500 text-sm text-center">
        <select id="groupFilter" onchange="filterDonors()" class="w-full p-3 border border-gray-200 rounded-xl font-bold text-red-600 outline-none text-sm bg-white text-center shadow-sm">
            <option value="">সব রক্তের গ্রুপ</option>
            <option value="A+">A+</option><option value="B+">B+</option><option value="O+">O+</option><option value="AB+">AB+</option>
            <option value="A-">A-</option><option value="B-">B-</option><option value="O-">O-</option><option value="AB-">AB-</option>
        </select>
    </div>

    <div id="loading" class="text-center py-20">
        <div class="animate-spin inline-block w-8 h-8 border-4 border-red-500 border-t-transparent rounded-full mb-2"></div>
        <p class="text-gray-400 text-xs font-bold">তথ্য লোড হচ্ছে...</p>
    </div>

    <div id="donorList" class="p-4 grid gap-4 hidden"></div>

    <script>
        const url = "https://script.google.com/macros/s/AKfycby31ZygLlJsE0aub_u40W0ElPclo1954I52deQLacB-WW1qqlTr1Z95w-oRq4WSnlvJ/exec";
        let allDonors = [];

        function toggleForm(id) { document.getElementById(id).classList.toggle('hidden'); }

        function loginAdmin() {
            if(document.getElementById('adminPass').value === "1234") {
                toggleForm('adminModal');
                toggleForm('memberPanel');
            } else { alert("ভুল পাসওয়ার্ড!"); }
        }

        // ৩ মাস (৯০ দিন) ক্যালকুলেশন লজিক
        function getDonationStatus(lastDateStr) {
            if (!lastDateStr || lastDateStr.trim() === "" || lastDateStr === "undefined" || lastDateStr === "N/A") {
                return { text: "কখনো দেয়নি", class: "text-green-600 bg-green-50 border-green-100", last: "কখনো দেয়নি" };
            }

            const lastDate = new Date(lastDateStr);
            if (isNaN(lastDate)) return { text: "তথ্য নেই", class: "text-gray-400 bg-gray-50 border-gray-100", last: "N/A" };
            
            const today = new Date();
            const diffTime = today - lastDate;
            const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24)); 
            
            const formattedDate = lastDate.toLocaleDateString('en-GB', { day: '2-digit', month: 'short', year: 'numeric' });

            if (diffDays >= 90) {
                return { text: "রক্ত দিতে পারবে", class: "text-green-600 bg-green-50 border-green-100", last: formattedDate };
            } else {
                const remaining = 90 - diffDays;
                return { text: remaining + " দিন বাকি", class: "text-red-600 bg-red-50 border-red-100", last: formattedDate };
            }
        }

        async function saveDonor() {
            const btn = document.getElementById('mSaveBtn');
            const data = {
                name: document.getElementById('mName').value,
                location: document.getElementById('mLoc').value,
                group: document.getElementById('mGroup').value,
                phone: document.getElementById('mPhone').value,
                lastDate: document.getElementById('mDate').value
            };

            if(!data.name || !data.phone) return alert("দয়া করে নাম এবং ফোন নম্বর দিন!");
            btn.innerText = "প্রসেসিং..."; btn.disabled = true;

            try {
                await fetch(url, { method: 'POST', mode: 'no-cors', body: JSON.stringify(data) });
                alert("সফল হয়েছে! ১ মিনিট পর রিফ্রেশ দিন।");
                location.reload();
            } catch (e) {
                alert("আবার চেষ্টা করুন।");
                btn.innerText = "জমা দিন"; btn.disabled = false;
            }
        }

        async function loadDonors() {
            try {
                const response = await fetch(url);
                allDonors = await response.json();
                displayDonors(allDonors);
                document.getElementById('loading').classList.add('hidden');
                document.getElementById('donorList').classList.remove('hidden');
            } catch (e) {
                document.getElementById('loading').innerHTML = "<p class='text-red-500'>লিঙ্ক ত্রুটি!</p>";
            }
        }

        function displayDonors(data) {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            data.forEach(d => {
                const status = getDonationStatus(d.last);
                list.innerHTML += `
                <div class="bg-white p-5 rounded-3xl shadow-sm border border-gray-100 relative mb-2">
                    <span class="absolute top-0 right-0 bg-gray-50 text-gray-400 text-[9px] px-3 py-1 rounded-bl-2xl font-bold italic">SL: ${d.sl}</span>
                    <div class="flex justify-between items-start mb-4 mt-2">
                        <div class="w-2/3">
                            <h3 class="font-bold text-lg text-gray-800 leading-tight">${d.n}</h3>
                            <p class="text-[10px] text-gray-400 mt-1">📍 ${d.l}</p>
                        </div>
                        <div class="bg-red-50 px-3 py-1 rounded-xl text-center border border-red-100 min-w-[60px]">
                            <p class="text-[8px] text-red-400 font-bold uppercase mb-1">গ্রুপ</p>
                            <p class="text-xl font-black text-red-600 leading-none">${d.g}</p>
                        </div>
                    </div>
                    <div class="grid grid-cols-2 gap-3 mb-5 text-center">
                        <div class="bg-slate-50 p-2 rounded-xl border border-slate-100">
                            <p class="text-[8px] uppercase font-bold text-slate-400 mb-1">শেষ দান</p>
                            <p class="text-[11px] font-bold text-slate-700">${status.last}</p>
                        </div>
                        <div class="${status.class} p-2 rounded-xl border">
                            <p class="text-[8px] uppercase font-bold opacity-70 mb-1">অবস্থা</p>
                            <p class="text-[11px] font-bold">${status.text}</p>
                        </div>
                    </div>
                    <a href="tel:${d.p}" class="w-full bg-red-600 text-white py-3 rounded-2xl font-bold flex justify-center items-center gap-2 shadow-lg text-sm active:scale-95 transition-all">📞 কল দিন</a>
                </div>`;
            });
        }

        function filterDonors() {
            let input = document.getElementById('searchInput').value.toLowerCase();
            let group = document.getElementById('groupFilter').value;
            let filtered = allDonors.filter(d => 
                (String(d.n).toLowerCase().includes(input) || String(d.l).toLowerCase().includes(input)) && 
                (group === "" || String(d.g).trim() === group)
            );
            displayDonors(filtered);
        }
        loadDonors();
    </script>
</body>
</html>
