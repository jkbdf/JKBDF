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

    <div id="loading" class="text-center py-20">
        <div class="animate-spin inline-block w-8 h-8 border-4 border-red-500 border-t-transparent rounded-full mb-2"></div>
        <p class="text-gray-500 font-bold">সার্ভার থেকে তথ্য আসছে...</p>
    </div>

    <div id="donorList" class="p-4 grid gap-4 pb-24 hidden"></div>

    <script>
        const url = "https://script.google.com/macros/s/AKfycbyT5Wy8zwAZw30r3bNetoQnhhvlxuWYsf8yaBQx_rQwWMCOy5UvmBI8M3jgbVT-7qUc/exec";
        let allDonors = [];

        // ৩ মাস বা ৯০ দিন গণনার ফাংশন
        function getDonationStatus(lastDateStr) {
            if (!lastDateStr || lastDateStr === "" || lastDateStr === "N/A") return { text: "তথ্য নেই", class: "text-gray-500 bg-gray-50 border-gray-100" };
            
            const lastDate = new Date(lastDateStr);
            if (isNaN(lastDate)) return { text: "সঠিক তারিখ নেই", class: "text-gray-500 bg-gray-50" };

            const today = new Date();
            const diffTime = (today - lastDate);
            const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)); 

            if (diffDays >= 90) {
                return { text: "রক্ত দিতে পারবে", class: "text-green-600 bg-green-50 border-green-100" };
            } else {
                return { text: "৩ মাস হয়নি", class: "text-red-600 bg-red-50 border-red-100" };
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
                document.getElementById('loading').innerHTML = "<p class='text-red-500'>সার্ভার কানেকশন সমস্যা!</p>";
            }
        }

        function displayDonors(data) {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            
            data.forEach(d => {
                const status = getDonationStatus(d.last);
                list.innerHTML += `
                    <div class="bg-white p-5 rounded-3xl shadow-sm border border-gray-100 relative mb-2">
                        <span class="absolute top-0 right-0 bg-gray-100 text-gray-400 text-[9px] px-3 py-1 rounded-bl-2xl font-bold">SL: ${d.sl}</span>
                        <div class="flex justify-between items-start mb-4 mt-2">
                            <div class="w-2/3">
                                <h3 class="font-bold text-xl text-gray-800 leading-tight">${d.n}</h3>
                                <p class="text-xs text-gray-500 mt-1">📍 ${d.l}</p>
                            </div>
                            <div class="bg-red-50 px-4 py-2 rounded-2xl text-center border border-red-100 min-w-[65px]">
                                <p class="text-[10px] text-red-400 font-bold uppercase mb-1">গ্রুপ</p>
                                <p class="text-2xl font-black text-red-600 leading-none">${d.g}</p>
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-3 mb-5 text-center">
                            <div class="bg-slate-50 p-2 rounded-xl border border-slate-100">
                                <p class="text-[9px] text-slate-400 font-bold uppercase mb-1">শেষ রক্তদান</p>
                                <p class="text-xs font-bold text-slate-700">${d.last || 'N/A'}</p>
                            </div>
                            <div class="${status.class} p-2 rounded-xl border">
                                <p class="text-[9px] uppercase font-bold mb-1 opacity-70">বর্তমান অবস্থা</p>
                                <p class="text-xs font-bold">${status.text}</p>
                            </div>
                        </div>
                        <a href="tel:${d.p}" class="w-full bg-green-600 text-white py-4 rounded-2xl font-bold flex justify-center items-center gap-2 shadow-lg active:bg-green-700">
                            📞 সরাসরি কল দিন
                        </a>
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
