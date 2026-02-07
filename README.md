<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>যুব কল্যাণ রক্তদান ফাউন্ডেশন - প্যানেল</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Hind Siliguri', sans-serif; background-color: #f1f5f9; }
        .hero-gradient { background: linear-gradient(135deg, #dc2626 0%, #991b1b 100%); }
    </style>
</head>
<body class="pb-20">

    <div id="loginPage" class="flex flex-col items-center justify-center min-h-screen px-6">
        <div class="bg-white p-8 rounded-[40px] shadow-2xl w-full max-w-sm text-center border border-gray-100">
            <img src="https://i.ibb.co/C3m2X9Y/1000001730.png" class="w-20 h-20 mx-auto mb-4 rounded-full border-4 border-red-50">
            <h2 class="text-2xl font-bold text-gray-800 mb-2">প্রবেশ করুন</h2>
            
            <div class="flex justify-center gap-4 mb-6 mt-4 p-1 bg-gray-100 rounded-2xl">
                <button onclick="setRole('Member')" id="roleMember" class="flex-1 py-3 rounded-xl text-xs font-bold bg-white text-red-600 shadow-sm transition-all">সদস্য</button>
                <button onclick="setRole('Admin')" id="roleAdmin" class="flex-1 py-3 rounded-xl text-xs font-bold text-gray-500 transition-all">এডমিন</button>
            </div>

            <div id="memberFields">
                <input type="tel" id="u
