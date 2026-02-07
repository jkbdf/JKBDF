<script>
    const url = "https://script.google.com/macros/s/AKfycbxmaoWmQ5kDH4IuoF71Wlsou49yUsrF0Vm9fFbtZ1msFynM84zacS3bUcSVo86oEQKH/exec";

    // তারিখ সুন্দর করার ফাংশন
    function formatDate(dateStr) {
        if (!dateStr || dateStr === "N/A") return "জানা নেই";
        try {
            const date = new Date(dateStr);
            if (isNaN(date)) return dateStr; // যদি তারিখ না হয়ে সাধারণ টেক্সট হয়
            
            const options = { day: '2-digit', month: 'short', year: 'numeric' };
            return date.toLocaleDateString('en-GB', options); // আউটপুট: 07 Feb 2026
        } catch (e) {
            return dateStr;
        }
    }

    async function loadDonors() {
        try {
            const response = await fetch(url);
            const data = await response.json();
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            
            data.forEach(d => {
                list.innerHTML += `
                    <div class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100 relative mb-4">
                        <span class="absolute top-0 right-0 bg-gray-100 text-gray-500 text-[10px] px-3 py-1 rounded-bl-xl font-bold italic">SL: ${d.sl}</span>
                        <div class="flex justify-between items-start mb-4 mt-2">
                            <div>
                                <h3 class="font-bold text-lg text-gray-800">${d.n}</h3>
                                <p class="text-xs text-gray-500 mt-1">📍 ${d.l}</p>
                            </div>
                            <div class="bg-red-50 px-3 py-1 rounded-lg text-center border border-red-100">
                                <p class="text-[10px] text-red-400 font-bold uppercase mb-1 leading-none">গ্রুপ</p>
                                <p class="text-xl font-black text-red-600 leading-none">${d.g}</p>
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-3 mb-4 text-center text-[11px]">
                            <div class="bg-slate-50 p-2 rounded-xl text-gray-600">
                                <b>শেষ দান:</b><br>${formatDate(d.last)}
                            </div>
                            <div class="bg-green-50 p-2 rounded-xl text-green-700">
                                <b>পরবর্তী:</b><br>${formatDate(d.next)}
                            </div>
                        </div>
                        <a href="tel:${d.p}" class="w-full bg-green-600 text-white py-3 rounded-xl font-bold flex justify-center items-center gap-2 shadow-md hover:bg-green-700 transition-all">
                            📞 সরাসরি কল করুন
                        </a>
                    </div>`;
            });
        } catch (e) {
            document.getElementById('donorList').innerHTML = "<p class='text-center text-red-500 p-10'>তথ্য লোড করতে সমস্যা হয়েছে।</p>";
        }
    }
    loadDonors();
</script>
