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

    <div class="bg-red-600 text-white p-8 text-center shadow-lg">
        <h1 class="text-3xl font-bold uppercase tracking-tight">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-sm opacity-90 mt-1 italic">মানবতার কল্যাণে আমাদের রক্তদান</p>
        
        <div class="mt-6 pt-4 border-t border-red-400">
            <p class="text-lg font-bold text-yellow-300">প্রতিষ্ঠাতা পরিচালক: মোঃ মেহেদী হাসান</p>
            <a href="tel:01888354739" class="inline-block mt-2 bg-white text-red-600 px-6 py-2 rounded-full text-sm font-bold shadow-md">📞 কল করুন: 01888354739</a>
        </div>
    </div>

    <div class="m-4 p-4 bg-white shadow-md rounded-2xl border border-gray-100">
        <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা এলাকা লিখে খুঁজুন..." class="w-full p-3 border border-gray-200 rounded-xl mb-3 outline-none text-sm text-center shadow-inner focus:border-red-300">
        <select id="groupFilter" onchange="filterDonors()" class="w-full p-3 border border-gray-200 rounded-xl font-bold text-red-600 outline-none text-sm bg-white text-center cursor-pointer">
            <option value="">সব রক্তের গ্রুপ</option>
            <option value="A+">A+</option><option value="B+">B+</option><option value="O+">O+</option><option value="AB+">AB+</option>
            <option value="A-">A-</option><option value="B-">B-</option><option value="O-">O-</option><option value="AB-">AB-</option>
        </select>
    </div>

    <div id="loading" class="text-center py-20 text-gray-400 animate-pulse">তথ্য লোড হচ্ছে...</div>
    <div id="donorList" class="p-4 grid gap-4 hidden"></div>

    <script>
        const url = "https://script.google.com/macros/s/AKfycbzbHWB2_5S77EH29tXH9JGIZD7CiotsAYtUl778hn36ymr2OoQ_-N31X8K_NNr7uOag/exec"; 
        let allDonors = [];

        function getStatus(lastDateStr) {
            if (!lastDateStr || lastDateStr.trim() === "" || lastDateStr === "undefined" || lastDateStr === "N/A") {
                return { text: "N/A", class: "text-gray-400 bg-gray-50 border-gray-100", last: "তারিখ আপডেট করা হয়নি" };
            }
            const lastDate = new Date(lastDateStr);
            if (isNaN(lastDate)) return { text: "N/A", class: "text-gray-400 bg-gray-50", last: "তারিখ আপডেট করা হয়নি" };
            
            const diffDays = Math.floor((new Date() - lastDate) / (1000 * 60 * 60 * 24));
            const formatted = lastDate.toLocaleDateString('en-GB', { day: '2-digit', month: 'short', year: 'numeric' });
            
            if (diffDays >= 90) return { text: "রক্ত দিতে পারবে", class: "text-green-600 bg-green-50 border-green-100", last: formatted };
            return { text: (90 - diffDays) + " দিন বাকি", class: "text-red-600 bg-red-50 border-red-100", last: formatted };
        }

        async function loadDonors() {
            try {
                const response = await fetch(url);
                allDonors = await response.json();
                displayDonors(allDonors);
                document.getElementById('loading').classList.add('hidden');
                document.getElementById('donorList').classList.remove('hidden');
            } catch (e) {
            
