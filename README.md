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

    <div class="hero-gradient text-white pb-12 pt-8 px-4 rounded-b-[50px] shadow-2xl text-center">
        <div class="flex justify-center mb-5">
            <div class="bg-white p-1 rounded-full shadow-2xl border-4 border-red-500/20">
                <img src="https://lh3.googleusercontent.com/d/1I-DrnxE5DMaueSmBMWmCSAe2GqkL7j1b" 
                     alt="যুব কল্যাণ লোগো" 
                     class="w-24 h-24 md:w-28 md:h-28 rounded-full object-contain">
            </div>
        </div>
        
        <h1 class="text-2xl md:text-4xl font-bold uppercase tracking-tight mb-1">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-[10px] md:text-xs opacity-90 font-medium italic mb-6">জীবন দিয়ে জীবন নয়, রক্ত দিয়ে জীবন জয়</p>
        
        <div class="bg-white/10 backdrop-blur-lg rounded-[30px] p-5 inline-block border border-white/20 shadow-xl">
            <p class="text-sm md:text-base font-bold text-yellow-300 mb-4">প্রতিষ্ঠাতা পরিচালক: মোঃ মেহেদী হাসান</p>
            <div class="flex flex-wrap justify-center gap-3">
                <a href="tel:01888354739" class="bg-white text-red-600 px-6 py-2.5 rounded-2xl text-xs font-black shadow-lg flex items-center gap-2 active:scale-95 transition-all">
                    📞 01888354739
                </a>
                <a href="https://facebook.com/groups/jubokolyan.bdf/" target="_blank" class="bg-blue-600 text-white px-6 py-2.5 rounded-2xl text-xs font-black shadow-lg flex items-center gap-2 active:scale-95 transition-all">
                    👥 ফেসবুক গ্রুপ
                </a>
            </div>
        </div>
    </div>

    <div class="mx-6 -mt-10 p-6 bg-white shadow-2xl rounded-[35px] border border-gray-100 relative z-10">
        <div class="grid grid-cols-1 gap-3">
            <div class="relative">
                <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা এলাকা লিখে খুঁজুন..." 
                       class="w-full pl-12 pr-4 py-4 bg-gray-50 border border-gray-100 rounded-[20px] outline-none text-sm focus:border-red-500 transition-all shadow-inner">
                <span class="absolute left-4 top-4.5 opacity-40 text-xl">🔍</span>
            </div>
            <select id="groupFilter" onchange="filterDonors()" 
                    class="w-full p-4 bg-gray-50 border border-gray-100 rounded-[20px] font-bold text-red-600 outline-none text-sm cursor-pointer text-center shadow-inner appearance-none">
                <option value="">🩸 সব রক্তের গ্রুপ</option>
                <option value="A+">A+</option><option value="B+">B+</option><option value="O+">O+</option><option value="AB+">AB+</option>
                <option value="A-">A-</option><option value="B-">B-</option><option value="O-">O-</option><option value="AB-">AB-</option>
            </select>
        </div>
    </div>

    <div id="loading" class="text-center py-20">
        <div class="inline-block animate-spin rounded-full h-10 w-10 border-4 border-red-600 border-t-transparent"></div>
        <p class="mt-3 text-gray-500 font-bold">তথ্য লোড হচ্ছে...</p>
    </div>
    
    <div id="donorList" class="p-4 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5 hidden"></div>

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
                    loadingDiv.innerHTML = "<p class='text-gray-400 font-bold'>শিটে কোনো ডাটা পাওয়া যায়নি।</p>";
                }
            } catch (e) { 
                loadingDiv.innerHTML = "<p class='text-red-500 font-bold'>সার্ভার সংযোগ ত্রুটি!</p>"; 
            }
        }

        function getStatus(lastDateStr) {
            if (!lastDateStr || lastDateStr.trim() === "" || lastDateStr === "undefined") {
                return { text: "N/A", class: "text-gray-400 bg-gray-50", last: "তারিখ আপডেট করা হয়নি" };
            }
            const lastDate = new Date(lastDateStr);
            if (isNaN(lastDate)) return { text: "N/A", class: "text-gray-400 bg-gray-50", last: "তারিখ আপডেট করা হয়নি" };
            
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
                <div class="bg-white rounded-[40px] shadow-sm border border-gray-100 overflow-hidden hover:shadow-xl transition-all duration-300">
                    <div class="p-7">
                        <div class="flex justify-between items-center mb-5">
                            <div class="bg-red-600 text-white w-16 h-16 rounded-[22px] flex items-center justify-center font-black text-2xl shadow-lg ring-4 ring-red-50">
                                ${d.g}
                            </div>
                            <div class="text-right">
                                <h3 class="font-bold text-xl text-gray-800">${d.n}</h3>
                                <p class="text-sm text-gray-500 font-semibold flex items-
                                
