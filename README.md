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
        .container-custom { width: 100%; padding: 0 15px; max-width: 500px; margin: 0 auto; }
        .info-row { display: flex; border-bottom: 1px solid rgba(0,0,0,0.05); padding: 8px 0; align-items: center; }
        .info-label { font-weight: 800; color: #4b5563; min-width: 115px; font-size: 14px; }
        .info-value { font-weight: 700; font-size: 14px; flex: 1; }
        
        /* লোগো সেকশন স্টাইল */
        .logo-box {
            width: 90px;
            height: 90px;
            border-radius: 50%;
            background: #fff;
            padding: 4px;
            box-shadow: 0 4px 15px rgba(220, 38, 38, 0.2);
            border: 3px solid #fee2e2;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .logo-box img {
            width: 100%;
            height: 100%;
            object-fit: contain;
        }

        .card-0 { border-top-color: #dc2626; } .card-1 { border-top-color: #2563eb; }
        .card-2 { border-top-color: #059669; } .card-3 { border-top-color: #7c3aed; }
        .card-4 { border-top-color: #db2777; }
        
        .sl-badge { font-size: 32px; font-weight: 900; line-height: 1; opacity: 0.8; }
        .text-red-custom { color: #dc2626; }
    </style>
</head>
<body class="pb-10">

    <div class="bg-white p-6 shadow-md text-center flex flex-col items-center mb-4 border-b-2 border-red-50">
        <div class="logo-box mb-3">
            <img src="https://i.ibb.co/C3m2X9Y/1000001730.png" alt="JKBF Logo">
        </div>
        <h1 class="text-xl font-black text-red-600">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
    </div>

    <div id="loginPage" class="container-custom">
        <div class="bg-white p-6 rounded-[35px] shadow-xl text-center border">
            <h2 class="text-lg font-bold text-gray-800 mb-5">লগইন করুন</h2>
            <div class="flex gap-2 mb-4 p-1 bg-gray-100 rounded-xl">
                <button onclick="setRole('Member')" id="roleMember" class="flex-1 py-2 rounded-lg text-xs font-bold bg-white text-red-600 shadow-sm">সদস্য</button>
                <button onclick="setRole('Admin')" id="roleAdmin" class="flex-1 py-2 rounded-lg text-xs font-bold text-gray-500">এডমিন</button>
            </div>
            <div id="memberFields">
                <input type="tel" id="uPhone" placeholder="মোবাইল নম্বর" class="w-full p-4 mb-4 border rounded-2xl text-center font-bold outline-none focus:border-red-500 bg-gray-50">
            </div>
            <div id="adminFields" class="hidden">
                <input type="password" id="uPass" placeholder="এডমিন পাসওয়ার্ড" class="w-full p-4 mb-4 border rounded-2xl text-center font-bold outline-none focus:border-red-500 bg-gray-50">
            </div>
            <button onclick="handleLogin()" id="lBtn" class="w-full bg-red-600 text-white py-4 rounded-2xl font-bold shadow-lg mb-6">লগইন করুন</button>
            
            <div class="flex flex-col items-center gap-0 border-t pt-4 border-dashed border-gray-200">
                <p class="text-[11px] font-bold text-gray-500">সদস্য না হয়ে থাকলে</p>
                <button onclick="showReg()" class="text-xl font-black text-red-600">রেজিস্ট্রেশন করুন</button>
            </div>
            <p id="lErr" class="text-red-500 text-xs mt-3 hidden font-bold"></p>
        </div>
    </div>

    <div id="regPage" class="container-custom hidden">
        <div class="bg-white p-6 rounded-[35px] shadow-xl border">
            <h2 class="text-lg font-bold text-gray-800 mb-5 text-center">রেজিস্ট্রেশন করুন</h2>
            <input type="text" id="regName" placeholder="সম্পূর্ণ নাম" class="w-full p-3 mb-3 border rounded-xl font-bold bg-gray-50">
            <select id="regGroup" class="w-full p-3 mb-3 border rounded-xl font-bold bg-white">
                <option value="">রক্তের গ্রুপ</option>
                <option value="A+">A+</option><option value="A-">A-</option>
                <option value="B+">B+</option><option value="B-">B-</option>
                <option value="O+">O+</option><option value="O-">O-</option>
                <option value="AB+">AB+</option><option value="AB-">AB-</option>
            </select>
            <input type="text" id="regLoc" placeholder="ঠিকানা (গ্রাম, উপজেলা, জেলা)" class="w-full p-3 mb-3 border rounded-xl font-bold bg-gray-50">
            <input type="tel" id="regPhone" placeholder="মোবাইল নম্বর" class="w-full p-3 mb-3 border rounded-xl font-bold bg-gray-50">
            <p class="text-[10px] text-gray-500 mb-1 px-1 font-bold">সর্বশেষ রক্তদান (না দিয়ে থাকলে ফাঁকা রাখুন)</p>
            <input type="date" id="regLast" class="w-full p-3 mb-5 border rounded-xl font-bold bg-gray-50">
            <button onclick="handleRegister()" id="rBtn" class="w-full bg-green-600 text-white py-4 rounded-2xl font-bold shadow-lg">রেজিস্ট্রেশন সম্পন্ন করুন</button>
            <button onclick="location.reload()" class="w-full text-gray-500 mt-4 text-sm font-bold">ফিরে যান</button>
        </div>
    </div>

    <div id="mainPage" class="hidden">
        <div class="hero-gradient text-white p-8 rounded-b-[40px] text-center relative mb-6 shadow-xl border-b-4 border-red-700">
            <button onclick="location.reload()" class="absolute top-4 right-4 text-[11px] bg-white/20 px-3 py-1 rounded-full border border-white/30 font-bold">লগ আউট</button>
            <div class="mt-2">
                <h2 id="welcome" class="text-2xl font-black text-yellow-300"></h2>
                <p class="text-xs mt-1 text-white/80 font-bold tracking-widest uppercase">যুব কল্যাণ রক্তদান ফাউন্ডেশন</p>
            </div>
        </div>
        <div class="container-custom mb-4">
            <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা গ্রুপ দিয়ে খুঁজুন..." class="w-full p-4 rounded-2xl border-none shadow-md font-bold focus:ring-2 focus:ring-red-500 outline-none">
        </div>
        <div id="donorList" class="container-custom grid gap-6"></div>
    </div>

    <script>
        const scriptURL = "https://script.google.com/macros/s/AKfycbwaIFFoE5Kzs9BoJa6JOADxdDtM-k62CgFD2phNOhQ6vat0d3a7s5w_TiXHMmfia2B3/exec"; 
        let allDonors = [], loggedUser = null, currentRole = 'Member';

        function showReg() { 
            document.getElementById('loginPage').classList.add('hidden'); 
            document.getElementById('regPage').classList.remove('hidden'); 
        }

        function setRole(r) { 
            currentRole = r; 
            document.getElementById('memberFields').classList.toggle('hidden', r==='Admin'); 
            document.getElementById('adminFields').classList.toggle('hidden', r==='Member'); 
            document.getElementById('roleMember').classList.toggle('bg-white', r==='Member'); 
            document.getElementById('roleAdmin').classList.toggle('bg-white', r==='Admin'); 
        }

        async function handleLogin() {
            const phone = document.getElementById('uPhone').value.trim();
            const pass = document.getElementById('uPass').value.trim();
            const err = document.getElementById('lErr');
            err.innerText = "⏳ ডাটা যাচাই হচ্ছে..."; err.classList.remove('hidden');
            try {
                const res = await fetch(scriptURL); 
                allDonors = await res.json();
                if(currentRole === 'Admin' && pass === 'Mehedi4739') { 
                    loggedUser = { n: "এডমিন", role: "Admin" }; 
                    showMain(); 
                } else {
                    const user = allDonors.find(d => String(d.p).slice(-10) === phone.slice(-10));
                    if(user) { 
                        loggedUser = { ...user, role: "Member" }; 
                        showMain(); 
                    } else { 
                        err.innerText = "❌ মেম্বার পাওয়া যায়নি!"; 
                    }
                }
            } catch (e) { err.innerText = "❌ সার্ভার এরর!"; }
        }

        async function handleRegister() {
            const n = document.getElementById('regName').value;
            const g = document.getElementById('regLoc').value;
            const l = document.getElementById('regGroup').value;
            const p = document.getElementById('regPhone').value.trim();
            const last = document.getElementById('regLast').value;

            if(!n || !g || !l || !p) return alert("সব তথ্য দিন!");
            
            document.getElementById('rBtn').innerText = "⏳ তথ্য যাচাই হচ্ছে...";
            document.getElementById('rBtn').disabled = true;

            try {
                const res = await fetch(scriptURL);
                const data = await res.json();
                const exists = data.find(d => String(d.p).slice(-10) === p.slice(-10));
                
                if(exists) {
                    alert("রেজিস্ট্রেশন করা আছে আপনি সরাসরি লগইন করুন");
                    location.reload(); 
                    return;
                }

                await fetch(scriptURL, { 
                    method: 'POST', 
                    body: JSON.stringify({ action: "register", n: n, g: g, l: l, p: p, last: last }) 
                });
                alert("রেজিস্ট্রেশন সফল!"); 
                location.reload();
            } catch (e) { 
                alert("ব্যর্থ! আবার চেষ্টা করুন।"); 
                document.getElementById('rBtn').disabled = false;
            }
        }

        function showMain() {
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('mainPage').classList.remove('hidden');
            document.getElementById('welcome').innerText = "স্বাগতম, " + (loggedUser.n || "");
            renderDonors(allDonors);
        }

        function renderDonors(data) {
            const list = document.getElementById('donorList'); list.innerHTML = "";
            data.forEach((d, index) => {
                const cIdx = index % 5;
                list.innerHTML += `
                <div class="bg-white p-5 rounded-[30px] shadow-sm border-t-[6px] card-${cIdx} relative overflow-hidden">
                    <div class="absolute top-0 right-0 bg-red-600 text-white px-4 py-1.5 rounded-bl-2xl font-black text-lg shadow-sm">${d.l}</div>
                    <div class="space-y-1">
                        <div class="info-row"><span class="info-label">নামঃ</span><span class="text-xl font-black text-gray-900">${d.n}</span></div>
                        <div class="info-row"><span class="info-label">ঠিকানাঃ</span><span class="info-value text-gray-500">${d.g}</span></div>
                        <div class="info-row"><span class="info-label">মোবাইলঃ</span><span class="info-value text-blue-600">${d.p}</span></div>
                    </div>
                </div>`;
            });
        }
    </script>
</body>
</html>
