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
        <h1 class="text-2xl font-bold uppercase">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-sm opacity-90 mt-1 italic">মানবতার কল্যাণে আমাদের রক্তদান</p>
        
        <div class="mt-4 pt-3 border-t border-red-400">
            <p class="text-md font-bold text-yellow-300">প্রতিষ্ঠাতা পরিচালক: মোঃ মেহেদী হাসান</p>
            <a href="tel:01888354739" class="inline-block mt-1 bg-white text-red-600 px-4 py-1 rounded-full text-xs font-bold shadow-md">📞 01888354739</a>
        </div>

        <div class="flex justify-center gap-4 mt-6">
            <button onclick="showMemberForm()" class="bg-white text-green-700 px-4 py-2 rounded-xl text-xs font-bold shadow-lg active:scale-95">➕ ডোনার হিসেবে যোগ দিন</button>
            <button onclick="showAdminLogin()" class="bg-red-800 text-white px-4 py-2 rounded-xl text-[10px] opacity-80 active:scale-95">🔑 অ্যাডমিন লগইন</button>
        </div>
    </div>

    <div id="memberPanel" class="hidden mx-4 my-6 p-6 bg-white border-t-8 border-green-500 rounded-3xl shadow-xl">
        <h2 class="font-bold text-green-700 mb-4 text-center">রক্তদাতা নিবন্ধন ফর্ম</h2>
        <div class="space-y-3">
            <input type="text" id="mName" placeholder="আপনার নাম" class="w-full p-3 border rounded-xl text-sm outline-none">
            <input type="text" id="mLoc" placeholder="আপনার ঠিকানা / এলাকা" class="w-full p-3 border rounded-xl text-sm outline-none">
            <select id="mGroup" class="w-full p-3 border rounded-xl text-sm outline-none bg-white font-bold text-red-600">
                <option value="A+">A+</option><option value="B+">B+</option><option value="O+">O+</option><option value="AB+">AB+</option>
                <option value="A-">A-</option><option value="B-">B-</option><option value="O-">O-</option><option value="AB-">AB-</option>
            </select>
            <input type="text" id="mPhone" placeholder="মোবাইল নম্বর" class="w-full p-3 border rounded-xl text-sm outline-none">
            <button onclick="saveDonor('member')" id="mSaveBtn" class="w-full bg-green-600 text-white py-3 rounded-xl font-bold shadow-lg">নিবন্ধন সম্পন্ন করুন</button>
            <button onclick="hideForms()" class="w-full text-gray-400 text-xs py-2">পরে করব</button>
        </div>
    </div>

    <div id="adminModal" class="fixed inset-0 bg-black/60 hidden z-50 flex items-center justify-center p-4">
        <div class="bg-white p-6 rounded-3xl w-full max-w-sm shadow-2xl text-center">
            <h2 class="font-bold text-lg mb-4">অ্যাডমিন প্রবেশ</h2>
            <input type="password" id="adminPass" placeholder="পাসওয়ার্ড দিন" class="w-full p-3 border rounded-xl mb-4 text-center outline-none">
            <div class="flex gap-2">
                <button onclick="login()" class="flex-1 bg-red-600 text-white py-2 rounded-xl font-bold">প্রবেশ</button>
                <button onclick="hideForms()" class="flex-1 bg-gray-100 py-2 rounded-xl text-gray-500">বাতিল</button>
            </div>
        </div>
    </div>

    <div class="m-4 p-4 bg-white shadow-md rounded-2xl border border-gray-100 sticky top-4 z-10">
        <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা এলাকা লিখে খুঁজুন..." class="w-full p-3 border border-gray-200 rounded-xl mb-3 outline-none focus:ring-2 focus:ring-red-500 text-sm text-center">
        <select id="groupFilter" onchange="filterDonors()" class="w-full p-3 border border-gray-200 rounded-xl font-bold text-red-600 outline-none text-sm bg-white text-center">
            <option value="">সব রক্তের গ্রুপ</option>
            <option value="A+">A+</option><option value="B+">B+</option><option value="O+">O+</option><option value="AB+">AB+</option>
            <option value="A-">A-</option><option value="B-">B-</option><option value="O-">O-</option><option value="AB-">AB-</option>
        </select>
    </div>

    <div id="loading" class="text-center py-10">
        <div class="animate-spin inline-block w-8 h-8 border-4 border-red-500 border-t-transparent rounded-full mb-2"></div>
    </div>
    <div id="donorList" class="p-4 grid gap-4 hidden"></div>

    <script>
        const url = "https://script.google.com/macros/s/AKfycbznnAxsJMmct9dHPshnQit0ldFPOB8P-dQx05_ncHXusEDVJ5lK98Q2xPfosG31hlo/exec";
        let allDonors = [];

        function showMemberForm() { hideForms(); document.getElementById('memberPanel').classList.remove('hidden'); window.scrollTo(0, 300); }
        function showAdminLogin() { hideForms(); document.getElementById('adminModal').classList.remove('hidden'); }
        function hideForms() { 
            document.getElementById('memberPanel').classList.add('hidden'); 
            document.getElementById('adminModal').classList.add('hidden'); 
        }

        function login() {
            if(document.getElementById('adminPass').value === "1234") {
                showMemberForm(); // অ্যাডমিন লগইন করলে মেম্বার ফর্মই দেখাবে কিন্তু সে চাইলে সব ডাটা এডিট বা বেশি সুবিধা পাবে
                hideForms();
                document.getElementById('memberPanel').classList.remove('hidden');
                alert("স্বাগতম অ্যাডমিন!");
            } else { alert("ভুল পাসওয়ার্ড!"); }
        }

        async function saveDonor(type) {
            const btn = document.getElementById('mSaveBtn');
            const data = {
                name: document.getElementById('mName').value,
                location: document.getElementById('mLoc').value,
                group: document.getElementById('mGroup').value,
                phone: document.getElementById('mPhone').value,
                lastDate: "" // মেম্বাররা তারিখ পরে শিট থেকে আপডেট হবে অথবা তারা আজকের তারিখ দিতে পারে
            };

            if(!data.name || !data.phone) return alert("দয়া করে নাম এবং ফোন নম্বর দিন!");
            btn.innerText = "প্রসেসিং..."; btn.disabled = true;

            try {
                await fetch(url, { method: 'POST', body: JSON.stringify(data) });
                alert("নিবন্ধন সফল হয়েছে! মানবতার সাথে থাকার জন্য ধন্যবাদ।");
                location.reload();
            } catch (e) {
                alert("সমস্যা হয়েছে! আবার চেষ্টা করুন।");
                btn.innerText = "নিবন্ধন সম্পন্ন করুন"; btn.disabled = false;
            }
        }

        // লোডিং এবং ডিসপ্লে ফাংশন (আগের মতোই থাকবে)
        async function loadDonors() {
            try {
                const response = await fetch(url);
                allDonors = await response.json();
                displayDonors(allDonors);
                document.getElementById('loading').classList.add('hidden');
                document.getElementById('donorList').classList.remove('hidden');
            } catch (e) { }
        }

        function displayDonors(data) {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            data.forEach(d => {
                list.innerHTML += `<div class="bg-white p-5 rounded-3xl shadow-sm border border-gray-100 relative mb-2">
                    <span class="absolute top-0 right-0 bg-gray-100 text-gray-400 text-[9px] px-3 py-1 rounded-bl-2xl font-bold italic">SL: ${d.sl}</span>
                    <div class="flex justify-between items-start mb-4 mt-2">
                        <div class="w-2/3">
                            <h3 class="font-bold text-lg text-gray-800">${d.n}</h3>
                            <p class="text-[10px] text-gray-400 mt-1">📍 ${d.l}</p>
                        </div>
                        <div class="bg-red-50 px-3 py-1 rounded-xl text-center border border-red-100">
                            <p class="text-[8px] text-red-400 font-bold uppercase mb-1">গ্রুপ</p>
                            <p class="text-xl font-black text-red-600 leading-none">${d.g}</p>
                        </div>
