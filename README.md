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
    </style>
</head>
<body class="pb-20">

    <div class="hero-gradient text-white pb-10 pt-6 px-4 rounded-b-[40px] shadow-2xl text-center">
        <div class="flex justify-center mb-4">
            <div class="bg-white p-1 rounded-full shadow-lg">
                <img src="https://raw.githubusercontent.com/mehedi131600-cmd/mehedi131600-cmd.github.io/main/logo.png" 
                     alt="Logo" 
                     onerror="this.src='https://i.ibb.co/vxsL88m/logo.png'"
                     class="w-20 h-20 rounded-full object-contain">
            </div>
        </div>
        
        <h1 class="text-2xl md:text-4xl font-bold uppercase tracking-wide mb-2">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-xs opacity-80 italic mb-4">মানবতার কল্যাণে আমাদের রক্তদান</p>
        
        <div class="bg-black/20 backdrop-blur-sm rounded-2xl p-4 inline-block border border-white/10">
            <p class="text-sm font-semibold text-yellow-300 mb-3">প্রতিষ্ঠাতা পরিচালক: মোঃ মেহেদী হাসান</p>
            <div class="flex flex-wrap justify-center gap-3">
                <a href="tel:01888354739" class="bg-white text-red-600 px-5 py-2 rounded-xl text-xs font-bold shadow-lg flex items-center gap-2 active:scale-95 transition-all">
                    📞 01888354739
                </a>
                <a href="https://facebook.com/groups/jubokolyan.bdf/" target="_blank" class="bg-blue-600 text-white px-5 py-2 rounded-xl text-xs font-bold shadow-lg flex items-center gap-2 active:scale-95 transition-all">
                    👥 ফেসবুক গ্রুপ
                </a>
            </div>
        </div>
    </div>

    <div class="mx-4 -mt-6 p-5 bg-white shadow-xl rounded-3xl border border-gray-100 relative z-10">
        <div class="flex flex-col gap-3">
            <div class="relative">
                <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা এলাকা লিখে খুঁজুন..." 
                       class="w-full pl-10 pr-4 py-3 bg-gray-50 border border-gray-200 rounded-2xl outline-none text-sm focus:border-red-500 transition-all">
                <span class="absolute left-3 top-3.5 opacity-30 text-lg">🔍</span>
            </div>
            <select id="groupFilter" onchange="filterDonors()" 
                    class="w-full p-3 bg-gray-50 border border-gray-200 rounded-2xl font-bold text-red-600 outline-none text-sm cursor-pointer appearance-none text-center">
                <option value="">🩸 সব রক্তের গ্রুপ</option>
                <option value="A+">A+</option><option value="B+">B+</option><option value="O+">O+</option><option value="AB+">AB+</option>
                <option value="A-">A-</option><option value="B-">B-</option><option value="O-">O-</option><option value="AB-">AB-</option>
            </select>
        </div>
    </div>

    <div id="loading" class="text-center py-20">
        <div class="inline-block animate-spin rounded-full h-8 w-8 border-4 border-red-600 border-t-transparent"></div>
        <p class="mt-2 text-gray-500 font-semibold">তথ্য লোড হচ্ছে...</p>
    </div>
    
    <div id="donorList" class="p-4 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 hidden"></div>

    <script>
        const url = "https://script.google.com/macros/s/AKfycbzbHWB2_5S77EH29tXH9JGIZD7CiotsAYtUl778hn36ymr2OoQ_-N31X8K_NNr7uOag/exec"; 
        let allDonors = [];

        async function loadDonors() {
            const loadingDiv = document.getElementById('loading');
            try {
                const response = await fetch(url);
                allDonors = await response.json();
                if (allDonors && allDonors.length > 0) {
                    displayDonors(allDonors);
                    loadingDiv.classList.add('hidden');
                    document.getElementById('donorList').classList.remove('hidden');
                } else {
                    loadingDiv.innerHTML = "<p class='text-gray-400'>শিটে কোনো ডাটা পাওয়া যায়নি।</p>";
                }
            } catch (e) { 
                loadingDiv.innerHTML = "<p class='text-red-500 font-bold'>সার্ভার সংযোগ বিচ্ছিন্ন!</p>"; 
            }
        }

        function getStatus(lastDateStr) {
            if (!lastDateStr || lastDateStr.trim() === "" || lastDateStr === "undefined") {
                return { text: "N/A", class: "text-gray-400 bg-gray-100", last: "তারিখ আপডেট করা হয়নি" };
            }
            const lastDate = new Date(lastDateStr);
            if (isNaN(lastDate)) return { text: "N/A", class: "text-gray-400 bg-gray-100", last: "তারিখ আপডেট করা হয়নি" };
            
            const diffDays = Math.floor((new Date() - lastDate) / (1000 * 60 * 60 * 24));
            const formatted = lastDate.toLocaleDateString('en-GB', { day: '2-digit', month: 'short', year: 'numeric' });
            
            if (diffDays >= 90) return { text: "রক্ত দিতে পারবে", class: "text-green-600 bg-green-50 border-green-200", last: formatted };
            return { text: (90 - diffDays) + " দিন বাকি", class: "text-red-600 bg-red-50 border-red-200", last: formatted };
        }

        function displayDonors(data) {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            data.forEach(d => {
                const status = getStatus(d.last);
                list.innerHTML += `
                <div class="bg-white rounded-[30px] shadow-sm border border-gray-100 overflow-hidden hover:shadow-md transition-shadow">
                    <div class="p-5">
                        <div class="flex justify-between items-center mb-4">
                            <div class="bg-red-600 text-white w-14 h-14 rounded-2xl flex items-center justify-center font-black text-2xl shadow-inner">
                                ${d.g}
                            </div>
                            <div class="text-right">
                                <h3 class="font-bold text-xl text-gray-800">${d.n}</h3>
                                <p class="text-sm text-gray-500 flex items-center justify-end gap-1">📍 ${d.l}</p>
                            </div>
                        </div>
                        
                        <div class="grid grid-cols-2 gap-3 mb-5">
                            <div class="bg-gray-50 p-2.5 rounded-2xl border border-gray-100 text-center">
                                <p class="text-[9px] uppercase font-bold text-gray-400">শেষ রক্তদান</p>
                                <p class="text-[11px] font-bold text-gray-700">${status.last}</p>
                            </div>
                            <div class="${status.class} p-2.5 rounded-2xl border text-center">
                                <p class="text-[9px] uppercase font-bold opacity-60">অবস্থা</p>
                                <p class="text-[11px] font-bold">${status.text}</p>
                            </div>
                        </div>
                        
                        <a href="tel:${d.p}" class="flex items-center justify-center gap-2 w-full bg-red-600 hover:bg-red-700 text-white py-3.5 rounded-2xl font-bold text-sm shadow-lg active:scale-[0.98] transition-all">
                            📞 কল দিন
                        </a>
                    </div>
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
