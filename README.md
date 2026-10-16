<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Buku Tamu Digital - PT. Berca Niaga Medika</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Inter & Slab Serif fallback -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

    <style>
        body {
            font-family: 'Inter', sans-serif;
            transition: background 0.3s ease, color 0.3s ease;
        }

        /* Theme 1: Executive Navy & Pearl (Default - Soft, Modern, Professional) */
        body.theme-corporate {
            background-color: #f4f7fb;
            background-image: 
                radial-gradient(at 0% 0%, rgba(30, 58, 138, 0.07) 0px, transparent 50%),
                radial-gradient(at 100% 0%, rgba(225, 29, 72, 0.05) 0px, transparent 45%),
                radial-gradient(at 50% 100%, rgba(37, 99, 235, 0.05) 0px, transparent 50%);
        }

        /* Theme 2: Soft Tosca / Mint (Inspired by original form but softer on the eyes) */
        body.theme-mint {
            background-color: #ecfdf5;
            background-image: 
                radial-gradient(at 15% 15%, #ccfbf1 0px, transparent 55%),
                radial-gradient(at 85% 85%, #d1fae5 0px, transparent 55%);
        }

        /* Theme 3: Warm Alabaster Minimalist */
        body.theme-minimal {
            background-color: #f8fafc;
        }

        /* Question Card Hover & Focus Polish */
        .question-card {
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
            border-left: 4px solid transparent;
        }
        .question-card:focus-within {
            border-left-color: #1e3a8a;
            transform: translateY(-1px);
            box-shadow: 0 10px 25px -5px rgba(15, 23, 42, 0.07), 0 8px 10px -6px rgba(15, 23, 42, 0.04);
        }

        /* Custom Checkbox Animation */
        .checkbox-card {
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .checkbox-card:hover {
            transform: translateY(-1px);
        }
        .checkbox-input:checked + .checkbox-box {
            background-color: #1e3a8a;
            border-color: #1e3a8a;
        }
        .checkbox-input:checked + .checkbox-box svg {
            opacity: 1;
            transform: scale(1);
        }
        .checkbox-card:has(.checkbox-input:checked) {
            background-color: #eff6ff;
            border-color: #93c5fd;
        }

        /* Floating Focus Underline */
        .form-input-line {
            position: relative;
        }
        .form-input-line::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 0;
            height: 2px;
            background: linear-gradient(90deg, #1e3a8a, #2563eb);
            transition: width 0.3s ease;
        }
        .form-input-line:focus-within::after {
            width: 100%;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #94a3b8;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #64748b;
        }
    </style>
</head>

<body class="theme-corporate min-h-screen text-slate-800 flex flex-col justify-between">

    <!-- Top Admin & Integration Control Bar (Can be hidden in Kiosk/Guest Mode) -->
    <div id="adminToolbar" class="bg-slate-900/95 backdrop-blur-md text-white text-xs sm:text-sm py-2.5 px-4 shadow-md sticky top-0 z-40 border-b border-slate-800">
        <div class="max-w-4xl mx-auto flex flex-wrap items-center justify-between gap-2">
            <div class="flex items-center gap-2">
                <span id="connectionBadge" class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-full text-xs font-medium bg-amber-500/20 text-amber-300 border border-amber-500/30">
                    <span class="w-2 h-2 rounded-full bg-amber-400 animate-pulse"></span>
                    <span id="connectionText">Mode Lokal (Siap Hubung Spreadsheet)</span>
                </span>
            </div>

            <div class="flex flex-wrap items-center gap-1.5 sm:gap-2">
                <button onclick="switchTab('form')" id="tabBtnForm" class="px-3 py-1.5 rounded-lg font-medium bg-blue-600 text-white hover:bg-blue-500 transition flex items-center gap-1.5">
                    <i class="fa-regular fa-clipboard"></i>
                    <span>Form Tamu</span>
                </button>
                <button onclick="switchTab('data')" id="tabBtnData" class="px-3 py-1.5 rounded-lg font-medium bg-slate-800 text-slate-200 hover:bg-slate-700 transition flex items-center gap-1.5">
                    <i class="fa-solid fa-table"></i>
                    <span>Data Tamu</span>
                    <span id="guestCountBadge" class="bg-rose-600 text-white text-[11px] px-1.5 py-0.2 rounded-full font-bold">0</span>
                </button>
                <button onclick="openEventModal()" class="px-3 py-1.5 rounded-lg font-medium bg-indigo-600 text-white hover:bg-indigo-500 transition flex items-center gap-1.5" title="Ubah Judul Acara, Venue, dan Tanggal">
                    <i class="fa-regular fa-calendar-check"></i>
                    <span>Atur Info Acara</span>
                </button>
                <button onclick="openSetupModal()" class="px-3 py-1.5 rounded-lg font-medium bg-emerald-600 text-white hover:bg-emerald-500 transition flex items-center gap-1.5">
                    <i class="fa-solid fa-file-excel"></i>
                    <span>Integrasi Spreadsheet</span>
                </button>
                <button onclick="toggleTheme()" class="p-1.5 px-2.5 rounded-lg bg-slate-800 text-slate-300 hover:bg-slate-700 transition" title="Ganti Tema Warna Latar">
                    <i class="fa-solid fa-palette"></i>
                </button>
                <button onclick="toggleKioskMode(true)" class="p-1.5 px-2.5 rounded-lg bg-slate-800 text-slate-300 hover:bg-slate-700 transition" title="Mode Layar Penuh / Kiosk Tamu (Sembunyikan Bar Admin)">
                    <i class="fa-solid fa-expand"></i>
                </button>
            </div>
        </div>
    </div>

    <!-- Floating Button to Exit Kiosk Mode (Hidden by default) -->
    <button id="exitKioskBtn" onclick="toggleKioskMode(false)" class="hidden fixed bottom-4 right-4 z-50 bg-slate-900/70 hover:bg-slate-900 text-white text-xs px-3.5 py-2 rounded-full shadow-lg backdrop-blur-sm transition items-center gap-1.5">
        <i class="fa-solid fa-gear"></i>
        <span>Panel Admin</span>
    </button>

    <!-- Main Container -->
    <main class="flex-grow w-full max-w-3xl mx-auto px-3 sm:px-6 py-6 sm:py-9">

        <!-- ======================== VIEW 1: FORM TAMU ======================== -->
        <div id="viewForm" class="space-y-4">

            <!-- Header Card: Repositioned Logo + Event Title, Venue & Date Banner -->
            <div class="bg-white rounded-2xl shadow-lg shadow-slate-200/70 border border-slate-200/90 overflow-hidden">
                <!-- Refined Brand Top Accent Line -->
                <div class="h-2 w-full bg-gradient-to-r from-[#e11d48] via-[#be123c] to-[#000066]"></div>

                <!-- Top Brand Bar: Logo Positioned Cleanly on the Left, Official Badge on the Right -->
                <div class="px-5 sm:px-8 py-4 bg-white border-b border-slate-100 flex flex-wrap items-center justify-between gap-4">
                    <div class="relative group flex items-center gap-3">
                        <div id="bnmLogoContainer" class="bg-white py-1 rounded inline-block">
                            <!--
                                Precision-traced SVG from uploaded logo-bnm.png
                                Positioned proportionally in the top-left header bar
                            -->
                            <svg id="bnmDefaultSvg" viewBox="0 0 300 184" class="w-32 sm:w-36 h-auto select-none" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Logo PT. Berca Niaga Medika">
                                <!-- Top Red Bar -->
                                <rect x="2" y="1" width="295" height="22" fill="#FF0000" stroke="#450000" stroke-width="0.9"/>

                                <!-- LETTER B -->
                                <path fill="#000066" fill-rule="evenodd" d="
                                    M 5.5 39 H 49 C 64.5 39, 71.5 46, 71.5 57.5 C 71.5 67.5, 65 73.5, 56.5 76
                                    C 68 78.5, 76 85.5, 76 98 C 76 110.5, 66.5 117, 50 117 H 5.5 V 103.5 H 15 V 52.5 H 5.5 Z
                                    M 36 52.5 V 70.5 H 44.5 C 50.5 70.5, 53.5 67, 53.5 61.5 C 53.5 55.8, 50.5 52.5, 44.5 52.5 Z
                                    M 36 83 V 103.5 H 46 C 52.5 103.5, 56 99.5, 56 93.2 C 56 86.8, 52.5 83, 46 83 Z
                                "/>

                                <!-- LETTER N -->
                                <path fill="#000066" d="
                                    M 83.5 39 H 115.5 L 144 88.5 V 52.5 H 134 V 39 H 169.5 V 52.5 H 158.5 V 117 H 142.5
                                    L 108 60 V 103.5 H 118.5 V 117 H 83.5 V 103.5 H 93 V 52.5 H 83.5 Z
                                "/>

                                <!-- LETTER M -->
                                <path fill="#000066" d="
                                    M 174 39 H 213.5 L 232.5 91 L 247.5 39 H 287.5 V 52.5 H 277.5 V 103.5 H 287.5 V 117
                                    H 249 V 103.5 H 257 V 53.5 L 236 117 H 223.5 L 197.5 53.5 V 103.5 H 207.5 V 117
                                    H 174 V 103.5 H 183.5 V 52.5 H 174 Z
                                "/>

                                <!-- Bottom Red Bar -->
                                <rect x="2" y="132" width="295" height="19.5" fill="#FF0000" stroke="#450000" stroke-width="0.9"/>

                                <!-- Bottom Text: PT. BERCA NIAGA MEDIKA -->
                                <text x="149.5" y="179.5"
                                      fill="#000066"
                                      font-family="Rockwell, 'Roboto Slab', 'Clarendon', Georgia, serif"
                                      font-weight="900"
                                      font-size="21.5"
                                      text-anchor="middle"
                                      textLength="296"
                                      lengthAdjust="spacingAndGlyphs"
                                      style="letter-spacing: 0.5px;">PT. BERCA NIAGA MEDIKA</text>
                            </svg>

                            <!-- Custom uploaded image fallback -->
                            <img id="bnmCustomImg" src="" alt="Logo PT. Berca Niaga Medika" class="hidden max-w-[150px] max-h-[85px] object-contain">
                        </div>

                        <!-- Hover button in admin mode to upload custom logo file -->
                        <label id="uploadLogoLabel" class="cursor-pointer opacity-0 group-hover:opacity-100 transition-opacity bg-slate-800 text-white text-[11px] px-2.5 py-1 rounded-full shadow flex items-center gap-1 hover:bg-slate-700" title="Ganti dengan file gambar logo dari komputer Anda">
                            <i class="fa-solid fa-camera"></i>
                            <span>Ubah Logo</span>
                            <input type="file" id="logoFileInput" accept="image/*" class="hidden" onchange="handleCustomLogoUpload(event)">
                        </label>
                    </div>

                    <!-- Right side badge & quick edit button -->
                    <div class="flex items-center gap-2">
                        <span class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-semibold bg-blue-50 text-[#1e3a8a] border border-blue-200/80">
                            <i class="fa-solid fa-user-check text-blue-600"></i>
                            <span>Registrasi Kehadiran Tamu</span>
                        </span>
                        <button id="quickEditEventBtn" type="button" onclick="openEventModal()" class="text-xs text-slate-500 hover:text-indigo-600 bg-slate-100 hover:bg-indigo-50 px-2.5 py-1 rounded-lg transition flex items-center gap-1 font-medium" title="Ubah Judul Acara, Venue & Tanggal">
                            <i class="fa-solid fa-pen-to-square"></i>
                            <span class="hidden sm:inline">Edit Acara</span>
                        </button>
                    </div>
                </div>

                <!-- Main Event Info Section (Title, Venue, Date) -->
                <div class="p-6 sm:p-8 bg-gradient-to-br from-[#0f172a] via-[#1e293b] to-[#1e3a8a] text-white relative overflow-hidden">
                    <!-- Subtle decorative background circles -->
                    <div class="absolute -right-12 -top-12 w-48 h-48 rounded-full bg-blue-500/10 blur-2xl pointer-events-none"></div>
                    <div class="absolute right-16 -bottom-16 w-40 h-40 rounded-full bg-rose-500/10 blur-2xl pointer-events-none"></div>

                    <div class="relative z-10 space-y-4">
                        <div>
                            <p id="displayEventSubtitle" class="text-blue-300 text-xs sm:text-sm font-semibold uppercase tracking-wider mb-1.5">
                                Buku Tamu &amp; Registrasi Peserta
                            </p>
                            <h1 id="displayEventTitle" class="text-xl sm:text-3xl font-extrabold tracking-tight text-white leading-snug">
                                Seminar &amp; Workshop Solusi Instrumen Analitik Laboratorium
                            </h1>
                        </div>

                        <!-- Venue & Date Pills -->
                        <div class="pt-2 flex flex-col sm:flex-row sm:items-center gap-2.5 sm:gap-6 text-xs sm:text-sm text-slate-200">
                            <div class="flex items-start sm:items-center gap-2.5 bg-white/10 backdrop-blur-xs px-3.5 py-2 rounded-xl border border-white/10">
                                <i class="fa-solid fa-location-dot text-rose-400 text-base mt-0.5 sm:mt-0"></i>
                                <div>
                                    <span class="block text-[10px] uppercase tracking-wider text-slate-300 font-medium">Venue / Lokasi</span>
                                    <span id="displayEventVenue" class="font-semibold text-white">Grand Ballroom Hotel Mulia Senayan, Jakarta</span>
                                </div>
                            </div>

                            <div class="flex items-start sm:items-center gap-2.5 bg-white/10 backdrop-blur-xs px-3.5 py-2 rounded-xl border border-white/10">
                                <i class="fa-regular fa-calendar-days text-sky-400 text-base mt-0.5 sm:mt-0"></i>
                                <div>
                                    <span class="block text-[10px] uppercase tracking-wider text-slate-300 font-medium">Tanggal Pelaksanaan</span>
                                    <span id="displayEventDate" class="font-semibold text-white">Jumat, 16 Oktober 2026</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Instruction Footer Bar inside Header Card -->
                <div class="px-6 sm:px-8 py-3 bg-slate-50 border-t border-slate-200/80 flex flex-wrap items-center justify-between gap-2 text-xs">
                    <span class="text-slate-600">Mohon lengkapi data diri dan survei singkat di bawah ini dengan benar.</span>
                    <span class="text-rose-600 font-semibold">* Wajib diisi</span>
                </div>
            </div>

            <form id="guestForm" onsubmit="handleFormSubmit(event)" novalidate class="space-y-4">

                <!-- 1. Nama * -->
                <div class="question-card bg-white rounded-xl shadow-sm border border-slate-200/90 p-5 sm:p-6" id="card-nama">
                    <label for="inputNama" class="block text-sm sm:text-base font-semibold text-slate-800 mb-2.5">
                        Nama <span class="text-rose-600">*</span>
                    </label>
                    <div class="form-input-line">
                        <input
                            type="text"
                            id="inputNama"
                            name="nama"
                            required
                            placeholder="Nama lengkap beserta gelar (jika ada)"
                            class="w-full sm:w-3/4 py-2.5 px-3 rounded-t-lg text-sm sm:text-base text-slate-800 bg-slate-50/70 focus:bg-blue-50/30 border-b border-slate-300 focus:outline-none placeholder:text-slate-400 transition-colors"
                        >
                    </div>
                    <p id="err-nama" class="hidden text-xs text-rose-600 mt-2 flex items-center gap-1 font-medium">
                        <i class="fa-solid fa-circle-exclamation"></i> Pertanyaan ini wajib diisi.
                    </p>
                </div>

                <!-- 2. Perusahaan/Instansi * -->
                <div class="question-card bg-white rounded-xl shadow-sm border border-slate-200/90 p-5 sm:p-6" id="card-perusahaan">
                    <label for="inputPerusahaan" class="block text-sm sm:text-base font-semibold text-slate-800 mb-2.5">
                        Perusahaan/Instansi <span class="text-rose-600">*</span>
                    </label>
                    <div class="form-input-line">
                        <input
                            type="text"
                            id="inputPerusahaan"
                            name="perusahaan"
                            required
                            placeholder="Nama perusahaan, lembaga, universitas, atau instansi"
                            class="w-full sm:w-3/4 py-2.5 px-3 rounded-t-lg text-sm sm:text-base text-slate-800 bg-slate-50/70 focus:bg-blue-50/30 border-b border-slate-300 focus:outline-none placeholder:text-slate-400 transition-colors"
                        >
                    </div>
                    <p id="err-perusahaan" class="hidden text-xs text-rose-600 mt-2 flex items-center gap-1 font-medium">
                        <i class="fa-solid fa-circle-exclamation"></i> Pertanyaan ini wajib diisi.
                    </p>
                </div>

                <!-- 3. Kota * -->
                <div class="question-card bg-white rounded-xl shadow-sm border border-slate-200/90 p-5 sm:p-6" id="card-kota">
                    <label for="inputKota" class="block text-sm sm:text-base font-semibold text-slate-800 mb-2.5">
                        Kota <span class="text-rose-600">*</span>
                    </label>
                    <div class="form-input-line">
                        <input
                            type="text"
                            id="inputKota"
                            name="kota"
                            required
                            placeholder="Kota domisili tempat kerja / instansi Anda"
                            class="w-full py-2.5 px-3 rounded-t-lg text-sm sm:text-base text-slate-800 bg-slate-50/70 focus:bg-blue-50/30 border-b border-slate-300 focus:outline-none placeholder:text-slate-400 transition-colors"
                        >
                    </div>
                    <p id="err-kota" class="hidden text-xs text-rose-600 mt-2 flex items-center gap-1 font-medium">
                        <i class="fa-solid fa-circle-exclamation"></i> Pertanyaan ini wajib diisi.
                    </p>
                </div>

                <!-- 4. Posisi/Jabatan * -->
                <div class="question-card bg-white rounded-xl shadow-sm border border-slate-200/90 p-5 sm:p-6" id="card-posisi">
                    <label for="inputPosisi" class="block text-sm sm:text-base font-semibold text-slate-800 mb-2.5">
                        Posisi/Jabatan <span class="text-rose-600">*</span>
                    </label>
                    <div class="form-input-line">
                        <input
                            type="text"
                            id="inputPosisi"
                            name="posisi"
                            required
                            placeholder="Contoh: Lab Manager, QC Analyst, Researcher, Procurement"
                            class="w-full sm:w-3/4 py-2.5 px-3 rounded-t-lg text-sm sm:text-base text-slate-800 bg-slate-50/70 focus:bg-blue-50/30 border-b border-slate-300 focus:outline-none placeholder:text-slate-400 transition-colors"
                        >
                    </div>
                    <p id="err-posisi" class="hidden text-xs text-rose-600 mt-2 flex items-center gap-1 font-medium">
                        <i class="fa-solid fa-circle-exclamation"></i> Pertanyaan ini wajib diisi.
                    </p>
                </div>

                <!-- 5. Email * -->
                <div class="question-card bg-white rounded-xl shadow-sm border border-slate-200/90 p-5 sm:p-6" id="card-email">
                    <label for="inputEmail" class="block text-sm sm:text-base font-semibold text-slate-800 mb-2.5">
                        Email <span class="text-rose-600">*</span>
                    </label>
                    <div class="form-input-line">
                        <input
                            type="email"
                            id="inputEmail"
                            name="email"
                            required
                            placeholder="nama@perusahaan.com"
                            class="w-full sm:w-3/4 py-2.5 px-3 rounded-t-lg text-sm sm:text-base text-slate-800 bg-slate-50/70 focus:bg-blue-50/30 border-b border-slate-300 focus:outline-none placeholder:text-slate-400 transition-colors"
                        >
                    </div>
                    <p id="err-email" class="hidden text-xs text-rose-600 mt-2 flex items-center gap-1 font-medium">
                        <i class="fa-solid fa-circle-exclamation"></i> Masukkan alamat email yang valid.
                    </p>
                </div>

                <!-- 6. Nomor Handphone * -->
                <div class="question-card bg-white rounded-xl shadow-sm border border-slate-200/90 p-5 sm:p-6" id="card-hp">
                    <label for="inputHp" class="block text-sm sm:text-base font-semibold text-slate-800 mb-2.5">
                        Nomor Handphone <span class="text-rose-600">*</span>
                    </label>
                    <div class="form-input-line">
                        <input
                            type="tel"
                            id="inputHp"
                            name="hp"
                            required
                            placeholder="Contoh: 081234567890"
                            class="w-full sm:w-3/4 py-2.5 px-3 rounded-t-lg text-sm sm:text-base text-slate-800 bg-slate-50/70 focus:bg-blue-50/30 border-b border-slate-300 focus:outline-none placeholder:text-slate-400 transition-colors"
                        >
                    </div>
                    <p id="err-hp" class="hidden text-xs text-rose-600 mt-2 flex items-center gap-1 font-medium">
                        <i class="fa-solid fa-circle-exclamation"></i> Masukkan nomor handphone yang valid.
                    </p>
                </div>

                <!-- 7. Rencana Investasi 1 Tahun Ke Depan * -->
                <div class="question-card bg-white rounded-xl shadow-sm border border-slate-200/90 p-5 sm:p-6" id="card-investasi">
                    <div class="flex items-start justify-between gap-2 mb-4">
                        <label class="block text-sm sm:text-base font-semibold text-slate-800 leading-relaxed">
                            Apakah ada rencana investasi berikut di tempat Anda bekerja dalam periode 1 tahun ke depan?
                        </label>
                        <span class="text-rose-600 font-bold text-base">*</span>
                    </div>

                    <div id="checkboxContainer" class="space-y-2.5">
                        <!-- Dynamically populated -->
                    </div>

                    <p id="err-investasi" class="hidden text-xs text-rose-600 mt-3 flex items-center gap-1 font-medium">
                        <i class="fa-solid fa-circle-exclamation"></i> Pilih minimal satu opsi rencana investasi.
                    </p>
                </div>

                <!-- Form Action Buttons -->
                <div class="flex flex-wrap items-center justify-between gap-4 pt-2">
                    <button
                        type="submit"
                        id="submitBtn"
                        class="px-8 py-3.5 rounded-xl bg-gradient-to-r from-[#1e3a8a] to-[#1d4ed8] hover:from-[#172554] hover:to-[#1e40af] active:scale-[0.99] text-white font-semibold text-sm sm:text-base shadow-lg shadow-blue-900/20 transition flex items-center gap-2.5"
                    >
                        <span>Kirim Kehadiran</span>
                        <i class="fa-solid fa-paper-plane text-xs"></i>
                    </button>

                    <button
                        type="button"
                        onclick="resetFormFields()"
                        class="px-4 py-2.5 rounded-lg text-slate-500 hover:text-rose-600 hover:bg-white/80 font-medium text-sm transition"
                    >
                        Kosongkan Formulir
                    </button>
                </div>
            </form>

            <!-- Success Screen (Hidden until form is submitted) -->
            <div id="successScreen" class="hidden bg-white rounded-2xl shadow-md border border-slate-200 p-8 text-center space-y-5">
                <div class="w-16 h-16 rounded-full bg-emerald-100 text-emerald-600 flex items-center justify-center mx-auto text-3xl shadow-inner">
                    <i class="fa-solid fa-check"></i>
                </div>
                <div class="space-y-2">
                    <h2 class="text-2xl font-bold text-[#000066]">Terima Kasih!</h2>
                    <p class="text-slate-600 text-sm sm:text-base max-w-md mx-auto">
                        Data kehadiran Anda telah berhasil dicatat di buku tamu <strong class="text-slate-800">PT. Berca Niaga Medika</strong>.
                    </p>
                    <p id="syncStatusNote" class="text-xs text-emerald-600 font-medium"></p>
                </div>

                <!-- Submission Summary Box -->
                <div id="submissionSummary" class="bg-slate-50 rounded-xl p-4 text-left text-xs sm:text-sm space-y-1.5 max-w-lg mx-auto border border-slate-200/80">
                </div>

                <div class="pt-2">
                    <button
                        onclick="showNewForm()"
                        class="px-6 py-3 rounded-xl bg-[#000066] hover:bg-[#000088] text-white font-semibold text-sm shadow transition inline-flex items-center gap-2"
                    >
                        <i class="fa-solid fa-plus"></i>
                        <span>Isi Formulir Tamu Lainnya</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- ======================== VIEW 2: DATA TAMU (SPREADSHEET PREVIEW) ======================== -->
        <div id="viewData" class="hidden space-y-4">
            <div class="bg-white rounded-2xl shadow-md border border-slate-200 p-5 sm:p-6">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 pb-5 border-b border-slate-200">
                    <div>
                        <h2 class="text-lg sm:text-xl font-bold text-[#000066] flex items-center gap-2">
                            <i class="fa-solid fa-table-list text-emerald-600"></i>
                            <span>Rekapitulasi Data Buku Tamu (Spreadsheet)</span>
                        </h2>
                        <p class="text-xs sm:text-sm text-slate-500 mt-0.5">
                            Data tersimpan otomatis dan dapat diekspor ke Excel/CSV atau disinkronkan real-time ke Google Sheets.
                        </p>
                    </div>
                    <div class="flex flex-wrap items-center gap-2">
                        <button onclick="copyTableForSheets()" class="px-3.5 py-2 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs sm:text-sm font-medium transition flex items-center gap-1.5" title="Salin format tabel untuk ditempel (Ctrl+V) langsung di Google Spreadsheet">
                            <i class="fa-regular fa-copy"></i>
                            <span>Salin ke Clipboard</span>
                        </button>
                        <button onclick="exportToCSV()" class="px-3.5 py-2 rounded-lg bg-emerald-600 hover:bg-emerald-500 text-white text-xs sm:text-sm font-medium shadow-sm transition flex items-center gap-1.5">
                            <i class="fa-solid fa-download"></i>
                            <span>Unduh CSV / Excel</span>
                        </button>
                    </div>
                </div>

                <!-- Search & Filter -->
                <div class="my-4 flex flex-col sm:flex-row items-stretch sm:items-center justify-between gap-3">
                    <div class="relative flex-grow max-w-md">
                        <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-1/2 -translate-y-1/2 text-slate-400 text-sm"></i>
                        <input
                            type="text"
                            id="searchInput"
                            oninput="renderGuestTable()"
                            placeholder="Cari nama, instansi, kota, atau instrumen..."
                            class="w-full pl-9 pr-4 py-2 text-sm rounded-lg border border-slate-300 focus:outline-none focus:border-[#000066]"
                        >
                    </div>
                    <button onclick="loadDemoData()" class="text-xs text-blue-700 hover:underline font-medium self-end sm:self-center">
                        + Muat Contoh Data Tamu
                    </button>
                </div>

                <!-- Responsive Spreadsheet Table -->
                <div class="overflow-x-auto rounded-xl border border-slate-200">
                    <table class="w-full text-left border-collapse text-xs sm:text-sm">
                        <thead>
                            <tr class="bg-slate-100 text-slate-700 border-b border-slate-200 font-semibold">
                                <th class="py-3 px-3 w-10 text-center">#</th>
                                <th class="py-3 px-3 whitespace-nowrap">Waktu Input</th>
                                <th class="py-3 px-3 whitespace-nowrap">Nama</th>
                                <th class="py-3 px-3 whitespace-nowrap">Perusahaan/Instansi</th>
                                <th class="py-3 px-3 whitespace-nowrap">Kota</th>
                                <th class="py-3 px-3 whitespace-nowrap">Posisi/Jabatan</th>
                                <th class="py-3 px-3 whitespace-nowrap">Email</th>
                                <th class="py-3 px-3 whitespace-nowrap">No. Handphone</th>
                                <th class="py-3 px-3 min-w-[220px]">Rencana Investasi (1 Thn)</th>
                                <th class="py-3 px-3 text-center">Aksi</th>
                            </tr>
                        </thead>
                        <tbody id="guestTableBody" class="divide-y divide-slate-200 bg-white">
                            <!-- Populated via JavaScript -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </main>

    <!-- ======================== MODAL: ATUR INFO ACARA (JUDUL, VENUE, TANGGAL) ======================== -->
    <div id="eventModal" class="hidden fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-xs flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl max-w-lg w-full shadow-2xl border border-slate-200 overflow-hidden">
            <div class="px-6 py-4 bg-gradient-to-r from-[#0f172a] to-[#1e3a8a] text-white flex items-center justify-between">
                <div class="flex items-center gap-2.5">
                    <i class="fa-regular fa-calendar-check text-lg text-sky-400"></i>
                    <div>
                        <h3 class="font-bold text-base">Pengaturan Informasi Acara</h3>
                        <p class="text-xs text-slate-300">Sesuaikan Judul Acara, Venue, dan Tanggal di bagian atas form</p>
                    </div>
                </div>
                <button onclick="closeEventModal()" class="text-white/80 hover:text-white p-1.5 rounded-lg">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>

            <div class="p-6 space-y-4 text-xs sm:text-sm text-slate-700">
                <div>
                    <label for="editEventSubtitle" class="block font-semibold text-slate-700 mb-1">Label Kategori / Subjudul Atas</label>
                    <input type="text" id="editEventSubtitle" placeholder="Contoh: Buku Tamu & Registrasi Peserta" class="w-full px-3.5 py-2 rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-blue-600">
                </div>
                <div>
                    <label for="editEventTitle" class="block font-semibold text-slate-700 mb-1">Judul Acara <span class="text-rose-600">*</span></label>
                    <input type="text" id="editEventTitle" placeholder="Masukkan nama / judul acara..." class="w-full px-3.5 py-2 rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-blue-600 font-medium">
                </div>
                <div>
                    <label for="editEventVenue" class="block font-semibold text-slate-700 mb-1">Venue / Lokasi Acara <span class="text-rose-600">*</span></label>
                    <input type="text" id="editEventVenue" placeholder="Contoh: Hotel Mulia Senayan, Jakarta" class="w-full px-3.5 py-2 rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-blue-600">
                </div>
                <div>
                    <label for="editEventDate" class="block font-semibold text-slate-700 mb-1">Tanggal & Waktu Pelaksanaan <span class="text-rose-600">*</span></label>
                    <input type="text" id="editEventDate" placeholder="Contoh: Jumat, 16 Oktober 2026 | 08.30 - 15.00 WIB" class="w-full px-3.5 py-2 rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-blue-600">
                </div>
            </div>

            <div class="px-6 py-3.5 bg-slate-50 border-t border-slate-200 flex items-center justify-end gap-2">
                <button onclick="closeEventModal()" class="px-4 py-2 rounded-lg text-slate-600 hover:bg-slate-200 text-xs sm:text-sm font-medium transition">
                    Batal
                </button>
                <button onclick="saveEventInfo()" class="px-5 py-2 rounded-lg bg-[#1e3a8a] hover:bg-blue-800 text-white text-xs sm:text-sm font-semibold shadow-sm transition">
                    Simpan Perubahan
                </button>
            </div>
        </div>
    </div>

    <!-- ======================== MODAL: INTEGRASI GOOGLE SPREADSHEET ======================== -->
    <div id="setupModal" class="hidden fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-xs flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl max-w-2xl w-full max-h-[90vh] flex flex-col shadow-2xl border border-slate-200 overflow-hidden">
            <!-- Modal Header -->
            <div class="px-6 py-4 bg-[#000066] text-white flex items-center justify-between">
                <div class="flex items-center gap-2.5">
                    <i class="fa-solid fa-file-csv text-xl text-emerald-400"></i>
                    <div>
                        <h3 class="font-bold text-base sm:text-lg">Integrasi Otomatis Google Spreadsheet</h3>
                        <p class="text-xs text-blue-200">Kirim setiap input tamu langsung ke Google Sheets Anda secara real-time</p>
                    </div>
                </div>
                <button onclick="closeSetupModal()" class="text-white/80 hover:text-white p-1.5 rounded-lg">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>

            <!-- Modal Body -->
            <div class="p-6 overflow-y-auto space-y-5 text-xs sm:text-sm text-slate-700">
                <!-- Webhook URL Input -->
                <div class="bg-emerald-50 border border-emerald-200 rounded-xl p-4 space-y-2.5">
                    <label for="scriptUrlInput" class="block font-bold text-emerald-950">
                        URL Web App Google Apps Script:
                    </label>
                    <div class="flex flex-col sm:flex-row gap-2">
                        <input
                            type="url"
                            id="scriptUrlInput"
                            placeholder="https://script.google.com/macros/s/AKfycb.../exec"
                            class="flex-grow px-3.5 py-2 rounded-lg border border-emerald-300 bg-white text-slate-800 focus:outline-none focus:ring-2 focus:ring-emerald-600"
                        >
                        <button onclick="saveScriptUrl()" class="px-4 py-2 rounded-lg bg-emerald-600 hover:bg-emerald-500 text-white font-semibold whitespace-nowrap transition">
                            Simpan & Hubungkan
                        </button>
                    </div>
                    <div class="flex items-center justify-between pt-1">
                        <span class="text-[11px] text-emerald-800">
                            Setiap tamu menekan tombol <strong>Kirim Formulir</strong>, data otomatis masuk ke baris baru Google Sheet.
                        </span>
                        <button onclick="sendTestPing()" class="text-xs font-semibold text-emerald-700 hover:underline whitespace-nowrap ml-2">
                            <i class="fa-solid fa-bolt"></i> Tes Kirim Data
                        </button>
                    </div>
                </div>

                <!-- Step by step guide -->
                <div class="space-y-3">
                    <h4 class="font-bold text-slate-900 text-sm">Cara Menghubungkan ke Google Spreadsheet Anda (Hanya 2 Menit):</h4>
                    <ol class="list-decimal list-inside space-y-2 text-slate-600 leading-relaxed">
                        <li>Buka <strong>Google Spreadsheet</strong> baru di Google Drive Anda.</li>
                        <li>Klik menu <strong>Ekstensi (Extensions)</strong> &rarr; <strong>Apps Script</strong>.</li>
                        <li>Hapus kode yang ada di dalamnya, lalu salin dan tempel kode di bawah ini:</li>
                    </ol>

                    <!-- Ready-to-copy Google Apps Script code -->
                    <div class="relative bg-slate-900 text-slate-100 rounded-xl p-4 font-mono text-xs overflow-x-auto">
                        <button onclick="copyAppsScriptCode()" class="absolute top-2.5 right-2.5 bg-blue-600 hover:bg-blue-500 text-white font-sans text-xs px-3 py-1 rounded-md shadow flex items-center gap-1.5 transition">
                            <i class="fa-regular fa-copy"></i>
                            <span>Salin Kode Apps Script</span>
                        </button>
<pre id="appsScriptCode" class="leading-relaxed pr-28">function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  
  // Buat judul kolom otomatis jika baris pertama masih kosong
  if (sheet.getLastRow() === 0) {
    sheet.appendRow([
      "Timestamp",
      "Nama",
      "Perusahaan/Instansi",
      "Kota",
      "Posisi/Jabatan",
      "Email",
      "Nomor Handphone",
      "Rencana Investasi (1 Tahun Ke Depan)"
    ]);
  }

  var data = JSON.parse(e.postData.contents);
  
  sheet.appendRow([
    data.timestamp || new Date().toLocaleString("id-ID"),
    data.nama || "",
    data.perusahaan || "",
    data.kota || "",
    data.posisi || "",
    data.email || "",
    "'" + (data.hp || ""),
    data.investasi || ""
  ]);

  return ContentService.createTextOutput(
    JSON.stringify({ status: "success" })
  ).setMimeType(ContentService.MimeType.JSON);
}</pre>
                    </div>

                    <ol start="4" class="list-decimal list-inside space-y-2 text-slate-600 leading-relaxed">
                        <li>Klik tombol <strong>Terapkan (Deploy)</strong> di kanan atas &rarr; pilih <strong>Deployment baru (New deployment)</strong>.</li>
                        <li>Pilih jenis <strong>Aplikasi Web (Web app)</strong>. Pada bagian <em>"Siapa yang memiliki akses (Who has access)"</em>, pilih <strong>Siapa saja (Anyone)</strong>, lalu klik <strong>Deploy</strong>.</li>
                        <li>Salin <strong>URL Aplikasi Web</strong> yang muncul, lalu tempelkan ke kolom di atas dan klik <strong>Simpan & Hubungkan</strong>.</li>
                    </ol>
                </div>
            </div>

            <!-- Modal Footer -->
            <div class="px-6 py-3.5 bg-slate-50 border-t border-slate-200 flex justify-end">
                <button onclick="closeSetupModal()" class="px-5 py-2 rounded-lg bg-slate-800 hover:bg-slate-700 text-white text-xs sm:text-sm font-medium transition">
                    Tutup Panduan
                </button>
            </div>
        </div>
    </div>

    <!-- Toast Notification -->
    <div id="toastBox" class="fixed bottom-5 left-1/2 -translate-x-1/2 z-50 pointer-events-none opacity-0 translate-y-2 transition-all duration-300">
        <div class="bg-slate-900 text-white text-xs sm:text-sm px-4 py-2.5 rounded-xl shadow-xl flex items-center gap-2.5 border border-slate-700">
            <i id="toastIcon" class="fa-solid fa-circle-check text-emerald-400"></i>
            <span id="toastMessage">Berhasil disimpan</span>
        </div>
    </div>

    <!-- Footer -->
    <footer class="py-5 text-center text-xs text-slate-500">
        <p class="font-semibold text-slate-600">PT. Berca Niaga Medika</p>
        <p class="mt-0.5">Sistem Registrasi Buku Tamu Digital Terintegrasi Spreadsheet</p>
    </footer>

    <script>
        /**
         * KONFIGURASI URL GOOGLE APPS SCRIPT:
         * Anda dapat langsung mengisi URL Web App Google Script Anda di variabel di bawah ini,
         * atau mengisinya melalui tombol "Integrasi Spreadsheet" di bagian atas halaman.
         */
        let GOOGLE_SCRIPT_URL = "";

        // Exact 10 options from the user's 3rd screenshot
        const INVESTMENT_OPTIONS = [
            "Gas Chromatography (GC)",
            "Gas Chromatography - Mass Spectrometry (GCMS/ GCMSMS)",
            "Liquid Chromatography (HPLC/ UHPLC)",
            "Liquid Chromatography - Mass Spectrometry (LCMS/ LCMSMS)",
            "CPC (Centrifugal Partition Chromatography)",
            "Consumables & Supplies",
            "Sample preparation",
            "Gas generator (H2/ N2/ Zero Air)",
            "NIR BUCHI",
            "Saya tidak tahu"
        ];

        // In-memory guest list store
        let guestSubmissions = [];

        // Event Header Info State
        let eventInfo = {
            subtitle: "Buku Tamu & Registrasi Peserta",
            title: "Seminar & Workshop Solusi Instrumen Analitik Laboratorium",
            venue: "Grand Ballroom Hotel Mulia Senayan, Jakarta",
            date: ""
        };

        // Initialize Page
        window.onload = function () {
            initDefaultEventDate();
            renderEventHeader();
            renderCheckboxes();
            renderGuestTable();
            updateConnectionBadge();
        };

        function initDefaultEventDate() {
            const now = new Date();
            eventInfo.date = now.toLocaleDateString('id-ID', {
                weekday: 'long', day: 'numeric', month: 'long', year: 'numeric'
            });
        }

        function renderEventHeader() {
            document.getElementById('displayEventSubtitle').textContent = eventInfo.subtitle;
            document.getElementById('displayEventTitle').textContent = eventInfo.title;
            document.getElementById('displayEventVenue').textContent = eventInfo.venue;
            document.getElementById('displayEventDate').textContent = eventInfo.date;
        }

        function openEventModal() {
            document.getElementById('editEventSubtitle').value = eventInfo.subtitle;
            document.getElementById('editEventTitle').value = eventInfo.title;
            document.getElementById('editEventVenue').value = eventInfo.venue;
            document.getElementById('editEventDate').value = eventInfo.date;
            document.getElementById('eventModal').classList.remove('hidden');
        }

        function closeEventModal() {
            document.getElementById('eventModal').classList.add('hidden');
        }

        function saveEventInfo() {
            const sub = document.getElementById('editEventSubtitle').value.trim() || "Buku Tamu & Registrasi Peserta";
            const title = document.getElementById('editEventTitle').value.trim();
            const venue = document.getElementById('editEventVenue').value.trim();
            const date = document.getElementById('editEventDate').value.trim();

            if (!title || !venue || !date) {
                showToast("Judul Acara, Venue, dan Tanggal tidak boleh kosong", "error");
                return;
            }

            eventInfo = { subtitle: sub, title, venue, date };
            renderEventHeader();
            closeEventModal();
            showToast("Informasi acara berhasil diperbarui!", "success");
        }

        function renderCheckboxes() {
            const container = document.getElementById('checkboxContainer');
            container.innerHTML = INVESTMENT_OPTIONS.map((option) => {
                const isDontKnow = option === "Saya tidak tahu";
                return `
                    <label class="checkbox-card flex items-center gap-3.5 p-3.5 rounded-xl border border-slate-200/90 hover:border-blue-400 hover:bg-slate-50 cursor-pointer select-none">
                        <input
                            type="checkbox"
                            name="investasi"
                            value="${option}"
                            data-dontknow="${isDontKnow}"
                            onchange="handleCheckboxLogic(this)"
                            class="checkbox-input sr-only"
                        >
                        <div class="checkbox-box w-5 h-5 rounded-md border-2 border-slate-400 flex items-center justify-center transition-colors bg-white shrink-0">
                            <svg class="w-3.5 h-3.5 text-white opacity-0 scale-50 transition-all duration-150" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="3">
                                <path stroke-linecap="round" stroke-linejoin="round" d="M5 13l4 4L19 7" />
                            </svg>
                        </div>
                        <span class="text-sm sm:text-base text-slate-700 font-medium">${option}</span>
                    </label>
                `;
            }).join('');
        }

        // Smart interaction: if user checks "Saya tidak tahu", uncheck specific instruments (and vice versa)
        function handleCheckboxLogic(changedInput) {
            const allInputs = Array.from(document.querySelectorAll('input[name="investasi"]'));
            const isDontKnow = changedInput.getAttribute('data-dontknow') === 'true';

            if (changedInput.checked) {
                if (isDontKnow) {
                    allInputs.forEach(inp => {
                        if (inp !== changedInput) inp.checked = false;
                    });
                } else {
                    allInputs.forEach(inp => {
                        if (inp.getAttribute('data-dontknow') === 'true') inp.checked = false;
                    });
                }
                document.getElementById('err-investasi').classList.add('hidden');
                document.getElementById('card-investasi').classList.remove('border-red-500', 'ring-2', 'ring-red-200');
            }
        }

        function validateForm() {
            let isValid = true;

            const fields = [
                { id: 'inputNama', err: 'err-nama', card: 'card-nama', type: 'text' },
                { id: 'inputPerusahaan', err: 'err-perusahaan', card: 'card-perusahaan', type: 'text' },
                { id: 'inputKota', err: 'err-kota', card: 'card-kota', type: 'text' },
                { id: 'inputPosisi', err: 'err-posisi', card: 'card-posisi', type: 'text' },
                { id: 'inputEmail', err: 'err-email', card: 'card-email', type: 'email' },
                { id: 'inputHp', err: 'err-hp', card: 'card-hp', type: 'tel' }
            ];

            fields.forEach(f => {
                const el = document.getElementById(f.id);
                const errEl = document.getElementById(f.err);
                const cardEl = document.getElementById(f.card);
                const val = el.value.trim();

                let fieldOk = val.length > 0;
                if (fieldOk && f.type === 'email') {
                    fieldOk = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(val);
                }
                if (fieldOk && f.type === 'tel') {
                    fieldOk = /^[0-9+\-\s()]{7,20}$/.test(val);
                }

                if (!fieldOk) {
                    isValid = false;
                    errEl.classList.remove('hidden');
                    cardEl.classList.add('border-red-500', 'ring-2', 'ring-red-200');
                } else {
                    errEl.classList.add('hidden');
                    cardEl.classList.remove('border-red-500', 'ring-2', 'ring-red-200');
                }

                // Clear error on typing
                el.oninput = () => {
                    errEl.classList.add('hidden');
                    cardEl.classList.remove('border-red-500', 'ring-2', 'ring-red-200');
                };
            });

            // Checkboxes validation
            const selectedInvestasi = Array.from(document.querySelectorAll('input[name="investasi"]:checked')).map(i => i.value);
            const errInv = document.getElementById('err-investasi');
            const cardInv = document.getElementById('card-investasi');

            if (selectedInvestasi.length === 0) {
                isValid = false;
                errInv.classList.remove('hidden');
                cardInv.classList.add('border-red-500', 'ring-2', 'ring-red-200');
            } else {
                errInv.classList.add('hidden');
                cardInv.classList.remove('border-red-500', 'ring-2', 'ring-red-200');
            }

            return isValid;
        }

        async function handleFormSubmit(event) {
            event.preventDefault();

            if (!validateForm()) {
                showToast("Mohon lengkapi semua kolom wajib (*)", "error");
                const firstErr = document.querySelector('.border-red-500');
                if (firstErr) firstErr.scrollIntoView({ behavior: 'smooth', block: 'center' });
                return;
            }

            const submitBtn = document.getElementById('submitBtn');
            const originalBtnHtml = submitBtn.innerHTML;
            submitBtn.disabled = true;
            submitBtn.innerHTML = `<i class="fa-solid fa-circle-notch fa-spin"></i> <span>Menyimpan ke Spreadsheet...</span>`;

            const selectedInvestasi = Array.from(document.querySelectorAll('input[name="investasi"]:checked')).map(i => i.value);
            const now = new Date();
            const timestampStr = now.toLocaleDateString('id-ID', {
                day: '2-digit', month: '2-digit', year: 'numeric'
            }) + ' ' + now.toLocaleTimeString('id-ID', {
                hour: '2-digit', minute: '2-digit', second: '2-digit'
            });

            const entry = {
                id: Date.now(),
                timestamp: timestampStr,
                nama: document.getElementById('inputNama').value.trim(),
                perusahaan: document.getElementById('inputPerusahaan').value.trim(),
                kota: document.getElementById('inputKota').value.trim(),
                posisi: document.getElementById('inputPosisi').value.trim(),
                email: document.getElementById('inputEmail').value.trim(),
                hp: document.getElementById('inputHp').value.trim(),
                investasi: selectedInvestasi.join(', ')
            };

            // Add to local spreadsheet list
            guestSubmissions.unshift(entry);
            renderGuestTable();

            // Send to Google Spreadsheet if Webhook URL is configured
            let syncedToGoogle = false;
            if (GOOGLE_SCRIPT_URL) {
                try {
                    await fetch(GOOGLE_SCRIPT_URL, {
                        method: 'POST',
                        mode: 'no-cors',
                        headers: { 'Content-Type': 'text/plain;charset=utf-8' },
                        body: JSON.stringify(entry)
                    });
                    syncedToGoogle = true;
                } catch (err) {
                    console.error("Gagal sinkronisasi ke Google Script:", err);
                }
            }

            submitBtn.disabled = false;
            submitBtn.innerHTML = originalBtnHtml;

            // Show Confirmation View
            document.getElementById('guestForm').classList.add('hidden');
            document.getElementById('successScreen').classList.remove('hidden');

            const syncNote = document.getElementById('syncStatusNote');
            if (syncedToGoogle) {
                syncNote.innerHTML = `<i class="fa-solid fa-cloud-check"></i> Data telah terkirim otomatis ke Google Spreadsheet.`;
                syncNote.className = "text-xs text-emerald-600 font-semibold";
            } else {
                syncNote.innerHTML = `<i class="fa-solid fa-database"></i> Tersimpan di Tabel Data Tamu (Siap diekspor ke Excel/Spreadsheet).`;
                syncNote.className = "text-xs text-blue-600 font-medium";
            }

            document.getElementById('submissionSummary').innerHTML = `
                <div class="grid grid-cols-3 gap-1 py-1 border-b border-slate-200/60">
                    <span class="text-slate-500">Nama:</span>
                    <span class="col-span-2 font-semibold text-slate-800">${escapeHtml(entry.nama)}</span>
                </div>
                <div class="grid grid-cols-3 gap-1 py-1 border-b border-slate-200/60">
                    <span class="text-slate-500">Instansi:</span>
                    <span class="col-span-2 font-semibold text-slate-800">${escapeHtml(entry.perusahaan)} (${escapeHtml(entry.kota)})</span>
                </div>
                <div class="grid grid-cols-3 gap-1 py-1 border-b border-slate-200/60">
                    <span class="text-slate-500">Jabatan:</span>
                    <span class="col-span-2 font-medium text-slate-700">${escapeHtml(entry.posisi)}</span>
                </div>
                <div class="grid grid-cols-3 gap-1 py-1">
                    <span class="text-slate-500">Rencana Investasi:</span>
                    <span class="col-span-2 font-medium text-[#000066]">${escapeHtml(entry.investasi)}</span>
                </div>
            `;

            window.scrollTo({ top: 0, behavior: 'smooth' });
            showToast("Registrasi tamu berhasil disimpan!", "success");
        }

        function resetFormFields() {
            document.getElementById('guestForm').reset();
            const errIds = ['err-nama', 'err-perusahaan', 'err-kota', 'err-posisi', 'err-email', 'err-hp', 'err-investasi'];
            errIds.forEach(id => document.getElementById(id).classList.add('hidden'));

            const cardIds = ['card-nama', 'card-perusahaan', 'card-kota', 'card-posisi', 'card-email', 'card-hp', 'card-investasi'];
            cardIds.forEach(id => document.getElementById(id).classList.remove('border-red-500', 'ring-2', 'ring-red-200'));
        }

        function showNewForm() {
            resetFormFields();
            document.getElementById('successScreen').classList.add('hidden');
            document.getElementById('guestForm').classList.remove('hidden');
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function renderGuestTable() {
            const tbody = document.getElementById('guestTableBody');
            const badge = document.getElementById('guestCountBadge');
            const query = (document.getElementById('searchInput')?.value || '').toLowerCase();

            badge.textContent = guestSubmissions.length;

            const filtered = guestSubmissions.filter(item =>
                item.nama.toLowerCase().includes(query) ||
                item.perusahaan.toLowerCase().includes(query) ||
                item.kota.toLowerCase().includes(query) ||
                item.posisi.toLowerCase().includes(query) ||
                item.investasi.toLowerCase().includes(query)
            );

            if (filtered.length === 0) {
                tbody.innerHTML = `
                    <tr>
                        <td colspan="10" class="py-10 text-center text-slate-400">
                            <i class="fa-regular fa-folder-open text-2xl mb-2 block"></i>
                            Belum ada data tamu yang tercatat. Silakan isi melalui tab <strong>Form Tamu</strong>.
                        </td>
                    </tr>
                `;
                return;
            }

            tbody.innerHTML = filtered.map((item, index) => `
                <tr class="hover:bg-slate-50 transition">
                    <td class="py-3 px-3 text-center font-medium text-slate-500">${index + 1}</td>
                    <td class="py-3 px-3 whitespace-nowrap text-slate-500 text-xs">${escapeHtml(item.timestamp)}</td>
                    <td class="py-3 px-3 font-semibold text-slate-800">${escapeHtml(item.nama)}</td>
                    <td class="py-3 px-3 text-slate-700">${escapeHtml(item.perusahaan)}</td>
                    <td class="py-3 px-3 text-slate-700">${escapeHtml(item.kota)}</td>
                    <td class="py-3 px-3 text-slate-700">${escapeHtml(item.posisi)}</td>
                    <td class="py-3 px-3 text-blue-700">${escapeHtml(item.email)}</td>
                    <td class="py-3 px-3 whitespace-nowrap text-slate-700">${escapeHtml(item.hp)}</td>
                    <td class="py-3 px-3">
                        <div class="flex flex-wrap gap-1">
                            ${item.investasi.split(', ').map(tag => `
                                <span class="inline-block bg-blue-50 text-[#000066] border border-blue-200 text-[11px] px-2 py-0.5 rounded-md font-medium">
                                    ${escapeHtml(tag)}
                                </span>
                            `).join('')}
                        </div>
                    </td>
                    <td class="py-3 px-3 text-center">
                        <button onclick="deleteGuestEntry(${item.id})" class="text-slate-400 hover:text-red-600 p-1 transition" title="Hapus Baris">
                            <i class="fa-regular fa-trash-can"></i>
                        </button>
                    </td>
                </tr>
            `).join('');
        }

        function deleteGuestEntry(id) {
            guestSubmissions = guestSubmissions.filter(item => item.id !== id);
            renderGuestTable();
            showToast("Baris data telah dihapus", "info");
        }

        function exportToCSV() {
            if (guestSubmissions.length === 0) {
                showToast("Belum ada data untuk diekspor", "error");
                return;
            }

            const headers = [
                "Timestamp",
                "Nama",
                "Perusahaan/Instansi",
                "Kota",
                "Posisi/Jabatan",
                "Email",
                "Nomor Handphone",
                "Apakah ada rencana investasi berikut di tempat Anda bekerja dalam periode 1 tahun ke depan?"
            ];

            const csvRows = [
                headers.map(h => `"${h.replace(/"/g, '""')}"`).join(',')
            ];

            guestSubmissions.forEach(item => {
                const row = [
                    item.timestamp,
                    item.nama,
                    item.perusahaan,
                    item.kota,
                    item.posisi,
                    item.email,
                    `'` + item.hp,
                    item.investasi
                ];
                csvRows.push(row.map(v => `"${String(v).replace(/"/g, '""')}"`).join(','));
            });

            // Add BOM for UTF-8 Excel compatibility
            const csvString = "\uFEFF" + csvRows.join('\r\n');
            const blob = new Blob([csvString], { type: 'text/csv;charset=utf-8;' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `Buku_Tamu_BNM_${new Date().toISOString().slice(0,10)}.csv`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);

            showToast("File CSV/Spreadsheet berhasil diunduh!", "success");
        }

        function copyTableForSheets() {
            if (guestSubmissions.length === 0) {
                showToast("Belum ada data untuk disalin", "error");
                return;
            }

            const headers = [
                "Timestamp", "Nama", "Perusahaan/Instansi", "Kota",
                "Posisi/Jabatan", "Email", "Nomor Handphone", "Rencana Investasi (1 Tahun)"
            ];

            const rows = [headers.join('\t')];
            guestSubmissions.forEach(item => {
                rows.push([
                    item.timestamp, item.nama, item.perusahaan, item.kota,
                    item.posisi, item.email, item.hp, item.investasi
                ].join('\t'));
            });

            copyTextToClipboard(rows.join('\n'));
            showToast("Data disalin! Tekan Ctrl+V langsung di Google Spreadsheet.", "success");
        }

        function loadDemoData() {
            const sample = [
                {
                    id: Date.now() + 1,
                    timestamp: "16/10/2026 08:30:12",
                    nama: "Dr. Hendra Wijaya, M.Si.",
                    perusahaan: "PT. Bio Farma Laboratorium",
                    kota: "Bandung",
                    posisi: "Head of Analytical Development",
                    email: "hendra.wijaya@biofarma-example.co.id",
                    hp: "081288990011",
                    investasi: "Liquid Chromatography (HPLC/ UHPLC), Liquid Chromatography - Mass Spectrometry (LCMS/ LCMSMS), Consumables & Supplies"
                },
                {
                    id: Date.now() + 2,
                    timestamp: "16/10/2026 08:41:05",
                    nama: "Siti Rahmawati, S.Si.",
                    perusahaan: "Balai Pengujian Mutu Pangan",
                    kota: "Jakarta Timur",
                    posisi: "Supervisor Laboratorium QC",
                    email: "siti.rahma@mutupangan-example.go.id",
                    hp: "081377665544",
                    investasi: "Gas Chromatography - Mass Spectrometry (GCMS/ GCMSMS), Sample preparation, Gas generator (H2/ N2/ Zero Air)"
                }
            ];
            guestSubmissions = [...sample, ...guestSubmissions];
            renderGuestTable();
            showToast("Contoh data tamu berhasil dimuat", "success");
        }

        function openSetupModal() {
            document.getElementById('scriptUrlInput').value = GOOGLE_SCRIPT_URL;
            document.getElementById('setupModal').classList.remove('hidden');
        }

        function closeSetupModal() {
            document.getElementById('setupModal').classList.add('hidden');
        }

        function saveScriptUrl() {
            const val = document.getElementById('scriptUrlInput').value.trim();
            GOOGLE_SCRIPT_URL = val;
            updateConnectionBadge();
            closeSetupModal();
            if (val) {
                showToast("URL Google Spreadsheet berhasil dihubungkan!", "success");
            } else {
                showToast("Mode penyimpanan lokal diaktifkan", "info");
            }
        }

        async function sendTestPing() {
            const url = document.getElementById('scriptUrlInput').value.trim();
            if (!url) {
                showToast("Masukkan URL Web App Google Script terlebih dahulu", "error");
                return;
            }
            GOOGLE_SCRIPT_URL = url;
            updateConnectionBadge();

            const testPayload = {
                timestamp: new Date().toLocaleString('id-ID'),
                nama: "[TES KONEKSI] Admin BNM",
                perusahaan: "PT. Berca Niaga Medika",
                kota: "Jakarta",
                posisi: "Administrator",
                email: "admin@bercaniaga.co.id",
                hp: "081200000000",
                investasi: "Liquid Chromatography (HPLC/ UHPLC), NIR BUCHI"
            };

            try {
                await fetch(url, {
                    method: 'POST',
                    mode: 'no-cors',
                    headers: { 'Content-Type': 'text/plain;charset=utf-8' },
                    body: JSON.stringify(testPayload)
                });
                showToast("Data tes dikirim! Silakan cek baris baru di Google Sheet Anda.", "success");
            } catch (e) {
                showToast("Gagal menghubungi URL. Pastikan URL Web App benar.", "error");
            }
        }

        function updateConnectionBadge() {
            const badge = document.getElementById('connectionBadge');
            const text = document.getElementById('connectionText');
            if (GOOGLE_SCRIPT_URL) {
                badge.className = "inline-flex items-center gap-1.5 px-2.5 py-1 rounded-full text-xs font-medium bg-emerald-500/20 text-emerald-300 border border-emerald-500/30";
                badge.innerHTML = `<span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span><span>Terhubung ke Google Spreadsheet</span>`;
            } else {
                badge.className = "inline-flex items-center gap-1.5 px-2.5 py-1 rounded-full text-xs font-medium bg-amber-500/20 text-amber-300 border border-amber-500/30";
                badge.innerHTML = `<span class="w-2 h-2 rounded-full bg-amber-400"></span><span>Mode Lokal (Klik 'Integrasi Spreadsheet' untuk Auto-Sync)</span>`;
            }
        }

        function copyAppsScriptCode() {
            const code = document.getElementById('appsScriptCode').innerText;
            copyTextToClipboard(code);
            showToast("Kode Google Apps Script berhasil disalin!", "success");
        }

        function copyTextToClipboard(text) {
            const textarea = document.createElement('textarea');
            textarea.value = text;
            textarea.style.position = 'fixed';
            textarea.style.opacity = '0';
            document.body.appendChild(textarea);
            textarea.select();
            document.execCommand('copy');
            document.body.removeChild(textarea);
        }

        function switchTab(tab) {
            const viewForm = document.getElementById('viewForm');
            const viewData = document.getElementById('viewData');
            const btnForm = document.getElementById('tabBtnForm');
            const btnData = document.getElementById('tabBtnData');

            if (tab === 'form') {
                viewForm.classList.remove('hidden');
                viewData.classList.add('hidden');
                btnForm.className = "px-3 py-1.5 rounded-lg font-medium bg-blue-600 text-white hover:bg-blue-500 transition flex items-center gap-1.5";
                btnData.className = "px-3 py-1.5 rounded-lg font-medium bg-slate-800 text-slate-200 hover:bg-slate-700 transition flex items-center gap-1.5";
            } else {
                viewForm.classList.add('hidden');
                viewData.classList.remove('hidden');
                btnData.className = "px-3 py-1.5 rounded-lg font-medium bg-blue-600 text-white hover:bg-blue-500 transition flex items-center gap-1.5";
                btnForm.className = "px-3 py-1.5 rounded-lg font-medium bg-slate-800 text-slate-200 hover:bg-slate-700 transition flex items-center gap-1.5";
                renderGuestTable();
            }
        }

        function toggleTheme() {
            const body = document.body;
            if (body.classList.contains('theme-corporate')) {
                body.classList.remove('theme-corporate');
                body.classList.add('theme-mint');
                showToast("Tema Latar: Soft Mint / Tosca aktif", "info");
            } else if (body.classList.contains('theme-mint')) {
                body.classList.remove('theme-mint');
                body.classList.add('theme-minimal');
                showToast("Tema Latar: Clean Alabaster White aktif", "info");
            } else {
                body.classList.remove('theme-minimal');
                body.classList.add('theme-corporate');
                showToast("Tema Latar: Executive Navy Pearl aktif", "info");
            }
        }

        function toggleKioskMode(enableKiosk) {
            const toolbar = document.getElementById('adminToolbar');
            const exitBtn = document.getElementById('exitKioskBtn');
            const uploadLabel = document.getElementById('uploadLogoLabel');
            const quickEditBtn = document.getElementById('quickEditEventBtn');

            if (enableKiosk) {
                switchTab('form');
                toolbar.classList.add('hidden');
                exitBtn.classList.remove('hidden');
                exitBtn.classList.add('flex');
                uploadLabel.classList.add('hidden');
                if (quickEditBtn) quickEditBtn.classList.add('hidden');
                showToast("Mode Tamu / Kiosk Aktif", "info");
            } else {
                toolbar.classList.remove('hidden');
                exitBtn.classList.add('hidden');
                exitBtn.classList.remove('flex');
                uploadLabel.classList.remove('hidden');
                if (quickEditBtn) quickEditBtn.classList.remove('hidden');
            }
        }

        function handleCustomLogoUpload(event) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                const customImg = document.getElementById('bnmCustomImg');
                const defaultSvg = document.getElementById('bnmDefaultSvg');
                customImg.src = e.target.result;
                customImg.classList.remove('hidden');
                defaultSvg.classList.add('hidden');
                showToast("Logo berhasil diperbarui dengan file gambar Anda!", "success");
            };
            reader.readAsDataURL(file);
        }

        function showToast(message, type = 'success') {
            const box = document.getElementById('toastBox');
            const msg = document.getElementById('toastMessage');
            const icon = document.getElementById('toastIcon');

            msg.textContent = message;
            if (type === 'error') {
                icon.className = "fa-solid fa-circle-exclamation text-red-400";
            } else if (type === 'info') {
                icon.className = "fa-solid fa-circle-info text-blue-400";
            } else {
                icon.className = "fa-solid fa-circle-check text-emerald-400";
            }

            box.classList.remove('opacity-0', 'translate-y-2');
            box.classList.add('opacity-100', 'translate-y-0');

            clearTimeout(window._toastTimer);
            window._toastTimer = setTimeout(() => {
                box.classList.add('opacity-0', 'translate-y-2');
                box.classList.remove('opacity-100', 'translate-y-0');
            }, 3200);
        }

        function escapeHtml(str) {
            return String(str)
                .replace(/&/g, '&amp;')
                .replace(/</g, '&lt;')
                .replace(/>/g, '&gt;')
                .replace(/"/g, '&quot;')
                .replace(/'/g, '&#039;');
        }
    </script>
</body>
</html>
