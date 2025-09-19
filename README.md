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


  { "en": "Apa Pengertian Otonomi Daerah?", "id": "Kewenangan Mengatur Urusan Pemerintahan Sendiri." },
  { "en": "Dasar Hukum Otonomi Daerah?", "id": "Undang-Undang Dasar (UUD) 1945 Pasal 18." },
  { "en": "Prinsip Otonomi Daerah?", "id": "Luas, Nyata, Dan Bertanggung Jawab." },
  { "en": "Siapa Penyelenggara Pemerintahan Daerah?", "id": "Pemerintah Daerah Dan Dewan Perwakilan Rakyat Daerah (DPRD)." },
  { "en": "Apa Tujuan Otonomi Daerah?", "id": "Meningkatkan Kesejahteraan Masyarakat Dan Pelayanan Umum." },
  { "en": "Apa Itu Desentralisasi?", "id": "Penyerahan Wewenang Pemerintah Pusat Ke Daerah." },
  { "en": "Jenis Urusan Pemerintahan Daerah?", "id": "Urusan Wajib Dan Urusan Pilihan." },
  { "en": "Sumber Pendapatan Asli Daerah (PAD)?", "id": "Pajak, Retribusi, Dan Hasil Usaha Daerah." },
  { "en": "Apa Itu Peraturan Daerah (Perda)?", "id": "Peraturan Perundang-undangan Tingkat Daerah." },
  { "en": "Siapa Yang Menetapkan Peraturan Daerah (Perda)?", "id": "Dewan Perwakilan Rakyat Daerah (DPRD) Dan Kepala Daerah." },
  { "en": "Asas Otonomi Daerah?", "id": "Desentralisasi, Dekonsentrasi, Dan Tugas Pembantuan." },
  { "en": "Apa Itu Dekonsentrasi?", "id": "Pelimpahan Wewenang Dari Atasan Ke Bawahan." },
  { "en": "Apa Itu Tugas Pembantuan?", "id": "Penugasan Pemerintah Pusat Kepada Daerah." },
  { "en": "Apa Hak Daerah?", "id": "Mengatur Dan Mengurus Urusan Pemerintahan Sendiri." },
  { "en": "Kewajiban Daerah Dalam Otonomi?", "id": "Meningkatkan Kualitas Hidup Dan Kesejahteraan Masyarakat." },
  { "en": "Bentuk Negara Indonesia?", "id": "Negara Kesatuan Republik Indonesia (NKRI)." },
  { "en": "Sistem Pemerintahan Indonesia?", "id": "Sistem Pemerintahan Presidensial." },
  { "en": "Siapa Kepala Negara Indonesia?", "id": "Presiden Republik Indonesia." },
  { "en": "Siapa Kepala Pemerintahan Indonesia?", "id": "Presiden Republik Indonesia." },
  { "en": "Apa Lembaga Legislatif Indonesia?", "id": "Majelis Permusyawaratan Rakyat (MPR) Dan Dewan Perwakilan Rakyat (DPR)." },
  { "en": "Apa Lembaga Eksekutif Indonesia?", "id": "Presiden, Wakil Presiden, Dan Menteri." },
  { "en": "Apa Lembaga Yudikatif Indonesia?", "id": "Mahkamah Agung (MA) Dan Mahkamah Konstitusi (MK)." },
  { "en": "Dasar Negara Indonesia?", "id": "Pancasila Dan Undang-Undang Dasar (UUD) 1945." },
  { "en": "Apa Fungsi Dewan Perwakilan Rakyat (DPR)?", "id": "Legislasi, Anggaran, Dan Pengawasan." },
  { "en": "Berapa Masa Jabatan Presiden?", "id": "Lima Tahun Dan Dapat Dipilih Kembali." },
  { "en": "Siapa Yang Memilih Presiden?", "id": "Rakyat Melalui Pemilihan Umum (Pemilu)." },
  { "en": "Apa Tugas Mahkamah Konstitusi (MK)?", "id": "Menguji Undang-Undang Terhadap Undang-Undang Dasar (UUD)." },
  { "en": "Apa Ideologi Negara Indonesia?", "id": "Pancasila Sebagai Dasar Dan Ideologi Negara." },
  { "en": "Apa Itu Sistem Politik?", "id": "Mekanisme Interaksi Antara Pemerintah Dan Masyarakat." },
  { "en": "Apa Ciri Sistem Politik Demokrasi?", "id": "Kedaulatan Rakyat Dan Pemilihan Umum (Pemilu) Bebas." },
  { "en": "Partai Politik Di Indonesia?", "id": "Wadah Partisipasi Politik Warga Negara." },
  { "en": "Fungsi Partai Politik?", "id": "Pendidikan Politik Dan Rekrutmen Kader Pemimpin." },
  { "en": "Apa Itu Pemilihan Umum (Pemilu)?", "id": "Sarana Pelaksanaan Kedaulatan Rakyat." },
  { "en": "Asas Pemilihan Umum (Pemilu) Di Indonesia?", "id": "Langsung, Umum, Bebas, Rahasia, Jujur, Adil." },
  { "en": "Siapa Penyelenggara Pemilihan Umum (Pemilu)?", "id": "Komisi Pemilihan Umum (KPU)." },
  { "en": "Apa Itu Budaya Politik?", "id": "Orientasi Politik Khas Warga Negara." },
  { "en": "Apa Itu Sosialisasi Politik?", "id": "Proses Pembentukan Sikap Dan Orientasi Politik." },
  { "en": "Siapa Aktor Dalam Sistem Politik?", "id": "Lembaga Negara, Partai Politik, Dan Masyarakat." },
  { "en": "Apa Itu Kebijakan Publik?", "id": "Keputusan Pemerintah Untuk Kepentingan Umum." },
  { "en": "Bagaimana Proses Kebijakan Publik?", "id": "Perumusan, Implementasi, Dan Evaluasi Kebijakan." },
  { "en": "Hubungan Pusat Dan Daerah?", "id": "Hubungan Kewenangan, Keuangan, Dan Pengawasan." },
  { "en": "Apa Itu Daerah Otonom?", "id": "Kesatuan Masyarakat Hukum Dengan Batas Wilayah." },
  { "en": "Kepala Daerah Provinsi Disebut?", "id": "Gubernur Dibantu Oleh Wakil Gubernur." },
  { "en": "Kepala Daerah Kabupaten Disebut?", "id": "Bupati Dibantu Oleh Wakil Bupati." },
  { "en": "Kepala Daerah Kota Disebut?", "id": "Wali Kota Dibantu Wakil Wali Kota." },
  { "en": "Bagaimana Pemilihan Kepala Daerah?", "id": "Dipilih Secara Demokratis Melalui Pemilihan Kepala Daerah (Pilkada)." },
  { "en": "Struktur Pemerintahan Daerah Provinsi?", "id": "Gubernur Dan Dewan Perwakilan Rakyat Daerah (DPRD) Provinsi." },
  { "en": "Apa Itu Anggaran Pendapatan Dan Belanja Daerah (APBD)?", "id": "Rencana Keuangan Tahunan Pemerintah Daerah." },
  { "en": "Siapa Yang Menyetujui Anggaran Pendapatan Dan Belanja Daerah (APBD)?", "id": "Dewan Perwakilan Rakyat Daerah (DPRD) Bersama Kepala Daerah." },
  { "en": "Apa Konsekuensi Negara Kesatuan?", "id": "Kedaulatan Tunggal Ada Di Pemerintah Pusat." },
  { "en": "Bentuk Kedaulatan Di Indonesia?", "id": "Kedaulatan Berada Di Tangan Rakyat." },
  { "en": "Apa Itu Trias Politika?", "id": "Pemisahan Kekuasaan Legislatif, Eksekutif, Yudikatif." },
  { "en": "Siapa Pencetus Trias Politika?", "id": "Montesquieu, Filsuf Asal Perancis." },
  { "en": "Apa Itu Check And Balances?", "id": "Prinsip Saling Mengawasi Dan Mengimbangi." },
  { "en": "Tugas Majelis Permusyawaratan Rakyat (MPR)?", "id": "Mengubah Menetapkan Undang-Undang Dasar (UUD), Melantik Presiden." },
  { "en": "Apa Itu Hak Angket Dewan Perwakilan Rakyat (DPR)?", "id": "Hak Melakukan Penyelidikan Kebijakan Pemerintah." },
  { "en": "Apa Itu Hak Interpelasi Dewan Perwakilan Rakyat (DPR)?", "id": "Hak Meminta Keterangan Kepada Pemerintah." },
  { "en": "Tugas Mahkamah Agung (MA)?", "id": "Mengadili Pada Tingkat Kasasi." },
  { "en": "Apa Itu Negara Hukum?", "id": "Segala Aspek Berdasarkan Aturan Hukum." },
  { "en": "Partisipasi Politik Adalah?", "id": "Keterlibatan Warga Dalam Sistem Politik." },
  { "en": "Bentuk Partisipasi Politik?", "id": "Mengikuti Pemilu, Demonstrasi, Dan Diskusi." },
  { "en": "Apa Itu Kelompok Kepentingan?", "id": "Organisasi Memperjuangkan Kepentingan Tertentu." },
  { "en": "Apa Itu Kelompok Penekan?", "id": "Kelompok Mempengaruhi Keputusan Politik Pemerintah." },
  { "en": "Contoh Kelompok Kepentingan?", "id": "Kamar Dagang Dan Industri (KADIN)." },
  { "en": "Faktor Penentu Sistem Politik?", "id": "Sejarah, Budaya, Dan Struktur Sosial." },
  { "en": "Apa Itu Legitimasi Politik?", "id": "Pengakuan Dan Penerimaan Kekuasaan Pemerintah." },
  { "en": "Sumber Legitimasi Politik?", "id": "Hukum, Tradisi, Dan Karisma Pemimpin." },
  { "en": "Pilar Demokrasi Indonesia?", "id": "Pancasila, Undang-Undang Dasar (UUD) 1945, Negara Kesatuan Republik Indonesia (NKRI), Bhinneka Tunggal Ika." },
  { "en": "Pembagian Urusan Pemerintahan?", "id": "Absolut, Konkuren, Dan Pemerintahan Umum." },
  { "en": "Urusan Pemerintahan Absolut?", "id": "Sepenuhnya Wewenang Pemerintah Pusat." },
  { "en": "Contoh Urusan Absolut?", "id": "Politik Luar Negeri, Pertahanan, Keamanan." },
  { "en": "Urusan Pemerintahan Konkuren?", "id": "Urusan Dibagi Pusat Dan Daerah." },
  { "en": "Apa Itu Otonomi Khusus?", "id": "Kewenangan Khusus Diberikan Kepada Daerah Tertentu." },
  { "en": "Contoh Daerah Otonomi Khusus?", "id": "Aceh, Papua, Dan Daerah Khusus Ibukota (DKI) Jakarta." },
  { "en": "Apa Landasan Filosofis Otonomi Daerah?", "id": "Pengakuan Terhadap Keberagaman Bangsa Indonesia." },
  { "en": "Hierarki Peraturan Perundang-undangan?", "id": "Tata Urutan Peraturan Hukum Indonesia." },
  { "en": "Peraturan Tertinggi Di Indonesia?", "id": "Undang-Undang Dasar Negara Republik Indonesia (UUD NRI) 1945." },
  { "en": "Apa Itu Amandemen?", "id": "Perubahan Resmi Dokumen Resmi Atau Catatan." },
  { "en": "Berapa Kali Undang-Undang Dasar (UUD) 1945 Diamandemen?", "id": "Sebanyak Empat Kali Perubahan." },
  { "en": "Lembaga Negara Independen?", "id": "Lembaga Yang Bebas Dari Intervensi." },
  { "en": "Contoh Lembaga Independen?", "id": "Komisi Pemilihan Umum (KPU), Bank Indonesia (BI)." },
  { "en": "Apa Itu Suprastruktur Politik?", "id": "Lembaga-lembaga Politik Formal Negara." },
  { "en": "Apa Itu Infrastruktur Politik?", "id": "Kelompok Masyarakat Berpartisipasi Dalam Politik." },
  { "en": "Unsur Infrastruktur Politik?", "id": "Partai Politik, Ormas, Media Massa." },
  { "en": "Peran Media Massa Politik?", "id": "Menyebarkan Informasi Dan Membentuk Opini Publik." },
  { "en": "Apa Itu Demokrasi Langsung?", "id": "Rakyat Berpartisipasi Langsung Dalam Keputusan." },
  { "en": "Apa Itu Demokrasi Tidak Langsung?", "id": "Rakyat Memilih Wakil Untuk Membuat Keputusan." },
  { "en": "Sistem Kepartaian Di Indonesia?", "id": "Sistem Multipartai Sejak Era Reformasi." },
  { "en": "Fungsi Pengawasan Dewan Perwakilan Rakyat Daerah (DPRD)?", "id": "Mengawasi Pelaksanaan Perda Dan Anggaran Pendapatan Dan Belanja Daerah (APBD)." },
  { "en": "Apa Itu Dana Perimbangan?", "id": "Dana Dari Anggaran Pendapatan Dan Belanja Negara (APBN) Untuk Daerah." },
  { "en": "Jenis Dana Perimbangan?", "id": "Dana Bagi Hasil (DBH), Dana Alokasi Umum (DAU)." },
  { "en": "Tujuan Dana Perimbangan?", "id": "Mengurangi Kesenjangan Fiskal Antar Daerah." },
  { "en": "Apa Itu Konstitusi?", "id": "Hukum Dasar Tertulis Suatu Negara." },
  { "en": "Makna Kedaulatan Rakyat?", "id": "Kekuasaan Tertinggi Ada Di Tangan Rakyat." },
  { "en": "Peran Masyarakat Sipil?", "id": "Mengawasi Kinerja Pemerintah Dan Aparaturnya." },
  { "en": "Apa Itu Oposisi Politik?", "id": "Pihak Penentang Kebijakan Pemerintah Berkuasa." },
  { "en": "Pentingnya Oposisi Dalam Demokrasi?", "id": "Menciptakan Mekanisme Check And Balances." },
  { "en": "Apa Itu Otonomi Materiil?", "id": "Kewenangan Mengatur Urusan Berdasarkan Materi." },
  { "en": "Apa Itu Otonomi Riil?", "id": "Kewenangan Nyata Sesuai Potensi Daerah." },
  { "en": "Apa Itu Otonomi Formil?", "id": "Pembagian Wewenang Berdasarkan Undang-Undang." },
  { "en": "Fungsi Kepala Daerah?", "id": "Memimpin Penyelenggaraan Pemerintahan Daerah." },
  { "en": "Tugas Dewan Perwakilan Rakyat Daerah (DPRD)?", "id": "Membentuk Perda Bersama Kepala Daerah." },
  { "en": "Apa Itu Pilkada Serentak?", "id": "Pemilihan Kepala Daerah Di Beberapa Wilayah." },
  { "en": "Tujuan Pilkada Serentak?", "id": "Efisiensi Anggaran Dan Sinkronisasi Pembangunan." },
  { "en": "Apa Itu Dana Keistimewaan?", "id": "Dana Untuk Daerah Istimewa Yogyakarta (DIY)." },
  { "en": "Hubungan Struktural Pusat Dan Daerah?", "id": "Hubungan Koordinasi, Pembinaan, Dan Pengawasan." },
  { "en": "Apa Itu Peraturan Gubernur (Pergub)?", "id": "Peraturan Pelaksana Dari Peraturan Daerah (Perda)." },
  { "en": "Apa Bentuk Negara Federal?", "id": "Negara Bagian Memiliki Kedaulatan Sendiri." },
  { "en": "Landasan Idiil Tata Negara?", "id": "Pancasila Sebagai Dasar Filosofi Negara." },
  { "en": "Landasan Konstitusional Tata Negara?", "id": "Undang-Undang Dasar (UUD) 1945 Sebagai Hukum Dasar." },
  { "en": "Apa Itu Sistem Parlementer?", "id": "Parlemen Memiliki Peran Sangat Kuat." },
  { "en": "Ciri Sistem Presidensial?", "id": "Presiden Sebagai Kepala Negara Dan Pemerintahan." },
  { "en": "Kekuasaan Moneter Dipegang Oleh?", "id": "Bank Indonesia (BI) Sebagai Bank Sentral." },
  { "en": "Tugas Dewan Perwakilan Daerah (DPD)?", "id": "Mengajukan Rancangan Undang-Undang (RUU) Terkait Otonomi Daerah." },
  { "en": "Anggota Dewan Perwakilan Daerah (DPD) Berasal Dari?", "id": "Perwakilan Setiap Provinsi Di Indonesia." },
  { "en": "Apa Itu Komisi Yudisial (KY)?", "id": "Lembaga Mengawasi Perilaku Hakim." },
  { "en": "Apa Itu Impeachment?", "id": "Pemberhentian Presiden Atau Pejabat Tinggi." },
  { "en": "Siapa Yang Berwenang Memakzulkan Presiden?", "id": "Majelis Permusyawaratan Rakyat (MPR) Atas Usul Dewan Perwakilan Rakyat (DPR)." },
  { "en": "Apa Itu Hak Budget Dewan Perwakilan Rakyat (DPR)?", "id": "Hak Mengajukan Dan Membahas Anggaran." },
  { "en": "Apa Itu Warga Negara?", "id": "Orang Yang Sah Diakui Negara." },
  { "en": "Asas Kewarganegaraan Indonesia?", "id": "Ius Sanguinis Dan Ius Soli." },
  { "en": "Apa Itu Ius Sanguinis?", "id": "Kewarganegaraan Berdasarkan Keturunan Darah." },
  { "en": "Apa Itu Ius Soli?", "id": "Kewarganegaraan Berdasarkan Tempat Kelahiran." },
  { "en": "Sistem Politik Otoriter?", "id": "Kekuasaan Terpusat Dan Absolut." },
  { "en": "Apa Itu Koalisi Politik?", "id": "Kerja Sama Antar Partai Politik." },
  { "en": "Tujuan Koalisi Politik?", "id": "Mendapatkan Dukungan Mayoritas Di Parlemen." },
  { "en": "Apa Itu Kampanye Politik?", "id": "Kegiatan Meyakinkan Pemilih Sebelum Pemilu." },
  { "en": "Siapa Pengawas Pemilihan Umum (Pemilu)?", "id": "Badan Pengawas Pemilihan Umum (Bawaslu)." },
  { "en": "Tugas Badan Pengawas Pemilihan Umum (Bawaslu)?", "id": "Mengawasi Setiap Tahapan Penyelenggaraan Pemilu." },
  { "en": "Apa Itu Golongan Putih (Golput)?", "id": "Tidak Menggunakan Hak Pilih Dalam Pemilu." },
  { "en": "Apa Itu Opini Publik?", "id": "Pendapat Umum Masyarakat Tentang Suatu Isu." },
  { "en": "Peran Opini Publik?", "id": "Mempengaruhi Kebijakan Pemerintah Dan Politik." },
  { "en": "Apa Itu Gerakan Sosial?", "id": "Aksi Kolektif Masyarakat Sipil." },
  { "en": "Tujuan Gerakan Sosial?", "id": "Menuntut Perubahan Sosial Atau Politik." },
  { "en": "Apa Itu Tata Kelola Pemerintahan?", "id": "Praktik Penyelenggaraan Kekuasaan Negara." },
  { "en": "Prinsip Tata Kelola Pemerintahan Baik?", "id": "Transparansi, Akuntabilitas, Partisipasi, Supremasi Hukum." },
  { "en": "Apa Itu Birokrasi?", "id": "Struktur Tatanan Organisasi Pemerintahan." },
  { "en": "Pembagian Wilayah Administrasi?", "id": "Provinsi, Kabupaten, Kota, Kecamatan, Desa." },
  { "en": "Apa Itu Kecamatan?", "id": "Wilayah Kerja Camat Perangkat Daerah." },
  { "en": "Apa Itu Desa?", "id": "Kesatuan Masyarakat Hukum Punya Kewenangan." },
  { "en": "Siapa Kepala Desa?", "id": "Dipilih Langsung Oleh Penduduk Desa." },
  { "en": "Sumber Keuangan Desa?", "id": "Anggaran Pendapatan Dan Belanja Desa (APBDes)." },
  { "en": "Apa Itu Badan Permusyawaratan Desa (BPD)?", "id": "Lembaga Perwujudan Demokrasi Di Desa." },
  { "en": "Fungsi Badan Permusyawaratan Desa (BPD)?", "id": "Membahas Menyepakati Rancangan Peraturan Desa." },
  { "en": "Apa Sifat Negara Indonesia?", "id": "Kesatuan Dengan Otonomi Daerah Luas." },
  { "en": "Arti Bhinneka Tunggal Ika?", "id": "Berbeda-beda Tetapi Tetap Satu Jua." },
  { "en": "Lambang Negara Indonesia?", "id": "Garuda Pancasila Dengan Semboyan Bhinneka." },
  { "en": "Lagu Kebangsaan Indonesia?", "id": "Indonesia Raya Ciptaan Wage Rudolf Soepratman." },
  { "en": "Bahasa Persatuan Indonesia?", "id": "Bahasa Indonesia Sebagai Bahasa Resmi." },
  { "en": "Apa Itu Hak Asasi Manusia (HAM)?", "id": "Hak Melekat Pada Diri Manusia." },
  { "en": "Lembaga Penegak Hak Asasi Manusia (HAM) Di Indonesia?", "id": "Komisi Nasional Hak Asasi Manusia (Komnas HAM)." },
  { "en": "Apa Itu Konvensi?", "id": "Aturan Tak Tertulis Dalam Praktik." },
  { "en": "Apa Itu Traktat?", "id": "Perjanjian Internasional Antar Negara." },
  { "en": "Apa Itu Yurisprudensi?", "id": "Keputusan Hakim Terdahulu Diikuti Hakim." },
  { "en": "Apa Itu Doktrin?", "id": "Pendapat Para Ahli Hukum Terkemuka." },
  { "en": "Sistem Hukum Di Indonesia?", "id": "Sistem Hukum Eropa Kontinental (Civil Law)." },
  { "en": "Apa Itu Rekrutmen Politik?", "id": "Proses Seleksi Dan Pengangkatan Jabatan." },
  { "en": "Saluran Rekrutmen Politik?", "id": "Partai Politik, Organisasi Masyarakat, Militer." },
  { "en": "Apa Itu Komunikasi Politik?", "id": "Penyampaian Pesan Politik Kepada Masyarakat." },
  { "en": "Fungsi Komunikasi Politik?", "id": "Membentuk Opini Dan Sikap Politik." },
  { "en": "Apa Itu Elit Politik?", "id": "Kelompok Kecil Yang Memegang Kekuasaan." },
  { "en": "Apa Itu Massa Politik?", "id": "Sebagian Besar Warga Negara Biasa." },
  { "en": "Perbedaan Otonomi Dan Federalisme?", "id": "Federalisme Berbasis Kedaulatan Negara Bagian." },
  { "en": "Pengawasan Pemerintah Pusat Terhadap Daerah?", "id": "Pengawasan Preventif Dan Represif." },
  { "en": "Apa Itu Pengawasan Preventif?", "id": "Pengawasan Sebelum Kebijakan Dilaksanakan." },
  { "en": "Apa Itu Pengawasan Represif?", "id": "Pengawasan Setelah Kebijakan Dilaksanakan." },
  { "en": "Apa Itu Dana Otonomi Khusus?", "id": "Dana Untuk Provinsi Otonomi Khusus." },
  { "en": "Kewenangan Provinsi Sebagai Daerah Otonom?", "id": "Mengatur Urusan Pemerintahan Lintas Kabupaten." },
  { "en": "Apa Itu Peraturan Bersama Kepala Daerah?", "id": "Kerja Sama Antar Daerah Berdekatan." },
  { "en": "Sumber Hukum Tata Negara?", "id": "Undang-Undang Dasar, Undang-Undang, Peraturan Pemerintah." },
  { "en": "Objek Kajian Tata Negara?", "id": "Organisasi Negara, Jabatan, Dan Wewenang." },
  { "en": "Apa Itu Kekuasaan Eksekutif?", "id": "Kekuasaan Untuk Melaksanakan Undang-Undang." },
  { "en": "Apa Itu Kekuasaan Legislatif?", "id": "Kekuasaan Untuk Membentuk Undang-Undang." },
  { "en": "Apa Itu Kekuasaan Yudikatif?", "id": "Kekuasaan Untuk Mengadili Pelanggaran Hukum." },
  { "en": "Berapa Jumlah Anggota Dewan Perwakilan Rakyat (DPR)?", "id": "Sebanyak 580 Anggota Terpilih." },
  { "en": "Berapa Jumlah Anggota Dewan Perwakilan Daerah (DPD)?", "id": "Empat Anggota Dari Setiap Provinsi." },
  { "en": "Siapa Yang Melantik Anggota Dewan Perwakilan Rakyat (DPR)?", "id": "Ketua Mahkamah Agung (MA)." },
  { "en": "Sifat Keanggotaan Majelis Permusyawaratan Rakyat (MPR)?", "id": "Terdiri Atas Anggota Dewan Perwakilan Rakyat (DPR) Dan Dewan Perwakilan Daerah (DPD)." },
  { "en": "Apa Itu Demokrasi Pancasila?", "id": "Demokrasi Berdasarkan Nilai-nilai Pancasila." },
  { "en": "Prinsip Demokrasi Pancasila?", "id": "Musyawarah Mufakat Dan Kekeluargaan." },
  { "en": "Apa Itu Partai Oposisi?", "id": "Partai Di Luar Koalisi Pemerintahan." },
  { "en": "Apa Itu Threshold Parlemen?", "id": "Ambang Batas Suara Partai Politik." },
  { "en": "Tujuan Threshold Parlemen?", "id": "Menyederhanakan Jumlah Partai Di Parlemen." },
  { "en": "Apa Itu Daerah Pemilihan (Dapil)?", "id": "Wilayah Kompetisi Calon Legislatif." },
  { "en": "Sistem Pemilu Legislatif?", "id": "Sistem Proporsional Terbuka." },
  { "en": "Apa Itu Proporsional Terbuka?", "id": "Pemilih Memilih Calon Legislatif Langsung." },
  { "en": "Dampak Positif Otonomi Daerah?", "id": "Mempercepat Pembangunan Dan Pelayanan Publik." },
  { "en": "Dampak Negatif Otonomi Daerah?", "id": "Potensi Korupsi Dan Kesenjangan Antardaerah." },
  { "en": "Evaluasi Otonomi Daerah Dilakukan Oleh?", "id": "Pemerintah Pusat Secara Berkala." },
  { "en": "Apa Itu Politik Dinasti?", "id": "Kekuasaan Politik Diwariskan Secara Turun-temurun." },
  { "en": "Mengapa Politik Dinasti Dilarang?", "id": "Membatasi Kesempatan Dan Memicu Korupsi." },
  { "en": "Apa Itu Konstitusionalisme?", "id": "Gagasan Pembatasan Kekuasaan Pemerintah." },
  { "en": "Apa Itu Badan Pemeriksa Keuangan (BPK)?", "id": "Lembaga Pemeriksa Keuangan Negara." },
  { "en": "Tugas Badan Pemeriksa Keuangan (BPK)?", "id": "Memeriksa Pengelolaan Tanggung Jawab Keuangan." },
  { "en": "Asas Penyelenggaraan Pemerintahan Daerah?", "id": "Kepastian Hukum, Tertib, Kepentingan Umum." },
  { "en": "Apa Itu Urusan Pilihan Daerah?", "id": "Urusan Sesuai Potensi Unggulan Daerah." },
  { "en": "Contoh Urusan Wajib Pelayanan Dasar?", "id": "Pendidikan, Kesehatan, Pekerjaan Umum." },
  { "en": "Bagaimana Hubungan Keuangan Pusat Daerah?", "id": "Adanya Dana Perimbangan Dan Pinjaman." },
  { "en": "Apa Itu Dana Alokasi Khusus (DAK)?", "id": "Dana Bantu Kegiatan Khusus Daerah." },
  { "en": "Siapa Yang Mengawasi Peraturan Daerah (Perda)?", "id": "Pemerintah Pusat, Dalam Hal Ini Kementerian." },
  { "en": "Apa Itu Asosiasi Pemerintah Daerah?", "id": "Wadah Kerja Sama Antar Pemerintah Daerah." },
  { "en": "Contoh Asosiasi Pemerintah Daerah?", "id": "Asosiasi Pemerintah Provinsi Seluruh Indonesia (APPSI)." },
  { "en": "Apa Itu Pemerintahan Daerah?", "id": "Penyelenggaraan Urusan Oleh Pemerintah Daerah." },
  { "en": "Apa Itu Otonomi Asimetris?", "id": "Otonomi Berbeda Antara Satu Daerah." },
  { "en": "Pengertian Ilmu Tata Negara?", "id": "Ilmu Mempelajari Negara Secara Yuridis." },
  { "en": "Apa Itu Residu Kekuasaan?", "id": "Kekuasaan Sisa Yang Tidak Disebutkan." },
  { "en": "Siapa Yang Menjalankan Residu Kekuasaan?", "id": "Di Indonesia Oleh Pemerintah Pusat." },
  { "en": "Apa Itu Hak Prerogatif Presiden?", "id": "Hak Istimewa Presiden Tanpa Campur Tangan." },
  { "en": "Contoh Hak Prerogatif?", "id": "Memberi Grasi, Amnesti, Abolisi, Rehabilitasi." },
  { "en": "Perbedaan Grasi Dan Amnesti?", "id": "Grasi Pengampunan, Amnesti Penghapusan Tuntutan." },
  { "en": "Apa Itu Duta Dan Konsul?", "id": "Perwakilan Diplomatik Negara Di Luar Negeri." },
  { "en": "Siapa Yang Mengangkat Duta?", "id": "Presiden Dengan Pertimbangan Dewan Perwakilan Rakyat (DPR)." },
  { "en": "Apa Itu Stufenbau Theory?", "id": "Teori Hierarki Norma Hukum Hans Kelsen." },
  { "en": "Norma Tertinggi Menurut Hans Kelsen?", "id": "Grundnorm Atau Norma Dasar Negara." },
  { "en": "Apa Itu Peraturan Pemerintah (PP)?", "id": "Peraturan Menjalankan Undang-Undang." },
  { "en": "Siapa Yang Menetapkan Peraturan Pemerintah (PP)?", "id": "Presiden Republik Indonesia." },
  { "en": "Apa Itu Peraturan Presiden (Perpres)?", "id": "Peraturan Yang Dibuat Oleh Presiden." },
  { "en": "Apa Itu Hukum Administrasi Negara?", "id": "Hukum Mengatur Administrasi Publik." },
  { "en": "Hubungan Tata Negara Dengan Politik?", "id": "Tata Negara Adalah Kerangka Politik." },
  { "en": "Apa Itu Legitimasi?", "id": "Dasar Kewenangan Atau Keabsahan Kekuasaan." },
  { "en": "Definisi Sistem Politik David Easton?", "id": "Alokasi Nilai Yang Bersifat Otoritatif." },
  { "en": "Input Dalam Sistem Politik?", "id": "Tuntutan Dan Dukungan Dari Masyarakat." },
  { "en": "Output Dalam Sistem Politik?", "id": "Kebijakan Dan Keputusan Pemerintah." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Tanggapan Masyarakat Terhadap Output Politik." },
  { "en": "Pendekatan Dalam Ilmu Politik?", "id": "Pendekatan Kelembagaan, Perilaku, Dan Struktural." },
  { "en": "Apa Itu Ideologi Politik?", "id": "Sistem Gagasan Tentang Tatanan Politik." },
  { "en": "Contoh Ideologi Politik Dunia?", "id": "Liberalisme, Sosialisme, Komunisme, Fasisme." },
  { "en": "Apa Itu Polarisasi Politik?", "id": "Pembelahan Politik Menjadi Dua Kubu." },
  { "en": "Bahaya Polarisasi Politik?", "id": "Memicu Konflik Dan Perpecahan Sosial." },
  { "en": "Apa Itu Konstituen?", "id": "Masyarakat Yang Diwakili Oleh Pejabat." },
  { "en": "Apa Itu Masyarakat Madani (Civil Society)?", "id": "Masyarakat Beradab, Demokratis, Dan Mandiri." },
  { "en": "Peran Masyarakat Madani?", "id": "Menjadi Penyeimbang Kekuasaan Negara." },
  { "en": "Apa Itu Desentralisasi Fiskal?", "id": "Penyerahan Kewenangan Fiskal Ke Daerah." },
  { "en": "Tujuan Desentralisasi Fiskal?", "id": "Meningkatkan Kemandirian Fiskal Daerah." },
  { "en": "Apa Itu Kawasan Ekonomi Khusus (KEK)?", "id": "Kawasan Dengan Fasilitas Ekonomi Khusus." },
  { "en": "Tujuan Kawasan Ekonomi Khusus (KEK)?", "id": "Menarik Investasi Dan Mendorong Ekonomi." },
  { "en": "Apa Itu Pelayanan Publik?", "id": "Kegiatan Pelayanan Oleh Penyelenggara Negara." },
  { "en": "Asas Pelayanan Publik?", "id": "Kepentingan Umum, Kepastian Hukum, Keterbukaan." },
  { "en": "Lembaga Pengawas Pelayanan Publik?", "id": "Ombudsman Republik Indonesia." },
  { "en": "Apa Itu E-Government?", "id": "Penggunaan Teknologi Informasi Dalam Pemerintahan." },
  { "en": "Manfaat E-Government?", "id": "Meningkatkan Efisiensi Dan Transparansi Pelayanan." },
  { "en": "Apa Itu Sumpah Pemuda?", "id": "Ikrar Persatuan Pemuda Indonesia 1928." },
  { "en": "Isi Sumpah Pemuda?", "id": "Satu Tanah Air, Bangsa, Bahasa." },
  { "en": "Makna Proklamasi Kemerdekaan?", "id": "Pernyataan Kemerdekaan Bangsa Indonesia." },
  { "en": "Siapa Pembaca Teks Proklamasi?", "id": "Soekarno Didampingi Oleh Mohammad Hatta." },
  { "en": "Tujuan Negara Dalam Pembukaan Undang-Undang Dasar (UUD) 1945?", "id": "Melindungi, Mensejahterakan, Mencerdaskan, Ikut Perdamaian." },
  { "en": "Berapa Alinea Pembukaan Undang-Undang Dasar (UUD) 1945?", "id": "Terdiri Atas Empat Alinea." },
  { "en": "Sifat Pembukaan Undang-Undang Dasar (UUD) 1945?", "id": "Fundamental Dan Tidak Dapat Diubah." },
  { "en": "Apa Itu Wawasan Nusantara?", "id": "Cara Pandang Bangsa Indonesia." },
  { "en": "Hakikat Wawasan Nusantara?", "id": "Keutuhan Nusantara Dan Persatuan Bangsa." },
  { "en": "Aspek Trigatra Wawasan Nusantara?", "id": "Geografi, Demografi, Sumber Kekayaan Alam." },
  { "en": "Aspek Pancagatra Wawasan Nusantara?", "id": "Ideologi, Politik, Ekonomi, Sosial, Pertahanan." },
  { "en": "Apa Itu Geopolitik Indonesia?", "id": "Wawasan Nusantara Sebagai Dasar Pembangunan." },
  { "en": "Apa Itu Geostrategi Indonesia?", "id": "Ketahanan Nasional Sebagai Cara Hadapi Ancaman." },
  { "en": "Apa Itu Demokrasi Terpimpin?", "id": "Sistem Demokrasi Era Presiden Soekarno." },
  { "en": "Apa Itu Orde Baru?", "id": "Era Pemerintahan Presiden Soeharto." },
  { "en": "Apa Itu Reformasi?", "id": "Gerakan Menuju Tatanan Lebih Demokratis." },
  { "en": "Tuntutan Era Reformasi?", "id": "Adili Soeharto, Amandemen Undang-Undang Dasar (UUD), Otonomi." },
  { "en": "Apa Itu Pendidikan Politik?", "id": "Proses Meningkatkan Kesadaran Politik Masyarakat." },
  { "en": "Tujuan Pendidikan Politik?", "id": "Menciptakan Warga Negara Partisipatif Kritis." },
  { "en": "Apa Itu Perilaku Politik?", "id": "Tindakan Individu Dalam Arena Politik." },
  { "en": "Faktor Mempengaruhi Perilaku Politik?", "id": "Keluarga, Sekolah, Media Massa, Lingkungan." },
  { "en": "Apa Itu Konflik Politik?", "id": "Pertentangan Dalam Proses Politik." },
  { "en": "Apa Itu Konsensus Politik?", "id": "Kesepakatan Bersama Dalam Kehidupan Politik." },
  { "en": "Apa Itu Otokrasi?", "id": "Pemerintahan Oleh Satu Orang Absolut." },
  { "en": "Apa Itu Oligarki?", "id": "Pemerintahan Oleh Sekelompok Kecil Orang." },
  { "en": "Pemekaran Daerah Adalah?", "id": "Pembentukan Daerah Otonom Baru." },
  { "en": "Tujuan Pemekaran Daerah?", "id": "Mempercepat Pembangunan Dan Pelayanan Publik." },
  { "en": "Syarat Pemekaran Daerah?", "id": "Syarat Administratif, Teknis, Dan Fisik." },
  { "en": "Apa Itu Daerah Istimewa?", "id": "Daerah Dengan Sifat Istimewa." },
  { "en": "Keistimewaan Daerah Istimewa Yogyakarta (DIY)?", "id": "Jabatan Gubernur Dan Wakil Gubernur." },
  { "en": "Apa Itu Lembaga Kepresidenan?", "id": "Presiden Dan Wakil Presiden." },
  { "en": "Kementerian Koordinator Di Indonesia?", "id": "Mengkoordinasikan Beberapa Kementerian Teknis." },
  { "en": "Contoh Kementerian Koordinator?", "id": "Kementerian Koordinator Bidang Politik, Hukum, Dan Keamanan (Kemenko Polhukam)." },
  { "en": "Apa Itu Lembaga Pemerintah Non-Kementerian (LPNK)?", "id": "Lembaga Bantu Tugas Presiden." },
  { "en": "Contoh Lembaga Pemerintah Non-Kementerian (LPNK)?", "id": "Badan Intelijen Negara (BIN), Badan Narkotika Nasional (BNN)." },
  { "en": "Apa Itu Presidential Threshold?", "id": "Ambang Batas Pencalonan Presiden." },
  { "en": "Tujuan Presidential Threshold?", "id": "Memperkuat Sistem Presidensial." },
  { "en": "Apa Itu Sistem Pemilu Distrik?", "id": "Satu Daerah Pemilihan Satu Wakil." },
  { "en": "Kelebihan Sistem Distrik?", "id": "Hubungan Wakil Dan Konstituen Erat." },
  { "en": "Kelemahan Sistem Proporsional?", "id": "Hubungan Wakil Dan Konstituen Renggang." },
  { "en": "Apa Itu Budaya Politik Parokial?", "id": "Minimnya Kesadaran Politik Warga." },
  { "en": "Apa Itu Budaya Politik Kaula?", "id": "Warga Sadar Politik, Namun Pasif." },
  { "en": "Apa Itu Budaya Politik Partisipan?", "id": "Warga Sadar Dan Aktif Berpartisipasi." },
  { "en": "Indeks Demokrasi Indonesia (IDI)?", "id": "Ukuran Tingkat Perkembangan Demokrasi." },
  { "en": "Komponen Indeks Demokrasi Indonesia (IDI)?", "id": "Kebebasan Sipil, Hak Politik, Lembaga." },
  { "en": "Apa Itu Sentralisasi?", "id": "Pemusatan Wewenang Pada Pemerintah Pusat." },
  { "en": "Prinsip Akuntabilitas Otonomi Daerah?", "id": "Pertanggungjawaban Kinerja Kepada Publik." },
  { "en": "Apa Itu Perangkat Daerah?", "id": "Unsur Pembantu Kepala Daerah." },
  { "en": "Contoh Perangkat Daerah?", "id": "Sekretariat Daerah, Dinas, Badan Daerah." },
  { "en": "Fungsi Sekretariat Daerah?", "id": "Membantu Penyusunan Kebijakan Kepala Daerah." },
  { "en": "Apa Itu Dinas Daerah?", "id": "Unsur Pelaksana Urusan Pemerintahan Daerah." },
  { "en": "Apa Itu Badan Daerah?", "id": "Unsur Penunjang Urusan Pemerintahan Daerah." },
  { "en": "Apa Itu Dana Desa?", "id": "Dana Anggaran Pendapatan Dan Belanja Negara (APBN) Untuk Desa." },
  { "en": "Tujuan Dana Desa?", "id": "Membiayai Pembangunan Dan Pemberdayaan Masyarakat." },
  { "en": "Pengawasan Dana Desa?", "id": "Dilakukan Oleh Inspektorat Dan Masyarakat." },
  { "en": "Apa Itu Negara Maritim?", "id": "Negara Dengan Wilayah Laut Luas." },
  { "en": "Indonesia Sebagai Poros Maritim Dunia?", "id": "Cita-cita Menjadikan Indonesia Pusat Maritim." },
  { "en": "Apa Itu Kedaulatan Ke Dalam?", "id": "Kekuasaan Tertinggi Mengatur Dalam Negeri." },
  { "en": "Apa Itu Kedaulatan Ke Luar?", "id": "Kemampuan Menjalin Hubungan Negara Lain." },
  { "en": "Unsur-Unsur Negara?", "id": "Rakyat, Wilayah, Pemerintah, Pengakuan." },
  { "en": "Apa Itu Wilayah Negara?", "id": "Darat, Laut, Udara, Ekstrateritorial." },
  { "en": "Apa Batas Laut Teritorial?", "id": "12 Mil Laut Dari Garis Dasar." },
  { "en": "Apa Itu Zona Ekonomi Eksklusif (ZEE)?", "id": "200 Mil Laut Dari Garis Dasar." },
  { "en": "Apa Itu Mahkamah Militer?", "id": "Pengadilan Untuk Anggota Tentara Nasional Indonesia (TNI)." },
  { "en": "Siapa Panglima Tertinggi Tentara Nasional Indonesia (TNI)?", "id": "Presiden Republik Indonesia." },
  { "en": "Tugas Tentara Nasional Indonesia (TNI)?", "id": "Menegakkan Kedaulatan Dan Mempertahankan Keutuhan." },
  { "en": "Tugas Kepolisian Republik Indonesia (Polri)?", "id": "Memelihara Keamanan Dan Ketertiban Masyarakat." },
  { "en": "Siapa Kepala Kepolisian Republik Indonesia (Kapolri)?", "id": "Diangkat Dan Diberhentikan Oleh Presiden." },
  { "en": "Apa Itu Sistem Satu Kamar (Unikameral)?", "id": "Parlemen Hanya Terdiri Satu Badan." },
  { "en": "Apa Itu Sistem Dua Kamar (Bikameral)?", "id": "Parlemen Terdiri Atas Dua Badan." },
  { "en": "Sistem Parlemen Indonesia?", "id": "Sistem Tiga Kamar (Trikameral) Lembut." },
  { "en": "Apa Itu Voting?", "id": "Pengambilan Keputusan Berdasarkan Suara Terbanyak." },
  { "en": "Apa Itu Musyawarah Mufakat?", "id": "Pengambilan Keputusan Melalui Pembahasan Bersama." },
  { "en": "Pentingnya Kompromi Dalam Politik?", "id": "Mencapai Kesepakatan Yang Dapat Diterima." },
  { "en": "Apa Itu Reformasi Birokrasi?", "id": "Upaya Perbaikan Tata Kelola Pemerintahan." },
  { "en": "Tujuan Reformasi Birokrasi?", "id": "Mewujudkan Birokrasi Profesional Dan Bersih." },
  { "en": "Area Perubahan Reformasi Birokrasi?", "id": "Organisasi, Tatalaksana, Sumber Daya Manusia (SDM) Aparatur." },
  { "en": "Apa Itu Aparatur Sipil Negara (ASN)?", "id": "Profesi Pegawai Negeri Sipil (PNS) Dan Pegawai Pemerintah dengan Perjanjian Kerja (PPPK)." },
  { "en": "Fungsi Aparatur Sipil Negara (ASN)?", "id": "Pelaksana Kebijakan, Pelayan Publik, Perekat." },
  { "en": "Apa Itu Merit System?", "id": "Sistem Manajemen Sumber Daya Manusia (SDM) Berbasis Kualifikasi." },
  { "en": "Tujuan Merit System?", "id": "Merekrut Aparatur Sipil Negara (ASN) Kompeten Dan Profesional." },
  { "en": "Apa Itu Korupsi?", "id": "Penyalahgunaan Jabatan Untuk Keuntungan Pribadi." },
  { "en": "Lembaga Pemberantas Korupsi?", "id": "Komisi Pemberantasan Korupsi (KPK)." },
  { "en": "Dampak Korupsi?", "id": "Merugikan Keuangan Negara Dan Pembangunan." },
  { "en": "Dasar Hukum Otonomi Daerah Terbaru?", "id": "Undang-Undang Nomor 23 Tahun 2014." },
  { "en": "Urusan Pemerintahan Umum?", "id": "Urusan Yang Menjadi Kewenangan Presiden." },
  { "en": "Siapa Pelaksana Urusan Pemerintahan Umum?", "id": "Gubernur Dan Bupati Atau Wali Kota." },
  { "en": "Apa Itu Forum Koordinasi Pimpinan Daerah (Forkopimda)?", "id": "Wadah Koordinasi Antar Pimpinan Daerah." },
  { "en": "Anggota Forum Koordinasi Pimpinan Daerah (Forkopimda) Provinsi?", "id": "Gubernur, Ketua Dewan Perwakilan Rakyat Daerah (DPRD), Pangdam, Kapolda." },
  { "en": "Apa Itu Daerah Tertinggal?", "id": "Daerah Dengan Pembangunan Dan Ekonomi Rendah." },
  { "en": "Fokus Pembangunan Daerah Tertinggal?", "id": "Peningkatan Infrastruktur Dan Sumber Daya Manusia (SDM)." },
  { "en": "Apa Itu Perkotaan?", "id": "Kawasan Dengan Kegiatan Utama Bukan Pertanian." },
  { "en": "Masalah Utama Perkotaan?", "id": "Kemacetan, Banjir, Sampah, Urbanisasi." },
  { "en": "Apa Itu Urbanisasi?", "id": "Perpindahan Penduduk Dari Desa Ke Kota." },
  { "en": "Dampak Urbanisasi Bagi Kota?", "id": "Peningkatan Jumlah Penduduk Dan Tekanan." },
  { "en": "Teori Pembagian Kekuasaan John Locke?", "id": "Legislatif, Eksekutif, Dan Federatif." },
  { "en": "Kekuasaan Federatif Menurut John Locke?", "id": "Kekuasaan Mengatur Hubungan Luar Negeri." },
  { "en": "Semboyan Revolusi Prancis?", "id": "Liberte, Egalite, Fraternite." },
  { "en": "Apa Itu Magna Charta?", "id": "Piagam Pembatasan Kekuasaan Raja Inggris." },
  { "en": "Apa Itu Rule of Law?", "id": "Prinsip Supremasi Hukum Di Atas Segalanya." },
  { "en": "Unsur Rule of Law?", "id": "Supremasi Aturan, Kedudukan Sama, Jaminan Hak Asasi Manusia (HAM)." },
  { "en": "Apa Itu Utilitarianisme?", "id": "Tindakan Benar Jika Bermanfaat." },
  { "en": "Apa Itu Monarki Konstitusional?", "id": "Raja Berkuasa Berdasarkan Konstitusi." },
  { "en": "Negara Dengan Monarki Konstitusional?", "id": "Inggris, Jepang, Thailand, Malaysia." },
  { "en": "Apa Itu Monarki Absolut?", "id": "Raja Memiliki Kekuasaan Mutlak." },
  { "en": "Contoh Monarki Absolut?", "id": "Arab Saudi, Brunei Darussalam." },
  { "en": "Apa Itu Tirani?", "id": "Pemerintahan Otoriter Yang Kejam." },
  { "en": "Partai Tunggal Terdapat Pada Sistem?", "id": "Sistem Politik Totaliter Atau Otoriter." },
  { "en": "Apa Itu Demagogi?", "id": "Menghasut Rakyat Dengan Janji Palsu." },
  { "en": "Apa Itu Negosiasi Politik?", "id": "Proses Tawar-menawar Mencapai Kesepakatan." },
  { "en": "Pentingnya Literasi Politik?", "id": "Agar Masyarakat Cerdas Dalam Berpolitik." },
  { "en": "Apa Itu Apatisme Politik?", "id": "Sikap Acuh Tak Acuh Politik." },
  { "en": "Penyebab Apatisme Politik?", "id": "Kekecewaan Terhadap Pemerintah Atau Politisi." },
  { "en": "Apa Itu Gerakan Non-Blok (GNB)?", "id": "Kelompok Negara Tidak Memihak Blok." },
  { "en": "Politik Luar Negeri Indonesia?", "id": "Bebas Aktif Berdasarkan Pancasila." },
  { "en": "Arti Politik Bebas Aktif?", "id": "Bebas, Aktif Dalam Perdamaian Dunia." },
  { "en": "Pendiri Gerakan Non-Blok (GNB)?", "id": "Soekarno, Tito, Nehru, Nasser, Nkrumah." },
  { "en": "Apa Itu Association of Southeast Asian Nations (ASEAN)?", "id": "Organisasi Regional Negara Asia Tenggara." },
  { "en": "Tujuan Association of Southeast Asian Nations (ASEAN)?", "id": "Meningkatkan Pertumbuhan Ekonomi Dan Stabilitas." },
  { "en": "Prinsip Association of Southeast Asian Nations (ASEAN)?", "id": "Saling Menghormati Dan Tidak Campur Tangan." },
  { "en": "Apa Itu Ekstradisi?", "id": "Penyerahan Pelaku Kejahatan Antar Negara." },
  { "en": "Fungsi Ideologi Bagi Negara?", "id": "Memberi Arah, Tujuan, Identitas Bangsa." },
  { "en": "Pancasila Sebagai Ideologi Terbuka?", "id": "Mampu Berinteraksi Dengan Perkembangan Zaman." },
  { "en": "Dimensi Pancasila Sebagai Ideologi?", "id": "Dimensi Realitas, Idealitas, Dan Fleksibilitas." },
  { "en": "Apa Itu Konservatisme?", "id": "Ideologi Mendukung Tradisi Dan Stabilitas." },
  { "en": "Apa Itu Liberalisme?", "id": "Ideologi Menjunjung Kebebasan Individu." },
  { "en": "Apa Itu Fasisme?", "id": "Ideologi Nasionalis Otoriter Yang Radikal." },
  { "en": "Apa Itu Anarkisme?", "id": "Paham Politik Tanpa Struktur Negara." },
  { "en": "Perbedaan Bangsa Dan Negara?", "id": "Bangsa Ikatan Sosiologis, Negara Yuridis." },
  { "en": "Apa Itu Nasionalisme?", "id": "Paham Kebangsaan Dan Cinta Tanah Air." },
  { "en": "Nasionalisme Sempit Disebut Juga?", "id": "Chauvinisme Atau Mengagungkan Bangsa Sendiri." },
  { "en": "Apa Itu Patriotisme?", "id": "Sikap Rela Berkorban Untuk Negara." },
  { "en": "Apa Itu Bela Negara?", "id": "Sikap Dan Perilaku Cinta Negara." },
  { "en": "Landasan Hukum Bela Negara?", "id": "Pasal 27 Ayat 3 Undang-Undang Dasar (UUD) 1945." },
  { "en": "Contoh Bela Negara Non-Fisik?", "id": "Belajar Tekun, Membayar Pajak, Toleransi." },
  { "en": "Ancaman Terhadap Negara?", "id": "Ancaman Militer Dan Non-Militer." },
  { "en": "Contoh Ancaman Non-Militer?", "id": "Narkoba, Kemiskinan, Radikalisme, Separatisme." },
  { "en": "Apa Itu Separatisme?", "id": "Gerakan Memisahkan Diri Dari Negara." },
  { "en": "Apa Itu Urusan Konkuren?", "id": "Urusan Dibagi Antara Pusat Dan Daerah." },
  { "en": "Prinsip Pembagian Urusan Konkuren?", "id": "Akuntabilitas, Efisiensi, Eksternalitas, Kepentingan Strategis." },
  { "en": "Siapa Yang Menetapkan Norma Standar Prosedur?", "id": "Pemerintah Pusat Untuk Urusan Konkuren." },
  { "en": "Apa Itu Kawasan Perbatasan?", "id": "Bagian Wilayah Negara Di Perbatasan." },
  { "en": "Pengelolaan Kawasan Perbatasan?", "id": "Dilakukan Oleh Pemerintah Pusat Dan Daerah." },
  { "en": "Apa Itu Rencana Tata Ruang Wilayah (RTRW)?", "id": "Arahan Pemanfaatan Ruang Wilayah." },
  { "en": "Siapa Menyusun Rencana Tata Ruang Wilayah (RTRW) Provinsi?", "id": "Pemerintah Daerah Provinsi Bersama Dewan Perwakilan Rakyat Daerah (DPRD)." },
  { "en": "Apa Itu Izin Mendirikan Bangunan (IMB)?", "id": "Izin Mendirikan Bangunan Gedung." },
  { "en": "Tujuan Izin Mendirikan Bangunan (IMB)?", "id": "Mewujudkan Bangunan Sesuai Tata Ruang." },
  { "en": "Apa Itu Retribusi Daerah?", "id": "Pungutan Atas Jasa Atau Izin." },
  { "en": "Contoh Retribusi Jasa Umum?", "id": "Retribusi Pelayanan Kebersihan Dan Pasar." },
  { "en": "Apa Itu Pajak Daerah?", "id": "Kontribusi Wajib Kepada Daerah Terutang." },
  { "en": "Contoh Pajak Provinsi?", "id": "Pajak Kendaraan Bermotor (PKB)." },
  { "en": "Contoh Pajak Kabupaten Atau Kota?", "id": "Pajak Hotel, Restoran, Dan Hiburan." },
  { "en": "Apa Itu Sengketa Kewenangan Lembaga Negara?", "id": "Perselisihan Kewenangan Antar Lembaga Negara." },
  { "en": "Siapa Yang Memutus Sengketa Kewenangan?", "id": "Mahkamah Konstitusi (MK)." },
  { "en": "Apa Itu Judicial Review?", "id": "Hak Uji Materiil Peraturan Perundangan." },
  { "en": "Siapa Pelaku Judicial Review?", "id": "Mahkamah Agung (MA) Dan Mahkamah Konstitusi (MK)." },
  { "en": "Objek Judicial Review Mahkamah Agung (MA)?", "id": "Peraturan Di Bawah Undang-Undang." },
  { "en": "Objek Judicial Review Mahkamah Konstitusi (MK)?", "id": "Undang-Undang Terhadap Undang-Undang Dasar (UUD)." },
  { "en": "Apa Itu Kewarganegaraan Ganda Terbatas?", "id": "Untuk Anak Hasil Perkawinan Campuran." },
  { "en": "Kapan Anak Kehilangan Kewarganegaraan Ganda?", "id": "Setelah Usia 18 Tahun Harus Memilih." },
  { "en": "Apa Itu Naturalisasi?", "id": "Proses Pewarganegaraan Bagi Warga Asing." },
  { "en": "Syarat Naturalisasi Biasa?", "id": "Usia 18, Tinggal 5 Tahun." },
  { "en": "Apa Itu Repudiasi?", "id": "Menolak Status Kewarganegaraan." },
  { "en": "Apa Itu Apatride?", "id": "Seseorang Tidak Memiliki Kewarganegaraan." },
  { "en": "Apa Itu Bipatride?", "id": "Seseorang Memiliki Kewarganegaraan Rangkap." },
  { "en": "Penyebab Terjadinya Apatride?", "id": "Lahir Di Negara Ius Soli." },
  { "en": "Apa Itu Teori Kedaulatan Tuhan?", "id": "Kekuasaan Tertinggi Berasal Dari Tuhan." },
  { "en": "Apa Itu Teori Kedaulatan Raja?", "id": "Raja Adalah Pemegang Kedaulatan Absolut." },
  { "en": "Apa Itu Teori Kedaulatan Negara?", "id": "Negara Adalah Sumber Kedaulatan Tertinggi." },
  { "en": "Apa Itu Teori Kedaulatan Hukum?", "id": "Hukum Adalah Panglima Tertinggi." },
  { "en": "Apa Itu Teori Kedaulatan Rakyat?", "id": "Rakyat Pemegang Kekuasaan Tertinggi." },
  { "en": "Tokoh Teori Kedaulatan Rakyat?", "id": "Jean-Jacques Rousseau." },
  { "en": "Apa Itu Kontrak Sosial?", "id": "Perjanjian Masyarakat Membentuk Negara." },
  { "en": "Buku Kontrak Sosial?", "id": "Du Contrat Social Oleh Rousseau." },
  { "en": "Apa Itu Tirani Mayoritas?", "id": "Penindasan Minoritas Oleh Kelompok Mayoritas." },
  { "en": "Apa Itu Pluralisme?", "id": "Paham Menghargai Adanya Perbedaan." },
  { "en": "Pentingnya Pluralisme Dalam Politik?", "id": "Mencegah Dominasi Satu Kelompok." },
  { "en": "Apa Itu Totalitarianisme?", "id": "Negara Mengontrol Penuh Kehidupan Warga." },
  { "en": "Ciri Negara Totaliter?", "id": "Satu Partai, Ideologi Tunggal, Teror." },
  { "en": "Apa Itu Pragmatisme Politik?", "id": "Politik Praktis Berorientasi Hasil." },
  { "en": "Apa Itu Politik Identitas?", "id": "Politik Berbasis Identitas Kelompok." },
  { "en": "Bahaya Politik Identitas?", "id": "Memicu Sentimen Primordial Dan Perpecahan." },
  { "en": "Apa Itu Clientelism?", "id": "Hubungan Patron-Klien Dalam Politik." },
  { "en": "Dampak Clientelism?", "id": "Menghambat Meritokrasi Dan Memicu Korupsi." },
  { "en": "Apa Itu Teknokrat?", "id": "Pejabat Pemerintah Berlatar Belakang Keahlian." },
  { "en": "Peran Teknokrat Dalam Pemerintahan?", "id": "Memberikan Masukan Berbasis Data Ilmiah." },
  { "en": "Apa Itu Good Governance?", "id": "Tata Kelola Pemerintahan Yang Baik." },
  { "en": "Prinsip Good Governance?", "id": "Partisipasi, Supremasi Hukum, Transparansi." },
  { "en": "Apa Itu Clean Government?", "id": "Pemerintahan Bersih Dari Korupsi." },
  { "en": "Apa Itu Desentralisasi Asimetris?", "id": "Pemberian Otonomi Berbeda Tingkatannya." },
  { "en": "Tujuan Desentralisasi Asimetris?", "id": "Mengakomodasi Keberagaman Dan Kekhususan Daerah." },
  { "en": "Apa Itu Badan Usaha Milik Daerah (BUMD)?", "id": "Badan Usaha Milik Pemerintah Daerah." },
  { "en": "Tujuan Badan Usaha Milik Daerah (BUMD)?", "id": "Memberi Manfaat Ekonomi Dan Pendapatan Asli Daerah (PAD)." },
  { "en": "Apa Itu Hubungan Industrial?", "id": "Hubungan Antara Pekerja, Pengusaha, Pemerintah." },
  { "en": "Apa Itu Kebebasan Berserikat?", "id": "Hak Membentuk Dan Bergabung Organisasi." },
  { "en": "Apa Itu Kebebasan Berpendapat?", "id": "Hak Menyatakan Pikiran Lisan Tulisan." },
  { "en": "Batasan Kebebasan Berpendapat?", "id": "Tidak Melanggar Hak Orang Lain." },
  { "en": "Dasar Hukum Kebebasan Berpendapat?", "id": "Pasal 28E Ayat 3 Undang-Undang Dasar (UUD) 1945." },
  { "en": "Apa Itu Pers?", "id": "Lembaga Sosial Wahana Komunikasi Massa." },
  { "en": "Fungsi Pers?", "id": "Media Informasi, Pendidikan, Hiburan, Kontrol." },
  { "en": "Apa Itu Dewan Pers?", "id": "Lembaga Independen Mengawasi Kehidupan Pers." },
  { "en": "Apa Itu Hak Jawab?", "id": "Hak Seseorang Memberi Tanggapan." },
  { "en": "Apa Itu Hak Koreksi?", "id": "Hak Membetulkan Kekeliruan Informasi." },
  { "en": "Apa Itu Unjuk Rasa?", "id": "Menyampaikan Pendapat Di Muka Umum." },
  { "en": "Dasar Hukum Unjuk Rasa?", "id": "Undang-Undang Nomor 9 Tahun 1998." },
  { "en": "Apa Itu Negara Gagal (Failed State)?", "id": "Negara Tidak Mampu Jalankan Fungsinya." },
  { "en": "Indikator Negara Gagal?", "id": "Kehilangan Kontrol Wilayah, Erosi Legitimasi." },
  { "en": "Apa Itu Nation-Building?", "id": "Proses Membangun Identitas Nasional Bersama." },
  { "en": "Tantangan Nation-Building Di Indonesia?", "id": "Keberagaman Suku, Agama, Ras, Antargolongan (SARA)." },
  { "en": "Apa Itu Integrasi Nasional?", "id": "Penyatuan Berbagai Kelompok Dalam Kesatuan." },
  { "en": "Faktor Pendorong Integrasi Nasional?", "id": "Pancasila, Sumpah Pemuda, Rasa Senasib." },
  { "en": "Faktor Penghambat Integrasi Nasional?", "id": "Etnosentrisme Dan Ketidakpuasan Ekonomi." },
  { "en": "Apa Itu Etnosentrisme?", "id": "Menganggap Budaya Sendiri Lebih Baik." },
  { "en": "Apa Itu Politik Anggaran?", "id": "Proses Alokasi Sumber Daya Publik." },
  { "en": "Siapa Yang Membahas Politik Anggaran?", "id": "Pemerintah Bersama Dengan Dewan Perwakilan Rakyat (DPR)." },
  { "en": "Fungsi Anggaran Pendapatan Dan Belanja Negara (APBN)?", "id": "Alokasi, Distribusi, Dan Stabilisasi." },
  { "en": "Sumber Penerimaan Negara?", "id": "Pajak, Penerimaan Negara Bukan Pajak (PNBP)." },
  { "en": "Apa Itu Defisit Anggaran?", "id": "Belanja Lebih Besar Dari Penerimaan." },
  { "en": "Bagaimana Menutup Defisit Anggaran?", "id": "Melalui Pinjaman Atau Utang Negara." },
  { "en": "Apa Itu Money Politics (Politik Uang)?", "id": "Mempengaruhi Pilihan Pemilih Dengan Uang." },
  { "en": "Dampak Money Politics?", "id": "Mencederai Demokrasi Dan Melahirkan Korupsi." },
  { "en": "Apa Itu Black Campaign (Kampanye Hitam)?", "id": "Menyerang Lawan Politik Dengan Isu Negatif." },
  { "en": "Perbedaan Kampanye Hitam Dan Negatif?", "id": "Kampanye Hitam Berbasis Fitnah." },
  { "en": "Lembaga Survei Dalam Politik?", "id": "Mengukur Opini Publik Dan Elektabilitas." },
  { "en": "Apa Itu Quick Count (Hitung Cepat)?", "id": "Proyeksi Hasil Pemilu Berbasis Sampel." },
  { "en": "Apa Itu Real Count?", "id": "Penghitungan Suara Resmi Oleh Komisi Pemilihan Umum (KPU)." },
  { "en": "Sengketa Hasil Pemilu Diselesaikan Di?", "id": "Mahkamah Konstitusi (MK)." },
  { "en": "Apa Itu Hak Angket?", "id": "Hak Dewan Perwakilan Rakyat (DPR) Menyelidiki Kebijakan Pemerintah." },
  { "en": "Apa Itu Mosi Tidak Percaya?", "id": "Pernyataan Parlemen Tidak Percaya Pemerintah." },
  { "en": "Apa Itu Kecamatan?", "id": "Wilayah Kerja Camat Sebagai Perangkat Daerah." },
  { "en": "Siapa Yang Mengangkat Camat?", "id": "Bupati Atau Wali Kota." },
  { "en": "Apa Itu Kelurahan?", "id": "Wilayah Kerja Lurah Perangkat Daerah." },
  { "en": "Siapa Yang Mengangkat Lurah?", "id": "Bupati Atau Wali Kota Atas Usul Camat." },
  { "en": "Perbedaan Desa Dan Kelurahan?", "id": "Kepala Desa Dipilih, Lurah Diangkat." },
  { "en": "Apa Itu Rukun Warga (RW)?", "id": "Lembaga Kemasyarakatan Di Bawah Kelurahan." },
  { "en": "Apa Itu Rukun Tetangga (RT)?", "id": "Lembaga Kemasyarakatan Di Bawah Rukun Warga (RW)." },
  { "en": "Tugas Camat?", "id": "Koordinasi Pemerintahan, Pemberdayaan, Pelayanan Publik." },
  { "en": "Apa Itu Standar Pelayanan Minimal (SPM)?", "id": "Ketentuan Jenis Dan Mutu Pelayanan Dasar." },
  { "en": "Tujuan Standar Pelayanan Minimal (SPM)?", "id": "Menjamin Akses Pelayanan Dasar Bagi Rakyat." },
  { "en": "Apa Itu Negara Kesejahteraan (Welfare State)?", "id": "Negara Berperan Dalam Kesejahteraan Warga." },
  { "en": "Prinsip Negara Kesejahteraan?", "id": "Kesempatan Sama, Distribusi Pendapatan, Bantuan." },
  { "en": "Apa Itu Sistem Jaminan Sosial Nasional (SJSN)?", "id": "Program Negara Jamin Kebutuhan Hidup." },
  { "en": "Penyelenggara Sistem Jaminan Sosial Nasional (SJSN)?", "id": "Badan Penyelenggara Jaminan Sosial (BPJS)." },
  { "en": "Jenis Badan Penyelenggara Jaminan Sosial (BPJS)?", "id": "Badan Penyelenggara Jaminan Sosial (BPJS) Kesehatan Dan Ketenagakerjaan." },
  { "en": "Apa Itu Absolutisme?", "id": "Bentuk Pemerintahan Dengan Kekuasaan Absolut." },
  { "en": "Apa Itu Res Publica?", "id": "Urusan Publik Atau Kepentingan Umum." },
  { "en": "Apa Itu Status Quo?", "id": "Keadaan Tetap Atau Tidak Berubah." },
  { "en": "Apa Itu Makar?", "id": "Upaya Menjatuhkan Pemerintah Yang Sah." },
  { "en": "Hukuman Tindak Pidana Makar?", "id": "Diatur Dalam Kitab Undang-Undang Hukum Pidana (KUHP)." },
  { "en": "Apa Itu Menteri Negara?", "id": "Pembantu Presiden Yang Memimpin Kementerian." },
  { "en": "Siapa Yang Mengangkat Menteri?", "id": "Presiden Republik Indonesia." },
  { "en": "Apa Itu Reshuffle Kabinet?", "id": "Perombakan Susunan Menteri Dalam Kabinet." },
  { "en": "Tujuan Reshuffle Kabinet?", "id": "Meningkatkan Kinerja Dan Efektivitas Pemerintah." },
  { "en": "Apa Itu Sekretariat Negara (Setneg)?", "id": "Lembaga Memberi Dukungan Teknis Presiden." },
  { "en": "Apa Itu Sekretariat Kabinet (Setkab)?", "id": "Lembaga Memberi Dukungan Rapat Kabinet." },
  { "en": "Apa Itu Kantor Staf Presiden (KSP)?", "id": "Lembaga Dukungan Pengendalian Program Prioritas." },
  { "en": "Apa Itu Dewan Pertimbangan Presiden (Wantimpres)?", "id": "Lembaga Memberi Nasihat Kepada Presiden." },
  { "en": "Apa Itu Veto?", "id": "Hak Membatalkan Keputusan Atau Undang-Undang." },
  { "en": "Apakah Presiden Indonesia Memiliki Hak Veto?", "id": "Tidak Memiliki Hak Veto Absolut." },
  { "en": "Apa Itu Konstituante?", "id": "Badan Pembentuk Konstitusi Atau Undang-Undang Dasar (UUD)." },
  { "en": "Kapan Sidang Konstituante Indonesia?", "id": "Berlangsung Dari Tahun 1956 Hingga 1959." },
  { "en": "Hasil Sidang Konstituante?", "id": "Gagal Menetapkan Undang-Undang Dasar (UUD) Baru." },
  { "en": "Apa Itu Dekrit Presiden 5 Juli 1959?", "id": "Membubarkan Konstituante, Kembali Ke Undang-Undang Dasar (UUD) 1945." },
  { "en": "Apa Itu Kabinet Parlementer?", "id": "Kabinet Bertanggung Jawab Kepada Parlemen." },
  { "en": "Apa Itu Kabinet Presidensial?", "id": "Kabinet Bertanggung Jawab Kepada Presiden." },
  { "en": "Apa Itu Kabinet Zaken?", "id": "Kabinet Terdiri Para Ahli Profesional." },
  { "en": "Apa Itu Gerontokrasi?", "id": "Pemerintahan Yang Dipimpin Orang Tua." },
  { "en": "Apa Itu Aristokrasi?", "id": "Pemerintahan Oleh Kaum Bangsawan." },
  { "en": "Apa Itu Plutokrasi?", "id": "Pemerintahan Yang Dijalankan Orang Kaya." },
  { "en": "Apa Itu Kleptokrasi?", "id": "Pemerintahan Yang Dijalankan Para Pencuri." },
  { "en": "Apa Itu Timokrasi?", "id": "Pemerintahan Berdasarkan Kehormatan Dan Ambisi." },
  { "en": "Apa Itu Ekologi Politik?", "id": "Studi Hubungan Politik Dan Lingkungan." },
  { "en": "Isu Utama Ekologi Politik?", "id": "Keadilan Lingkungan, Konflik Sumber Daya Alam." },
  { "en": "Apa Itu Green Politics (Politik Hijau)?", "id": "Ideologi Politik Berbasis Isu Lingkungan." },
  { "en": "Apa Itu Pembangunan Berkelanjutan?", "id": "Pembangunan Memenuhi Kebutuhan Generasi Sekarang." },
  { "en": "Tiga Pilar Pembangunan Berkelanjutan?", "id": "Ekonomi, Sosial, Dan Lingkungan." },
  { "en": "Apa Itu Amdal?", "id": "Analisis Mengenai Dampak Lingkungan Hidup." },
  { "en": "Tujuan Amdal?", "id": "Mencegah Pencemaran Dan Kerusakan Lingkungan." },
  { "en": "Siapa Yang Wajib Menyusun Amdal?", "id": "Pelaku Usaha Dengan Dampak Penting." },
  { "en": "Apa Itu Kebijakan Fiskal?", "id": "Kebijakan Pemerintah Terkait Anggaran Negara." },
  { "en": "Tujuan Kebijakan Fiskal?", "id": "Mempengaruhi Perekonomian Dan Stabilitas Harga." },
  { "en": "Apa Itu Kebijakan Moneter?", "id": "Kebijakan Bank Sentral Mengatur Uang." },
  { "en": "Tujuan Kebijakan Moneter?", "id": "Menjaga Kestabilan Nilai Mata Uang." },
  { "en": "Instrumen Kebijakan Moneter?", "id": "Suku Bunga, Cadangan Wajib, Operasi Pasar." },
  { "en": "Apa Itu Inflasi?", "id": "Kenaikan Harga Barang Jasa Umum." },
  { "en": "Apa Itu Deflasi?", "id": "Penurunan Harga Barang Jasa Umum." },
  { "en": "Apa Itu Resesi Ekonomi?", "id": "Penurunan Signifikan Aktivitas Ekonomi." },
  { "en": "Apa Itu Globalisasi?", "id": "Proses Integrasi Dunia Internasional." },
  { "en": "Dampak Positif Globalisasi Politik?", "id": "Penyebaran Nilai Demokrasi Dan Hak Asasi Manusia (HAM)." },
  { "en": "Dampak Negatif Globalisasi Politik?", "id": "Intervensi Asing Dan Melemahnya Kedaulatan." },
  { "en": "Apa Itu Terorisme?", "id": "Aksi Kekerasan Menciptakan Teror Ketakutan." },
  { "en": "Badan Nasional Penanggulangan Terorisme (BNPT)?", "id": "Lembaga Pemerintah Non-Kementerian (LPNK) Menangani Terorisme." },
  { "en": "Apa Itu Radikalisme?", "id": "Paham Menginginkan Perubahan Drastis." },
  { "en": "Bahaya Radikalisme?", "id": "Mengarah Pada Intoleransi Dan Aksi Kekerasan." },
  { "en": "Apa Itu Deradikalisasi?", "id": "Upaya Menetralisir Paham Radikal." },
  { "en": "Pentingnya Toleransi Dalam Politik?", "id": "Menjaga Persatuan Dalam Keberagaman." },
  { "en": "Apa Itu Disintegrasi Bangsa?", "id": "Perpecahan Atau Ancaman Keutuhan Bangsa." },
  { "en": "Penyebab Disintegrasi Bangsa?", "id": "Konflik Suku, Agama, Ras, Antargolongan (SARA), Kesenjangan, Ketidakadilan." },
  { "en": "Apa Itu Reintegrasi Sosial?", "id": "Upaya Membangun Kembali Persatuan." },
  { "en": "Peran Pemerintah Dalam Reintegrasi?", "id": "Fasilitator, Mediator, Dan Rekonsiliasi." },
  { "en": "Apa Itu Rekonsiliasi?", "id": "Proses Pemulihan Hubungan Setelah Konflik." },
  { "en": "Apa Itu Hak Pilih Aktif?", "id": "Hak Untuk Memilih Dalam Pemilu." },
  { "en": "Apa Itu Hak Pilih Pasif?", "id": "Hak Untuk Dipilih Dalam Pemilu." },
  { "en": "Syarat Pemilih Pemilu?", "id": "Warga Negara Indonesia (WNI), 17 Tahun, Terdaftar." },
  { "en": "Apa Itu Daftar Pemilih Tetap (DPT)?", "id": "Daftar Warga Yang Memenuhi Syarat." },
  { "en": "Apa Itu Tempat Pemungutan Suara (TPS)?", "id": "Tempat Pelaksanaan Pemungutan Suara Pemilu." },
  { "en": "Siapa Petugas Di Tempat Pemungutan Suara (TPS)?", "id": "Kelompok Penyelenggara Pemungutan Suara (KPPS)." },
  { "en": "Apa Itu Surat Suara?", "id": "Kertas Digunakan Untuk Memberikan Suara." },
  { "en": "Kapan Surat Suara Dinyatakan Sah?", "id": "Dicoblos Sesuai Ketentuan Yang Berlaku." },
  { "en": "Apa Itu Saksi Pemilu?", "id": "Pihak Mengawasi Proses Pemungutan Suara." },
  { "en": "Tugas Saksi Pemilu?", "id": "Memastikan Proses Berjalan Jujur Adil." },
  { "en": "Apa Itu Exit Poll?", "id": "Survei Dilakukan Setelah Pemilih Keluar." },
  { "en": "Tujuan Exit Poll?", "id": "Memprediksi Hasil Pemilu Secara Cepat." },
  { "en": "Penyelesaian Sengketa Proses Pemilu?", "id": "Melalui Badan Pengawas Pemilihan Umum (Bawaslu)." },
  { "en": "Apa Itu Tindak Pidana Pemilu?", "id": "Pelanggaran Hukum Dalam Proses Pemilu." },
  { "en": "Contoh Tindak Pidana Pemilu?", "id": "Politik Uang, Pemalsuan Suara." },
  { "en": "Lembaga Penegak Hukum Pidana Pemilu?", "id": "Sentra Penegakan Hukum Terpadu (Gakkumdu)." },
  { "en": "Apa Itu Pemungutan Suara Ulang (PSU)?", "id": "Pemilu Ulang Di Tempat Pemungutan Suara (TPS) Tertentu." },
  { "en": "Alasan Pemungutan Suara Ulang (PSU)?", "id": "Adanya Pelanggaran Atau Kerusakan Surat." },
  { "en": "Apa Itu Lembaga Swadaya Masyarakat (LSM)?", "id": "Organisasi Nirlaba Didirikan Oleh Masyarakat." },
  { "en": "Peran Lembaga Swadaya Masyarakat (LSM) Dalam Politik?", "id": "Melakukan Advokasi Dan Pengawasan Sosial." },
  { "en": "Apa Itu Otonomi Fiskal?", "id": "Kewenangan Daerah Mengelola Keuangan Sendiri." },
  { "en": "Indikator Otonomi Fiskal?", "id": "Tingkat Ketergantungan Daerah Pada Pusat." },
  { "en": "Apa Itu Pinjaman Daerah?", "id": "Sumber Pembiayaan Pembangunan Daerah." },
  { "en": "Jenis Pinjaman Daerah?", "id": "Jangka Pendek, Menengah, Dan Panjang." },
  { "en": "Apa Itu Obligasi Daerah?", "id": "Surat Utang Jangka Menengah Panjang." },
  { "en": "Tujuan Penerbitan Obligasi Daerah?", "id": "Membiayai Proyek Investasi Publik." },
  { "en": "Apa Itu Kerja Sama Daerah?", "id": "Kesepakatan Antar Daerah Dalam Urusan." },
  { "en": "Bentuk Kerja Sama Daerah?", "id": "Kerja Sama Wajib Dan Sukarela." },
  { "en": "Apa Itu Badan Kerja Sama Antar Daerah?", "id": "Wadah Mengkoordinasikan Pelaksanaan Kerja Sama." },
  { "en": "Apa Itu Kota Tanpa Otonomi?", "id": "Kota Administratif Yang Bagian Dari Kabupaten." },
  { "en": "Apa Itu Teokrasi?", "id": "Pemerintahan Berdasarkan Hukum Agama." },
  { "en": "Apakah Indonesia Negara Agama?", "id": "Bukan Negara Agama, Berdasar Pancasila." },
  { "en": "Hubungan Agama Dan Negara?", "id": "Negara Menjamin Kebebasan Beragama." },
  { "en": "Dasar Hukum Kebebasan Beragama?", "id": "Pasal 29 Undang-Undang Dasar (UUD) 1945." },
  { "en": "Lembaga Yang Mengurus Agama?", "id": "Kementerian Agama Republik Indonesia." },
  { "en": "Apa Itu Eksekutif Kuat (Strong Executive)?", "id": "Kekuasaan Presiden Sangat Dominan." },
  { "en": "Apa Itu Legislatif Kuat (Strong Legislative)?", "id": "Kekuasaan Parlemen Sangat Dominan." },
  { "en": "Apa Itu Mosi?", "id": "Keputusan Rapat Parlemen Atau Organisasi." },
  { "en": "Apa Itu Hak Inisiatif?", "id": "Hak Dewan Perwakilan Rakyat (DPR) Mengajukan Rancangan Undang-Undang (RUU)." },
  { "en": "Tahapan Pembentukan Undang-Undang?", "id": "Perencanaan, Penyusunan, Pembahasan, Pengesahan." },
  { "en": "Siapa Yang Mengesahkan Undang-Undang?", "id": "Presiden Republik Indonesia." },
  { "en": "Kapan Undang-Undang Mulai Berlaku?", "id": "Sejak Tanggal Diundangkan Dalam Lembaran." },
  { "en": "Apa Itu Lembaran Negara?", "id": "Media Resmi Mengundangkan Peraturan." },
  { "en": "Apa Itu Berita Negara?", "id": "Media Resmi Mengumumkan Peristiwa Resmi." },
  { "en": "Apa Itu Asas Legalitas?", "id": "Tiada Pidana Tanpa Peraturan Terlebih Dahulu." },
  { "en": "Apa Itu Asas Praduga Tak Bersalah?", "id": "Seseorang Dianggap Tak Bersalah." },
  { "en": "Apa Itu Bantuan Hukum?", "id": "Jasa Hukum Diberikan Cuma-cuma." },
  { "en": "Siapa Yang Berhak Mendapat Bantuan Hukum?", "id": "Setiap Orang Miskin Yang Tersangkut." },
  { "en": "Apa Itu Komunisme?", "id": "Ideologi Penghapusan Hak Milik Pribadi." },
  { "en": "Apa Itu Sosialisme?", "id": "Ideologi Kepemilikan Kolektif Alat Produksi." },
  { "en": "Perbedaan Sosialisme Dan Komunisme?", "id": "Cara Mencapai Tujuan Akhir." },
  { "en": "Apa Itu Kapitalisme?", "id": "Sistem Ekonomi Berbasis Modal Swasta." },
  { "en": "Sistem Ekonomi Indonesia?", "id": "Sistem Ekonomi Pancasila." },
  { "en": "Ciri Ekonomi Pancasila?", "id": "Asas Kekeluargaan Dan Gotong Royong." },
  { "en": "Apa Itu Badan Usaha Milik Negara (BUMN)?", "id": "Badan Usaha Milik Pemerintah Pusat." },
  { "en": "Peran Badan Usaha Milik Negara (BUMN)?", "id": "Menyediakan Barang Jasa Bagi Publik." },
  { "en": "Apa Itu Swastanisasi Atau Privatisasi?", "id": "Penjualan Badan Usaha Milik Negara (BUMN) Kepada Pihak Swasta." },
  { "en": "Tujuan Privatisasi?", "id": "Meningkatkan Efisiensi Dan Penerimaan Negara." },
  { "en": "Apa Itu Indeks Persepsi Korupsi (IPK)?", "id": "Ukuran Tingkat Korupsi Sektor Publik." },
  { "en": "Siapa Yang Merilis Indeks Persepsi Korupsi (IPK)?", "id": "Transparency International." },
  { "en": "Apa Itu Gratifikasi?", "id": "Pemberian Dalam Arti Luas." },
  { "en": "Apakah Semua Gratifikasi Dilarang?", "id": "Tidak, Jika Dilaporkan Kepada Komisi Pemberantasan Korupsi (KPK)." },
  { "en": "Apa Itu Whistleblower?", "id": "Orang Yang Melaporkan Tindak Pelanggaran." },
  { "en": "Lembaga Perlindungan Saksi Dan Korban (LPSK)?", "id": "Memberikan Perlindungan Kepada Saksi Korban." },
  { "en": "Apa Itu Laporan Harta Kekayaan Penyelenggara Negara (LHKPN)?", "id": "Laporan Harta Milik Penyelenggara Negara." },
  { "en": "Tujuan Laporan Harta Kekayaan Penyelenggara Negara (LHKPN)?", "id": "Mencegah Korupsi Dan Akumulasi Harta." },
  { "en": "Apa Itu Ombudsman?", "id": "Lembaga Negara Pengawas Pelayanan Publik." },
  { "en": "Apa Itu Maladministrasi?", "id": "Perilaku Atau Perbuatan Melawan Hukum." },
  { "en": "Contoh Maladministrasi?", "id": "Penundaan Berlarut, Penyalahgunaan Wewenang." },
  { "en": "Apa Itu Demografi?", "id": "Ilmu Mempelajari Dinamika Kependudukan." },
  { "en": "Apa Itu Bonus Demografi?", "id": "Jumlah Penduduk Usia Produktif Dominan." },
  { "en": "Tantangan Bonus Demografi?", "id": "Penyediaan Lapangan Kerja Dan Pendidikan." },
  { "en": "Apa Itu Pembangunan Manusia?", "id": "Proses Perluasan Pilihan Bagi Manusia." },
  { "en": "Apa Itu Indeks Pembangunan Manusia (IPM)?", "id": "Ukuran Capaian Pembangunan Manusia." },
  { "en": "Komponen Indeks Pembangunan Manusia (IPM)?", "id": "Kesehatan, Pendidikan, Dan Standar Hidup." },
  { "en": "Apa Itu Kemiskinan?", "id": "Ketidakmampuan Memenuhi Kebutuhan Dasar." },
  { "en": "Jenis Kemiskinan?", "id": "Kemiskinan Absolut Dan Relatif." },
  { "en": "Apa Itu Kesenjangan Ekonomi?", "id": "Distribusi Pendapatan Yang Tidak Merata." },
  { "en": "Apa Itu Gini Ratio?", "id": "Ukuran Ketimpangan Distribusi Pendapatan." },
  { "en": "Apa Itu Politik Luar Negeri?", "id": "Kebijakan Negara Mengatur Hubungan." },
  { "en": "Apa Itu Diplomasi?", "id": "Seni Berunding Antar Negara." },
  { "en": "Jenis Diplomasi?", "id": "Bilateral, Multilateral, Dan Diplomasi Publik." },
  { "en": "Apa Itu Perjanjian Internasional?", "id": "Kesepakatan Tertulis Antar Subjek Hukum." },
  { "en": "Tahapan Perjanjian Internasional?", "id": "Perundingan, Penandatanganan, Dan Ratifikasi." },
  { "en": "Apa Itu Ratifikasi?", "id": "Pengesahan Perjanjian Internasional Oleh Negara." },
  { "en": "Apa Itu Perserikatan Bangsa-Bangsa (PBB)?", "id": "Organisasi Internasional Menjaga Perdamaian." },
  { "en": "Organ Utama Perserikatan Bangsa-Bangsa (PBB)?", "id": "Majelis Umum, Dewan Keamanan, Sekretariat." },
  { "en": "Apa Itu Dewan Keamanan Perserikatan Bangsa-Bangsa (PBB)?", "id": "Badan Penjaga Perdamaian Keamanan." },
  { "en": "Anggota Tetap Dewan Keamanan?", "id": "Amerika Serikat, Rusia, Tiongkok, Inggris, Prancis." },
  { "en": "Apa Itu Hak Veto Dewan Keamanan?", "id": "Hak Membatalkan Rancangan Resolusi." },
  { "en": "Apa Itu Mahkamah Internasional (ICJ)?", "id": "Badan Kehakiman Utama Perserikatan Bangsa-Bangsa (PBB)." },
  { "en": "Fungsi Mahkamah Internasional (ICJ)?", "id": "Menyelesaikan Sengketa Hukum Antar Negara." },
  { "en": "Apa Itu Hukum Humaniter Internasional?", "id": "Hukum Yang Berlaku Dalam Sengketa." },
  { "en": "Apa Itu Konvensi Jenewa?", "id": "Aturan Perlindungan Korban Perang." },
  { "en": "Apa Itu Kejahatan Perang?", "id": "Pelanggaran Berat Terhadap Hukum Humaniter." },
  { "en": "Apa Itu Genosida?", "id": "Tindakan Pembasmian Etnis Atau Kelompok." },
  { "en": "Apa Itu Kejahatan Kemanusiaan?", "id": "Serangan Meluas Terhadap Penduduk Sipil." },
  { "en": "Pengadilan Kejahatan Internasional?", "id": "Mahkamah Pidana Internasional (ICC)." },
  { "en": "Apa Itu Populisme?", "id": "Gaya Politik Klaim Wakil Rakyat." },
  { "en": "Ciri Politik Populis?", "id": "Anti-Elit, Anti-Korupsi, Nasionalis." },
  { "en": "Apa Itu Hoax?", "id": "Informasi Palsu Yang Sengaja Dibuat." },
  { "en": "Bahaya Hoax Dalam Politik?", "id": "Menyesatkan Opini Dan Memicu Konflik." },
  { "en": "Apa Itu Post-Truth (Pasca-Kebenaran)?", "id": "Fakta Objektif Kurang Berpengaruh." },
  { "en": "Penyebab Era Post-Truth?", "id": "Dominasi Media Sosial Dan Emosi." },
  { "en": "Apa Itu Cybercrime?", "id": "Kejahatan Yang Dilakukan Di Dunia Maya." },
  { "en": "Undang-Undang Mengatur Transaksi Elektronik?", "id": "Undang-Undang Informasi Dan Transaksi Elektronik (ITE)." },
  { "en": "Apa Itu Oposisi Konstruktif?", "id": "Oposisi Yang Memberikan Kritik Solutif." },
  { "en": "Apa Itu Kooptasi Politik?", "id": "Proses Penyerapan Unsur Oposisi." },
  { "en": "Apa Itu Hegemoni?", "id": "Dominasi Satu Kelompok Atas Kelompok Lain." },
  { "en": "Hegemoni Menurut Antonio Gramsci?", "id": "Dominasi Melalui Ideologi Dan Budaya." },
  { "en": "Apa Itu Alienasi Politik?", "id": "Perasaan Terasing Dari Proses Politik." },
  { "en": "Penyebab Alienasi Politik?", "id": "Merasa Tidak Diwakili Dan Tidak Berdaya." },
  { "en": "Apa Itu Kekuasaan?", "id": "Kemampuan Mempengaruhi Pihak Lain." },
  { "en": "Apa Itu Perimbangan Kekuasaan?", "id": "Pembagian Kekuasaan Antar Lembaga Negara." },
  { "en": "Apa Itu Pemimpin Informal?", "id": "Tokoh Dihormati Tanpa Jabatan Formal." },
  { "en": "Contoh Pemimpin Informal?", "id": "Tokoh Agama, Tokoh Adat, Cendekiawan." },
  { "en": "Peran Pemimpin Informal?", "id": "Menjembatani Komunikasi Pemerintah Dan Masyarakat." },
  { "en": "Apa Itu Rembug Desa?", "id": "Musyawarah Antara Pemerintah Desa." },
  { "en": "Tujuan Rembug Desa?", "id": "Menjaring Aspirasi Perencanaan Pembangunan Desa." },
  { "en": "Apa Itu Musrenbang?", "id": "Musyawarah Perencanaan Pembangunan." },
  { "en": "Tingkatan Musrenbang?", "id": "Desa, Kecamatan, Kabupaten, Provinsi, Nasional." },
  { "en": "Prinsip Otonomi Desa?", "id": "Rekognisi Dan Subsidiaritas." },
  { "en": "Arti Asas Rekognisi?", "id": "Pengakuan Terhadap Hak Asal-Usul Desa." },
  { "en": "Arti Asas Subsidiaritas?", "id": "Kewenangan Skala Lokal Ditangani Desa." },
  { "en": "Apa Itu Sistem Pemerintahan Campuran?", "id": "Gabungan Sistem Presidensial Dan Parlementer." },
  { "en": "Negara Dengan Sistem Campuran?", "id": "Prancis, Rusia, Finlandia." },
  { "en": "Apa Itu Mosi Percaya?", "id": "Dukungan Parlemen Terhadap Pemerintah." },
  { "en": "Apa Itu Perdana Menteri?", "id": "Kepala Pemerintahan Dalam Sistem Parlementer." },
  { "en": "Siapa Yang Memilih Perdana Menteri?", "id": "Biasanya Dipilih Oleh Parlemen." },
  { "en": "Apa Itu Kanselir?", "id": "Sebutan Kepala Pemerintahan Di Jerman." },
  { "en": "Apa Itu Checks and Balances?", "id": "Mekanisme Saling Kontrol Antar Lembaga." },
  { "en": "Contoh Checks and Balances Di Indonesia?", "id": "Dewan Perwakilan Rakyat (DPR) Mengawasi Presiden." },
  { "en": "Apa Itu Hak Imunitas?", "id": "Hak Kekebalan Hukum Pejabat Negara." },
  { "en": "Tujuan Hak Imunitas?", "id": "Melindungi Pejabat Dari Tuntutan." },
  { "en": "Batasan Hak Imunitas?", "id": "Tidak Berlaku Untuk Tindak Pidana." },
  { "en": "Apa Itu Hukum Tata Usaha Negara (HTUN)?", "id": "Mengatur Hubungan Pemerintah Dan Warga." },
  { "en": "Objek Sengketa Tata Usaha Negara?", "id": "Keputusan Pejabat Tata Usaha Negara." },
  { "en": "Pengadilan Tata Usaha Negara (PTUN)?", "id": "Mengadili Sengketa Tata Usaha Negara." },
  { "en": "Apa Itu Fiktif Positif?", "id": "Permohonan Dianggap Dikabulkan Secara Hukum." },
  { "en": "Apa Itu Asas Contrarius Actus?", "id": "Pejabat Mengeluarkan Keputusan, Juga Berwenang." },
  { "en": "Apa Itu Diskresi?", "id": "Kebebasan Bertindak Pejabat Pemerintahan." },
  { "en": "Syarat Penggunaan Diskresi?", "id": "Sesuai Tujuan, Tidak Bertentangan Hukum." },
  { "en": "Apa Itu Demoralisasi Politik?", "id": "Penurunan Moralitas Dalam Kehidupan Politik." },
  { "en": "Penyebab Demoralisasi Politik?", "id": "Korupsi, Politik Uang, Janji Palsu." },
  { "en": "Apa Itu Etika Politik?", "id": "Nilai Moral Dalam Praktik Politik." },
  { "en": "Pentingnya Etika Politik?", "id": "Mewujudkan Pemerintahan Bersih Dan Bermartabat." },
  { "en": "Apa Itu Identitas Nasional?", "id": "Jati Diri Atau Ciri Khas Bangsa." },
  { "en": "Unsur Pembentuk Identitas Nasional?", "id": "Bahasa, Bendera, Lagu Kebangsaan, Lambang." },
  { "en": "Apa Itu Multikulturalisme?", "id": "Paham Mengakui Dan Menghargai Keragaman." },
  { "en": "Perbedaan Multikulturalisme Dan Pluralisme?", "id": "Multikulturalisme Menekankan Kesetaraan Budaya." },
  { "en": "Apa Itu Asimilasi?", "id": "Peleburan Dua Kebudayaan Atau Lebih." },
  { "en": "Apa Itu Akulturasi?", "id": "Percampuran Budaya Tanpa Hilangnya Ciri." },
  { "en": "Apa Itu Stereotip?", "id": "Penilaian Terhadap Seseorang Berbasis Kelompok." },
  { "en": "Apa Itu Prasangka (Prejudice)?", "id": "Sikap Negatif Terhadap Kelompok Tertentu." },
  { "en": "Apa Itu Diskriminasi?", "id": "Perlakuan Berbeda Berdasarkan Karakteristik Tertentu." },
  { "en": "Apa Itu Affirmative Action (Aksi Afirmatif)?", "id": "Kebijakan Memberi Kesempatan Kelompok Minoritas." },
  { "en": "Tujuan Aksi Afirmatif?", "id": "Mengatasi Diskriminasi Struktural Masa Lalu." },
  { "en": "Apa Itu Civic Education (Pendidikan Kewarganegaraan)?", "id": "Pendidikan Membentuk Warga Negara Baik." },
  { "en": "Tujuan Pendidikan Kewarganegaraan?", "id": "Meningkatkan Kesadaran Hak Dan Kewajiban." },
  { "en": "Apa Itu Anomi?", "id": "Keadaan Tanpa Norma Atau Aturan." },
  { "en": "Apa Itu Konformitas?", "id": "Perilaku Sesuai Dengan Norma Kelompok." },
  { "en": "Apa Itu Inovasi (Dalam Sosiologi)?", "id": "Menerima Tujuan, Menolak Cara Konvensional." },
  { "en": "Apa Itu Ritualisme?", "id": "Menolak Tujuan, Menerima Cara Konvensional." },
  { "en": "Apa Itu Pemberontakan (Rebellion)?", "id": "Menolak Tujuan Dan Cara Konvensional." },
  { "en": "Apa Itu Kontrol Sosial?", "id": "Upaya Mencegah Penyimpangan Sosial." },
  { "en": "Jenis Kontrol Sosial?", "id": "Preventif, Represif, Persuasif, Koersif." },
  { "en": "Apa Itu Lembaga Politik?", "id": "Wadah Aktivitas Politik Dalam Masyarakat." },
  { "en": "Fungsi Laten Lembaga Politik?", "id": "Fungsi Yang Tidak Disadari Langsung." },
  { "en": "Contoh Fungsi Laten?", "id": "Partai Politik Sebagai Ajang Sosialisasi." },
  { "en": "Apa Itu Agenda Setting?", "id": "Kemampuan Media Mempengaruhi Isu Publik." },
  { "en": "Apa Itu Framing?", "id": "Cara Media Membingkai Suatu Berita." },
  { "en": "Apa Itu Priming?", "id": "Media Mengarahkan Perhatian Pada Isu." },
  { "en": "Apa Itu Spin Doctor?", "id": "Pakar Komunikasi Memanipulasi Opini Publik." },
  { "en": "Peran Buzzer Dalam Politik?", "id": "Mendengungkan Isu Tertentu Di Media Sosial." },
  { "en": "Apa Itu Influencer Politik?", "id": "Tokoh Berpengaruh Mempromosikan Pandangan Politik." },
  { "en": "Apa Itu E-Voting?", "id": "Pemungutan Suara Menggunakan Perangkat Elektronik." },
  { "en": "Kelebihan E-Voting?", "id": "Cepat, Akurat, Dan Efisien." },
  { "en": "Tantangan E-Voting?", "id": "Keamanan Siber Dan Kesiapan Infrastruktur." },
  { "en": "Apa Itu Smart City?", "id": "Konsep Kota Cerdas Berbasis Teknologi." },
  { "en": "Tujuan Smart City?", "id": "Meningkatkan Kualitas Hidup Dan Pelayanan." },
  { "en": "Pilar Smart City?", "id": "Smart Governance, Economy, Living, Environment." },
  { "en": "Apa Itu Otorita Ibu Kota Nusantara (IKN)?", "id": "Lembaga Setingkat Kementerian Urus Ibu Kota Nusantara (IKN)." },
  { "en": "Siapa Kepala Otorita Ibu Kota Nusantara (IKN)?", "id": "Diangkat Dan Diberhentikan Oleh Presiden." },
  { "en": "Sumber Pendanaan Ibu Kota Nusantara (IKN)?", "id": "Anggaran Pendapatan Dan Belanja Negara (APBN), Swasta, Kerja Sama Pemerintah Dan Badan Usaha (KPBU)." },
  { "en": "Landasan Hukum Pemindahan Ibu Kota?", "id": "Undang-Undang Nomor 3 Tahun 2022." },
  { "en": "Apa Itu Pemilihan Umum Sela?", "id": "Pemilu Mengisi Kekosongan Jabatan." },
  { "en": "Penyebab Pemilu Sela?", "id": "Pejabat Meninggal, Mengundurkan Diri, Diberhentikan." },
  { "en": "Apa Itu Recall Politik?", "id": "Penarikan Kembali Anggota Legislatif." },
  { "en": "Siapa Yang Berhak Melakukan Recall?", "id": "Partai Politik Yang Mengusungnya." },
  { "en": "Alasan Recall Politik?", "id": "Melanggar Aturan Partai Atau Tak Aspiratif." },
  { "en": "Apa Itu Ambang Batas Pencalonan (Threshold)?", "id": "Syarat Dukungan Untuk Mencalonkan Diri." },
  { "en": "Apa Itu Nepotisme?", "id": "Mengutamakan Kerabat Dalam Jabatan." },
  { "en": "Apa Itu Kolusi?", "id": "Kerja Sama Rahasia Melawan Hukum." },
  { "en": "Korupsi, Kolusi, Dan Nepotisme (KKN)?", "id": "Tiga Praktik Yang Merusak Pemerintahan." },
  { "en": "Dasar Hukum Pemberantasan Korupsi, Kolusi, Dan Nepotisme (KKN)?", "id": "Ketetapan Majelis Permusyawaratan Rakyat (TAP MPR) Nomor XI/MPR/1998." },
  { "en": "Apa Itu Referendum?", "id": "Pemungutan Suara Rakyat Secara Langsung." },
  { "en": "Jenis Referendum?", "id": "Wajib, Tidak Wajib, Dan Fakultatif." },
  { "en": "Pernahkah Indonesia Mengadakan Referendum?", "id": "Pernah, Penentuan Pendapat Rakyat (Pepera) 1969." },
  { "en": "Apa Itu Separasi Kekuasaan?", "id": "Pemisahan Kekuasaan Secara Tegas." },
  { "en": "Perbedaan Separasi Dan Distribusi Kekuasaan?", "id": "Separasi Tegas, Distribusi Bersifat Koordinatif." },
  { "en": "Negara Penganut Separasi Kekuasaan?", "id": "Amerika Serikat." },
  { "en": "Apa Itu Sentimen Publik?", "id": "Kecenderungan Opini Masyarakat Terhadap Isu." },
  { "en": "Apa Itu Isu Strategis?", "id": "Isu Penting Mempengaruhi Masa Depan." },
  { "en": "Manajemen Isu Dalam Politik?", "id": "Proses Mengelola Isu Agar Menguntungkan." },
  { "en": "Apa Itu Government Shutdown?", "id": "Penghentian Layanan Pemerintah Akibat Anggaran." },
  { "en": "Penyebab Government Shutdown?", "id": "Tidak Adanya Kesepakatan Anggaran." },
  { "en": "Apa Itu Lobi Politik?", "id": "Upaya Mempengaruhi Pengambilan Keputusan Politik." },
  { "en": "Apa Itu Kewenangan Atribusi?", "id": "Kewenangan Yang Melekat Pada Jabatan." },
  { "en": "Apa Itu Kewenangan Delegasi?", "id": "Pelimpahan Wewenang Dari Pejabat Atas." },
  { "en": "Apa Itu Kewenangan Mandat?", "id": "Melaksanakan Wewenang Atas Nama Pemberi." },
  { "en": "Apa Itu Peraturan Kebijakan (Beleidsregel)?", "id": "Aturan Tidak Langsung Mengikat Umum." },
  { "en": "Fungsi Peraturan Kebijakan?", "id": "Pedoman Internal Bagi Aparat Pemerintah." },
  { "en": "Perbedaan Otonomi Dan Medebewind?", "id": "Otonomi Urusan Sendiri, Medebewind Tugas." },
  { "en": "Apa Itu Rumah Tangga Daerah?", "id": "Keseluruhan Penyelenggaraan Urusan Pemerintahan." },
  { "en": "Apa Itu Pajak Progresif?", "id": "Tarif Pajak Meningkat Dengan Dasar." },
  { "en": "Contoh Pajak Progresif?", "id": "Pajak Penghasilan (PPh) Dan Pajak Kendaraan." },
  { "en": "Apa Itu Tax Ratio?", "id": "Perbandingan Penerimaan Pajak Terhadap Produk Domestik Bruto (PDB)." },
  { "en": "Fungsi Tax Ratio?", "id": "Mengukur Kinerja Perpajakan Suatu Negara." },
  { "en": "Apa Itu Amnesti Pajak (Tax Amnesty)?", "id": "Pengampunan Pajak Dengan Syarat Tertentu." },
  { "en": "Tujuan Amnesti Pajak?", "id": "Meningkatkan Penerimaan Pajak Dan Kepatuhan." },
  { "en": "Apa Itu Unifikasi Negara?", "id": "Proses Penyatuan Wilayah Menjadi Negara." },
  { "en": "Contoh Unifikasi Negara?", "id": "Penyatuan Jerman Pada Abad Ke-19." },
  { "en": "Apa Itu Suksesi Negara?", "id": "Pergantian Kedaulatan Atas Suatu Wilayah." },
  { "en": "Bentuk Suksesi Negara?", "id": "Merger, Aneksasi, Disolusi, Separasi." },
  { "en": "Apa Itu Persona Non Grata?", "id": "Orang Yang Tidak Diterima (Diplomat)." },
  { "en": "Apa Itu Suzerenitas?", "id": "Hak Negara Mengontrol Negara Lain." },
  { "en": "Apa Itu Negara Protektorat?", "id": "Negara Di Bawah Perlindungan Negara Lain." },
  { "en": "Apa Itu Wilayah Enklave?", "id": "Wilayah Dikelilingi Wilayah Negara Lain." },
  { "en": "Apa Itu Wilayah Eksklave?", "id": "Wilayah Terpisah Dari Wilayah Utama." },
  { "en": "Apa Itu Parlemen Gantung (Hung Parliament)?", "id": "Tidak Ada Partai Mayoritas Mutlak." },
  { "en": "Solusi Parlemen Gantung?", "id": "Membentuk Pemerintahan Koalisi Atau Minoritas." },
  { "en": "Apa Itu Mosi Kepercayaan?", "id": "Ujian Dukungan Parlemen Terhadap Pemerintah." },
  { "en": "Apa Itu Shadow Cabinet (Kabinet Bayangan)?", "id": "Kabinet Dibentuk Partai Oposisi." },
  { "en": "Fungsi Kabinet Bayangan?", "id": "Mengawasi Kinerja Dan Menyiapkan Alternatif." },
  { "en": "Apa Itu Filibuster?", "id": "Taktik Mengulur Waktu Debat Parlemen." },
  { "en": "Apa Itu Gerrymandering?", "id": "Manipulasi Batas Daerah Pemilihan." },
  { "en": "Tujuan Gerrymandering?", "id": "Menguntungkan Partai Politik Tertentu." },
  { "en": "Apa Itu Politik Kartel?", "id": "Kerja Sama Partai Politik Membatasi." },
  { "en": "Ciri Politik Kartel?", "id": "Minimnya Oposisi Ideologis Yang Signifikan." },
  { "en": "Apa Itu Volatilitas Elektoral?", "id": "Tingkat Perubahan Perilaku Memilih." },
  { "en": "Penyebab Volatilitas Elektoral Tinggi?", "id": "Figur Calon, Isu Sesaat." },
  { "en": "Apa Itu Swing Voters (Pemilih Mengambang)?", "id": "Pemilih Belum Tentukan Pilihan." },
  { "en": "Karakteristik Swing Voters?", "id": "Cenderung Rasional Dan Fokus Pada Isu." },
  { "en": "Apa Itu Loyal Voters (Pemilih Loyal)?", "id": "Pemilih Setia Pada Partai Tertentu." },
  { "en": "Karakteristik Loyal Voters?", "id": "Memiliki Ikatan Ideologis Atau Emosional." },
  { "en": "Apa Itu Segmentasi Pemilih?", "id": "Pengelompokan Pemilih Berdasarkan Karakteristik." },
  { "en": "Dasar Segmentasi Pemilih?", "id": "Demografi, Geografi, Psikografi, Perilaku." },
  { "en": "Apa Itu Analisis SWOT Politik?", "id": "Analisis Kekuatan, Kelemahan, Peluang, Ancaman." },
  { "en": "Tujuan Analisis SWOT Politik?", "id": "Merumuskan Strategi Pemenangan Pemilu." },
  { "en": "Apa Itu Personal Branding Politik?", "id": "Upaya Membentuk Citra Diri Politisi." },
  { "en": "Unsur Personal Branding?", "id": "Karakter, Kompetensi, Dan Gaya Komunikasi." },
  { "en": "Apa Itu Krisis Politik?", "id": "Situasi Ketidakstabilan Politik Yang Serius." },
  { "en": "Penyebab Krisis Politik?", "id": "Skandal, Resesi Ekonomi, Konflik Elit." },
  { "en": "Apa Itu Transisi Politik?", "id": "Perubahan Rezim Atau Sistem Politik." },
  { "en": "Contoh Transisi Politik Indonesia?", "id": "Dari Orde Baru Ke Era Reformasi." },
  { "en": "Apa Itu Konsolidasi Demokrasi?", "id": "Proses Pematangan Sistem Demokrasi." },
  { "en": "Indikator Konsolidasi Demokrasi?", "id": "Supremasi Sipil, Pemilu Teratur, Budaya." },
  { "en": "Apa Itu Dekadensi Demokrasi?", "id": "Kemunduran Kualitas Praktik Demokrasi." },
  { "en": "Gejala Dekadensi Demokrasi?", "id": "Pembatasan Kebebasan, Pelemahan Oposisi." },
  { "en": "Apa Itu Politik Kesejahteraan?", "id": "Politik Yang Berfokus Pada Kesejahteraan." },
  { "en": "Instrumen Politik Kesejahteraan?", "id": "Jaminan Sosial, Subsidi, Pelayanan Publik." },
  { "en": "Apa Itu Universal Basic Income (UBI)?", "id": "Pendapatan Dasar Tanpa Syarat." },
  { "en": "Tujuan Universal Basic Income?", "id": "Mengurangi Kemiskinan Dan Ketimpangan." },
  { "en": "Apa Itu Negara Adidaya (Superpower)?", "id": "Negara Dengan Pengaruh Dominan Global." },
  { "en": "Apa Itu Bipolar World Order?", "id": "Tatanan Dunia Didominasi Dua Kekuatan." },
  { "en": "Contoh Bipolar World Order?", "id": "Era Perang Dingin (Amerika Serikat Dan Uni Soviet)." },
  { "en": "Apa Itu Unipolar World Order?", "id": "Tatanan Dunia Didominasi Satu Kekuatan." },
  { "en": "Apa Itu Multipolar World Order?", "id": "Tatanan Dunia Dengan Banyak Pusat." },
  { "en": "Apa Itu Soft Power?", "id": "Kemampuan Mempengaruhi Melalui Daya Tarik." },
  { "en": "Contoh Soft Power?", "id": "Budaya Populer, Diplomasi, Nilai-nilai." },
  { "en": "Apa Itu Hard Power?", "id": "Kemampuan Mempengaruhi Melalui Paksaan." },
  { "en": "Contoh Hard Power?", "id": "Kekuatan Militer Dan Sanksi Ekonomi." },
  { "en": "Apa Itu Smart Power?", "id": "Kombinasi Cerdas Soft Dan Hard Power." },
  { "en": "Apa Itu Perang Asimetris?", "id": "Perang Antara Pihak Berbeda Kekuatan." },
  { "en": "Contoh Perang Asimetris?", "id": "Perang Melawan Kelompok Teroris." },
  { "en": "Apa Itu Perang Proksi (Proxy War)?", "id": "Perang Antara Dua Pihak Ketiga." },
  { "en": "Apa Itu Intelijen Negara?", "id": "Informasi Untuk Keamanan Nasional." },
  { "en": "Fungsi Intelijen?", "id": "Penyelidikan, Pengamanan, Dan Penggalangan." },
  { "en": "Badan Intelijen Negara (BIN)?", "id": "Lembaga Intelijen Utama Indonesia." },
  { "en": "Apa Itu Kontra-Intelijen?", "id": "Kegiatan Melawan Operasi Intelijen Asing." },
  { "en": "Apa Itu Spionase?", "id": "Kegiatan Mata-mata Untuk Mencuri Informasi." },
  { "en": "Apa Itu Sabotase?", "id": "Tindakan Perusakan Terhadap Instalasi Strategis." },
  { "en": "Apa Itu Ketahanan Nasional?", "id": "Kondisi Dinamis Bangsa Hadapi Tantangan." },
  { "en": "Sifat Ketahanan Nasional?", "id": "Mandiri, Dinamis, Wibawa, Konsultatif." },
  { "en": "Asas Ketahanan Nasional?", "id": "Kesejahteraan Dan Keamanan." },
  { "en": "Lembaga Ketahanan Nasional (Lemhannas)?", "id": "Lembaga Pendidikan Pimpinan Nasional." },
  { "en": "Apa Itu Sistem Peringatan Dini?", "id": "Sistem Mendeteksi Potensi Ancaman." },
  { "en": "Pentingnya Sistem Peringatan Dini?", "id": "Memungkinkan Mitigasi Dan Respons Cepat." },
  { "en": "Apa Itu Konflik Horizontal?", "id": "Konflik Antar Kelompok Masyarakat." },
  { "en": "Penyebab Konflik Horizontal?", "id": "Suku, Agama, Ras, Antargolongan (SARA), Ekonomi, Dan Politik." },
  { "en": "Apa Itu Konflik Vertikal?", "id": "Konflik Antara Masyarakat Dan Pemerintah." },
  { "en": "Penyebab Konflik Vertikal?", "id": "Ketidakpuasan Kebijakan, Ketidakadilan, Pelanggaran Hak Asasi Manusia (HAM)." },
  { "en": "Apa Itu Manajemen Konflik?", "id": "Proses Mengelola Perbedaan Pendapat." },
  { "en": "Pendekatan Manajemen Konflik?", "id": "Negosiasi, Mediasi, Arbitrase, Konsiliasi." },
  { "en": "Apa Itu Mediasi?", "id": "Penyelesaian Sengketa Melalui Pihak Ketiga." },
  { "en": "Peran Mediator?", "id": "Netral, Fasilitator, Tidak Memutuskan." },
  { "en": "Apa Itu Arbitrase?", "id": "Penyelesaian Sengketa Oleh Arbiter." },
  { "en": "Sifat Putusan Arbitrase?", "id": "Final Dan Mengikat Para Pihak." },
  { "en": "Tujuan Pengawasan Daerah?", "id": "Menjamin Penyelenggaraan Sesuai Rencana." },
  { "en": "Siapa Pengawas Internal Pemerintah Daerah?", "id": "Inspektorat Daerah." },
  { "en": "Siapa Pengawas Eksternal Pemerintah Daerah?", "id": "Badan Pemeriksa Keuangan (BPK), Dewan Perwakilan Rakyat Daerah (DPRD), Masyarakat." },
  { "en": "Apa Itu Sanksi Administratif?", "id": "Sanksi Atas Pelanggaran Administrasi." },
  { "en": "Contoh Sanksi Administratif Daerah?", "id": "Pembatalan Perda Atau Keputusan." },
  { "en": "Apa Itu Daerah Perkotaan Metropolitan?", "id": "Kawasan Perkotaan Terdiri Beberapa Kota." },
  { "en": "Contoh Kawasan Metropolitan Indonesia?", "id": "Jabodetabek (Jakarta, Bogor, Depok, Tangerang, Bekasi)." },
  { "en": "Apa Itu Aglomerasi Perkotaan?", "id": "Kawasan Perkotaan Menyatu Secara Fisik." },
  { "en": "Tantangan Aglomerasi Perkotaan?", "id": "Koordinasi Pembangunan Dan Pelayanan Publik." },
  { "en": "Apa Itu Dana Darurat Daerah?", "id": "Dana Untuk Penanggulangan Bencana." },
  { "en": "Sumber Dana Darurat?", "id": "Anggaran Pendapatan Dan Belanja Negara (APBN) Dan Anggaran Pendapatan Dan Belanja Daerah (APBD)." },
  { "en": "Apa Itu Teori Perpisahan Kekuasaan?", "id": "Separation of Powers." },
  { "en": "Apa Itu Teori Pembagian Kekuasaan?", "id": "Distribution of Powers." },
  { "en": "Penganut Teori Perpisahan Kekuasaan?", "id": "Amerika Serikat." },
  { "en": "Penganut Teori Pembagian Kekuasaan?", "id": "Indonesia." },
  { "en": "Apa Itu Hukum Tata Negara Darurat?", "id": "Hukum Berlaku Saat Keadaan Bahaya." },
  { "en": "Siapa Yang Menyatakan Keadaan Bahaya?", "id": "Presiden Dengan Persetujuan Dewan Perwakilan Rakyat (DPR)." },
  { "en": "Hak-Hak Yang Dapat Dibatasi?", "id": "Hak Berkumpul, Berpendapat, Kebebasan Pers." },
  { "en": "Apa Itu Keppres (Keputusan Presiden)?", "id": "Penetapan Bersifat Konkret Dan Individual." },
  { "en": "Perbedaan Perpres Dan Keppres?", "id": "Perpres Mengatur, Keppres Menetapkan." },
  { "en": "Apa Itu Inpres (Instruksi Presiden)?", "id": "Perintah Presiden Kepada Bawahan." },
  { "en": "Sifat Instruksi Presiden (Inpres)?", "id": "Bersifat Internal Dalam Lingkup Eksekutif." },
  { "en": "Apa Itu Komisi Negara?", "id": "Lembaga Penunjang Kekuasaan Eksekutif." },
  { "en": "Contoh Komisi Negara?", "id": "Komisi Pemilihan Umum (KPU), Komisi Pemberantasan Korupsi (KPK), Komisi Nasional Hak Asasi Manusia (Komnas HAM)." },
  { "en": "Dasar Hukum Pembentukan Komisi Negara?", "id": "Undang-Undang Dasar (UUD) 1945 atau Undang-Undang." },
  { "en": "Apa Itu Kekuasaan Eksaminatif?", "id": "Kekuasaan Terkait Pemeriksaan Keuangan." },
  { "en": "Siapa Pemegang Kekuasaan Eksaminatif?", "id": "Badan Pemeriksa Keuangan (BPK)." },
  { "en": "Sifat Laporan Hasil Pemeriksaan (LHP) Badan Pemeriksa Keuangan (BPK)?", "id": "Bebas, Mandiri, Dan Profesional." },
  { "en": "Apa Itu Opini Badan Pemeriksa Keuangan (BPK)?", "id": "Pernyataan Profesional Atas Laporan Keuangan." },
  { "en": "Jenis Opini Badan Pemeriksa Keuangan (BPK)?", "id": "Wajar Tanpa Pengecualian (WTP), Wajar Dengan Pengecualian (WDP)." },
  { "en": "Apa Itu Tindak Lanjut Hasil Pemeriksaan?", "id": "Kewajiban Pejabat Untuk Menindaklanjuti." },
  { "en": "Partai Kader Adalah?", "id": "Partai Berorientasi Kualitas Anggota." },
  { "en": "Partai Massa Adalah?", "id": "Partai Berorientasi Kuantitas Anggota." },
  { "en": "Apa Itu Deideologisasi Partai?", "id": "Menurunnya Peran Ideologi Dalam Partai." },
  { "en": "Penyebab Deideologisasi Partai?", "id": "Pragmatisme Politik Dan Orientasi Kekuasaan." },
  { "en": "Apa Itu Politik Transaksional?", "id": "Pertukaran Dukungan Politik Dengan Materi." },
  { "en": "Dampak Politik Transaksional?", "id": "Biaya Politik Tinggi Dan Korupsi." },
  { "en": "Apa Itu Political Marketing?", "id": "Pemasaran Gagasan Dan Figur Politik." },
  { "en": "Strategi Political Marketing?", "id": "Segmentasi, Targeting, Positioning, Branding." },
  { "en": "Apa Itu Konsultan Politik?", "id": "Profesional Memberi Saran Strategi Politik." },
  { "en": "Peran Konsultan Politik?", "id": "Survei, Branding, Manajemen Kampanye." },
  { "en": "Apa Itu Elektabilitas?", "id": "Tingkat Keterpilihan Calon Dalam Pemilu." },
  { "en": "Faktor Mempengaruhi Elektabilitas?", "id": "Popularitas, Akseptabilitas, Dan Kompetensi." },
  { "en": "Apa Itu Popularitas?", "id": "Tingkat Dikenal Publik." },
  { "en": "Apa Itu Akseptabilitas?", "id": "Tingkat Diterima Publik." },
  { "en": "Apa Itu Sivilisasi Politik?", "id": "Proses Pematangan Sikap Berpolitik." },
  { "en": "Ciri Sivilisasi Politik?", "id": "Toleran, Rasional, Dan Menghargai Perbedaan." },
  { "en": "Apa Itu Absenteisme?", "id": "Ketidakhadiran Dalam Partisipasi Politik." },
  { "en": "Bentuk Absenteisme?", "id": "Tidak Memilih (Golput), Tidak Aktif." },
  { "en": "Apa Itu Radikalisme Politik?", "id": "Tuntutan Perubahan Politik Secara Ekstrem." },
  { "en": "Apa Itu Ekstremisme?", "id": "Paham Berlebihan Melampaui Batas." },
  { "en": "Apa Itu Fundamentalisme?", "id": "Gerakan Keagamaan Yang Sangat Konservatif." },
  { "en": "Apa Itu Sekularisme?", "id": "Pemisahan Urusan Agama Dan Negara." },
  { "en": "Apa Itu Negara Sekuler?", "id": "Negara Yang Tidak Menganut Agama." },
  { "en": "Hubungan Politik Dan Ekonomi?", "id": "Saling Mempengaruhi Secara Timbal Balik." },
  { "en": "Apa Itu Ekonomi Politik?", "id": "Studi Interaksi Antara Politik Ekonomi." },
  { "en": "Apa Itu Kebijakan Proteksionisme?", "id": "Melindungi Industri Dalam Negeri." },
  { "en": "Bentuk Proteksionisme?", "id": "Tarif Impor, Kuota, Dan Subsidi." },
  { "en": "Apa Itu Perdagangan Bebas?", "id": "Perdagangan Tanpa Hambatan Tarif." },
  { "en": "Organisasi Perdagangan Dunia?", "id": "World Trade Organization (WTO)." },
  { "en": "Apa Itu Masyarakat Ekonomi Asean (MEA)?", "id": "Integrasi Ekonomi Kawasan Asia Tenggara." },
  { "en": "Tujuan Masyarakat Ekonomi Asean (MEA)?", "id": "Menciptakan Pasar Tunggal Dan Basis." },
  { "en": "Apa Itu Diplomasi Ekonomi?", "id": "Diplomasi Untuk Mencapai Tujuan Ekonomi." },
  { "en": "Fokus Diplomasi Ekonomi?", "id": "Perdagangan, Investasi, Dan Pariwisata." },
  { "en": "Apa Itu Resolusi Konflik?", "id": "Proses Mengakhiri Suatu Konflik." },
  { "en": "Apa Itu Transformasi Konflik?", "id": "Mengubah Konflik Destruktif Menjadi Konstruktif." },
  { "en": "Apa Itu Peacebuilding (Pembangunan Perdamaian)?", "id": "Menciptakan Kondisi Damai Berkelanjutan." },
  { "en": "Apa Itu Peacekeeping (Pemeliharaan Perdamaian)?", "id": "Mencegah Konflik Kembali Terjadi." },
  { "en": "Apa Itu Peacemaking (Penciptaan Perdamaian)?", "id": "Menghentikan Konflik Melalui Negosiasi." },
  { "en": "Apa Itu Keadilan Transisional?", "id": "Keadilan Pasca-Konflik Atau Represi." },
  { "en": "Mekanisme Keadilan Transisional?", "id": "Pengadilan, Komisi Kebenaran, Reparasi." },
  { "en": "Apa Itu Komisi Kebenaran Dan Rekonsiliasi?", "id": "Mengungkap Kebenaran Pelanggaran Hak Asasi Manusia (HAM) Masa Lalu." },
  { "en": "Apa Itu Amnesia Kolektif?", "id": "Upaya Melupakan Tragedi Masa Lalu." },
  { "en": "Bahaya Amnesia Kolektif?", "id": "Mengulangi Kesalahan Sejarah Yang Sama." },
  { "en": "Pentingnya Memori Kolektif?", "id": "Pelajaran Bagi Generasi Mendatang." },
  { "en": "Apa Itu Hak Veto Rakyat?", "id": "Hak Rakyat Membatalkan Undang-Undang." },
  { "en": "Apa Itu Inisiatif Rakyat?", "id": "Hak Rakyat Mengajukan Rancangan Undang-Undang." },
  { "en": "Apa Itu Petisi?", "id": "Pernyataan Tertulis Kepada Pemerintah." },
  { "en": "Apa Itu Class Action?", "id": "Gugatan Hukum Oleh Sekelompok Orang." },
  { "en": "Apa Itu Citizen Lawsuit?", "id": "Gugatan Warga Negara Atas Kebijakan." },
  { "en": "Apa Itu Supremasi Sipil?", "id": "Militer Tunduk Pada Otoritas Sipil." },
  { "en": "Dwifungsi Angkatan Bersenjata Republik Indonesia (ABRI)?", "id": "Peran Pertahanan Dan Sosial Politik." },
  { "en": "Kapan Dwifungsi Angkatan Bersenjata Republik Indonesia (ABRI) Dihapuskan?", "id": "Secara Bertahap Sejak Era Reformasi." },
  { "en": "Reformasi Sektor Keamanan?", "id": "Penataan Ulang Sektor Keamanan." },
  { "en": "Tujuan Reformasi Sektor Keamanan?", "id":"Menciptakan Aparat Profesional, Demokratis." },
  { "en": "Peran Intelijen Dalam Demokrasi?", "id": "Melindungi Negara Tanpa Melanggar Hak Asasi Manusia (HAM)." },
  { "en": "Apa Itu Negara Polisi (Police State)?", "id": "Negara Mengawasi Ketat Kehidupan Warga." },
  { "en": "Apa Itu Machiavellianisme?", "id": "Politik Menghalalkan Segala Cara." },
  { "en": "Prinsip Machiavelli?", "id": "Lebih Baik Ditakuti Daripada Dicintai." },
  { "en": "Apa Itu Realpolitik?", "id": "Politik Berdasarkan Kepentingan Nasional." },
  { "en": "Apa Itu Balance of Power?", "id": "Keseimbangan Kekuatan Antar Negara." },
  { "en": "Tujuan Balance of Power?", "id": "Mencegah Hegemoni Satu Negara." },
  { "en": "Dasar Pertimbangan Pembentukan Daerah?", "id": "Kemampuan Ekonomi, Potensi, Luas Wilayah." },
  { "en": "Apa Itu Toponimi?", "id": "Penamaan Unsur Geografis." },
  { "en": "Pentingnya Toponimi Dalam Pemerintahan?", "id": "Kepastian Hukum Dan Tertib Administrasi." },
  { "en": "Apa Itu Batas Daerah?", "id": "Garis Batas Wilayah Antar Daerah." },
  { "en": "Siapa Yang Menetapkan Batas Daerah?", "id": "Menteri Dalam Negeri." },
  { "en": "Sengketa Batas Wilayah?", "id": "Diselesaikan Secara Musyawarah Atau Fasilitasi." },
  { "en": "Apa Itu Inovasi Daerah?", "id": "Pembaruan Dalam Penyelenggaraan Pemerintahan." },
  { "en": "Tujuan Inovasi Daerah?", "id": "Meningkatkan Kinerja Dan Kualitas Layanan." },
  { "en": "Apa Itu Indeks Inovasi Daerah?", "id": "Pengukuran Tingkat Inovasi Pemerintah Daerah." },
  { "en": "Apa Itu Otonomi Award?", "id": "Penghargaan Bagi Daerah Berkinerja Baik." },
  { "en": "Fungsi Legislasi Dewan Perwakilan Rakyat Daerah (DPRD)?", "id": "Membentuk Perda Bersama Kepala Daerah." },
  { "en": "Fungsi Anggaran Dewan Perwakilan Rakyat Daerah (DPRD)?", "id": "Membahas Dan Menyetujui Anggaran Pendapatan Dan Belanja Daerah (APBD)." },
  { "en": "Fungsi Pengawasan Dewan Perwakilan Rakyat Daerah (DPRD)?", "id": "Mengawasi Pelaksanaan Perda Dan Anggaran Pendapatan Dan Belanja Daerah (APBD)." },
  { "en": "Alat Kelengkapan Dewan Perwakilan Rakyat Daerah (DPRD)?", "id": "Pimpinan, Komisi, Badan Musyawarah (Bamus)." },
  { "en": "Apa Itu Fraksi?", "id": "Pengelompokan Anggota Dewan Perwakilan Rakyat Daerah (DPRD) Berdasarkan Partai." },
  { "en": "Apa Itu Hak Interpelasi Dewan Perwakilan Rakyat Daerah (DPRD)?", "id": "Hak Meminta Keterangan Kebijakan Daerah." },
  { "en": "Apa Itu Hak Angket Dewan Perwakilan Rakyat Daerah (DPRD)?", "id": "Hak Melakukan Penyelidikan Kebijakan Daerah." },
  { "en": "Apa Itu Hak Menyatakan Pendapat?", "id": "Hak Menyikapi Kebijakan Kepala Daerah." },
  { "en": "Hubungan Kepala Daerah Dan Dewan Perwakilan Rakyat Daerah (DPRD)?", "id": "Hubungan Kemitraan Sejajar." },
  { "en": "Pemberhentian Kepala Daerah?", "id": "Diusulkan Dewan Perwakilan Rakyat Daerah (DPRD) Kepada Presiden." },
  { "en": "Apa Itu Sistem Proporsional Tertutup?", "id": "Pemilih Hanya Memilih Partai Politik." },
  { "en": "Kelebihan Proporsional Tertutup?", "id": "Memperkuat Institusi Partai Politik." },
  { "en": "Kelemahan Proporsional Tertutup?", "id": "Rakyat Tidak Tahu Siapa Calonnya." },
  { "en": "Kelebihan Proporsional Terbuka?", "id": "Rakyat Dapat Memilih Langsung Calonnya." },
  { "en": "Kelemahan Proporsional Terbuka?", "id": "Mendorong Politik Uang Dan Popularitas." },
  { "en": "Apa Itu Sistem Pemilu Campuran?", "id": "Gabungan Sistem Distrik Dan Proporsional." },
  { "en": "Tujuan Sistem Pemilu Campuran?", "id": "Mengambil Kelebihan Dari Dua Sistem." },
  { "en": "Apa Itu Konversi Suara?", "id": "Proses Mengubah Suara Menjadi Kursi." },
  { "en": "Metode Konversi Suara?", "id": "Sainte-Lague Murni." },
  { "en": "Apa Itu Bilangan Pembagi Pemilih (BPP)?", "id": "Angka Untuk Menghitung Perolehan Kursi." },
  { "en": "Apa Itu Verifikasi Partai Politik?", "id": "Pemeriksaan Kelengkapan Syarat Partai." },
  { "en": "Siapa Yang Melakukan Verifikasi?", "id": "Komisi Pemilihan Umum (KPU)." },
  { "en": "Syarat Partai Politik Peserta Pemilu?", "id": "Berbadan Hukum, Punya Kantor, Anggota." },
  { "en": "Apa Itu Dana Kampanye?", "id": "Dana Untuk Kegiatan Kampanye Pemilu." },
  { "en": "Sumber Dana Kampanye?", "id": "Calon, Partai, Sumbangan Pihak Lain." },
  { "en": "Siapa Yang Mengaudit Dana Kampanye?", "id": "Kantor Akuntan Publik (KAP) Ditunjuk Komisi Pemilihan Umum (KPU)." },
  { "en": "Apa Itu Jurdil?", "id": "Jujur Dan Adil, Asas Pemilu." },
  { "en": "Apa Itu Luber?", "id": "Langsung, Umum, Bebas, Rahasia." },
  { "en": "Siapa Penemu Asas Luber?", "id": "Mohammad Hatta." },
  { "en": "Makna Langsung Dalam Pemilu?", "id": "Pemilih Memberikan Suara Tanpa Perantara." },
  { "en": "Makna Umum Dalam Pemilu?", "id": "Berlaku Bagi Semua Warga Syarat." },
  { "en": "Makna Bebas Dalam Pemilu?", "id": "Memilih Sesuai Hati Nurani." },
  { "en": "Makna Rahasia Dalam Pemilu?", "id": "Pilihan Pemilih Dijamin Kerahasiaannya." },
  { "en": "Makna Jujur Dalam Pemilu?", "id": "Penyelenggara, Peserta, Pemilih Jujur." },
  { "en": "Makna Adil Dalam Pemilu?", "id": "Perlakuan Sama Terhadap Peserta Pemilu." },
  { "en": "Kode Etik Penyelenggara Pemilu?", "id": "Pedoman Perilaku Bagi Penyelenggara." },
  { "en": "Siapa Pengawas Kode Etik?", "id": "Dewan Kehormatan Penyelenggara Pemilu (DKPP)." },
  { "en": "Sanksi Pelanggaran Kode Etik?", "id": "Teguran, Pemberhentian Sementara Atau Tetap." },
  { "en": "Apa Itu Pemantau Pemilu?", "id": "Lembaga Independen Memantau Proses Pemilu." },
  { "en": "Peran Pemantau Pemilu?", "id": "Meningkatkan Transparansi Dan Akuntabilitas Pemilu." },
  { "en": "Syarat Menjadi Pemantau Pemilu?", "id": "Independen, Punya Sumber Dana Jelas." },
  { "en": "Apa Itu Politik Aliran?", "id": "Keterikatan Pemilih Pada Kelompok Sosial." },
  { "en": "Contoh Politik Aliran Di Indonesia?", "id": "Santri, Abangan, Priyayi (Masa Lalu)." },
  { "en": "Apakah Politik Aliran Masih Relevan?", "id": "Mulai Melemah, Berganti Politik Rasional." },
  { "en": "Apa Itu Psikologi Politik?", "id": "Studi Perilaku Politik Dari Perspektif." },
  { "en": "Fokus Kajian Psikologi Politik?", "id": "Kepribadian Pemimpin, Perilaku Pemilih." },
  { "en": "Apa Itu Kognisi Politik?", "id": "Cara Individu Memproses Informasi Politik." },
  { "en": "Apa Itu Afeksi Politik?", "id": "Perasaan Atau Emosi Terhadap Politik." },
  { "en": "Pengaruh Emosi Dalam Politik?", "id": "Dapat Mendorong Atau Menghambat Partisipasi." },
  { "en": "Apa Itu Teori Pilihan Rasional?", "id": "Individu Bertindak Untuk Maksimalkan Manfaat." },
  { "en": "Aplikasi Teori Pilihan Rasional?", "id": "Pemilih Memilih Calon Paling Menguntungkan." },
  { "en": "Kritik Terhadap Pilihan Rasional?", "id": "Mengabaikan Faktor Emosi Dan Identitas." },
  { "en": "Apa Itu Ekonomi Politik Internasional?", "id": "Studi Interaksi Politik Ekonomi Global." },
  { "en": "Apa Itu Neoliberalisme?", "id": "Paham Ekonomi Tekankan Pasar Bebas." },
  { "en": "Ciri Kebijakan Neoliberal?", "id": "Privatisasi, Deregulasi, Dan Liberalisasi." },
  { "en": "Dampak Neoliberalisme?", "id": "Meningkatnya Pertumbuhan Dan Juga Ketimpangan." },
  { "en": "Apa Itu Lembaga Keuangan Internasional?", "id": "Contohnya International Monetary Fund (IMF) Dan World Bank." },
  { "en": "Fungsi International Monetary Fund (IMF)?", "id": "Menjaga Stabilitas Sistem Moneter Global." },
  { "en": "Fungsi World Bank (Bank Dunia)?", "id": "Memberikan Pinjaman Untuk Proyek Pembangunan." },
  { "en": "Kritik Terhadap International Monetary Fund (IMF) Dan World Bank?", "id": "Dianggap Menerapkan Kebijakan Neoliberal." },
  { "en": "Apa Itu Utang Luar Negeri?", "id": "Utang Pemerintah Kepada Pihak Asing." },
  { "en": "Risiko Utang Luar Negeri?", "id": "Beban Anggaran Pendapatan Dan Belanja Negara (APBN) Dan Ketergantungan." },
  { "en": "Apa Itu Bantuan Luar Negeri?", "id": "Transfer Sumber Daya Antar Negara." },
  { "en": "Jenis Bantuan Luar Negeri?", "id": "Hibah, Pinjaman Lunak, Bantuan Teknis." },
  { "en": "Motif Bantuan Luar Negeri?", "id": "Kemanusiaan, Ekonomi, Dan Politik." },
  { "en": "Apa Itu Regionalisme?", "id": "Kerja Sama Negara Dalam Satu Kawasan." },
  { "en": "Contoh Organisasi Regional?", "id": "Association of Southeast Asian Nations (ASEAN), Uni Eropa (EU)." },
  { "en": "Apa Itu Integrasi Regional?", "id": "Proses Penyatuan Ekonomi Politik." },
  { "en": "Tingkatan Integrasi Regional?", "id": "Kawasan Perdagangan Bebas, Uni Pabean." },
  { "en": "Apa Itu Non-Governmental Organization (NGO) Internasional?", "id": "Lembaga Swadaya Masyarakat (LSM) Beroperasi Lintas Negara." },
  { "en": "Contoh Non-Governmental Organization (NGO) Internasional?", "id": "Amnesty International, Greenpeace, WWF." },
  { "en": "Peran Non-Governmental Organization (NGO) Internasional?", "id": "Advokasi, Pengawasan, Dan Bantuan Kemanusiaan." },
  { "en": "Apa Itu Public Sphere (Ruang Publik)?", "id": "Arena Diskusi Isu Kepentingan Umum." },
  { "en": "Konsep Ruang Publik Menurut Habermas?", "id": "Ruang Antara Negara Dan Masyarakat." },
  { "en": "Peran Ruang Publik Dalam Demokrasi?", "id": "Membentuk Opini Publik Dan Kontrol." },
  { "en": "Tantangan Ruang Publik Saat Ini?", "id": "Fragmentasi, Polarisasi, Dan Berita Palsu." },
  { "en": "Apa Itu deliberative democracy (Demokrasi Deliberatif)?", "id": "Demokrasi Berbasis Diskusi Dan Argumentasi." },
  { "en": "Tujuan Demokrasi Deliberatif?", "id": "Mencapai Keputusan Lebih Rasional Legitimat." },
  { "en": "Apa Itu E-Democracy?", "id": "Penggunaan Teknologi Untuk Partisipasi Demokratis." },
  { "en": "Bentuk E-Democracy?", "id": "E-Voting, E-Consultation, E-Petition." },
  { "en": "Potensi E-Democracy?", "id": "Meningkatkan Partisipasi Dan Transparansi." },
  { "en": "Risiko E-Democracy?", "id": "Kesenjangan Digital Dan Keamanan Siber." },
  { "en": "Apa Itu Perizinan Terpadu Satu Pintu (PTSP)?", "id": "Layanan Perizinan Terintegrasi Satu Tempat." },
  { "en": "Tujuan Perizinan Terpadu Satu Pintu (PTSP)?", "id": "Mempermudah Dan Mempercepat Proses Perizinan." },
  { "en": "Dasar Hukum Perizinan Terpadu Satu Pintu (PTSP)?", "id": "Peraturan Presiden." },
  { "en": "Apa Itu Online Single Submission (OSS)?", "id": "Sistem Perizinan Berusaha Terintegrasi Elektronik." },
  { "en": "Lembaga Pengelola Online Single Submission (OSS)?", "id": "Badan Koordinasi Penanaman Modal (BKPM)." },
  { "en": "Manfaat Online Single Submission (OSS)?", "id": "Kepastian Hukum, Efisiensi Waktu Biaya." },
  { "en": "Apa Itu Laporan Penyelenggaraan Pemerintahan Daerah (LPPD)?", "id": "Laporan Kinerja Tahunan Kepala Daerah." },
  { "en": "Kepada Siapa Laporan Penyelenggaraan Pemerintahan Daerah (LPPD) Disampaikan?", "id": "Presiden Melalui Menteri Dalam Negeri." },
  { "en": "Apa Itu Evaluasi Kinerja Penyelenggaraan Pemerintahan Daerah (EKPPD)?", "id": "Penilaian Atas Laporan Penyelenggaraan Pemerintahan Daerah (LPPD)." },
  { "en": "Tujuan Evaluasi Kinerja Penyelenggaraan Pemerintahan Daerah (EKPPD)?", "id": "Melihat Capaian Kinerja Dan Pembinaan." },
  { "en": "Peringkat Daerah Berdasarkan Evaluasi Kinerja Penyelenggaraan Pemerintahan Daerah (EKPPD)?", "id": "Sangat Tinggi, Tinggi, Sedang, Rendah." },
  { "en": "Apa Itu Asas Proporsionalitas?", "id": "Keseimbangan Antara Kewenangan Dan Tanggung." },
  { "en": "Apa Itu Asas Profesionalitas?", "id": "Mengutamakan Keahlian Berlandaskan Kode Etik." },
  { "en": "Apa Itu Asas Keterbukaan?", "id": "Membuka Diri Terhadap Hak Masyarakat." },
  { "en": "Pentingnya Asas Keterbukaan?", "id": "Mencegah Korupsi Dan Meningkatkan Partisipasi." },
  { "en": "Apa Itu Hak Atas Informasi Publik?", "id": "Hak Setiap Orang Untuk Memperoleh." },
  { "en": "Lembaga Penjamin Hak Informasi?", "id": "Komisi Informasi." },
  { "en": "Informasi Yang Dikecualikan?", "id": "Informasi Dapat Membahayakan Negara." },
  { "en": "Apa Itu Pengadaan Barang Jasa Pemerintah?", "id": "Kegiatan Memperoleh Barang Jasa Pemerintah." },
  { "en": "Prinsip Pengadaan Barang Jasa?", "id": "Efisien, Efektif, Transparan, Terbuka, Bersaing." },
  { "en": "Metode Pengadaan Barang Jasa?", "id": "E-purchasing, Tender, Penunjukan Langsung." },
  { "en": "Lembaga Kebijakan Pengadaan Barang Jasa Pemerintah (LKPP)?", "id": "Lembaga Mengembangkan Kebijakan Pengadaan." },
  { "en": "Apa Itu Unit Layanan Pengadaan (ULP)?", "id": "Unit Melaksanakan Proses Pengadaan." },
  { "en": "Apa Itu Sanggahan Dalam Tender?", "id": "Protes Dari Peserta Tender." },
  { "en": "Apa Itu Daftar Hitam (Blacklist)?", "id": "Sanksi Bagi Penyedia Jasa Wanprestasi." },
  { "en": "Apa Itu Politik Hukum?", "id": "Arah Kebijakan Hukum Suatu Negara." },
  { "en": "Tujuan Politik Hukum Indonesia?", "id": "Mewujudkan Sistem Hukum Nasional Pancasila." },
  { "en": "Apa Itu Living Law?", "id": "Hukum Yang Hidup Dalam Masyarakat." },
  { "en": "Pentingnya Living Law?", "id": "Agar Hukum Sesuai Rasa Keadilan." },
  { "en": "Apa Itu Pluralisme Hukum?", "id": "Keberadaan Beberapa Sistem Hukum." },
  { "en": "Contoh Pluralisme Hukum Di Indonesia?", "id": "Hukum Adat, Hukum Agama, Hukum Negara." },
  { "en": "Apa Itu Penemuan Hukum (Rechtsvinding)?", "id": "Proses Hakim Menemukan Hukum." },
  { "en": "Metode Penemuan Hukum?", "id": "Interpretasi Dan Konstruksi Hukum." },
  { "en": "Apa Itu Interpretasi Gramatikal?", "id": "Penafsiran Berdasarkan Arti Kata." },
  { "en": "Apa Itu Interpretasi Sistematis?", "id": "Penafsiran Dihubungkan Dengan Peraturan Lain." },
  { "en": "Apa Itu Interpretasi Historis?", "id": "Penafsiran Berdasarkan Sejarah Pembentukan." },
  { "en": "Apa Itu Interpretasi Teleologis?", "id": "Penafsiran Berdasarkan Tujuan." },
  { "en": "Apa Itu Analogi?", "id": "Menerapkan Aturan Pada Peristiwa." },
  { "en": "Apa Itu Argumentum A Contrario?", "id": "Penalaran Secara Berlawanan." },
  { "en": "Apa Itu Fiksi Hukum?", "id": "Sesuatu Dianggap Ada Demi Kepastian." },
  { "en": "Contoh Fiksi Hukum?", "id": "Semua Orang Dianggap Tahu Hukum." },
  { "en": "Apa Itu Adagium Hukum?", "id": "Peribahasa Atau Pepatah Hukum." },
  { "en": "Contoh Adagium Hukum?", "id": "Lex Superiori Derogat Legi Inferiori." },
  { "en": "Apa Itu Lex Specialis Derogat Legi Generali?", "id": "Hukum Khusus Mengesampingkan Hukum Umum." },
  { "en": "Apa Itu Lex Posterior Derogat Legi Priori?", "id": "Hukum Baru Mengesampingkan Hukum Lama." },
  { "en": "Apa Itu Geopolitik?", "id": "Studi Pengaruh Geografi Terhadap Politik." },
  { "en": "Teori Geopolitik Ratzel?", "id": "Negara Adalah Organisme Yang Tumbuh." },
  { "en": "Teori Geopolitik Mackinder?", "id": "Siapa Menguasai Jantung Dunia." },
  { "en": "Apa Itu Heartland (Jantung Dunia)?", "id": "Kawasan Erasia Tengah." },
  { "en": "Teori Geopolitik Spykman?", "id": "Siapa Menguasai Rimland (Daerah Pinggir)." },
  { "en": "Apa Itu Rimland (Daerah Pinggir)?", "id": "Pesisir Benua Eurasia." },
  { "en": "Geopolitik Indonesia Berdasarkan?", "id": "Wawasan Nusantara." },
  { "en": "Apa Itu Geostrategi?", "id": "Metode Mewujudkan Cita-cita Geopolitik." },
  { "en": "Geostrategi Indonesia Adalah?", "id": "Ketahanan Nasional." },
  { "en": "Apa Itu Astagatra?", "id": "Delapan Aspek Kehidupan Nasional." },
  { "en": "Apa Itu Trigatra?", "id": "Tiga Aspek Alamiah (Geografi, Demografi)." },
  { "en": "Apa Itu Pancagatra?", "id": "Lima Aspek Sosial (Ideologi, Politik)." },
  { "en": "Ancaman, Tantangan, Hambatan, Gangguan (ATHG)?", "id": "Hal-hal Yang Menguji Ketahanan Nasional." },
  { "en": "Apa Itu Sekuritisasi?", "id": "Proses Menjadikan Isu Biasa Ancaman." },
  { "en": "Dampak Sekuritisasi?", "id": "Membenarkan Penggunaan Tindakan Luar Biasa." },
  { "en": "Apa Itu Human Security (Keamanan Manusia)?", "id": "Keamanan Berfokus Pada Individu." },
  { "en": "Aspek Human Security?", "id": "Ekonomi, Pangan, Kesehatan, Lingkungan." },
  { "en": "Apa Itu Responsibility to Protect (R2P)?", "id": "Tanggung Jawab Melindungi Warga Sipil." },
  { "en": "Kapan Responsibility to Protect (R2P) Diterapkan?", "id": "Saat Negara Gagal Melindungi Rakyatnya." },
  { "en": "Apa Itu Intervensi Kemanusiaan?", "id": "Campur Tangan Militer Hentikan Pelanggaran." },
  { "en": "Perdebatan Intervensi Kemanusiaan?", "id": "Antara Kedaulatan Negara Dan Hak Asasi Manusia (HAM)." },
  { "en": "Apa Itu Diplomasi Publik?", "id": "Upaya Mempengaruhi Publik Negara Lain." },
  { "en": "Tujuan Diplomasi Publik?", "id": "Membangun Citra Positif Dan Dukungan." },
  { "en": "Contoh Diplomasi Publik Indonesia?", "id": "Promosi Budaya, Beasiswa, Dialog Lintas." },
  { "en": "Apa Itu Nation Branding?", "id": "Upaya Memasarkan Citra Suatu Negara." },
  { "en": "Pentingnya Nation Branding?", "id": "Menarik Investasi, Turis, Dan Pengaruh." },
  { "en": "Apa Itu Indeks Demokrasi?", "id": "Ukuran Kualitas Demokrasi Suatu Negara." },
  { "en": "Lembaga Yang Merilis Indeks Demokrasi?", "id": "The Economist Intelligence Unit (EIU)." },
  { "en": "Kategori Indeks Demokrasi?", "id": "Demokrasi Penuh, Cacat, Hibrida, Otoriter." },
  { "en": "Apa Itu Indeks Kebebasan Pers?", "id": "Ukuran Kebebasan Jurnalis Di Dunia." },
  { "en": "Lembaga Perilis Indeks Kebebasan Pers?", "id": "Reporters Without Borders (RSF)." },
  { "en": "Faktor Mempengaruhi Kebebasan Pers?", "id": "Pluralisme Media, Independensi, Sensor." },
  { "en": "Apa Itu Literasi Digital?", "id": "Kemampuan Menggunakan Teknologi Digital." },
  { "en": "Pentingnya Literasi Digital Politik?", "id": "Agar Kritis Terhadap Informasi Online." },
  { "en": "Apa Itu Echo Chamber (Ruang Gema)?", "id": "Lingkungan Informasi Tertutup." },
  { "en": "Bahaya Echo Chamber?", "id": "Memperkuat Keyakinan Sendiri, Sulit Menerima." },
  { "en": "Apa Itu Filter Bubble (Gelembung Filter)?", "id": "Isolasi Intelektual Akibat Algoritma." },
  { "en": "Dampak Filter Bubble?", "id": "Menyempitkan Pandangan Dan Wawasan." },
  { "en": "Cara Mengatasi Echo Chamber?", "id": "Mencari Sumber Informasi Yang Beragam." },
  { "en": "Apa Itu Fact-Checking (Pemeriksaan Fakta)?", "id": "Proses Verifikasi Kebenaran Informasi." },
  { "en": "Peran Fact-Checking?", "id": "Melawan Misinformasi Dan Disinformasi." },
  { "en": "Perbedaan Misinformasi Dan Disinformasi?", "id": "Disinformasi Sengaja Dibuat Untuk Menyesatkan." },
  { "en": "Apa Itu Aparatur Sipil Negara (ASN)?", "id": "Profesi Bagi Pegawai Negeri Sipil (PNS) Dan Pegawai Pemerintah dengan Perjanjian Kerja (PPPK)." },
  { "en": "Apa Itu Pegawai Negeri Sipil (PNS)?", "id": "Warga Negara Indonesia (WNI) Diangkat Sebagai Pegawai Aparatur Sipil Negara (ASN)." },
  { "en": "Apa Itu Pegawai Pemerintah dengan Perjanjian Kerja (PPPK)?", "id": "Diangkat Berdasarkan Perjanjian Kerja." },
  { "en": "Asas Manajemen Aparatur Sipil Negara (ASN)?", "id": "Merit, Netralitas, Profesionalitas, Keadilan." },
  { "en": "Apa Itu Netralitas Aparatur Sipil Negara (ASN)?", "id": "Tidak Berpihak Dari Pengaruh Politik." },
  { "en": "Lembaga Pengawas Netralitas Aparatur Sipil Negara (ASN)?", "id": "Komisi Aparatur Sipil Negara (KASN)." },
  { "en": "Apa Itu Jabatan Pimpinan Tinggi (JPT)?", "id": "Kelompok Jabatan Tinggi Dalam Pemerintahan." },
  { "en": "Pengisian Jabatan Pimpinan Tinggi (JPT)?", "id": "Melalui Seleksi Terbuka Dan Kompetitif." },
  { "en": "Apa Itu Sistem Informasi Aparatur Sipil Negara (ASN)?", "id": "Data Aparatur Sipil Negara (ASN) Terintegrasi Nasional." },
  { "en": "Manfaat Sistem Informasi Aparatur Sipil Negara (ASN)?", "id": "Mendukung Manajemen Aparatur Sipil Negara (ASN) Berbasis Merit." },
  { "en": "Apa Itu Tata Naskah Dinas?", "id": "Pengelolaan Informasi Tertulis Kedinasan." },
  { "en": "Pentingnya Tata Naskah Dinas?", "id": "Menciptakan Kelancaran Komunikasi Tertulis." },
  { "en": "Apa Itu Arsip?", "id": "Rekaman Kegiatan Dalam Berbagai Bentuk." },
  { "en": "Lembaga Pengelola Arsip Nasional?", "id": "Arsip Nasional Republik Indonesia (ANRI)." },
  { "en": "Apa Itu Jadwal Retensi Arsip (JRA)?", "id": "Jangka Waktu Penyimpanan Arsip." },
  { "en": "Apa Itu Korupsi?", "id": "Tindakan Melawan Hukum, Memperkaya Diri." },
  { "en": "Jenis Korupsi?", "id": "Suap, Gratifikasi, Penggelapan, Pemerasan." },
  { "en": "Dasar Hukum Pemberantasan Korupsi?", "id": "Undang-Undang Nomor 31 Tahun 1999." },
  { "en": "Strategi Pemberantasan Korupsi?", "id": "Pencegahan, Penindakan, Dan Pendidikan." },
  { "en": "Apa Itu Tiga Dosa Besar Pendidikan?", "id": "Intoleransi, Kekerasan Seksual, Perundungan." },
  { "en": "Apa Itu Revolusi Mental?", "id": "Gerakan Mengubah Cara Pikir, Sikap." },
  { "en": "Nilai Revolusi Mental?", "id": "Integritas, Etos Kerja, Gotong Royong." },
  { "en": "Apa Itu Ideologi?", "id": "Kumpulan Ide Atau Gagasan." },
  { "en": "Fungsi Ideologi?", "id": "Memberi Arah Dan Tujuan Bangsa." },
  { "en": "Pancasila Sebagai Ideologi Terbuka?", "id": "Mampu Menyesuaikan Diri Dengan Zaman." },
  { "en": "Nilai Dasar Pancasila?", "id": "Nilai Ketuhanan, Kemanusiaan, Persatuan." },
  { "en": "Nilai Instrumental Pancasila?", "id": "Penjabaran Nilai Dasar Dalam Peraturan." },
  { "en": "Nilai Praksis Pancasila?", "id": "Realisasi Nilai Dalam Kehidupan Sehari-hari." },
  { "en": "Hubungan Antar Sila Pancasila?", "id": "Saling Mendasari Dan Menjiwai." },
  { "en": "Pancasila Sebagai Sumber Segala Sumber Hukum?", "id": "Semua Hukum Harus Bersumber Pancasila." },
  { "en": "Apa Itu Badan Pembinaan Ideologi Pancasila (BPIP)?", "id": "Lembaga Bantu Presiden Rumuskan Arah." },
  { "en": "Tugas Badan Pembinaan Ideologi Pancasila (BPIP)?", "id": "Merumuskan Arah Kebijakan Pembinaan." },
  { "en": "Apa Itu Gotong Royong?", "id": "Bekerja Bersama Untuk Kepentingan Bersama." },
  { "en": "Inti Pancasila Menurut Soekarno?", "id": "Gotong Royong." },
  { "en": "Apa Itu Ekasila?", "id": "Prinsip Gotong Royong." },
  { "en": "Apa Itu Trisila?", "id": "Sosio-nasionalisme, Sosio-demokrasi, Ketuhanan." },
  { "en": "Kapan Hari Lahir Pancasila?", "id": "Tanggal 1 Juni 1945." },
  { "en": "Pidato Lahirnya Pancasila?", "id": "Disampaikan Oleh Soekarno." },
  { "en": "Siapa Perumus Teks Proklamasi?", "id": "Soekarno, Hatta, Dan Ahmad Soebardjo." },
  { "en": "Siapa Pengetik Teks Proklamasi?", "id": "Sayuti Melik." },
  { "en": "Di Mana Proklamasi Dibacakan?", "id": "Jalan Pegangsaan Timur 56 Jakarta." },
  { "en": "Siapa Yang Menjahit Bendera Pusaka?", "id": "Ibu Fatmawati." },
  { "en": "Makna Warna Merah Putih?", "id": "Merah Berarti Berani, Putih Suci." },
  { "en": "Apa Itu Panitia Persiapan Kemerdekaan Indonesia (PPKI)?", "id": "Badan Lanjutan Badan Penyelidik Usaha-Usaha Persiapan Kemerdekaan Indonesia (BPUPKI)." },
  { "en": "Ketua Panitia Persiapan Kemerdekaan Indonesia (PPKI)?", "id": "Ir. Soekarno." },
  { "en": "Hasil Sidang Panitia Persiapan Kemerdekaan Indonesia (PPKI) 18 Agustus 1945?", "id": "Mengesahkan Undang-Undang Dasar (UUD), Memilih Presiden." },
  { "en": "Apa Itu Piagam Jakarta?", "id": "Rancangan Pembukaan Undang-Undang Dasar (UUD) 1945." },
  { "en": "Perbedaan Piagam Jakarta Dan Pembukaan Undang-Undang Dasar (UUD)?", "id": "Sila Pertama, Tujuh Kata Dihapus." },
  { "en": "Apa Itu Demokrasi Liberal?", "id": "Sistem Pemerintahan Indonesia 1950-1959." },
  { "en": "Ciri Demokrasi Liberal?", "id": "Sering Terjadi Pergantian Kabinet." },
  { "en": "Mengapa Disebut Demokrasi Terpimpin?", "id": "Kekuasaan Presiden Sangat Besar." },
  { "en": "Penyimpangan Demokrasi Terpimpin?", "id": "Pembubaran Dewan Perwakilan Rakyat (DPR), Pengangkatan Presiden Seumur." },
  { "en": "Apa Itu Tiga Tuntutan Rakyat (Tritura)?", "id": "Tuntutan Aksi Mahasiswa Tahun 1966." },
  { "en": "Isi Tiga Tuntutan Rakyat (Tritura)?", "id": "Bubarkan Partai Komunis Indonesia (PKI), Rombak Kabinet, Turunkan Harga." },
  { "en": "Apa Itu Surat Perintah Sebelas Maret (Supersemar)?", "id": "Surat Perintah Dari Soekarno Ke Soeharto." },
  { "en": "Dampak Surat Perintah Sebelas Maret (Supersemar)?", "id": "Awal Peralihan Kekuasaan Orde Baru." },
  { "en": "Ciri Pemerintahan Orde Baru?", "id": "Sentralistis, Pembangunan Ekonomi, Stabilitas." },
  { "en": "Penyebab Jatuhnya Orde Baru?", "id": "Krisis Moneter, Korupsi, Kolusi, Dan Nepotisme (KKN), Tuntutan Demokrasi." },
  { "en": "Agenda Reformasi 1998?", "id": "Supremasi Hukum, Pemberantasan Korupsi, Kolusi, Dan Nepotisme (KKN), Amandemen." },
  { "en": "Apa Itu Amandemen Undang-Undang Dasar (UUD) 1945?", "id": "Perubahan Terhadap Undang-Undang Dasar (UUD) 1945." },
  { "en": "Berapa Kali Amandemen Dilakukan?", "id": "Empat Kali (1999, 2000, 2001, 2002)." },
  { "en": "Lembaga Yang Melakukan Amandemen?", "id": "Majelis Permusyawaratan Rakyat (MPR)." },
  { "en": "Perubahan Mendasar Hasil Amandemen?", "id": "Pemilihan Presiden Langsung, Pembentukan Dewan Perwakilan Daerah (DPD)." },
  { "en": "Apa Itu Masyarakat Multikultural?", "id": "Masyarakat Terdiri Atas Beragam Budaya." },
  { "en": "Potensi Positif Masyarakat Multikultural?", "id": "Kekayaan Budaya Dan Kreativitas." },
  { "en": "Potensi Negatif Masyarakat Multikultural?", "id": "Rawan Terjadi Konflik Sosial." },
  { "en": "Solusi Hadapi Potensi Negatif?", "id": "Toleransi, Dialog, Dan Penegakan Hukum." },
  { "en": "Apa Itu Kearifan Lokal (Local Wisdom)?", "id": "Gagasan Lokal Yang Bijaksana." },
  { "en": "Contoh Kearifan Lokal?", "id": "Subak Di Bali, Sasi Di Maluku." },
  { "en": "Pentingnya Kearifan Lokal?", "id": "Menjaga Keseimbangan Alam Dan Sosial." },
  { "en": "Apa Itu Globalisasi?", "id": "Proses Mendunianya Suatu Hal." },
  { "en": "Dampak Positif Globalisasi?", "id": "Kemudahan Akses Informasi Dan Teknologi." },
  { "en": "Dampak Negatif Globalisasi?", "id": "Lunturnya Nilai Budaya, Kesenjangan Ekonomi." },
  { "en": "Sikap Terhadap Globalisasi?", "id": "Selektif, Menerima Yang Baik." },
  { "en": "Apa Itu Westernisasi?", "id": "Meniru Gaya Hidup Kebarat-baratan." },
  { "en": "Apa Itu Modernisasi?", "id": "Proses Menuju Masyarakat Modern." },
  { "en": "Perbedaan Modernisasi Dan Westernisasi?", "id": "Modernisasi Mutlak, Westernisasi Tidak." },
  { "en": "Apa Itu Konsumerisme?", "id": "Paham Gaya Hidup Boros." },
  { "en": "Apa Itu Hedonisme?", "id": "Pandangan Hidup Mengejar Kesenangan." },
  { "en": "Apa Itu Individualisme?", "id": "Paham Mengutamakan Kepentingan Individu." },
  { "en": "Sikap Yang Sesuai Pancasila?", "id": "Keseimbangan Antara Hak Dan Kewajiban." },
  { "en": "Hak Warga Negara?", "id": "Mendapat Pendidikan, Pekerjaan, Perlindungan." },
  { "en": "Kewajiban Warga Negara?", "id": "Membayar Pajak, Bela Negara, Hormati." },
  { "en": "Kasus Pelanggaran Hak?", "id": "Tidak Mendapat Pendidikan Layak." },
  { "en": "Kasus Pengingkaran Kewajiban?", "id": "Tidak Membayar Pajak Tepat Waktu." },
  { "en": "Penyebab Pelanggaran Hak?", "id": "Egoisme, Rendahnya Kesadaran Hukum." },
  { "en": "Upaya Pencegahan Pelanggaran?", "id": "Supremasi Hukum, Pendidikan Karakter." },
  { "en": "Apa Itu Perlindungan Hukum?", "id": "Upaya Melindungi Hak-hak Subjek." },
  { "en": "Apa Itu Penegakan Hukum?", "id": "Upaya Menjalankan Norma Hukum." },
  { "en": "Unsur Penegakan Hukum?", "id": "Aparat, Peraturan, Kesadaran, Sarana." },
  { "en": "Lembaga Penegak Hukum?", "id": "Polisi, Jaksa, Hakim, Advokat, Komisi Pemberantasan Korupsi (KPK)." },
  { "en": "Apa Itu Desentralisasi?", "id": "Penyerahan Wewenang Dari Pusat Ke Daerah." },
  { "en": "Tujuan Desentralisasi?", "id": "Mendekatkan Pelayanan Kepada Masyarakat." },
  { "en": "Apa Itu Dekonsentrasi?", "id": "Pelimpahan Wewenang Ke Instansi Vertikal." },
  { "en": "Contoh Instansi Vertikal?", "id": "Kantor Wilayah Kementerian Hukum dan Hak Asasi Manusia (Kanwil Kemenkumham)." },
  { "en": "Apa Itu Tugas Pembantuan (Medebewind)?", "id": "Penugasan Pusat Ke Daerah." },
  { "en": "Contoh Tugas Pembantuan?", "id": "Pelaksanaan Program Keluarga Harapan (PKH) Oleh Dinas Sosial." },
  { "en": "Apa Itu Urusan Pemerintahan Absolut?", "id": "Urusan Sepenuhnya Wewenang Pusat." },
  { "en": "Contoh Urusan Absolut?", "id": "Pertahanan, Keamanan, Moneter, Yustisi." },
  { "en": "Apa Itu Urusan Pemerintahan Konkuren?", "id": "Urusan Dibagi Pusat Dan Daerah." },
  { "en": "Pembagian Urusan Konkuren?", "id": "Pusat Menetapkan Norma, Daerah Melaksanakan." },
  { "en": "Apa Itu Urusan Pemerintahan Umum?", "id": "Urusan Kewenangan Presiden Sebagai Kepala." },
  { "en": "Pelaksana Urusan Pemerintahan Umum?", "id":"Gubernur, Bupati, Wali Kota." },
  { "en": "Apa Itu Otonomi Luas?", "id": "Keleluasaan Daerah Menyelenggarakan Kewenangan." },
  { "en": "Apa Itu Otonomi Nyata?", "id": "Kewenangan Menangani Urusan Berdasarkan Tugas." },
  { "en": "Apa Itu Otonomi Bertanggung Jawab?", "id": "Otonomi Selaras Dengan Tujuan." },
  { "en": "Apa Itu Daerah Khusus?", "id": "Daerah Diberi Otonomi Khusus." },
  { "en": "Contoh Daerah Khusus?", "id": "Daerah Khusus Ibukota (DKI) Jakarta, Aceh, Papua." },
  { "en": "Apa Itu Daerah Istimewa?", "id": "Daerah Dengan Pengaturan Istimewa." },
  { "en": "Contoh Daerah Istimewa?", "id": "Daerah Istimewa Yogyakarta (DIY)." },
  { "en": "Dasar Hukum Otonomi Daerah?", "id": "Undang-Undang Nomor 23 Tahun 2014." },
  { "en": "Apa Itu Perda (Peraturan Daerah)?", "id": "Peraturan Perundangan Dibentuk Dewan Perwakilan Rakyat Daerah (DPRD)." },
  { "en": "Siapa Yang Menetapkan Perda?", "id": "Dewan Perwakilan Rakyat Daerah (DPRD) Dengan Persetujuan Kepala Daerah." },
  { "en": "Apa Itu Perkada (Peraturan Kepala Daerah)?", "id": "Peraturan Ditetapkan Kepala Daerah." },
  { "en": "Contoh Perkada?", "id": "Peraturan Gubernur (Pergub) Dan Peraturan Bupati (Perbup)." },
  { "en": "Apa Itu Anggaran Pendapatan Dan Belanja Daerah (APBD)?", "id": "Rencana Keuangan Tahunan Daerah." },
  { "en": "Fungsi Anggaran Pendapatan Dan Belanja Daerah (APBD)?", "id": "Otorisasi, Perencanaan, Pengawasan, Alokasi." },
  { "en": "Sumber Pendapatan Asli Daerah (PAD)?", "id": "Pajak Daerah, Retribusi, Hasil BUMD." },
  { "en": "Apa Itu Dana Perimbangan?", "id": "Dana Dari Anggaran Pendapatan Dan Belanja Negara (APBN) Untuk Daerah." },
  { "en": "Jenis Dana Perimbangan?", "id": "Dana Bagi Hasil (DBH), Dana Alokasi Umum (DAU), Dana Alokasi Khusus (DAK)." },
  { "en": "Apa Itu Dana Bagi Hasil (DBH)?", "id": "Dana Bersumber Dari Pajak Tertentu." },
  { "en": "Apa Itu Dana Alokasi Umum (DAU)?", "id": "Dana Untuk Pemerataan Kemampuan Keuangan." },
  { "en": "Apa Itu Dana Alokasi Khusus (DAK)?", "id": "Dana Untuk Mendanai Kegiatan Khusus." },
  { "en": "Apa Itu Lain-lain Pendapatan Daerah Sah?", "id": "Hibah, Dana Darurat, Dana Desa." },
  { "en": "Struktur Organisasi Pemerintah Daerah?", "id": "Kepala Daerah Dan Perangkat Daerah." },
  { "en": "Unsur Perangkat Daerah?", "id": "Sekretariat, Dinas, Badan, Kecamatan." },
  { "en": "Siapa Yang Memimpin Sekretariat Daerah?", "id": "Sekretaris Daerah (Sekda)." },
  { "en": "Apa Itu Negara Kesatuan?", "id": "Negara Merdeka Berdaulat." },
  { "en": "Ciri Negara Kesatuan?", "id": "Satu Pemerintah Pusat, Satu Konstitusi." },
  { "en": "Apa Itu Negara Serikat (Federal)?", "id": "Gabungan Beberapa Negara Bagian." },
  { "en": "Ciri Negara Serikat?", "id": "Pusat Punya Kedaulatan Keluar." },
  { "en": "Bentuk Pemerintahan Indonesia?", "id": "Republik Konstitusional." },
  { "en": "Sistem Pemerintahan Indonesia?", "id": "Sistem Presidensial." },
  { "en": "Ciri Sistem Presidensial?", "id": "Presiden Kepala Negara Dan Pemerintahan." },
  { "en": "Ciri Sistem Parlementer?", "id": "Kepala Pemerintahan Adalah Perdana Menteri." },
  { "en": "Pernahkah Indonesia Menerapkan Parlementer?", "id": "Pernah, Pada Masa Demokrasi Liberal." },
  { "en": "Lembaga Tinggi Negara?", "id": "Presiden, Majelis Permusyawaratan Rakyat (MPR), Dewan Perwakilan Rakyat (DPR), Dewan Perwakilan Daerah (DPD), Mahkamah Agung (MA), Mahkamah Konstitusi (MK), Badan Pemeriksa Keuangan (BPK)." },
  { "en": "Tugas Majelis Permusyawaratan Rakyat (MPR)?", "id": "Mengubah Undang-Undang Dasar (UUD), Melantik Presiden." },
  { "en": "Fungsi Dewan Perwakilan Rakyat (DPR)?", "id": "Legislasi, Anggaran, Pengawasan." },
  { "en": "Hak Dewan Perwakilan Rakyat (DPR)?", "id": "Interpelasi, Angket, Menyatakan Pendapat." },
  { "en": "Tugas Dewan Perwakilan Daerah (DPD)?", "id": "Mengajukan Rancangan Undang-Undang (RUU) Otonomi Daerah." },
  { "en": "Tugas Presiden?", "id": "Memegang Kekuasaan Pemerintahan." },
  { "en": "Hak Prerogatif Presiden?", "id": "Memberi Grasi, Rehabilitasi, Amnesti, Abolisi." },
  { "en": "Tugas Mahkamah Agung (MA)?", "id": "Mengadili Tingkat Kasasi, Menguji Peraturan." },
  { "en": "Tugas Mahkamah Konstitusi (MK)?", "id": "Menguji Undang-Undang, Memutus Sengketa." },
  { "en": "Wewenang Komisi Yudisial (KY)?", "id": "Mengusulkan Pengangkatan Hakim Agung." },
  { "en": "Tugas Badan Pemeriksa Keuangan (BPK)?", "id": "Memeriksa Pengelolaan Keuangan Negara." },
  { "en": "Apa Itu Budaya Politik?", "id": "Orientasi Politik Khas Warga Negara." },
  { "en": "Tipe Budaya Politik?", "id": "Parokial, Kaula (Subjek), Partisipan." },
  { "en": "Budaya Politik Parokial?", "id": "Tingkat Partisipasi Sangat Rendah." },
  { "en": "Budaya Politik Kaula?", "id": "Ada Kesadaran, Tapi Pasif." },
  { "en": "Budaya Politik Partisipan?", "id": "Kesadaran Dan Partisipasi Politik Tinggi." },
  { "en": "Apa Itu Sosialisasi Politik?", "id": "Proses Pembentukan Sikap Politik." },
  { "en": "Agen Sosialisasi Politik?", "id": "Keluarga, Sekolah, Partai Politik, Media Massa." },
  { "en": "Apa Itu Partisipasi Politik?", "id": "Keterlibatan Warga Dalam Sistem Politik." },
  { "en": "Bentuk Partisipasi Politik?", "id": "Konvensional (Memilih) Dan Non-Konvensional." },
  { "en": "Apa Itu Partai Politik?", "id": "Organisasi Mencari Dan Mempertahankan Kekuasaan." },
  { "en": "Fungsi Partai Politik?", "id": "Pendidikan, Rekrutmen, Agregasi Kepentingan." },
  { "en": "Sistem Kepartaian Indonesia?", "id": "Sistem Multipartai." },
  { "en": "Apa Itu Kelompok Kepentingan?", "id": "Kelompok Memperjuangkan Kepentingan Tertentu." },
  { "en": "Apa Itu Kelompok Penekan?", "id": "Kelompok Mempengaruhi Keputusan Pemerintah." },
  { "en": "Apa Itu Pemilu?", "id": "Sarana Kedaulatan Rakyat." },
  { "en": "Asas Pemilu?", "id": "Langsung, Umum, Bebas, Rahasia, Jujur, Adil." },
  { "en": "Siapa Penyelenggara Pemilu?", "id": "Komisi Pemilihan Umum (KPU), Badan Pengawas Pemilihan Umum (Bawaslu), Dewan Kehormatan Penyelenggara Pemilu (DKPP)." },
  { "en": "Tahapan Pemilu?", "id": "Pendaftaran, Kampanye, Pemungutan, Penghitungan." },
  { "en": "Apa Itu Sistem Pemilu Legislatif?", "id": "Proporsional Terbuka." },
  { "en": "Apa Itu Ambang Batas Parlemen?", "id": "Syarat Suara Minimum Partai." },
  { "en": "Apa Itu Ambang Batas Presiden?", "id": "Syarat Dukungan Pencalonan Presiden." },
  { "en": "Apa Itu Demokrasi?", "id": "Pemerintahan Dari, Oleh, Untuk Rakyat." },
  { "en": "Prinsip Demokrasi?", "id": "Kedaulatan Rakyat, Supremasi Hukum." },
  { "en": "Demokrasi Pancasila?", "id": "Demokrasi Berdasarkan Sila-Sila Pancasila." },
  { "en": "Ciri Demokrasi Pancasila?", "id": "Mengutamakan Musyawarah Mufakat." },
  { "en": "Apa Itu Hak Asasi Manusia (HAM)?", "id": "Hak Dasar Melekat Pada Manusia." },
  { "en": "Ciri Hak Asasi Manusia (HAM)?", "id": "Hakiki, Universal, Tidak Dapat Dicabut." },
  { "en": "Dasar Hukum Hak Asasi Manusia (HAM) Di Indonesia?", "id": "Undang-Undang Dasar (UUD) 1945, Undang-Undang Nomor 39 Tahun 1999." },
  { "en": "Lembaga Perlindungan Hak Asasi Manusia (HAM)?", "id": "Komisi Nasional Hak Asasi Manusia (Komnas HAM), Pengadilan Hak Asasi Manusia (HAM)." },
  { "en": "Pelanggaran Hak Asasi Manusia (HAM) Berat?", "id": "Genosida Dan Kejahatan Kemanusiaan." },
  { "en": "Apa Itu Wawasan Nusantara?", "id": "Cara Pandang Bangsa Indonesia." },
  { "en": "Konsep Wawasan Nusantara?", "id": "Kesatuan Politik, Ekonomi, Sosial, Budaya." },
  { "en": "Tujuan Wawasan Nusantara?", "id": "Mewujudkan Tujuan Nasional Bangsa." },
  { "en": "Apa Itu Ketahanan Nasional?", "id": "Kondisi Dinamis Hadapi Ancaman." },
  { "en": "Konsep Ketahanan Nasional?", "id": "Model Astagatra." },
  { "en": "Apa Itu Bela Negara?", "id": "Sikap Dan Perilaku Cinta Negara." },
  { "en": "Dasar Hukum Bela Negara?", "id": "Pasal 27 Ayat 3 Undang-Undang Dasar (UUD) 1945." },
  { "en": "Ancaman Terhadap Integrasi Nasional?", "id": "Separatisme, Terorisme, Radikalisme, Konflik." },
  { "en": "Pentingnya Integrasi Nasional?", "id": "Menjaga Keutuhan Negara Kesatuan Republik Indonesia (NKRI)." },
  { "en": "Apa Itu Norma?", "id": "Aturan Atau Kaidah Perilaku." },
  { "en": "Macam-Macam Norma?", "id": "Agama, Kesusilaan, Kesopanan, Hukum." },
  { "en": "Sanksi Norma Agama?", "id": "Dosa, Siksa Di Akhirat." },
  { "en": "Sanksi Norma Kesusilaan?", "id": "Penyesalan, Rasa Malu, Dikucilkan." },
  { "en": "Sanksi Norma Kesopanan?", "id": "Cemoohan, Celaan, Diasingkan." },
  { "en": "Sanksi Norma Hukum?", "id": "Tegas, Nyata, Mengikat (Penjara)." },
  { "en": "Apa Itu Keadilan?", "id": "Memberi Sesuatu Sesuai Haknya." },
  { "en": "Jenis Keadilan Menurut Aristoteles?", "id": "Distributif, Komutatif, Kodrat Alam." },
  { "en": "Keadilan Distributif?", "id": "Keadilan Berdasarkan Jasa Atau Prestasi." },
  { "en": "Keadilan Komutatif?", "id": "Keadilan Tanpa Memandang Jasa." },
  { "en": "Apa Itu Konstitusi?", "id": "Hukum Dasar Tertulis Suatu Negara." },
  { "en": "Fungsi Konstitusi?", "id": "Membatasi Kekuasaan, Menjamin Hak." },
  { "en": "Sifat Konstitusi?", "id": "Fleksibel Dan Rigid (Kaku)." },
  { "en": "Konstitusi Yang Pernah Berlaku?", "id": "Undang-Undang Dasar (UUD) 1945, Konstitusi Republik Indonesia Serikat (RIS), Undang-Undang Dasar Sementara (UUDS) 1950." },
  { "en": "Sistematika Undang-Undang Dasar (UUD) 1945 Sebelum Amandemen?", "id": "Pembukaan, Batang Tubuh, Penjelasan." },
  { "en": "Sistematika Undang-Undang Dasar (UUD) 1945 Setelah Amandemen?", "id": "Pembukaan Dan Pasal-pasal." },
  { "en": "Makna Alinea Pertama Pembukaan Undang-Undang Dasar (UUD)?", "id": "Dalil Objektif Dan Subjektif." },
  { "en": "Makna Alinea Kedua Pembukaan Undang-Undang Dasar (UUD)?", "id": "Cita-cita Kemerdekaan Indonesia." },
  { "en": "Makna Alinea Ketiga Pembukaan Undang-Undang Dasar (UUD)?", "id": "Motivasi Spiritual Kemerdekaan." },
  { "en": "Makna Alinea Keempat Pembukaan Undang-Undang Dasar (UUD)?", "id": "Tujuan, Bentuk, Dan Dasar Negara." },
  { "en": "Pokok Pikiran Pembukaan Undang-Undang Dasar (UUD) 1945?", "id": "Persatuan, Keadilan, Kedaulatan, Ketuhanan." },
  { "en": "Tata Urutan Peraturan Perundang-undangan?", "id": "Hierarki Peraturan Hukum Di Indonesia." },
  { "en": "Urutan Tertinggi Peraturan?", "id": "Undang-Undang Dasar Negara Republik Indonesia (UUD NRI) 1945." },
  { "en": "Urutan Terendah Peraturan?", "id": "Peraturan Daerah Kabupaten Atau Kota." },
  { "en": "Asas Pembentukan Peraturan?", "id": "Kejelasan Tujuan, Kelembagaan, Kesesuaian." },
  { "en": "Partisipasi Masyarakat Dalam Pembentukan?", "id": "Memberikan Masukan Lisan Atau Tertulis." },
  { "en": "Apa Itu Bhinneka Tunggal Ika?", "id": "Berbeda-beda Tetapi Tetap Satu." },
  { "en": "Makna Bhinneka Tunggal Ika?", "id": "Persatuan Dalam Keragaman Bangsa." },
  { "en": "Faktor Pendorong Persatuan?", "id": "Pancasila, Sumpah Pemuda, Rasa Senasib." },
  { "en": "Faktor Penghambat Persatuan?", "id": "Etnosentrisme, Primordialisme, Chauvinisme." },
  { "en": "Apa Itu Etnosentrisme?", "id": "Menganggap Budaya Sendiri Paling Baik." },
  { "en": "Apa Itu Primordialisme?", "id": "Memegang Teguh Hal Sejak Kecil." },
  { "en": "Apa Itu Chauvinisme?", "id": "Mengagungkan Bangsa Sendiri Secara Berlebihan." },
  { "en": "Keragaman Suku Di Indonesia?", "id": "Suku Jawa, Sunda, Batak, Dayak." },
  { "en": "Keragaman Agama Di Indonesia?", "id": "Islam, Kristen, Katolik, Hindu, Buddha." },
  { "en": "Keragaman Ras Di Indonesia?", "id": "Malayan-Mongoloid, Melanesoid, Asiatik." },
  { "en": "Semboyan Persatuan Dalam Perbedaan?", "id": "Bhinneka Tunggal Ika Tan Hana." },
  { "en": "Sistem Pertahanan Indonesia?", "id": "Sistem Pertahanan Dan Keamanan Rakyat Semesta (Sishankamrata)." },
  { "en": "Komponen Utama Sistem Pertahanan Dan Keamanan Rakyat Semesta (Sishankamrata)?", "id": "Tentara Nasional Indonesia (TNI)." },
  { "en": "Komponen Cadangan Sistem Pertahanan Dan Keamanan Rakyat Semesta (Sishankamrata)?", "id": "Warga Negara, Sumber Daya Alam." },
  { "en": "Komponen Pendukung Sistem Pertahanan Dan Keamanan Rakyat Semesta (Sishankamrata)?", "id": "Sarana Dan Prasarana Nasional." },
  { "en": "Tugas Tentara Nasional Indonesia (TNI)?", "id": "Menegakkan Kedaulatan, Mempertahankan Keutuhan." },
  { "en": "Tugas Kepolisian Republik Indonesia (Polri)?", "id": "Memelihara Keamanan Dan Ketertiban Masyarakat." },
  { "en": "Ancaman Militer?", "id": "Agresi, Spionase, Sabotase, Pemberontakan." },
  { "en": "Ancaman Non-Militer?", "id": "Ideologi, Politik, Ekonomi, Sosial Budaya." },
  { "en": "Contoh Ancaman Ideologi?", "id": "Masuknya Paham Komunisme Atau Liberalisme." },
  { "en": "Contoh Ancaman Ekonomi?", "id": "Inflasi, Pengangguran, Ketergantungan Asing." },
  { "en": "Contoh Ancaman Sosial Budaya?", "id": "Gaya Hidup Konsumtif, Hedonisme." },
  { "en": "Apa Itu Hubungan Internasional?", "id": "Hubungan Antarnegara Atau Antarindividu." },
  { "en": "Pentingnya Hubungan Internasional?", "id": "Memenuhi Kebutuhan, Menjaga Perdamaian." },
  { "en": "Politik Luar Negeri Indonesia?", "id": "Bebas Aktif." },
  { "en": "Makna Bebas?", "id": "Tidak Memihak Blok Manapun." },
  { "en": "Makna Aktif?", "id": "Aktif Dalam Menciptakan Perdamaian Dunia." },
  { "en": "Landasan Politik Luar Negeri?", "id": "Pancasila Dan Undang-Undang Dasar (UUD) 1945." },
  { "en": "Organisasi Internasional Diikuti Indonesia?", "id": "Perserikatan Bangsa-Bangsa (PBB), Association of Southeast Asian Nations (ASEAN), Gerakan Non-Blok (GNB)." },
  { "en": "Apa Itu Perserikatan Bangsa-Bangsa (PBB)?", "id": "Organisasi Internasional Menjaga Perdamaian." },
  { "en": "Tujuan Perserikatan Bangsa-Bangsa (PBB)?", "id": "Menjaga Perdamaian, Memajukan Hak Asasi Manusia (HAM)." },
  { "en": "Peran Indonesia Di Perserikatan Bangsa-Bangsa (PBB)?", "id": "Mengirim Pasukan Garuda, Anggota Dewan." },
  { "en": "Apa Itu Association of Southeast Asian Nations (ASEAN)?", "id": "Organisasi Regional Asia Tenggara." },
  { "en": "Tujuan Association of Southeast Asian Nations (ASEAN)?", "id": "Mempercepat Pertumbuhan Ekonomi Dan Stabilitas." },
  { "en": "Peran Indonesia Di Association of Southeast Asian Nations (ASEAN)?", "id": "Salah Satu Negara Pendiri." },
  { "en": "Apa Itu Gerakan Non-Blok (GNB)?", "id": "Kelompok Negara Tidak Memihak." },
  { "en": "Tujuan Gerakan Non-Blok (GNB)?", "id": "Meredakan Ketegangan Blok Barat Timur." },
  { "en": "Peran Indonesia Di Gerakan Non-Blok (GNB)?", "id": "Salah Satu Pelopor Dan Penyelenggara." },
  { "en": "Apa Itu Perwakilan Diplomatik?", "id": "Perwakilan Negara Di Negara Lain." },
  { "en": "Tugas Perwakilan Diplomatik?", "id": "Representasi, Negosiasi, Proteksi, Observasi." },
  { "en": "Tingkatan Perwakilan Diplomatik?", "id": "Duta Besar, Duta, Kuasa Usaha." },
  { "en": "Apa Itu Perjanjian Internasional?", "id": "Kesepakatan Antar Subjek Hukum Internasional." },
  { "en": "Tahap Perjanjian Internasional?", "id": "Perundingan, Penandatanganan, Pengesahan (Ratifikasi)." },
  { "en": "Penyebab Sengketa Internasional?", "id": "Perebutan Wilayah, Sumber Daya Alam." },
  { "en": "Penyelesaian Sengketa Internasional?", "id": "Jalur Damai Atau Kekerasan." },
  { "en": "Penyelesaian Jalur Damai?", "id": "Negosiasi, Mediasi, Arbitrase, Pengadilan." },
  { "en": "Lembaga Peradilan Internasional?", "id": "Mahkamah Internasional (ICJ)." },
  { "en": "Apa Itu Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK)?", "id": "Perkembangan Ilmu Pengetahuan Dan Teknologi." },
  { "en": "Dampak Positif Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK)?", "id": "Mempermudah Komunikasi, Transportasi, Informasi." },
  { "en": "Dampak Negatif Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK)?", "id": "Penyalahgunaan, Kejahatan Siber, Ketergantungan." },
  { "en": "Sikap Terhadap Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK)?", "id": "Selektif Dan Bertanggung Jawab." },
  { "en": "Pengaruh Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK) Bidang Politik?", "id": "Keterbukaan, Kontrol Sosial, E-Government." },
  { "en": "Pengaruh Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK) Bidang Ekonomi?", "id": "E-commerce, Peningkatan Produktivitas, Persaingan." },
  { "en": "Pengaruh Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK) Bidang Sosial Budaya?", "id": "Difusi Budaya, Perubahan Gaya Hidup." },
  { "en": "Pengaruh Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK) Bidang Hukum?", "id": "Perlindungan Data, Kejahatan Siber." },
  { "en": "Etika Pemanfaatan Ilmu Pengetahuan Dan Teknologi (IPTEK)?", "id": "Tidak Merugikan Manusia Dan Lingkungan." },
  { "en": "Pancasila Sebagai Dasar Pengembangan Ilmu?", "id": "Ilmu Harus Berdasar Nilai Pancasila." },
  { "en": "Sila Pertama Dalam Pengembangan Ilmu?", "id": "Ilmu Tidak Boleh Bertentangan Agama." },
  { "en": "Sila Kedua Dalam Pengembangan Ilmu?", "id": "Ilmu Harus Menjunjung Tinggi Kemanusiaan." },
  { "en": "Sila Ketiga Dalam Pengembangan Ilmu?", "id": "Ilmu Harus Memperkuat Persatuan." },
  { "en": "Sila Keempat Dalam Pengembangan Ilmu?", "id": "Ilmu Harus Demokratis Dan Terbuka." },
  { "en": "Sila Kelima Dalam Pengembangan Ilmu?", "id": "Ilmu Harus Untuk Keadilan Sosial." },
  { "en": "Apa Itu Pers?", "id": "Media Massa Cetak Dan Elektronik." },
  { "en": "Fungsi Pers?", "id": "Informasi, Pendidikan, Hiburan, Kontrol Sosial." },
  { "en": "Peran Pers Dalam Demokrasi?", "id": "Pilar Keempat Demokrasi." },
  { "en": "Kebebasan Pers?", "id": "Kebebasan Menyampaikan Informasi Tanpa Sensor." },
  { "en": "Penyalahgunaan Kebebasan Pers?", "id": "Menyebarkan Berita Bohong Atau Fitnah." },
  { "en": "Kode Etik Jurnalistik?", "id": "Pedoman Perilaku Profesional Wartawan." },
  { "en": "Lembaga Pengawas Pers?", "id": "Dewan Pers." },
  { "en": "Apa Itu Norma?", "id": "Aturan Atau Kaidah Perilaku." },
  { "en": "Macam-Macam Norma?", "id": "Agama, Kesusilaan, Kesopanan, Hukum." },
  { "en": "Sanksi Norma Agama?", "id": "Dosa, Siksa Di Akhirat." },
  { "en": "Sanksi Norma Kesusilaan?", "id": "Penyesalan, Rasa Malu, Dikucilkan." },
  { "en": "Sanksi Norma Kesopanan?", "id": "Cemoohan, Celaan, Diasingkan." },
  { "en": "Sanksi Norma Hukum?", "id": "Tegas, Nyata, Mengikat (Penjara)." },
  { "en": "Apa Itu Keadilan?", "id": "Memberi Sesuatu Sesuai Haknya." },
  { "en": "Jenis Keadilan Menurut Aristoteles?", "id": "Distributif, Komutatif, Kodrat Alam." },
  { "en": "Keadilan Distributif?", "id": "Keadilan Berdasarkan Jasa Atau Prestasi." },
  { "en": "Keadilan Komutatif?", "id": "Keadilan Tanpa Memandang Jasa." },
  { "en": "Apa Itu Konstitusi?", "id": "Hukum Dasar Tertulis Suatu Negara." },
  { "en": "Fungsi Konstitusi?", "id": "Membatasi Kekuasaan, Menjamin Hak." },
  { "en": "Sifat Konstitusi?", "id": "Fleksibel Dan Rigid (Kaku)." },
  { "en": "Konstitusi Yang Pernah Berlaku?", "id": "Undang-Undang Dasar (UUD) 1945, Konstitusi Republik Indonesia Serikat (RIS), Undang-Undang Dasar Sementara (UUDS) 1950." },
  { "en": "Sistematika Undang-Undang Dasar (UUD) 1945 Sebelum Amandemen?", "id": "Pembukaan, Batang Tubuh, Penjelasan." },
  { "en": "Sistematika Undang-Undang Dasar (UUD) 1945 Setelah Amandemen?", "id": "Pembukaan Dan Pasal-pasal." },
  { "en": "Makna Alinea Pertama Pembukaan Undang-Undang Dasar (UUD)?", "id": "Dalil Objektif Dan Subjektif." },
  { "en": "Makna Alinea Kedua Pembukaan Undang-Undang Dasar (UUD)?", "id": "Cita-cita Kemerdekaan Indonesia." },
  { "en": "Makna Alinea Ketiga Pembukaan Undang-Undang Dasar (UUD)?", "id": "Motivasi Spiritual Kemerdekaan." },
  { "en": "Makna Alinea Keempat Pembukaan Undang-Undang Dasar (UUD)?", "id": "Tujuan, Bentuk, Dan Dasar Negara." },
  { "en": "Pokok Pikiran Pembukaan Undang-Undang Dasar (UUD) 1945?", "id": "Persatuan, Keadilan, Kedaulatan, Ketuhanan." },
  { "en": "Tata Urutan Peraturan Perundang-undangan?", "id": "Hierarki Peraturan Hukum Di Indonesia." },
  { "en": "Urutan Tertinggi Peraturan?", "id": "Undang-Undang Dasar Negara Republik Indonesia (UUD NRI) 1945." },
  { "en": "Urutan Terendah Peraturan?", "id": "Peraturan Daerah Kabupaten Atau Kota." },
  { "en": "Asas Pembentukan Peraturan?", "id": "Kejelasan Tujuan, Kelembagaan, Kesesuaian." },
  { "en": "Partisipasi Masyarakat Dalam Pembentukan?", "id": "Memberikan Masukan Lisan Atau Tertulis." },
  { "en": "Apa Itu Bhinneka Tunggal Ika?", "id": "Berbeda-beda Tetapi Tetap Satu." },
  { "en": "Makna Bhinneka Tunggal Ika?", "id": "Persatuan Dalam Keragaman Bangsa." },
  { "en": "Faktor Pendorong Persatuan?", "id": "Pancasila, Sumpah Pemuda, Rasa Senasib." },
  { "en": "Faktor Penghambat Persatuan?", "id": "Etnosentrisme, Primordialisme, Chauvinisme." },
  { "en": "Apa Itu Etnosentrisme?", "id": "Menganggap Budaya Sendiri Paling Baik." },
  { "en": "Apa Itu Primordialisme?", "id": "Memegang Teguh Hal Sejak Kecil." },
  { "en": "Apa Itu Chauvinisme?", "id": "Mengagungkan Bangsa Sendiri Secara Berlebihan." },
  { "en": "Keragaman Suku Di Indonesia?", "id": "Suku Jawa, Sunda, Batak, Dayak." },
  { "en": "Keragaman Agama Di Indonesia?", "id": "Islam, Kristen, Katolik, Hindu, Buddha." },
  { "en": "Keragaman Ras Di Indonesia?", "id": "Malayan-Mongoloid, Melanesoid, Asiatik." },
  { "en": "Semboyan Persatuan Dalam Perbedaan?", "id": "Bhinneka Tunggal Ika Tan Hana." },
  { "en": "Sistem Pertahanan Indonesia?", "id": "Sistem Pertahanan Dan Keamanan Rakyat Semesta (Sishankamrata)." },
  { "en": "Komponen Utama Sistem Pertahanan Dan Keamanan Rakyat Semesta (Sishankamrata)?", "id": "Tentara Nasional Indonesia (TNI)." },
  { "en": "Komponen Cadangan Sistem Pertahanan Dan Keamanan Rakyat Semesta (Sishankamrata)?", "id": "Warga Negara, Sumber Daya Alam." },
  { "en": "Komponen Pendukung Sistem Pertahanan Dan Keamanan Rakyat Semesta (Sishankamrata)?", "id": "Sarana Dan Prasarana Nasional." },
  { "en": "Tugas Tentara Nasional Indonesia (TNI)?", "id": "Menegakkan Kedaulatan, Mempertahankan Keutuhan." },
  { "en": "Tugas Kepolisian Republik Indonesia (Polri)?", "id": "Memelihara Keamanan Dan Ketertiban Masyarakat." },
  { "en": "Ancaman Militer?", "id": "Agresi, Spionase, Sabotase, Pemberontakan." },
  { "en": "Ancaman Non-Militer?", "id": "Ideologi, Politik, Ekonomi, Sosial Budaya." },
  { "en": "Contoh Ancaman Ideologi?", "id": "Masuknya Paham Komunisme Atau Liberalisme." },
  { "en": "Contoh Ancaman Ekonomi?", "id": "Inflasi, Pengangguran, Ketergantungan Asing." },
  { "en": "Contoh Ancaman Sosial Budaya?", "id": "Gaya Hidup Konsumtif, Hedonisme." },
  { "en": "Apa Itu Hubungan Internasional?", "id": "Hubungan Antarnegara Atau Antarindividu." },
  { "en": "Pentingnya Hubungan Internasional?", "id": "Memenuhi Kebutuhan, Menjaga Perdamaian." },
  { "en": "Politik Luar Negeri Indonesia?", "id": "Bebas Aktif." },
  { "en": "Makna Bebas?", "id": "Tidak Memihak Blok Manapun." },
  { "en": "Makna Aktif?", "id": "Aktif Dalam Menciptakan Perdamaian Dunia." },
  { "en": "Landasan Politik Luar Negeri?", "id": "Pancasila Dan Undang-Undang Dasar (UUD) 1945." },
  { "en": "Organisasi Internasional Diikuti Indonesia?", "id": "Perserikatan Bangsa-Bangsa (PBB), Association of Southeast Asian Nations (ASEAN), Gerakan Non-Blok (GNB)." },
  { "en": "Apa Itu Perserikatan Bangsa-Bangsa (PBB)?", "id": "Organisasi Internasional Menjaga Perdamaian." },
  { "en": "Tujuan Perserikatan Bangsa-Bangsa (PBB)?", "id": "Menjaga Perdamaian, Memajukan Hak Asasi Manusia (HAM)." },
  { "en": "Peran Indonesia Di Perserikatan Bangsa-Bangsa (PBB)?", "id": "Mengirim Pasukan Garuda, Anggota Dewan." },
  { "en": "Apa Itu Association of Southeast Asian Nations (ASEAN)?", "id": "Organisasi Regional Asia Tenggara." },
  { "en": "Tujuan Association of Southeast Asian Nations (ASEAN)?", "id": "Mempercepat Pertumbuhan Ekonomi Dan Stabilitas." },
  { "en": "Peran Indonesia Di Association of Southeast Asian Nations (ASEAN)?", "id": "Salah Satu Negara Pendiri." },
  { "en": "Apa Itu Gerakan Non-Blok (GNB)?", "id": "Kelompok Negara Tidak Memihak." },
  { "en": "Tujuan Gerakan Non-Blok (GNB)?", "id": "Meredakan Ketegangan Blok Barat Timur." },
  { "en": "Peran Indonesia Di Gerakan Non-Blok (GNB)?", "id": "Salah Satu Pelopor Dan Penyelenggara." },
  { "en": "Apa Itu Perwakilan Diplomatik?", "id": "Perwakilan Negara Di Negara Lain." },
  { "en": "Tugas Perwakilan Diplomatik?", "id": "Representasi, Negosiasi, Proteksi, Observasi." },
  { "en": "Tingkatan Perwakilan Diplomatik?", "id": "Duta Besar, Duta, Kuasa Usaha." },
  { "en": "Apa Itu Perjanjian Internasional?", "id": "Kesepakatan Antar Subjek Hukum Internasional." },
  { "en": "Tahap Perjanjian Internasional?", "id": "Perundingan, Penandatanganan, Pengesahan (Ratifikasi)." },
  { "en": "Penyebab Sengketa Internasional?", "id": "Perebutan Wilayah, Sumber Daya Alam." },
  { "en": "Penyelesaian Sengketa Internasional?", "id": "Jalur Damai Atau Kekerasan." },
  { "en": "Penyelesaian Jalur Damai?", "id": "Negosiasi, Mediasi, Arbitrase, Pengadilan." },
  { "en": "Lembaga Peradilan Internasional?", "id": "Mahkamah Internasional (ICJ)." },
  { "en": "Apa Itu Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK)?", "id": "Perkembangan Ilmu Pengetahuan Dan Teknologi." },
  { "en": "Dampak Positif Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK)?", "id": "Mempermudah Komunikasi, Transportasi, Informasi." },
  { "en": "Dampak Negatif Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK)?", "id": "Penyalahgunaan, Kejahatan Siber, Ketergantungan." },
  { "en": "Sikap Terhadap Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK)?", "id": "Selektif Dan Bertanggung Jawab." },
  { "en": "Pengaruh Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK) Bidang Politik?", "id": "Keterbukaan, Kontrol Sosial, E-Government." },
  { "en": "Pengaruh Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK) Bidang Ekonomi?", "id": "E-commerce, Peningkatan Produktivitas, Persaingan." },
  { "en": "Pengaruh Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK) Bidang Sosial Budaya?", "id": "Difusi Budaya, Perubahan Gaya Hidup." },
  { "en": "Pengaruh Kemajuan Ilmu Pengetahuan Dan Teknologi (IPTEK) Bidang Hukum?", "id": "Perlindungan Data, Kejahatan Siber." },
  { "en": "Etika Pemanfaatan Ilmu Pengetahuan Dan Teknologi (IPTEK)?", "id": "Tidak Merugikan Manusia Dan Lingkungan." },
  { "en": "Pancasila Sebagai Dasar Pengembangan Ilmu?", "id": "Ilmu Harus Berdasar Nilai Pancasila." },
  { "en": "Sila Pertama Dalam Pengembangan Ilmu?", "id": "Ilmu Tidak Boleh Bertentangan Agama." },
  { "en": "Sila Kedua Dalam Pengembangan Ilmu?", "id": "Ilmu Harus Menjunjung Tinggi Kemanusiaan." },
  { "en": "Sila Ketiga Dalam Pengembangan Ilmu?", "id": "Ilmu Harus Memperkuat Persatuan." },
  { "en": "Sila Keempat Dalam Pengembangan Ilmu?", "id": "Ilmu Harus Demokratis Dan Terbuka." },
  { "en": "Sila Kelima Dalam Pengembangan Ilmu?", "id": "Ilmu Harus Untuk Keadilan Sosial." },
  { "en": "Apa Itu Pers?", "id": "Media Massa Cetak Dan Elektronik." },
  { "en": "Fungsi Pers?", "id": "Informasi, Pendidikan, Hiburan, Kontrol Sosial." },
  { "en": "Peran Pers Dalam Demokrasi?", "id": "Pilar Keempat Demokrasi." },
  { "en": "Kebebasan Pers?", "id": "Kebebasan Menyampaikan Informasi Tanpa Sensor." },
  { "en": "Penyalahgunaan Kebebasan Pers?", "id": "Menyebarkan Berita Bohong Atau Fitnah." },
  { "en": "Kode Etik Jurnalistik?", "id": "Pedoman Perilaku Profesional Wartawan." },
  { "en": "Lembaga Pengawas Pers?", "id": "Dewan Pers." },
  { "en": "Apa Itu Otonomi Seluas-luasnya?", "id": "Kewenangan Penuh Urus Rumah Tangga." },
  { "en": "Apa Itu Daerah Perbatasan Negara?", "id": "Kecamatan Berbatasan Langsung Negara Tetangga." },
  { "en": "Badan Pengelola Perbatasan Negara?", "id": "Badan Nasional Pengelola Perbatasan (BNPP)." },
  { "en": "Apa Itu Kawasan Strategis Nasional?", "id": "Wilayah Penataan Ruangnya Diprioritaskan." },
  { "en": "Contoh Kawasan Strategis Nasional?", "id": "Kawasan Ibu Kota Nusantara (IKN)." },
  { "en": "Apa Itu Pelimpahan Wewenang?", "id": "Delegasi Atau Penyerahan Wewenang." },
  { "en": "Perbedaan Delegasi Dan Mandat?", "id": "Delegasi Pindah, Mandat Atas Nama." },
  { "en": "Prinsip Otonomi Daerah Menurut Undang-Undang Nomor 23 Tahun 2014?", "id": "Efisiensi, Eksternalitas, Akuntabilitas, Kepentingan." },
  { "en": "Apa Itu Locus Delicti?", "id": "Tempat Terjadinya Tindak Pidana." },
  { "en": "Apa Itu Tempus Delicti?", "id": "Waktu Terjadinya Tindak Pidana." },
  { "en": "Apa Itu Asas Teritorial?", "id": "Hukum Berlaku Di Wilayah Negara." },
  { "en": "Apa Itu Asas Personalitas?", "id": "Hukum Mengikuti Warga Negara." },
  { "en": "Apa Itu Asas Perlindungan?", "id": "Hukum Melindungi Kepentingan Nasional." },
  { "en": "Apa Itu Asas Universal?", "id": "Hukum Untuk Kejahatan Internasional." },
  { "en": "Apa Itu Ius Constitutum?", "id": "Hukum Yang Berlaku Saat Ini." },
  { "en": "Apa Itu Ius Constituendum?", "id": "Hukum Yang Dicita-citakan." },
  { "en": "Apa Itu Vacum of Power?", "id": "Kekosongan Kekuasaan Dalam Pemerintahan." },
  { "en": "Apa Itu Demisioner?", "id": "Kabinet Yang Menjalankan Tugas Rutin." },
  { "en": "Penyebab Kabinet Demisioner?", "id": "Menunggu Terbentuknya Kabinet Baru." },
  { "en": "Apa Itu Intervensi Negara?", "id": "Campur Tangan Negara Dalam Urusan." },
  { "en": "Bentuk Intervensi Ekonomi?", "id": "Subsidi, Pajak, Dan Regulasi." },
  { "en": "Apa Itu Deregulasi?", "id": "Pengurangan Atau Penghapusan Peraturan." },
  { "en": "Tujuan Deregulasi?", "id": "Meningkatkan Efisiensi Dan Daya Saing." },
  { "en": "Apa Itu Birokratisasi?", "id": "Proses Menjadi Semakin Birokratis." },
  { "en": "Dampak Negatif Birokratisasi?", "id": "Prosedur Berbelit, Lambat, Tidak Fleksibel." },
  { "en": "Apa Itu Depolitisasi?", "id": "Proses Menghilangkan Unsur Politik." },
  { "en": "Contoh Depolitisasi?", "id": "Netralitas Aparatur Sipil Negara (ASN) Dan Tentara Nasional Indonesia (TNI) Atau Kepolisian Republik Indonesia (Polri)." },
  { "en": "Apa Itu Judicial Activism?", "id": "Hakim Menafsirkan Hukum Secara Progresif." },
  { "en": "Apa Itu Judicial Restraint?", "id": "Hakim Menahan Diri Menafsirkan Hukum." },
  { "en": "Perdebatan Judicial Activism?", "id": "Antara Keadilan Substantif Dan Kepastian." },
  { "en": "Apa Itu Contempt of Court?", "id": "Perbuatan Menghina Lembaga Peradilan." },
  { "en": "Contoh Contempt of Court?", "id": "Tidak Menaati Perintah Hakim." },
  { "en": "Apa Itu Dissenting Opinion?", "id": "Pendapat Berbeda Dari Hakim." },
  { "en": "Pentingnya Dissenting Opinion?", "id": "Menunjukkan Adanya Perbedaan Penafsiran." },
  { "en": "Apa Itu Stare Decisis?", "id": "Hakim Terikat Putusan Hakim Sebelumnya." },
  { "en": "Sistem Hukum Yang Menganut Stare Decisis?", "id": "Sistem Anglo-Saxon (Common Law)." },
  { "en": "Apa Itu Civil Disobedience?", "id": "Pembangkangan Sipil Terhadap Hukum." },
  { "en": "Tujuan Pembangkangan Sipil?", "id": "Protes Terhadap Hukum Yang Dianggap." },
  { "en": "Contoh Pembangkangan Sipil?", "id": "Gerakan Hak Sipil Di Amerika Serikat." },
  { "en": "Apa Itu Pressure Group?", "id": "Kelompok Penekan Yang Mempengaruhi Kebijakan." },
  { "en": "Metode Pressure Group?", "id": "Lobi, Demonstrasi, Kampanye Media." },
  { "en": "Apa Itu Civil Society Organization (CSO)?", "id": "Organisasi Masyarakat Sipil." },
  { "en": "Peran Civil Society Organization (CSO) Dalam Politik?", "id": "Advokasi, Pemberdayaan, Dan Pengawasan." },
  { "en": "Apa Itu Voting Bloc?", "id": "Kelompok Pemilih Dengan Kepentingan Sama." },
  { "en": "Contoh Voting Bloc?", "id": "Kelompok Etnis, Agama, Atau Profesi." },
  { "en": "Apa Itu Incumbent?", "id": "Pejabat Yang Sedang Menjabat." },
  { "en": "Keuntungan Incumbent?", "id": "Popularitas, Akses Media, Sumber Daya." },
  { "en": "Apa Itu Oposisi?", "id": "Pihak Penentang Pemerintah." },
  { "en": "Peran Oposisi Dalam Demokrasi?", "id": "Menjadi Mekanisme Kontrol Dan Keseimbangan." },
  { "en": "Apa Itu Koalisi Gemuk?", "id": "Koalisi Melibatkan Terlalu Banyak Partai." },
  { "en": "Risiko Koalisi Gemuk?", "id": "Tidak Efektif Dan Rawan Konflik." },
  { "en": "Apa Itu Power Sharing?", "id": "Pembagian Kekuasaan Antar Kelompok." },
  { "en": "Tujuan Power Sharing?", "id": "Mencegah Dominasi Dan Menjaga Stabilitas." },
  { "en": "Bentuk Power Sharing?", "id": "Koalisi, Federalisme, Otonomi Khusus." },
  { "en": "Apa Itu Politik Akomodatif?", "id": "Politik Yang Mencari Kompromi." },
  { "en": "Pentingnya Politik Akomodatif?", "id": "Menjaga Persatuan Dalam Masyarakat Majemuk." },
  { "en": "Apa Itu Konsosiasionalisme?", "id": "Model Demokrasi Untuk Masyarakat Terbelah." },
  { "en": "Prinsip Konsosiasionalisme?", "id": "Koalisi Besar, Veto Minoritas, Otonomi." },
  { "en": "Apa Itu Hak Asal-Usul?", "id": "Hak Bawaan Suatu Daerah." },
  { "en": "Apa Itu Kewenangan Lokal Berskala Desa?", "id": "Kewenangan Mengatur Kepentingan Masyarakat Desa." },
  { "en": "Apa Itu Pemilu Serentak?", "id": "Pemilu Legislatif Dan Presiden Bersamaan." },
  { "en": "Tujuan Pemilu Serentak?", "id": "Efisiensi Dan Sinkronisasi Pemerintahan." },
  { "en": "Dampak Pemilu Serentak?", "id": "Beban Kerja Penyelenggara, Efek Ekor Jas." },
  { "en": "Apa Itu Efek Ekor Jas (Coattail Effect)?", "id": "Pengaruh Calon Presiden Terhadap Legislatif." },
  { "en": "Apa Itu Split-Ticket Voting?", "id": "Memilih Calon Dari Partai Berbeda." },
  { "en": "Apa Itu Straight-Ticket Voting?", "id": "Memilih Semua Calon Dari Partai." },
  { "en": "Penyebab Split-Ticket Voting?", "id": "Fokus Pada Kualitas Individu Calon." },
  { "en": "Apa Itu Identifikasi Partai?", "id": "Keterikatan Psikologis Pemilih Pada Partai." },
  { "en": "Tren Identifikasi Partai?", "id": "Cenderung Menurun Di Banyak Negara." },
  { "en": "Apa Itu Dealignment?", "id": "Melemahnya Keterikatan Pemilih Pada Partai." },
  { "en": "Apa Itu Realignment?", "id": "Pergeseran Dukungan Pemilih Antar Partai." },
  { "en": "Apa Itu Isu Publik?", "id": "Masalah Yang Menjadi Perhatian Umum." },
  { "en": "Siklus Kebijakan Publik?", "id": "Agenda, Formulasi, Adopsi, Implementasi." },
  { "en": "Model Pembuatan Kebijakan?", "id": "Model Rasional, Inkremental, Keranjang Sampah." },
  { "en": "Apa Itu Model Rasional?", "id": "Pembuatan Kebijakan Berbasis Analisis." },
  { "en": "Apa Itu Model Inkremental?", "id": "Perubahan Kecil Dari Kebijakan Lama." },
  { "en": "Apa Itu Model Keranjang Sampah?", "id": "Keputusan Diambil Secara Acak." },
  { "en": "Apa Itu Evaluasi Kebijakan?", "id": "Penilaian Terhadap Hasil Suatu Kebijakan." },
  { "en": "Jenis Evaluasi Kebijakan?", "id": "Evaluasi Proses, Dampak, Dan Hasil." },
  { "en": "Apa Itu Advokasi Kebijakan?", "id": "Upaya Mempengaruhi Proses Kebijakan." },
  { "en": "Aktor Advokasi Kebijakan?", "id": "Lembaga Swadaya Masyarakat (LSM), Kelompok Kepentingan, Akademisi." },
  { "en": "Apa Itu Jaringan Kebijakan (Policy Network)?", "id": "Interaksi Aktor Dalam Suatu Sektor." },
  { "en": "Apa Itu Komunitas Epistemik?", "id": "Jaringan Profesional Berbasis Keahlian." },
  { "en": "Peran Komunitas Epistemik?", "id": "Memberikan Masukan Ilmiah Dalam Kebijakan." },
  { "en": "Apa Itu Regulasi?", "id": "Aturan Dibuat Pemerintah Mengatur Perilaku." },
  { "en": "Apa Itu Kegagalan Pasar (Market Failure)?", "id": "Pasar Gagal Alokasikan Sumber Daya." },
  { "en": "Contoh Kegagalan Pasar?", "id": "Eksternalitas, Barang Publik, Informasi Asimetris." },
  { "en": "Apa Itu Eksternalitas?", "id": "Dampak Tindakan Ekonomi Pihak Ketiga." },
  { "en": "Contoh Eksternalitas Negatif?", "id": "Polusi Udara Dari Pabrik." },
  { "en": "Apa Itu Barang Publik?", "id": "Barang Non-Rival Dan Non-Ekskludabel." },
  { "en": "Contoh Barang Publik?", "id": "Pertahanan Nasional, Penerangan Jalan." },
  { "en": "Apa Itu Kegagalan Pemerintah (Government Failure)?", "id": "Intervensi Pemerintah Gagal Capai Hasil." },
  { "en": "Penyebab Kegagalan Pemerintah?", "id": "Informasi Terbatas, Kepentingan Pribadi." },
  { "en": "Apa Itu Pemerintahan Kolaboratif?", "id": "Pemerintah Bekerja Sama Dengan Non-Pemerintah." },
  { "en": "Aktor Pemerintahan Kolaboratif?", "id": "Pemerintah, Swasta, Masyarakat Sipil." },
  { "en": "Prinsip Pemerintahan Kolaboratif?", "id": "Kepercayaan, Komitmen Bersama, Komunikasi." },
  { "en": "Apa Itu Open Government?", "id": "Pemerintahan Terbuka, Transparan, Partisipatif." },
  { "en": "Tiga Pilar Open Government?", "id": "Transparansi, Partisipasi, Dan Kolaborasi." },
  { "en": "Apa Itu Open Data?", "id": "Data Pemerintah Yang Dapat Diakses." },
  { "en": "Manfaat Open Data?", "id": "Meningkatkan Transparansi Dan Inovasi." },
  { "en": "Apa Itu Sensus Penduduk?", "id": "Penghitungan Jumlah Penduduk Secara Lengkap." },
  { "en": "Penyelenggara Sensus Penduduk?", "id": "Badan Pusat Statistik (BPS)." },
  { "en": "Berapa Tahun Sekali Sensus Diadakan?", "id": "Sepuluh Tahun Sekali." },
  { "en": "Apa Itu Proyeksi Penduduk?", "id": "Perkiraan Jumlah Penduduk Masa Depan." },
  { "en": "Faktor Mempengaruhi Pertumbuhan Penduduk?", "id": "Kelahiran, Kematian, Dan Migrasi." },
  { "en": "Apa Itu Piramida Penduduk?", "id": "Grafik Komposisi Penduduk Berdasarkan Usia." },
  { "en": "Bentuk Piramida Penduduk?", "id": "Ekspansif (Muda), Stasioner, Konstruktif (Tua)." },
  { "en": "Apa Itu Transisi Demografi?", "id": "Perubahan Tingkat Kelahiran Kematian." },
  { "en": "Apa Itu Pembangunan Berwawasan Kependudukan?", "id": "Pembangunan Mempertimbangkan Aspek Kependudukan." },
  { "en": "Lembaga Pengelola Kependudukan?", "id": "Badan Kependudukan dan Keluarga Berencana Nasional (BKKBN)." },
  { "en": "Apa Itu Keluarga Berencana (KB)?", "id": "Upaya Mengatur Kelahiran Anak." },
  { "en": "Apa Itu Adat Istiadat?", "id": "Aturan Tidak Tertulis Di Masyarakat." },
  { "en": "Apa Itu Hukum Adat?", "id": "Adat Yang Memiliki Sanksi." },
  { "en": "Eksistensi Hukum Adat?", "id": "Diakui Selama Sesuai Pancasila." },
  { "en": "Apa Itu Masyarakat Hukum Adat?", "id": "Kelompok Masyarakat Dengan Tatanan Adat." },
  { "en": "Hak Ulayat?", "id": "Hak Kolektif Masyarakat Hukum Adat." },
  { "en": "Penyelesaian Sengketa Adat?", "id": "Melalui Musyawarah Dan Peradilan Adat." },
  { "en": "Apa Itu Peradilan Adat?", "id": "Lembaga Peradilan Berdasarkan Hukum Adat." },
  { "en": "Apa Itu Trias Politika?", "id": "Pemisahan Kekuasaan Menjadi Tiga." },
  { "en": "Tiga Kekuasaan Trias Politika?", "id": "Legislatif, Eksekutif, Dan Yudikatif." },
  { "en": "Pencetus Trias Politika?", "id": "Montesquieu." },
  { "en": "Tujuan Trias Politika?", "id": "Mencegah Penyalahgunaan Kekuasaan (Abuse of Power)." },
  { "en": "Indonesia Menganut Trias Politika?", "id": "Menganut Pembagian Kekuasaan." },
  { "en": "Apa Itu Kekuasaan Moneter?", "id": "Kekuasaan Mengatur Dan Menjaga Kelancaran." },
  { "en": "Pemegang Kekuasaan Moneter?", "id": "Bank Indonesia (BI) Sebagai Bank Sentral." },
  { "en": "Status Bank Indonesia (BI)?", "id": "Lembaga Negara Yang Independen." },
  { "en": "Tugas Bank Indonesia (BI)?", "id": "Mencapai Dan Memelihara Kestabilan Rupiah." },
  { "en": "Apa Itu Otoritas Jasa Keuangan (OJK)?", "id": "Lembaga Mengawasi Sektor Jasa Keuangan." },
  { "en": "Sektor Yang Diawasi Otoritas Jasa Keuangan (OJK)?", "id": "Perbankan, Pasar Modal, Asuransi." },
  { "en": "Apa Itu Lembaga Penjamin Simpanan (LPS)?", "id": "Lembaga Menjamin Simpanan Nasabah Bank." },
  { "en": "Tujuan Lembaga Penjamin Simpanan (LPS)?", "id": "Menjaga Kepercayaan Masyarakat Pada Bank." },
  { "en": "Pancasila Sebagai Paradigma Pembangunan?", "id": "Pancasila Menjadi Acuan Pembangunan." },
  { "en": "Pembangunan Bidang Politik?", "id": "Mengembangkan Sistem Politik Demokratis." },
  { "en": "Pembangunan Bidang Ekonomi?", "id": "Mengembangkan Sistem Ekonomi Kerakyatan." },
  { "en": "Pembangunan Bidang Sosial Budaya?", "id": "Menciptakan Masyarakat Beradab Dan Berbudaya." },
  { "en": "Pembangunan Bidang Hukum?", "id": "Mewujudkan Sistem Hukum Nasional." },
  { "en": "Pembangunan Bidang Pertahanan Keamanan?", "id": "Mewujudkan Sistem Pertahanan Dan Keamanan Rakyat Semesta (Sishankamrata) Berbasis Kesadaran." },
  { "en": "Apa Itu Negara Hukum?", "id": "Negara Berdasarkan Atas Hukum." },
  { "en": "Ciri Negara Hukum?", "id": "Supremasi Hukum, Persamaan Kedudukan." },
  { "en": "Indonesia Adalah Negara Hukum?", "id": "Tercantum Dalam Pasal 1 Ayat 3 Undang-Undang Dasar (UUD) 1945." },
  { "en": "Apa Itu Due Process of Law?", "id": "Proses Hukum Yang Adil." },
  { "en": "Apa Itu Pemerintahan Yang Bersih?", "id": "Bebas Dari Korupsi, Kolusi, Dan Nepotisme (KKN)." },
  { "en": "Apa Itu Good Governance?", "id": "Penyelenggaraan Pemerintahan Yang Baik." },
  { "en": "Prinsip Good Governance?", "id": "Transparansi, Akuntabilitas, Partisipasi, Supremasi." },
  { "en": "Apa Itu Akuntabilitas?", "id": "Kewajiban Mempertanggungjawabkan Kinerja." },
  { "en": "Apa Itu Transparansi?", "id": "Keterbukaan Informasi Kepada Publik." },
  { "en": "Apa Itu Partisipasi?", "id": "Keterlibatan Masyarakat Dalam Pembangunan." },
  { "en": "Apa Itu Supremasi Hukum?", "id": "Hukum Memiliki Kedudukan Tertinggi." },
  { "en": "Apa Itu Reformasi?", "id": "Perubahan Tatanan Kehidupan Lama." },
  { "en": "Latar Belakang Reformasi 1998?", "id": "Krisis Ekonomi, Politik, Dan Kepercayaan." },
  { "en": "Tujuan Reformasi?", "id": "Mewujudkan Demokrasi Dan Pemerintahan Bersih." },
  { "en": "Dampak Positif Reformasi?", "id": "Kebebasan Pers, Pemilu Demokratis." },
  { "en": "Tantangan Era Reformasi?", "id": "Korupsi, Penegakan Hukum, Disintegrasi." },
  { "en": "Pilar Demokrasi Indonesia?", "id": "Pancasila, Undang-Undang Dasar (UUD) 1945, Negara Kesatuan Republik Indonesia (NKRI), Bhinneka." },
  { "en": "Empat Konsensus Dasar Berbangsa?", "id": "Pancasila, Undang-Undang Dasar (UUD) 1945, Negara Kesatuan Republik Indonesia (NKRI), Bhinneka." },
  { "en": "Pentingnya Empat Konsensus Dasar?", "id": "Menjaga Keutuhan Dan Persatuan Bangsa." },
  { "en": "Sosialisasi Empat Pilar?", "id": "Upaya Memasyarakatkan Konsensus Dasar Bangsa." },
  { "en": "Pelaksana Sosialisasi Empat Pilar?", "id": "Majelis Permusyawaratan Rakyat (MPR) Republik Indonesia." },
  { "en": "Apa Itu Kewarganegaraan Digital?", "id": "Norma Perilaku Di Dunia Digital." },
  { "en": "Aspek Kewarganegaraan Digital?", "id": "Akses, Komunikasi, Literasi Digital." },
  { "en": "Apa Itu Jejak Digital (Digital Footprint)?", "id": "Data Ditinggalkan Saat Aktivitas Online." },
  { "en": "Jenis Jejak Digital?", "id": "Jejak Digital Aktif Dan Pasif." },
  { "en": "Pentingnya Menjaga Jejak Digital?", "id": "Melindungi Privasi Dan Reputasi Diri." },
  { "en": "Apa Itu Keamanan Siber (Cyber Security)?", "id": "Praktik Melindungi Sistem Dari Serangan." },
  { "en": "Ancaman Keamanan Siber?", "id": "Malware, Phishing, Peretasan (Hacking)." },
  { "en": "Lembaga Keamanan Siber Nasional?", "id": "Badan Siber dan Sandi Negara (BSSN)." },
  { "en": "Apa Itu Privasi?", "id": "Hak Untuk Mengontrol Informasi Pribadi." },
  { "en": "Dasar Hukum Perlindungan Data Pribadi?", "id": "Undang-Undang Perlindungan Data Pribadi (UU PDP)." },
  { "en": "Apa Itu Neutralitas Net?", "id": "Prinsip Perlakuan Sama Semua Data." },
  { "en": "Perdebatan Neutralitas Net?", "id": "Inovasi Versus Akses Terjangkau." },
  { "en": "Apa Itu Gig Economy?", "id": "Pasar Tenaga Kerja Berbasis Kontrak." },
  { "en": "Dampak Gig Economy?", "id": "Fleksibilitas Kerja, Kurangnya Jaminan Sosial." },
  { "en": "Apa Itu Artificial Intelligence (AI)?", "id": "Kecerdasan Buatan Pada Sistem Komputer." },
  { "en": "Dampak Artificial Intelligence (AI) Pada Politik?", "id": "Analisis Data, Kampanye, Pengawasan." },
  { "en": "Etika Penggunaan Artificial Intelligence (AI)?", "id": "Transparansi, Keadilan, Dan Akuntabilitas." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Terhubung Internet." },
  { "en": "Contoh Internet of Things (IoT) Dalam Pemerintahan?", "id": "Smart City, Pemantauan Lingkungan." },
  { "en": "Apa Itu Big Data?", "id": "Kumpulan Data Sangat Besar Kompleks." },
  { "en": "Pemanfaatan Big Data Politik?", "id": "Memahami Sentimen Publik, Mikrotargeting." },
  { "en": "Apa Itu Mikrotargeting?", "id": "Penyampaian Pesan Politik Sangat Spesifik." },
  { "en": "Risiko Mikrotargeting?", "id": "Manipulasi Opini Dan Polarisasi." },
  { "en": "Apa Itu Gerakan Sosial Baru?", "id": "Gerakan Fokus Isu Identitas." },
  { "en": "Contoh Gerakan Sosial Baru?", "id": "Gerakan Lingkungan, Feminisme, Lesbian, Gay, Biseksual, Dan Transgender (LGBT)." },
  { "en": "Perbedaan Gerakan Sosial Lama?", "id": "Gerakan Lama Fokus Pada Isu Kelas." },
  { "en": "Apa Itu Politik Pengakuan?", "id": "Perjuangan Pengakuan Identitas Kelompok." }




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
