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
        .handwritten-box { border: 1px dashed #cbd5e1; background-color: #fff; }
    </style>
</head>
<body class="pb-20">

    <div class="hero-gradient text-white pb-12 pt-8 px-4 rounded-b-[50px] shadow-2xl text-center">
        <div class="flex justify-center mb-5">
            <div class="bg-white p-1 rounded-full shadow-2xl border-4 border-red-500/20">
                <img src="logo.png" onerror="this.src='https://i.ibb.co/C3m2X9Y/1000001730.png'" alt="Logo" class="w-24 h-24 md:w-28 md:h-28 rounded-full object-contain">
            </div>
        </div>
        <h1 class="text-2xl md:text-4xl font-bold uppercase tracking-tight mb-1">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-[10px] md:text-xs opacity-90 font-medium italic mb-6">জীবন দিয়ে জীবন নয়, রক্ত দিয়ে জীবন জয়</p>
        
        <div class="bg-white/10 backdrop-blur-lg rounded-[30px] p-5 inline-block border border-white/20 shadow-xl">
            <p class="text-sm md:text-base font-bold text-yellow-300 mb-4">প্রতিষ্ঠাতা পরিচালক: মোঃ মেহেদী হাসান</p>
            <div class="flex flex-wrap justify-center gap-3">
                <a href="tel:01888354739" class="bg-white text-red-600 px-6 py-2.5 rounded-2xl text-xs font-black shadow-lg flex items-center gap-2 active:scale-95 transition-all">📞 01888354739</a>
                <a href="https://facebook.com/groups/jubokolyan.bdf/" target="_blank" class="bg-blue-600 text-white px-6 py-2.5 rounded-2xl text-xs font-black shadow-lg flex items-center gap-2 active:scale-95 transition-all">👥 ফেসবুক গ্রুপ</a>
            </div>
        </div>
    </div>

    <div class="mx-6 -mt-10 p-6 bg-white shadow-2xl rounded-[35px] border border-gray-100 relative z-10">
        <div class="grid grid-cols-1 gap-3">
            <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা এলাকা লিখে খুঁজুন..." class="w-full pl-6 pr-4 py-4 bg-gray-50 border border-gray-100 rounded-[20px] outline-none text-sm focus:border-red-500 shadow-inner">
            <select id="groupFilter" onchange="filterDonors()" class="w-full p-4 bg-gray-50 border border-gray-100 rounded-[20px] font-bold text-red-600 outline-none text-sm cursor-pointer text-center shadow-inner appearance-none">
                <option value="">🩸 সব রক্তের গ্রুপ</option>
                <option value="A+">A+</option><option value="B+">B+</option><option value="O+">O+</option><option value="AB+">AB+</option>
                <option value="A-">A-</option><option value="B-">B-</option><option value="O-">O-</option><option value="AB-">AB-</option>
            </select>
        </div>
    </div>

    <div id="loading" class="text-center py-20">
        <div class="inline-block animate-spin rounded-full h-10 w-10 border-4 border-red-600 border-t-transparent"></div>
        <p class="mt-3 text-gray-500 font-bold">সার্ভার থেকে তথ্য আসছে...</p>
    </div>
    
    <div id="donorList" class="p-4 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5 hidden"></div>

    <script>
        const url = "https://script.google.com/macros/s/AKfycbzbHWB2_5S77EH29tXH9JGIZD7CiotsAYtUl778hn36ymr2OoQ_-N31X8K_NNr7uOag/exec"; 
        let allDonors = [];

        async function loadDonors() {
            const loadingDiv = document.getElementById('loading');
            try {
                const response = await fetch(url);
                const data = await response.json();
                
                allDonors = data.map(d => ({
                    n: d.n || d.Name || d.name || "অজানা নাম",
                    l: d.l || d.Location || d.address || "ঠিকানা নেই",
                    g: d.g || d.Group || d["Blood Group"] || "N/A",
                    p: d.p || d.Phone || d.Contact || "",
                    last: d.last || d.LastDate || d["Last Donation"] || "",
                    note: d.note || d.Comments || d["মন্তব্য"] || ""
                }));

                if (allDonors.length > 0) {
                    displayDonors(allDonors);
                    loadingDiv.classList.add('hidden');
                    document.getElementById('donorList').classList.remove('hidden');
                } else {
                    loadingDiv.innerHTML = "শিটে কোনো ডাটা নেই!";
                }
            } catch (e) { 
                loadingDiv.innerHTML = "<p class='text-red-500'>তথ্য পাওয়া যাচ্ছে না।</p>"; 
            }
        }

        function getStatus(lastDateStr) {
            if (!lastDateStr || lastDateStr.trim() === "" || lastDateStr === "undefined" || lastDateStr === "N/A") {
                return { text: "রক্ত দিতে পারবে", class: "text-green-600 bg-green-50 border-green-200", last: "এখনও রক্ত দেয়নি" };
            }
            const lastDate = new Date(lastDateStr);
            if (isNaN(lastDate)) return { text: "রক্ত দিতে পারবে", class: "text-green-600 bg-green-50 border-green-200", last: lastDateStr };
            
            const diffDays = Math.floor((new Date() - lastDate) / (1000 * 60 * 60 * 24));
            const formatted = lastDate.toLocaleDateString('en-GB', { day: '2-digit', month: 'short', year: 'numeric' });
            
            if (diffDays >= 90) return { text: "রক্ত দিতে পারবে", class: "text-green-600 bg-green-50 border-green-200", last: formatted };
            return { text: (90 - diffDays) + " দিন বাকি", class: "text-red-600 bg-red-50 border-red-200", last: formatted };
        }

        function displayDonors(data) {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            data.forEach((d, index) => {
                const status = getStatus(d.last);
                list.innerHTML += `
                <div class="bg-white rounded-[40px] shadow-sm border border-gray-100 p-7 hover:shadow-xl transition-all relative overflow-hidden">
                    <div class="absolute top-4 left-4 bg-gray-100 text-gray-500 w-7 h-7 rounded-full flex items-center justify-center text-[10px] font-bold border border-white shadow-sm">
                        ${index + 1}
                    </div>
                    
                    <div class="flex justify-between items-center mb-5">
                        <div class="bg-red-600 text-white w-16 h-16 rounded-[22px] flex items-center justify-center font-black text-2xl shadow-lg ring-4 ring-red-50">${d.g}</div>
                        <div class="text-right">
                            <h3 class="font-bold text-xl text-gray-800">${d.n}</h3>
                            <p class="text-sm text-gray-500 font-semibold">📍 ${d.l}</p>
                        </div>
                    </div>

                    <div class="grid grid-cols-2 gap-4 mb-4 text-center">
                        <div class="handwritten-box p-3 rounded-2xl shadow-inner">
                            <p class="text-[9px] uppercase font-black text-blue-500 tracking-wider">রক্তদানের তারিখ</p>
                            <p class="text-[11px] font-bold text-gray-700 mt-1 underline decoration-dotted">${status.last}</p>
                        </div>
                        <div class="${status.class} p-3 rounded-2xl shadow-sm border">
                            <p class="text-[9px] uppercase font-black opacity-60 tracking-wider">বর্তমান অবস্থা</p>
                            <p class="text-[11px] font-bold mt-1">${status.text}</p>
                        </div>
                    </div>

                    ${d.note ? `<div class="bg-yellow-50 p-2 rounded-xl mb-4 border border-yellow-100 text-[10px] text-gray-600 italic text-center">"${d.note}"</div>` : ''}

                    <a href="tel:${d.p}" class="flex items-center justify-center gap-2 w-full bg-red-600 text-white py-4 rounded-[20px] font-black text-sm shadow-xl active:scale-95 transition-all">📞 কল দিন</a>
                </div>`;
            });
        }

        function filterDonors() {
            let input = document.getElementById('searchInput').value.toLowerCase();
            let group = document.getElementById('groupFilter').value;
            let filtered = allDonors.filter(d => 
                (d.n.toLowerCase().includes(input) || d.l.toLowerCase().includes(input)) && 
                (group === "" || d.g.trim() === group)
            );
            displayDonors(filtered);
        }
        loadDonors();
    </script>
</body>
</html>
