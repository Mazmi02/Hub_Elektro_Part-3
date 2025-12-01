<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


    { "en": "Apa Itu Pendinginan ONAN Trafo?", "id": "Oil Natural Air Natural." },
    { "en": "Apa Itu Pendinginan ONAF Trafo?", "id": "Oil Natural Air Forced." },
    { "en": "Apa Itu Pendinginan OFAF Trafo?", "id": "Oil Forced Air Forced." },
    { "en": "Apa Itu Pendinginan OFWF Trafo?", "id": "Oil Forced Water Forced." },
    { "en": "Apa Itu Vector Group Dyn11?", "id": "Delta Star Geser 30 Derajat." },
    { "en": "Apa Itu Vector Group Yyn0?", "id": "Star Star Fasa Sama." },
    { "en": "Apa Isi Breather Trafo?", "id": "Silica Gel Penyerap Lembap." },
    { "en": "Apa Itu Bushing Trafo?", "id": "Isolator Terminal Tembus Tangki." },
    { "en": "Apa Itu Arcing Horn Bushing?", "id": "Proteksi Bushing Dari Petir." },
    { "en": "Apa Itu Tap Changer On-Load?", "id": "Pindah Tap Saat Berbeban." },
    { "en": "Apa Itu Tap Changer Off-Load?", "id": "Pindah Tap Saat Mati." },
    { "en": "Apa Itu Short Circuit Voltage (Uk)?", "id": "Tegangan Impedansi Trafo." },
    { "en": "Apa Itu Inrush Current Trafo?", "id": "Arus Kejut Saat Energize." },
    { "en": "Berapa Lama Inrush Current Terjadi?", "id": "Beberapa Siklus Gelombang Awal." },
    { "en": "Apa Itu Harmonisa Ke-3 Trafo?", "id": "Sirkulasi Di Belitan Delta." },
    { "en": "Apa Itu Eddy Current Loss?", "id": "Rugi Panas Arus Pusar." },
    { "en": "Apa Itu Hysteresis Loss?", "id": "Rugi Panas Magnetisasi Inti." },
    { "en": "Bagaimana Cara Mengurangi Histeresis?", "id": "Gunakan Baja Silikon Lunak." },
    { "en": "Bagaimana Cara Mengurangi Arus Eddy?", "id": "Laminasi Inti Besi Tipis." },
    { "en": "Apa Itu Ferroresonance Trafo?", "id": "Osilasi Tegangan Lebih Non-linear." },
    { "en": "Apa Gas Dalam Buchholz?", "id": "Gas Hidrogen Dan Asetilen." },
    { "en": "Apa Itu Sudden Pressure Relay?", "id": "Deteksi Lonjakan Tekanan Minyak." },
    { "en": "Apa Itu Thermal Image Relay?", "id": "Simulasi Suhu Winding Trafo." },
    { "en": "Apa Itu Winding Temperature Indicator?", "id": "Alat Ukur Suhu Belitan." },
    { "en": "Apa Itu Oil Temperature Indicator?", "id": "Alat Ukur Suhu Minyak Atas." },
    { "en": "Apa Itu Transformer Oil BDV?", "id": "Breakdown Voltage Minyak Trafo." },
    { "en": "Berapa Standar BDV Minyak Baru?", "id": "Minimal 30 Kilo Volt." },
    { "en": "Apa Itu DGA Minyak Trafo?", "id": "Dissolved Gas Analysis." },
    { "en": "Apa Indikasi Gas Asetilen DGA?", "id": "Terjadi Busur Api (Arcing)." },
    { "en": "Apa Indikasi Gas Hidrogen DGA?", "id": "Terjadi Partial Discharge." },
    { "en": "Apa Indikasi Gas Etilena DGA?", "id": "Terjadi Overheating Minyak Berat." },
    { "en": "Apa Indikasi Kertas Isolasi Rusak?", "id": "Muncul Gas Karbon Monoksida." },
    { "en": "Apa Itu Furan Analysis?", "id": "Cek Umur Kertas Isolasi." },
    { "en": "Apa Itu Degree Of Polymerization?", "id": "Ukuran Kekuatan Mekanis Kertas." },
    { "en": "Apa Itu Tan Delta Test?", "id": "Ukur Faktor Disipasi Isolasi." },
    { "en": "Apa Itu Sweep Frequency Response Analysis?", "id": "Deteksi Pergeseran Mekanis Belitan." },
    { "en": "Apa Itu Partial Discharge Test?", "id": "Deteksi Peluahan Listrik Kecil." },
    { "en": "Apa Itu Ratio Test Trafo?", "id": "Cek Perbandingan Lilitan Kumparan." },
    { "en": "Apa Itu Polarity Test Trafo?", "id": "Cek Arah Lilitan Kumparan." },
    { "en": "Apa Itu Excitation Current Test?", "id": "Cek Kondisi Inti Besi." },
    { "en": "Apa Itu Motor Induksi Slip Ring?", "id": "Rotor Lilit Dengan Cincin." },
    { "en": "Apa Itu Motor Induksi Sangkar Tupai?", "id": "Rotor Batang Hubung Singkat." },
    { "en": "Apa Kelebihan Motor Slip Ring?", "id": "Torsi Start Tinggi Terkendali." },
    { "en": "Apa Kelebihan Motor Sangkar Tupai?", "id": "Konstruksi Kuat Minim Perawatan." },
    { "en": "Apa Itu Kelas Isolasi B Motor?", "id": "Tahan Suhu 130 Celcius." },
    { "en": "Apa Itu Kelas Isolasi F Motor?", "id": "Tahan Suhu 155 Celcius." },
    { "en": "Apa Itu Kelas Isolasi H Motor?", "id": "Tahan Suhu 180 Celcius." },
    { "en": "Apa Itu Duty Cycle S1?", "id": "Operasi Kontinyu Beban Tetap." },
    { "en": "Apa Itu Duty Cycle S2?", "id": "Operasi Waktu Pendek." },
    { "en": "Apa Itu Duty Cycle S3?", "id": "Operasi Intermiten Tanpa Start." },
    { "en": "Apa Itu Locked Rotor Current?", "id": "Arus Saat Rotor Diam." },
    { "en": "Berapa Besar Arus Start Motor?", "id": "5 Hingga 7 Kali Nominal." },
    { "en": "Apa Itu Full Load Current (FLA)?", "id": "Arus Saat Beban Penuh." },
    { "en": "Apa Itu No Load Current?", "id": "Arus Saat Tanpa Beban." },
    { "en": "Apa Itu Service Factor 1.15?", "id": "Boleh Beban Lebih 15%." },
    { "en": "Apa Itu Frame Size Motor?", "id": "Tinggi Poros Dari Kaki." },
    { "en": "Apa Itu IP55 Pada Motor?", "id": "Tahan Debu Dan Air." },
    { "en": "Apa Itu Cooling Method IC411?", "id": "Pendingin Kipas Luar Tertutup." },
    { "en": "Apa Itu Thermal Overload Relay?", "id": "Proteksi Panas Beban Lebih." },
    { "en": "Apa Itu Single Phasing Motor?", "id": "Hilang Satu Fasa Suplai." },
    { "en": "Apa Akibat Single Phasing?", "id": "Motor Panas Dan Terbakar." },
    { "en": "Apa Itu Phase Unbalance Motor?", "id": "Tegangan Antar Fasa Beda." },
    { "en": "Apa Batas Unbalance Tegangan NEMA?", "id": "Maksimal Satu Persen." },
    { "en": "Apa Itu Motor Heater (Space Heater)?", "id": "Pemanas Anti Embun Saat Mati." },
    { "en": "Apa Fungsi Thermistor PTC Motor?", "id": "Sensor Suhu Winding Internal." },
    { "en": "Apa Itu Bearing DE (Drive End)?", "id": "Bearing Sisi Poros Beban." },
    { "en": "Apa Itu Bearing NDE (Non-Drive End)?", "id": "Bearing Sisi Kipas Pendingin." },
    { "en": "Apa Itu Grease Relief Valve?", "id": "Saluran Buang Gemuk Lama." },
    { "en": "Apa Itu Alignment Laser Motor?", "id": "Luruskan Poros Presisi Tinggi." },
    { "en": "Apa Itu Soft Foot Motor?", "id": "Kaki Motor Tidak Rata." },
    { "en": "Apa Akibat Soft Foot?", "id": "Frame Motor Bengkok Getar." },
    { "en": "Apa Itu VFD Vector Control?", "id": "Kendali Torsi Dan Fluks Terpisah." },
    { "en": "Apa Itu VFD V/f Control?", "id": "Kendali Tegangan Frekuensi Konstan." },
    { "en": "Apa Itu Braking Resistor VFD?", "id": "Buang Energi Regeneratif Motor." },
    { "en": "Apa Itu DC Injection Braking?", "id": "Pengereman Suntik Arus DC." },
    { "en": "Apa Itu Carrier Frequency VFD?", "id": "Frekuensi Switching PWM Inverter." },
    { "en": "Apa Efek Carrier Frequency Tinggi?", "id": "Motor Halus Inverter Panas." },
    { "en": "Apa Itu EMI Filter VFD?", "id": "Saring Noise Frekuensi Tinggi." },
    { "en": "Apa Itu Line Reactor VFD?", "id": "Haluskan Arus Input VFD." },
    { "en": "Apa Itu Load Reactor VFD?", "id": "Lindungi Isolasi Motor Panjang." },
    { "en": "Apa Itu Kabel Shielded VFD?", "id": "Kabel Motor Tahan Interferensi." },
    { "en": "Apa Itu Common Mode Voltage VFD?", "id": "Tegangan Poros Merusak Bearing." },
    { "en": "Apa Itu Shaft Grounding Ring?", "id": "Buang Tegangan Poros Ke Ground." },
    { "en": "Apa Itu Insulated Bearing?", "id": "Bearing Tahan Arus Poros." },
    { "en": "Apa Itu STC Panel Surya?", "id": "Standard Test Conditions 25C." },
    { "en": "Apa Itu NOCT Panel Surya?", "id": "Nominal Operating Cell Temperature." },
    { "en": "Apa Itu Voc Panel Surya?", "id": "Tegangan Open Circuit Maksimal." },
    { "en": "Apa Itu Isc Panel Surya?", "id": "Arus Short Circuit Maksimal." },
    { "en": "Apa Itu Vmp Panel Surya?", "id": "Tegangan Pada Daya Maksimal." },
    { "en": "Apa Itu Imp Panel Surya?", "id": "Arus Pada Daya Maksimal." },
    { "en": "Apa Itu Fill Factor Panel?", "id": "Kualitas Sel Panel Surya." },
    { "en": "Apa Itu Bypass Diode Panel?", "id": "Atasi Efek Bayangan (Shading)." },
    { "en": "Apa Itu Blocking Diode Panel?", "id": "Cegah Arus Balik Malam." },
    { "en": "Apa Itu Solar Tracker?", "id": "Alat Pengikut Gerak Matahari." },
    { "en": "Apa Itu String Inverter?", "id": "Inverter Untuk Rangkaian Seri." },
    { "en": "Apa Itu Central Inverter?", "id": "Inverter Kapasitas Besar Terpusat." },
    { "en": "Apa Itu Micro Inverter?", "id": "Inverter Kecil Per Panel." },
    { "en": "Apa Itu DC Optimizer?", "id": "Pengoptimal Daya Per Panel." },
    { "en": "Apa Itu Anti-Islanding Inverter?", "id": "Mati Saat Grid PLN Padam." },
    { "en": "Apa Itu Net Metering EXIM?", "id": "Ekspor Impor Listrik PLN." },
    { "en": "Apa Itu Sistem Grounding TT?", "id": "Netral Sumber Dan Beban Tanah Terpisah." },
    { "en": "Apa Itu Sistem Grounding TN-C?", "id": "Netral Dan Ground Gabung Satu Kabel." },
    { "en": "Apa Itu Sistem Grounding TN-S?", "id": "Netral Dan Ground Terpisah Dari Sumber." },
    { "en": "Apa Itu Sistem Grounding TN-C-S?", "id": "Gabungan TN-C Diikuti TN-S." },
    { "en": "Apa Itu Sistem Grounding IT?", "id": "Tidak Ada Hubungan Langsung Ke Tanah." },
    { "en": "Apa Keunggulan Sistem IT?", "id": "Tetap Operasi Saat Satu Fasa Bocor." },
    { "en": "Apa Kekurangan Sistem TN-C?", "id": "Bahaya Jika Kabel PEN Putus." },
    { "en": "Apa Warna Kabel PEN Standar?", "id": "Hijau Kuning Dengan Ujung Biru." },
    { "en": "Apa Fungsi Equipotential Bonding?", "id": "Samakan Potensial Semua Logam Terbuka." },
    { "en": "Apa Itu Grounding Grid?", "id": "Jaring Kawat Tembaga Bawah Tanah." },
    { "en": "Apa Fungsi Bak Kontrol Grounding?", "id": "Tempat Pengukuran Nilai Tahanan Tanah." },
    { "en": "Apa Itu Earth Resistance Tester?", "id": "Alat Ukur Tahanan Pembumian." },
    { "en": "Apa Metode Pengukuran 3 Kutub?", "id": "Ukur Tanah Pakai Dua Pasak Bantu." },
    { "en": "Apa Jarak Ideal Pasak Bantu?", "id": "Sekitar 5 Hingga 10 Meter." },
    { "en": "Apa Nilai Grounding Ruang Server?", "id": "Wajib Di Bawah 1 Ohm." },
    { "en": "Apa Nilai Grounding Penangkal Petir?", "id": "Maksimal 5 Ohm." },
    { "en": "Apa Fungsi Grounding Rod Tembaga?", "id": "Elektroda Batang Tanam Ke Tanah." },
    { "en": "Apa Fungsi Semen Konduktif?", "id": "Turunkan Resistansi Tanah Berbatu." },
    { "en": "Apa Bahasa PLC Ladder Diagram?", "id": "Logika Grafis Mirip Rangkaian Relay." },
    { "en": "Apa Bahasa PLC Function Block?", "id": "Blok Logika Input Proses Output." },
    { "en": "Apa Bahasa PLC Structured Text?", "id": "Bahasa Teks Mirip Pascal Atau C." },
    { "en": "Apa Bahasa PLC Instruction List?", "id": "Kode Mnemonic Mirip Assembly." },
    { "en": "Apa Bahasa PLC SFC?", "id": "Sequential Function Chart Alur Grafis." },
    { "en": "Apa Fungsi Rung Pada Ladder?", "id": "Baris Logika Eksekusi Kiri Kanan." },
    { "en": "Apa Itu Kontak Rising Edge?", "id": "Aktif Sesaat Saat Sinyal Naik." },
    { "en": "Apa Itu Kontak Falling Edge?", "id": "Aktif Sesaat Saat Sinyal Turun." },
    { "en": "Apa Fungsi Coil Set PLC?", "id": "Aktifkan Output Dan Kunci (Latch)." },
    { "en": "Apa Fungsi Coil Reset PLC?", "id": "Matikan Output Yang Terkunci." },
    { "en": "Apa Itu Timer Retentive PLC?", "id": "Simpan Waktu Saat Power Mati." },
    { "en": "Apa Itu Counter High Speed?", "id": "Hitung Pulsa Cepat Dari Encoder." },
    { "en": "Apa Fungsi Analog Scaling PLC?", "id": "Konversi Nilai Raw Ke Engineering." },
    { "en": "Apa Itu PID Loop PLC?", "id": "Algoritma Kontrol Proporsional Integral Derivatif." },
    { "en": "Apa Kabel Coaxial RG-8?", "id": "Kabel RF Tebal Rugi Rendah." },
    { "en": "Apa Kabel Coaxial RG-58?", "id": "Kabel RF Tipis Standar Radio." },
    { "en": "Apa Kabel Coaxial RG-174?", "id": "Kabel RF Sangat Tipis Fleksibel." },
    { "en": "Apa Kabel Coaxial LMR-400?", "id": "Kabel Low Loss Frekuensi Tinggi." },
    { "en": "Apa Itu Kabel Twinax?", "id": "Kabel Tembaga Kecepatan Tinggi Server." },
    { "en": "Apa Itu Kabel Triax?", "id": "Coaxial Dengan Dua Lapisan Shield." },
    { "en": "Apa Fungsi Kabel Ribbon?", "id": "Kabel Pita Pipih Koneksi Internal." },
    { "en": "Apa Itu Kabel FFC (Flat Flex)?", "id": "Kabel Pipih Tipis Elektronik Modern." },
    { "en": "Apa Itu Kabel NYMHY?", "id": "Kabel Serabut Putih Fleksibel." },
    { "en": "Apa Itu Kabel NYYHY?", "id": "Kabel Serabut Hitam Kulit Keras." },
    { "en": "Apa Fungsi Cycloconverter?", "id": "Ubah Frekuensi AC Tanpa DC Link." },
    { "en": "Apa Fungsi Chopper DC?", "id": "Ubah Tegangan DC Ke DC Variabel." },
    { "en": "Apa Itu Buck Regulator?", "id": "Chopper Penurun Tegangan DC." },
    { "en": "Apa Itu Boost Regulator?", "id": "Chopper Penaik Tegangan DC." },
    { "en": "Apa Itu Buck-Boost Regulator?", "id": "Bisa Naik Dan Turun Tegangan." },
    { "en": "Apa Itu Cuk Converter?", "id": "Regulator DC Output Terbalik Polaritas." },
    { "en": "Apa Fungsi Snubber RCD?", "id": "Resistor Kapasitor Dioda Pelindung." },
    { "en": "Apa Itu ZCS (Zero Current Switching)?", "id": "Switching Saat Arus Nol." },
    { "en": "Apa Itu ZVS (Zero Voltage Switching)?", "id": "Switching Saat Tegangan Nol." },
    { "en": "Apa Keuntungan Soft Switching?", "id": "Kurangi Rugi Daya Dan EMI." },
    { "en": "Apa Itu Matrix Converter?", "id": "Konverter AC Ke AC Langsung." },
    { "en": "Apa Itu Multilevel Inverter?", "id": "Inverter Output Tangga Halus." },
    { "en": "Apa Fungsi Gate Drive Optocoupler?", "id": "Isolasi Dan Drive MOSFET IGBT." },
    { "en": "Apa Itu Dead Time Control?", "id": "Cegah Shoot-through Pada H-Bridge." },
    { "en": "Apa Fungsi Power Quality Analyzer?", "id": "Analisis Detail Gangguan Listrik." },
    { "en": "Apa Itu Clamp Meter Leakage?", "id": "Tang Ampere Ukur Arus Bocor." },
    { "en": "Apa Resolusi Clamp Meter Leakage?", "id": "Biasanya 0.01 Miliampere." },
    { "en": "Apa Fungsi Phase Rotation Meter?", "id": "Cek Urutan Fasa R-S-T." },
    { "en": "Apa Fungsi Insulation Tester Digital?", "id": "Megger Dengan Tampilan Angka." },
    { "en": "Apa Fungsi Micro Ohm Meter?", "id": "Ukur Tahanan Kontak Sangat Kecil." },
    { "en": "Apa Fungsi Battery Impedance Tester?", "id": "Ukur Kesehatan Sel Baterai." },
    { "en": "Apa Fungsi Lux Meter Logging?", "id": "Ukur Dan Rekam Data Cahaya." },
    { "en": "Apa Fungsi Sound Level Calibrator?", "id": "Kalibrasi Alat Ukur Kebisingan." },
    { "en": "Apa Itu Luminous Efficacy?", "id": "Rasio Lumen Per Watt Listrik." },
    { "en": "Apa Itu Candela Distribution Curve?", "id": "Grafik Sebaran Cahaya Lampu." },
    { "en": "Apa Itu UGR (Unified Glare Rating)?", "id": "Indeks Kesilauan Cahaya Ruangan." },
    { "en": "Apa Itu CRI (Color Rendering Index)?", "id": "Akurasi Warna Cahaya Lampu." },
    { "en": "Apa Nilai CRI Matahari?", "id": "100 (Sempurna)." },
    { "en": "Apa Nilai CRI Lampu LED Bagus?", "id": "Di Atas 90." },
    { "en": "Apa Itu CCT 2700K?", "id": "Warna Cahaya Pijar (Kuning)." },
    { "en": "Apa Itu CCT 4000K?", "id": "Warna Cahaya Natural (Putih Netral)." },
    { "en": "Apa Itu CCT 6500K?", "id": "Warna Cahaya Siang (Putih Biru)." },
    { "en": "Apa Itu Lampu High Bay?", "id": "Lampu Gantung Langit-langit Tinggi." },
    { "en": "Apa Itu Lampu Street Light?", "id": "Lampu Penerangan Jalan Umum (PJU)." },
    { "en": "Apa Itu Lampu Flood Light?", "id": "Lampu Sorot Area Luas." },
    { "en": "Apa Itu Driver LED Isolated?", "id": "Aman Dari Setrum Listrik." },
    { "en": "Apa Itu Driver LED Non-isolated?", "id": "Murah Tapi Rawan Setrum." },
    { "en": "Apa Itu Smart Lighting Zigbee?", "id": "Lampu Pintar Protokol Zigbee." },
    { "en": "Apa Itu Smart Lighting DALI?", "id": "Sistem Kontrol Lampu Digital Gedung." },
    { "en": "Apa Fungsi Heat Shrink Busbar?", "id": "Isolasi Warna Pada Rel Tembaga." },
    { "en": "Apa Warna Heat Shrink Fasa R?", "id": "Merah." },
    { "en": "Apa Warna Heat Shrink Fasa S?", "id": "Kuning." },
    { "en": "Apa Warna Heat Shrink Fasa T?", "id": "Hitam." },
    { "en": "Apa Warna Heat Shrink Netral?", "id": "Biru." },
    { "en": "Apa Warna Heat Shrink Ground?", "id": "Hijau Kuning." },
    { "en": "Apa Itu Skun Ring Tembaga?", "id": "Terminal Kabel Bulat Baut." },
    { "en": "Apa Itu Skun Garpu Tembaga?", "id": "Terminal Kabel Huruf Y." },
    { "en": "Apa Itu Skun Pin (Jarum)?", "id": "Terminal Kabel Masuk MCB." },
    { "en": "Apa Itu Ferrule Twin (Ganda)?", "id": "Press Dua Kabel Satu Terminal." },
    { "en": "Apa Itu Cable Gland Nickel?", "id": "Pengikat Kabel Bahan Logam." },
    { "en": "Apa Itu Spiral Wrapping Band?", "id": "Pelindung Kabel Fleksibel Spiral." },
    { "en": "Apa Itu Wiring Duct Slotted?", "id": "Jalur Kabel Panel Berlubang." },
    { "en": "Apa Fungsi Din Rail Cutter?", "id": "Alat Potong Rel Panel Rapi." },
    { "en": "Apa Fungsi Wiring Duct Cutter?", "id": "Alat Potong Trunking Plastik." },
    { "en": "Apa Itu Label Printer Handheld?", "id": "Mesin Cetak Label Kabel Portable." },
    { "en": "Apa Itu Heat Gun Digital?", "id": "Pemanas Udara Suhu Teratur." },
    { "en": "Apa Itu Form Factor Gelombang AC?", "id": "Nilai RMS Dibagi Nilai Rata-rata." },
    { "en": "Apa Itu Peak Factor Gelombang AC?", "id": "Nilai Puncak Dibagi Nilai RMS." },
    { "en": "Apa Nilai Form Factor Gelombang Sinus?", "id": "1.11 (Satu Koma Satu Satu)." },
    { "en": "Apa Nilai Peak Factor Gelombang Sinus?", "id": "1.414 (Akar Dua)." },
    { "en": "Apa Itu Ripple Voltage?", "id": "Komponen AC Sisa Pada Output DC." },
    { "en": "Apa Itu Load Regulation?", "id": "Kestabilan Tegangan Saat Beban Berubah." },
    { "en": "Apa Itu Line Regulation?", "id": "Kestabilan Tegangan Saat Input Berubah." },
    { "en": "Apa Itu Dropout Voltage Regulator?", "id": "Selisih Minimum Input Output Agar Stabil." },
    { "en": "Apa Itu PSRR (Power Supply Rejection Ratio)?", "id": "Kemampuan Redam Noise Input Power." },
    { "en": "Apa Itu Efficiency Power Supply?", "id": "Daya Output Dibagi Daya Input." },
    { "en": "Apa Itu Standar IEEE 802.11?", "id": "Standar Jaringan Nirkabel WiFi." },
    { "en": "Apa Itu Standar IEEE 802.3?", "id": "Standar Jaringan Kabel Ethernet." },
    { "en": "Apa Itu Standar IEEE 802.15.1?", "id": "Standar Jaringan Bluetooth." },
    { "en": "Apa Itu Standar IEEE 802.15.4?", "id": "Standar Low Rate WPAN (Zigbee)." },
    { "en": "Apa Itu ISM Band?", "id": "Frekuensi Bebas Industri Medis Ilmiah." },
    { "en": "Apa Frekuensi ISM Paling Umum?", "id": "2.4 Giga Hertz." },
    { "en": "Apa Itu Fresnel Zone Wireless?", "id": "Area Ellipsoid Bebas Halangan Sinyal." },
    { "en": "Apa Itu LOS (Line Of Sight)?", "id": "Jalur Pandang Langsung Antar Antena." },
    { "en": "Apa Itu NLOS (Non Line Of Sight)?", "id": "Jalur Sinyal Terhalang Objek." },
    { "en": "Apa Itu Fading Sinyal?", "id": "Fluktuasi Kekuatan Sinyal Penerima." },
    { "en": "Apa Itu Multipath Propagation?", "id": "Sinyal Sampai Melalui Banyak Jalur." },
    { "en": "Apa Itu Beamforming Antena?", "id": "Arahkan Sinyal Ke Perangkat Tertentu." },
    { "en": "Apa Itu MIMO (Multiple Input Multiple Output)?", "id": "Banyak Antena Kirim Terima Data." },
    { "en": "Apa Itu Antena Dipole?", "id": "Antena Dua Kutub Paling Dasar." },
    { "en": "Apa Itu Antena Monopole?", "id": "Antena Satu Kutub Dengan Ground Plane." },
    { "en": "Apa Itu Antena Yagi-Uda?", "id": "Antena Pengarah Dengan Elemen Parasitik." },
    { "en": "Apa Itu Antena Parabolik?", "id": "Antena Piringan Fokuskan Sinyal." },
    { "en": "Apa Itu Antena Patch (Microstrip)?", "id": "Antena Cetak Pada Papan PCB." },
    { "en": "Apa Itu Antena Horn?", "id": "Antena Corong Pandu Gelombang Mikro." },
    { "en": "Apa Itu VSWR Meter?", "id": "Alat Ukur Rasio Gelombang Berdiri." },
    { "en": "Apa Nilai VSWR Sempurna?", "id": "1 Banding 1." },
    { "en": "Apa Itu Return Loss?", "id": "Daya Yang Memantul Kembali." },
    { "en": "Apa Itu Insertion Loss?", "id": "Daya Hilang Saat Komponen Dipasang." },
    { "en": "Apa Itu Characteristic Impedance?", "id": "Impedansi Kabel Saluran Transmisi." },
    { "en": "Apa Itu Velocity Factor Kabel?", "id": "Kecepatan Sinyal Dalam Kabel." },
    { "en": "Apa Itu Metrologi Listrik?", "id": "Ilmu Pengukuran Besaran Listrik." },
    { "en": "Apa Itu Kalibrasi Alat Ukur?", "id": "Bandingkan Alat Dengan Standar Acuan." },
    { "en": "Apa Itu Traceability (Ketertelusuran)?", "id": "Rantai Kalibrasi Ke Standar Internasional." },
    { "en": "Apa Itu Uncertainty (Ketidakpastian)?", "id": "Rentang Keraguan Hasil Pengukuran." },
    { "en": "Apa Itu Resolution (Resolusi) Alat?", "id": "Perubahan Terkecil Yang Terbaca." },
    { "en": "Apa Itu Sensitivity (Sensitivitas)?", "id": "Respon Alat Terhadap Perubahan Input." },
    { "en": "Apa Itu Hysteresis Error?", "id": "Beda Bacaan Saat Naik Turun." },
    { "en": "Apa Itu Parallax Error?", "id": "Kesalahan Baca Sudut Pandang Jarum." },
    { "en": "Apa Itu Zero Error?", "id": "Alat Tidak Nol Saat Diam." },
    { "en": "Apa Itu Multimeter True RMS?", "id": "Akurat Ukur Gelombang Non-Sinus." },
    { "en": "Apa Itu Clamp Meter Hall Effect?", "id": "Tang Ampere Bisa Ukur DC." },
    { "en": "Apa Itu Galvanometer D'Arsonval?", "id": "Gerakan Jarum Kumparan Putar Magnet." },
    { "en": "Apa Itu Shunt Resistor Ammeter?", "id": "Resistor Paralel Perluas Ukur Arus." },
    { "en": "Apa Itu Multiplier Resistor Voltmeter?", "id": "Resistor Seri Perluas Ukur Tegangan." },
    { "en": "Apa Itu Wheatstone Bridge?", "id": "Rangkaian Ukur Resistansi Presisi." },
    { "en": "Apa Itu Kelvin Double Bridge?", "id": "Ukur Resistansi Sangat Rendah." },
    { "en": "Apa Itu Maxwell Bridge?", "id": "Ukur Induktansi Kumparan." },
    { "en": "Apa Itu Schering Bridge?", "id": "Ukur Kapasitansi Dan Sudut Rugi." },
    { "en": "Apa Itu Wien Bridge Oscillator?", "id": "Pembangkit Gelombang Sinus Audio." },
    { "en": "Apa Itu Lissajous Figure?", "id": "Pola Grafik Beda Fasa Osiloskop." },
    { "en": "Apa Itu Spectrum Analyzer?", "id": "Tampilkan Amplitudo Terhadap Frekuensi." },
    { "en": "Apa Itu Logic Analyzer?", "id": "Analisis Banyak Sinyal Digital." },
    { "en": "Apa Itu Function Generator?", "id": "Pembangkit Berbagai Bentuk Gelombang." },
    { "en": "Apa Itu Frequency Counter?", "id": "Penghitung Frekuensi Sinyal Akurat." },
    { "en": "Apa Itu LCR Meter?", "id": "Ukur Induktansi Kapasitansi Resistansi." },
    { "en": "Apa Itu ESR Meter?", "id": "Ukur Resistansi Seri Ekuivalen Kapasitor." },
    { "en": "Apa Itu Megohmmeter (Megger)?", "id": "Ukur Tahanan Isolasi Tinggi." },
    { "en": "Apa Itu Earth Tester?", "id": "Ukur Tahanan Pembumian Tanah." },
    { "en": "Apa Itu Phase Sequence Meter?", "id": "Cek Urutan Fasa R-S-T." },
    { "en": "Apa Itu Lux Meter?", "id": "Ukur Intensitas Cahaya Ruangan." },
    { "en": "Apa Itu Sound Level Meter?", "id": "Ukur Tingkat Kebisingan Suara." },
    { "en": "Apa Itu Tachometer?", "id": "Ukur Kecepatan Putaran Poros." },
    { "en": "Apa Itu Anemometer?", "id": "Ukur Kecepatan Aliran Udara." },
    { "en": "Apa Itu Thermal Imager?", "id": "Kamera Deteksi Panas Inframerah." },
    { "en": "Apa Itu Vibration Meter?", "id": "Ukur Getaran Mesin Motor." },
    { "en": "Apa Itu Thickness Gauge?", "id": "Ukur Ketebalan Cat Atau Plat." },
    { "en": "Apa Itu Coating Thickness?", "id": "Ketebalan Lapisan Pelindung." },
    { "en": "Apa Itu Hardness Tester?", "id": "Ukur Kekerasan Material Logam." },
    { "en": "Apa Itu Roughness Tester?", "id": "Ukur Kekasaran Permukaan Benda." },
    { "en": "Apa Itu pH Meter Digital?", "id": "Ukur Keasaman Larutan Cairan." },
    { "en": "Apa Itu TDS Meter?", "id": "Ukur Padatan Terlarut Air." },
    { "en": "Apa Itu Conductivity Meter?", "id": "Ukur Daya Hantar Listrik Air." },
    { "en": "Apa Itu Refractometer?", "id": "Ukur Indeks Bias Cairan." },
    { "en": "Apa Itu Gas Detector?", "id": "Deteksi Kebocoran Gas Berbahaya." },
    { "en": "Apa Itu Particle Counter?", "id": "Hitung Jumlah Partikel Debu Udara." },
    { "en": "Apa Itu Gauss Meter?", "id": "Ukur Kekuatan Medan Magnet." },
    { "en": "Apa Itu Tesla Meter?", "id": "Nama Lain Dari Gauss Meter." },
    { "en": "Apa Itu EMF Meter?", "id": "Ukur Radiasi Elektromagnetik Lingkungan." },
    { "en": "Apa Itu Cable Fault Locator?", "id": "Lacak Lokasi Putus Kabel Tanah." },
    { "en": "Apa Itu TDR (Time Domain Reflectometer)?", "id": "Ukur Panjang Kabel Lewat Pantulan." },
    { "en": "Apa Itu OTDR Fiber Optik?", "id": "Optical Time Domain Reflectometer." },
    { "en": "Apa Itu Optical Power Meter?", "id": "Ukur Daya Sinyal Cahaya Fiber." },
    { "en": "Apa Itu Visual Fault Locator?", "id": "Laser Merah Cek Fiber Putus." },
    { "en": "Apa Itu Battery Tester?", "id": "Ukur Kesehatan Dan Kapasitas Aki." },
    { "en": "Apa Itu Load Bank?", "id": "Beban Buatan Tes Genset." },
    { "en": "Apa Itu Solar Power Meter?", "id": "Ukur Intensitas Radiasi Matahari." },
    { "en": "Apa Itu UV Meter?", "id": "Ukur Intensitas Sinar Ultraviolet." },
    { "en": "Apa Itu Moisture Meter?", "id": "Ukur Kadar Air Kayu/Beton." },
    { "en": "Apa Itu Ultrasonic Thickness Gauge?", "id": "Ukur Tebal Pipa Tanpa Rusak." },
    { "en": "Apa Itu Gloss Meter?", "id": "Ukur Tingkat Kilap Permukaan." },
    { "en": "Apa Itu Colorimeter?", "id": "Ukur Konsistensi Warna Produk." },
    { "en": "Apa Itu Spectrophotometer?", "id": "Analisis Spektrum Warna Cahaya." },
    { "en": "Apa Itu Network Analyzer (VNA)?", "id": "Ukur Parameter S Jaringan RF." },
    { "en": "Apa Itu Signal Generator RF?", "id": "Pembangkit Sinyal Frekuensi Tinggi." },
    { "en": "Apa Warna Resistor 100 Ohm?", "id": "Cokelat Hitam Cokelat Emas." },
    { "en": "Apa Warna Resistor 220 Ohm?", "id": "Merah Merah Cokelat Emas." },
    { "en": "Apa Warna Resistor 330 Ohm?", "id": "Oranye Oranye Cokelat Emas." },
    { "en": "Apa Warna Resistor 1k Ohm?", "id": "Cokelat Hitam Merah Emas." },
    { "en": "Apa Warna Resistor 10k Ohm?", "id": "Cokelat Hitam Oranye Emas." },
    { "en": "Apa Warna Resistor 100k Ohm?", "id": "Cokelat Hitam Kuning Emas." },
    { "en": "Apa Warna Resistor 1M Ohm?", "id": "Cokelat Hitam Hijau Emas." },
    { "en": "Apa Kode Kapasitor 1nF?", "id": "102." },
    { "en": "Apa Kode Kapasitor 10nF?", "id": "103." },
    { "en": "Apa Kode Kapasitor 100nF?", "id": "104." },
    { "en": "Apa Kode Kapasitor 1uF?", "id": "105." },
    { "en": "Apa Fungsi Varistor?", "id": "Proteksi Lonjakan Tegangan." },
    { "en": "Apa Fungsi Thermistor NTC?", "id": "Sensor Suhu Atau Inrush Limiter." },
    { "en": "Apa Fungsi Thermistor PTC?", "id": "Proteksi Arus Lebih Reset Otomatis." },
    { "en": "Apa Fungsi LDR?", "id": "Sensor Cahaya Resistif." },
    { "en": "Apa Fungsi Photodiode?", "id": "Sensor Cahaya Respons Cepat." },
    { "en": "Apa Fungsi Phototransistor?", "id": "Sensor Cahaya Dengan Penguatan." },
    { "en": "Apa Fungsi Optocoupler?", "id": "Isolasi Sinyal Listrik Cahaya." },
    { "en": "Apa Fungsi Relay?", "id": "Saklar Mekanik Kontrol Listrik." },
    { "en": "Apa Fungsi SSR (Solid State Relay)?", "id": "Saklar Elektronik Tanpa Gerak." },
    { "en": "Apa Fungsi MOSFET?", "id": "Saklar Elektronik Kecepatan Tinggi." },
    { "en": "Apa Fungsi IGBT?", "id": "Saklar Daya Tinggi Frekuensi Menengah." },
    { "en": "Apa Fungsi TRIAC?", "id": "Saklar AC Dua Arah." },
    { "en": "Apa Fungsi SCR?", "id": "Saklar DC Satu Arah Terkendali." },
    { "en": "Apa Fungsi Diac?", "id": "Pemicu Gate TRIAC." },
    { "en": "Apa Fungsi Zener Diode?", "id": "Stabilisasi Tegangan Reverse Bias." },
    { "en": "Apa Fungsi Schottky Diode?", "id": "Penyearah Cepat Drop Rendah." },
    { "en": "Apa Fungsi Bridge Diode?", "id": "Penyearah Gelombang Penuh." },
    { "en": "Apa Fungsi LED?", "id": "Indikator Cahaya Hemat Energi." },
    { "en": "Apa Fungsi Seven Segment?", "id": "Tampilan Angka Digital." },
    { "en": "Apa Fungsi LCD 16x2?", "id": "Tampilan Teks Karakter." },
    { "en": "Apa Fungsi OLED Display?", "id": "Tampilan Grafis Kontras Tinggi." },
    { "en": "Apa Fungsi Buzzer?", "id": "Hasilkan Suara Beep." },
    { "en": "Apa Fungsi Speaker?", "id": "Hasilkan Suara Audio." },
    { "en": "Apa Fungsi Microphone?", "id": "Ubah Suara Jadi Listrik." },
    { "en": "Apa Fungsi Potensiometer?", "id": "Resistor Variabel Putar." },
    { "en": "Apa Fungsi Trimpot?", "id": "Resistor Variabel Setelan Obeng." },
    { "en": "Apa Fungsi Encoder?", "id": "Input Putar Digital." },
    { "en": "Apa Fungsi Switch Tactile?", "id": "Tombol Tekan Sesaat." },
    { "en": "Apa Fungsi Switch Toggle?", "id": "Saklar Tuas Pengunci." },
    { "en": "Apa Fungsi Switch Slide?", "id": "Saklar Geser." },
    { "en": "Apa Fungsi Switch Rocker?", "id": "Saklar Jungkit Power." },
    { "en": "Apa Fungsi Fuse Holder?", "id": "Rumah Sekering." },
    { "en": "Apa Fungsi Battery Holder?", "id": "Rumah Baterai." },
    { "en": "Apa Fungsi DC Jack?", "id": "Konektor Daya Adaptor." },
    { "en": "Apa Fungsi USB Port?", "id": "Konektor Data Dan Daya." },
    { "en": "Apa Fungsi Header Pin?", "id": "Konektor Kaki Komponen." },
    { "en": "Apa Fungsi Terminal Block?", "id": "Konektor Kabel Baut." },
    { "en": "Apa Fungsi Jumper Cap?", "id": "Penghubung Pin Header." },
    { "en": "Apa Fungsi Breadboard?", "id": "Papan Rangkaian Percobaan." },
    { "en": "Apa Fungsi PCB?", "id": "Papan Sirkuit Cetak." },
    { "en": "Apa Fungsi Solder?", "id": "Alat Pemanas Timah." },
    { "en": "Apa Fungsi Timah Solder?", "id": "Bahan Penyambung Logam." },
    { "en": "Apa Fungsi Flux?", "id": "Cairan Pembersih Oksidasi Solder." },
    { "en": "Apa Fungsi Desoldering Pump?", "id": "Penyedot Timah Cair." },
    { "en": "Apa Fungsi Multimeter?", "id": "Alat Ukur Listrik Serbaguna." },
    { "en": "Apa Fungsi Oscilloscope?", "id": "Alat Ukur Bentuk Gelombang." },
    { "en": "Apa Fungsi Logic Analyzer?", "id": "Alat Analisis Sinyal Digital." },
    { "en": "Apa Fungsi Signal Generator?", "id": "Pembangkit Sinyal Uji." },
    { "en": "Apa Fungsi Power Supply?", "id": "Sumber Daya Listrik DC." },
    { "en": "Apa Fungsi Frequency Counter?", "id": "Penghitung Frekuensi Sinyal." },
    { "en": "Apa Fungsi LCR Meter?", "id": "Ukur Induktansi Kapasitansi Resistansi." },
    { "en": "Apa Fungsi ESR Meter?", "id": "Ukur Kesehatan Kapasitor Elco." },
    { "en": "Apa Fungsi Transistor Tester?", "id": "Cek Jenis Kaki Transistor." },
    { "en": "Apa Fungsi IC Tester?", "id": "Cek Fungsi Logika IC." },
    { "en": "Apa Fungsi Kabel Jumper?", "id": "Kabel Penghubung Rangkaian." },
    { "en": "Apa Fungsi Kabel Probe?", "id": "Kabel Tusuk Alat Ukur." },
    { "en": "Apa Fungsi Kabel Alligator?", "id": "Kabel Jepit Buaya." },
    { "en": "Apa Fungsi Kabel USB?", "id": "Kabel Data Komputer." },
    { "en": "Apa Fungsi Kabel LAN?", "id": "Kabel Jaringan Komputer." },
    { "en": "Apa Fungsi Kabel Coaxial?", "id": "Kabel Antena RF." },
    { "en": "Apa Fungsi Kabel Fiber Optik?", "id": "Kabel Transmisi Cahaya." },
    { "en": "Apa Fungsi Kabel HDMI?", "id": "Kabel Video Audio Digital." },
    { "en": "Apa Fungsi Kabel VGA?", "id": "Kabel Video Analog." },
    { "en": "Apa Fungsi Kabel RCA?", "id": "Kabel Audio Video Analog." },
    { "en": "Apa Fungsi Kabel Audio Jack?", "id": "Kabel Headphone Speaker." },
    { "en": "Apa Fungsi Kabel Power AC?", "id": "Kabel Listrik Jala-jala." },
    { "en": "Apa Fungsi Kabel Ribbon?", "id": "Kabel Pita Pipih." },
    { "en": "Apa Fungsi Heat Shrink?", "id": "Pelindung Sambungan Kabel." },
    { "en": "Apa Fungsi Cable Ties?", "id": "Pengikat Rapikan Kabel." },
    { "en": "Apa Fungsi Spacer?", "id": "Penyangga PCB." },
    { "en": "Apa Fungsi Baut Mur?", "id": "Pengunci Mekanis Komponen." },
    { "en": "Apa Fungsi Box Project?", "id": "Wadah Rangkaian Jadi." },
    { "en": "Apa Fungsi Knob Potensiometer?", "id": "Pegangan Putar Potensio." },
    { "en": "Apa Fungsi Rubber Feet?", "id": "Kaki Karet Box." },
    { "en": "Apa Fungsi Label Sticker?", "id": "Penanda Tombol Indikator." },
    { "en": "Apa Fungsi Glue Gun?", "id": "Lem Tembak Perekat." },
    { "en": "Apa Fungsi Double Tape?", "id": "Perekat Dua Sisi." },
    { "en": "Apa Fungsi Amplas?", "id": "Pembersih PCB Polos." },
    { "en": "Apa Fungsi Ferric Chloride?", "id": "Pelarut Tembaga PCB." },
    { "en": "Apa Fungsi Spidol PCB?", "id": "Penulis Jalur PCB Manual." },
    { "en": "Apa Fungsi Bor PCB?", "id": "Pelubang Kaki Komponen." },
    { "en": "Apa Fungsi Tang Potong?", "id": "Pemotong Kaki Komponen." },
    { "en": "Apa Fungsi Tang Lancip?", "id": "Penjepit Komponen Kecil." },
    { "en": "Apa Fungsi Obeng?", "id": "Pengencang Baut Mur." },
    { "en": "Apa Fungsi Cutter?", "id": "Pemotong Isolasi Kabel." },
    { "en": "Apa Fungsi Pinset?", "id": "Pegang Komponen SMD." },
    { "en": "Apa Fungsi Kaca Pembesar?", "id": "Alat Bantu Lihat Detail." },
    { "en": "Apa Fungsi Lampu Meja?", "id": "Penerangan Area Kerja." },
    { "en": "Apa Kepanjangan BEV?", "id": "Battery Electric Vehicle." },
    { "en": "Apa Kepanjangan PHEV?", "id": "Plug-in Hybrid Electric Vehicle." },
    { "en": "Apa Kepanjangan HEV?", "id": "Hybrid Electric Vehicle." },
    { "en": "Apa Kepanjangan FCEV?", "id": "Fuel Cell Electric Vehicle." },
    { "en": "Apa Sumber Energi FCEV?", "id": "Gas Hidrogen." },
    { "en": "Apa Itu ICE Vehicle?", "id": "Kendaraan Mesin Pembakaran Internal." },
    { "en": "Apa Satuan Kapasitas Baterai EV?", "id": "Kilo Watt Hour (kWh)." },
    { "en": "Apa Itu Range Anxiety?", "id": "Ketakutan Kehabisan Baterai Di Jalan." },
    { "en": "Apa Itu Regenerative Braking?", "id": "Pengereman Hasilkan Listrik Kembali." },
    { "en": "Apa Itu One Pedal Driving?", "id": "Gas Dan Rem Satu Pedal." },
    { "en": "Apa Fungsi Thermal Management System?", "id": "Jaga Suhu Baterai Tetap Optimal." },
    { "en": "Apa Pendingin Baterai EV Umum?", "id": "Cairan Coolant Glycol." },
    { "en": "Apa Itu SOH Baterai?", "id": "State Of Health (Kesehatan)." },
    { "en": "Apa Itu SOC Baterai?", "id": "State Of Charge (Isi)." },
    { "en": "Apa Tegangan Nominal Sel LiFePO4?", "id": "3.2 Volt." },
    { "en": "Apa Tegangan Penuh Sel LiFePO4?", "id": "3.65 Volt." },
    { "en": "Apa Tegangan Cut-off Sel LiFePO4?", "id": "2.5 Volt." },
    { "en": "Apa Tegangan Nominal Sel LTO?", "id": "2.3 Volt." },
    { "en": "Apa Keunggulan Baterai LTO?", "id": "Siklus Hidup Sangat Panjang." },
    { "en": "Apa Itu Baterai NMC?", "id": "Nickel Manganese Cobalt." },
    { "en": "Apa Itu Baterai NCA?", "id": "Nickel Cobalt Aluminum." },
    { "en": "Apa Fungsi BMS Balancing Pasif?", "id": "Buang Energi Sel Tinggi (Resistor)." },
    { "en": "Apa Fungsi BMS Balancing Aktif?", "id": "Transfer Energi Antar Sel." },
    { "en": "Apa Itu C-Rating Discharge?", "id": "Kecepatan Pengosongan Arus Baterai." },
    { "en": "Apa Arti Baterai 2C?", "id": "Habis Dalam 30 Menit." },
    { "en": "Apa Arti Baterai 0.5C?", "id": "Habis Dalam 2 Jam." },
    { "en": "Apa Itu Cycle Life Baterai?", "id": "Jumlah Cas Hingga Kapasitas Turun." },
    { "en": "Apa Cycle Life LiFePO4 Umum?", "id": "Lebih Dari 2000 Siklus." },
    { "en": "Apa Cycle Life Lead Acid?", "id": "Sekitar 300 Hingga 500 Siklus." },
    { "en": "Apa Itu Hybrid Inverter?", "id": "Gabungan On-Grid Dan Off-Grid." },
    { "en": "Apa Fungsi Grid Tie Limiter?", "id": "Cegah Listrik Masuk Ke PLN." },
    { "en": "Apa Itu Zero Export Device?", "id": "Alat Pembatas Ekspor Daya." },
    { "en": "Apa Itu Bifacial Solar Panel?", "id": "Panel Surya Dua Muka Aktif." },
    { "en": "Apa Keuntungan Panel Bifacial?", "id": "Tangkap Cahaya Pantulan Belakang." },
    { "en": "Apa Itu Albedo Effect?", "id": "Pantulan Cahaya Dari Permukaan Tanah." },
    { "en": "Apa Itu Half-Cut Cell Panel?", "id": "Sel Surya Potong Dua Bagian." },
    { "en": "Apa Keunggulan Half-Cut Cell?", "id": "Kurangi Rugi Resistansi Dan Panas." },
    { "en": "Apa Itu PERC Technology?", "id": "Passivated Emitter and Rear Cell." },
    { "en": "Apa Keunggulan Teknologi PERC?", "id": "Efisiensi Tangkap Cahaya Lebih Tinggi." },
    { "en": "Apa Itu Floating PV?", "id": "Panel Surya Terapung Di Air." },
    { "en": "Apa Itu Agrivoltaic?", "id": "Pertanian Dan Panel Surya Digabung." },
    { "en": "Apa Fungsi Pyranometer?", "id": "Ukur Radiasi Matahari Global." },
    { "en": "Apa Fungsi Pyrheliometer?", "id": "Ukur Radiasi Matahari Langsung." },
    { "en": "Apa Itu Solar Tracker Dual Axis?", "id": "Ikuti Matahari Vertikal Dan Horizontal." },
    { "en": "Apa Itu Solar Tracker Single Axis?", "id": "Ikuti Matahari Timur Ke Barat." },
    { "en": "Apa Fungsi Combiner Box DC?", "id": "Gabung Banyak String Panel Surya." },
    { "en": "Apa Komponen Dalam Combiner Box?", "id": "Fuse, MCB DC, Surge Arrester." },
    { "en": "Apa Tegangan String Umum Inverter?", "id": "Mencapai 600 Hingga 1000 Volt." },
    { "en": "Apa Bahaya DC Arc Fault?", "id": "Api Listrik DC Sulit Padam." },
    { "en": "Apa Itu Rapid Shutdown PV?", "id": "Matikan Panel Surya Saat Darurat." },
    { "en": "Apa Fungsi Kunci MC4?", "id": "Buka Pasang Konektor Panel Surya." },
    { "en": "Apa Fungsi Crimping Tool MC4?", "id": "Press Pin Konektor MC4." },
    { "en": "Apa Rating IP Konektor MC4?", "id": "Minimal IP67 Tahan Hujan." },
    { "en": "Apa Kabel Khusus Panel Surya?", "id": "Kabel PV1-F Tahan UV." },
    { "en": "Apa Warna Kabel Positif Solar?", "id": "Merah." },
    { "en": "Apa Warna Kabel Negatif Solar?", "id": "Hitam." },
    { "en": "Apa Itu Voltage Rise Inverter?", "id": "Kenaikan Tegangan Sisi AC Inverter." },
    { "en": "Apa Efek Voltage Rise Tinggi?", "id": "Inverter Trip (Overvoltage Protection)." },
    { "en": "Apa Fungsi Export Import Meter?", "id": "Catat Listrik Masuk Dan Keluar." },
    { "en": "Apa Itu kWh Meter EXIM?", "id": "Meteran PLN Untuk Net Metering." },
    { "en": "Apa Itu Bi-directional Inverter?", "id": "Inverter Dua Arah (Cas/Pakai)." },
    { "en": "Apa Itu V2L (Vehicle To Load)?", "id": "Mobil Listrik Suplai Alat Elektronik." },
    { "en": "Apa Itu V2H (Vehicle To Home)?", "id": "Mobil Listrik Suplai Listrik Rumah." },
    { "en": "Apa Fungsi Wallbox Charger?", "id": "Charger Mobil Listrik Di Dinding." },
    { "en": "Apa Daya Wallbox Charger Umum?", "id": "7 kW Atau 22 kW." },
    { "en": "Apa Itu Port Type 2 Mennekes?", "id": "Konektor AC Standar Mobil Eropa." },
    { "en": "Apa Itu Port CCS2?", "id": "Konektor Combo AC Dan DC." },
    { "en": "Apa Itu Port CHAdeMO?", "id": "Konektor DC Standar Mobil Jepang." },
    { "en": "Apa Itu Supercharger?", "id": "Stasiun Pengisian Cepat Merek Tesla." },
    { "en": "Apa Itu Battery Swapping?", "id": "Tukar Baterai Kosong Dengan Penuh." },
    { "en": "Apa Fungsi Inverter Pump?", "id": "Pompa Air Tenaga Surya Langsung." },
    { "en": "Apa Itu Solar Home System (SHS)?", "id": "Paket PLTS Kecil Untuk Rumah." },
    { "en": "Apa Itu Penerangan Jalan Umum Tenaga Surya?", "id": "PJU-TS (Lampu Jalan Mandiri)." },
    { "en": "Apa Baterai Umum PJU-TS?", "id": "LiFePO4." },
    { "en": "Apa Fungsi Controller PJU-TS?", "id": "Atur Cas Dan Nyala Otomatis." },
    { "en": "Apa Itu Dimming PJU-TS?", "id": "Turunkan Terang Saat Tengah Malam." },
    { "en": "Apa Fungsi Angkur Baut Tiang?", "id": "Pondasi Tiang Lampu Jalan." },
    { "en": "Apa Itu Lumen Per Watt LED?", "id": "Efisiensi Cahaya Lampu LED." },
    { "en": "Berapa Efikasi LED Jalan Bagus?", "id": "Di Atas 130 Lumen Per Watt." },
    { "en": "Apa Itu IP66 Lampu Jalan?", "id": "Tahan Semprotan Air Tekanan Tinggi." },
    { "en": "Apa Fungsi Surge Protection PJU?", "id": "Lindungi Lampu Dari Petir." },
    { "en": "Apa Itu Turbin Angin Sumbu Horizontal?", "id": "HAWT (Kincir Angin Biasa)." },
    { "en": "Apa Itu Turbin Angin Sumbu Vertikal?", "id": "VAWT (Berputar Seperti Gasing)." },
    { "en": "Apa Kelebihan VAWT?", "id": "Tangkap Angin Dari Segala Arah." },
    { "en": "Apa Kelebihan HAWT?", "id": "Efisiensi Lebih Tinggi." },
    { "en": "Apa Fungsi Ekor Turbin Angin?", "id": "Arahkan Baling-baling Ke Angin." },
    { "en": "Apa Itu Furling Turbin Angin?", "id": "Mekanisme Pelindung Angin Kencang." },
    { "en": "Apa Itu Dump Load?", "id": "Beban Pembuang Energi Berlebih." },
    { "en": "Mengapa Perlu Dump Load?", "id": "Cegah Turbin Putar Terlalu Cepat." },
    { "en": "Apa Itu Anemometer?", "id": "Sensor Kecepatan Angin." },
    { "en": "Apa Itu Wind Vane?", "id": "Sensor Arah Angin." },
    { "en": "Apa Itu Betz Limit?", "id": "Batas Teoretis Efisiensi Turbin Angin." },
    { "en": "Berapa Nilai Betz Limit?", "id": "59.3 Persen." },
    { "en": "Apa Itu Tip Speed Ratio?", "id": "Rasio Kecepatan Ujung Baling-baling." },
    { "en": "Apa Generator Turbin Angin Kecil?", "id": "Permanent Magnet Generator (PMG)." },
    { "en": "Apa Output Tegangan PMG?", "id": "AC 3 Fasa Variabel Frekuensi." },
    { "en": "Apa Fungsi Rectifier Turbin Angin?", "id": "Ubah AC Liar Jadi DC." },
    { "en": "Apa Itu Grid Tie Inverter Wind?", "id": "Inverter Angin Sinkron Ke PLN." },
    { "en": "Apa Itu Tegangan Tembus Dielektrik?", "id": "Batas Tegangan Material Menjadi Konduktor." },
    { "en": "Apa Satuan Kekuatan Dielektrik?", "id": "Kilovolt Per Milimeter (kV/mm)." },
    { "en": "Apa Itu Rugi-rugi Dielektrik?", "id": "Energi Hilang Menjadi Panas Isolasi." },
    { "en": "Apa Itu Tan Delta Isolasi?", "id": "Ukuran Kualitas Isolasi Listrik." },
    { "en": "Apa Arti Tan Delta Tinggi?", "id": "Isolasi Buruk Atau Lembap." },
    { "en": "Apa Itu Polarisasi Dielektrik?", "id": "Pergeseran Muatan Dalam Medan Listrik." },
    { "en": "Apa Itu Permitivitas Relatif?", "id": "Kemampuan Material Menyimpan Medan Listrik." },
    { "en": "Apa Itu Fenomena Treeing Kabel?", "id": "Kerusakan Isolasi Menyerupai Akar Pohon." },
    { "en": "Apa Itu Water Treeing?", "id": "Treeing Akibat Kelembapan Dalam Isolasi." },
    { "en": "Apa Itu Electrical Treeing?", "id": "Treeing Akibat Peluahan Listrik Internal." },
    { "en": "Apa Itu Tracking Isolator?", "id": "Jalur Konduktif Karbon Di Permukaan." },
    { "en": "Apa Itu ESDD Isolator?", "id": "Kepadatan Garam Ekuivalen Permukaan." },
    { "en": "Apa Itu NSDD Isolator?", "id": "Kepadatan Debu Tidak Larut Permukaan." },
    { "en": "Apa Itu Hidrofobisitas Isolator?", "id": "Kemampuan Permukaan Menolak Air." },
    { "en": "Apa Kelebihan Isolator Polimer?", "id": "Ringan Dan Hidrofobisitas Baik." },
    { "en": "Apa Kelemahan Isolator Polimer?", "id": "Penuaan Akibat Sinar UV." },
    { "en": "Apa Kelebihan Isolator Keramik?", "id": "Tahan Lama Dan Stabil." },
    { "en": "Apa Kelebihan Isolator Kaca?", "id": "Mudah Deteksi Kerusakan Secara Visual." },
    { "en": "Apa Itu Grading Ring?", "id": "Ratakan Distribusi Tegangan String Isolator." },
    { "en": "Apa Itu Arcing Horn?", "id": "Lindungi Isolator Dari Busur Api." },
    { "en": "Apa Itu Korona (Corona Discharge)?", "id": "Ionisasi Udara Sekitar Konduktor." },
    { "en": "Apa Warna Cahaya Korona?", "id": "Ungu Kebiruan (Violet)." },
    { "en": "Apa Gas Yang Dihasilkan Korona?", "id": "Gas Ozon (O3)." },
    { "en": "Apa Bunyi Khas Korona?", "id": "Suara Mendesis (Hissing Sound)." },
    { "en": "Apa Itu Sphere Gap?", "id": "Alat Ukur Tegangan Tinggi Puncak." },
    { "en": "Apa Itu Rod Gap?", "id": "Sela Batang Proteksi Sederhana." },
    { "en": "Apa Itu Impulse Voltage Generator?", "id": "Pembangkit Tegangan Kejut Petir Buatan." },
    { "en": "Apa Bentuk Gelombang Petir Standar?", "id": "1.2 Garis Miring 50 Mikrodetik." },
    { "en": "Apa Bentuk Gelombang Switching Standar?", "id": "250 Garis Miring 2500 Mikrodetik." },
    { "en": "Apa Itu BIL Trafo?", "id": "Basic Insulation Level Tegangan Petir." },
    { "en": "Apa Itu SIL Trafo?", "id": "Switching Impulse Level Tegangan Switching." },
    { "en": "Apa Itu Paschen's Law?", "id": "Hubungan Tegangan Tembus Dan Tekanan." },
    { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Arus Mengalir Di Permukaan Konduktor." },
    { "en": "Apa Itu Efek Kedekatan (Proximity)?", "id": "Distribusi Arus Akibat Konduktor Lain." },
    { "en": "Apa Itu Ferranti Effect?", "id": "Tegangan Ujung Terima Lebih Tinggi." },
    { "en": "Kapan Terjadi Ferranti Effect?", "id": "Beban Ringan Saluran Panjang." },
    { "en": "Apa Itu Characteristic Impedance (Z0)?", "id": "Impedansi Saluran Tanpa Pantulan." },
    { "en": "Apa Itu Surge Impedance Loading (SIL)?", "id": "Daya Natural Saluran Transmisi." },
    { "en": "Apa Itu Propagation Constant?", "id": "Ukuran Perubahan Amplitudo Dan Fasa." },
    { "en": "Apa Itu Attenuation Constant?", "id": "Ukuran Penurunan Sinyal Per Jarak." },
    { "en": "Apa Itu Phase Constant?", "id": "Ukuran Pergeseran Fasa Per Jarak." },
    { "en": "Apa Itu Transposisi Kabel?", "id": "Tukar Posisi Fasa Seimbangkan Impedansi." },
    { "en": "Apa Itu Bundle Conductor?", "id": "Beberapa Kabel Per Fasa." },
    { "en": "Apa Manfaat Bundle Conductor?", "id": "Kurangi Korona Dan Reaktansi Induktif." },
    { "en": "Apa Itu GPR (Ground Potential Rise)?", "id": "Kenaikan Tegangan Tanah Saat Gangguan." },
    { "en": "Apa Itu Tegangan Langkah (Step Voltage)?", "id": "Beda Potensial Antara Dua Kaki." },
    { "en": "Apa Itu Tegangan Sentuh (Touch Voltage)?", "id": "Beda Potensial Tangan Dan Kaki." },
    { "en": "Apa Itu Tegangan Mesh (Mesh Voltage)?", "id": "Tegangan Maksimum Dalam Grid Grounding." },
    { "en": "Apa Itu Grounding Grid?", "id": "Jaring Konduktor Bawah Tanah Gardu." },
    { "en": "Apa Fungsi Batu Koral Di Gardu?", "id": "Tingkatkan Resistansi Permukaan Tanah." },
    { "en": "Apa Itu Exothermic Welding?", "id": "Las Kimia Sambung Tembaga Permanen." },
    { "en": "Apa Itu Soil Resistivity (Rho)?", "id": "Tahanan Jenis Tanah Area Tertentu." },
    { "en": "Apa Metode Wenner 4 Titik?", "id": "Cara Ukur Tahanan Jenis Tanah." },
    { "en": "Apa Itu Counterpoise Grounding?", "id": "Kawat Tanah Paralel Transmisi." },
    { "en": "Apa Fungsi Kawat Tanah (GW)?", "id": "Lindungi Fasa Dari Sambaran Petir." },
    { "en": "Apa Sudut Lindung (Shielding Angle)?", "id": "Sudut Proteksi Kawat Tanah." },
    { "en": "Apa Itu Back Flashover?", "id": "Loncatan Api Dari Menara Ke Fasa." },
    { "en": "Apa Penyebab Back Flashover?", "id": "Resistansi Kaki Menara Tinggi." },
    { "en": "Apa Itu OPGW (Optical Ground Wire)?", "id": "Kawat Tanah Berisi Fiber Optik." },
    { "en": "Apa Itu Stockbridge Damper?", "id": "Peredam Getaran Kabel Transmisi." },
    { "en": "Apa Itu Spacer Damper?", "id": "Pemisah Bundle Dan Peredam Getar." },
    { "en": "Apa Itu Galloping Kabel?", "id": "Osilasi Amplitudo Besar Frekuensi Rendah." },
    { "en": "Apa Itu Aeolian Vibration?", "id": "Getaran Frekuensi Tinggi Amplitudo Kecil." },
    { "en": "Apa Itu Sagging Kabel?", "id": "Lengkungan Kabel Akibat Berat Sendiri." },
    { "en": "Apa Pengaruh Suhu Terhadap Sag?", "id": "Suhu Naik Sag Bertambah." },
    { "en": "Apa Itu ACSR Conductor?", "id": "Aluminium Conductor Steel Reinforced." },
    { "en": "Apa Itu ACCC Conductor?", "id": "Aluminium Conductor Composite Core." },
    { "en": "Apa Kelebihan Kabel ACCC?", "id": "Lebih Ringan Dan Tahan Panas." },
    { "en": "Apa Itu GIS (Gas Insulated Switchgear)?", "id": "Switchgear Terisolasi Gas SF6." },
    { "en": "Apa Tekanan Gas SF6 Di GIS?", "id": "Sekitar 3 Hingga 6 Bar." },
    { "en": "Apa Warna Tabung Gas SF6?", "id": "Kuning Bagian Atas." },
    { "en": "Apa Bahaya Kebocoran SF6?", "id": "Gas Rumah Kaca Sangat Poten." },
    { "en": "Apa Itu Partial Discharge (PD)?", "id": "Peluahan Listrik Lokal Dalam Isolasi." },
    { "en": "Apa Alat Deteksi PD?", "id": "Sensor UHF Atau Akustik." },
    { "en": "Apa Satuan Partial Discharge?", "id": "Pico Coulomb (pC)." },
    { "en": "Apa Itu Bushing OIP?", "id": "Oil Impregnated Paper Bushing." },
    { "en": "Apa Itu Bushing RIP?", "id": "Resin Impregnated Paper Bushing." },
    { "en": "Apa Fungsi Kapasitif Tap Bushing?", "id": "Titik Ukur Tegangan Dan Tan Delta." },
    { "en": "Apa Itu Hot Spot Temperature Trafo?", "id": "Suhu Terpanas Belitan Trafo." },
    { "en": "Apa Itu Thermal Aging?", "id": "Penuaan Isolasi Akibat Panas." },
    { "en": "Apa Itu Degree Of Polymerization (DP)?", "id": "Ukuran Kualitas Kertas Isolasi." },
    { "en": "Apa Itu Furan Compound?", "id": "Produk Urai Kertas Dalam Minyak." },
    { "en": "Apa Fungsi Analisis Furan?", "id": "Estimasi Sisa Umur Trafo." },
    { "en": "Apa Itu Corrosive Sulfur Minyak?", "id": "Zat Korosif Perusak Tembaga." },
    { "en": "Apa Itu Interfacial Tension Minyak?", "id": "Indikator Adanya Lumpur (Sludge)." },
    { "en": "Apa Itu Acid Number Minyak?", "id": "Tingkat Keasaman Minyak Trafo." },
    { "en": "Apa Itu Moisture Content (PPM)?", "id": "Kadar Air Dalam Minyak." },
    { "en": "Apa Efek Air Dalam Minyak Trafo?", "id": "Turunkan Tegangan Tembus Drastis." },
    { "en": "Apa Itu Breakdown Voltage Test?", "id": "Uji Kekuatan Dielektrik Minyak." },
    { "en": "Apa Itu SFRA Test Trafo?", "id": "Sweep Frequency Response Analysis." },
    { "en": "Apa Tujuan Tes SFRA?", "id": "Deteksi Pergeseran Mekanis Belitan." },
    { "en": "Apa Itu TTR Test?", "id": "Transformer Turn Ratio Test." },
    { "en": "Apa Itu Excitation Current Test?", "id": "Deteksi Masalah Inti Besi." },
    { "en": "Apa Itu Winding Resistance Test?", "id": "Cek Koneksi Kendor Atau Putus." },
    { "en": "Apa Itu Dynamic Resistance Measurement?", "id": "Cek Kondisi Kontak Tap Changer." },
    { "en": "Apa Itu Peukert's Law Baterai?", "id": "Kapasitas Turun Jika Arus Naik." },
    { "en": "Apa Itu Self-Discharge Rate?", "id": "Kecepatan Baterai Kosong Sendiri." },
    { "en": "Berapa Self-Discharge Baterai NiMH?", "id": "Sekitar 30 Persen Per Bulan." },
    { "en": "Berapa Self-Discharge Baterai Li-ion?", "id": "Sekitar 2 Hingga 3 Persen." },
    { "en": "Apa Itu Battery Sulfation?", "id": "Kristal Timbal Sulfat Menutup Pelat." },
    { "en": "Apa Itu Battery Stratification?", "id": "Asam Mengendap Di Bawah Sel." },
    { "en": "Apa Fungsi Equalizing Charge?", "id": "Aduk Elektrolit Cegah Stratifikasi." },
    { "en": "Apa Itu Thermal Runaway Baterai?", "id": "Panas Hasilkan Panas Lebih Besar." },
    { "en": "Apa Itu Floating Voltage?", "id": "Tegangan Jaga Baterai Penuh." },
    { "en": "Apa Itu Cut-Off Voltage?", "id": "Batas Tegangan Bawah Aman." },
    { "en": "Apa Itu AH (Ampere Hour)?", "id": "Satuan Kapasitas Muatan Baterai." },
    { "en": "Apa Itu WH (Watt Hour)?", "id": "Satuan Total Energi Baterai." },
    { "en": "Apa Itu Energy Density?", "id": "Energi Per Satuan Berat/Volume." },
    { "en": "Apa Itu Power Density?", "id": "Daya Keluaran Per Satuan Berat." },
    { "en": "Apa Itu Baterai Primer?", "id": "Baterai Sekali Pakai Buang." },
    { "en": "Apa Itu Baterai Sekunder?", "id": "Baterai Bisa Diisi Ulang." },
    { "en": "Apa Anoda Baterai Li-ion?", "id": "Grafit (Karbon)." },
    { "en": "Apa Katoda Baterai Li-ion?", "id": "Oksida Logam Lithium." },
    { "en": "Apa Elektrolit Baterai Li-ion?", "id": "Garam Lithium Dalam Pelarut Organik." },
    { "en": "Apa Fungsi Separator Baterai?", "id": "Pemisah Anoda Katoda Cegah Short." },
    { "en": "Apa Itu THDi (Current Harmonic)?", "id": "Total Distorsi Harmonisa Arus." },
    { "en": "Apa Itu THDv (Voltage Harmonic)?", "id": "Total Distorsi Harmonisa Tegangan." },
    { "en": "Apa Efek Harmonisa Ke-3?", "id": "Arus Netral Menjadi Panas." },
    { "en": "Apa Efek Harmonisa Ke-5?", "id": "Torsi Lawan Pada Motor." },
    { "en": "Apa Itu K-Factor Trafo?", "id": "Rating Penanganan Beban Non-linear." },
    { "en": "Apa Itu Crest Factor?", "id": "Rasio Puncak Terhadap RMS." },
    { "en": "Apa Nilai Crest Factor Sinus?", "id": "1.414 (Akar Dua)." },
    { "en": "Apa Itu Power Factor Displacement?", "id": "Faktor Daya Akibat Beda Fasa." },
    { "en": "Apa Itu Power Factor Distortion?", "id": "Faktor Daya Akibat Harmonisa." },
    { "en": "Apa Itu True Power Factor?", "id": "Gabungan Displacement Dan Distortion." },
    { "en": "Apa Itu Flicker Tegangan?", "id": "Kedipan Cahaya Akibat Fluktuasi." },
    { "en": "Apa Itu Voltage Sag?", "id": "Tegangan Turun Sesaat." },
    { "en": "Apa Itu Voltage Swell?", "id": "Tegangan Naik Sesaat." },
    { "en": "Apa Itu Transient Impulse?", "id": "Lonjakan Tegangan Sangat Singkat." },
    { "en": "Apa Itu Notch Tegangan?", "id": "Cacat Tukik Pada Gelombang." },
    { "en": "Apa Penyebab Notch Tegangan?", "id": "Komutasi SCR Pada Konverter." },
    { "en": "Apa Fungsi Line Reactor?", "id": "Redam Harmonisa Dan Transient." },
    { "en": "Apa Fungsi Active Harmonic Filter?", "id": "Kompensasi Arus Harmonisa Dinamis." },
    { "en": "Apa Fungsi Kapasitor Detuned?", "id": "Perbaiki PF Tanpa Resonansi Harmonisa." },
    { "en": "Apa Itu Skin Effect Konduktor?", "id": "Arus Mengalir Di Kulit Kabel." },
    { "en": "Apa Itu Proximity Effect Kabel?", "id": "Distribusi Arus Dipengaruhi Kabel Lain." },
    { "en": "Apa Itu Ampacity Derating?", "id": "Penurunan Kapasitas Arus Kabel." },
    { "en": "Apa Faktor Derating Kabel?", "id": "Suhu Lingkungan Dan Jumlah Kabel." },
    { "en": "Apa Bahan Isolasi XLPE?", "id": "Cross Linked Polyethylene." },
    { "en": "Apa Suhu Kerja Kabel XLPE?", "id": "Maksimal 90 Derajat Celcius." },
    { "en": "Apa Suhu Kerja Kabel PVC?", "id": "Maksimal 70 Derajat Celcius." },
    { "en": "Apa Itu Kabel NYA?", "id": "Kabel Tembaga Tunggal Isolasi PVC." },
    { "en": "Apa Itu Kabel NYM?", "id": "Kabel Multi Core Isolasi Putih." },
    { "en": "Apa Itu Kabel NYY?", "id": "Kabel Multi Core Isolasi Hitam." },
    { "en": "Apa Itu Kabel NYFGBY?", "id": "Kabel Tanah Perisai Baja Pipih." },
    { "en": "Apa Itu Kabel N2XSY?", "id": "Kabel Tegangan Menengah Isolasi XLPE." },
    { "en": "Apa Itu Kabel Twisted Pair?", "id": "Kabel Pasangan Berpilin (Data)." },
    { "en": "Apa Itu Kabel Coaxial?", "id": "Kabel Inti Tunggal Shielding Anyam." },
    { "en": "Apa Itu Kabel Triaxial?", "id": "Coaxial Dengan Dua Lapis Shield." },
    { "en": "Apa Itu Kabel Twinaxial?", "id": "Dua Inti Dalam Satu Shield." },
    { "en": "Apa Fungsi Screen Kabel?", "id": "Lindungi Sinyal Dari Interferensi." },
    { "en": "Apa Itu Drain Wire Kabel?", "id": "Kawat Grounding Pada Kabel Shield." },
    { "en": "Apa Itu Relay Buchholz?", "id": "Proteksi Gas Internal Trafo." },
    { "en": "Apa Itu Relay Jansen?", "id": "Proteksi Tap Changer Trafo." },
    { "en": "Apa Itu Thermal Image Relay?", "id": "Simulasi Suhu Hotspot Trafo." },
    { "en": "Apa Itu Restricted Earth Fault?", "id": "Proteksi Gangguan Tanah Terbatas." },
    { "en": "Apa Itu Differential Relay?", "id": "Bandingkan Arus Masuk Keluar." },
    { "en": "Apa Itu Distance Relay?", "id": "Proteksi Berdasar Impedansi Saluran." },
    { "en": "Apa Itu Overcurrent Relay (OCR)?", "id": "Proteksi Arus Lebih Fasa." },
    { "en": "Apa Itu Ground Fault Relay (GFR)?", "id": "Proteksi Arus Lebih Tanah." },
    { "en": "Apa Itu Undervoltage Relay (UVR)?", "id": "Proteksi Tegangan Turun." },
    { "en": "Apa Itu Overvoltage Relay (OVR)?", "id": "Proteksi Tegangan Naik." },
    { "en": "Apa Itu Frequency Relay?", "id": "Proteksi Frekuensi Tidak Normal." },
    { "en": "Apa Itu Reverse Power Relay?", "id": "Proteksi Daya Balik Generator." },
    { "en": "Apa Itu Synchrocheck Relay?", "id": "Cek Syarat Sinkronisasi Paralel." },
    { "en": "Apa Itu Lockout Relay (86)?", "id": "Kunci Sistem Setelah Trip." },
    { "en": "Apa Itu Reclosing Relay (79)?", "id": "Tutup Balik Breaker Otomatis." },
    { "en": "Apa Itu CT Saturation?", "id": "Inti CT Jenuh Arus Cacat." },
    { "en": "Apa Itu Knee Point CT?", "id": "Titik Batas Kejenuhan CT." },
    { "en": "Apa Itu Burden CT?", "id": "Beban Rangkaian Sekunder CT." },
    { "en": "Apa Itu CT Kelas 0.2S?", "id": "CT Metering Sangat Presisi." },
    { "en": "Apa Itu CT Kelas 5P20?", "id": "CT Proteksi Error 5%." },
    { "en": "Apa Arti 5P20?", "id": "Akurat Hingga 20 Kali Nominal." },
    { "en": "Apa Itu PT (Potential Transformer)?", "id": "Trafo Tegangan Untuk Metering." },
    { "en": "Apa Itu CVT (Capacitive VT)?", "id": "Trafo Tegangan Kapasitor Transmisi." },
    { "en": "Apa Fungsi Wave Trap?", "id": "Blokir Sinyal Komunikasi PLC." },
    { "en": "Apa Itu PLC (Power Line Carrier)?", "id": "Komunikasi Lewat Kabel Listrik." },
    { "en": "Apa Itu Scada RTU?", "id": "Remote Terminal Unit Lapangan." },
    { "en": "Apa Itu Scada Master Station?", "id": "Pusat Kontrol Dan Monitoring." },
    { "en": "Apa Protokol IEC 61850?", "id": "Standar Komunikasi Gardu Induk." },
    { "en": "Apa Itu GOOSE Message?", "id": "Pesan Cepat Antar Relay." },
    { "en": "Apa Itu DNP3 Protocol?", "id": "Protokol Telemetri Utilitas Listrik." },
    { "en": "Apa Itu Modbus Protocol?", "id": "Protokol Serial Master Slave." },
    { "en": "Apa Itu Motor Induksi 3 Fasa?", "id": "Motor AC Paling Umum Industri." },
    { "en": "Apa Itu Slip Motor?", "id": "Selisih Kecepatan Sinkron Rotor." },
    { "en": "Apa Itu Torque Speed Curve?", "id": "Grafik Hubungan Torsi Kecepatan." },
    { "en": "Apa Itu Breakdown Torque?", "id": "Torsi Maksimum Sebelum Stall." },
    { "en": "Apa Itu Locked Rotor Torque?", "id": "Torsi Saat Start Awal." },
    { "en": "Apa Itu Pull-up Torque?", "id": "Torsi Minimum Saat Akselerasi." },
    { "en": "Apa Itu Motor Efficiency Class?", "id": "Standar Efisiensi Energi Motor." },
    { "en": "Apa Itu IE1 Motor?", "id": "Efisiensi Standar." },
    { "en": "Apa Itu IE2 Motor?", "id": "Efisiensi Tinggi." },
    { "en": "Apa Itu IE3 Motor?", "id": "Efisiensi Premium." },
    { "en": "Apa Itu IE4 Motor?", "id": "Efisiensi Super Premium." },
    { "en": "Apa Itu Control Valve Air To Open?", "id": "Membuka Saat Diberi Tekanan Udara." },
    { "en": "Apa Itu Control Valve Air To Close?", "id": "Menutup Saat Diberi Tekanan Udara." },
    { "en": "Apa Itu Fail Safe Open Valve?", "id": "Terbuka Penuh Saat Tenaga Hilang." },
    { "en": "Apa Itu Fail Safe Close Valve?", "id": "Tertutup Penuh Saat Tenaga Hilang." },
    { "en": "Apa Karakteristik Valve Linear?", "id": "Aliran Sebanding Lurus Bukaan Katup." },
    { "en": "Apa Karakteristik Valve Equal Percentage?", "id": "Perubahan Aliran Konstan Persen Bukaan." },
    { "en": "Apa Karakteristik Valve Quick Opening?", "id": "Buka Cepat Di Awal Gerakan." },
    { "en": "Apa Fungsi Valve Positioner?", "id": "Posisikan Katup Sesuai Sinyal Kontrol." },
    { "en": "Apa Itu I/P Converter?", "id": "Ubah Arus Listrik Jadi Tekanan." },
    { "en": "Apa Sinyal Input I/P Converter?", "id": "4 Hingga 20 Miliampere." },
    { "en": "Apa Sinyal Output I/P Converter?", "id": "3 Hingga 15 PSI." },
    { "en": "Apa Itu Sensor 2-Wire?", "id": "Daya Dan Sinyal Satu Kabel." },
    { "en": "Apa Itu Sensor 3-Wire?", "id": "Daya Dan Sinyal Kabel Terpisah." },
    { "en": "Apa Itu Sensor 4-Wire?", "id": "Sumber Daya Terpisah Isolasi Penuh." },
    { "en": "Apa Itu Enclosure Motor ODP?", "id": "Open Drip Proof (Terbuka)." },
    { "en": "Apa Itu Enclosure Motor TEFC?", "id": "Totally Enclosed Fan Cooled (Tertutup)." },
    { "en": "Apa Itu Enclosure Motor TENV?", "id": "Totally Enclosed Non Ventilated." },
    { "en": "Apa Itu Enclosure Motor TEAO?", "id": "Totally Enclosed Air Over." },
    { "en": "Apa Itu Enclosure Motor Explosion Proof?", "id": "Tahan Ledakan Gas Di Dalam." },
    { "en": "Apa Fungsi Frame Motor?", "id": "Dudukan Stator Dan Pelindung Mekanis." },
    { "en": "Apa Fungsi Terminal Box Motor?", "id": "Tempat Sambungan Kabel Power Motor." },
    { "en": "Apa Itu Kapasitor Keramik Class 1?", "id": "Sangat Stabil Suhu (Contoh NP0/C0G)." },
    { "en": "Apa Itu Kapasitor Keramik Class 2?", "id": "Kapasitas Besar Tapi Kurang Stabil." },
    { "en": "Apa Kode Dielektrik X7R?", "id": "Stabil Suhu Menengah Toleransi Sedang." },
    { "en": "Apa Kode Dielektrik Y5V?", "id": "Kapasitas Besar Tapi Tidak Stabil." },
    { "en": "Apa Itu Kapasitor Film Polypropylene?", "id": "Rugi Rendah Untuk Aplikasi Pulse." },
    { "en": "Apa Itu Kapasitor Film Polyester (Mylar)?", "id": "Ukuran Kecil Untuk Aplikasi Umum." },
    { "en": "Apa Itu Kapasitor Silver Mica?", "id": "Sangat Presisi Untuk Frekuensi Tinggi." },
    { "en": "Apa Itu Supercap Back-up?", "id": "Pengganti Baterai Memori Sementara." },
    { "en": "Apa Itu Konektor M12 Sensor?", "id": "Konektor Bulat 12mm Tahan Air." },
    { "en": "Apa Itu Konektor M8 Sensor?", "id": "Konektor Bulat 8mm Sensor Kecil." },
    { "en": "Apa Itu Konektor DIN 43650?", "id": "Konektor Kotak Solenoid Valve." },
    { "en": "Apa Itu Konektor Harting Heavy Duty?", "id": "Konektor Industri Multipin Kotak Besar." },
    { "en": "Apa Itu Terminal Block Spring Clamp?", "id": "Koneksi Kabel Jepit Per Cepat." },
    { "en": "Apa Itu Terminal Block Barrier Strip?", "id": "Terminal Baut Dengan Sekat Isolator." },
    { "en": "Apa Fungsi Cable Lug Skun?", "id": "Ujung Kabel Agar Mudah Dibaut." },
    { "en": "Apa Itu Ferrule Bootlace?", "id": "Selongsong Ujung Kabel Serabut." },
    { "en": "Apa Itu Busbar Isolator (Support)?", "id": "Dudukan Rel Tembaga Di Panel." },
    { "en": "Apa Itu Phase Busbar MCB?", "id": "Rel Sisir Jamper MCB Rapi." },
    { "en": "Apa Itu Grounding Bar?", "id": "Batang Tembaga Kumpul Kabel Tanah." },
    { "en": "Apa Itu Neutral Link?", "id": "Terminal Sambungan Netral Di Panel." },
    { "en": "Apa Fungsi Panel Heater?", "id": "Cegah Kondensasi Embun Dalam Panel." },
    { "en": "Apa Fungsi Panel Thermostat?", "id": "Atur Suhu Kipas Atau Heater." },
    { "en": "Apa Fungsi Hygrostat Panel?", "id": "Atur Kelembapan Udara Dalam Panel." },
    { "en": "Apa Itu Filter Fan Panel?", "id": "Kipas Dengan Busa Penyaring Debu." },
    { "en": "Apa Arah Angin Kipas Panel?", "id": "Tiup Keluar (Exhaust) Atau Masuk." },
    { "en": "Apa Tekanan Positif Dalam Panel?", "id": "Cegah Debu Masuk Celah Panel." },
    { "en": "Apa Itu Push Button Momentary?", "id": "Kembali Semula Saat Dilepas." },
    { "en": "Apa Itu Push Button Latching?", "id": "Mengunci Posisi Saat Ditekan." },
    { "en": "Apa Itu Selector Switch 2 Posisi?", "id": "Pilih Antara Dua Mode Operasi." },
    { "en": "Apa Itu Selector Switch 3 Posisi?", "id": "Manual, Off, Auto (MOA)." },
    { "en": "Apa Fungsi Lampu Tower (Tower Lamp)?", "id": "Indikator Status Mesin Bertingkat." },
    { "en": "Apa Arti Lampu Tower Merah?", "id": "Mesin Berhenti Atau Error." },
    { "en": "Apa Arti Lampu Tower Kuning?", "id": "Peringatan Atau Standby." },
    { "en": "Apa Arti Lampu Tower Hijau?", "id": "Mesin Beroperasi Normal." },
    { "en": "Apa Itu Buzzer Panel?", "id": "Alarm Suara Gangguan Mesin." },
    { "en": "Apa Fungsi Foot Switch?", "id": "Saklar Injak Kaki Operator." },
    { "en": "Apa Itu Knee Switch?", "id": "Saklar Tekan Lutut (Medis)." },
    { "en": "Apa Itu Pull Cord Switch?", "id": "Saklar Tali Darurat Conveyor." },
    { "en": "Apa Fungsi Safety Curtain Sensor?", "id": "Tirai Cahaya Pengaman Tangan Operator." },
    { "en": "Apa Fungsi Safety Mat Switch?", "id": "Karpet Deteksi Orang Berdiri." },
    { "en": "Apa Itu Magnetic Door Switch?", "id": "Sensor Pintu Panel Terbuka." },
    { "en": "Apa Itu Relay Safety?", "id": "Relay Ganda Pengaman Rangkaian K3." },
    { "en": "Apa Itu Zero Speed Switch?", "id": "Deteksi Mesin Berhenti Berputar." },
    { "en": "Apa Fungsi Encoder Wheel?", "id": "Roda Ukur Panjang Material Berjalan." },
    { "en": "Apa Itu Kabel Resolver?", "id": "Kabel Feedback Posisi Motor Servo." },
    { "en": "Apa Itu Motor Brake 24V?", "id": "Rem Elektromagnetik Penahan Poros." },
    { "en": "Kapan Motor Brake Aktif?", "id": "Saat Listrik Brake Hilang (Fail-safe)." },
    { "en": "Apa Itu Resistor Braking Dinamis?", "id": "Buang Energi Putaran Jadi Panas." },
    { "en": "Apa Itu Kabel Servo Power?", "id": "Suplai Daya Utama Motor Servo." },
    { "en": "Apa Itu Kabel Servo Encoder?", "id": "Sinyal Balik Posisi Motor Servo." },
    { "en": "Apa Fungsi Noise Filter Servo?", "id": "Cegah Gangguan Frekuensi Inverter." },
    { "en": "Apa Itu CN1 Connector Servo?", "id": "Port I/O Kontrol Driver Servo." },
    { "en": "Apa Itu Pulse Direction Control?", "id": "Kontrol Gerak Dengan Pulsa Arah." },
    { "en": "Apa Itu Analog Speed Control?", "id": "Kontrol Kecepatan Dengan Tegangan 0-10V." },
    { "en": "Apa Itu Torque Control Mode?", "id": "Atur Kekuatan Putar Motor." },
    { "en": "Apa Itu Electronic Gearing?", "id": "Rasio Pulsa Input Dan Gerakan." },
    { "en": "Apa Fungsi Homing Servo?", "id": "Cari Posisi Nol Awal Mesin." },
    { "en": "Apa Itu Hard Limit Switch?", "id": "Batasan Gerak Fisik Mesin." },
    { "en": "Apa Itu Soft Limit Switch?", "id": "Batasan Gerak Program Software." },
    { "en": "Apa Itu PLC Sourcing Output?", "id": "Keluarkan Tegangan Positif (PNP)." },
    { "en": "Apa Itu PLC Sinking Output?", "id": "Sambung Ke Ground (NPN)." },
    { "en": "Apa Fungsi Relay Output PLC?", "id": "Saklar Mekanis Beban AC/DC." },
    { "en": "Apa Fungsi Transistor Output PLC?", "id": "Saklar Cepat Beban DC Saja." },
    { "en": "Apa Kelebihan Output Transistor?", "id": "Kecepatan Tinggi Dan Tahan Lama." },
    { "en": "Apa Kelebihan Output Relay?", "id": "Bisa Beban Arus Lebih Besar." },
    { "en": "Apa Itu Module Expansion PLC?", "id": "Modul Tambahan I/O PLC." },
    { "en": "Apa Itu Remote I/O PLC?", "id": "Modul I/O Jarak Jauh." },
    { "en": "Apa Protokol Remote I/O Umum?", "id": "Modbus, Profibus, Atau EtherNet/IP." },
    { "en": "Apa Itu Distribusi I/O Terpusat?", "id": "Semua Kabel Masuk Satu Panel." },
    { "en": "Apa Itu Distribusi I/O Desentralisasi?", "id": "Modul I/O Disebar Dekat Sensor." },
    { "en": "Apa Keuntungan I/O Desentralisasi?", "id": "Hemat Kabel Kontrol Panjang." },
    { "en": "Apa Itu HMI Resistive Touch?", "id": "Layar Sentuh Tekan (Plastik)." },
    { "en": "Apa Itu HMI Capacitive Touch?", "id": "Layar Sentuh Jari (Kaca HP)." },
    { "en": "Apa Itu Industrial PC (IPC)?", "id": "Komputer Tahan Lingkungan Pabrik." },
    { "en": "Apa Beda IPC Dan PC Biasa?", "id": "IPC Tahan Debu, Panas, Getaran." },
    { "en": "Apa Itu Fanless IPC?", "id": "Komputer Pendingin Pasif Tanpa Kipas." },
    { "en": "Apa Itu Panel Mount PC?", "id": "Komputer Layar Tanam Di Pintu." },
    { "en": "Apa Itu Scada Tag Database?", "id": "Daftar Variabel Data Sistem." },
    { "en": "Apa Itu Integral Windup Pada PID?", "id": "Penumpukan Error Integral Saat Saturasi." },
    { "en": "Apa Itu PLC Scan Cycle?", "id": "Siklus Baca Input Proses Output." },
    { "en": "Apa Fungsi Watchdog Timer PLC?", "id": "Deteksi Kesalahan Hardware Atau Software." },
    { "en": "Apa Itu Jerk Dalam Motion Control?", "id": "Laju Perubahan Percepatan Per Waktu." },
    { "en": "Apa Itu Profil Gerak S-Curve?", "id": "Akselerasi Dan Deselerasi Yang Halus." },
    { "en": "Apa Itu Following Error Servo?", "id": "Selisih Posisi Perintah Dan Aktual." },
    { "en": "Apa Penyebab Resonansi Motor Stepper?", "id": "Getaran Pada Frekuensi Alami Motor." },
    { "en": "Apa Manfaat Microstepping Motor Stepper?", "id": "Gerakan Lebih Halus Resolusi Tinggi." },
    { "en": "Apa Fungsi Resistor Terminasi Fieldbus?", "id": "Cegah Pantulan Sinyal Di Ujung." },
    { "en": "Apa Fungsi Modbus Function 03?", "id": "Membaca Data Holding Register." },
    { "en": "Apa Fungsi Modbus Function 06?", "id": "Menulis Data Ke Satu Register." },
    { "en": "Apa Arti Modbus Exception 01?", "id": "Fungsi Ilegal Tidak Didukung Device." },
    { "en": "Bagaimana Grounding Shielding Profibus?", "id": "Dihubungkan Ke Ground Kedua Ujung." },
    { "en": "Berapa Impedansi Kabel Ethernet Standar?", "id": "100 Ohm." },
    { "en": "Berapa Level Tegangan Sinyal RS485?", "id": "Beda Potensial 1.5 Hingga 5 Volt." },
    { "en": "Berapa Level Tegangan Sinyal RS232?", "id": "Plus Minus 3 Hingga 15 Volt." },
    { "en": "Apa Itu Burden Resistance 4-20mA?", "id": "Hambatan Total Maksimal Loop Arus." },
    { "en": "Berapa Frekuensi Logika 1 HART?", "id": "1200 Hertz." },
    { "en": "Berapa Frekuensi Logika 0 HART?", "id": "2200 Hertz." },
    { "en": "Berapa Kecepatan Foundation Fieldbus H1?", "id": "31.25 Kilobit Per Detik." },
    { "en": "Apa Itu Holding Current Thyristor?", "id": "Arus Minimal Agar Tetap On." },
    { "en": "Apa Itu Latching Current Thyristor?", "id": "Arus Minimal Untuk Memicu On." },
    { "en": "Apa Itu Tail Current IGBT?", "id": "Arus Sisa Saat Mematikan IGBT." },
    { "en": "Apa Itu Miller Capacitance MOSFET?", "id": "Kapasitansi Parasitik Antara Gate Drain." },
    { "en": "Apa Itu Reverse Recovery Time?", "id": "Waktu Dioda Berhenti Alirkan Balik." },
    { "en": "Apa Efek ESR Tinggi Kapasitor?", "id": "Panas Berlebih Dan Ripple Tegangan." },
    { "en": "Apa Itu Saturation Current Induktor?", "id": "Arus Saat Nilai Induktansi Drop." },
    { "en": "Apa Penyebab Inrush Current Trafo?", "id": "Sisa Magnetisme Dan Sudut Fasa." },
    { "en": "Apa Itu CT Saturation Factor?", "id": "Rasio Arus Jenuh Dan Nominal." },
    { "en": "Apa Penyebab Ferroresonance Trafo Tegangan?", "id": "Osilasi Induktansi Trafo Kapasitansi Kabel." },
    { "en": "Apa Itu Traceability Kalibrasi?", "id": "Rantai Ukur Ke Standar Internasional." },
    { "en": "Apa Itu Uncertainty (Ketidakpastian) Pengukuran?", "id": "Rentang Kemungkinan Nilai Sebenarnya." },
    { "en": "Apa Itu TUR (Test Uncertainty Ratio)?", "id": "Rasio Akurasi Alat Dan Standar." },
    { "en": "Apa Itu Interval Kalibrasi?", "id": "Jarak Waktu Antar Kalibrasi Rutin." },
    { "en": "Apa Itu Data As Found?", "id": "Hasil Ukur Sebelum Alat Disetel." },
    { "en": "Apa Itu Data As Left?", "id": "Hasil Ukur Setelah Alat Disetel." },
    { "en": "Apa Itu Span Adjustment?", "id": "Kalibrasi Nilai Atas Rentang Ukur." },
    { "en": "Apa Itu Zero Adjustment?", "id": "Kalibrasi Nilai Bawah Rentang Ukur." },
    { "en": "Apa Fungsi Linearization Sensor?", "id": "Koreksi Output Sensor Tidak Linear." },
    { "en": "Apa Itu Hysteresis Error Sensor?", "id": "Selisih Bacaan Saat Naik Turun." },
    { "en": "Apa Itu Deadband Instrumentasi?", "id": "Rentang Input Tanpa Perubahan Output." },
    { "en": "Apa Itu Resolusi Alat Ukur?", "id": "Perubahan Terkecil Yang Bisa Dibaca." },
    { "en": "Apa Itu Sensitivitas Sensor?", "id": "Rasio Perubahan Output Terhadap Input." },
    { "en": "Apa Beda Presisi Dan Akurasi?", "id": "Presisi Konsisten, Akurasi Tepat Sasaran." },
    { "en": "Apa Itu Standard Deviation Data?", "id": "Ukuran Sebaran Data Dari Rata-rata." },
    { "en": "Apa Itu Nilai RMS (Root Mean Square)?", "id": "Nilai Efektif Setara Panas DC." },
    { "en": "Apa Fungsi True RMS Meter?", "id": "Ukur Akurat Gelombang Non Sinus." },
    { "en": "Apa Rumus Crest Factor?", "id": "Nilai Puncak Dibagi Nilai RMS." },
    { "en": "Apa Rumus Form Factor?", "id": "Nilai RMS Dibagi Nilai Rata-rata." },
    { "en": "Apa Itu Harmonisa Listrik?", "id": "Gelombang Kelipatan Bulat Frekuensi Dasar." },
    { "en": "Apa Itu Interharmonics?", "id": "Gelombang Kelipatan Pecahan Frekuensi Dasar." },
    { "en": "Apa Itu THD (Total Harmonic Distortion)?", "id": "Total Cacat Gelombang Akibat Harmonisa." },
    { "en": "Apa Itu Leading Power Factor?", "id": "Arus Mendahului Tegangan (Beban Kapasitif)." },
    { "en": "Apa Itu Lagging Power Factor?", "id": "Arus Tertinggal Tegangan (Beban Induktif)." },
    { "en": "Apa Itu Displacement Power Factor?", "id": "Faktor Daya Gelombang Fundamental Saja." },
    { "en": "Apa Itu Distortion Power Factor?", "id": "Penurunan Faktor Daya Akibat Harmonisa." },
    { "en": "Apa Itu K-Factor Trafo?", "id": "Kemampuan Trafo Tahan Panas Harmonisa." },
    { "en": "Apa Itu Skin Effect Kabel?", "id": "Arus AC Mengalir Di Kulit." },
    { "en": "Apa Itu Proximity Effect Kabel?", "id": "Arus Terdesak Akibat Kabel Sebelah." },
    { "en": "Apa Itu Corona Discharge?", "id": "Peluahan Listrik Di Udara Konduktor." },
    { "en": "Apa Itu Partial Discharge Isolasi?", "id": "Loncatan Listrik Lokal Dalam Rongga." },
    { "en": "Apa Itu Treeing Pada Isolasi?", "id": "Jalur Kerusakan Menyerupai Akar Pohon." },
    { "en": "Apa Itu Tracking Pada Isolasi?", "id": "Jalur Konduktif Di Permukaan Isolator." },
    { "en": "Apa Itu Dielectric Absorption?", "id": "Isolasi Simpan Muatan Setelah Discharge." },
    { "en": "Apa Itu Polarization Index (PI)?", "id": "Rasio Tahanan 10 Menit Semenit." },
    { "en": "Apa Itu DAR (Dielectric Absorption Ratio)?", "id": "Rasio Tahanan 60 Detik 30 Detik." },
    { "en": "Apa Itu Insulation Resistance (IR)?", "id": "Tahanan Bahan Isolator Terhadap Arus." },
    { "en": "Apa Itu Hipot Test?", "id": "Uji Ketahanan Tegangan Tinggi Isolasi." },
    { "en": "Apa Itu Ground Bond Test?", "id": "Uji Koneksi Grounding Arus Besar." },
    { "en": "Apa Itu Leakage Current Test?", "id": "Ukur Arus Bocor Lewat Isolasi." },
    { "en": "Apa Itu Earth Loop Impedance?", "id": "Impedansi Jalur Gangguan Ke Tanah." },
    { "en": "Apa Itu RCD Trip Time?", "id": "Waktu Pemutusan Saat Terjadi Bocor." },
    { "en": "Apa Itu RCD Trip Current?", "id": "Arus Bocor Minimal Untuk Trip." },
    { "en": "Apa Itu Prospective Short Circuit?", "id": "Arus Maksimal Saat Hubung Singkat." },
    { "en": "Apa Dasar Perhitungan Voltage Drop?", "id": "Arus, Panjang Kabel, Dan Impedansi." },
    { "en": "Apa Itu Cable Derating Factor?", "id": "Faktor Pengurang Kapasitas Arus Kabel." },
    { "en": "Apa Itu Conduit Fill Ratio?", "id": "Persentase Pipa Terisi Kabel." },
    { "en": "Apa Itu Cable Bend Radius?", "id": "Radius Tekukan Kabel Minimum Aman." },
    { "en": "Apa Arti Digit Pertama IP Rating?", "id": "Perlindungan Terhadap Benda Padat." },
    { "en": "Apa Arti Digit Kedua IP Rating?", "id": "Perlindungan Terhadap Benda Cair." },
    { "en": "Apa Itu NEMA Type 1?", "id": "Box Indoor Perlindungan Dasar." },
    { "en": "Apa Itu NEMA Type 3R?", "id": "Box Outdoor Tahan Hujan Es." },
    { "en": "Apa Itu NEMA Type 4X?", "id": "Box Tahan Air Dan Korosi." },
    { "en": "Apa Itu Hazardous Zone 0?", "id": "Gas Meledak Selalu Ada Terus." },
    { "en": "Apa Itu Hazardous Zone 1?", "id": "Gas Meledak Mungkin Ada Normal." },
    { "en": "Apa Itu Hazardous Zone 2?", "id": "Gas Meledak Jarang Sekali Ada." },
    { "en": "Apa Itu Intrinsic Safety (IS)?", "id": "Energi Dibatasi Bawah Titik Nyala." },
    { "en": "Apa Itu Explosion Proof (Ex d)?", "id": "Tahan Ledakan Di Dalam Enclosure." },
    { "en": "Apa Itu Purged Enclosure (Ex p)?", "id": "Tekanan Udara Positif Cegah Gas." },
    { "en": "Apa Kepanjangan SIL?", "id": "Safety Integrity Level." },
    { "en": "Apa Kepanjangan SIS?", "id": "Safety Instrumented System." },
    { "en": "Apa Kepanjangan SIF?", "id": "Safety Instrumented Function." },
    { "en": "Apa Kepanjangan MTBF?", "id": "Mean Time Between Failures." },
    { "en": "Apa Kepanjangan MTTR?", "id": "Mean Time To Repair." },
    { "en": "Apa Itu Failure Rate (Laju Kegagalan)?", "id": "Frekuensi Kerusakan Per Satuan Waktu." },
    { "en": "Apa Itu Bathtub Curve?", "id": "Kurva Pola Kegagalan Alat Elektronik." },
    { "en": "Apa Itu Burn-in Test?", "id": "Uji Nyala Awal Deteksi Cacat." },
    { "en": "Apa Itu Stress Testing?", "id": "Uji Alat Di Luar Batas." },
    { "en": "Apa Itu Environmental Testing?", "id": "Uji Alat Di Suhu Lembap." },
    { "en": "Apa Itu EMC Testing?", "id": "Uji Kompatibilitas Elektromagnetik Perangkat." },
    { "en": "Apa Itu ACB (Air Circuit Breaker)?", "id": "Pemutus Arus Utama Udara Tegangan Rendah." },
    { "en": "Apa Itu VCB (Vacuum Circuit Breaker)?", "id": "Pemutus Arus Ruang Hampa Tegangan Menengah." },
    { "en": "Apa Keunggulan VCB Dibanding OCB?", "id": "Bebas Perawatan Dan Tahan Lama." },
    { "en": "Apa Itu OCB (Oil Circuit Breaker)?", "id": "Pemutus Arus Media Minyak Isolasi." },
    { "en": "Apa Itu LBS (Load Break Switch)?", "id": "Saklar Pemutus Beban Tegangan Menengah." },
    { "en": "Apa Beda LBS Dan Disconnecting Switch?", "id": "LBS Bisa Memutus Arus Beban." },
    { "en": "Apa Itu Cubicle Tegangan Menengah?", "id": "Lemari Panel Distribusi 20kV." },
    { "en": "Apa Fungsi Heater Pada Cubicle?", "id": "Cegah Kelembapan Penyebab Flashover." },
    { "en": "Apa Itu Incoming Feeder?", "id": "Jalur Kabel Masuk Suplai Utama." },
    { "en": "Apa Itu Outgoing Feeder?", "id": "Jalur Kabel Keluar Ke Beban." },
    { "en": "Apa Itu Metering Cubicle?", "id": "Lemari Khusus Peralatan Ukur PLN." },
    { "en": "Apa Fungsi Relay Proteksi Sepam?", "id": "Relay Proteksi Digital Terintegrasi Schneider." },
    { "en": "Apa Fungsi Relay Proteksi Micom?", "id": "Relay Proteksi Digital Sistem Tenaga." },
    { "en": "Apa Itu Tripping Coil?", "id": "Kumparan Pemicu Mekanik Breaker Lepas." },
    { "en": "Apa Itu Closing Coil?", "id": "Kumparan Pemicu Mekanik Breaker Masuk." },
    { "en": "Apa Itu Spring Charging Motor?", "id": "Motor Pengisi Pegas Mekanik Breaker." },
    { "en": "Apa Indikator Pegas Charged?", "id": "Tulisan Charged Atau Warna Kuning." },
    { "en": "Apa Itu Rack-in Rack-out Breaker?", "id": "Mekanisme Masuk Keluar Breaker Draw-out." },
    { "en": "Apa Posisi Test Pada Breaker?", "id": "Sirkuit Kontrol Aktif Power Mati." },
    { "en": "Apa Posisi Service Pada Breaker?", "id": "Sirkuit Kontrol Dan Power Terhubung." },
    { "en": "Apa Itu Shutter Mekanis Cubicle?", "id": "Penutup Otomatis Lubang Kontak Tegangan." },
    { "en": "Apa Itu Capacitive Voltage Indicator?", "id": "Lampu Tanda Adanya Tegangan Tinggi." },
    { "en": "Apa Fungsi Earthing Switch Cubicle?", "id": "Grounding Kabel Saat Pemeliharaan." },
    { "en": "Apa Interlock Earthing Switch?", "id": "Tidak Bisa Masuk Jika Ada Tegangan." },
    { "en": "Apa Itu Kabel N2XSEBY?", "id": "Kabel 20kV Tembaga Isolasi XLPE." },
    { "en": "Apa Itu Kabel NA2XSEBY?", "id": "Kabel 20kV Aluminium Isolasi XLPE." },
    { "en": "Apa Fungsi Indoor Termination Kit?", "id": "Terminasi Kabel Tegangan Menengah Dalam." },
    { "en": "Apa Fungsi Outdoor Termination Kit?", "id": "Terminasi Kabel Tegangan Menengah Luar." },
    { "en": "Apa Fungsi Stress Control Tape?", "id": "Ratakan Medan Listrik Ujung Kabel." },
    { "en": "Apa Itu Skun Bimetal?", "id": "Konektor Kabel Aluminium Ke Tembaga." },
    { "en": "Apa Fungsi Jointing Kabel TM?", "id": "Sambungan Kabel Tegangan Menengah." },
    { "en": "Apa Itu Partial Discharge Scanner?", "id": "Alat Deteksi Kebocoran Isolasi Dini." },
    { "en": "Apa Itu Thermal Imager Panel?", "id": "Kamera Panas Deteksi Koneksi Kendor." },
    { "en": "Apa Itu Contact Resistance Test?", "id": "Ukur Tahanan Kontak Breaker Utama." },
    { "en": "Apa Itu Insulation Resistance Test 5kV?", "id": "Megger Kabel Tegangan Menengah." },
    { "en": "Apa Itu Hi-Pot Test (VLF)?", "id": "Uji Ketahanan Tegangan Frekuensi Rendah." },
    { "en": "Apa Itu Robot SCARA?", "id": "Robot Lengan Kaku Gerak Horizontal." },
    { "en": "Apa Itu Robot Delta?", "id": "Robot Paralel Kecepatan Tinggi Pick-place." },
    { "en": "Apa Itu Robot Cartesian (Gantry)?", "id": "Robot Gerak Linear Sumbu XYZ." },
    { "en": "Apa Itu Cobot (Collaborative Robot)?", "id": "Robot Aman Bekerja Samping Manusia." },
    { "en": "Apa Itu End Effector Gripper?", "id": "Pencapit Barang Ujung Lengan Robot." },
    { "en": "Apa Itu End Effector Vacuum?", "id": "Penyedot Barang Ujung Lengan Robot." },
    { "en": "Apa Itu Teach Pendant?", "id": "Remot Kontrol Pemrograman Manual Robot." },
    { "en": "Apa Itu Robot Axis 6-DOF?", "id": "Robot Enam Derajat Kebebasan Gerak." },
    { "en": "Apa Itu Singularity Pada Robot?", "id": "Posisi Dimana Robot Kehilangan Kebebasan." },
    { "en": "Apa Itu TCP (Tool Center Point)?", "id": "Titik Koordinat Ujung Alat Robot." },
    { "en": "Apa Itu Joint Jogging?", "id": "Gerakkan Robot Per Sendi Manual." },
    { "en": "Apa Itu World Coordinate System?", "id": "Sistem Koordinat Tetap Lantai Robot." },
    { "en": "Apa Itu Machine Vision System?", "id": "Kamera Cerdas Inspeksi Produk Otomatis." },
    { "en": "Apa Fungsi Lighting Backlight Vision?", "id": "Buat Siluet Untuk Ukur Dimensi." },
    { "en": "Apa Fungsi Lighting Ring Light?", "id": "Pencahayaan Rata Dari Depan Kamera." },
    { "en": "Apa Itu OCR Vision System?", "id": "Optical Character Recognition (Baca Teks)." },
    { "en": "Apa Itu Barcode Reader Industri?", "id": "Pemindai Kode Batang Kecepatan Tinggi." },
    { "en": "Apa Itu RFID Tag UHF?", "id": "Tag Radio Jarak Jauh (Meter)." },
    { "en": "Apa Itu Access Control Door?", "id": "Sistem Kunci Pintu Kartu/Sidik Jari." },
    { "en": "Apa Fungsi Electromagnetic Lock (Maglock)?", "id": "Kunci Pintu Kekuatan Magnet Kuat." },
    { "en": "Apa Fungsi Electric Drop Bolt?", "id": "Kunci Pintu Baut Turun Elektrik." },
    { "en": "Apa Fungsi Emergency Break Glass?", "id": "Pemutus Pintu Darurat Saat Kebakaran." },
    { "en": "Apa Fungsi Door Contact Sensor?", "id": "Deteksi Status Pintu Buka Tutup." },
    { "en": "Apa Itu Exit Button?", "id": "Tombol Keluar Ruangan Akses Kontrol." },
    { "en": "Apa Itu FACP Addressable?", "id": "Panel Fire Alarm Detektor Beralamat." },
    { "en": "Apa Itu FACP Conventional?", "id": "Panel Fire Alarm Sistem Zona." },
    { "en": "Apa Fungsi Smoke Detector Ionization?", "id": "Deteksi Asap Api Cepat Menyala." },
    { "en": "Apa Fungsi Smoke Detector Photoelectric?", "id": "Deteksi Asap Tebal Api Membara." },
    { "en": "Apa Fungsi Heat Detector ROR?", "id": "Deteksi Kenaikan Suhu Mendadak." },
    { "en": "Apa Fungsi Heat Detector Fixed?", "id": "Deteksi Suhu Titik Tertentu (57C)." },
    { "en": "Apa Fungsi Flame Detector?", "id": "Deteksi Gelombang UV/IR Api." },
    { "en": "Apa Fungsi Beam Detector?", "id": "Deteksi Asap Area Luas (Gudang)." },
    { "en": "Apa Itu EOL Resistor (End of Line)?", "id": "Resistor Ujung Loop Zona Alarm." },
    { "en": "Apa Fungsi Hydrant Box?", "id": "Kotak Selang Pemadam Kebakaran Gedung." },
    { "en": "Apa Fungsi Jockey Pump?", "id": "Pompa Jaga Tekanan Pipa Hydrant." },
    { "en": "Apa Fungsi Main Electric Pump?", "id": "Pompa Utama Hydrant Tenaga Listrik." },
    { "en": "Apa Fungsi Diesel Fire Pump?", "id": "Pompa Cadangan Tenaga Mesin Diesel." },
    { "en": "Apa Itu Sprinkler Head?", "id": "Kepala Penyemprot Air Otomatis Panas." },
    { "en": "Apa Warna Bulb Sprinkler 68C?", "id": "Merah." },
    { "en": "Apa Warna Bulb Sprinkler 57C?", "id": "Jingga." },
    { "en": "Apa Warna Bulb Sprinkler 79C?", "id": "Kuning." },
    { "en": "Apa Warna Bulb Sprinkler 93C?", "id": "Hijau." },
    { "en": "Apa Itu Flow Switch Sprinkler?", "id": "Deteksi Aliran Air Pipa Sprinkler." },
    { "en": "Apa Itu Pressure Switch Hydrant?", "id": "Start Pompa Saat Tekanan Turun." },
    { "en": "Apa Itu Fireman Switch Lift?", "id": "Mode Lift Khusus Petugas Damkar." },
    { "en": "Apa Itu Pressurization Fan?", "id": "Kipas Tekanan Positif Tangga Darurat." },
    { "en": "Apa Itu Smoke Extraction Fan?", "id": "Kipas Penyedot Asap Kebakaran Gedung." },
    { "en": "Apa Kabel Tahan Api (FRC)?", "id": "Fire Resistant Cable (Tahan Bakar)." },
    { "en": "Apa Warna Umum Kabel FRC?", "id": "Oranye." },
    { "en": "Apa Itu Konektor BNC Drat?", "id": "Konektor CCTV Putar Tanpa Solder." },
    { "en": "Apa Itu Konektor DC Male Baut?", "id": "Konektor Power CCTV Tanpa Solder." },
    { "en": "Apa Fungsi CCTV Balun Pasif?", "id": "Kirim Video Lewat Kabel LAN." },
    { "en": "Apa Jarak Maksimal CCTV Coaxial?", "id": "Sekitar 100 Hingga 300 Meter." },
    { "en": "Apa Itu DVR (Digital Video Recorder)?", "id": "Perekam Kamera Analog." },
    { "en": "Apa Itu NVR (Network Video Recorder)?", "id": "Perekam Kamera IP Digital." },
    { "en": "Apa Itu IP Camera PoE?", "id": "Kamera Digital Power Lewat LAN." },
    { "en": "Apa Itu PTZ Camera?", "id": "Pan Tilt Zoom (Gerak Putar)." },
    { "en": "Apa Fungsi IR Illuminator CCTV?", "id": "Lampu Inframerah Untuk Malam Hari." },
    { "en": "Apa Itu Resolusi 5MP CCTV?", "id": "Lima Megapixel (Kualitas Tinggi)." },
    { "en": "Apa Itu Harddisk Surveillance?", "id": "HDD Khusus Rekam 24 Jam." },
    { "en": "Apa Fungsi Ground Loop Isolator CCTV?", "id": "Hilangkan Garis Gangguan Gambar." },
    { "en": "Apa Itu Kabel RG59+Power?", "id": "Kabel Coaxial Gabung Kabel Listrik." },
    { "en": "Apa Itu Kabel Snake Audio?", "id": "Kabel Multicore Audio Panggung." },
    { "en": "Apa Itu Konektor Speakon 4 Pole?", "id": "Konektor Speaker Bi-amp Profesional." },
    { "en": "Apa Itu Kabel Microphone Balanced?", "id": "Dua Sinyal Satu Ground." },
    { "en": "Apa Itu Kabel Instrument Unbalanced?", "id": "Satu Sinyal Satu Ground." },
    { "en": "Apa Fungsi DI Box Pasif?", "id": "Sesuaikan Impedansi Pakai Trafo." },
    { "en": "Apa Fungsi DI Box Aktif?", "id": "Sesuaikan Impedansi Pakai Elektronik." },
    { "en": "Apa Fungsi Crossover Aktif?", "id": "Pisah Frekuensi Sebelum Amplifier." },
    { "en": "Apa Fungsi Crossover Pasif?", "id": "Pisah Frekuensi Setelah Amplifier." },
    { "en": "Apa Itu Amplifier Kelas AB?", "id": "Kompromi Efisiensi Dan Kualitas." },
    { "en": "Apa Itu Amplifier Kelas H?", "id": "Tegangan Suplai Berubah Sesuai Sinyal." },
    { "en": "Apa Itu Damping Factor Amplifier?", "id": "Kemampuan Kontrol Gerak Speaker." },
    { "en": "Apa Itu Slew Rate Amplifier?", "id": "Kecepatan Respon Perubahan Tegangan." },
    { "en": "Apa Itu Gain Sensitivity?", "id": "Input Tegangan Untuk Output Maksimum." },
    { "en": "Apa Protokol Dante Audio?", "id": "Audio Digital Lewat Jaringan IP." },
    { "en": "Apa Itu Latency Audio?", "id": "Waktu Tunda Sinyal Masuk Keluar." },
    { "en": "Apa Itu Sample Rate 48kHz?", "id": "Standar Audio Video Digital." },
    { "en": "Apa Itu Bit Depth 24 Bit?", "id": "Dinamika Suara Sangat Luas." },
    { "en": "Apa Itu Aliasing Audio?", "id": "Frekuensi Palsu Akibat Sampling Rendah." },
    { "en": "Apa Fungsi Low Cut Filter?", "id": "Potong Frekuensi Bass Gemuruh." },
    { "en": "Apa Fungsi Ground Lift Switch?", "id": "Putus Ground Atasi Hum Loop." },
    { "en": "Apa Itu EMI Shielding Tape?", "id": "Isolasi Tembaga Anti Interferensi." },
    { "en": "Apa Itu Heat Shrink Ratio 3:1?", "id": "Menyusut Sepertiga Ukuran Asli." },
    { "en": "Apa Itu Heat Shrink Ratio 4:1?", "id": "Menyusut Seperempat Ukuran Asli." },
    { "en": "Apa Itu Kabel NYM 3x2.5mm?", "id": "Kabel Putih Tiga Inti Tembaga." },
    { "en": "Apa Itu Kabel NYY 4x10mm?", "id": "Kabel Tanah Hitam Empat Inti." },
    { "en": "Apa Itu Kabel Twist (TIC)?", "id": "Kabel Udara Aluminium Berpilin." },
    { "en": "Apa Itu Konektor Piercing (Tap)?", "id": "Sadap Kabel Twist Tanpa Kupas." },
    { "en": "Apa Itu Dead End Clamp?", "id": "Klem Ujung Tarik Kabel Twist." },
    { "en": "Apa Itu Suspension Clamp?", "id": "Klem Gantung Kabel Twist Lurus." },
    { "en": "Apa Itu Stainless Steel Strap?", "id": "Pita Besi Pengikat Tiang." },
    { "en": "Apa Itu Stopping Buckle?", "id": "Kunci Pengikat Stainless Strap." },
    { "en": "Apa Itu Obeng Tespen DC?", "id": "Cek Kelistrikan Mobil 12-24 Volt." },
    { "en": "Apa Itu Logic Probe Digital?", "id": "Cek Logika High Low Pulse." },
    { "en": "Apa Itu Kapasitor Mylar 100V?", "id": "Tahan Tegangan Maksimal 100 Volt." },
    { "en": "Apa Itu Kapasitor Fan 1.5uF?", "id": "Kapasitor Run Kipas Angin." },
    { "en": "Apa Ciri Kapasitor Fan Lemah?", "id": "Kipas Putar Lambat Atau Dengung." },
    { "en": "Apa Itu Thermofuse Kipas Angin?", "id": "Sekering Suhu Di Gulungan." },
    { "en": "Apa Suhu Thermofuse Kipas Umum?", "id": "Sekitar 130 Hingga 135 Celcius." },
    { "en": "Apa Itu Bushing Kipas Angin?", "id": "Bantalan Poros As Kipas." },
    { "en": "Apa Itu Gearbox Kipas Angin?", "id": "Mekanisme Geleng Kiri Kanan." },
    { "en": "Apa Itu Saklar Tarik (Pull Switch)?", "id": "Saklar Tali Kipas Dinding." },
    { "en": "Apa Itu Timer Kipas Angin?", "id": "Saklar Waktu Mekanis Pegas." },
    { "en": "Apa Fungsi Kapasitor Mesin Cuci?", "id": "Geser Fasa Motor Dua Arah." },
    { "en": "Apa Nilai Kapasitor Mesin Cuci?", "id": "10 Mikro Farad Dan 5 Mikro." },
    { "en": "Apa Itu Gearbox Mesin Cuci?", "id": "Reduksi Putaran Untuk Pulsator." },
    { "en": "Apa Itu Drain Motor (Retractor)?", "id": "Motor Penarik Klep Buang Air." },
    { "en": "Apa Itu Pressure Switch Sensor?", "id": "Sensor Level Air Tabung Cuci." },
    { "en": "Apa Itu Door Switch Mesin Cuci?", "id": "Sensor Pengaman Pintu Spin." },
    { "en": "Apa Itu Pulsator Mesin Cuci?", "id": "Piringan Pemutar Air Bawah." },
    { "en": "Apa Itu V-Belt Mesin Cuci?", "id": "Sabuk Karet Penggerak Gearbox." },
    { "en": "Apa Elemen Pemanas Water Heater?", "id": "Resistansi Pemanas Dalam Tangki." },
    { "en": "Apa Thermostat Water Heater?", "id": "Saklar Suhu Air Otomatis." },
    { "en": "Apa Fungsi ELCB Water Heater?", "id": "Proteksi Anti Setrum Kamar Mandi." },
    { "en": "Apa Itu Magnesium Anode?", "id": "Korban Karat Tangki Pemanas." },
    { "en": "Apa Safety Valve Water Heater?", "id": "Buang Tekanan Uap Berlebih." },
    { "en": "Apa Flow Switch Water Heater?", "id": "Deteksi Aliran Air Keran." },
    { "en": "Apa Itu Pemantik Gas Elektrik?", "id": "Hasilkan Percikan Api Tegangan Tinggi." },
    { "en": "Apa Modul Igniter Water Heater?", "id": "Kontrol Pemantik Dan Gas Otomatis." },
    { "en": "Apa Itu Solenoid Gas Valve?", "id": "Katup Buka Tutup Gas Elpiji." },
    { "en": "Apa Itu Sensor Api (Flame Rod)?", "id": "Deteksi Nyala Api Lewat Ionisasi." },
    { "en": "Apa Termokopel Kompor Gas?", "id": "Pengaman Tutup Gas Saat Padam." },
    { "en": "Apa Piezo Igniter Kompor?", "id": "Pemantik Tekan Kristal Piezo." },
    { "en": "Apa Itu Kulkas Inverter?", "id": "Kompresor Kecepatan Variabel Hemat Listrik." },
    { "en": "Apa Itu Kulkas Non-Inverter?", "id": "Kompresor On Off Kecepatan Tetap." },
    { "en": "Apa Itu Defrost Timer 1-3?", "id": "Konfigurasi Kaki Timer Tertentu." },
    { "en": "Apa Itu Defrost Timer 1-4?", "id": "Konfigurasi Kaki Timer Umum." },
    { "en": "Apa Bimetal Defrost Kulkas?", "id": "Sensor Dingin Pemicu Heater." },
    { "en": "Apa Fan Evaporator Kulkas?", "id": "Kipas Sirkulasi Udara Dingin." },
    { "en": "Apa Overload Relay Kompresor?", "id": "Proteksi Panas Dan Arus Lebih." },
    { "en": "Apa Kapasitor Running Kulkas?", "id": "Efisiensi Kerja Motor Kompresor." },
    { "en": "Apa Itu AC Split Wall?", "id": "Unit Indoor Dan Outdoor Terpisah." },
    { "en": "Apa Itu AC Cassette?", "id": "Unit Indoor Tanam Plafon." },
    { "en": "Apa Itu AC Floor Standing?", "id": "Unit Indoor Berdiri Di Lantai." },
    { "en": "Apa Itu Pipa Tembaga 1/4?", "id": "Pipa Kecil Tekanan Tinggi (Cair)." },
    { "en": "Apa Itu Pipa Tembaga 3/8?", "id": "Pipa Besar Tekanan Rendah (Gas)." },
    { "en": "Apa Itu Isolasi Pipa (Armaflex)?", "id": "Busa Penahan Suhu Dingin Pipa." },
    { "en": "Apa Itu Flaring Tool AC?", "id": "Alat Mekar Ujung Pipa Tembaga." },
    { "en": "Apa Itu Nepel AC (Flare Nut)?", "id": "Mur Kuningan Sambungan Pipa." },
    { "en": "Apa Itu Manifold Gauge AC?", "id": "Alat Ukur Tekanan Freon." },
    { "en": "Apa Itu Freon R32?", "id": "Refrigeran Ramah Lingkungan Tekanan Tinggi." },
    { "en": "Apa Itu Freon R410A?", "id": "Campuran R32 Dan R125." },
    { "en": "Apa Itu Kapasitor Kompresor AC?", "id": "Kapasitor Start Dan Run Kompresor." },
    { "en": "Apa Thermistor Ruangan AC?", "id": "Sensor Suhu Udara Kamar." },
    { "en": "Apa Thermistor Pipa AC?", "id": "Sensor Suhu Evaporator Indoor." },
    { "en": "Apa Itu Motor Swing AC?", "id": "Motor Stepper Kecil Penggerak Sirip." },
    { "en": "Apa Remote AC Universal?", "id": "Remote Pengganti Banyak Merek." },
    { "en": "Apa Modul PCB Universal AC?", "id": "Mainboard Pengganti Segala Merek AC." },
    { "en": "Apa Fungsi Sensor MAP Mobil?", "id": "Ukur Tekanan Udara Manifold Masuk." },
    { "en": "Apa Fungsi Sensor MAF Mobil?", "id": "Ukur Massa Udara Yang Masuk." },
    { "en": "Apa Fungsi Sensor TPS Throttle?", "id": "Deteksi Posisi Bukaan Katup Gas." },
    { "en": "Apa Fungsi Sensor CKP Crankshaft?", "id": "Deteksi Posisi Putaran Poros Engkol." },
    { "en": "Apa Fungsi Sensor CMP Camshaft?", "id": "Deteksi Posisi Noken As Katup." },
    { "en": "Apa Fungsi Sensor Knocking Mesin?", "id": "Deteksi Getaran Akibat Pembakaran Dini." },
    { "en": "Apa Fungsi Sensor Oksigen (O2)?", "id": "Ukur Sisa Oksigen Gas Buang." },
    { "en": "Apa Fungsi Sensor Suhu ECT?", "id": "Ukur Suhu Cairan Pendingin Mesin." },
    { "en": "Apa Fungsi Idle Air Control?", "id": "Atur Udara Saat Mesin Langsam." },
    { "en": "Apa Fungsi Injektor Bahan Bakar?", "id": "Semprotkan Bensin Ke Ruang Bakar." },
    { "en": "Apa Fungsi Koil Pengapian (Ignition)?", "id": "Naikkan Tegangan Busi Jadi Ribuan." },
    { "en": "Apa Fungsi Busi (Spark Plug)?", "id": "Percikkan Api Bakar Campuran Bensin." },
    { "en": "Apa Celah Standar Busi Mobil?", "id": "Sekitar 0.8 Hingga 1.1 Milimeter." },
    { "en": "Apa Fungsi Alternator Rectifier?", "id": "Ubah Arus AC Jadi DC." },
    { "en": "Apa Fungsi Voltage Regulator Alternator?", "id": "Stabilkan Tegangan Output Pengisian Aki." },
    { "en": "Apa Fungsi Motor Starter Mobil?", "id": "Putar Mesin Awal Hidupkan Mobil." },
    { "en": "Apa Fungsi Solenoid Starter?", "id": "Dorong Gigi Pinion Ke Flywheel." },
    { "en": "Apa Itu ECU Remapping?", "id": "Modifikasi Software Kontrol Mesin Mobil." },
    { "en": "Apa Fungsi ABS Modulator?", "id": "Atur Tekanan Minyak Rem Otomatis." },
    { "en": "Apa Fungsi Sensor Kecepatan Roda?", "id": "Kirim Data Putaran Ke ABS." },
    { "en": "Apa Itu Protokol NMEA 0183?", "id": "Standar Komunikasi Data Elektronik Kapal." },
    { "en": "Apa Itu Protokol NMEA 2000?", "id": "Jaringan Kapal Berbasis CAN Bus." },
    { "en": "Apa Fungsi Fish Finder (Sonar)?", "id": "Deteksi Ikan Dengan Gelombang Suara." },
    { "en": "Apa Fungsi Transduser Sonar?", "id": "Ubah Listrik Jadi Gelombang Suara." },
    { "en": "Apa Fungsi Radar Kapal Laut?", "id": "Deteksi Objek Sekitar Saat Gelap." },
    { "en": "Apa Fungsi AIS (Automatic Identification System)?", "id": "Kirim Data Posisi Kapal Otomatis." },
    { "en": "Apa Fungsi EPIRB Darurat?", "id": "Kirim Sinyal Bahaya Ke Satelit." },
    { "en": "Apa Fungsi SART (Search Rescue)?", "id": "Pantulkan Sinyal Radar Tim SAR." },
    { "en": "Apa Fungsi Radio VHF Marine?", "id": "Komunikasi Suara Antar Kapal Pantai." },
    { "en": "Apa Saluran Darurat VHF Marine?", "id": "Channel 16 (Enam Belas)." },
    { "en": "Apa Fungsi Autopilot Kapal?", "id": "Kemudikan Kapal Sesuai Rute GPS." },
    { "en": "Apa Fungsi Gyrocompass Kapal?", "id": "Tunjuk Arah Utara Sejati Stabil." },
    { "en": "Apa Fungsi Anemometer Kapal?", "id": "Ukur Kecepatan Dan Arah Angin." },
    { "en": "Apa Fungsi Altimeter Pesawat?", "id": "Ukur Ketinggian Berdasarkan Tekanan Udara." },
    { "en": "Apa Fungsi Pitot Tube Pesawat?", "id": "Ukur Kecepatan Udara (Airspeed)." },
    { "en": "Apa Fungsi Transponder Pesawat?", "id": "Kirim Kode Identitas Ke Radar." },
    { "en": "Apa Fungsi TCAS Pesawat?", "id": "Hindari Tabrakan Udara Otomatis." },
    { "en": "Apa Fungsi FDR (Black Box)?", "id": "Rekam Data Penerbangan Pesawat." },
    { "en": "Apa Fungsi CVR (Cockpit Voice)?", "id": "Rekam Suara Percakapan Pilot." },
    { "en": "Apa Itu Sistem Fly-By-Wire?", "id": "Kendali Pesawat Lewat Sinyal Elektronik." },
    { "en": "Apa Fungsi ILS (Instrument Landing)?", "id": "Pandu Pesawat Mendarat Presisi." },
    { "en": "Apa Itu Avionics Bus ARINC 429?", "id": "Standar Komunikasi Data Pesawat Terbang." },
    { "en": "Apa Fungsi BEC Pada ESC?", "id": "Suplai Daya Ke Receiver Servo." },
    { "en": "Apa Itu UBEC (Universal BEC)?", "id": "Regulator Tegangan Eksternal Efisien." },
    { "en": "Apa Arti Rating kV Motor?", "id": "RPM Per Volt Tanpa Beban." },
    { "en": "Apa Fungsi ESC (Electronic Speed Controller)?", "id": "Atur Putaran Motor Brushless." },
    { "en": "Apa Itu Protokol DSHOT ESC?", "id": "Sinyal Digital Kontrol Motor Cepat." },
    { "en": "Apa Itu Firmware BLHeli_32?", "id": "Software Pengendali ESC 32-bit." },
    { "en": "Apa Fungsi Flight Controller (FC)?", "id": "Otak Penstabil Gerakan Drone." },
    { "en": "Apa Sensor Utama Flight Controller?", "id": "Gyroscope Dan Accelerometer (IMU)." },
    { "en": "Apa Fungsi PDB (Power Distribution Board)?", "id": "Bagi Daya Baterai Ke ESC." },
    { "en": "Apa Fungsi VTX (Video Transmitter)?", "id": "Kirim Gambar Kamera Ke Goggles." },
    { "en": "Apa Satuan Daya VTX?", "id": "Miliwatt (mW)." },
    { "en": "Apa Fungsi Antena Circular Polarization?", "id": "Kurangi Gangguan Pantulan Sinyal Video." },
    { "en": "Apa Itu Antena RHCP?", "id": "Right Hand Circular Polarization." },
    { "en": "Apa Itu Antena LHCP?", "id": "Left Hand Circular Polarization." },
    { "en": "Apa Fungsi OSD (On Screen Display)?", "id": "Tampilkan Data Terbang Di Layar." },
    { "en": "Apa Fungsi Receiver (RX) Drone?", "id": "Terima Perintah Dari Remote Control." },
    { "en": "Apa Itu Protokol SBUS?", "id": "Sinyal Serial Digital Futaba/FrSky." },
    { "en": "Apa Itu Protokol IBUS?", "id": "Sinyal Serial Digital FlySky." },
    { "en": "Apa Itu Protokol ELRS (ExpressLRS)?", "id": "Link Radio Jarak Jauh Open Source." },
    { "en": "Apa Itu Protokol Crossfire (CRF)?", "id": "Link Radio Jarak Jauh TBS." },
    { "en": "Apa Fungsi Telemetry Drone?", "id": "Kirim Data Status Balik Remote." },
    { "en": "Apa Itu RSSI (Signal Strength)?", "id": "Indikator Kekuatan Sinyal Diterima." },
    { "en": "Apa Fungsi GPS Module Drone?", "id": "Kunci Posisi Dan Return Home." },
    { "en": "Apa Itu Mode Angle/Stabilize?", "id": "Drone Otomatis Rata Saat Lepas." },
    { "en": "Apa Itu Mode Acro/Manual?", "id": "Kendali Penuh Tanpa Limit Sudut." },
    { "en": "Apa Fungsi Failsafe Drone?", "id": "Aksi Otomatis Saat Hilang Sinyal." },
    { "en": "Apa Itu Smoke Stopper?", "id": "Sekering Pembatas Arus Saat Tes." },
    { "en": "Apa Fungsi Capacitor Low ESR?", "id": "Filter Noise Video Di Drone." },
    { "en": "Apa Itu LiPo Battery C-Rating?", "id": "Kemampuan Baterai Lepas Arus Besar." },
    { "en": "Apa Bahaya Baterai LiPo Kembung?", "id": "Tidak Stabil Dan Mudah Terbakar." },
    { "en": "Apa Tegangan Storage LiPo?", "id": "3.80 Hingga 3.85 Volt." },
    { "en": "Apa Fungsi Balance Charger?", "id": "Samakan Tegangan Tiap Sel Baterai." },
    { "en": "Apa Konektor Balance JST-XH?", "id": "Kabel Kecil Untuk Charging LiPo." },
    { "en": "Apa Konektor Power XT60?", "id": "Konektor Kuning Standar Drone Balap." },
    { "en": "Apa Konektor Power XT30?", "id": "Konektor Kuning Kecil Drone Mikro." },
    { "en": "Apa Fungsi Propeller CW?", "id": "Baling-baling Putar Searah Jam." },
    { "en": "Apa Fungsi Propeller CCW?", "id": "Baling-baling Putar Lawan Jam." },
    { "en": "Apa Arti Ukuran Prop 5040?", "id": "Diameter 5 Inci Pitch 4.0." },
    { "en": "Apa Itu Pitch Baling-baling?", "id": "Jarak Maju Satu Kali Putaran." },
    { "en": "Apa Fungsi Gimbal Kamera?", "id": "Stabilkan Kamera Secara Mekanis." },
    { "en": "Apa Itu Motor Brushless Gimbal?", "id": "Motor Halus Penggerak Gimbal." },
    { "en": "Apa Itu Vibration Damper Karet?", "id": "Redam Getaran Ke Flight Controller." },
    { "en": "Apa Fungsi Buzzer Lost Model?", "id": "Bunyi Keras Saat Drone Hilang." },
    { "en": "Apa Fungsi LED Strip Programmable?", "id": "Lampu Hias Warna-warni Diatur FC." },
    { "en": "Apa Itu Blackbox Data Logger?", "id": "Rekam Data Penerbangan Untuk Analisa." },
    { "en": "Apa Fungsi Betaflight Configurator?", "id": "Software Setting Flight Controller Drone." },
    { "en": "Apa Itu PID Tuning Drone?", "id": "Atur Respon Terbang Agar Stabil." },
    { "en": "Apa Itu Rate Profile Drone?", "id": "Atur Sensitivitas Stick Remote." },
    { "en": "Apa Itu Throttle Curve?", "id": "Grafik Respon Gas Motor." },
    { "en": "Apa Fungsi Arming Switch?", "id": "Saklar Pengaman Hidupkan Motor." },
    { "en": "Apa Itu Air Mode Betaflight?", "id": "Kontrol Stabil Saat Gas Nol." },
    { "en": "Apa Itu Turtle Mode (Flip Over)?", "id": "Balikkan Drone Terbalik Tanpa Tangan." },
    { "en": "Apa Fungsi Camera FPV?", "id": "Mata Pilot Saat Terbang First Person." },
    { "en": "Apa Itu TVL (TV Lines)?", "id": "Resolusi Kamera Analog FPV." },
    { "en": "Apa Itu FOV (Field Of View)?", "id": "Sudut Pandang Lensa Kamera." },
    { "en": "Apa Itu Latency Video FPV?", "id": "Jeda Waktu Gambar Tampil Layar." },
    { "en": "Apa Fungsi ND Filter Kamera?", "id": "Kurangi Cahaya Dan Efek Jello." },
    { "en": "Apa Itu Digital FPV System?", "id": "Video Kualitas HD Latensi Rendah." },
    { "en": "Apa Itu PLC Scan Time?", "id": "Waktu Baca Input Proses Output." },
    { "en": "Apa Itu Digital Input PLC?", "id": "Sinyal Masukan On Atau Off." },
    { "en": "Apa Itu Digital Output PLC?", "id": "Sinyal Keluaran Hidup Atau Mati." },
    { "en": "Apa Itu Analog Input PLC?", "id": "Sinyal Masukan Nilai Variabel Kontinyu." },
    { "en": "Apa Itu Analog Output PLC?", "id": "Sinyal Keluaran Nilai Tegangan/Arus." },
    { "en": "Apa Resolusi ADC 12-Bit?", "id": "Nilai 0 Hingga 4095." },
    { "en": "Apa Resolusi ADC 10-Bit?", "id": "Nilai 0 Hingga 1023." },
    { "en": "Apa Itu Ladder Logic?", "id": "Bahasa Pemrograman Grafis Mirip Tangga." },
    { "en": "Apa Simbol Kontak NO Di Ladder?", "id": "Dua Garis Vertikal Terpisah." },
    { "en": "Apa Simbol Kontak NC Di Ladder?", "id": "Dua Garis Vertikal Ada Garis Miring." },
    { "en": "Apa Simbol Coil Output Di Ladder?", "id": "Tanda Kurung Buka Tutup." },
    { "en": "Apa Fungsi Instruksi TON (Timer On Delay)?", "id": "Hitung Waktu Setelah Input Nyala." },
    { "en": "Apa Fungsi Instruksi TOF (Timer Off Delay)?", "id": "Hitung Waktu Setelah Input Mati." },
    { "en": "Apa Fungsi Instruksi RTO (Retentive Timer)?", "id": "Simpan Waktu Meski Daya Hilang." },
    { "en": "Apa Fungsi Instruksi CTU (Count Up)?", "id": "Hitung Naik Setiap Pulsa Masuk." },
    { "en": "Apa Fungsi Instruksi CTD (Count Down)?", "id": "Hitung Turun Setiap Pulsa Masuk." },
    { "en": "Apa Itu Preset Value (PV)?", "id": "Nilai Target Timer Atau Counter." },
    { "en": "Apa Itu Accumulated Value (ACC)?", "id": "Nilai Berjalan Timer Atau Counter." },
    { "en": "Apa Fungsi Instruksi MOV (Move)?", "id": "Salin Nilai Data Ke Register Lain." },
    { "en": "Apa Fungsi Instruksi CMP (Compare)?", "id": "Bandingkan Dua Nilai (Sama/Beda)." },
    { "en": "Apa Itu Latching (Set Coil)?", "id": "Output Tetap Nyala Meski Input Mati." },
    { "en": "Apa Itu Unlatching (Reset Coil)?", "id": "Matikan Output Yang Terkunci." },
    { "en": "Apa Itu One Shot Rising (OSR)?", "id": "Satu Pulsa Saat Sinyal Naik." },
    { "en": "Apa Itu One Shot Falling (OSF)?", "id": "Satu Pulsa Saat Sinyal Turun." },
    { "en": "Apa Itu PID Loop Control?", "id": "Kontrol Umpan Balik Presisi Otomatis." },
    { "en": "Apa Parameter P (Proportional) PID?", "id": "Respon Sesuai Besar Kesalahan Saat Ini." },
    { "en": "Apa Parameter I (Integral) PID?", "id": "Koreksi Akumulasi Kesalahan Masa Lalu." },
    { "en": "Apa Parameter D (Derivative) PID?", "id": "Prediksi Kesalahan Di Masa Depan." },
    { "en": "Apa Itu Tuning PID?", "id": "Mencari Nilai P I D Optimal." },
    { "en": "Apa Itu Setpoint (SP)?", "id": "Nilai Target Yang Diinginkan." },
    { "en": "Apa Itu Process Variable (PV)?", "id": "Nilai Aktual Dari Sensor Lapangan." },
    { "en": "Apa Itu Control Variable (CV)?", "id": "Sinyal Output Ke Actuator." },
    { "en": "Apa Itu SCADA System?", "id": "Supervisory Control And Data Acquisition." },
    { "en": "Apa Fungsi HMI (Human Machine Interface)?", "id": "Layar Operator Kendalikan Mesin." },
    { "en": "Apa Itu Tag Database HMI?", "id": "Daftar Alamat Variabel PLC." },
    { "en": "Apa Itu Alarm HMI?", "id": "Peringatan Visual Saat Ada Gangguan." },
    { "en": "Apa Itu Trend HMI?", "id": "Grafik Riwayat Data Proses." },
    { "en": "Apa Itu Historian Database?", "id": "Penyimpanan Data Jangka Panjang SCADA." },
    { "en": "Apa Itu Distributed Control System (DCS)?", "id": "Sistem Kontrol Pabrik Skala Besar." },
    { "en": "Apa Beda PLC Dan DCS?", "id": "PLC Cepat Diskrit, DCS Proses Kontinyu." },
    { "en": "Apa Itu Remote I/O?", "id": "Modul Input Output Jarak Jauh." },
    { "en": "Apa Fungsi Industrial Ethernet Switch?", "id": "Hubungkan PLC HMI Dalam Jaringan." },
    { "en": "Apa Itu Managed Switch?", "id": "Switch Pintar Bisa Diatur (VLAN)." },
    { "en": "Apa Itu Unmanaged Switch?", "id": "Switch Plug And Play Sederhana." },
    { "en": "Apa Fungsi Sensor Proximity Induktif?", "id": "Deteksi Logam Jarak Dekat." },
    { "en": "Apa Fungsi Sensor Proximity Kapasitif?", "id": "Deteksi Benda Padat Cair Non-logam." },
    { "en": "Apa Fungsi Sensor Photoelectric Diffuse?", "id": "Deteksi Objek Dari Pantulan Cahaya." },
    { "en": "Apa Fungsi Sensor Photoelectric Retro-reflective?", "id": "Deteksi Objek Memotong Cahaya Reflektor." },
    { "en": "Apa Fungsi Sensor Photoelectric Through-beam?", "id": "Deteksi Paling Jauh Pemancar Penerima Terpisah." },
    { "en": "Apa Itu Sensor Output NPN?", "id": "Switching Ke Negatif (Sinking)." },
    { "en": "Apa Itu Sensor Output PNP?", "id": "Switching Ke Positif (Sourcing)." },
    { "en": "Apa Itu Sensor Normally Open (NO)?", "id": "Sinyal Mati Jika Tidak Ada Objek." },
    { "en": "Apa Itu Sensor Normally Closed (NC)?", "id": "Sinyal Hidup Jika Tidak Ada Objek." },
    { "en": "Apa Itu Encoder Rotary?", "id": "Sensor Posisi Sudut Putaran Poros." },
    { "en": "Apa Itu Incremental Encoder?", "id": "Hanya Hitung Perubahan Posisi." },
    { "en": "Apa Itu Absolute Encoder?", "id": "Tahu Posisi Sebenarnya Meski Restart." },
    { "en": "Apa Itu Resolution PPR Encoder?", "id": "Pulse Per Revolution (Pulsa Per Putaran)." },
    { "en": "Apa Fungsi Limit Switch?", "id": "Deteksi Batas Gerak Mekanis Fisik." },
    { "en": "Apa Itu Reed Switch?", "id": "Saklar Kaca Aktif Oleh Magnet." },
    { "en": "Apa Itu Load Cell?", "id": "Sensor Pengukur Berat Tekanan." },
    { "en": "Apa Itu Thermocouple?", "id": "Sensor Suhu Dua Logam Berbeda." },
    { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu Perubahan Resistansi Logam." },
    { "en": "Apa Itu Pt100?", "id": "RTD Platinum 100 Ohm Pada 0C." },
    { "en": "Apa Itu Transmitter 4-20mA?", "id": "Pengirim Sinyal Arus Standar Industri." },
    { "en": "Mengapa Pakai Sinyal 4-20mA?", "id": "Tahan Noise Dan Deteksi Putus Kabel." },
    { "en": "Apa Itu Flowmeter Elektromagnetik?", "id": "Ukur Aliran Cairan Konduktif Tanpa Hambatan." },
    { "en": "Apa Itu Flowmeter Ultrasonic?", "id": "Ukur Aliran Menggunakan Gelombang Suara." },
    { "en": "Apa Itu Flowmeter Vortex?", "id": "Ukur Aliran Berdasarkan Pusaran Fluida." },
    { "en": "Apa Itu Pressure Transmitter?", "id": "Ubah Tekanan Menjadi Sinyal Listrik." },
    { "en": "Apa Itu Level Transmitter Ultrasonik?", "id": "Ukur Tinggi Tangki Tanpa Sentuh." },
    { "en": "Apa Itu Solenoid Valve?", "id": "Katup Buka Tutup Elektrik." },
    { "en": "Apa Itu Control Valve?", "id": "Katup Pengatur Bukaan Persentase (Proporsional)." },
    { "en": "Apa Itu Valve Positioner?", "id": "Alat Posisikan Katup Sesuai Sinyal." },
    { "en": "Apa Fungsi Inverter (VFD)?", "id": "Atur Kecepatan Motor Listrik AC." },
    { "en": "Apa Itu Soft Starter?", "id": "Kurangi Lonjakan Arus Awal Motor." },
    { "en": "Apa Fungsi Relay Safety?", "id": "Relay Ganda Untuk Keamanan Mesin." },
    { "en": "Apa Itu Light Curtain?", "id": "Tirai Sensor Cahaya Pengaman Operator." },
    { "en": "Apa Itu Emergency Stop Button?", "id": "Tombol Merah Stop Darurat Mengunci." },
    { "en": "Apa Itu Isolator Signal?", "id": "Pemisah Ground Loop Antar Instrumen." },
    { "en": "Apa Itu Power Supply DIN Rail?", "id": "Adaptor DC Pasang Di Rel Panel." },
    { "en": "Apa Tegangan Kontrol Umum PLC?", "id": "24 Volt DC." },
    { "en": "Apa Kabel Komunikasi RS-485?", "id": "Kabel Data Serial Tahan Noise Jauh." },
    { "en": "Apa Protokol Modbus RTU?", "id": "Bahasa Komunikasi Serial Paling Umum." },
    { "en": "Apa Protokol Modbus TCP/IP?", "id": "Modbus Lewat Kabel Jaringan Ethernet." },
    { "en": "Apa Itu Profibus?", "id": "Protokol Fieldbus Standar Siemens." },
    { "en": "Apa Itu EtherNet/IP?", "id": "Protokol Ethernet Industri Rockwell." },
    { "en": "Apa Itu MQTT?", "id": "Protokol Pesan IoT Ringan." },
    { "en": "Apa Itu IoT Gateway?", "id": "Jembatan Sensor Ke Internet Cloud." },
    { "en": "Apa Itu Edge Computing?", "id": "Olah Data Di Lokasi Bukan Cloud." },
    { "en": "Apa Itu OEE (Overall Equipment Effectiveness)?", "id": "Ukuran Efisiensi Kinerja Mesin Produksi." },
    { "en": "Apa Itu Andon System?", "id": "Sistem Tampilan Status Produksi Visual." },
    { "en": "Apa Fungsi Tower Lamp?", "id": "Lampu Menara Status Mesin (Merah/Kuning/Hijau)." },
    { "en": "Apa Warna Tombol Start Standar?", "id": "Hijau." },
    { "en": "Apa Warna Tombol Stop Standar?", "id": "Merah." },
    { "en": "Apa Warna Tombol Reset Standar?", "id": "Biru." },
    { "en": "Apa Itu Ferrule Kabel?", "id": "Selongsong Ujung Kabel Serabut." },
    { "en": "Apa Itu Cable Gland?", "id": "Pengunci Kabel Masuk Box Panel." },
    { "en": "Apa Itu Wiring Duct?", "id": "Jalur Kabel Berlubang Dalam Panel." },
    { "en": "Apa Itu DIN Rail?", "id": "Rel Besi Dudukan Komponen Panel." },
    { "en": "Apa Itu Grounding Rod Tembaga Bonded?", "id": "Baja Lapis Tembaga Hemat Biaya." },
    { "en": "Apa Itu Grounding Rod Solid Copper?", "id": "Tembaga Murni Tahan Korosi." },
    { "en": "Apa Fungsi Bentonit Pada Grounding?", "id": "Turunkan Resistansi Tanah Kering." },
    { "en": "Apa Itu GEM (Ground Enhancement Material)?", "id": "Semen Konduktif Penurun Tahanan." },
    { "en": "Apa Itu Busbar Grounding Cabinet?", "id": "Titik Kumpul Kabel Tanah Panel." },
    { "en": "Apa Fungsi Lightning Counter?", "id": "Hitung Jumlah Sambaran Petir." },
    { "en": "Apa Itu ESE Lightning Terminal?", "id": "Early Streamer Emission (Radius Luas)." },
    { "en": "Apa Itu Faraday Cage Protection?", "id": "Sangkar Kawat Kelilingi Bangunan." },
    { "en": "Apa Itu Down Conductor Petir?", "id": "Kabel Penyalur Arus Ke Tanah." },
    { "en": "Apa Itu Exothermic Welding Grounding?", "id": "Las Cadwelding Sambungan Permanen." },
    { "en": "Apa Itu PID Proportional Band?", "id": "Kebalikan Dari Gain Kontroler." },
    { "en": "Apa Itu PID Integral Action Time?", "id": "Waktu Koreksi Error Masa Lalu." },
    { "en": "Apa Itu PID Derivative Action Time?", "id": "Waktu Prediksi Error Depan." },
    { "en": "Apa Itu Offset Error Kontrol?", "id": "Selisih Setpoint Dan Process Variable." },
    { "en": "Apa Itu Overshoot Respon Sistem?", "id": "Lonjakan Melebihi Nilai Setpoint." },
    { "en": "Apa Itu Settling Time?", "id": "Waktu Mencapai Kestabilan Output." },
    { "en": "Apa Itu Rise Time Sistem?", "id": "Waktu Naik 10 Ke 90 Persen." },
    { "en": "Apa Itu Steady State Error?", "id": "Error Tetap Setelah Sistem Stabil." },
    { "en": "Apa Itu Tuning PID Auto-tune?", "id": "PLC Cari Parameter PID Otomatis." },
    { "en": "Apa Itu Bang-bang Control?", "id": "Kontrol On-Off Sederhana Histeresis." },
    { "en": "Apa Itu Transistor Darlington TIP120?", "id": "NPN Darlington Power Switching." },
    { "en": "Apa Itu Transistor Darlington TIP127?", "id": "PNP Darlington Power Switching." },
    { "en": "Apa Itu Mosfet IRFZ44N?", "id": "N-Channel Mosfet Umum 49 Ampere." },
    { "en": "Apa Itu Mosfet IRF9540?", "id": "P-Channel Mosfet Umum." },
    { "en": "Apa Itu Triac BT136?", "id": "Triac 4 Ampere Umum." },
    { "en": "Apa Itu Triac BTA16?", "id": "Triac 16 Ampere Isolated." },
    { "en": "Apa Itu SCR TIC106?", "id": "Thyristor Sensitif Arus Kecil." },
    { "en": "Apa Itu Diode Zener 12V?", "id": "Stabilkan Tegangan Di 12 Volt." },
    { "en": "Apa Itu Diode Schottky 1N5819?", "id": "Dioda Cepat Drop Tegangan Rendah." },
    { "en": "Apa Itu Diode Bridge KBP206?", "id": "Jembatan 2 Ampere 600 Volt." },
    { "en": "Apa Fungsi Thermal Paste CPU?", "id": "Isi Celah Mikroskopis Heatsink." },
    { "en": "Apa Itu Thermal Pad Silicone?", "id": "Bantalan Karet Penghantar Panas." },
    { "en": "Apa Itu Insulator Mica TO-220?", "id": "Isolasi Listrik Transistor Heatsink." },
    { "en": "Apa Itu Bushing Isolator Baut?", "id": "Cegah Baut Konek Body Transistor." },
    { "en": "Apa Itu Heatsink Extruded?", "id": "Pendingin Aluminium Sirip Memanjang." },
    { "en": "Apa Itu Fan Cooling Ball Bearing?", "id": "Kipas Laher Bola Awet." },
    { "en": "Apa Itu Fan Cooling Sleeve Bearing?", "id": "Kipas Laher Bosh Murah." },
    { "en": "Apa Fungsi Fan Guard?", "id": "Pelindung Jari Dari Baling-baling." },
    { "en": "Apa Itu Peltier TEC1-12715?", "id": "Elemen Pendingin Daya Besar." },
    { "en": "Apa Sisi Dingin Peltier?", "id": "Sisi Yang Ada Tulisan Tipe." },
    { "en": "Apa Sisi Panas Peltier?", "id": "Sisi Polos Tanpa Tulisan." },
    { "en": "Apa Itu VRLA Battery 12V 7Ah?", "id": "Aki Kering Standar UPS." },
    { "en": "Apa Itu CCA Aki Mobil?", "id": "Cold Cranking Amps (Arus Start)." },
    { "en": "Apa Itu Aki MF (Maintenance Free)?", "id": "Aki Bebas Perawatan Cairan." },
    { "en": "Apa Itu Aki Basah (Premium)?", "id": "Aki Perlu Isi Air Zuur." },
    { "en": "Apa Warna Tutup Aki Positif?", "id": "Merah." },
    { "en": "Apa Warna Tutup Aki Negatif?", "id": "Hitam Atau Biru." },
    { "en": "Apa Itu Jumper Aki?", "id": "Kabel Bantu Start Mobil Mogok." },
    { "en": "Apa Itu Battery Hydrometer?", "id": "Alat Ukur Berat Jenis Air Aki." },
    { "en": "Apa Itu Battery Load Tester?", "id": "Alat Uji Kekuatan Beban Aki." },
    { "en": "Apa Fungsi Inverter Pure Sine Wave?", "id": "Aman Untuk Peralatan Induktif." },
    { "en": "Apa Fungsi Inverter Modified Sine Wave?", "id": "Murah Tapi Bisa Dengung." },
    { "en": "Apa Itu MPPT Solar Charge Controller?", "id": "Efisiensi Tinggi Lacak Daya Puncak." },
    { "en": "Apa Itu PWM Solar Charge Controller?", "id": "Efisiensi Rendah Pemotongan Pulsa." },
    { "en": "Apa Tegangan Panel Surya 100WP?", "id": "Biasanya Sekitar 18 Hingga 21 Volt." },
    { "en": "Apa Arus Panel Surya 100WP?", "id": "Sekitar 5 Hingga 6 Ampere." },
    { "en": "Apa Itu Konektor MC4 Paralel (Y)?", "id": "Gabung Dua Panel Jadi Satu." },
    { "en": "Apa Itu Kabel PV1-F?", "id": "Kabel Khusus Solar Tahan UV." },
    { "en": "Apa Itu Deep Cycle Battery?", "id": "Tahan Discharges Dalam Berulang." },
    { "en": "Apa Itu Gel Battery Solar?", "id": "Elektrolit Gel Tahan Panas." },
    { "en": "Apa Itu Kabel Coaxial RG-6?", "id": "Kabel Antena TV Standar." },
    { "en": "Apa Itu Kabel Coaxial RG-59?", "id": "Kabel CCTV Analog Lama." },
    { "en": "Apa Itu Kabel Coaxial RG-11?", "id": "Kabel Jarak Jauh Minim Rugi." },
    { "en": "Apa Impedansi Kabel RG-6?", "id": "75 Ohm." },
    { "en": "Apa Impedansi Kabel RG-58?", "id": "50 Ohm (Radio Komunikasi)." },
    { "en": "Apa Konektor F-Type Drat?", "id": "Konektor Ulir Antena Parabola." },
    { "en": "Apa Konektor BNC Crimp?", "id": "Konektor CCTV Jepit Tang." },
    { "en": "Apa Konektor PL-259?", "id": "Konektor UHF Male Radio." },
    { "en": "Apa Konektor SO-239?", "id": "Konektor UHF Female Radio." },
    { "en": "Apa Konektor N-Type?", "id": "Konektor RF Frekuensi Tinggi Waterproof." },
    { "en": "Apa Itu SWR Meter?", "id": "Ukur Rasio Gelombang Berdiri." },
    { "en": "Apa Itu Dummy Load 50 Ohm?", "id": "Beban Buatan Tes Pemancar." },
    { "en": "Apa Itu Antena Telex?", "id": "Antena Vertikal Omnidirectional VHF." },
    { "en": "Apa Itu Antena Yagi 3 Elemen?", "id": "Antena Pengarah Gain Sedang." },
    { "en": "Apa Itu Antena Dipole 1/2 Lambda?", "id": "Antena Dasar Paling Populer." },
    { "en": "Apa Itu Balun 1:1?", "id": "Penyesuai Impedansi Simetris Asimetris." },
    { "en": "Apa Itu Kabel Ladder Line?", "id": "Kabel Feeder Dua Kawat Sejajar." },
    { "en": "Apa Itu Kabel Twin Lead TV?", "id": "Kabel Pita Antena TV Jadul." },
    { "en": "Apa Fungsi Rotator Antena?", "id": "Motor Pemutar Arah Antena." },
    { "en": "Apa Itu Lightning Arrester Coaxial?", "id": "Pelindung Petir Jalur Antena." },
    { "en": "Apa Fungsi Tang Ampere (Clamp Meter)?", "id": "Ukur Arus Tanpa Putus Kabel." },
    { "en": "Apa Itu Multimeter Analog?", "id": "Alat Ukur Jarum Penunjuk." },
    { "en": "Apa Itu Multimeter Digital (DMM)?", "id": "Alat Ukur Layar Angka." },
    { "en": "Apa Itu Parallax Error Analog?", "id": "Salah Baca Sudut Jarum." },
    { "en": "Apa Itu Zero Adjustment Ohm?", "id": "Kalibrasi Nol Jarum Ohm." },
    { "en": "Apa Fungsi Buzzer Continuity?", "id": "Cek Sambungan Kabel Putus." },
    { "en": "Apa Fungsi HFE Socket?", "id": "Cek Penguatan Transistor Bipolar." },
    { "en": "Apa Fungsi Hold Button DMM?", "id": "Tahan Angka Di Layar." },
    { "en": "Apa Fungsi Backlight DMM?", "id": "Lampu Layar Tempat Gelap." },
    { "en": "Apa Itu Kategori CAT III Meter?", "id": "Aman Untuk Instalasi Gedung." },
    { "en": "Apa Itu Kategori CAT IV Meter?", "id": "Aman Untuk Sumber Listrik Luar." },
    { "en": "Apa Bahaya Meter Murah CAT I?", "id": "Bisa Meledak Di Tegangan Tinggi." },
    { "en": "Apa Itu Fuse Keramik Multimeter?", "id": "Sekering Pasir Tahan Ledakan." },
    { "en": "Apa Itu True RMS Meter?", "id": "Akurat Ukur Gelombang Cacat." },
    { "en": "Apa Itu Auto Ranging Meter?", "id": "Otomatis Pilih Skala Ukur." },
    { "en": "Apa Itu Manual Ranging Meter?", "id": "Putar Selektor Pilih Skala." },
    { "en": "Apa Fungsi Non Contact Voltage (NCV)?", "id": "Deteksi Kabel Bertegangan Dinding." },
    { "en": "Apa Fungsi Live Wire Check?", "id": "Bedakan Fasa Dan Netral." },
    { "en": "Apa Fungsi Thermocouple Probe DMM?", "id": "Ukur Suhu Kontak Langsung." },
    { "en": "Apa Fungsi Capacitance Meter DMM?", "id": "Ukur Nilai Kapasitor Farad." },
    { "en": "Apa Output S-R Latch Jika S=1 R=0?", "id": "Output Q Menjadi High (Set)." },
    { "en": "Apa Output S-R Latch Jika S=0 R=1?", "id": "Output Q Menjadi Low (Reset)." },
    { "en": "Apa Output S-R Latch Jika S=0 R=0?", "id": "Output Q Tetap (Memory)." },
    { "en": "Apa Kondisi Terlarang S-R Latch?", "id": "Input S=1 Dan R=1." },
    { "en": "Apa Fungsi D Flip-flop?", "id": "Transfer Data Saat Clock Aktif." },
    { "en": "Apa Itu JK Flip-flop Toggle Mode?", "id": "Input J=1 Dan K=1." },
    { "en": "Apa Itu Master-Slave Flip-flop?", "id": "Gabungan Dua Flip-flop Hindari Race." },
    { "en": "Apa Itu T Flip-flop?", "id": "Toggle Output Setiap Pulsa Clock." },
    { "en": "Apa Itu Asynchronous Counter?", "id": "Clock Memicu Flip-flop Secara Beruntun." },
    { "en": "Apa Itu Synchronous Counter?", "id": "Clock Memicu Semua Flip-flop Serentak." },
    { "en": "Apa Kelemahan Asynchronous Counter?", "id": "Propagation Delay Terakumulasi (Lambat)." },
    { "en": "Apa Fungsi Shift Register SIPO?", "id": "Serial In Parallel Out." },
    { "en": "Apa Fungsi Shift Register PISO?", "id": "Parallel In Serial Out." },
    { "en": "Apa Itu Ring Counter?", "id": "Data Bergeser Memutar Kembali Awal." },
    { "en": "Apa Itu Johnson Counter?", "id": "Output Terakhir Dibalik Masuk Awal." },
    { "en": "Apa Itu Multiplexer (MUX) 4-to-1?", "id": "Pilih 1 Dari 4 Input." },
    { "en": "Apa Itu Demultiplexer (DEMUX) 1-to-4?", "id": "Sebar 1 Input Ke 4 Output." },
    { "en": "Apa Fungsi Decoder 3-to-8?", "id": "Konversi 3 Bit Ke 8 Line." },
    { "en": "Apa Fungsi Encoder 8-to-3?", "id": "Konversi 8 Line Ke 3 Bit." },
    { "en": "Apa Itu Priority Encoder?", "id": "Encoder Memilih Input Prioritas Tertinggi." },
    { "en": "Apa Itu Schmitt Trigger Input?", "id": "Input Histeresis Tahan Noise." },
    { "en": "Apa Itu Open Collector Output?", "id": "Perlu Pull-up Resistor Eksternal." },
    { "en": "Apa Itu Tri-state Buffer?", "id": "Output High, Low, Dan Hi-Z." },
    { "en": "Apa Fungsi ALU (Arithmetic Logic Unit)?", "id": "Proses Operasi Hitung Dan Logika." },
    { "en": "Apa Fungsi Register Accumulator?", "id": "Simpan Hasil Sementara Operasi ALU." },
    { "en": "Apa Fungsi Program Counter (PC)?", "id": "Simpan Alamat Instruksi Berikutnya." },
    { "en": "Apa Fungsi Stack Pointer (SP)?", "id": "Tunjuk Alamat Memori Tumpukan Data." },
    { "en": "Apa Itu Interrupt Vector?", "id": "Alamat Memori Penanganan Interupsi." },
    { "en": "Apa Itu Polling Dalam Mikrokontroler?", "id": "Cek Status Perangkat Secara Berulang." },
    { "en": "Apa Kelebihan Interrupt Dibanding Polling?", "id": "Efisien Waktu Prosesor Tidak Menunggu." },
    { "en": "Apa Itu PWM Duty Cycle 25%?", "id": "Sinyal On Seperempat Periode Waktu." },
    { "en": "Apa Itu PWM Duty Cycle 75%?", "id": "Sinyal On Tiga Perempat Periode." },
    { "en": "Apa Itu ADC Successive Approximation?", "id": "Metode Konversi Analog Digital Umum." },
    { "en": "Apa Itu ADC Flash Converter?", "id": "ADC Tercepat Tapi Mahal Komponen." },
    { "en": "Apa Itu DAC R-2R Ladder?", "id": "Konversi Digital Analog Resistor Tangga." },
    { "en": "Apa Itu Resolution ADC?", "id": "Jumlah Bit Representasi Nilai Analog." },
    { "en": "Apa Itu Quantization Error?", "id": "Selisih Nilai Asli Dan Digital." },
    { "en": "Apa Itu Aliasing Signal?", "id": "Frekuensi Palsu Akibat Sampling Rendah." },
    { "en": "Apa Syarat Nyquist Sampling?", "id": "Sampling Minimal 2 Kali Frekuensi." },
    { "en": "Apa Itu I2C ACK Bit?", "id": "Sinyal Konfirmasi Data Diterima." },
    { "en": "Apa Itu I2C NACK Bit?", "id": "Sinyal Penolakan Atau Akhir Data." },
    { "en": "Apa Itu Clock Stretching I2C?", "id": "Slave Tahan Clock Agar Sempat." },
    { "en": "Apa Itu SPI Mode 0?", "id": "Clock Idle Low, Sample Rising." },
    { "en": "Apa Itu SPI Mode 1?", "id": "Clock Idle Low, Sample Falling." },
    { "en": "Apa Itu SPI Mode 2?", "id": "Clock Idle High, Sample Falling." },
    { "en": "Apa Itu SPI Mode 3?", "id": "Clock Idle High, Sample Rising." },
    { "en": "Apa Itu UART Parity Even?", "id": "Jumlah Bit 1 Genap Valid." },
    { "en": "Apa Itu UART Parity Odd?", "id": "Jumlah Bit 1 Ganjil Valid." },
    { "en": "Apa Itu UART Frame Error?", "id": "Stop Bit Tidak Terdeteksi." },
    { "en": "Apa Itu Buck Converter CCM?", "id": "Arus Induktor Tidak Pernah Nol." },
    { "en": "Apa Itu Buck Converter DCM?", "id": "Arus Induktor Sempat Nol." },
    { "en": "Apa Itu Push-Pull Converter?", "id": "Dua Transistor Switching Bergantian." },
    { "en": "Apa Itu Half-Bridge Converter?", "id": "Dua Transistor Dengan Kapasitor Pembagi." },
    { "en": "Apa Itu Full-Bridge Converter?", "id": "Empat Transistor Bentuk H-Bridge." },
    { "en": "Apa Fungsi Snubber RCD Clamp?", "id": "Lindungi MOSFET Dari Spike Tegangan." },
    { "en": "Apa Itu Synchronous Rectification?", "id": "Ganti Dioda Output Dengan MOSFET." },
    { "en": "Apa Keuntungan Synchronous Rectification?", "id": "Efisiensi Tinggi Drop Tegangan Rendah." },
    { "en": "Apa Itu Power Factor Correction (PFC)?", "id": "Bentuk Gelombang Arus Ikuti Tegangan." },
    { "en": "Apa Itu Passive PFC?", "id": "PFC Menggunakan Induktor Besar." },
    { "en": "Apa Itu Active PFC?", "id": "PFC Menggunakan Rangkaian Switching Boost." },
    { "en": "Apa Itu Galvanic Isolation?", "id": "Pemisahan Listrik Input Dan Output." },
    { "en": "Apa Itu Feedback Loop Control?", "id": "Umpan Balik Jaga Output Stabil." },
    { "en": "Apa Itu Load Transient Response?", "id": "Respon Tegangan Saat Beban Berubah." },
    { "en": "Apa Itu Line Transient Response?", "id": "Respon Tegangan Saat Input Berubah." },
    { "en": "Apa Itu Heat Sink Thermal Resistance?", "id": "Kemampuan Buang Panas Per Watt." },
    { "en": "Apa Satuan Thermal Resistance?", "id": "Derajat Celcius Per Watt." },
    { "en": "Apa Fungsi Thermal Paste?", "id": "Isi Rongga Udara Antara Logam." },
    { "en": "Apa Itu SOA (Safe Operating Area)?", "id": "Batas Aman Operasi Transistor." },
    { "en": "Apa Itu Derating Komponen?", "id": "Operasi Di Bawah Batas Maksimum." },
    { "en": "Apa Itu MTBF (Mean Time Between Failure)?", "id": "Rata-rata Waktu Sebelum Rusak." },
    { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Lonjakan Listrik Statis Merusak Chip." },
    { "en": "Apa Fungsi Gelang Statis?", "id": "Buang Muatan Tubuh Ke Ground." },
    { "en": "Apa Itu EMI (Electromagnetic Interference)?", "id": "Gangguan Gelombang Elektromagnetik Luar." },
    { "en": "Apa Itu EMC (Electromagnetic Compatibility)?", "id": "Kemampuan Alat Bekerja Tanpa Mengganggu." },
    { "en": "Apa Fungsi Ferrite Bead Kabel?", "id": "Redam Noise Frekuensi Tinggi." },
    { "en": "Apa Itu Twisted Pair Cable?", "id": "Kabel Pilin Redam Noise Magnetik." },
    { "en": "Apa Itu Differential Signaling?", "id": "Kirim Data Beda Tegangan Dua Kabel." },
    { "en": "Apa Keunggulan Differential Signaling?", "id": "Sangat Tahan Terhadap Noise Common-mode." },
    { "en": "Apa Itu RS-485 Termination?", "id": "Resistor 120 Ohm Di Ujung." },
    { "en": "Apa Itu Ground Loop Problem?", "id": "Beda Potensial Ground Timbul Arus." },
    { "en": "Cara Mengatasi Ground Loop?", "id": "Gunakan Isolator Optik Atau Trafo." },
    { "en": "Apa Itu Bootloader Arduino?", "id": "Program Awal Upload Sketch USB." },
    { "en": "Apa Fungsi Fuse Bit AVR?", "id": "Konfigurasi Hardware Chip Mikrokontroler." },
    { "en": "Apa Fungsi Pin RESET?", "id": "Restart Program Dari Awal." },
    { "en": "Apa Itu Brown-out Reset?", "id": "Reset Saat Tegangan Suplai Turun." },
    { "en": "Apa Itu Watchdog Timer?", "id": "Reset Jika Program Macet (Hang)." },
    { "en": "Apa Fungsi Crystal Oscillator?", "id": "Sumber Detak Jantung Sistem Digital." },
    { "en": "Apa Itu Internal RC Oscillator?", "id": "Sumber Detak Internal Kurang Akurat." },
    { "en": "Apa Itu RTC (Real Time Clock)?", "id": "Modul Jam Waktu Nyata Akurat." },
    { "en": "Apa Fungsi Baterai CMOS CR2032?", "id": "Jaga Waktu RTC Tetap Jalan." },
    { "en": "Apa Itu EEPROM Memory?", "id": "Simpan Data Permanen Kapasitas Kecil." },
    { "en": "Apa Itu Flash Memory?", "id": "Simpan Program Kode Mikrokontroler." },
    { "en": "Apa Itu SRAM Memory?", "id": "Simpan Variabel Sementara Saat Hidup." },
    { "en": "Apa Itu GPIO (General Purpose I/O)?", "id": "Pin Digital Serbaguna Input Output." },
    { "en": "Apa Itu Logic Level 3.3V?", "id": "Tegangan Standar Modul Modern (ESP)." },
    { "en": "Apa Itu Logic Level 5V?", "id": "Tegangan Standar Arduino Uno Lama." },
    { "en": "Apa Fungsi Level Shifter?", "id": "Hubungkan Logika 3.3V Dan 5V." },
    { "en": "Apa Itu Breadboard?", "id": "Papan Rangkaian Prototipe Tanpa Solder." },
    { "en": "Apa Fungsi Tunnel Diode?", "id": "Osilator Gelombang Mikro Resistansi Negatif." },
    { "en": "Apa Fungsi PIN Diode?", "id": "Saklar RF Dan Attenuator Variabel." },
    { "en": "Apa Fungsi Varactor Diode?", "id": "Kapasitor Variabel Kontrol Tegangan." },
    { "en": "Apa Fungsi Step Recovery Diode?", "id": "Pembangkit Pulsa Harmonisa Sangat Cepat." },
    { "en": "Apa Fungsi Gunn Diode?", "id": "Pembangkit Gelombang Mikro Radar Murah." },
    { "en": "Apa Fungsi IMPATT Diode?", "id": "Osilator Daya Tinggi Frekuensi Tinggi." },
    { "en": "Apa Fungsi Schottky Diode?", "id": "Penyearah Cepat Drop Tegangan Rendah." },
    { "en": "Apa Fungsi TVS Diode?", "id": "Peredam Lonjakan Tegangan Transien." },
    { "en": "Apa Fungsi Photodiode Avalanche?", "id": "Detektor Cahaya Sangat Sensitif." },
    { "en": "Apa Fungsi Laser Diode?", "id": "Sumber Cahaya Koheren Serat Optik." },
    { "en": "Apa Fungsi UJT (Unijunction Transistor)?", "id": "Pemicu SCR Dalam Rangkaian Osilator." },
    { "en": "Apa Fungsi PUT (Programmable UJT)?", "id": "UJT Dengan Tegangan Pemicu Terprogram." },
    { "en": "Apa Fungsi JFET (Junction FET)?", "id": "Penguat Impedansi Input Tinggi." },
    { "en": "Apa Keunggulan JFET Dibanding MOSFET?", "id": "Noise Lebih Rendah Di Audio." },
    { "en": "Apa Fungsi HEMT (High Electron Mobility Transistor)?", "id": "Transistor Super Cepat Komunikasi Satelit." },
    { "en": "Apa Fungsi IGBT (Insulated Gate Bipolar Transistor)?", "id": "Saklar Daya Tinggi Efisiensi Tinggi." },
    { "en": "Apa Fungsi GTO (Gate Turn-Off Thyristor)?", "id": "Thyristor Bisa Mati Lewat Gate." },
    { "en": "Apa Fungsi IGCT (Integrated Gate Commutated Thyristor)?", "id": "Thyristor Modern Efisiensi Sangat Tinggi." },
    { "en": "Apa Fungsi MCT (MOS Controlled Thyristor)?", "id": "Thyristor Dikontrol Tegangan MOS." },
    { "en": "Apa Fungsi Tabung Vakum Triode?", "id": "Penguat Sinyal Analog Kualitas Audiophile." },
    { "en": "Apa Fungsi Tabung Klystron?", "id": "Penguat Gelombang Mikro Daya Besar." },
    { "en": "Apa Fungsi Tabung Magnetron?", "id": "Osilator Gelombang Mikro Oven Radar." },
    { "en": "Apa Fungsi Tabung TWT (Traveling Wave Tube)?", "id": "Penguat Wideband Komunikasi Satelit." },
    { "en": "Apa Fungsi Tabung Nixie?", "id": "Tampilan Angka Neon Klasik." },
    { "en": "Apa Fungsi VFD (Vacuum Fluorescent Display)?", "id": "Layar Hijau Kebiruan Perangkat Audio." },
    { "en": "Apa Fungsi CRT (Cathode Ray Tube)?", "id": "Layar Tabung TV Monitor Lama." },
    { "en": "Apa Fungsi Photomultiplier Tube?", "id": "Detektor Foton Cahaya Sangat Lemah." },
    { "en": "Apa Itu Kabel UTP Cat 7?", "id": "Kabel LAN Shielded 10 Gigabit." },
    { "en": "Apa Itu Kabel UTP Cat 8?", "id": "Kabel LAN Shielded 40 Gigabit." },
    { "en": "Apa Konektor Standar Cat 7?", "id": "GG45 Atau TERA (Bukan RJ45)." },
    { "en": "Apa Fungsi Shielding Kabel STP?", "id": "Cegah Crosstalk Dan Interferensi EMI." },
    { "en": "Apa Itu Kabel Fiber MPO/MTP?", "id": "Konektor Fiber Multi-Core Padat." },
    { "en": "Apa Jumlah Core Konektor MPO 12?", "id": "Dua Belas Serat Optik." },
    { "en": "Apa Itu Kabel DAC (Direct Attach Cable)?", "id": "Kabel Tembaga SFP+ Jarak Pendek." },
    { "en": "Apa Itu Kabel AOC (Active Optical Cable)?", "id": "Kabel Fiber Modul SFP Terintegrasi." },
    { "en": "Apa Fungsi Patch Cord Fiber?", "id": "Kabel Jumper Antar Perangkat Jaringan." },
    { "en": "Apa Warna Kabel Patch Cord OM3?", "id": "Aqua (Biru Muda)." },
    { "en": "Apa Warna Kabel Patch Cord OM4?", "id": "Violet (Ungu)." },
    { "en": "Apa Warna Kabel Patch Cord OS2?", "id": "Kuning (Single Mode)." },
    { "en": "Apa Itu Baterai Nickel Iron (Ni-Fe)?", "id": "Baterai Edison Sangat Awet." },
    { "en": "Apa Itu Baterai Zinc Air?", "id": "Baterai Alat Bantu Dengar." },
    { "en": "Apa Itu Baterai Silver Oxide?", "id": "Baterai Jam Tangan Tegangan Stabil." },
    { "en": "Apa Itu Baterai Lithium Sulfur?", "id": "Baterai Eksperimental Densitas Tinggi." },
    { "en": "Apa Itu Solid State Battery?", "id": "Baterai Elektrolit Padat Aman Ledakan." },
    { "en": "Apa Itu Baterai Flow (Flow Battery)?", "id": "Baterai Cairan Tangki Skala Grid." },
    { "en": "Apa Keunggulan Baterai Flow?", "id": "Kapasitas Tergantung Ukuran Tangki." },
    { "en": "Apa Itu Solder Bridge (Jembatan Timah)?", "id": "Dua Kaki Konslet Terhubung Timah." },
    { "en": "Apa Itu Cold Joint (Solder Dingin)?", "id": "Sambungan Kusam Tidak Menempel Kuat." },
    { "en": "Apa Itu Solder Balling?", "id": "Bola Timah Liar Di PCB." },
    { "en": "Apa Itu Solder Void?", "id": "Rongga Udara Dalam Sambungan Solder." },
    { "en": "Apa Itu Tombstoning Component?", "id": "Komponen Chip Berdiri Sebelah Kaki." },
    { "en": "Apa Itu Head-in-Pillow Defect?", "id": "Bola BGA Tidak Menyatu Pad." },
    { "en": "Apa Itu Popcorn Effect Chip?", "id": "IC Retak Akibat Uap Air." },
    { "en": "Apa Itu Delamination PCB?", "id": "Lapisan PCB Terkelupas Akibat Panas." },
    { "en": "Apa Itu Pad Lifting?", "id": "Pad Tembaga Terlepas Dari Papan." },
    { "en": "Apa Itu Trace Cut?", "id": "Jalur Tembaga Putus Tidak Sengaja." },
    { "en": "Apa Fungsi Epoxy Underfill?", "id": "Lem Penguat Mekanis Chip BGA." },
    { "en": "Apa Fungsi Potting Compound?", "id": "Cor Resin Pelindung Modul Elektronik." },
    { "en": "Apa Itu Kapton Tape?", "id": "Isolasi Polimida Tahan Panas Tinggi." },
    { "en": "Apa Itu Thermal Tape?", "id": "Selotip Perekat Penghantar Panas." },
    { "en": "Apa Itu Copper Tape?", "id": "Selotip Tembaga Perisai EMI." },
    { "en": "Apa Itu Aluminium Tape?", "id": "Selotip Aluminium Reflektor Panas." },
    { "en": "Apa Fungsi Ferrite Core Clamp?", "id": "Redam Noise Kabel Tanpa Potong." },
    { "en": "Apa Itu Heat Shrink Ratio 4:1?", "id": "Penyusutan Diameter Sangat Besar." },
    { "en": "Apa Itu Heat Shrink Adhesive Lined?", "id": "Selongsong Bakar Ada Lem Dalam." },
    { "en": "Apa Fungsi Breadboard Power Module?", "id": "Suplai 3.3V/5V Ke Breadboard." },
    { "en": "Apa Fungsi Logic Probe Tester?", "id": "Cek Status Logika High/Low/Pulse." },
    { "en": "Apa Fungsi IC Extractor?", "id": "Alat Cabut IC Dari Soket." },
    { "en": "Apa Fungsi Vacuum Pen?", "id": "Alat Angkat Komponen SMD." },
    { "en": "Apa Fungsi PCB Holder?", "id": "Penjepit Papan PCB Saat Solder." },
    { "en": "Apa Fungsi Smoke Absorber?", "id": "Penyedot Asap Solder Beracun." },
    { "en": "Apa Itu Sensor LVDT?", "id": "Ukur Pergeseran Linear Sangat Presisi." },
    { "en": "Apa Itu Sensor Strain Gauge?", "id": "Ukur Regangan Benda Akibat Gaya." },
    { "en": "Apa Itu Sensor Piezoelectric?", "id": "Ukur Getaran Atau Tekanan Dinamis." },
    { "en": "Apa Itu Sensor Pyroelectric (PIR)?", "id": "Deteksi Radiasi Panas (Gerak Manusia)." },
    { "en": "Apa Itu Sensor Hall Effect Linear?", "id": "Ukur Kekuatan Medan Magnet." },
    { "en": "Apa Itu Sensor Reed Switch?", "id": "Saklar Magnetik Kontak Kering." },
    { "en": "Apa Itu Sensor Eddy Current?", "id": "Deteksi Retak Atau Posisi Logam." },
    { "en": "Apa Itu Sensor Ultrasonic Level?", "id": "Ukur Tinggi Cairan Tanpa Sentuh." },
    { "en": "Apa Itu Sensor Radar Level?", "id": "Ukur Level Cairan Berbusa/Uap." },
    { "en": "Apa Itu Sensor Capacitive Level?", "id": "Ukur Level Cairan Non-konduktif." },
    { "en": "Apa Itu Sensor pH Tanah?", "id": "Ukur Keasaman Tanah Pertanian." },
    { "en": "Apa Itu Sensor NPK Tanah?", "id": "Ukur Kandungan Nitrogen Fosfor Kalium." },
    { "en": "Apa Itu Sensor CO2 NDIR?", "id": "Deteksi Gas CO2 Inframerah Non-dispersif." },
    { "en": "Apa Itu Sensor PM2.5?", "id": "Deteksi Partikel Debu Halus Udara." },
    { "en": "Apa Itu Sensor VOC?", "id": "Deteksi Senyawa Organik Mudah Menguap." },
    { "en": "Apa Itu Sensor Geiger Counter?", "id": "Deteksi Radiasi Nuklir (Radioaktif)." },
    { "en": "Apa Itu Sensor Load Cell S-Beam?", "id": "Sensor Berat Gantung Bentuk S." },
    { "en": "Apa Itu Sensor Load Cell Single Point?", "id": "Sensor Timbangan Duduk Platform." },
    { "en": "Apa Itu Sensor Torque Rotary?", "id": "Ukur Torsi Poros Berputar." },
    { "en": "Apa Itu Sensor Inclinometer?", "id": "Ukur Sudut Kemiringan Gravitasi." },
    { "en": "Apa Itu Sensor Accelerometer MEMS?", "id": "Ukur Percepatan Tiga Sumbu." },
    { "en": "Apa Itu Sensor Gyroscope MEMS?", "id": "Ukur Kecepatan Sudut Rotasi." },
    { "en": "Apa Itu Sensor Magnetometer MEMS?", "id": "Ukur Arah Medan Magnet Bumi." },
    { "en": "Apa Itu IMU 9-DOF?", "id": "Gabungan Accel, Gyro, Magnetometer." },
    { "en": "Apa Itu AHRS System?", "id": "Sistem Referensi Arah Dan Sikap." },
    { "en": "Apa Itu Sensor LiDAR?", "id": "Pemetaan Jarak Menggunakan Laser." },
    { "en": "Apa Itu Sensor ToF (Time of Flight)?", "id": "Ukur Jarak Waktu Tempuh Cahaya." },
    { "en": "Apa Fungsi Register DDR Pada AVR?", "id": "Atur Arah Data Input Output." },
    { "en": "Apa Fungsi Register PORT Pada AVR?", "id": "Tulis Logika High Low Output." },
    { "en": "Apa Fungsi Register PIN Pada AVR?", "id": "Baca Logika Sinyal Masukan Pin." },
    { "en": "Apa Itu ADC Prescaler AVR?", "id": "Pembagi Clock Untuk Kecepatan ADC." },
    { "en": "Apa Itu ISR (Interrupt Service Routine)?", "id": "Fungsi Eksekusi Saat Interupsi Terjadi." },
    { "en": "Apa Fungsi Global Interrupt Enable?", "id": "Izinkan Semua Interupsi Berjalan." },
    { "en": "Apa Itu Volatile Variable?", "id": "Variabel Berubah Di Luar Program Utama." },
    { "en": "Apa Fungsi Pin AREF Arduino?", "id": "Referensi Tegangan Eksternal ADC." },
    { "en": "Apa Tegangan Referensi Internal Arduino Uno?", "id": "1.1 Volt." },
    { "en": "Apa Itu PROGMEM Arduino?", "id": "Simpan Data Di Flash Memory." },
    { "en": "Apa Fungsi EEPROM.put()?", "id": "Simpan Data Objek Ke EEPROM." },
    { "en": "Apa Fungsi EEPROM.get()?", "id": "Ambil Data Objek Dari EEPROM." },
    { "en": "Apa Itu Bootloader Optiboot?", "id": "Bootloader Ringan Arduino Uno Modern." },
    { "en": "Apa Warna Jacket Kabel Fiber OM1?", "id": "Oranye (62.5 Mikron)." },
    { "en": "Apa Warna Jacket Kabel Fiber OM2?", "id": "Oranye (50 Mikron)." },
    { "en": "Apa Warna Jacket Kabel Fiber OM3?", "id": "Aqua (Biru Muda Laser Optimized)." },
    { "en": "Apa Warna Jacket Kabel Fiber OM4?", "id": "Violet (Ungu Laser Optimized)." },
    { "en": "Apa Warna Jacket Kabel Fiber OM5?", "id": "Lime Green (Hijau Kapur)." },
    { "en": "Apa Warna Jacket Kabel Fiber OS1?", "id": "Kuning (Single Mode Indoor)." },
    { "en": "Apa Warna Jacket Kabel Fiber OS2?", "id": "Kuning (Single Mode Outdoor)." },
    { "en": "Apa Itu Konektor Fiber LC Duplex?", "id": "Dua Konektor LC Gabung Satu." },
    { "en": "Apa Itu Konektor Fiber MPO 12?", "id": "Konektor Multi Fiber 12 Core." },
    { "en": "Apa Itu Kabel DAC SFP+?", "id": "Kabel Tembaga Direct Attach 10G." },
    { "en": "Apa Itu Kabel AOC SFP+?", "id": "Kabel Optik Aktif Terintegrasi." },
    { "en": "Apa Tegangan Nominal Sel Li-HV?", "id": "3.8 Volt (High Voltage)." },
    { "en": "Apa Tegangan Penuh Sel Li-HV?", "id": "4.35 Volt." },
    { "en": "Apa Tegangan Nominal Baterai NiZn?", "id": "1.6 Volt." },
    { "en": "Apa Itu Baterai 18650 Protected?", "id": "Ada Sirkuit Proteksi Kutub Negatif." },
    { "en": "Apa Itu Baterai 18650 Unprotected?", "id": "Sel Telanjang Tanpa Proteksi PCB." },
    { "en": "Apa Itu CID (Current Interrupt Device)?", "id": "Pengaman Tekanan Internal Baterai 18650." },
    { "en": "Apa Itu PTC Pada Baterai 18650?", "id": "Pembatas Arus Saat Suhu Panas." },
    { "en": "Apa Itu Kapasitas Nominal Baterai?", "id": "Kapasitas Tertulis Di Label." },
    { "en": "Apa Itu Kapasitas Aktual Baterai?", "id": "Kapasitas Hasil Tes Discharge." },
    { "en": "Apa Itu Peukert Effect Baterai?", "id": "Kapasitas Turun Jika Beban Berat." },
    { "en": "Apa Itu Battery Memory Effect?", "id": "Kapasitas Berkurang Jika Cas Tanggung." },
    { "en": "Apa Baterai Yang Punya Memory Effect?", "id": "NiCd Dan Sedikit Di NiMH." },
    { "en": "Apa Itu Via Tenting PCB?", "id": "Lubang Via Tertutup Solder Mask." },
    { "en": "Apa Itu Via Plugging PCB?", "id": "Lubang Via Diisi Resin Epoxy." },
    { "en": "Apa Itu Via In Pad?", "id": "Lubang Via Tepat Di Pad." },
    { "en": "Apa Itu Buried Via?", "id": "Via Di Lapisan Dalam Saja." },
    { "en": "Apa Itu Blind Via?", "id": "Via Dari Luar Ke Dalam." },
    { "en": "Apa Itu Surface Finish OSP?", "id": "Pelapis Organik Tembaga Polos." },
    { "en": "Apa Itu Surface Finish Immersion Silver?", "id": "Pelapis Perak Datar Solder Bagus." },
    { "en": "Apa Itu Surface Finish Hard Gold?", "id": "Emas Tebal Untuk Konektor Gesek." },
    { "en": "Apa Itu Carbon Ink PCB?", "id": "Tinta Karbon Untuk Kontak Tombol." },
    { "en": "Apa Itu Peelable Mask PCB?", "id": "Pelindung Sementara Bisa Dikelupas." },
    { "en": "Apa Itu Impedance Control PCB?", "id": "Lebar Jalur Diatur Sesuai Frekuensi." },
    { "en": "Apa Itu Rogers PCB?", "id": "Bahan PCB Frekuensi Tinggi Keramik." },
    { "en": "Apa Fungsi Kapasitor Snubber?", "id": "Redam Lonjakan Tegangan Switching." },
    { "en": "Apa Fungsi Kapasitor Feedthrough?", "id": "Filter Frekuensi Tinggi Tembus Dinding." },
    { "en": "Apa Fungsi Kapasitor Trimmer?", "id": "Kapasitor Variabel Setelan Obeng." },
    { "en": "Apa Fungsi Induktor Adjustable?", "id": "Induktor Inti Putar Setel Frekuensi." },
    { "en": "Apa Fungsi Resistor Network (SIP)?", "id": "Deretan Resistor Satu Kaki Gabung." },
    { "en": "Apa Fungsi Thermistor NTC 10D-9?", "id": "Pembatas Arus Inrush Diameter 9mm." },
    { "en": "Apa Fungsi Thermistor PTC Degaussing?", "id": "Hilangkan Magnet Layar Tabung CRT." },
    { "en": "Apa Fungsi Transistor Photo?", "id": "Saklar Cahaya Sensitivitas Tinggi." },
    { "en": "Apa Fungsi Photo Triac?", "id": "Isolasi Pemicu Triac AC." },
    { "en": "Apa Fungsi Photo Darlington?", "id": "Isolasi Sinyal Penguatan Ganda." },
    { "en": "Apa Itu Solid State Relay DC-DC?", "id": "Input DC Output Beban DC." },
    { "en": "Apa Itu Solid State Relay DC-AC?", "id": "Input DC Output Beban AC." },
    { "en": "Apa Itu Zero Cross SSR?", "id": "Aktif Saat Gelombang AC Nol." },
    { "en": "Apa Itu Random Turn-on SSR?", "id": "Aktif Seketika Sinyal Masuk." },
    { "en": "Apa Aplikasi Random Turn-on SSR?", "id": "Kontrol Dimmer Fase AC." },
    { "en": "Apa Fungsi Heatsink SSR?", "id": "Wajib Buang Panas Beban Besar." },
    { "en": "Apa Itu Motor BLDC Sensored?", "id": "Ada Sensor Hall Posisi Rotor." },
    { "en": "Apa Itu Motor BLDC Sensorless?", "id": "Deteksi Posisi Lewat Back EMF." },
    { "en": "Apa Kelebihan BLDC Sensored?", "id": "Start Halus Torsi Rendah Bagus." },
    { "en": "Apa Kelebihan BLDC Sensorless?", "id": "Kabel Sedikit Tahan Lingkungan Kasar." },
    { "en": "Apa Itu KV Motor Brushless?", "id": "Konstanta Kecepatan (RPM per Volt)." },
    { "en": "Apa Itu Torsi Cogging?", "id": "Gerakan Patah-patah Saat Putar Manual." },
    { "en": "Apa Itu Slotless Motor?", "id": "Stator Tanpa Gigi Putaran Halus." },
    { "en": "Apa Itu Coreless Motor?", "id": "Rotor Tanpa Besi Sangat Ringan." },
    { "en": "Apa Fungsi Load Cell Amplifier?", "id": "Perkuat Sinyal Jembatan Timbang." },
    { "en": "Apa Chip Load Cell Populer?", "id": "HX711 (24-bit ADC)." },
    { "en": "Apa Warna Kabel Excitation Load Cell?", "id": "Merah (+) Dan Hitam (-)." },
    { "en": "Apa Warna Kabel Signal Load Cell?", "id": "Hijau (+) Dan Putih (-)." },
    { "en": "Apa Itu Strain Gauge Rosette?", "id": "Susunan Gauge Ukur Arah Regangan." },
    { "en": "Apa Itu Gauge Factor?", "id": "Sensitivitas Strain Gauge Terhadap Regangan." },
    { "en": "Apa Fungsi Solenoid Valve NC?", "id": "Tertutup Saat Tidak Ada Listrik." },
    { "en": "Apa Fungsi Solenoid Valve NO?", "id": "Terbuka Saat Tidak Ada Listrik." },
    { "en": "Apa Itu Solenoid Valve Latching?", "id": "Kunci Posisi Dengan Pulsa Singkat." },
    { "en": "Apa Fungsi Pilot Operated Valve?", "id": "Pakai Tekanan Fluida Bantu Buka." },
    { "en": "Apa Syarat Pilot Operated Valve?", "id": "Perlu Tekanan Minimal Untuk Kerja." },
    { "en": "Apa Itu Direct Acting Valve?", "id": "Solenoid Langsung Tarik Katup." },
    { "en": "Apa Kelebihan Direct Acting Valve?", "id": "Bisa Kerja Di Tekanan Nol." },
    { "en": "Apa Itu Motor Stepper NEMA 17?", "id": "Ukuran Frame 1.7 Inci Standar." },
    { "en": "Apa Itu Motor Stepper NEMA 23?", "id": "Ukuran Frame 2.3 Inci Kuat." },
    { "en": "Apa Itu Step Angle 1.8 Derajat?", "id": "200 Langkah Per Satu Putaran." },
    { "en": "Apa Itu Step Angle 0.9 Derajat?", "id": "400 Langkah Per Satu Putaran." },
    { "en": "Apa Driver Stepper Silent?", "id": "TMC2208 Atau TMC2209." },
    { "en": "Apa Fitur StealthChop TMC?", "id": "Gerakan Motor Sangat Hening." },
    { "en": "Apa Fitur StallGuard TMC?", "id": "Deteksi Motor Macet Tanpa Sensor." },
    { "en": "Apa Fungsi Kabel Encoder Shielded?", "id": "Cegah Noise Ganggu Sinyal Posisi." },
    { "en": "Apa Itu Kabel Chainflex?", "id": "Kabel Tahan Gerakan Tekuk Berulang." },
    { "en": "Apa Itu Drag Chain Kabel?", "id": "Rantai Pelindung Kabel Bergerak." },
    { "en": "Apa Fungsi Emergency Stop Mushroom?", "id": "Tombol Jamur Stop Darurat." },
    { "en": "Apa Kontak Standar Emergency Stop?", "id": "NC (Normally Closed)." },
    { "en": "Apa Fungsi Safety Relay Unit?", "id": "Monitor Sirkuit E-Stop Ganda." },
    { "en": "Apa Kategori Safety CAT 4?", "id": "Sistem Redundan Deteksi Kesalahan Sendiri." },
    { "en": "Apa Itu Kontaktor Kategori AC-1?", "id": "Beban Resistif Non-Induktif (Pemanas)." },
    { "en": "Apa Itu Kontaktor Kategori AC-3?", "id": "Start Stop Motor Sangkar Tupai." },
    { "en": "Apa Itu Kontaktor Kategori AC-4?", "id": "Beban Motor Inching Dan Plugging." },
    { "en": "Apa Itu Kontaktor Kategori DC-1?", "id": "Beban Resistif Arus Searah." },
    { "en": "Apa Itu Kontaktor Kategori DC-3?", "id": "Start Stop Motor DC Shunt." },
    { "en": "Apa Itu Sekering Tipe gG?", "id": "Sekering Penggunaan Umum Kabel Line." },
    { "en": "Apa Itu Sekering Tipe aM?", "id": "Sekering Khusus Back-up Motor." },
    { "en": "Apa Itu Sekering Tipe gR?", "id": "Sekering Semikonduktor Respon Cepat." },
    { "en": "Apa Itu Sekering HRC Size 00?", "id": "Ukuran Fisik Sekering Pisau Kecil." },
    { "en": "Apa Itu Sekering HRC Size 3?", "id": "Ukuran Fisik Sekering Pisau Besar." },
    { "en": "Apa Itu Work Envelope Robot?", "id": "Ruang Jangkauan Gerak Lengan Robot." },
    { "en": "Apa Itu Payload Capacity Robot?", "id": "Beban Maksimal Yang Bisa Diangkat." },
    { "en": "Apa Itu Repeatability Robot?", "id": "Akurasi Kembali Ke Posisi Sama." },
    { "en": "Apa Itu Joint Space Robot?", "id": "Koordinat Berdasarkan Sudut Sendi." },
    { "en": "Apa Itu Cartesian Space Robot?", "id": "Koordinat Berdasarkan Posisi XYZ Dunia." },
    { "en": "Apa Itu Compliance Dalam Robotika?", "id": "Kemampuan Robot Menyerap Gaya Luar." },
    { "en": "Apa Itu Teach Pendant Robot?", "id": "Remot Kontrol Pemrograman Manual Robot." },
    { "en": "Apa Itu Hysteresis Sensor?", "id": "Beda Output Saat Input Naik Turun." },
    { "en": "Apa Itu Linearity Error Sensor?", "id": "Penyimpangan Dari Garis Lurus Ideal." },
    { "en": "Apa Itu Sensitivity Drift?", "id": "Perubahan Sensitivitas Akibat Suhu Waktu." },
    { "en": "Apa Itu Response Time Sensor?", "id": "Waktu Sensor Bereaksi Terhadap Perubahan." },
    { "en": "Apa Itu Span Error Sensor?", "id": "Kesalahan Pada Nilai Maksimum Ukur." },
    { "en": "Apa Itu Zero Offset Error?", "id": "Kesalahan Baca Nilai Saat Nol." },
    { "en": "Apa Itu ASI Bus Cable Color?", "id": "Kabel Kuning Pipih (Data Power)." },
    { "en": "Apa Itu Foundation Fieldbus H1?", "id": "Jaringan Digital Level Instrumen Lapangan." },
    { "en": "Apa Itu Foundation Fieldbus HSE?", "id": "High Speed Ethernet Untuk Backbone." },
    { "en": "Apa Kecepatan Profibus DP?", "id": "Hingga 12 Megabit Per Detik." },
    { "en": "Apa Itu DeviceNet Trunk Line?", "id": "Kabel Utama Jaringan Tebal." },
    { "en": "Apa Itu DeviceNet Drop Line?", "id": "Kabel Cabang Ke Perangkat Tipis." },
    { "en": "Apa Itu Safety PLC?", "id": "PLC Khusus Standar Keamanan SIL." },
    { "en": "Apa Itu Two Hand Control?", "id": "Wajib Tekan Dua Tombol Bersamaan." },
    { "en": "Apa Itu Safety Interlock Switch?", "id": "Saklar Pintu Pengaman Mesin Berbahaya." },
    { "en": "Apa Itu Enabling Switch?", "id": "Tombol Izin Gerak Mode Manual." },
    { "en": "Apa Itu IP Rating IP20?", "id": "Tahan Jari Tidak Tahan Air." },
    { "en": "Apa Itu IP Rating IP44?", "id": "Tahan Benda 1mm Cipratan Air." },
    { "en": "Apa Itu Cable Tray Ladder?", "id": "Tray Kabel Bentuk Tangga Udara." },
    { "en": "Apa Itu Cable Tray Solid?", "id": "Tray Kabel Tertutup Tanpa Lubang." },
    { "en": "Apa Itu Cable Tray Perforated?", "id": "Tray Kabel Dengan Lubang Ventilasi." },
    { "en": "Apa Itu Cable Cleat?", "id": "Klem Kabel Kuat Tahan Short." },
    { "en": "Apa Itu Busduct Sandwich?", "id": "Busbar Padat Tanpa Celah Udara." },
    { "en": "Apa Itu Busduct Air Insulated?", "id": "Busbar Dengan Jarak Isolasi Udara." },
    { "en": "Apa Itu CT Class X?", "id": "CT Proteksi Khusus Diferensial (PX)." },
    { "en": "Apa Itu CT Class 0.5S?", "id": "Akurasi Tinggi Pada Arus Rendah." },
    { "en": "Apa Itu Remanence Flux CT?", "id": "Magnet Sisa Setelah Arus Hilang." },
    { "en": "Apa Itu Baterai OpzV?", "id": "Baterai Tubular Gel Umur Panjang." },
    { "en": "Apa Itu Baterai OpzS?", "id": "Baterai Tubular Basah Umur Panjang." },
    { "en": "Apa Itu Baterai NiCd Pocket Plate?", "id": "Konstruksi Plat Kuat Tahan Banting." },
    { "en": "Apa Itu Solar Panel Amorphous?", "id": "Silikon Non-kristal Fleksibel Efisiensi Rendah." },
    { "en": "Apa Itu Solar Panel HJT?", "id": "Heterojunction Technology Efisiensi Tinggi." },
    { "en": "Apa Itu Solar Panel TOPCon?", "id": "Tunnel Oxide Passivated Contact." },
    { "en": "Apa Itu Clamp-on Ground Tester?", "id": "Ukur Grounding Tanpa Lepas Kabel." },
    { "en": "Apa Itu MCB 2 Pole?", "id": "Putus Fasa Dan Netral Bersamaan." },
    { "en": "Apa Itu MCB 3 Pole?", "id": "Putus Tiga Fasa Sekaligus." },
    { "en": "Apa Itu MCB 4 Pole?", "id": "Putus Tiga Fasa Dan Netral." },
    { "en": "Apa Itu MCCB Fixed Type?", "id": "Setting Arus Trip Tidak Bisa Diubah." },
    { "en": "Apa Itu MCCB Adjustable Type?", "id": "Setting Arus Trip Bisa Diatur." },
    { "en": "Apa Itu Cable Gland Metric?", "id": "Drat Standar Metrik (M20, M25)." },
    { "en": "Apa Itu Cable Gland PG?", "id": "Drat Standar Jerman (PG16, PG21)." },
    { "en": "Apa Itu Cable Gland NPT?", "id": "Drat Pipa Tirus Standar Amerika." },
    { "en": "Apa Itu Cable End Sleeve?", "id": "Nama Lain Dari Ferrule Kabel." },
    { "en": "Apa Itu Mata Solder Conical?", "id": "Ujung Kerucut Lancip Standar." },
    { "en": "Apa Itu Mata Solder Chisel?", "id": "Ujung Pahat Pipih Transfer Panas." },
    { "en": "Apa Itu Mata Solder Bevel?", "id": "Ujung Miring Menampung Timah." },
    { "en": "Apa Itu Mata Solder Knife?", "id": "Ujung Pisau Untuk Kupas Potong." },
    { "en": "Apa Itu PCB Trace Width?", "id": "Lebar Jalur Tembaga Tentukan Arus." },
    { "en": "Apa Itu PCB Clearance?", "id": "Jarak Aman Antar Jalur Tembaga." },
    { "en": "Apa Itu PCB Annular Ring?", "id": "Cincin Tembaga Sekeliling Lubang Bor." },
    { "en": "Apa Itu Solder Mask Bridge?", "id": "Lapisan Hijau Antar Pad IC." },
    { "en": "Apa Itu IC DIP-8?", "id": "Dual Inline Package 8 Kaki." },
    { "en": "Apa Itu IC DIP-14?", "id": "Dual Inline Package 14 Kaki." },
    { "en": "Apa Itu IC DIP-16?", "id": "Dual Inline Package 16 Kaki." },
    { "en": "Apa Itu IC SOP-8?", "id": "Small Outline Package 8 Kaki." },
    { "en": "Apa Itu IC TSSOP?", "id": "Thin Shrink Small Outline Package." },
    { "en": "Apa Itu Resistor 0 Ohm?", "id": "Jumper Kawat Bentuk Resistor." },
    { "en": "Apa Itu Resistor Shunt 0.1 Ohm?", "id": "Resistor Ukur Arus Nilai Kecil." },
    { "en": "Apa Itu Resistor Array 8 Pin?", "id": "4 Resistor Terpisah Atau Bus." },
    { "en": "Apa Itu Kapasitor MLCC?", "id": "Multi Layer Ceramic Capacitor." },
    { "en": "Apa Itu Kapasitor Low ESR?", "id": "Impedansi Rendah Untuk Switching." },
    { "en": "Apa Itu Transistor SOT-23?", "id": "Paket SMD Kecil 3 Kaki." },
    { "en": "Apa Itu Transistor TO-247?", "id": "Paket Daya Besar Plastik Hitam." },
    { "en": "Apa Itu Transistor TO-3?", "id": "Paket Logam Jengkol Jadul." },
    { "en": "Apa Itu Dioda DO-41?", "id": "Paket Dioda Plastik 1 Ampere." },
    { "en": "Apa Itu Dioda DO-214?", "id": "Paket Dioda SMD (SMA/SMB/SMC)." },
    { "en": "Apa Itu Dioda SOD-123?", "id": "Paket Dioda SMD Sangat Kecil." },
    { "en": "Apa Itu LED 3mm?", "id": "Indikator Kecil Panel Elektronik." },
    { "en": "Apa Itu LED 5mm?", "id": "Indikator Standar Paling Umum." },
    { "en": "Apa Itu LED 10mm?", "id": "Indikator Besar Jarak Jauh." },
    { "en": "Apa Itu LED SMD 3528?", "id": "Chip LED Ukuran 3.5x2.8mm." },
    { "en": "Apa Itu LED SMD 5050?", "id": "Chip LED Ukuran 5.0x5.0mm." },
    { "en": "Apa Fungsi Space Vector Modulation?", "id": "Teknik PWM Tiga Fasa Efisien." },
    { "en": "Apa Fungsi Six Step Commutation?", "id": "Metode Komutasi Dasar Motor BLDC." },
    { "en": "Apa Itu Direct Torque Control?", "id": "Kontrol Torsi Motor Tanpa PWM." },
    { "en": "Apa Fungsi Free Wheeling Diode?", "id": "Alirkan Arus Induktif Saat Mati." },
    { "en": "Apa Fungsi Resonant Converter?", "id": "Konverter Switching Zero Voltage/Current." },
    { "en": "Apa Itu Thyristor Phase Control?", "id": "Atur Daya Dengan Potong Gelombang." },
    { "en": "Apa Itu Dual Converter?", "id": "Dua Konverter Back-to-back 4 Kuadran." },
    { "en": "Apa Itu Gain Scheduling PID?", "id": "Ubah Parameter PID Sesuai Kondisi." },
    { "en": "Apa Fungsi Deadband Kontrol?", "id": "Cegah Osilasi Akibat Noise Kecil." },
    { "en": "Apa Output Gerbang AND Jika Input 1, 1?", "id": "Logika 1 (High)." },
    { "en": "Apa Output Gerbang AND Jika Input 1, 0?", "id": "Logika 0 (Low)." },
    { "en": "Apa Output Gerbang OR Jika Input 0, 0?", "id": "Logika 0 (Low)." },
    { "en": "Apa Output Gerbang OR Jika Input 1, 0?", "id": "Logika 1 (High)." },
    { "en": "Apa Output Gerbang NAND Jika Input 1, 1?", "id": "Logika 0 (Low)." },
    { "en": "Apa Output Gerbang NOR Jika Input 0, 0?", "id": "Logika 1 (High)." },
    { "en": "Apa Output Gerbang XOR Jika Input Berbeda?", "id": "Logika 1 (High)." },
    { "en": "Apa Output Gerbang XOR Jika Input Sama?", "id": "Logika 0 (Low)." },
    { "en": "Apa Fungsi Pin RESET Arduino?", "id": "Restart Program Dari Awal." },
    { "en": "Apa Fungsi Pin VIN Arduino?", "id": "Input Tegangan Sumber Eksternal." },
    { "en": "Apa Fungsi Pin IOREF Arduino?", "id": "Referensi Level Tegangan Logic Board." },
    { "en": "Apa Fungsi Pin SDA Arduino?", "id": "Data Serial Komunikasi I2C." },
    { "en": "Apa Fungsi Pin SCL Arduino?", "id": "Clock Serial Komunikasi I2C." },
    { "en": "Apa Fungsi Pin SS (Slave Select)?", "id": "Aktifkan Perangkat Slave SPI." },
    { "en": "Apa Fungsi Pin MOSI?", "id": "Master Out Slave In SPI." },
    { "en": "Apa Fungsi Pin MISO?", "id": "Master In Slave Out SPI." },
    { "en": "Apa Fungsi Pin SCK?", "id": "Serial Clock Komunikasi SPI." },
    { "en": "Apa Kode Warna Resistor 100 Ohm?", "id": "Cokelat Hitam Cokelat Emas." },
    { "en": "Apa Kode Warna Resistor 220 Ohm?", "id": "Merah Merah Cokelat Emas." },
    { "en": "Apa Kode Warna Resistor 330 Ohm?", "id": "Oranye Oranye Cokelat Emas." },
    { "en": "Apa Kode Warna Resistor 1k Ohm?", "id": "Cokelat Hitam Merah Emas." },
    { "en": "Apa Kode Warna Resistor 2k2 Ohm?", "id": "Merah Merah Merah Emas." },
    { "en": "Apa Kode Warna Resistor 4k7 Ohm?", "id": "Kuning Ungu Merah Emas." },
    { "en": "Apa Kode Warna Resistor 10k Ohm?", "id": "Cokelat Hitam Oranye Emas." },
    { "en": "Apa Kode Warna Resistor 100k Ohm?", "id": "Cokelat Hitam Kuning Emas." },
    { "en": "Apa Kode Warna Resistor 1M Ohm?", "id": "Cokelat Hitam Hijau Emas." },
    { "en": "Apa Toleransi Gelang Warna Emas?", "id": "Lima Persen." },
    { "en": "Apa Toleransi Gelang Warna Perak?", "id": "Sepuluh Persen." },
    { "en": "Apa Arti Kode Kapasitor 102?", "id": "1 Nano Farad (1nF)." },
    { "en": "Apa Arti Kode Kapasitor 103?", "id": "10 Nano Farad (10nF)." },
    { "en": "Apa Arti Kode Kapasitor 104?", "id": "100 Nano Farad (100nF)." },
    { "en": "Apa Arti Kode Kapasitor 22?", "id": "22 Pico Farad (22pF)." },
    { "en": "Apa Arti Kode Kapasitor 473?", "id": "47 Nano Farad (47nF)." },
    { "en": "Apa Arti Kode Kapasitor 2A104?", "id": "100nF Tegangan 100 Volt." },
    { "en": "Apa Fungsi Dioda Zener 5V1?", "id": "Regulasi Tegangan 5.1 Volt." },
    { "en": "Apa Fungsi Dioda 1N4148?", "id": "Dioda Switching Sinyal Cepat." },
    { "en": "Apa Fungsi Dioda 1N4007?", "id": "Penyearah Umum 1 Ampere." },
    { "en": "Apa Fungsi Dioda 1N5408?", "id": "Penyearah Daya 3 Ampere." },
    { "en": "Apa Fungsi Dioda Schottky 1N5819?", "id": "Penyearah Cepat Drop Rendah." },
    { "en": "Apa Fungsi Transistor BC547?", "id": "Transistor NPN Sinyal Kecil." },
    { "en": "Apa Fungsi Transistor BC557?", "id": "Transistor PNP Sinyal Kecil." },
    { "en": "Apa Fungsi Transistor 2N2222?", "id": "Transistor NPN Switching Umum." },
    { "en": "Apa Fungsi Transistor TIP31?", "id": "Transistor Power NPN Menengah." },
    { "en": "Apa Fungsi Transistor TIP32?", "id": "Transistor Power PNP Menengah." },
    { "en": "Apa Fungsi Transistor TIP120?", "id": "Transistor Darlington NPN Power." },
    { "en": "Apa Fungsi MOSFET IRFZ44N?", "id": "Saklar Daya Arus Besar." },
    { "en": "Apa Fungsi IC Voltage Regulator 7805?", "id": "Output Tegangan 5V Tetap." },
    { "en": "Apa Fungsi IC Voltage Regulator 7812?", "id": "Output Tegangan 12V Tetap." },
    { "en": "Apa Fungsi IC Voltage Regulator LM317?", "id": "Output Tegangan Positif Variabel." },
    { "en": "Apa Fungsi IC Timer NE555?", "id": "Pembangkit Pulsa Dan Timer." },
    { "en": "Apa Fungsi IC Op-Amp LM741?", "id": "Penguat Operasional Single Channel." },
    { "en": "Apa Fungsi IC Op-Amp LM358?", "id": "Penguat Operasional Dual Channel." },
    { "en": "Apa Fungsi IC Op-Amp LM324?", "id": "Penguat Operasional Quad Channel." },
    { "en": "Apa Fungsi IC Driver L293D?", "id": "Driver Motor DC Dual H-Bridge." },
    { "en": "Apa Fungsi IC Driver L298N?", "id": "Driver Motor Daya Lebih Besar." },
    { "en": "Apa Fungsi IC Shift Register 74HC595?", "id": "Serial In Parallel Out." },
    { "en": "Apa Fungsi IC Counter 4017?", "id": "Penghitung Dekade (10 Output)." },
    { "en": "Apa Fungsi Optocoupler PC817?", "id": "Isolasi Sinyal Fototransistor Tunggal." },
    { "en": "Apa Fungsi Modul Relay 5V?", "id": "Saklar AC Kontrol Mikrokontroler." },
    { "en": "Apa Fungsi Modul Step Down LM2596?", "id": "Turunkan Tegangan DC Variabel." },
    { "en": "Apa Fungsi Modul Step Up XL6009?", "id": "Naikkan Tegangan DC Variabel." },
    { "en": "Apa Fungsi Modul Charger TP4056?", "id": "Pengisi Baterai Lithium Ion." },
    { "en": "Apa Fungsi Sensor Suhu LM35?", "id": "Sensor Suhu Analog Linear." },
    { "en": "Apa Fungsi Sensor Suhu DHT11?", "id": "Suhu Dan Kelembapan Digital." },
    { "en": "Apa Fungsi Sensor Jarak HC-SR04?", "id": "Ukur Jarak Gelombang Ultrasonik." },
    { "en": "Apa Fungsi Sensor Gerak PIR?", "id": "Deteksi Gerak Tubuh Manusia." },
    { "en": "Apa Fungsi Sensor Gas MQ-2?", "id": "Deteksi Asap Dan Gas LPG." },
    { "en": "Apa Fungsi Sensor Cahaya LDR?", "id": "Resistansi Berubah Sesuai Cahaya." },
    { "en": "Apa Fungsi Sensor Suara KY-037?", "id": "Deteksi Kebisingan Suara Microphone." },
    { "en": "Apa Fungsi Sensor Getar SW-420?", "id": "Deteksi Guncangan Atau Getaran." },
    { "en": "Apa Fungsi Sensor Hujan Raindrop?", "id": "Deteksi Tetesan Air Hujan." },
    { "en": "Apa Fungsi Sensor Soil Moisture?", "id": "Ukur Kelembapan Tanah Pot." },
    { "en": "Apa Fungsi Modul Bluetooth HC-05?", "id": "Komunikasi Wireless Master Slave." },
    { "en": "Apa Fungsi Modul WiFi ESP8266?", "id": "Koneksi Mikrokontroler Ke Internet." },
    { "en": "Apa Fungsi Modul GSM SIM800L?", "id": "Komunikasi Seluler SMS GPRS." },
    { "en": "Apa Fungsi Modul GPS NEO-6M?", "id": "Penentu Lokasi Koordinat Satelit." },
    { "en": "Apa Fungsi Modul RTC DS3231?", "id": "Jam Waktu Nyata Presisi." },
    { "en": "Apa Fungsi Modul SD Card?", "id": "Penyimpanan Data Logging Eksternal." },
    { "en": "Apa Fungsi LCD 16x2 I2C?", "id": "Tampilan Karakter Hemat Pin." },
    { "en": "Apa Fungsi OLED 0.96 Inch?", "id": "Layar Grafis Kecil Tajam." },
    { "en": "Apa Fungsi Servo Motor SG90?", "id": "Gerakan Sudut Presisi Kecil." },
    { "en": "Apa Fungsi Stepper Motor 28BYJ-48?", "id": "Motor Langkah Kecil Gearbox." },
    { "en": "Apa Fungsi Breadboard 830 Point?", "id": "Papan Prototipe Tanpa Solder." },
    { "en": "Apa Fungsi Kabel Jumper Male-Male?", "id": "Koneksi Antar Lubang Breadboard." },
    { "en": "Apa Fungsi Kabel Jumper Male-Female?", "id": "Koneksi Pin Header Breadboard." },
    { "en": "Apa Fungsi Multimeter Digital Mode Buzzer?", "id": "Cek Kabel Putus (Kontinuitas)." },
    { "en": "Apa Fungsi Solder 40 Watt?", "id": "Pemanas Timah Komponen Elektronik." },
    { "en": "Apa Fungsi Desoldering Pump Atraktor?", "id": "Sedot Timah Cair Solderan." },
    { "en": "Apa Fungsi Flux Solder?", "id": "Memudahkan Timah Menempel Rapi." },
    { "en": "Apa Fungsi Pinset Lurus?", "id": "Memegang Komponen Kecil Sempit." },
    { "en": "Apa Fungsi Tang Potong?", "id": "Memotong Kaki Komponen Berlebih." },
    { "en": "Apa Fungsi Obeng Plus Kecil?", "id": "Membuka Baut Mainan Elektronik." },
    { "en": "Apa Fungsi Baterai 9V Kotak?", "id": "Sumber Daya Multimeter/Arduino." },
    { "en": "Apa Fungsi Baterai 18650?", "id": "Sumber Daya Kapasitas Besar." },
    { "en": "Apa Fungsi Holder Baterai?", "id": "Tempat Baterai Agar Terhubung." },
    { "en": "Apa Fungsi Saklar Toggle?", "id": "Saklar Tuas On Off." },
    { "en": "Apa Fungsi Push Button Tactile?", "id": "Tombol Tekan Sesaat Reset." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
