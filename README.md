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
        .donor-card { transition: transform 0.2s; }
        .donor-card:active { transform: scale(0.98); }
    </style>
</head>
<body class="bg-gray-50">

    <div class="bg-red-600 text-white p-6 text-center shadow-lg sticky top-0 z-20">
        <h1 class="text-2xl font-bold">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-sm opacity-90 mt-1">মানবতার কল্যাণে আমাদের রক্তদান</p>
    </div>

    <div class="p-4 bg-white shadow-md sticky top-[88px] z-10 border-b">
        <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা এলাকা লিখে খুঁজুন..." class="w-full p-3 border border-gray-300 rounded-xl mb-3 outline-none focus:ring-2 focus:ring-red-500 shadow-sm text-sm">
        
        <select id="groupFilter" onchange="filterDonors()" class="w-full p-3 border border-gray-300 rounded-xl font-bold text-red-600 outline-none shadow-sm text-sm bg-white">
            <option value="">সব রক্তের গ্রুপ</option>
            <option value="A+">A+</option>
            <option value="A-">A-</option>
            <option value="B+">B+</option>
            <option value="B-">B-</option>
            <option value="O+">O+</option>
            <option value="O-">O-</option>
            <option value="AB+">AB+</option>
            <option value="AB-">AB-</option>
        </select>
    </div>

    <div id="donorList" class="p-4 grid gap-4 pb-24">
        <div class="text-center py-10 text-gray-400">
            <div class="animate-spin inline-block w-8 h-8 border-4 border-red-500 border-t-transparent rounded-full mb-2"></div>
            <p>তথ্য লোড হচ্ছে...</p>
        </div>
    </div>

    <script>
        // আপনার জেনারেট করা লিঙ্কটি এখানে অলরেডি বসিয়ে দিয়েছি
        const url = "https://script.google.com/macros/s/AKfycbxmaoWmQ5kDH4IuoF71Wlsou49yUsrF0Vm9fFbtZ1msFynM84zacS3bUcSVo86oEQKH/exec";

        let allDonors = [];

        async function loadDonors() {
            try {
                const response = await fetch(url);
                allDonors = await response.json();
                displayDonors(allDonors);
            } catch (e) {
                document.getElementById('donorList').innerHTML = `
                    <div class="text-center text-red-500 p-10 bg-white rounded-xl shadow">
                        সার্ভার কানেকশন সমস্যা! ইন্টারনেট চেক করে আবার রিফ্রেশ দিন।
                    </div>`;
            }
        }

        function displayDonors(data) {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            
            if(data.length === 0) {
                list.innerHTML = "<p class='text-center text-gray-500 mt-10'>কোনো তথ্য পাওয়া যায়নি।</p>";
                return;
            }

            data.forEach(d => {
                list.innerHTML += `
                    <div class="donor-card bg-white p-4 rounded-2xl shadow-sm border border-gray-100 relative overflow-hidden">
                        <span class="absolute top-0 right-0 bg-gray-100 text-gray-500 text-[10px] px-3 py-1 rounded-bl-xl font-bold italic">SL: ${d.sl}</span>
                        
                        <div class="flex justify-between items-start mb-4">
                            <div class="mt-1">
                                <h3 class="font-bold text-lg text-gray-800 leading-tight">${d.n}</h3>
                                <p class="text-xs text-gray-500 mt-1 flex items-center gap-1">📍 ${d.l}</p>
                            </div>
                            <div class="bg-red-50 px-3 py-1 rounded-lg text-center border border-red-100">
                                <p class="text-[10px] text-red-400 font-bold uppercase leading-none mb-1">গ্রুপ</p>
                                <p class="text-xl font-black text-red-600 leading-none">${d.g}</p>
                            </div>
                        </div>
                        
                        <div class="grid grid-cols-2 gap-3 mb-4">
                            <div class="bg-slate-50 p-2 rounded-xl text-center border border-slate-100">
                                <p class="text-[9px] text-slate-400 uppercase font-bold mb-1">শেষ দান</p>
                                <p class="text-xs font-bold text-slate-700">${d.last || 'জানা নেই'}</p>
                            </div>
                            <div class="bg-green-50 p-2 rounded-xl text-center border border-green-100">
                                <p class="text-[9px] text-green-400 uppercase font-bold mb-1">পরবর্তী দান</p>
                                <p class="text-xs font-bold text-green-600">${d.next || 'এখনই পারবেন'}</p>
                            </div>
                        </div>

                        <a href="tel:${d.p}" class="w-full bg-green-600 text-white py-3 rounded-xl font-bold flex justify-center items-center gap-2 shadow-md hover:bg-green-700 active:bg-green-800 transition-colors">
                            📞 সরাসরি কল করুন
                        </a>
                    </div>
                `;
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
