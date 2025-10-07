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


{ "en": "Apa Fungsi Utama Dari Organ Jantung?", "id": "Memompa Darah Ke Seluruh Tubuh." },
{ "en": "Dimanakah Letak Organ Paru-Paru Manusia?", "id": "Di Dalam Rongga Dada Manusia." },
{ "en": "Organ Apa Yang Berfungsi Menyaring Darah?", "id": "Ginjal, Penyaring Limbah Dan Racun." },
{ "en": "Apa Nama Lain Penyakit Kuning?", "id": "Ikterus, Atau Penyakit Kuning Medis." },
{ "en": "Dokter Spesialis Anak Disebut Juga Apa?", "id": "Pediatrician, Dokter Ahli Penyakit Anak." },
{ "en": "Apa Fungsi Utama Dari Organ Hati?", "id": "Menetralkan Racun Dan Produksi Empedu." },
{ "en": "Bagian Otak Mana Mengatur Keseimbangan Tubuh?", "id": "Otak Kecil Atau Serebelum." },
{ "en": "Penyakit Radang Sendi Dikenal Sebagai Apa?", "id": "Artritis, Peradangan Nyeri Pada Sendi." },
{ "en": "Ahli Penyakit Jantung Dan Pembuluh Darah?", "id": "Kardiolog, Spesialis Jantung Pembuluh Darah." },
{ "en": "Apa Fungsi Kulit Bagi Tubuh Manusia?", "id": "Melindungi Tubuh Dari Kuman, Cedera." },
{ "en": "Organ Pencernaan Utama Setelah Lambung Adalah?", "id": "Usus Halus, Tempat Penyerapan Nutrisi." },
{ "en": "Penyakit Gula Darah Tinggi Disebut Apa?", "id": "Diabetes Melitus, Kekurangan Hormon Insulin." },
{ "en": "Siapakah Dokter Spesialis Bedah Tulang?", "id": "Ortopedi, Ahli Bedah Masalah Tulang." },
{ "en": "Apa Fungsi Rangka Bagi Tubuh Manusia?", "id": "Menopang Tubuh Dan Melindungi Organ." },
{ "en": "Dimana Proses Pencernaan Makanan Dimulai?", "id": "Di Dalam Mulut Oleh Enzim Ptialin." },
{ "en": "Tekanan Darah Tinggi Dikenal Istilah Apa?", "id": "Hipertensi, Kondisi Tekanan Darah Abnormal." },
{ "en": "Dokter Ahli Penyakit Kulit Disebut Apa?", "id": "Dermatolog, Spesialis Kulit Dan Kelamin." },
{ "en": "Apa Fungsi Utama Dari Organ Lambung?", "id": "Mencerna Makanan Dengan Asam Lambung." },
{ "en": "Sistem Saraf Pusat Terdiri Dari Apa?", "id": "Otak Dan Sumsum Tulang Belakang." },
{ "en": "Penyakit Kanker Darah Dikenal Sebagai Apa?", "id": "Leukemia, Kanker Sel Darah Putih." },
{ "en": "Siapakah Ahli Penyakit Gangguan Jiwa?", "id": "Psikiater Atau Seorang Dokter Jiwa." },
{ "en": "Apa Fungsi Alveolus Di Paru-Paru?", "id": "Tempat Pertukaran Oksigen, Karbon Dioksida." },
{ "en": "Apa Nama Sel Pembawa Oksigen?", "id": "Eritrosit Atau Sel Darah Merah." },
{ "en": "Penyakit Gondok Disebabkan Kekurangan Apa?", "id": "Kekurangan Yodium Dalam Jangka Waktu Panjang." },
{ "en": "Dokter Spesialis Penyakit Telinga Hidung Tenggorokan?", "id": "Otolaringolog, Ahli THT (Telinga, Hidung, Tenggorokan)." },
{ "en": "Apa Fungsi Hormon Insulin Bagi Tubuh?", "id": "Mengatur Kadar Gula Dalam Darah." },
{ "en": "Bagian Mata Yang Mengatur Cahaya Masuk?", "id": "Pupil, Dapat Melebar Dan Menyempit." },
{ "en": "Penyakit Demam Berdarah Disebabkan Oleh Apa?", "id": "Virus Dengue Dari Gigitan Nyamuk." },
{ "en": "Ahli Penyakit Kanker Disebut Juga Apa?", "id": "Onkolog, Merawat Pasien Penderita Kanker." },
{ "en": "Apa Fungsi Utama Dari Usus Besar?", "id": "Menyerap Air Sisa Dan Membentuk Feses." },
{ "en": "Apa Nama Lapisan Terluar Dari Gigi?", "id": "Email, Jaringan Paling Keras Tubuh." },
{ "en": "Penyakit Campak Disebut Juga Dengan Istilah?", "id": "Morbili, Disebabkan Oleh Infeksi Virus." },
{ "en": "Siapa Dokter Spesialis Untuk Ibu Hamil?", "id": "Obstetrisian, Ahli Kandungan Dan Ginekologi." },
{ "en": "Apa Fungsi Kelenjar Tiroid Di Leher?", "id": "Menghasilkan Hormon Untuk Metabolisme Tubuh." },
{ "en": "Bagian Telinga Mana Menjaga Keseimbangan?", "id": "Telinga Dalam, Saluran Setengah Lingkaran." },
{ "en": "Penyakit Cacar Air Disebabkan Oleh Apa?", "id": "Infeksi Virus Varicella-Zoster Yang Menular." },
{ "en": "Dokter Ahli Penyakit Saraf Disebut Apa?", "id": "Neurolog, Menangani Gangguan Sistem Saraf." },
{ "en": "Apa Fungsi Sel Darah Putih?", "id": "Melawan Infeksi Kuman Penyebab Penyakit." },
{ "en": "Organ Apa Yang Memproduksi Sel Telur?", "id": "Ovarium Atau Indung Telur Wanita." },
{ "en": "Istilah Medis Untuk Penyakit Maag Adalah?", "id": "Gastritis, Peradangan Pada Dinding Lambung." },
{ "en": "Ahli Penyakit Ginjal Dan Saluran Kemih?", "id": "Nefrolog, Spesialis Fungsi Organ Ginjal." },
{ "en": "Apa Fungsi Pankreas Dalam Sistem Pencernaan?", "id": "Menghasilkan Enzim Pencernaan Serta Insulin." },
{ "en": "Apa Tulang Terpanjang Di Tubuh Manusia?", "id": "Tulang Paha Atau Disebut Femur." },
{ "en": "Penyakit TBC Singkatan (Tuberkulosis) Menyerang Organ?", "id": "Paru-Paru, Namun Bisa Organ Lain." },
{ "en": "Dokter Spesialis Anestesi Disebut Juga Apa?", "id": "Anestesiolog, Mengelola Nyeri Saat Operasi." },
{ "en": "Apa Fungsi Empedu Yang Dihasilkan Hati?", "id": "Membantu Mencerna Lemak Di Usus." },
{ "en": "Apa Nama Medis Untuk Sel Otot?", "id": "Miosit, Sel Pembentuk Jaringan Otot." },
{ "en": "Penyakit Asma Adalah Gangguan Pada Organ?", "id": "Saluran Pernapasan, Menyebabkan Sesak Napas." },
{ "en": "Siapakah Ahli Penyakit Pada Saluran Cerna?", "id": "Gastroenterolog, Spesialis Sistem Pencernaan Manusia." },
{ "en": "Apa Fungsi Lensa Pada Organ Mata?", "id": "Memfokuskan Cahaya Ke Bagian Retina." },
{ "en": "Bagian Darah Yang Membantu Proses Pembekuan?", "id": "Trombosit Atau Keping Sel Darah." },
{ "en": "Istilah Medis Untuk Sulit Tidur Adalah?", "id": "Insomnia, Gangguan Tidur Malam Hari." },
{ "en": "Ahli Patologi Mendiagnosis Penyakit Melalui Apa?", "id": "Pemeriksaan Jaringan Tubuh Dan Cairan." },
{ "en": "Apa Fungsi Sumsum Tulang Belakang Manusia?", "id": "Menghubungkan Sinyal Saraf Otak Tubuh." },
{ "en": "Apa Nama Sendi Peluru Pada Bahu?", "id": "Sendi Glenohumeral, Gerakan Paling Bebas." },
{ "en": "Penyakit Hepatitis Merupakan Peradangan Pada Organ?", "id": "Organ Hati, Akibat Infeksi Virus." },
{ "en": "Dokter Spesialis Mata Disebut Juga Apa?", "id": "Oftalmolog, Menangani Kesehatan Organ Mata." },
{ "en": "Apa Fungsi Otot Diafragma Saat Bernapas?", "id": "Membantu Proses Inspirasi Dan Ekspirasi." },
{ "en": "Organ Mana Yang Memproduksi Sel Sperma?", "id": "Testis, Organ Reproduksi Pria Utama." },
{ "en": "Istilah Medis Untuk Sakit Kepala Hebat?", "id": "Migrain, Nyeri Berdenyut Di Kepala." },
{ "en": "Ahli Radiologi Menggunakan Apa Untuk Diagnosis?", "id": "Pencitraan Medis Seperti Sinar-X, MRI." },
{ "en": "Apa Fungsi Dari Kelenjar Adrenal?", "id": "Menghasilkan Hormon Adrenalin Dan Kortisol." },
{ "en": "Struktur Dalam Hidung Yang Menyaring Udara?", "id": "Rambut Atau Bulu Hidung (Vibrissae)." },
{ "en": "Penyakit Osteoporosis Adalah Pengeroposan Pada Apa?", "id": "Tulang, Membuatnya Menjadi Rapuh, Lemah." },
{ "en": "Dokter Spesialis Paru Dan Pernapasan Adalah?", "id": "Pulmonolog, Ahli Penyakit Sistem Pernapasan." },
{ "en": "Apa Fungsi Epiglotis Di Tenggorokan?", "id": "Mencegah Makanan Masuk Saluran Napas." },
{ "en": "Apa Nama Medis Untuk Sel Saraf?", "id": "Neuron, Unit Dasar Sistem Saraf." },
{ "en": "Penyakit Stroke Terjadi Akibat Gangguan Apa?", "id": "Gangguan Pasokan Darah Menuju Ke Otak." },
{ "en": "Ahli Endokrinologi Menangani Penyakit Terkait Apa?", "id": "Sistem Hormon Dan Kelenjar Endokrin." },
{ "en": "Apa Fungsi Rahim Dalam Sistem Reproduksi?", "id": "Tempat Berkembangnya Janin Selama Kehamilan." },
{ "en": "Bagian Otak Manakah Mengontrol Detak Jantung?", "id": "Batang Otak, Tepatnya Medula Oblongata." },
{ "en": "Penyakit Anemia Adalah Kekurangan Sel Apa?", "id": "Sel Darah Merah Atau Hemoglobin." },
{ "en": "Siapakah Ahli Penyakit Alergi Dan Imunologi?", "id": "Alergolog-Imunolog, Ahli Sistem Kekebalan." },
{ "en": "Apa Fungsi Keping Darah Atau Trombosit?", "id": "Membantu Dalam Proses Pembekuan Darah." },
{ "en": "Organ Vestigial Yang Sering Radang?", "id": "Usus Buntu Atau Apendiks Vermiformis." },
{ "en": "Istilah Untuk Penyakit Radang Otak Adalah?", "id": "Ensefalitis, Peradangan Pada Jaringan Otak." },
{ "en": "Dokter Spesialis Darah Disebut Juga Apa?", "id": "Hematolog, Ahli Penyakit Kelainan Darah." },
{ "en": "Apa Fungsi Pembuluh Darah Vena Pulmonalis?", "id": "Membawa Darah Kaya Oksigen Ke Jantung." },
{ "en": "Apa Jaringan Ikat Penghubung Tulang Otot?", "id": "Tendon, Jaringan Ikat Fibrosa Kuat." },
{ "en": "Penyakit Epilepsi Disebut Juga Penyakit Apa?", "id": "Ayan, Gangguan Aktivitas Listrik Otak." },
{ "en": "Ahli Yang Mempelajari Jaringan Tubuh Adalah?", "id": "Histolog, Spesialis Dalam Bidang Histologi." },
{ "en": "Apa Fungsi Dari Kelenjar Getah Bening?", "id": "Menyaring Cairan Limfe, Melawan Infeksi." },
{ "en": "Apa Otot Terkuat Di Tubuh Manusia?", "id": "Otot Masseter, Berada Di Rahang." },
{ "en": "Istilah Medis Untuk Radang Tenggorokan Adalah?", "id": "Faringitis, Peradangan Di Bagian Faring." },
{ "en": "Dokter Ahli Bayi Baru Lahir Adalah?", "id": "Neonatolog, Subspesialis Dari Ilmu Anak." },
{ "en": "Apa Fungsi Dari Saluran Eustachius?", "id": "Menyeimbangkan Tekanan Udara Telinga Tengah." },
{ "en": "Dimana Letak Kelenjar Pituitari Atau Hipofisis?", "id": "Di Dasar Organ Otak Manusia." },
{ "en": "Penyakit Gagal Jantung Kongestif Artinya Apa?", "id": "Jantung Tidak Dapat Memompa Darah Efisien." },
{ "en": "Ahli Geriatri Memberikan Perawatan Medis Kepada?", "id": "Pasien Lanjut Usia Atau Para Lansia." },
{ "en": "Apa Fungsi Kornea Pada Organ Mata?", "id": "Melindungi Mata, Membantu Memfokuskan Cahaya." },
{ "en": "Apa Nama Cairan Sendi Pelumas Tulang?", "id": "Cairan Sinovial, Mengurangi Gesekan Antartulang." },
{ "en": "Penyakit Rabun Jauh Dikenal Dengan Istilah?", "id": "Miopi, Sulit Melihat Objek Jauh." },
{ "en": "Siapa Ahli Yang Menangani Masalah Kesuburan?", "id": "Spesialis Fertilitas Atau Ahli Reproduksi." },
{ "en": "Apa Fungsi Utama Dari Organ Limpa?", "id": "Menyaring Darah, Melawan Infeksi Tertentu." },
{ "en": "Bagian Otak Terbesar Yang Mengatur Pikiran?", "id": "Otak Besar Atau Dikenal Serebrum." },
{ "en": "Penyakit Radang Selaput Otak Disebut Apa?", "id": "Meningitis, Infeksi Selaput Pelindung Otak." },
{ "en": "Dokter Spesialis Saluran Kemih Disebut Apa?", "id": "Urolog, Ahli Sistem Saluran Kemih." },
{ "en": "Apa Fungsi Dari Kantung Empedu Manusia?", "id": "Menyimpan Dan Mengentalkan Cairan Empedu." },
{ "en": "Apa Nama Pipa Penghubung Ginjal Kandung Kemih?", "id": "Ureter, Menyalurkan Urine Dari Ginjal." },
{ "en": "Istilah Medis Untuk Kebutaan Warna Adalah?", "id": "Akromatopsia, Ketidakmampuan Melihat Warna." },
{ "en": "Ahli Penyakit Rematik Dan Autoimun Adalah?", "id": "Rematolog, Spesialis Sendi, Otot, Tulang." },
{ "en": "Apa Fungsi Kelenjar Timus Bagi Imunitas?", "id": "Mematangkan Sel Limfosit T (Sel T)." },
{ "en": "Apa Bagian Putih Pada Bola Mata?", "id": "Sklera, Melindungi Struktur Dalam Mata." },
{ "en": "Penyakit Lensa Mata Keruh Disebut Apa?", "id": "Katarak, Menyebabkan Pandangan Menjadi Kabur." },
{ "en": "Siapa Ahli Yang Melakukan Bedah Plastik?", "id": "Dokter Bedah Plastik Dan Rekonstruksi." },
{ "en": "Apa Fungsi Dari Otot Rangka Manusia?", "id": "Menggerakkan Tulang Dan Bagian Tubuh." },
{ "en": "Struktur Telinga Mana Mengubah Getaran Suara?", "id": "Koklea Atau Rumah Siput Telinga." },
{ "en": "Penyakit Radang Pada Paru-Paru Disebut Apa?", "id": "Pneumonia, Infeksi Kantung Udara Paru." },
{ "en": "Dokter Spesialis Bedah Umum Disebut Apa?", "id": "Surgeon, Melakukan Prosedur Bedah Umum." },
{ "en": "Apa Fungsi Pembuluh Darah Arteri Koroner?", "id": "Memasok Darah Kaya Oksigen Ke Jantung." },
{ "en": "Apa Bagian Terluar Dari Struktur Otak?", "id": "Korteks Serebral, Pusat Fungsi Kognitif." },
{ "en": "Istilah Medis Untuk Mimisan Adalah Apa?", "id": "Epistaksis, Perdarahan Dari Dalam Hidung." },
{ "en": "Ahli Penyakit Infeksi Menular Disebut Apa?", "id": "Spesialis Penyakit Infeksi Tropik Infeksi." },
{ "en": "Apa Fungsi Dari Kelenjar Keringat?", "id": "Mengatur Suhu Tubuh Melalui Keringat." },
{ "en": "Apa Nama Medis Untuk Sel Lemak?", "id": "Adiposit, Sel Penyimpan Energi Lemak." },
{ "en": "Penyakit Degeneratif Otak Penyebab Demensia Adalah?", "id": "Alzheimer, Penurunan Fungsi Kognitif Otak." },
{ "en": "Dokter Spesialis Kaki Dan Pergelangan Kaki?", "id": "Podiatris, Ahli Kesehatan Organ Kaki." },
{ "en": "Apa Fungsi Pita Suara Di Laring?", "id": "Bergetar Untuk Menghasilkan Suara Manusia." },
{ "en": "Apa Jaringan Yang Menutupi Permukaan Sendi?", "id": "Tulang Rawan Atau Dikenal Kartilago." },
{ "en": "Penyakit Kulit Kronis Bercak Merah Bersisik?", "id": "Psoriasis, Kondisi Penyakit Autoimun Kulit." },
{ "en": "Ahli Gangguan Pendengaran Dan Keseimbangan Adalah?", "id": "Audiolog, Spesialis Dalam Bidang Audiologi." },
{ "en": "Apa Fungsi Dari Lobus Frontal Otak?", "id": "Mengontrol Gerakan, Memori, Dan Perilaku." },
{ "en": "Apa Nama Sel Yang Membentuk Tulang?", "id": "Osteoblas, Berperan Dalam Pembentukan Tulang." },
{ "en": "Istilah Medis Untuk Pusing Berputar Adalah?", "id": "Vertigo, Sensasi Diri Berputar Lingkungan." },
{ "en": "Siapa Ahli Yang Mempelajari Racun Kimia?", "id": "Toksikolog, Ilmuwan Bidang Ilmu Toksikologi." },
{ "en": "Apa Fungsi Dari Pembuluh Darah Kapiler?", "id": "Pertukaran Zat Antara Darah, Jaringan." },
{ "en": "Apa Hormon Yang Mengatur Siklus Tidur?", "id": "Melatonin, Diproduksi Oleh Kelenjar Pineal." },
{ "en": "Penyakit Kelainan Pembekuan Darah Genetik Adalah?", "id": "Hemofilia, Kekurangan Faktor Pembekuan Darah." },
{ "en": "Dokter Bedah Jantung Dan Dada Adalah?", "id": "Dokter Bedah Toraks Dan Kardiovaskular." },
{ "en": "Apa Fungsi Air Liur Di Mulut?", "id": "Membantu Menelan, Mencerna, Melawan Kuman." },
{ "en": "Apa Jenis Otot Yang Bekerja Otomatis?", "id": "Otot Polos, Ditemukan Di Organ Dalam." },
{ "en": "Penyakit Kelengkungan Tulang Belakang Samping Adalah?", "id": "Skoliosis, Kurva Tulang Belakang Abnormal." },
{ "en": "Ahli Yang Mempelajari Virus Disebut Apa?", "id": "Virolog, Seorang Ilmuwan Bidang Virologi." },
{ "en": "Apa Fungsi Saraf Optik Di Mata?", "id": "Mengirimkan Informasi Visual Ke Otak." },
{ "en": "Dimana Lokasi Produksi Sel Darah Merah?", "id": "Di Sumsum Tulang Merah Belakang." },
{ "en": "Istilah Medis Untuk Perut Kembung Adalah?", "id": "Meteorismus, Akumulasi Gas Saluran Cerna." },
{ "en": "Siapa Ahli Yang Mempelajari Bakteri Patogen?", "id": "Bakteriolog, Seorang Ahli Bidang Bakteriologi." },
{ "en": "Apa Fungsi Dari Tulang Rusuk Manusia?", "id": "Melindungi Jantung Dan Organ Paru-Paru." },
{ "en": "Apa Nama Medis Untuk Sel Kulit?", "id": "Keratinosit, Sel Utama Di Epidermis." },
{ "en": "Penyakit Malaria Ditularkan Melalui Gigitan Apa?", "id": "Nyamuk Anopheles Betina Yang Terinfeksi." },
{ "en": "Dokter Ahli Perawatan Intensif Disebut Apa?", "id": "Intensivis, Dokter Unit Perawatan Intensif (ICU)." },
{ "en": "Apa Fungsi Plasenta Selama Masa Kehamilan?", "id": "Menyalurkan Nutrisi Oksigen Dari Ibu." },
{ "en": "Apa Jaringan Yang Menghubungkan Tulang-Tulang?", "id": "Ligamen, Jaringan Ikat Fibrosa Elastis." },
{ "en": "Penyakit Infeksi Bakteri Pada Usus Halus?", "id": "Tifus Atau Demam Tifoid Salmonella Typhi." },
{ "en": "Ahli Gizi Medis Disebut Juga Apa?", "id": "Dietisien, Ahli Gizi Klinis Terdaftar." },
{ "en": "Apa Fungsi Lobus Oksipital Di Otak?", "id": "Memproses Informasi Visual Dari Mata." },
{ "en": "Apa Hormon Pemicu Stres Dalam Tubuh?", "id": "Kortisol, Dihasilkan Oleh Kelenjar Adrenal." },
{ "en": "Penyakit Tulang Lunak Pada Anak-Anak Adalah?", "id": "Rakhitis, Akibat Kekurangan Vitamin D." },
{ "en": "Dokter Yang Mengkhususkan Diri Fisiologi Olahraga?", "id": "Spesialis Kedokteran Olahraga Atau Sport Medicine." },
{ "en": "Apa Fungsi Dari Otot Jantung (Miokardium)?", "id": "Memompa Darah Secara Terus Menerus." },
{ "en": "Apa Bagian Telinga Yang Menangkap Suara?", "id": "Daun Telinga Atau Pinna Aurikula." },
{ "en": "Istilah Medis Untuk Radang Sendi Pinggul?", "id": "Artritis Panggul, Peradangan Sendi Panggul." },
{ "en": "Siapa Ahli Yang Menangani Terapi Fisik?", "id": "Fisioterapis, Profesional Rehabilitasi Gerak Tubuh." },
{ "en": "Apa Fungsi Kelenjar Paratiroid Di Leher?", "id": "Mengatur Kadar Kalsium Dalam Darah." },
{ "en": "Apa Nama Lain Selaput Pembungkus Jantung?", "id": "Perikardium, Kantung Fibrosa Pelindung Jantung." },
{ "en": "Penyakit Radang Pada Selaput Perut Adalah?", "id": "Peritonitis, Infeksi Rongga Peritoneum Abdomen." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Tidur?", "id": "Somnolog Atau Ahli Kedokteran Tidur." },
{ "en": "Apa Fungsi Tulang Belakang Atau Vertebra?", "id": "Menopang Tubuh, Melindungi Sumsum Tulang." },
{ "en": "Apa Sel Utama Sistem Kekebalan Tubuh?", "id": "Limfosit, Jenis Dari Sel Darah Putih." },
{ "en": "Penyakit Kuning Pada Bayi Baru Lahir?", "id": "Jaundice Neonatal, Akibat Bilirubin Tinggi." },
{ "en": "Ahli Forensik Medis Disebut Juga Apa?", "id": "Patolog Forensik, Dokter Pemeriksa Jenazah." },
{ "en": "Apa Fungsi Dari Amandel Atau Tonsil?", "id": "Membantu Mencegah Infeksi Masuk Tubuh." },
{ "en": "Dimana Letak Hipotalamus Dan Fungsinya Apa?", "id": "Di Otak, Mengatur Suhu, Lapar." },
{ "en": "Penyakit Infeksi Bakteri Tetanus Disebabkan Oleh?", "id": "Bakteri Clostridium Tetani Melalui Luka." },
{ "en": "Dokter Yang Mengelola Bank Darah Adalah?", "id": "Spesialis Kedokteran Transfusi Atau Transfusion Medicine." },
{ "en": "Apa Fungsi Rambut Di Kepala Manusia?", "id": "Melindungi Kulit Kepala Dari Sinar Matahari." },
{ "en": "Apa Nama Ilmiah Untuk Tulang Tengkorak?", "id": "Kranium, Struktur Tulang Kepala Manusia." },
{ "en": "Istilah Medis Untuk Keringat Berlebih Adalah?", "id": "Hiperhidrosis, Produksi Keringat Secara Berlebihan." },
{ "en": "Siapa Ahli Yang Mempelajari Penyebaran Penyakit?", "id": "Epidemiolog, Ahli Dalam Bidang Epidemiologi." },
{ "en": "Apa Fungsi Lobus Temporal Pada Otak?", "id": "Memproses Pendengaran, Memori, Dan Emosi." },
{ "en": "Apa Jenis Sendi Pada Lutut Manusia?", "id": "Sendi Engsel, Memungkinkan Gerakan Satu Arah." },
{ "en": "Penyakit Infeksi Kolera Disebabkan Oleh Bakteri?", "id": "Vibrio Cholerae, Melalui Air Terkontaminasi." },
{ "en": "Ahli Farmasi Klinis Bekerja Di Bidang?", "id": "Penggunaan Obat Yang Tepat, Aman." },
{ "en": "Apa Fungsi Pembuluh Vena Kava Superior?", "id": "Membawa Darah Kotor Tubuh Atas." },
{ "en": "Apa Cairan Bening Yang Mengisi Mata?", "id": "Aqueous Humour, Menjaga Bentuk Bola Mata." },
{ "en": "Penyakit Degenerasi Saraf Motorik Progresif Adalah?", "id": "Sklerosis Lateral Amiotrofik (ALS)." },
{ "en": "Dokter Spesialis Kesehatan Masyarakat Disebut Apa?", "id": "Spesialis Kesehatan Masyarakat Atau Public Health Specialist." },
{ "en": "Apa Fungsi Dari Kuku Jari Manusia?", "id": "Melindungi Ujung Jari Tangan, Kaki." },
{ "en": "Apa Bagian Berwarna Pada Organ Mata?", "id": "Iris, Mengatur Ukuran Organ Pupil." },
{ "en": "Istilah Medis Untuk Kondisi Bibir Sumbing?", "id": "Celah Bibir Atau Dikenal Palatoschisis." },
{ "en": "Ahli Terapi Wicara Menangani Gangguan Apa?", "id": "Gangguan Komunikasi, Bahasa, Dan Menelan." },
{ "en": "Apa Fungsi Utama Dari Kelenjar Pineal?", "id": "Menghasilkan Hormon Melatonin Pengatur Tidur." },
{ "en": "Apa Tulang Pipih Di Tengah Dada?", "id": "Tulang Dada Atau Dikenal Sternum." },
{ "en": "Penyakit Peradangan Hati Akibat Alkohol Adalah?", "id": "Hepatitis Alkoholik, Kerusakan Hati Kronis." },
{ "en": "Dokter Ahli Kedokteran Nuklir Menggunakan Apa?", "id": "Zat Radioaktif Untuk Diagnosis, Terapi." },
{ "en": "Apa Nama Pipa Udara Sebelum Paru-Paru?", "id": "Trakea Atau Batang Tenggorokan Udara." },
{ "en": "Apa Bagian Awal Dari Usus Halus?", "id": "Duodenum Atau Usus Dua Belas Jari." },
{ "en": "Pelebaran Pembuluh Darah Abnormal Disebut Apa?", "id": "Aneurisma, Berisiko Pecah Menyebabkan Pendarahan." },
{ "en": "Ahli Genetika Penyakit Keturunan Disebut Apa?", "id": "Genetika Medis Atau Seorang Geneticist." },
{ "en": "Apa Fungsi Dari Rambut Getar Silia?", "id": "Menyaring Debu Di Saluran Pernapasan." },
{ "en": "Dimana Letak Kelenjar Ludah Terbesar?", "id": "Kelenjar Parotis, Terletak Di Pipi." },
{ "en": "Penyakit Asam Lambung Naik Kerongkongan Adalah?", "id": "GERD (Gastroesophageal Reflux Disease)." },
{ "en": "Siapa Ahli Terapi Aktivitas Sehari-Hari?", "id": "Terapis Okupasi Atau Occupational Therapist." },
{ "en": "Apa Fungsi Cairan Serebrospinal Di Otak?", "id": "Melindungi Otak Sumsum Tulang Belakang." },
{ "en": "Apa Lapisan Pelindung Terluar Tulang Keras?", "id": "Periosteum, Membran Fibrosa Pelindung Tulang." },
{ "en": "Istilah Medis Untuk Penyumbatan Pembuluh Darah?", "id": "Emboli, Sumbatan Oleh Benda Asing." },
{ "en": "Ahli Yang Menangani Perawatan Paliatif Adalah?", "id": "Dokter Spesialis Perawatan Paliatif Medis." },
{ "en": "Apa Fungsi Unit Penyaring Ginjal (Nefron)?", "id": "Menyaring Darah, Membentuk Urine Primer." },
{ "en": "Apa Selaput Tipis Pembungkus Organ Paru-Paru?", "id": "Pleura, Membran Serosa Pelindung Paru." },
{ "en": "Penyakit Radang Usus Kronis Autoimun Adalah?", "id": "Penyakit Crohn, Menyerang Saluran Pencernaan." },
{ "en": "Siapa Ahli Yang Menyelaraskan Tulang Belakang?", "id": "Kiropraktor, Fokus Pada Sistem Muskuloskeletal." },
{ "en": "Apa Fungsi Dari Pembuluh Darah Aorta?", "id": "Membawa Darah Kaya Oksigen Jantung." },
{ "en": "Apa Nama Tonjolan Kecil Di Usus?", "id": "Vili, Memperluas Area Penyerapan Nutrisi." },
{ "en": "Penyakit Pertumbuhan Jaringan Rahim Diluar Rahim?", "id": "Endometriosis, Menyebabkan Nyeri Panggul Hebat." },
{ "en": "Petugas Medis Pengambil Sampel Darah Adalah?", "id": "Phlebotomist Atau Analis Kesehatan Profesional." },
{ "en": "Apa Fungsi Kelenjar Minyak (Sebaceous Gland)?", "id": "Menghasilkan Sebum Untuk Melumasi Kulit." },
{ "en": "Apa Nama Kantung Tempat Tumbuh Rambut?", "id": "Folikel Rambut, Struktur Di Kulit." },
{ "en": "Penyakit Nyeri Otot Saraf Kronis Adalah?", "id": "Fibromyalgia, Nyeri Tersebar Di Tubuh." },
{ "en": "Siapa Ahli Yang Membuat Anggota Tubuh Tiruan?", "id": "Prosthetist, Mendesain Dan Membuat Prostesis." },
{ "en": "Apa Fungsi Dari Otot Polos Involunter?", "id": "Mengontrol Organ Dalam Secara Otomatis." },
{ "en": "Apa Cairan Yang Mengisi Rongga Sendi?", "id": "Cairan Sinovial, Berfungsi Sebagai Pelumas." },
{ "en": "Penyakit Akibat Kristal Asam Urat Adalah?", "id": "Gout Atau Pirai, Radang Sendi." },
{ "en": "Ahli Yang Menganalisis Sel Tubuh Manusia?", "id": "Sitolog Atau Cytologist Bidang Sitologi." },
{ "en": "Apa Fungsi Utama Dari Sumsum Tulang?", "id": "Memproduksi Sel-Sel Darah Merah Putih." },
{ "en": "Apa Percabangan Dari Trakea Menuju Paru-Paru?", "id": "Bronkus, Saluran Udara Utama Paru." },
{ "en": "Penyakit Autoimun Yang Menyerang Banyak Organ?", "id": "Lupus Eritematosus Sistemik (SLE)." },
{ "en": "Siapa Ahli Kesehatan Reproduksi Pria Spesifik?", "id": "Androlog, Spesialis Sistem Reproduksi Pria." },
{ "en": "Apa Hormon Yang Dilepaskan Saat Senang?", "id": "Endorfin, Dopamin, Serotonin, Dan Oksitosin." },
{ "en": "Apa Bagian Tengah Dari Usus Halus?", "id": "Jejunum, Tempat Penyerapan Gula, Asam Amino." },
{ "en": "Penyakit Infeksi Ulang Virus Cacar Air?", "id": "Herpes Zoster Atau Penyakit Cacar Ular." },
{ "en": "Petugas Medis Gawat Darurat Pra-Rumah Sakit?", "id": "Paramedis, Memberikan Pertolongan Pertama Lanjutan." },
{ "en": "Apa Fungsi Dari Pembuluh Arteri Pulmonalis?", "id": "Membawa Darah Kotor Ke Paru-Paru." },
{ "en": "Struktur Apa Yang Memisahkan Rongga Dada-Perut?", "id": "Diafragma, Otot Pernapasan Utama Manusia." },
{ "en": "Istilah Medis Untuk Infeksi Jamur Kulit?", "id": "Tinea Atau Kurap, Infeksi Dermatofita." },
{ "en": "Ahli Yang Membuat Alat Bantu Ortopedi?", "id": "Orthotist, Mendesain Dan Membuat Ortotik." },
{ "en": "Apa Fungsi Dari Kelenjar Keringat Apokrin?", "id": "Menghasilkan Keringat Berbau Di Ketiak." },
{ "en": "Apa Nama Sel Yang Menghancurkan Tulang?", "id": "Osteoklas, Berperan Dalam Resorpsi Tulang." },
{ "en": "Penyakit Kulit Akibat Tungau Gatal Adalah?", "id": "Skabies Atau Kudis, Sangat Menular." },
{ "en": "Ahli Yang Mempelajari Gerak Tubuh Manusia?", "id": "Kinesiolog, Spesialis Dalam Bidang Kinesiologi." },
{ "en": "Apa Fungsi Dari Lobus Parietal Otak?", "id": "Memproses Informasi Sensorik Seperti Sentuhan." },
{ "en": "Apa Bagian Akhir Dari Usus Halus?", "id": "Ileum, Menyerap Vitamin B12, Garam Empedu." },
{ "en": "Istilah Medis Untuk Kebotakan Rambut Adalah?", "id": "Alopecia, Kerontokan Rambut Berlebihan Abnormal." },
{ "en": "Siapa Teknisi Laboratorium Medis Di Rumah Sakit?", "id": "Analis Laboratorium Medis Atau Medical Technologist." },
{ "en": "Apa Fungsi Dari Otot Lurik Atau Skeletal?", "id": "Menggerakkan Rangka Tubuh Secara Sadar." },
{ "en": "Apa Bagian Bola Mata Peka Cahaya?", "id": "Retina, Mengandung Sel Fotoreseptor Mata." },
{ "en": "Penyakit Cedera Otak Traumatis Ringan Disebut?", "id": "Konkusi Atau Gegar Otak Ringan." },
{ "en": "Ahli Kesehatan Lingkungan Mempelajari Apa Saja?", "id": "Faktor Lingkungan Yang Mempengaruhi Kesehatan." },
{ "en": "Apa Fungsi Dari Katup Jantung Manusia?", "id": "Menjaga Aliran Darah Satu Arah." },
{ "en": "Apa Nama Lubang Di Tengah Iris?", "id": "Pupil, Mengatur Jumlah Cahaya Masuk." },
{ "en": "Istilah Medis Saraf Terjepit Di Pinggang?", "id": "Skiatika, Nyeri Menjalar Dari Pinggang." },
{ "en": "Dokter Spesialis Pengobatan Ketergantungan Disebut Apa?", "id": "Adiktolog Atau Spesialis Kedokteran Adiksi." },
{ "en": "Apa Fungsi Dari Kerongkongan Atau Esofagus?", "id": "Menyalurkan Makanan Dari Mulut Lambung." },
{ "en": "Apa Sendi Yang Menghubungkan Lengan Atas-Bawah?", "id": "Sendi Engsel Di Bagian Siku." },
{ "en": "Penyakit Hormonal Pada Wanita Dengan Kista Ovarium?", "id": "Sindrom Ovarium Polikistik Atau PCOS." },
{ "en": "Siapa Ahli Yang Menangani Terapi Rekreasi?", "id": "Terapis Rekreasi, Menggunakan Aktivitas Rekreasi." },
{ "en": "Apa Fungsi Kelenjar Prostat Pada Pria?", "id": "Menghasilkan Cairan Untuk Melindungi Sperma." },
{ "en": "Apa Nama Lain Selaput Pembungkus Otak?", "id": "Meninges, Terdiri Dari Tiga Lapisan." },
{ "en": "Penyakit Radang Pada Kantong Dinding Usus?", "id": "Divertikulitis, Infeksi Pada Divertikula Usus." },
{ "en": "Ahli Farmakologi Mempelajari Interaksi Apa Saja?", "id": "Interaksi Obat Dengan Sistem Biologis." },
{ "en": "Apa Fungsi Dari Pembuluh Balik Atau Vena?", "id": "Membawa Darah Kembali Menuju Ke Jantung." },
{ "en": "Apa Bagian Gigi Yang Terlihat Putih?", "id": "Mahkota Gigi, Dilapisi Oleh Email." },
{ "en": "Istilah Medis Untuk Pengerasan Pembuluh Arteri?", "id": "Aterosklerosis, Akibat Plak Kolesterol Lemak." },
{ "en": "Ahli Akupunktur Menggunakan Apa Untuk Terapi?", "id": "Jarum Tipis Pada Titik-Titik Tubuh." },
{ "en": "Apa Fungsi Reseptor Sensorik Di Kulit?", "id": "Mendeteksi Sentuhan, Tekanan, Suhu, Nyeri." },
{ "en": "Apa Tulang Yang Membentuk Tempurung Lutut?", "id": "Patela, Melindungi Sendi Lutut Manusia." },
{ "en": "Penyakit Radang Pada Bursa Sendi Adalah?", "id": "Bursitis, Peradangan Kantung Cairan Sendi." },
{ "en": "Dokter Spesialis Kedokteran Penerbangan Memeriksa Siapa?", "id": "Pilot, Pramugari, Dan Awak Pesawat." },
{ "en": "Apa Fungsi Air Ketuban Selama Kehamilan?", "id": "Melindungi Janin Dari Guncangan Cedera." },
{ "en": "Apa Bagian Terkeras Dari Struktur Tulang?", "id": "Tulang Kompak, Padat Dan Keras." },
{ "en": "Istilah Medis Untuk Gangguan Irama Jantung?", "id": "Aritmia, Detak Jantung Tidak Teratur." },
{ "en": "Siapa Ahli Yang Mempelajari Penuaan Manusia?", "id": "Gerontolog, Mempelajari Aspek Sosial Psikologis." },
{ "en": "Apa Fungsi Usus Buntu Sebenarnya Di Tubuh?", "id": "Berperan Dalam Sistem Imun Tubuh." },
{ "en": "Apa Cabang Terkecil Dari Saluran Bronkus?", "id": "Bronkiolus, Menyalurkan Udara Ke Alveoli." },
{ "en": "Penyakit Kulit Hilangnya Pigmen Melanin Adalah?", "id": "Vitiligo, Menyebabkan Bercak Putih Kulit." },
{ "en": "Ahli Pengobatan Herbal Disebut Juga Dengan?", "id": "Herbalis, Menggunakan Tanaman Untuk Pengobatan." },
{ "en": "Apa Fungsi Dari Sel Kerucut Retina?", "id": "Mendeteksi Warna Dan Detail Visual." },
{ "en": "Apa Nama Medis Untuk Sel Darah?", "id": "Hemocyte, Istilah Umum Sel Darah." },
{ "en": "Istilah Untuk Radang Jaringan Ikat Tendon?", "id": "Tendinitis, Peradangan Atau Iritasi Tendon." },
{ "en": "Dokter Hewan Disebut Juga Dengan Istilah?", "id": "Veterinarian, Merawat Kesehatan Hewan Peliharaan." },
{ "en": "Apa Fungsi Gerak Peristaltik Di Esofagus?", "id": "Mendorong Makanan Menuju Organ Lambung." },
{ "en": "Apa Tulang Yang Membentuk Dinding Panggul?", "id": "Tulang Panggul Atau Pelvis Manusia." },
{ "en": "Penyakit Saraf Terjepit Di Pergelangan Tangan?", "id": "Sindrom Lorong Karpal Atau Carpal Tunnel." },
{ "en": "Ahli Gizi Holistik Fokus Pada Apa?", "id": "Kesehatan Menyeluruh, Mencakup Pikiran, Tubuh." },
{ "en": "Apa Fungsi Dari Hormon Testosteron Pria?", "id": "Mengatur Perkembangan Karakteristik Seksual Pria." },
{ "en": "Apa Nama Medis Untuk Tahi Lalat?", "id": "Nevus, Pertumbuhan Sel Pigmen Kulit." },
{ "en": "Istilah Medis Untuk Penyakit Cacingan Adalah?", "id": "Helminthiasis, Infeksi Oleh Cacing Parasit." },
{ "en": "Siapa Ahli Yang Melakukan Pijat Terapeutik?", "id": "Terapis Pijat Atau Massage Therapist." },
{ "en": "Apa Pembangkit Energi Utama Dalam Sel?", "id": "Mitokondria, Tempat Terjadinya Respirasi Seluler." },
{ "en": "Apa Jaringan Yang Melapisi Permukaan Tubuh?", "id": "Jaringan Epitel, Sebagai Pelindung Utama." },
{ "en": "Istilah Medis Untuk Sesak Napas Adalah?", "id": "Dispnea, Kesulitan Atau Ketidaknyamanan Bernapas." },
{ "en": "Ahli Penyakit Hati Secara Khusus Adalah?", "id": "Hepatolog, Subspesialis Dari Bidang Gastroenterologi." },
{ "en": "Apa Fungsi Dari Ribosom Di Sitoplasma?", "id": "Tempat Sintesis Atau Produksi Protein." },
{ "en": "Apa Komponen Cairan Utama Dalam Darah?", "id": "Plasma Darah, Berwarna Kuning Bening." },
{ "en": "Penyakit Akibat Kekurangan Vitamin C Adalah?", "id": "Skorbut Atau Scurvy, Menyebabkan Pendarahan Gusi." },
{ "en": "Siapa Teknisi Pengoperasi Mesin Sinar-X?", "id": "Radiografer Atau Teknisi Radiologi Profesional." },
{ "en": "Apa Celah Antara Dua Sel Saraf?", "id": "Sinapsis, Tempat Transmisi Impuls Saraf." },
{ "en": "Apa Selubung Lemak Pelindung Akson Saraf?", "id": "Selubung Mielin, Mempercepat Hantaran Impuls." },
{ "en": "Istilah Medis Untuk Denyut Jantung Cepat?", "id": "Takikardia, Detak Jantung Di Atas Normal." },
{ "en": "Ahli Imunologi Klinis Menangani Gangguan Apa?", "id": "Gangguan Pada Sistem Kekebalan Tubuh." },
{ "en": "Apa Fungsi Hormon Glukagon Dari Pankreas?", "id": "Meningkatkan Kadar Gula Darah Rendah." },
{ "en": "Apa Enzim Pemecah Amilum Di Mulut?", "id": "Enzim Ptialin Atau Amilase Saliva." },
{ "en": "Penyakit Genetik Kelainan Kromosom 21 Adalah?", "id": "Sindrom Down Atau Trisomi 21." },
{ "en": "Siapa Asisten Dokter Di Ruang Operasi?", "id": "Teknolog Bedah Atau Surgical Technologist." },
{ "en": "Apa Pusat Kontrol Aktivitas Seluler Tubuh?", "id": "Nukleus Atau Inti Sel Manusia." },
{ "en": "Apa Jaringan Yang Mengikat Mendukung Organ?", "id": "Jaringan Ikat, Memberikan Dukungan Struktural." },
{ "en": "Istilah Medis Untuk Pembengkakan Akibat Cairan?", "id": "Edema, Penumpukan Cairan Di Jaringan." },
{ "en": "Ahli Yang Membersihkan Karang Gigi Adalah?", "id": "Higienis Gigi Atau Dental Hygienist Profesional." },
{ "en": "Apa Zat Kimia Pembawa Pesan Antarsaraf?", "id": "Neurotransmitter, Contohnya Asetilkolin, Dopamin, Serotonin." },
{ "en": "Apa Protein Dalam Darah Pengikat Oksigen?", "id": "Hemoglobin, Memberi Warna Merah Darah." },
{ "en": "Penyakit Akibat Kekurangan Vitamin B1 Adalah?", "id": "Penyakit Beri-Beri, Menyerang Sistem Saraf." },
{ "en": "Praktisi Pengobatan Alternatif Alami Disebut Apa?", "id": "Naturopat, Menggunakan Metode Penyembuhan Alami." },
{ "en": "Apa Fungsi Hormon Estrogen Pada Wanita?", "id": "Mengatur Siklus Menstruasi, Karakteristik Seksual." },
{ "en": "Apa Enzim Di Lambung Pemecah Protein?", "id": "Pepsin, Bekerja Dalam Suasana Asam." },
{ "en": "Istilah Medis Untuk Kulit Berwarna Kebiruan?", "id": "Sianosis, Akibat Kekurangan Oksigen Darah." },
{ "en": "Siapa Koordinator Dalam Uji Coba Klinis?", "id": "Koordinator Penelitian Klinis Atau Clinical Research." },
{ "en": "Apa Tahap Awal Pembelahan Sel Mitosis?", "id": "Profase, Kromosom Mulai Memadat Jelas." },
{ "en": "Apa Lapisan Gigi Di Bawah Email?", "id": "Dentin, Jaringan Keras Kuning Pucat." },
{ "en": "Penyakit Radang Sendi Autoimun Kronis Adalah?", "id": "Artritis Reumatoid, Menyerang Lapisan Sendi." },
{ "en": "Ahli Pengobatan Dengan Dosis Sangat Kecil?", "id": "Homeopat, Praktisi Dalam Bidang Homeopati." },
{ "en": "Apa Fungsi Hormon Tiroksin Kelenjar Tiroid?", "id": "Mengatur Metabolisme Energi Seluruh Tubuh." },
{ "en": "Apa Jenis Gigi Untuk Memotong Makanan?", "id": "Gigi Seri Atau Dikenal Insisivus." },
{ "en": "Istilah Prosedur Pengambilan Sampel Jaringan Adalah?", "id": "Biopsi, Untuk Pemeriksaan Mikroskopis Patologi." },
{ "en": "Dokter Spesialis Yang Melakukan Proses Otopsi?", "id": "Patolog Anatomis, Menentukan Penyebab Kematian." },
{ "en": "Apa Fungsi Dari Sitoplasma Di Sel?", "id": "Tempat Organel Sel, Reaksi Metabolik." },
{ "en": "Apa Bagian Dalam Gigi Berisi Saraf?", "id": "Pulpa, Jaringan Lunak Di Tengah." },
{ "en": "Penyakit Rabun Senja Akibat Kekurangan Vitamin?", "id": "Vitamin A, Kesulitan Melihat Cahaya Redup." },
{ "en": "Personel Yang Membantu Perawat Pasien Adalah?", "id": "Asisten Perawat Bersertifikat Atau Nursing Assistant." },
{ "en": "Apa Hormon Yang Menyiapkan Rahim Untuk Kehamilan?", "id": "Progesteron, Hormon Reproduksi Wanita Utama." },
{ "en": "Apa Enzim Yang Mencerna Lemak (Lipid)?", "id": "Lipase, Dihasilkan Oleh Organ Pankreas." },
{ "en": "Istilah Medis Untuk Denyut Jantung Lambat?", "id": "Bradikardia, Detak Jantung Di Bawah Normal." },
{ "en": "Ahli Terapi Pernapasan Menangani Penyakit Apa?", "id": "Penyakit Paru-Paru Dan Gangguan Pernapasan." },
{ "en": "Apa Jenis Jaringan Utama Penyusun Otak?", "id": "Jaringan Saraf, Terdiri Dari Neuron." },
{ "en": "Apa Nama Jenis Gigi Geraham Belakang?", "id": "Molar, Untuk Menggiling Dan Menghancurkan Makanan." },
{ "en": "Penyakit Genetik Lendir Kental Di Paru?", "id": "Fibrosis Kistik Atau Cystic Fibrosis (CF)." },
{ "en": "Siapa Yang Merancang Program Diet Pasien?", "id": "Ahli Diet Atau Dietisien Terdaftar." },
{ "en": "Apa Fungsi Hormon Kalsitonin Dari Tiroid?", "id": "Menurunkan Kadar Kalsium Dalam Darah." },
{ "en": "Apa Tahap Pembelahan Kromosom Sel Mitosis?", "id": "Anafase, Kromatid Bergerak Ke Kutub." },
{ "en": "Prosedur Melihat Dalam Organ Dengan Teropong?", "id": "Endoskopi, Menggunakan Alat Bernama Endoskop." },
{ "en": "Dokter Spesialis Yang Menangani Vena Varises?", "id": "Flebolog, Ahli Dalam Penyakit Vena." },
{ "en": "Apa Materi Genetik Dalam Inti Sel?", "id": "Asam Deoksiribonukleat Atau DNA (Deoxyribonucleic Acid)." },
{ "en": "Apa Nama Otot Yang Mengelilingi Jantung?", "id": "Miokardium, Otot Jantung Yang Bekerja." },
{ "en": "Istilah Medis Untuk Gangguan Suasana Hati?", "id": "Depresi, Perasaan Sedih Terus Menerus." },
{ "en": "Pemberi Saran Genetik Kepada Keluarga Adalah?", "id": "Konselor Genetik, Ahli Informasi Genetik." },
{ "en": "Apa Hormon Yang Melawan Reaksi Alergi?", "id": "Kortisol, Dihasilkan Oleh Kelenjar Adrenal." },
{ "en": "Apa Pembelahan Sel Yang Menghasilkan Gamet?", "id": "Meiosis, Mengurangi Jumlah Kromosom Setengah." },
{ "en": "Penyakit Darah Sulit Membeku Secara Genetik?", "id": "Hemofilia, Kekurangan Faktor Pembekuan Darah." },
{ "en": "Petugas Yang Menjalankan Mesin Dialisis Adalah?", "id": "Teknisi Dialisis Atau Teknisi Hemodialisis." },
{ "en": "Apa Fungsi Membran Sel Di Sekeliling?", "id": "Mengatur Keluar Masuknya Zat Ke Sel." },
{ "en": "Apa Jenis Gigi Runcing Di Samping?", "id": "Gigi Taring Atau Kaninus Manusia." },
{ "en": "Istilah Untuk Proses Cuci Darah Medis?", "id": "Dialisis, Menggantikan Fungsi Organ Ginjal." },
{ "en": "Ahli Anatomi Mempelajari Tentang Struktur Apa?", "id": "Struktur Tubuh Dan Hubungan Antarbagian." },
{ "en": "Apa Hormon Yang Meningkatkan Detak Jantung?", "id": "Adrenalin Atau Epinefrin Dari Adrenal." },
{ "en": "Apa Tahap Akhir Pembelahan Sel Mitosis?", "id": "Telofase, Dua Sel Anak Terbentuk." },
{ "en": "Penyakit Kekurangan Hemoglobin Dalam Darah Adalah?", "id": "Anemia Defisiensi Besi, Paling Umum." },
{ "en": "Siapa Yang Membantu Ahli Patologi Laboratorium?", "id": "Asisten Patolog Atau Pathologists' Assistant." },
{ "en": "Apa Struktur Seperti Jeli Pengisi Sel?", "id": "Sitoplasma, Tempat Reaksi Kimiawi Sel." },
{ "en": "Apa Sendi Yang Tidak Bisa Bergerak?", "id": "Sinartrosis, Contohnya Sutura Di Tengkorak." },
{ "en": "Prosedur Membuka Sumbatan Arteri Jantung Adalah?", "id": "Angioplasti, Menggunakan Balon Dan Stent." },
{ "en": "Spesialis Pengobatan Nyeri Kronis Disebut Apa?", "id": "Algolog Atau Spesialis Manajemen Nyeri." },
{ "en": "Apa Fungsi Lisosom Dalam Sel Hewan?", "id": "Mencerna Limbah Dan Benda Asing." },
{ "en": "Apa Hormon Yang Merangsang Produksi ASI?", "id": "Prolaktin, Dihasilkan Oleh Kelenjar Pituitari." },
{ "en": "Gangguan Mental Dengan Perubahan Mood Ekstrem?", "id": "Gangguan Bipolar, Episode Mania Depresi." },
{ "en": "Petugas Yang Menyiapkan Obat Resep Dokter?", "id": "Apoteker Atau Farmasis Di Apotek." },
{ "en": "Apa Itu Proses Katabolisme Dalam Tubuh?", "id": "Pemecahan Molekul Kompleks Menjadi Sederhana." },
{ "en": "Apa Jaringan Yang Menghasilkan Sel Darah?", "id": "Jaringan Hematopoietik Di Sumsum Tulang." },
{ "en": "Istilah Medis Untuk Mati Rasa Kesemutan?", "id": "Parestesia, Sensasi Abnormal Tanpa Rangsangan." },
{ "en": "Ahli Fisiologi Mempelajari Tentang Fungsi Apa?", "id": "Fungsi Normal Organ Dan Sistem." },
{ "en": "Apa Hormon Pertumbuhan Manusia Atau HGH Singkatan (Human Growth Hormone)?", "id": "Somatotropin, Merangsang Pertumbuhan Sel Tubuh." },
{ "en": "Apa Itu Proses Anabolisme Dalam Tubuh?", "id": "Penyusunan Molekul Sederhana Menjadi Kompleks." },
{ "en": "Penyakit Akibat Kekurangan Vitamin B3 Adalah?", "id": "Pellagra, Menyebabkan Dermatitis, Diare, Demensia." },
{ "en": "Ahli Yang Merawat Lansia Di Komunitas?", "id": "Perawat Geriatri, Spesialis Kesehatan Lansia." },
{ "en": "Apa Fungsi Badan Golgi Dalam Sel?", "id": "Memodifikasi, Menyortir, Mengepak Protein, Lipid." },
{ "en": "Apa Nama Tulang Di Lengan Bawah?", "id": "Radius Dan Tulang Ulna Manusia." },
{ "en": "Istilah Untuk Pemeriksaan Mayat Secara Medis?", "id": "Otopsi Atau Nekropsi, Menentukan Kematian." },
{ "en": "Siapa Yang Memberi Edukasi Pasien Diabetes?", "id": "Edukator Diabetes, Profesional Kesehatan Terlatih." },
{ "en": "Apa Fungsi Hormon Antidiuretik Atau ADH Singkatan (Antidiuretic Hormone)?", "id": "Mengatur Keseimbangan Air Dalam Tubuh." },
{ "en": "Apa Bagian Terkecil Dari Jaringan Otot?", "id": "Sarkomer, Unit Fungsional Dasar Otot." },
{ "en": "Penyakit Autoimun Yang Menyerang Selubung Mielin?", "id": "Sklerosis Ganda Atau Multiple Sclerosis (MS)." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Alergi?", "id": "Alergolog, Ahli Alergi Dan Imunologi." },
{ "en": "Apa Nama Sel Yang Menghasilkan Lendir?", "id": "Sel Goblet, Melapisi Saluran Pernapasan." },
{ "en": "Apa Tiga Tulang Pendengaran Di Telinga?", "id": "Maleus, Inkus, Dan Stapes (Martil-Landasan-Sanggurdi)." },
{ "en": "Istilah Medis Untuk Kematian Jaringan Adalah?", "id": "Nekrosis, Akibat Cedera Atau Penyakit." },
{ "en": "Siapa Pengoperasi Mesin Jantung-Paru Saat Bedah?", "id": "Perfusionis, Teknisi Kardiovaskular Profesional Terlatih." },
{ "en": "Apa Fungsi Dari Vitamin K Bagi Darah?", "id": "Membantu Dalam Proses Pembekuan Darah." },
{ "en": "Apa Sel Imun Yang Melawan Parasit?", "id": "Eosinofil, Sejenis Sel Darah Putih." },
{ "en": "Penyakit Kanker Kulit Paling Ganas Adalah?", "id": "Melanoma, Berasal Dari Sel Pigmen Melanosit." },
{ "en": "Ahli Yang Membuat Ilustrasi Medis Adalah?", "id": "Ilustrator Medis, Menggabungkan Seni, Sains." },
{ "en": "Apa Fungsi Cairan Pleura Di Paru-Paru?", "id": "Sebagai Pelumas Untuk Mencegah Gesekan." },
{ "en": "Apa Tonjolan Kecil Di Permukaan Lidah?", "id": "Papila, Mengandung Kuncup Pengecap Rasa." },
{ "en": "Istilah Medis Penyusutan Ukuran Sel Adalah?", "id": "Atrofi, Akibat Tidak Digunakan, Penyakit." },
{ "en": "Siapa Yang Mengelola Data Rekam Medis?", "id": "Manajer Informasi Kesehatan Atau Health Information Manager." },
{ "en": "Apa Fungsi Dari Ligamen Dalam Sendi?", "id": "Menghubungkan Tulang Dengan Tulang Lainnya." },
{ "en": "Apa Organ Yang Mengontrol Rasa Haus?", "id": "Hipotalamus, Pusat Pengatur Keseimbangan Cairan." },
{ "en": "Penyakit Infeksi Bakteri Pada Tenggorokan Adalah?", "id": "Difteri, Membentuk Lapisan Abu-Abu Tebal." },
{ "en": "Petugas Yang Mengawetkan Jenazah Disebut Apa?", "id": "Mortician Atau Embalmer Di Rumah Duka." },
{ "en": "Apa Fungsi Dari Sel Mast Imunitas?", "id": "Melepaskan Histamin Saat Reaksi Alergi." },
{ "en": "Apa Tali Penghubung Janin Dengan Plasenta?", "id": "Tali Pusat Atau Umbilical Cord." },
{ "en": "Istilah Untuk Pembesaran Ukuran Sel Adalah?", "id": "Hipertrofi, Peningkatan Ukuran Sel Organ." },
{ "en": "Siapa Yang Memberi Kode Diagnosis Medis?", "id": "Medical Coder, Menerjemahkan Diagnosis Ke Kode." },
{ "en": "Apa Enzim Yang Memecah Laktosa Susu?", "id": "Laktase, Dihasilkan Di Usus Halus." },
{ "en": "Apa Sel Fagosit Besar Pemakan Kuman?", "id": "Makrofag, Bagian Dari Sistem Kekebalan." },
{ "en": "Penyakit Kanker Pada Sistem Getah Bening?", "id": "Limfoma, Menyerang Sel Limfosit Imun." },
{ "en": "Ahli Yang Merawat Kuku Kaki Adalah?", "id": "Chiropodist Atau Podiatris Spesialis Kaki." },
{ "en": "Apa Fungsi Sekresi Hormon Oksitosin?", "id": "Merangsang Kontraksi Rahim, Produksi ASI." },
{ "en": "Apa Tingkat Keasaman Normal Darah Manusia?", "id": "pH Sekitar 7.35 Hingga 7.45." },
{ "en": "Istilah Untuk Kematian Sel Terprogram Adalah?", "id": "Apoptosis, Penting Untuk Perkembangan Normal." },
{ "en": "Penyedia Layanan Kesehatan Primer Disebut Apa?", "id": "Dokter Keluarga Atau General Practitioner (GP)." },
{ "en": "Apa Jenis Tulang Rawan Paling Umum?", "id": "Tulang Rawan Hialin, Terdapat Sendi." },
{ "en": "Apa Organ Yang Mengatur Keseimbangan Elektrolit?", "id": "Ginjal, Menjaga Keseimbangan Garam, Mineral." },
{ "en": "Penyakit Virus Yang Menyerang Saraf Tulang Belakang?", "id": "Poliomielitis Atau Polio, Menyebabkan Kelumpuhan." },
{ "en": "Siapa Penasihat Etika Di Rumah Sakit?", "id": "Etikawan Klinis Atau Clinical Ethicist." },
{ "en": "Apa Fungsi Dari Sel Batang Retina?", "id": "Mendeteksi Cahaya Redup, Penglihatan Malam." },
{ "en": "Apa Sel Hasil Fertilisasi Pertama Kali?", "id": "Zigot, Hasil Peleburan Sperma, Ovum." },
{ "en": "Istilah Untuk Kekurangan Aliran Darah Jaringan?", "id": "Iskemia, Menyebabkan Kekurangan Oksigen Nutrisi." },
{ "en": "Ahli Yang Mengukur Fungsi Paru-Paru Adalah?", "id": "Teknisi Fungsi Paru Atau Pulmonary Function Technologist." },
{ "en": "Apa Fungsi Sistem Saraf Otonom Simpatik?", "id": "Memicu Respons 'Lawan Atau Lari'." },
{ "en": "Apa Nama Kantung Yang Berisi Janin?", "id": "Kantung Amnion, Berisi Cairan Ketuban." },
{ "en": "Penyakit Virus Menyerang Sistem Kekebalan Tubuh?", "id": "HIV Singkatan (Human Immunodeficiency Virus)." },
{ "en": "Siapa Yang Menjalankan Tes Stres Jantung?", "id": "Teknolog Kardiovaskular Atau Fisiolog Latihan." },
{ "en": "Apa Zat Yang Memberi Warna Kulit?", "id": "Melanin, Diproduksi Oleh Sel Melanosit." },
{ "en": "Apa Organ Sensorik Untuk Bau Manusia?", "id": "Epitel Olfaktori Di Rongga Hidung." },
{ "en": "Istilah Untuk Serangan Jantung Kematian Otot?", "id": "Infark Miokard, Akibat Iskemia Jantung." },
{ "en": "Ahli Yang Mempelajari Penyakit Pada Hewan?", "id": "Patolog Veteriner, Mendiagnosis Penyakit Hewan." },
{ "en": "Apa Fungsi Sistem Saraf Parasimpatik?", "id": "Memicu Respons 'Istirahat Dan Cerna'." },
{ "en": "Apa Bagian Otak Mengatur Suhu Tubuh?", "id": "Hipotalamus, Bertindak Seperti Termostat Internal." },
{ "en": "Penyakit Kanker Berasal Dari Jaringan Epitel?", "id": "Karsinoma, Jenis Kanker Paling Umum." },
{ "en": "Siapa Yang Mengoperasikan Mesin EEG Singkatan (Electroencephalography)?", "id": "Teknolog Neurofisiologi Atau Teknisi EEG." },
{ "en": "Apa Fungsi Vitamin B12 Untuk Tubuh?", "id": "Pembentukan Sel Darah Merah, Fungsi Saraf." },
{ "en": "Apa Jenis Rasa Kelima Selain Manis Asin?", "id": "Umami, Rasa Gurih Seperti Kaldu." },
{ "en": "Istilah Obat Untuk Menurunkan Demam Adalah?", "id": "Antipiretik, Contohnya Seperti Obat Parasetamol." },
{ "en": "Ahli Yang Membantu Pasien Pasca Stroke?", "id": "Terapis Rehabilitasi Stroke Terpadu Profesional." },
{ "en": "Apa Saluran Limfatik Terbesar Dalam Tubuh?", "id": "Duktus Torasikus Atau Thoracic Duct." },
{ "en": "Apa Bagian Mata Yang Mengandung Fotoreseptor?", "id": "Retina, Lapisan Peka Cahaya Mata." },
{ "en": "Penyakit Kanker Jaringan Ikat Tulang Otot?", "id": "Sarkoma, Jenis Kanker Yang Langka." },
{ "en": "Petugas Yang Memberikan Imunisasi Disebut Apa?", "id": "Petugas Imunisasi Atau Perawat Vaksinasi." },
{ "en": "Apa Fungsi Mineral Seng (Zinc) Tubuh?", "id": "Mendukung Sistem Imun, Penyembuhan Luka." },
{ "en": "Apa Sel Yang Terlibat Pembekuan Darah?", "id": "Trombosit Atau Keping Sel Darah." },
{ "en": "Istilah Obat Untuk Mengobati Infeksi Bakteri?", "id": "Antibiotik, Menghambat Pertumbuhan Bakteri Patogen." },
{ "en": "Ahli Yang Menangani Korban Luka Bakar?", "id": "Spesialis Luka Bakar Atau Burn Specialist." },
{ "en": "Apa Fungsi Mukus Di Dinding Lambung?", "id": "Melindungi Dinding Lambung Dari Asam." },
{ "en": "Apa Tahap Perkembangan Setelah Zigot Manusia?", "id": "Embrio, Tahap Awal Perkembangan Janin." },
{ "en": "Penyakit Menular Seksual Oleh Bakteri Spiroseta?", "id": "Sifilis, Disebabkan Bakteri Treponema Pallidum." },
{ "en": "Siapa Yang Melakukan Terapi Seni Visual?", "id": "Terapis Seni Atau Art Therapist." },
{ "en": "Apa Fungsi Mineral Magnesium Bagi Tubuh?", "id": "Mendukung Fungsi Otot, Saraf, Energi." },
{ "en": "Apa Jaringan Yang Menjadi Bantalan Sendi?", "id": "Kartilago Artikular, Melapisi Ujung Tulang." },
{ "en": "Istilah Obat Yang Mendorong Produksi Urine?", "id": "Diuretik, Membantu Mengurangi Cairan Tubuh." },
{ "en": "Ahli Yang Menangani Masalah Kesehatan Kerja?", "id": "Dokter Spesialis Kedokteran Okupasi (SpKO)." },
{ "en": "Apa Fungsi Utama Dari Kelenjar Limfa?", "id": "Menyaring Patogen, Menampung Sel Imun." },
{ "en": "Apa Tahap Perkembangan Janin Setelah Embrio?", "id": "Fetus, Dari Minggu Kesembilan Kehamilan." },
{ "en": "Penyakit Peradangan Pada Hati Kronis Adalah?", "id": "Sirosis, Pembentukan Jaringan Parut Hati." },
{ "en": "Pekerja Sosial Medis Membantu Pasien Dengan?", "id": "Masalah Psikososial, Emosional, Dan Finansial." },
{ "en": "Apa Hormon Yang Mengatur Tekanan Darah?", "id": "Renin Dan Angiotensin, Dari Ginjal." },
{ "en": "Apa Itu Reseptor Nyeri Pada Kulit?", "id": "Nosiseptor, Mendeteksi Rangsangan Berbahaya Potensial." },
{ "en": "Istilah Obat Pencegah Pembekuan Darah Adalah?", "id": "Antikoagulan, Atau Dikenal Pengencer Darah." },
{ "en": "Ahli Yang Menangani Terapi Musik Adalah?", "id": "Terapis Musik Atau Music Therapist." },
{ "en": "Apa Fungsi Sistem Limfatik Secara Keseluruhan?", "id": "Keseimbangan Cairan, Pertahanan Tubuh Imun." },
{ "en": "Apa Bagian Otak Yang Mengontrol Pernapasan?", "id": "Medula Oblongata Di Batang Otak." },
{ "en": "Penyakit Infeksi Menular Seksual HPV Singkatan (Human Papillomavirus)?", "id": "Kutil Kelamin, Dapat Menyebabkan Kanker." },
{ "en": "Petugas Yang Merespons Panggilan Medis Darurat?", "id": "Teknisi Medis Darurat Atau Emergency Medical Technician." },
{ "en": "Apa Fungsi Lapisan Endometrium Di Rahim?", "id": "Tempat Implantasi Sel Telur Dibuahi." },
{ "en": "Apa Nama Medis Untuk Memar Darah?", "id": "Hematoma, Kumpulan Darah Di Luar Pembuluh." },
{ "en": "Istilah Untuk Penyakit Yang Didapat RS?", "id": "Infeksi Nosokomial Atau Hospital-Acquired Infection (HAI)." },
{ "en": "Siapa Yang Memberikan Perawatan Kaki Diabetes?", "id": "Podiatris Dan Perawat Luka Diabetes." },
{ "en": "Apa Fungsi Sel Darah Putih Basofil?", "id": "Melepaskan Histamin Dalam Respons Peradangan." },
{ "en": "Apa Bagian Otak Koordinasi Gerakan Halus?", "id": "Serebelum Atau Otak Kecil Manusia." },
{ "en": "Penyakit Akibat Racun Bakteri Clostridium Tetani?", "id": "Tetanus, Menyebabkan Kejang Otot Kaku." },
{ "en": "Ahli Gizi Yang Bekerja Di Masyarakat?", "id": "Ahli Gizi Masyarakat Atau Public Health Nutritionist." },
{ "en": "Apa Alat Pacu Jantung Alami Manusia?", "id": "Nodus Sinoatrial Atau Sinoatrial (SA) Node." },
{ "en": "Apa Lapisan Terluar Pelindung Organ Otak?", "id": "Dura Mater, Lapisan Paling Keras." },
{ "en": "Akhiran '-itis' Dalam Istilah Medis Artinya?", "id": "Menandakan Adanya Peradangan Atau Inflamasi." },
{ "en": "Ahli Bedah Yang Mengoperasi Otak Saraf?", "id": "Dokter Bedah Saraf Atau Neurosurgeon." },
{ "en": "Apa Nama Fase Kontraksi Otot Jantung?", "id": "Sistol, Saat Darah Dipompa Keluar." },
{ "en": "Apa Nama Ilmiah Untuk Sel Telur?", "id": "Ovum, Gamet Betina Pada Manusia." },
{ "en": "Penyakit Akibat Protein Abnormal Di Otak?", "id": "Penyakit Prion, Seperti Penyakit Creutzfeldt-Jakob." },
{ "en": "Siapa Teknisi Yang Menyiapkan Jaringan Mikroskopis?", "id": "Histoteknolog, Teknisi Laboratorium Bidang Histologi." },
{ "en": "Apa Fungsi Hormon Paratiroid (PTH) Singkatan (Parathyroid Hormone)?", "id": "Meningkatkan Kadar Kalsium Dalam Darah." },
{ "en": "Apa Jalur Saraf Untuk Gerak Refleks?", "id": "Lengkung Refleks, Respons Tanpa Perintah Otak." },
{ "en": "Istilah Fase Relaksasi Otot Jantung Adalah?", "id": "Diastol, Saat Jantung Terisi Darah." },
{ "en": "Ahli Bedah Pembuluh Darah Vena Arteri?", "id": "Dokter Bedah Vaskular Atau Vascular Surgeon." },
{ "en": "Apa Kromosom Penentu Jenis Kelamin Manusia?", "id": "Kromosom X Dan Kromosom Y." },
{ "en": "Apa Nama Ilmiah Untuk Sel Sperma?", "id": "Sperma, Gamet Jantan Pada Manusia." },
{ "en": "Istilah Awalan 'Hiper-' Dalam Medis Artinya?", "id": "Tinggi, Berlebihan, Atau Di Atas Normal." },
{ "en": "Siapa Teknisi Pemeriksa Sel Kanker Pap Smear?", "id": "Sitoteknolog, Menganalisis Sel Di Mikroskop." },
{ "en": "Apa Fungsi Katup Mitral Di Jantung?", "id": "Mencegah Darah Kembali Ke Atrium Kiri." },
{ "en": "Apa Lapisan Tengah Pelindung Organ Otak?", "id": "Arachnoid Mater, Seperti Jaring Laba-Laba." },
{ "en": "Penyakit Infeksi Jamur Umum Di Mulut?", "id": "Kandidiasis Oral, Disebabkan Jamur Candida Albicans." },
{ "en": "Ahli Bedah Usus Besar Dan Rektum?", "id": "Dokter Bedah Kolorektal Atau Colorectal Surgeon." },
{ "en": "Apa Variasi Alternatif Dari Suatu Gen?", "id": "Alel, Mempengaruhi Sifat Atau Fenotipe." },
{ "en": "Proses Sel 'Memakan' Partikel Besar Adalah?", "id": "Fagositosis, Dilakukan Oleh Sel Fagosit." },
{ "en": "Akhiran '-oma' Dalam Istilah Medis Artinya?", "id": "Menandakan Adanya Tumor Atau Benjolan." },
{ "en": "Siapa Yang Mengelola Administrasi Rumah Sakit?", "id": "Administrator Rumah Sakit Atau Hospital Administrator." },
{ "en": "Apa Fungsi Katup Trikuspid Di Jantung?", "id": "Mencegah Darah Kembali Ke Atrium Kanan." },
{ "en": "Apa Lapisan Terdalam Pelindung Organ Otak?", "id": "Pia Mater, Melekat Langsung Otak." },
{ "en": "Istilah Awalan 'Hipo-' Dalam Medis Artinya?", "id": "Rendah, Kurang, Atau Di Bawah Normal." },
{ "en": "Petugas Kesehatan Yang Menjaga Sanitasi Lingkungan?", "id": "Sanitarian Atau Inspektur Kesehatan Lingkungan." },
{ "en": "Apa Susunan Genetik Suatu Organisme Hidup?", "id": "Genotipe, Informasi Genetik Yang Diwariskan." },
{ "en": "Proses Sel 'Meminum' Cairan Ekstraseluler Adalah?", "id": "Pinositosis, Penelanan Tetesan Cairan Kecil." },
{ "en": "Penyakit Genetik Kelainan Kromosom Seks Wanita?", "id": "Sindrom Turner, Hanya Memiliki Satu Kromosom X." },
{ "en": "Petugas Yang Memberi Pendidikan Kesehatan Masyarakat?", "id": "Edukator Kesehatan Atau Health Educator Profesional." },
{ "en": "Apa Fungsi Katup Aorta Di Jantung?", "id": "Mencegah Darah Kembali Ke Ventrikel Kiri." },
{ "en": "Apa Saraf Kranial Terpanjang Dalam Tubuh?", "id": "Saraf Vagus (Saraf Kranial X)." },
{ "en": "Akhiran '-ektomi' Dalam Istilah Medis Artinya?", "id": "Tindakan Pembedahan Untuk Mengangkat Sesuatu." },
{ "en": "Petugas Yang Mengurus Penagihan Medis Pasien?", "id": "Penagih Medis Atau Medical Biller." },
{ "en": "Apa Karakteristik Fisik Yang Terlihat Langsung?", "id": "Fenotipe, Hasil Interaksi Genotipe, Lingkungan." },
{ "en": "Proses Pelepasan Zat Dari Dalam Sel?", "id": "Eksositosis, Menggunakan Vesikel Transportasi Sel." },
{ "en": "Penyakit Genetik Kromosom Seks Pria Adalah?", "id": "Sindrom Klinefelter, Kelebihan Satu Kromosom X (XXY)." },
{ "en": "Dokter Gigi Spesialis Penyakit Gusi Adalah?", "id": "Periodontis, Ahli Jaringan Pendukung Gigi." },
{ "en": "Apa Katup Antara Ventrikel Kanan Paru-Paru?", "id": "Katup Pulmonal, Mengatur Aliran Darah." },
{ "en": "Apa Saraf Kranial Untuk Fungsi Penciuman?", "id": "Saraf Olfaktori (Saraf Kranial I)." },
{ "en": "Istilah Awalan 'Bradi-' Dalam Medis Artinya?", "id": "Lambat, Seperti Bradikardia Denyut Lambat." },
{ "en": "Ahli Fisioterapi Yang Bekerja Dengan Atlet?", "id": "Fisioterapis Olahraga Atau Sport Physiotherapist." },
{ "en": "Apa Itu Pemeriksaan Darah Lengkap Atau CBC Singkatan (Complete Blood Count)?", "id": "Tes Menghitung Sel Darah Merah, Putih." },
{ "en": "Apa Mekanisme Kontrol Utama Sistem Endokrin?", "id": "Umpan Balik Negatif Atau Negative Feedback." },
{ "en": "Penyakit Genetik Jaringan Ikat Lemah Adalah?", "id": "Sindrom Marfan, Mempengaruhi Jantung, Mata, Tulang." },
{ "en": "Dokter Gigi Spesialis Perawatan Saluran Akar?", "id": "Endodontis, Menangani Penyakit Pulpa Gigi." },
{ "en": "Bunyi Jantung Normal Disebabkan Oleh Apa?", "id": "Penutupan Katup-Katup Jantung Secara Normal." },
{ "en": "Apa Bagian Otak Pengatur Emosi, Memori?", "id": "Sistem Limbik, Termasuk Amigdala, Hipokampus." },
{ "en": "Awalan 'Taki-' Dalam Istilah Medis Artinya?", "id": "Cepat, Seperti Takikardia Denyut Cepat." },
{ "en": "Siapa Yang Membantu Pasien Dengan Gangguan Menelan?", "id": "Terapis Wicara Dan Bahasa (Speech Therapist)." },
{ "en": "Apa Itu Tes Darah Fungsi Ginjal?", "id": "Tes BUN Singkatan (Blood Urea Nitrogen) Dan Kreatinin." },
{ "en": "Apa Hormon Yang Dihasilkan Kelenjar Pineal?", "id": "Melatonin, Mengatur Ritme Sirkadian Tubuh." },
{ "en": "Kondisi Lahir Dengan Celah Di Langit-Langit?", "id": "Palatoskisis Atau Sumbing Langit-Langit Mulut." },
{ "en": "Dokter Gigi Spesialis Merapikan Posisi Gigi?", "id": "Ortodontis, Pemasang Kawat Gigi Profesional." },
{ "en": "Apa Pembuluh Darah Terkecil Di Tubuh?", "id": "Kapiler, Tempat Pertukaran Gas Nutrisi." },
{ "en": "Apa Bagian Otak Mengatur Fungsi Luhur?", "id": "Korteks Serebral, Untuk Berpikir, Berbahasa." },
{ "en": "Akhiran '-osis' Dalam Istilah Medis Artinya?", "id": "Menandakan Kondisi Abnormal Atau Penyakit." },
{ "en": "Ahli Yang Mempelajari Efek Obat Tubuh?", "id": "Farmakolog, Ilmuwan Dalam Bidang Farmakologi." },
{ "en": "Apa Itu Pencitraan Tomografi Terkomputasi (CT Scan)?", "id": "Pencitraan Sinar-X Dari Berbagai Sudut." },
{ "en": "Apa Hormon Yang Meningkatkan Gula Darah?", "id": "Glukagon, Kortisol, Adrenalin, Hormon Pertumbuhan." },
{ "en": "Kondisi Lahir Tulang Belakang Tidak Tertutup?", "id": "Spina Bifida, Cacat Tabung Saraf." },
{ "en": "Dokter Gigi Spesialis Kesehatan Gigi Anak?", "id": "Pedodontis Atau Dokter Gigi Anak." },
{ "en": "Apa Tekanan Darah Saat Jantung Berkontraksi?", "id": "Tekanan Sistolik, Angka Atas Pengukuran." },
{ "en": "Apa Cairan Yang Melindungi Otak, Tulang Belakang?", "id": "Cairan Serebrospinal, Meredam Guncangan Otak." },
{ "en": "Istilah Medis Untuk Prosedur Sterilisasi Wanita?", "id": "Ligasi Tuba, Mengikat Saluran Tuba Falopi." },
{ "en": "Siapa Peneliti Yang Mempelajari Penyebab Penyakit?", "id": "Etiolog, Ahli Dalam Bidang Etiologi." },
{ "en": "Apa Itu Pencitraan Emisi Positron (PET Scan)?", "id": "Mendeteksi Aktivitas Metabolik Sel Tubuh." },
{ "en": "Apa Hormon Yang Menurunkan Gula Darah?", "id": "Insulin, Dihasilkan Oleh Sel Beta Pankreas." },
{ "en": "Kondisi Ketika Testis Tidak Turun Skrotum?", "id": "Kriptorkismus, Testis Yang Tidak Turun." },
{ "en": "Dokter Spesialis Yang Membuat Gigi Tiruan?", "id": "Prostodontis, Ahli Restorasi Gigi Profesional." },
{ "en": "Apa Tekanan Darah Saat Jantung Berelaksasi?", "id": "Tekanan Diastolik, Angka Bawah Pengukuran." },
{ "en": "Apa Bagian Sistem Saraf Tepi Sadar?", "id": "Sistem Saraf Somatik, Mengontrol Otot Rangka." },
{ "en": "Istilah Medis Prosedur Sterilisasi Pria Adalah?", "id": "Vasektomi, Memotong Saluran Vas Deferens." },
{ "en": "Ahli Biologi Yang Mempelajari Sel Adalah?", "id": "Sitolog Atau Biolog Seluler Profesional." },
{ "en": "Apa Itu Elektrokardiogram Atau EKG Singkatan (Electrocardiogram)?", "id": "Tes Merekam Aktivitas Listrik Jantung." },
{ "en": "Kelenjar Apa Yang Disebut Kelenjar Master?", "id": "Kelenjar Pituitari, Mengontrol Kelenjar Lain." },
{ "en": "Apa Jenis Vaksin Yang Mengandung Virus Dilemahkan?", "id": "Vaksin Hidup Yang Dilemahkan (Live Attenuated)." },
{ "en": "Ahli Yang Mengkaji Aspek Moral Kedokteran?", "id": "Bioetikawan Atau Ahli Bioetika Medis." },
{ "en": "Apa Sirkulasi Darah Dari Jantung Paru-Paru?", "id": "Sirkulasi Pulmonal Atau Sirkulasi Kecil." },
{ "en": "Apa Bagian Sistem Saraf Tepi Otomatis?", "id": "Sistem Saraf Otonom, Mengontrol Organ." },
{ "en": "Efek Psikologis Dari Pengobatan Tidak Aktif?", "id": "Efek Plasebo, Perbaikan Kondisi Pasien." },
{ "en": "Personel Ambulans Tingkat Lanjut Disebut Apa?", "id": "Paramedis, Pelatihan Medis Lebih Lanjut." },
{ "en": "Pemeriksaan Apa Yang Menggunakan Gelombang Suara?", "id": "Ultrasonografi Atau USG (Ultrasonography) Medis." },
{ "en": "Apa Hormon Yang Dilepaskan Saat Stres?", "id": "Kortisol Dan Adrenalin (Epinefrin)." },
{ "en": "Apa Jenis Vaksin Mengandung Virus Mati?", "id": "Vaksin Inaktif Atau Vaksin Mati." },
{ "en": "Dokter Spesialis Penyakit Tropis Dan Infeksi?", "id": "Spesialis Penyakit Infeksi Dan Tropik." },
{ "en": "Apa Sirkulasi Darah Jantung Seluruh Tubuh?", "id": "Sirkulasi Sistemik Atau Sirkulasi Besar." },
{ "en": "Apa Refleks Sentakan Pada Tempurung Lutut?", "id": "Refleks Patela, Menguji Segmen Tulang Belakang." },
{ "en": "Istilah Untuk Pengangkatan Payudara Secara Bedah?", "id": "Mastektomi, Prosedur Bedah Pengangkatan Payudara." },
{ "en": "Ahli Yang Mempelajari Interaksi Obat Makanan?", "id": "Ahli Farmakokinetik Atau Ahli Gizi." },
{ "en": "Apa Otot Kecil Penegak Rambut Kulit?", "id": "Otot Arektor Pili, Menyebabkan Merinding." },
{ "en": "Apa Lapisan Kulit Di Bawah Epidermis?", "id": "Dermis, Mengandung Saraf Dan Pembuluh Darah." },
{ "en": "Awalan 'A-' Atau 'An-' Artinya Apa?", "id": "Tidak, Tanpa, Atau Kekurangan Sesuatu." },
{ "en": "Siapa Asisten Terapis Okupasi Di Klinik?", "id": "Asisten Terapis Okupasi (Occupational Therapy Assistant)." },
{ "en": "Apa Tahap Awal Penyembuhan Luka Tubuh?", "id": "Fase Inflamasi, Terjadi Pembengkakan Kemerahan." },
{ "en": "Apa Nama Medis Untuk Sel Tulang?", "id": "Osteosit, Sel Tulang Dewasa Matang." },
{ "en": "Gangguan Cemas Berulang Pikiran Tindakan Adalah?", "id": "OCD Singkatan (Obsessive-Compulsive Disorder)." },
{ "en": "Siapa Asisten Terapis Fisik Di Rumah Sakit?", "id": "Asisten Terapis Fisik (Physical Therapy Assistant)." },
{ "en": "Apa Fungsi Otot Siliaris Di Mata?", "id": "Mengubah Bentuk Lensa Untuk Akomodasi." },
{ "en": "Apa Proses Pembentukan Sel Darah Merah?", "id": "Eritropoiesis, Terjadi Di Sumsum Tulang." },
{ "en": "Akhiran '-algia' Dalam Istilah Medis Artinya?", "id": "Menandakan Rasa Sakit Atau Nyeri." },
{ "en": "Siapa Petugas Medis Di Helikopter Ambulans?", "id": "Perawat Penerbangan Atau Flight Nurse." },
{ "en": "Apa Jenis Patah Tulang Tak Menembus Kulit?", "id": "Patah Tulang Tertutup Atau Simple Fracture." },
{ "en": "Apa Katup Antara Lambung Usus Halus?", "id": "Katup Pilorus Atau Pyloric Sphincter." },
{ "en": "Gangguan Stres Pasca Trauma Disebut Juga?", "id": "PTSD Singkatan (Post-Traumatic Stress Disorder)." },
{ "en": "Petugas Yang Membantu Dokter Di Klinik?", "id": "Asisten Medis Atau Medical Assistant." },
{ "en": "Apa Lapisan Terdalam Kulit Berisi Lemak?", "id": "Hipodermis Atau Jaringan Subkutan Bawah." },
{ "en": "Apa Sel Imun Di Lapisan Epidermis?", "id": "Sel Langerhans, Mendeteksi Benda Asing." },
{ "en": "Akhiran '-pati' Dalam Istilah Medis Artinya?", "id": "Menandakan Adanya Suatu Penyakit Kelainan." },
{ "en": "Siapa Manajer Uji Coba Obat Klinis?", "id": "Manajer Penelitian Klinis Atau Clinical Trial Manager." },
{ "en": "Apa Tahap Kedua Penyembuhan Luka Tubuh?", "id": "Fase Proliferasi, Pembentukan Jaringan Baru." },
{ "en": "Apa Nama Tendon Terbesar Di Belakang Kaki?", "id": "Tendon Achilles, Menghubungkan Otot Betis." },
{ "en": "Istilah Penyebaran Sel Kanker Jauh Adalah?", "id": "Metastasis, Penyebaran Kanker Ke Organ." },
{ "en": "Dokter Spesialis Kaki Tangan Bedah Mikro?", "id": "Dokter Bedah Tangan Atau Hand Surgeon." },
{ "en": "Apa Molekul Pembawa Energi Utama Sel?", "id": "ATP Singkatan (Adenosine Triphosphate) Sel." },
{ "en": "Apa Sel Reseptor Sentuhan Di Kulit?", "id": "Sel Merkel, Untuk Sensasi Sentuhan." },
{ "en": "Awalan 'Dis-' Dalam Bahasa Medis Artinya?", "id": "Sulit, Sakit, Atau Abnormal Sesuatu." },
{ "en": "Ahli Biostatistik Bekerja Menganalisis Data Apa?", "id": "Data Biologis Dan Kesehatan Masyarakat." },
{ "en": "Apa Jenis Patah Tulang Menembus Lapisan Kulit?", "id": "Patah Tulang Terbuka Atau Compound Fracture." },
{ "en": "Apa Katup Antara Kerongkongan Dan Lambung?", "id": "Sfinter Esofagus Bawah Mencegah Refluks." },
{ "en": "Rasa Takut Berlebih Pada Laba-Laba Disebut?", "id": "Arachnofobia, Jenis Fobia Spesifik Umum." },
{ "en": "Siapa Ahli Yang Membuat Lensa Kacamata?", "id": "Optician Atau Ahli Kacamata Profesional." },
{ "en": "Apa Tahap Akhir Penyembuhan Luka Tubuh?", "id": "Fase Maturasi Atau Remodeling Jaringan." },
{ "en": "Apa Ligamen Penting Penstabil Sendi Lutut?", "id": "ACL Singkatan (Anterior Cruciate Ligament)." },
{ "en": "Sistem Klasifikasi Stadium Kanker Disebut Apa?", "id": "Sistem Staging TNM (Tumor, Node, Metastasis)." },
{ "en": "Ahli Yang Menangani Terapi Perilaku Kognitif?", "id": "Terapis CBT Singkatan (Cognitive Behavioral Therapy)." },
{ "en": "Apa Proses Pembelahan Sel Menjadi Empat?", "id": "Meiosis, Menghasilkan Sel Kelamin (Gamet)." },
{ "en": "Apa Struktur Di Lidah Mengandung Reseptor?", "id": "Kuncup Pengecap Atau Taste Buds." },
{ "en": "Istilah Rute Pemberian Obat Lewat Mulut?", "id": "Pemberian Secara Oral Atau Per Oral." },
{ "en": "Siapa Yang Mengevaluasi Program Kesehatan Publik?", "id": "Evaluator Program Kesehatan Atau Program Evaluator." },
{ "en": "Jenis Patah Tulang Sebagian Pada Anak?", "id": "Patah Tulang Greenstick, Seperti Ranting Basah." },
{ "en": "Gelombang Kontraksi Otot Saluran Cerna Adalah?", "id": "Gerak Peristaltik, Mendorong Makanan Maju." },
{ "en": "Rasa Takut Berlebih Ruang Terbuka Adalah?", "id": "Agorafobia, Menghindari Tempat Ramai, Terbuka." },
{ "en": "Ahli Yang Menangani Terapi Pernikahan Keluarga?", "id": "Terapis Pernikahan Dan Keluarga (MFT)." },
{ "en": "Apa Tahap Pertama Pembelahan Sel Meiosis?", "id": "Profase I, Kromosom Homolog Berpasangan." },
{ "en": "Apa Nama Saluran Penghubung Telinga-Tenggorokan?", "id": "Tuba Eustachius, Menyeimbangkan Tekanan Udara." },
{ "en": "Rute Pemberian Obat Langsung Vena Adalah?", "id": "Intravena (IV), Efek Cepat Masuk Darah." },
{ "en": "Personel Kesehatan Mental Di Komunitas Adalah?", "id": "Pekerja Kesehatan Mental Komunitas Profesional." },
{ "en": "Apa Proses Pengerasan Tulang Rawan Menjadi Tulang?", "id": "Osifikasi, Proses Pembentukan Tulang Keras." },
{ "en": "Cairan Apa Yang Dihasilkan Kelenjar Lakrimal?", "id": "Air Mata, Melumasi Melindungi Mata." },
{ "en": "Penyakit Keturunan Darah Merah Bentuk Sabit?", "id": "Anemia Sel Sabit, Menyumbat Aliran Darah." },
{ "en": "Ahli Terapi Yang Menggunakan Hipnosis Adalah?", "id": "Hipnoterapis, Terapis Klinis Bidang Hipnoterapi." },
{ "en": "Apa Tahap Kedua Pembelahan Sel Meiosis?", "id": "Metafase I, Kromosom Berjajar Ekuator." },
{ "en": "Apa Sel Yang Menghancurkan Patogen Asing?", "id": "Fagosit, Termasuk Neutrofil Dan Makrofag." },
{ "en": "Rute Pemberian Obat Ke Dalam Otot?", "id": "Intramuskular (IM), Penyerapan Lebih Lambat." },
{ "en": "Siapa Yang Mengoperasikan Peralatan Terapi Radiasi?", "id": "Terapis Radiasi Atau Radiation Therapist." },
{ "en": "Apa Proses Penguraian Glikogen Menjadi Glukosa?", "id": "Glikogenolisis, Terjadi Di Hati, Otot." },
{ "en": "Apa Bagian Tengkorak Yang Melindungi Otak?", "id": "Kranium, Terdiri Dari Beberapa Lempeng Tulang." },
{ "en": "Penyakit Gangguan Perdarahan Genetik Pada Pria?", "id": "Hemofilia, Kekurangan Faktor Pembekuan Darah." },
{ "en": "Pendidik Kesehatan Seksual Memberikan Informasi Tentang?", "id": "Kesehatan Reproduksi, Penyakit Menular Seksual." },
{ "en": "Apa Tahap Pemisahan Kromosom Meiosis I?", "id": "Anafase I, Kromosom Homolog Dipisahkan." },
{ "en": "Apa Cairan Kuning Peninggalan Pembekuan Darah?", "id": "Serum Darah, Plasma Tanpa Fibrinogen." },
{ "en": "Bedah Menggunakan Teropong Untuk Melihat Sendi?", "id": "Artroskopi, Prosedur Bedah Invasif Minimal." },
{ "en": "Ahli Biokimia Mempelajari Proses Apa Saja?", "id": "Proses Kimia Dalam Organisme Hidup." },
{ "en": "Apa Prioritas Penilaian Gawat Darurat Medis?", "id": "ABC Singkatan (Airway, Breathing, Circulation)." },
{ "en": "Struktur Apa Yang Menghubungkan Otot Tulang?", "id": "Tendon, Jaringan Ikat Fibrosa Kuat." },
{ "en": "Kondisi Kurangnya Produksi Hormon Tiroid Adalah?", "id": "Hipotiroidisme, Menyebabkan Metabolisme Melambat Tajam." },
{ "en": "Spesialis Pengobatan Untuk Para Atlet Adalah?", "id": "Dokter Kedokteran Olahraga (Sports Medicine Physician)." },
{ "en": "Apa Proses Pembentukan Glikogen Dari Glukosa?", "id": "Glikogenesis, Menyimpan Glukosa Untuk Cadangan." },
{ "en": "Apa Bagian Putih Mata Yang Terlihat?", "id": "Sklera, Jaringan Ikat Pelindung Mata." },
{ "en": "Kondisi Kelebihan Produksi Hormon Tiroid Adalah?", "id": "Hipertiroidisme, Metabolisme Tubuh Sangat Cepat." },
{ "en": "Ahli Yang Merancang Alat Bantu Disabilitas?", "id": "Insinyur Rehabilitasi Atau Rehabilitation Engineer." },
{ "en": "Sistem Penilaian Pasien Darurat Disebut Apa?", "id": "Triase, Mengurutkan Pasien Berdasar Prioritas." },
{ "en": "Apa Nama Medis Untuk Tulang Selangka?", "id": "Klavikula, Menghubungkan Lengan Dengan Badan." },
{ "en": "Penyakit Autoimun Yang Menyerang Kelenjar Tiroid?", "id": "Penyakit Graves (Hipertiroidisme), Penyakit Hashimoto (Hipotiroidisme)." },
{ "en": "Ahli Toksikologi Forensik Menganalisis Sampel Apa?", "id": "Sampel Biologis Untuk Racun, Obat-Obatan." },
{ "en": "Apa Tindakan Membuka Jalan Napas Seseorang?", "id": "Manuver Head-Tilt Chin-Lift Atau Jaw-Thrust." },
{ "en": "Apa Nama Medis Untuk Tulang Belikat?", "id": "Skapula, Tulang Pipih Di Punggung." },
{ "en": "Istilah Gula Darah Rendah Adalah Apa?", "id": "Hipoglikemia, Kadar Glukosa Darah Rendah." },
{ "en": "Siapa Yang Memberi Dukungan Emosional Pasien?", "id": "Konselor Kesehatan Mental Atau Psikolog." },
{ "en": "Apa Fungsi Utama Dari Enzim Katalase?", "id": "Memecah Hidrogen Peroksida Menjadi Air." },
{ "en": "Apa Bagian Otak Untuk Memori Jangka Panjang?", "id": "Hipokampus, Berada Di Lobus Temporal." },
{ "en": "Istilah Gula Darah Tinggi Adalah Apa?", "id": "Hiperglikemia, Kadar Glukosa Darah Tinggi." },
{ "en": "Ahli Yang Menangani Masalah Gizi Masyarakat?", "id": "Ahli Gizi Komunitas Atau Community Nutritionist." },
{ "en": "Apa Proses Sintesis Glukosa Dari Non-Karbohidrat?", "id": "Glukoneogenesis, Terjadi Terutama Di Hati." },
{ "en": "Apa Zat Yang Menghantarkan Impuls Listrik?", "id": "Elektrolit, Seperti Natrium, Kalium, Kalsium." },
{ "en": "Kondisi Kekurangan Hormon Pertumbuhan Pada Anak?", "id": "Dwarfisme Pituitari, Pertumbuhan Terhambat Pendek." },
{ "en": "Siapa Yang Menulis Resep Obat Untuk Pasien?", "id": "Dokter, Perawat Praktisi, Asisten Dokter." },
{ "en": "Sistem Hormon Pengatur Tekanan Darah Ginjal?", "id": "Sistem Renin-Angiotensin-Aldosteron Atau RAAS." },
{ "en": "Apa Sel Saraf Pengirim Impuls Otak?", "id": "Neuron Motorik Atau Neuron Eferen." },
{ "en": "Akhiran '-rhea' Dalam Istilah Medis Artinya?", "id": "Menandakan Aliran Atau Keluarnya Cairan." },
{ "en": "Siapa Yang Membela Hak-Hak Pasien RS?", "id": "Advokat Pasien Atau Patient Advocate Profesional." },
{ "en": "Apa Proses Transportasi Sel Butuh Energi?", "id": "Transpor Aktif, Melawan Gradien Konsentrasi." },
{ "en": "Apa Bagian Neuron Yang Menerima Sinyal?", "id": "Dendrit, Struktur Bercabang Seperti Pohon." },
{ "en": "Bakteri Umum Penyebab Infeksi Tenggorokan Adalah?", "id": "Streptococcus Pyogenes, Menyebabkan Radang Tenggorokan." },
{ "en": "Spesialis Bedah Mulut Dan Rahang Adalah?", "id": "Dokter Bedah Mulut Dan Maksilofasial." },
{ "en": "Apa Protein Utama Dalam Plasma Darah?", "id": "Albumin, Menjaga Tekanan Osmotik Darah." },
{ "en": "Apa Bagian Awal Dari Struktur Nefron?", "id": "Glomerulus, Jaringan Kapiler Penyaring Darah." },
{ "en": "Akhiran '-penia' Dalam Istilah Medis Artinya?", "id": "Kekurangan Atau Penurunan Jumlah Sel." },
{ "en": "Siapa Yang Mengelola Risiko Di Rumah Sakit?", "id": "Manajer Risiko Atau Hospital Risk Manager." },
{ "en": "Apa Proses Transportasi Sel Tanpa Energi?", "id": "Transpor Pasif, Seperti Difusi, Osmosis." },
{ "en": "Apa Bagian Neuron Yang Mengirim Sinyal?", "id": "Akson, Serat Panjang Penghantar Impuls." },
{ "en": "Virus Umum Penyebab Flu Biasa Adalah?", "id": "Rhinovirus, Paling Sering Menginfeksi Manusia." },
{ "en": "Ahli Yang Merancang Program Latihan Kebugaran?", "id": "Pelatih Pribadi Bersertifikat Atau Personal Trainer." },
{ "en": "Apa Jenis Golongan Darah Resipien Universal?", "id": "Golongan Darah AB, Menerima Semua." },
{ "en": "Apa Struktur Nefron Setelah Glomerulus Manusia?", "id": "Kapsul Bowman, Mengumpulkan Filtrat Darah." },
{ "en": "Awalan 'Endo-' Dalam Bahasa Medis Artinya?", "id": "Di Dalam, Internal, Atau Lapisan Dalam." },
{ "en": "Siapa Yang Melakukan Uji Tidur Polisomnografi?", "id": "Teknolog Polisomnografi Atau Teknisi Tidur." },
{ "en": "Pergerakan Air Melewati Membran Semi-permeabel Adalah?", "id": "Osmosis, Menyeimbangkan Konsentrasi Zat Terlarut." },
{ "en": "Apa Sel Saraf Penghubung Antar Neuron?", "id": "Interneuron, Memproses Sinyal Di Otak." },
{ "en": "Virus Penyebab Penyakit Influenza Tahunan Adalah?", "id": "Virus Influenza Tipe A Dan B." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Kaki?", "id": "Podiatris, Ahli Penyakit Kaki, Pergelangan." },
{ "en": "Apa Jenis Golongan Darah Donor Universal?", "id": "Golongan Darah O, Mendonorkan Semua." },
{ "en": "Apa Bagian Nefron Berbentuk 'U' Panjang?", "id": "Lengkung Henle, Menyerap Air, Garam." },
{ "en": "Awalan 'Peri-' Dalam Bahasa Medis Artinya?", "id": "Di Sekitar Atau Mengelilingi Sesuatu." },
{ "en": "Siapa Penanggung Jawab Apotek Rumah Sakit?", "id": "Kepala Instalasi Farmasi Rumah Sakit." },
{ "en": "Pergerakan Zat Dari Konsentrasi Tinggi Rendah?", "id": "Difusi, Terjadi Secara Alami Saja." },
{ "en": "Apa Jaringan Otak Berwarna Kelabu Abu-Abu?", "id": "Materi Kelabu (Gray Matter), Badan Sel." },
{ "en": "Penyakit Autoimun Kelenjar Air Mata Kering?", "id": "Sindrom Sjogren, Mata Kering Mulut." },
{ "en": "Terapis Yang Menggunakan Media Seni Adalah?", "id": "Terapis Seni Atau Art Therapist." },
{ "en": "Apa Antigen Yang Menentukan Rhesus Darah?", "id": "Faktor Rh Atau Antigen D Darah." },
{ "en": "Apa Bagian Akhir Dari Struktur Nefron?", "id": "Duktus Pengumpul, Mengumpulkan Urine Akhir." },
{ "en": "Istilah Prosedur Penyambungan Dua Saluran Bedah?", "id": "Anastomosis, Contohnya Pada Pembuluh Darah." },
{ "en": "Terapis Yang Menggunakan Media Musik Adalah?", "id": "Terapis Musik Atau Music Therapist." },
{ "en": "Apa Organel Sel Tempat Sintesis Lipid?", "id": "Retikulum Endoplasma Halus (Smooth ER)." },
{ "en": "Apa Jaringan Otak Berwarna Putih Pucat?", "id": "Materi Putih (White Matter), Akson Bermielin." },
{ "en": "Penyakit Autoimun Pengerasan Kulit Jaringan Ikat?", "id": "Skleroderma, Menyebabkan Kulit Menjadi Keras." },
{ "en": "Siapa Yang Menjalankan Tes Fungsi Pendengaran?", "id": "Audiolog, Spesialis Gangguan Dengar Keseimbangan." },
{ "en": "Protein Apa Yang Menggumpalkan Darah Akhirnya?", "id": "Fibrin, Membentuk Jaring Untuk Bekuan." },
{ "en": "Apa Hormon Yang Merangsang Produksi Aldosteron?", "id": "Angiotensin II, Bagian Dari Sistem RAAS." },
{ "en": "Istilah Pembersihan Jaringan Mati Luka Adalah?", "id": "Debridement, Membantu Proses Penyembuhan Luka." },
{ "en": "Siapa Yang Mengelola Terapi Oksigen Hiperbarik?", "id": "Teknisi Medis Hiperbarik Atau Hyperbaric Technologist." },
{ "en": "Apa Organel Sel Tempat Sintesis Protein?", "id": "Retikulum Endoplasma Kasar (Rough ER)." },
{ "en": "Apa Saraf Kranial Untuk Gerakan Wajah?", "id": "Saraf Wajah Atau Saraf Kranial VII." },
{ "en": "Penyakit Metabolik Keturunan Akumulasi Fenilalanin Adalah?", "id": "Fenilketonuria Atau PKU (Phenylketonuria)." },
{ "en": "Ahli Yang Mempelajari Perilaku Manusia Adalah?", "id": "Psikolog, Menangani Masalah Pikiran, Perilaku." },
{ "en": "Enzim Apa Yang Mengubah Fibrinogen Fibrin?", "id": "Trombin, Enzim Kunci Pembekuan Darah." },
{ "en": "Apa Hormon Yang Menyerap Natrium Ginjal?", "id": "Aldosteron, Meningkatkan Retensi Air, Garam." },
{ "en": "Istilah Untuk Mengikat Pembuluh Darah Bedah?", "id": "Ligasi, Menggunakan Benang Untuk Menghentikan Pendarahan." },
{ "en": "Siapa Yang Bertugas Mengambil Gambar Retina?", "id": "Fotografer Oftalmik Atau Ophthalmic Photographer." },
{ "en": "Apa Proses Pembentukan Urine Dari Darah?", "id": "Filtrasi, Reabsorpsi, Dan Juga Sekresi." },
{ "en": "Apa Saraf Kranial Sensasi Wajah Rahang?", "id": "Saraf Trigeminal Atau Saraf Kranial V." },
{ "en": "Penyakit Genetik Fatal Penumpukan Lipid Otak?", "id": "Penyakit Tay-Sachs, Kerusakan Sel Saraf." },
{ "en": "Ahli Gizi Yang Bekerja Dengan Atlet?", "id": "Ahli Gizi Olahraga Atau Sports Dietitian." },
{ "en": "Apa Nama Lain Untuk Sel Darah Merah?", "id": "Eritrosit, Berfungsi Membawa Oksigen Ke Jaringan." },
{ "en": "Apa Proses Penyerapan Kembali Zat Ginjal?", "id": "Reabsorpsi, Mengembalikan Zat Penting Darah." },
{ "en": "Istilah Untuk Pengangkatan Kandung Empedu Adalah?", "id": "Kolesistektomi, Prosedur Bedah Paling Umum." },
{ "en": "Ahli Yang Merancang Alat Bantu Visual?", "id": "Spesialis Penglihatan Rendah Atau Low Vision Specialist." },
{ "en": "Apa Nama Lain Sel Darah Putih?", "id": "Leukosit, Berperan Dalam Sistem Kekebalan." },
{ "en": "Apa Proses Pengeluaran Zat Sisa Ginjal?", "id": "Sekresi, Membuang Zat Dari Darah." },
{ "en": "Penyakit Kronis Kelemahan Otot Autoimun Adalah?", "id": "Miastenia Gravis, Gangguan Komunikasi Saraf-Otot." },
{ "en": "Ahli Yang Menangani Terapi Aktivitas Rekreasi?", "id": "Terapis Rekreasi Atau Recreational Therapist." },
{ "en": "Apa Nama Lain Keping Sel Darah?", "id": "Trombosit, Berperan Dalam Pembekuan Darah." },
{ "en": "Apa Hormon Yang Dihasilkan Oleh Ginjal?", "id": "Eritropoietin (EPO), Merangsang Produksi Eritrosit." },
{ "en": "Kondisi Berhentinya Siklus Menstruasi Secara Permanen?", "id": "Menopause, Terjadi Pada Wanita Usia Lanjut." },
{ "en": "Spesialis Penyakit Dalam Khusus Orang Tua?", "id": "Geriatris, Fokus Pada Kesehatan Lansia." },
{ "en": "Sel Darah Putih Mana Paling Banyak?", "id": "Neutrofil, Fagosit Utama Melawan Bakteri." },
{ "en": "Apa Unit Fungsional Dasar Dari Ginjal?", "id": "Nefron, Menyaring Darah Memproduksi Urine." },
{ "en": "Istilah Medis Untuk Nyeri Saat Menstruasi?", "id": "Dismenore, Kram Atau Nyeri Perut." },
{ "en": "Ahli Yang Menangani Pasien Ketergantungan Narkoba?", "id": "Konselor Ketergantungan Atau Addiction Counselor." },
{ "en": "Sel Darah Putih Terbesar Pemakan Patogen?", "id": "Monosit, Berkembang Menjadi Makrofag Jaringan." },
{ "en": "Apa Indikator Laju Filtrasi Ginjal Manusia?", "id": "Laju Filtrasi Glomerulus Atau GFR (Glomerular Filtration Rate)." },
{ "en": "Penyakit Radang Panggul Pada Wanita Adalah?", "id": "PID Singkatan (Pelvic Inflammatory Disease)." },
{ "en": "Siapa Yang Memberikan Perawatan Kaki Rutin?", "id": "Perawat Kaki Atau Foot Care Nurse." },
{ "en": "Apa Bagian Dari Sel Darah Putih?", "id": "Granulosit (Neutrofil, Eosinofil, Basofil), Agranulosit." },
{ "en": "Apa Yang Disaring Oleh Glomerulus Ginjal?", "id": "Plasma Darah, Kecuali Sel, Protein." },
{ "en": "Penyakit Benjolan Rahim Bukan Kanker Adalah?", "id": "Fibroid Uteri Atau Mioma Uteri." },
{ "en": "Ahli Yang Melakukan Tes Alergi Kulit?", "id": "Perawat Alergi Atau Allergy Nurse." },
{ "en": "Antibodi Apa Paling Melimpah Dalam Sirkulasi?", "id": "Imunoglobulin G Atau IgG (Immunoglobulin G)." },
{ "en": "Apa Hormon Yang Membuat Rasa Kenyang?", "id": "Leptin, Diproduksi Oleh Sel Lemak." },
{ "en": "Kondisi Pembesaran Prostat Jinak Pada Pria?", "id": "BPH Singkatan (Benign Prostatic Hyperplasia)." },
{ "en": "Ahli Genetika Yang Memberikan Konseling Adalah?", "id": "Konselor Genetik, Memberi Informasi Risiko." },
{ "en": "Antibodi Apa Yang Terlibat Reaksi Alergi?", "id": "Imunoglobulin E Atau IgE (Immunoglobulin E)." },
{ "en": "Apa Hormon Yang Membuat Rasa Lapar?", "id": "Ghrelin, Diproduksi Terutama Di Lambung." },
{ "en": "Istilah Untuk Pengangkatan Rahim Secara Bedah?", "id": "Histerektomi, Prosedur Bedah Pengangkatan Rahim." },
{ "en": "Siapa Yang Mengelola Program Kesehatan Korporat?", "id": "Manajer Kesehatan Korporat Atau Wellness Manager." },
{ "en": "Apa Sel Glial Utama Di Otak?", "id": "Astrosit, Memberi Dukungan Struktural, Nutrisi." },
{ "en": "Apa Saluran Limfatik Utama Di Dada?", "id": "Duktus Torasikus Atau Saluran Limfe Kiri." },
{ "en": "Istilah Pembentukan Jaringan Baru Abnormal Adalah?", "id": "Neoplasia, Bisa Ganas Atau Jinak." },
{ "en": "Siapa Yang Merencanakan Dosis Terapi Radiasi?", "id": "Dosimetris Medis Atau Medical Dosimetrist." },
{ "en": "Apa Siklus Metabolisme Penghasil Energi Utama?", "id": "Siklus Asam Sitrat Atau Siklus Krebs." },
{ "en": "Apa Sel Yang Membentuk Selubung Mielin?", "id": "Sel Schwann (Saraf Tepi), Oligodendrosit (Pusat)." },
{ "en": "Penyakit Genetik Kerusakan Saraf Progresif Adalah?", "id": "Penyakit Huntington, Gerakan Tak Terkendali." },
{ "en": "Spesialis Jantung Yang Menangani Anak-Anak Adalah?", "id": "Kardiolog Pediatrik Atau Pediatric Cardiologist." },
{ "en": "Apa Penghalang Pelindung Antara Darah Otak?", "id": "Sawer Darah Otak (Blood-Brain Barrier)." },
{ "en": "Di Mana Cairan Serebrospinal Diproduksi Utamanya?", "id": "Pleksus Koroid Di Ventrikel Otak." },
{ "en": "Akhiran '-genesis' Dalam Istilah Medis Artinya?", "id": "Pembentukan, Produksi, Atau Asal Mula." },
{ "en": "Spesialis Saraf Mata Dan Otak Adalah?", "id": "Neuro-Oftalmolog, Menangani Koneksi Saraf Mata." },
{ "en": "Apa Tanda Klasik Dari Proses Peradangan?", "id": "Rubor (Merah), Kalor (Panas), Tumor (Bengkak)." },
{ "en": "Apa Fase Awal Siklus Menstruasi Wanita?", "id": "Fase Folikular, Pematangan Folikel Ovarium." },
{ "en": "Istilah Tumor Yang Tidak Menyebar Ganas?", "id": "Tumor Jinak Atau Tumor Benigna." },
{ "en": "Spesialis Hormon Reproduksi Dan Kesuburan Adalah?", "id": "Endokrinolog Reproduksi, Ahli Infertilitas Wanita." },
{ "en": "Apa Tahap Akhir Respirasi Seluler Aerobik?", "id": "Rantai Transpor Elektron, Menghasilkan ATP." },
{ "en": "Di Mana Kumpulan Jaringan Limfoid Usus?", "id": "Plak Peyer, Memantau Patogen Usus." },
{ "en": "Akhiran '-lisis' Dalam Istilah Medis Artinya?", "id": "Perusakan, Pemecahan, Atau Pelepasan Sesuatu." },
{ "en": "Siapa Yang Mencatat Aktivitas Listrik Otak?", "id": "Teknolog Elektoneurodiagnostik Atau Teknisi END." },
{ "en": "Apa Sel Epitel Berbentuk Pipih Tipis?", "id": "Epitel Pipih Selapis (Skuamosa Simpleks)." },
{ "en": "Apa Saluran Penyimpanan Pematangan Sel Sperma?", "id": "Epididimis, Terletak Di Atas Testis." },
{ "en": "Istilah Tumor Ganas Yang Bisa Menyebar?", "id": "Tumor Ganas Atau Tumor Maligna." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Alergi?", "id": "Alergolog, Ahli Alergi Dan Imunologi." },
{ "en": "Hormon Apa Yang Memicu Proses Ovulasi?", "id": "LH Singkatan (Luteinizing Hormone) Meningkat." },
{ "en": "Apa Sel Epitel Berbentuk Seperti Kubus?", "id": "Epitel Kubus Selapis (Kuboid Simpleks)." },
{ "en": "Penyakit Akibat Kekurangan Vitamin B9 (Folat)?", "id":"Anemia Megaloblastik, Cacat Tabung Saraf." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Rematik?", "id": "Rematolog, Ahli Penyakit Autoimun Sendi." },
{ "en": "Hormon Apa Yang Merangsang Pertumbuhan Folikel?", "id": "FSH Singkatan (Follicle-Stimulating Hormone)." },
{ "en": "Apa Fase Kedua Siklus Menstruasi Wanita?", "id": "Fase Luteal, Pembentukan Korpus Luteum." },
{ "en": "Zat Yang Mengaktifkan Reseptor Sel Adalah?", "id": "Agonis, Meniru Efek Zat Alami." },
{ "en": "Dokter Spesialis Yang Menangani Penyakit Ginjal?", "id": "Nefrolog, Ahli Kesehatan Organ Ginjal." },
{ "en": "Apa Jenis Jaringan Ikat Paling Umum?", "id": "Jaringan Ikat Areolar, Pengisi Ruang." },
{ "en": "Saluran Apa Yang Membawa Sperma Testis?", "id": "Vas Deferens Atau Duktus Deferens." },
{ "en": "Zat Yang Menghambat Reseptor Sel Adalah?", "id": "Antagonis, Memblokir Aksi Zat Agonis." },
{ "en": "Dokter Spesialis Yang Menangani Penyakit Paru?", "id": "Pulmonolog, Ahli Sistem Pernapasan Manusia." },
{ "en": "Apa Fungsi Utama Mineral Fosfor Tubuh?", "id": "Pembentukan Tulang, Gigi, Energi (ATP)." },
{ "en": "Apa Sel Epitel Berbentuk Kolom Tinggi?", "id": "Epitel Kolumnar Selapis (Kolumnar Simpleks)." },
{ "en": "Waktu Paruh Obat Dalam Farmakologi Artinya?", "id": "Waktu Konsentrasi Obat Berkurang Setengah." },
{ "en": "Spesialis Yang Mendiagnosis Penyakit Jaringan Adalah?", "id": "Patolog, Menganalisis Sampel Biopsi Jaringan." },
{ "en": "Apa Sisa Folikel Setelah Proses Ovulasi?", "id": "Korpus Luteum, Menghasilkan Hormon Progesteron." },
{ "en": "Jenis Jaringan Ikat Penyimpan Lemak Adalah?", "id": "Jaringan Adiposa Atau Jaringan Lemak." },
{ "en": "Penyakit Akibat Kekurangan Vitamin B6 Adalah?", "id": "Anemia, Gangguan Kulit, Dan Saraf." },
{ "en": "Ahli Patologi Yang Mengkhususkan Diri Darah?", "id": "Hematopatolog, Mendiagnosis Kelainan Darah Kanker." },
{ "en": "Apa Hormon Yang Mempertahankan Dinding Rahim?", "id": "Progesteron, Penting Untuk Awal Kehamilan." },
{ "en": "Apa Itu Aponeurosis Dalam Sistem Otot?", "id": "Lembaran Jaringan Ikat Penghubung Otot." },
{ "en": "Istilah Medis Untuk Pengangkatan Amandel Adalah?", "id": "Tonsilektomi, Prosedur Bedah Pengangkatan Tonsil." },
{ "en": "Spesialis Yang Menangani Masalah Pencernaan Adalah?", "id": "Gastroenterolog, Ahli Sistem Gastrointestinal (GI)." },
{ "en": "Apa Luruhnya Dinding Rahim Saat Menstruasi?", "id": "Endometrium, Dinding Rahim Yang Menebal." },
{ "en": "Apa Bagian Otak Besar Yang Terlihat?", "id": "Korteks Serebral, Pusat Pikiran, Kesadaran." },
{ "en": "Penyakit Radang Pada Kantung Divertikula Adalah?", "id": "Divertikulitis, Infeksi Kantung Di Usus." },
{ "en": "Spesialis Yang Menangani Masalah Hormon Adalah?", "id": "Endokrinolog, Ahli Kelenjar Dan Hormon." },
{ "en": "Struktur Apa Yang Menutupi Trakea Saat Menelan?", "id": "Epiglotis, Mencegah Makanan Masuk Paru." },
{ "en": "Apa Bagian Neuron Yang Mengandung Nukleus?", "id": "Badan Sel Atau Soma Neuron." },
{ "en": "Istilah Untuk Pengangkatan Usus Buntu Bedah?", "id": "Apendektomi, Prosedur Bedah Pengangkatan Apendiks." },
{ "en": "Spesialis Yang Menangani Masalah Vena Adalah?", "id": "Flebolog, Ahli Penyakit Pembuluh Vena." },
{ "en": "Apa Proses Pelepasan Sel Telur Matang?", "id": "Ovulasi, Terjadi Di Tengah Siklus." },
{ "en": "Apa Ruang Antara Paru-Paru Dan Jantung?", "id": "Mediastinum, Rongga Di Tengah Dada." },
{ "en": "Penyakit Radang Selaput Jantung Dalam Adalah?", "id": "Endokarditis, Infeksi Lapisan Endokardium Jantung." },
{ "en": "Ahli Fisiologi Yang Mempelajari Olahraga Adalah?", "id": "Fisiolog Latihan Atau Exercise Physiologist." },
{ "en": "Apa Pembuluh Darah Yang Membawa Darah Oksigen?", "id": "Arteri, Kecuali Arteri Pulmonalis Darah." },
{ "en": "Apa Bagian Terluar Dari Ginjal Manusia?", "id": "Korteks Ginjal, Mengandung Jutaan Nefron." },
{ "en": "Istilah Untuk Pembuatan Lubang Trakea Bedah?", "id": "Trakeostomi, Untuk Membantu Proses Pernapasan." },
{ "en": "Dokter Spesialis Yang Menangani Luka Adalah?", "id": "Spesialis Perawatan Luka Atau Wound Care." },
{ "en": "Apa Pembuluh Darah Yang Membawa Darah Kotor?", "id": "Vena, Kecuali Vena Pulmonalis Darah." },
{ "en": "Apa Bagian Terdalam Dari Organ Ginjal?", "id": "Medula Ginjal, Berisi Lengkung Henle." },
{ "en": "Penyakit Radang Otot Jantung Miokardium Adalah?", "id": "Miokarditis, Biasanya Disebabkan Oleh Virus." },
{ "en": "Ahli Mikrobiologi Yang Mempelajari Jamur Adalah?", "id": "Mikolog, Ilmuwan Dalam Bidang Mikologi." },
{ "en": "Apa Cairan Yang Mengelilingi Sendi Sinovial?", "id": "Cairan Sinovial, Berfungsi Sebagai Pelumas." },
{ "en": "Apa Jaringan Yang Menyelimuti Otot Individu?", "id": "Endomisium, Lapisan Jaringan Ikat Halus." },
{ "en": "Penyakit Radang Kantung Pembungkus Jantung Adalah?", "id": "Perikarditis, Infeksi Atau Peradangan Perikardium." },
{ "en": "Ahli Mikrobiologi Yang Mempelajari Parasit Adalah?", "id": "Parasitolog, Ilmuwan Dalam Bidang Parasitologi." },
{ "en": "Apa Membran Yang Melapisi Rongga Perut?", "id": "Peritoneum, Membran Serosa Terbesar Tubuh." },
{ "en": "Apa Jaringan Yang Membungkus Berkas Otot?", "id": "Perimisium, Melapisi Sekelompok Serat Otot." },
{ "en": "Istilah Untuk Batu Di Kantung Empedu?", "id": "Kolelitiasis, Penyakit Batu Kantung Empedu." },
{ "en": "Dokter Spesialis Kedokteran Bawah Air Adalah?", "id": "Dokter Hiperbarik Atau Dokter Selam." },
{ "en": "Apa Gelombang Otak Saat Relaksasi Sadar?", "id": "Gelombang Alfa, Terdeteksi Oleh Alat EEG." },
{ "en": "Apa Jaringan Yang Membungkus Seluruh Otot?", "id": "Epimisium, Lapisan Terluar Dari Otot." },
{ "en": "Istilah Untuk Batu Di Saluran Kemih?", "id": "Urolitiasis, Penyakit Batu Saluran Kemih." },
{ "en": "Spesialis Pengobatan Untuk Perjalanan Jauh Adalah?", "id": "Spesialis Kedokteran Perjalanan Atau Travel Medicine." },
{ "en": "Apa Gelombang Otak Saat Tidur Lelap?", "id": "Gelombang Delta, Frekuensi Paling Lambat." },
{ "en": "Apa Titik Pertemuan Tiga Tulang Panggul?", "id": "Asetabulum, Soket Sendi Panggul Utama." },
{ "en": "Kondisi Adanya Darah Dalam Urine Adalah?", "id": "Hematuria, Bisa Terlihat Atau Mikroskopis." },
{ "en": "Ahli Yang Membantu Proses Persalinan Adalah?", "id": "Bidan Atau Doula Profesional Terlatih." },
{ "en": "Apa Gelombang Otak Saat Aktif Berpikir?", "id": "Gelombang Beta, Saat Waspada, Konsentrasi." },
{ "en": "Apa Enzim Yang Ditemukan Di Air Mata?", "id": "Lisozim, Memiliki Sifat Antibakteri Alami." },
{ "en": "Kondisi Adanya Protein Dalam Urine Adalah?", "id": "Proteinuria, Tanda Potensial Kerusakan Ginjal." },
{ "en": "Ahli Yang Mempelajari Penyakit Pada Tanaman?", "id": "Patolog Tumbuhan Atau Plant Pathologist." },
{ "en": "Sistem Vena Unik Dari Usus Hati?", "id": "Sistem Vena Portal Hepatik Darah." },
{ "en": "Apa Sel T Imun Yang Membantu Sel Lain?", "id": "Sel T Helper, Mengoordinasikan Respons Imun." },
{ "en": "Tindakan Mengambil Cairan Otak Belakang Adalah?", "id": "Pungsi Lumbal Atau Lumbar Puncture." },
{ "en": "Ahli Radiologi Yang Melakukan Prosedur Minimal?", "id": "Radiolog Intervensi Atau Interventional Radiologist." },
{ "en": "Hormon Apa Merangsang Sekresi Asam Lambung?", "id": "Gastrin, Dihasilkan Oleh Sel G Lambung." },
{ "en": "Apa Lingkaran Arteri Di Dasar Otak?", "id": "Lingkaran Willis, Memasok Darah Ke Otak." },
{ "en": "Pembentukan Pembuluh Darah Baru Tumor Adalah?", "id": "Angiogenesis, Penting Untuk Pertumbuhan Tumor." },
{ "en": "Dokter Spesialis Pengobatan Rasa Nyeri Adalah?", "id": "Algolog Atau Spesialis Manajemen Nyeri." },
{ "en": "Apa Sel T Imun Yang Membunuh Sel Terinfeksi?", "id": "Sel T Sitotoksik Atau Sel CD8+." },
{ "en": "Apa Lubang Jantung Janin Antara Atrium?", "id": "Foramen Ovale, Menutup Setelah Bayi Lahir." },
{ "en": "Istilah Untuk Pencegahan Penyakit Adalah Apa?", "id": "Profilaksis, Tindakan Mencegah Sebelum Terjadi." },
{ "en": "Siapa Yang Mentranskrip Rekaman Suara Dokter?", "id": "Transkripsionis Medis Atau Medical Transcriptionist." },
{ "en": "Hormon Apa Merangsang Pelepasan Enzim Pankreas?", "id": "Kolesistokinin Atau CCK (Cholecystokinin)." },
{ "en": "Apa Sel Imun Yang Menghasilkan Antibodi?", "id": "Sel B, Berkembang Menjadi Sel Plasma." },
{ "en": "Tindakan Mengambil Cairan Rongga Dada Adalah?", "id": "Torasentesis, Untuk Diagnosis Atau Terapi." },
{ "en": "Ahli Jantung Prosedur Kateterisasi Jantung Adalah?", "id": "Kardiolog Intervensi Atau Interventional Cardiologist." },
{ "en": "Apa Pembuluh Penghubung Aorta Arteri Pulmonalis Janin?", "id": "Duktus Arteriosus, Menutup Setelah Lahir." },
{ "en": "Apa Antibodi Utama Di Air Mata Ludah?", "id": "Imunoglobulin A Atau IgA (Immunoglobulin A)." },
{ "en": "Istilah Ketersediaan Obat Diserap Tubuh Adalah?", "id": "Bioavailabilitas, Ukuran Kecepatan, Jumlah Penyerapan." },
{ "en": "Siapa Pengacara Spesialis Kasus Medis Malapraktik?", "id": "Pengacara Malapraktik Medis Profesional Hukum." },
{ "en": "Hormon Apa Merangsang Pankreas Melepas Bikarbonat?", "id": "Sekretin, Menetralkan Asam Dari Lambung." },
{ "en": "Apa Protein Sistem Imun Pelengkap Antibodi?", "id": "Sistem Komplemen, Melisiskan Sel Patogen." },
{ "en": "Tindakan Mengambil Cairan Rongga Perut Adalah?", "id": "Parasentesis, Mengurangi Asites Atau Cairan." },
{ "en": "Dokter Spesialis Kulit Dengan Prosedur Bedah?", "id": "Dokter Bedah Dermatologi Atau Dermatologic Surgeon." },
{ "en": "Apa Gambaran Visual Kromosom Sel Manusia?", "id": "Kariotipe, Digunakan Menganalisis Kelainan Kromosom." },
{ "en": "Apa Antibodi Pertama Kali Diproduksi Infeksi?", "id": "Imunoglobulin M Atau IgM (Immunoglobulin M)." },
{ "en": "Rentang Dosis Obat Efektif Aman Adalah?", "id": "Indeks Terapeutik, Rasio Dosis Toksik-Efektif." },
{ "en": "Siapa Yang Mengelola Data Epidemiologi Kesehatan?", "id": "Informatikawan Kesehatan Masyarakat Atau Public Health Informatician." },
{ "en": "Apa Pewarisan Gen Terletak Kromosom Non-Seks?", "id": "Pewarisan Autosomal, Bisa Dominan Resesif." },
{ "en": "Apa Tes Darah Penanda Peradangan Umum?", "id": "Protein C-Reaktif Atau CRP (C-Reactive Protein)." },
{ "en": "Tindakan Mengambil Cairan Dari Sendi Adalah?", "id": "Artrosentesis, Mendiagnosis Masalah Pada Sendi." },
{ "en": "Ahli Patologi Yang Mendiagnosis Lewat Mikroskop?", "id": "Ahli Sitopatologi, Memeriksa Sel Individu." },
{ "en": "Apa Pewarisan Gen Terletak Kromosom Seks?", "id": "Pewarisan Terkait-X (X-Linked Inheritance)." },
{ "en": "Apa Tes Darah Mengukur Kecepatan Endap?", "id": "Laju Endap Darah Atau LED (ESR)." },
{ "en": "Terapi Mengganti Gen Rusak Normal Adalah?", "id": "Terapi Gen, Teknik Eksperimental Masa Depan." },
{ "en": "Dokter Spesialis Yang Menangani Kanker Darah?", "id": "Hematolog-Onkolog, Gabungan Spesialisasi Darah Kanker." },
{ "en": "Apa Cairan Yang Membawa Sel-Sel Limfa?", "id": "Limfa Atau Getah Bening Bening." },
{ "en": "Proses Mikroba Tumbuh Media Laboratorium Adalah?", "id": "Kultur, Untuk Mengidentifikasi Jenis Mikroba." },
{ "en": "Istilah Untuk Pengangkatan Kandung Kemih Adalah?", "id": "Sistektomi, Prosedur Bedah Pengangkatan Kandung Kemih." },
{ "en": "Dokter Spesialis Yang Merawat Pasien Kritis?", "id": "Intensivis Atau Dokter Perawatan Kritis." },
{ "en": "Apa Jenis Kekebalan Yang Didapat Vaksinasi?", "id": "Kekebalan Aktif Buatan, Tubuh Membuat Antibodi." },
{ "en": "Apa Organ Limfoid Terbesar Di Tubuh?", "id": "Limpa, Menyaring Darah, Menyimpan Sel." },
{ "en": "Tes Menentukan Kepekaan Bakteri Antibiotik Adalah?", "id": "Tes Sensitivitas Atau Uji Kepekaan." },
{ "en": "Ahli Perilaku Hewan Terapan Disebut Apa?", "id": "Behavioris Hewan Atau Animal Behaviorist." },
{ "en": "Apa Jenis Kekebalan Bayi Dari Ibu?", "id": "Kekebalan Pasif Alami, Melalui Plasenta ASI." },
{ "en": "Apa Pembuluh Limfa Terkecil Di Jaringan?", "id": "Kapiler Limfatik, Menyerap Cairan Interstisial." },
{ "en": "Istilah Untuk Gagal Hati Akut Adalah?", "id": "Gagal Hati Fulminan, Kondisi Serius." },
{ "en": "Dokter Spesialis Imunologi Dan Reumatologi Adalah?", "id": "Imunolog-Reumatolog, Menangani Penyakit Sistemik Autoimun." },
{ "en": "Apa Memori Sistem Imun Setelah Infeksi?", "id": "Sel Memori (B Dan T)." },
{ "en": "Struktur Otot Yang Mengelilingi Saluran Tertentu?", "id": "Sfinkter, Berfungsi Seperti Katup Otot." },
{ "en": "Kondisi Irama Jantung Sangat Cepat Kacau?", "id": "Fibrilasi, Bisa Atrial Atau Ventrikular." },
{ "en": "Ahli Yang Mempelajari Tulang Kuno Adalah?", "id": "Paleopatolog, Mempelajari Penyakit Di Masa Lalu." },
{ "en": "Apa Proses Pematangan Sel T Limfosit?", "id": "Terjadi Di Kelenjar Timus Dada." },
{ "en": "Apa Lapisan Otot Di Dinding Rahim?", "id": "Miometrium, Berkontraksi Saat Persalinan Normal." },
{ "en": "Kondisi Kurva Tulang Belakang Berbentuk 'S'?", "id": "Skoliosis, Kelengkungan Abnormal Ke Samping." },
{ "en": "Dokter Spesialis Yang Menangani Vaskulitis Adalah?", "id": "Rematolog Atau Spesialis Vaskulitis Langka." },
{ "en": "Apa Proses Pematangan Sel B Limfosit?", "id": "Terjadi Di Sumsum Tulang Belakang." },
{ "en": "Apa Lapisan Otot Dinding Pembuluh Darah?", "id": "Tunika Media, Terdiri Dari Otot Polos." },
{ "en": "Kondisi Kurva Tulang Punggung Bungkuk Adalah?", "id": "Kifosis, Kelengkungan Berlebihan Ke Depan." },
{ "en": "Ahli Yang Mempelajari Penyakit Lewat Molekul?", "id": "Patolog Molekuler, Menganalisis DNA, RNA, Protein." },
{ "en": "Apa Sel Yang Menghasilkan Pigmen Kulit?", "id": "Melanosit, Memproduksi Melanin Pelindung UV." },
{ "en": "Lapisan Terdalam Pembuluh Darah Disebut Apa?", "id": "Tunika Intima, Berkontak Langsung Darah." },
{ "en": "Kondisi Kurva Tulang Pinggang Berlebih Adalah?", "id": "Lordosis, Kelengkungan Lumbal Ke Dalam." },
{ "en": "Dokter Spesialis Yang Menangani Luka Bakar?", "id": "Dokter Bedah Luka Bakar Profesional." },
{ "en": "Apa Sel Yang Membentuk Jaringan Tulang?", "id": "Osteosit, Osteoblas (Pembangun), Osteoklas (Penghancur)." },
{ "en": "Lapisan Terluar Pembuluh Darah Disebut Apa?", "id": "Tunika Eksterna Atau Tunika Adventitia." },
{ "en": "Istilah Untuk Pengangkatan Limpa Secara Bedah?", "id": "Splenektomi, Prosedur Bedah Pengangkatan Limpa." },
{ "en": "Spesialis Radiasi Yang Fokus Pada Kanker?", "id": "Onkolog Radiasi, Menggunakan Radiasi Terapi." },
{ "en": "Apa Sel Yang Membentuk Jaringan Tulang Rawan?", "id": "Kondrosit, Terletak Dalam Lakuna Jaringan." },
{ "en": "Apa Rongga Di Tulang Tempat Sumsum?", "id": "Rongga Meduler, Berisi Sumsum Tulang Kuning." },
{ "en": "Penyakit Kekurangan Enzim Laktase Dalam Usus?", "id": "Intoleransi Laktosa, Sulit Mencerna Susu." },
{ "en": "Dokter Spesialis Yang Menangani Racun Gigitan?", "id": "Toksikolog Klinis, Ahli Keracunan Medis." },
{ "en": "Apa Bagian Dari Struktur Tulang Panjang?", "id": "Diafisis (Batang), Epifisis (Ujung Tulang)." },
{ "en": "Apa Titik Masuk Pembuluh Saraf Organ?", "id": "Hilum, Cekungan Pada Permukaan Organ." },
{ "en": "Penyakit Autoimun Tidak Bisa Toleransi Gluten?", "id": "Penyakit Seliak, Merusak Lapisan Usus." },
{ "en": "Siapa Yang Menjalankan Tes Diagnostik Vaskular?", "id": "Teknolog Vaskular, Menggunakan USG Doppler." },
{ "en": "Apa Sendi Antara Tulang Tengkorak Dewasa?", "id": "Sutura, Jenis Sendi Fibrosa Kaku." },
{ "en": "Struktur Mirip Kantung Udara Di Paru?", "id": "Alveolus, Tempat Pertukaran Gas Oksigen." },
{ "en": "Istilah Untuk Prolaps Atau Turunnya Organ?", "id": "-ptosis, Seperti Ptosis Kelopak Mata." },
{ "en": "Ahli Yang Menangani Terapi Biofeedback Adalah?", "id": "Terapis Biofeedback, Mengajarkan Kontrol Fisiologis." },
{ "en": "Apa Sendi Antara Gigi Dan Rahang?", "id": "Gomfosis, Jenis Sendi Fibrosa Khusus." },
{ "en": "Apa Hormon Yang Dihasilkan Buah Zakar?", "id": "Testosteron, Mengatur Sifat Kelamin Pria." },
{ "en": "Kondisi Peradangan Pada Vena Darah Adalah?", "id": "Flebitis, Sering Terjadi Di Kaki." },
{ "en": "Dokter Spesialis Yang Menangani Penuaan Sehat?", "id": "Dokter Kedokteran Anti-Penuaan (Anti-Aging)." },
{ "en": "Apa Tulang Yang Tidak Bersendi Tulang Lain?", "id": "Tulang Hioid, Terletak Di Leher." },
{ "en": "Apa Hormon Yang Dihasilkan Indung Telur?", "id": "Estrogen Dan Progesteron, Hormon Wanita." },
{ "en": "Kondisi Gumpalan Darah Di Vena Dalam?", "id": "Trombosis Vena Dalam Atau DVT (Deep Vein Thrombosis)." },
{ "en": "Siapa Peneliti Yang Mempelajari Interaksi Manusia-Mesin?", "id": "Ergonomis Atau Ahli Faktor Manusia." },
{ "en": "Apa Bagian Terluar Dari Kelenjar Adrenal?", "id": "Korteks Adrenal, Menghasilkan Hormon Steroid." },
{ "en": "Apa Bagian Terdalam Dari Kelenjar Adrenal?", "id": "Medula Adrenal, Menghasilkan Hormon Adrenalin." },
{ "en": "Istilah Untuk Pengerasan Dan Penebalan Arteri?", "id": "Arteriosklerosis, Kehilangan Elastisitas Pembuluh Arteri." },
{ "en": "Ahli Yang Mempelajari Embrio Dan Perkembangannya?", "id": "Embriolog, Ilmuwan Dalam Bidang Embriologi." },
{ "en": "Apa Persambungan Sel Yang Mencegah Kebocoran?", "id": "Sambungan Ketat Atau Dikenal Tight Junction." },
{ "en": "Kumpulan Sel Penghasil Insulin Di Pankreas?", "id": "Pulau Langerhans, Terutama Sel Beta." },
{ "en": "Istilah Medis Untuk Kesulitan Menelan Adalah?", "id": "Disfagia, Gangguan Proses Menelan Makanan." },
{ "en": "Siapa Insinyur Perancang Peralatan Medis Canggih?", "id": "Insinyur Biomedis Atau Biomedical Engineer." },
{ "en": "Apa Tiga Lapisan Germinal Awal Embrio?", "id": "Ektoderm, Mesoderm, Dan Juga Endoderm." },
{ "en": "Apa Sel Khas Pada Jaringan Tulang?", "id": "Osteon Atau Sistem Haversian Fungsional." },
{ "en": "Studi Bagaimana Obat Mempengaruhi Tubuh Disebut?", "id": "Farmakodinamik, Studi Efek Obat Tubuh." },
{ "en": "Perawat Spesialis Yang Merawat Pasien Kanker?", "id": "Perawat Onkologi Atau Oncology Nurse." },
{ "en": "Apa Persambungan Sel Untuk Komunikasi Langsung?", "id": "Sambungan Celah Atau Gap Junction." },
{ "en": "Apa Kelenjar Di Dinding Usus Halus?", "id": "Kriptus Lieberkühn, Menghasilkan Enzim Usus." },
{ "en": "Istilah Medis Untuk Kesulitan Berbicara Adalah?", "id": "Disfasia, Gangguan Kemampuan Produksi Bahasa." },
{ "en": "Ilmuwan Yang Bekerja Di Laboratorium Klinis?", "id": "Ilmuwan Laboratorium Klinis (Clinical Laboratory Scientist)." },
{ "en": "Lapisan Germinal Apa Membentuk Sistem Saraf?", "id": "Ektoderm, Juga Membentuk Kulit, Rambut." },
{ "en": "Apa Aparatus Di Ginjal Pengatur Tekanan?", "id": "Aparatus Juxtaglomerular, Menghasilkan Hormon Renin." },
{ "en": "Studi Bagaimana Tubuh Memproses Obat Disebut?", "id": "Farmakokinetik, Meliputi Absorpsi, Distribusi, Metabolisme." },
{ "en": "Perawat Spesialis Di Unit Perawatan Intensif?", "id": "Perawat Perawatan Kritis (Critical Care Nurse)." },
{ "en": "Apa Persambungan Sel Perekat Yang Kuat?", "id": "Desmosom, Menjaga Sel Tetap Bersatu." },
{ "en": "Sistem Keseimbangan Di Telinga Dalam Adalah?", "id": "Sistem Vestibular, Terdiri Utrikulus Sakulus." },
{ "en": "Istilah Medis Untuk Kehilangan Kesadaran Singkat?", "id": "Sinkop, Atau Pingsan Akibat Kurang Oksigen." },
{ "en": "Siapa Pemimpin Utama Proyek Penelitian Medis?", "id": "Penyelidik Utama Atau Principal Investigator (PI)." },
{ "en": "Lapisan Germinal Apa Membentuk Otot Tulang?", "id": "Mesoderm, Juga Membentuk Sistem Sirkulasi." },
{ "en": "Apa Kemampuan Otak Untuk Merasakan Posisi?", "id": "Propriosepsi, Kesadaran Posisi Tubuh Ruang." },
{ "en": "Efek Negatif Akibat Sugesti Pengobatan Adalah?", "id": "Efek Nocebo, Kebalikan Dari Efek Plasebo." },
{ "en": "Peneliti Setelah Gelar Doktor Disebut Apa?", "id": "Peneliti Pascadoktoral Atau Postdoctoral Researcher." },
{ "en": "Lapisan Germinal Apa Membentuk Saluran Cerna?", "id": "Endoderm, Juga Membentuk Hati, Pankreas." },
{ "en": "Apa Molekul Koenzim Pembawa Elektron Utama?", "id": "NAD+ (Nicotinamide Adenine Dinucleotide) Dan FAD." },
{ "en": "Istilah Medis Untuk Telinga Berdenging Adalah?", "id": "Tinnitus, Persepsi Bunyi Tanpa Rangsangan." },
{ "en": "Ahli Ekonomi Yang Menganalisis Biaya Kesehatan?", "id": "Ekonom Kesehatan Atau Health Economist." },
{ "en": "Apa Proses Pemecahan Glukosa Menjadi Piruvat?", "id": "Glikolisis, Langkah Awal Respirasi Seluler." },
{ "en": "Apa Bagian Otak Yang Mengatur Keseimbangan?", "id": "Serebelum, Mengoordinasikan Gerakan Motorik Halus." },
{ "en": "Istilah Perubahan Abnormal Sel Dewasa Adalah?", "id": "Displasia, Seringkali Merupakan Kondisi Prakanker." },
{ "en": "Perawat Spesialis Kesehatan Mental Jiwa Adalah?", "id": "Perawat Psikiatri Atau Psychiatric Nurse." },
{ "en": "Apa Alat Untuk Memeriksa Bagian Dalam Mata?", "id": "Oftalmoskop, Melihat Retina Dan Saraf Optik." },
{ "en": "Apa Bagian Dari Sistem Vestibular Telinga?", "id": "Utrikulus Dan Sakulus, Mendeteksi Gravitasi." },
{ "en": "Istilah Penggantian Satu Jenis Sel Dewasa?", "id": "Metaplasia, Respons Adaptif Terhadap Stres." },
{ "en": "Ahli Budaya Yang Mempelajari Kesehatan Adalah?", "id": "Antropolog Medis Atau Medical Anthropologist." },
{ "en": "Apa Alat Untuk Memeriksa Saluran Telinga?", "id": "Otoskop, Melihat Membran Timpani Telinga." },
{ "en": "Apa Hormon Yang Dihasilkan Sel Alfa Pankreas?", "id": "Glukagon, Berfungsi Menaikkan Gula Darah." },
{ "en": "Jenis Nekrosis Khas Infeksi Tuberkulosis Adalah?", "id": "Nekrosis Kaseosa, Bertekstur Seperti Keju." },
{ "en": "Perawat Spesialis Yang Merawat Bayi Prematur?", "id": "Perawat Neonatal, Bekerja Di NICU (Neonatal Intensive Care Unit)." },
{ "en": "Apa Alat Untuk Mengukur Tekanan Darah?", "id": "Sfigmomanometer, Atau Dikenal Dengan Tensi Meter." },
{ "en": "Apa Hormon Yang Dihasilkan Sel Delta Pankreas?", "id": "Somatostatin, Menghambat Pelepasan Hormon Lain." },
{ "en": "Jenis Nekrosis Akibat Infeksi Bakteri Bernanah?", "id": "Nekrosis Likuefaktif, Jaringan Menjadi Cair." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Tidur?", "id": "Somnolog Atau Ahli Kedokteran Tidur." },
{ "en": "Organ Apa Yang Memproduksi Angiotensinogen?", "id": "Hati, Prekursor Dalam Sistem RAAS." },
{ "en": "Apa Enzim Yang Mengubah Angiotensin I?", "id": "ACE Singkatan (Angiotensin-Converting Enzyme)." },
{ "en": "Istilah Medis Untuk Nyeri Otot Adalah?", "id": "Mialgia, Rasa Sakit Pada Jaringan Otot." },
{ "en": "Siapa Yang Membantu Pasien Memilih Makanan?", "id": "Teknisi Diet Atau Dietetic Technician." },
{ "en": "Dimana Lokasi Produksi Enzim ACE Singkatan (Angiotensin-Converting Enzyme) Terbanyak?", "id": "Di Paru-Paru Manusia Dewasa Sehat." },
{ "en": "Apa Reseptor Yang Mendeteksi Perubahan Kimia?", "id": "Kemoreseptor, Seperti Pada Kuncup Pengecap." },
{ "en": "Istilah Medis Untuk Sendawa Berlebihan Adalah?", "id": "Eruktasi, Pelepasan Gas Dari Lambung." },
{ "en": "Spesialis Yang Menangani Masalah Kaki Diabetes?", "id": "Podiatris Dan Perawat Luka Kaki Diabetes." },
{ "en": "Apa Reseptor Yang Mendeteksi Perubahan Tekanan?", "id": "Mekanoreseptor, Terdapat Pada Kulit Telinga." },
{ "en": "Apa Sel Yang Menghasilkan Asam Lambung?", "id": "Sel Parietal, Menghasilkan Asam Klorida (HCl)." },
{ "en": "Istilah Medis Untuk Buang Gas Kentut?", "id": "Flatulensi, Keluarnya Gas Dari Anus." },
{ "en": "Dokter Spesialis Yang Fokus Pada Vaksin?", "id": "Vaksinolog, Ahli Dalam Pengembangan Vaksin." },
{ "en": "Apa Reseptor Yang Mendeteksi Suhu Panas Dingin?", "id": "Termoreseptor, Tersebar Di Seluruh Kulit." },
{ "en": "Apa Sel Yang Menghasilkan Pepsinogen Lambung?", "id": "Sel Chief, Prekursor Enzim Pepsin." },
{ "en": "Kondisi Peradangan Pada Kantung Mata Adalah?", "id": "Konjungtivitis, Atau Dikenal Mata Merah." },
{ "en": "Ahli Yang Mempelajari Penyakit Lewat Darah?", "id": "Patolog Klinis, Menganalisis Cairan Tubuh." },
{ "en": "Apa Jenis Kekebalan Yang Diperantarai Sel?", "id": "Imunitas Seluler, Diperankan Oleh Sel T." },
{ "en": "Apa Hormon Yang Merangsang Rasa Lapar?", "id": "Ghrelin, Diproduksi Oleh Organ Lambung." },
{ "en": "Kondisi Penyempitan Katup Jantung Aorta Adalah?", "id": "Stenosis Aorta, Menghambat Aliran Darah." },
{ "en": "Perawat Dengan Gelar Praktik Tingkat Lanjut?", "id": "Perawat Praktisi Atau Nurse Practitioner (NP)." },
{ "en": "Apa Jenis Kekebalan Yang Diperantarai Antibodi?", "id": "Imunitas Humoral, Diperankan Oleh Sel B." },
{ "en": "Apa Hormon Yang Menekan Rasa Lapar?", "id": "Leptin, Diproduksi Oleh Sel Jaringan Lemak." },
{ "en": "Kondisi Katup Mitral Tidak Menutup Rapat?", "id": "Regurgitasi Mitral, Darah Bocor Kembali." },
{ "en": "Ahli Anestesi Yang Berspesialisasi Nyeri Adalah?", "id": "Anestesiolog Nyeri Atau Pain Management Anesthesiologist." },
{ "en": "Apa Bagian Dari Sistem Saraf Sadar?", "id": "Sistem Saraf Somatik, Mengontrol Gerakan Volunter." },
{ "en": "Apa Cairan Bening Yang Mengisi Bola Mata?", "id": "Vitreous Humour, Menjaga Bentuk Bola Mata." },
{ "en": "Peradangan Pada Testis Pria Disebut Apa?", "id": "Orkitis, Sering Disebabkan Oleh Infeksi." },
{ "en": "Dokter Yang Mengkhususkan Diri Kedokteran Remaja?", "id": "Spesialis Kedokteran Remaja Atau Adolescent Medicine Specialist." },
{ "en": "Apa Bagian Sistem Saraf Tak Sadar?", "id": "Sistem Saraf Otonom, Mengontrol Fungsi Involunter." },
{ "en": "Apa Titik Buta Pada Retina Mata?", "id": "Cakram Optik, Tempat Saraf Optik Keluar." },
{ "en": "Peradangan Pada Epididimis Testis Disebut Apa?", "id": "Epididimitis, Penyebab Umum Nyeri Testis." },
{ "en": "Ahli Yang Mempelajari Mekanisme Penyakit Adalah?", "id": "Patofisiolog, Fokus Pada Perubahan Fungsi." },
{ "en": "Apa Refleks Otomatis Pupil Terhadap Cahaya?", "id": "Refleks Cahaya Pupil, Pupil Menyempit." },
{ "en": "Apa Titik Penglihatan Paling Tajam Retina?", "id": "Fovea, Kaya Akan Sel Kerucut." },
{ "en": "Penyakit Kriptorkismus Adalah Kegagalan Organ Apa?", "id": "Kegagalan Testis Turun Ke Skrotum." },
{ "en": "Siapa Yang Menjalankan Tes Fungsi Saraf?", "id": "Teknolog Konduksi Saraf Atau Nerve Conduction Technologist." },
{ "en": "Apa Saraf Yang Mengontrol Gerakan Mata?", "id": "Saraf Okulomotor, Troklear, Dan Abdusen." },
{ "en": "Apa Proses Pencernaan Kimiawi Di Mulut?", "id": "Amilase Saliva Memecah Karbohidrat Kompleks." },
{ "en": "Kondisi Varikokel Adalah Pembengkakan Vena Dimana?", "id": "Pembengkakan Vena Di Dalam Skrotum." },
{ "en": "Dokter Spesialis Penyakit Menular Seksual (PMS)?", "id": "Spesialis Venereologi Atau Dokter PMS." },
{ "en": "Apa Saraf Yang Mengontrol Otot Lidah?", "id": "Saraf Hipoglosus (Saraf Kranial XII)." },
{ "en": "Apa Gelombang Lambat Kontraksi Otot Perut?", "id": "Gerak Peristaltik, Mencampur Makanan (Chyme)." },
{ "en": "Kondisi Peradangan Pada Kelenjar Prostat Adalah?", "id": "Prostatitis, Bisa Akut Maupun Kronis." },
{ "en": "Ahli Yang Mempelajari Obat Dari Tumbuhan?", "id": "Farmakognosis, Ilmu Bahan Obat Alami." },
{ "en": "Apa Bagian Sistem Saraf 'Lawan Lari'?", "id": "Sistem Saraf Simpatik, Respons Stres." },
{ "en": "Apa Bagian Sistem Saraf 'Istirahat Cerna'?", "id": "Sistem Saraf Parasimpatik, Kondisi Relaksasi." },
{ "en": "Penyakit Peyronie Adalah Kelainan Pada Organ?", "id": "Kelainan Bentuk Penis Akibat Plak." },
{ "en": "Ahli Yang Mempelajari Asal Usul Penyakit?", "id": "Etiolog, Fokus Pada Penyebab Penyakit." },
{ "en": "Apa Proses Penyalinan DNA Ke RNA?", "id": "Transkripsi, Terjadi Di Dalam Inti Sel." },
{ "en": "Apa Sel Imun Spesifik Di Otak?", "id": "Mikroglia, Makrofag Sistem Saraf Pusat." },
{ "en": "Gen Yang Memicu Pertumbuhan Kanker Adalah?", "id": "Onkogen, Versi Mutasi Dari Proto-Onkogen." },
{ "en": "Siapa Ahli Yang Membuat Mata Tiruan?", "id": "Okularis Atau Dikenal Sebagai Ocularist Profesional." },
{ "en": "Teknik Pemeriksaan Mendengarkan Suara Tubuh Adalah?", "id": "Auskultasi, Menggunakan Stetoskop Untuk Mendengar." },
{ "en": "Apa Proses Sintesis Protein Dari RNA?", "id": "Translasi, Terjadi Di Ribosom Sel." },
{ "en": "Apa Sel Imun Spesifik Di Hati?", "id": "Sel Kupffer, Makrofag Residen Hati." },
{ "en": "Siapa Ahli Yang Membuat Prostetik Wajah?", "id": "Anaplastolog, Spesialis Prostetik Maksilofasial Terlatih." },
{ "en": "Jenis RNA Apa Pembawa Kode Genetik?", "id": "RNA Duta Atau mRNA (Messenger RNA)." },
{ "en": "Apa Bagian Otak Untuk Memori Spasial?", "id": "Hipokampus, Penting Untuk Navigasi, Memori." },
{ "en": "Gen Penekan Tumor Paling Terkenal Adalah?", "id": "Gen P53, Penjaga Genom Manusia." },
{ "en": "Ahli Informatika Yang Bekerja Di Klinik?", "id": "Spesialis Informatika Klinis Atau Clinical Informaticist." },
{ "en": "Teknik Pemeriksaan Mengetuk Permukaan Tubuh Adalah?", "id": "Perkusi, Menilai Organ Di Bawahnya." },
{ "en": "Jenis RNA Apa Pembawa Asam Amino?", "id": "RNA Transfer Atau tRNA (Transfer RNA)." },
{ "en": "Apa Bagian Otak Pengatur Kewaspadaan Tidur?", "id": "Formasi Retikuler Di Batang Otak." },
{ "en": "Siapa Yang Menulis Proposal Hibah Penelitian?", "id": "Penulis Hibah Atau Grant Writer." },
{ "en": "Istilah 'Segera' Dalam Instruksi Medis Adalah?", "id": "STAT Singkatan (Statim), Berarti Seketika." },
{ "en": "Jenis RNA Apa Komponen Utama Ribosom?", "id": "RNA Ribosomal Atau rRNA (Ribosomal RNA)." },
{ "en": "Apa Kelompok Inti Saraf Pengatur Gerakan?", "id": "Ganglia Basal, Terletak Di Dasar Otak." },
{ "en": "Siapa Yang Menganalisis Kebijakan Sektor Kesehatan?", "id": "Analis Kebijakan Kesehatan (Health Policy Analyst)." },
{ "en": "Istilah 'Puasa' Dalam Instruksi Medis Adalah?", "id": "NPO Singkatan (Nil Per Os), Tanpa Makan." },
{ "en": "Tiga Basa Nukleotida Pada mRNA Disebut?", "id": "Kodon, Mengkode Satu Asam Amino." },
{ "en": "Apa Stasiun Pemancar Informasi Sensorik Otak?", "id": "Talamus, Meneruskan Sinyal Ke Korteks." },
{ "en": "Koordinator Tanggap Darurat Bencana Medis Adalah?", "id": "Koordinator Kesiapsiagaan Bencana Atau Disaster Coordinator." },
{ "en": "Istilah 'Sesuai Kebutuhan' Dalam Resep Adalah?", "id": "PRN Singkatan (Pro Re Nata)." },
{ "en": "Apa Proses Pembentukan Lempeng Saraf Embrio?", "id": "Neurulasi, Awal Pembentukan Sistem Saraf." },
{ "en": "Apa Zat Yang Memblokir Reseptor Sepenuhnya?", "id": "Antagonis Kompetitif, Menghambat Aksi Agonis." },
{ "en": "Dokter Spesialis Yang Menangani Keracunan Adalah?", "id": "Toksikolog Medis Atau Medical Toxicologist." },
{ "en": "Teknik Pemeriksaan Menggunakan Tangan Perabaan Adalah?", "id": "Palpasi, Merasakan Ukuran, Bentuk, Tekstur." },
{ "en": "Apa Proses Pembentukan Tiga Lapisan Germinal?", "id": "Gastrulasi, Tahap Kritis Perkembangan Embrio." },
{ "en": "Apa Zat Mengaktifkan Reseptor Sebagian Saja?", "id": "Agonis Parsial, Respon Lebih Lemah." },
{ "en": "Ahli Yang Mempelajari Penyakit Pada Populasi?", "id": "Epidemiolog, Melacak Pola, Penyebab Penyakit." },
{ "en": "Bunyi Jantung Abnormal Yang Terdengar Stetoskop?", "id": "Murmur Jantung, Akibat Aliran Darah Turbulen." },
{ "en": "Apa Proses Duplikasi Seluruh DNA Genom?", "id": "Replikasi DNA, Terjadi Sebelum Pembelahan Sel." },
{ "en": "Metabolisme Obat Pertama Kali Di Hati?", "id": "Metabolisme Lintas Pertama (First-Pass Metabolism)." },
{ "en": "Dokter Gigi Spesialis Pencitraan Rahang Wajah?", "id": "Radiolog Oral Dan Maksilofasial Profesional." },
{ "en": "Bunyi Napas Tambahan Seperti Retakan Kering?", "id": "Rales Atau Krepitasi, Tanda Cairan Paru." },
{ "en": "Hasil Akhir Glikolisis Anaerob Otot Adalah?", "id": "Asam Laktat, Menyebabkan Kelelahan Otot." },
{ "en": "Apa Sel Yang Menghasilkan Testosteron Testis?", "id": "Sel Leydig, Terletak Antara Tubulus." },
{ "en": "Penyakit Akibat Kekurangan Enzim G6PD Adalah?", "id": "Defisiensi G6PD, Menyebabkan Anemia Hemolitik." },
{ "en": "Ahli Patologi Yang Fokus Pada Mulut?", "id": "Patolog Oral, Mendiagnosis Penyakit Mulut." },
{ "en": "Bunyi Napas Tambahan Seperti Siulan Keras?", "id": "Rhonchi, Tanda Penyempitan Saluran Napas." },
{ "en": "Apa Sel Yang Memberi Nutrisi Sperma?", "id": "Sel Sertoli, Di Dalam Tubulus Seminiferus." },
{ "en": "Kondisi Kelebihan Zat Besi Dalam Tubuh?", "id": "Hemokromatosis, Dapat Merusak Organ Hati." },
{ "en": "Ahli Yang Mempelajari Interaksi Manusia Komputer?", "id": "Spesialis Faktor Manusia Atau Human Factors." },
{ "en": "Proses Pemecahan Asam Lemak Menjadi Energi?", "id": "Beta-Oksidasi, Terjadi Di Dalam Mitokondria." },
{ "en": "Apa Jaringan Yang Menghasilkan Cairan Sinovial?", "id": "Membran Sinovial, Melapisi Kapsul Sendi." },
{ "en": "Kondisi Kelebihan Tembaga Dalam Tubuh Adalah?", "id": "Penyakit Wilson, Mempengaruhi Hati, Otak." },
{ "en": "Dokter Yang Mengkhususkan Diri Kedokteran Nuklir?", "id": "Spesialis Kedokteran Nuklir Atau Nuclear Medicine Physician." },
{ "en": "Proses Pengeluaran Keringat Dari Kelenjar Disebut?", "id": "Perspirasi, Untuk Mengatur Suhu Tubuh." },
{ "en": "Apa Hormon Yang Merangsang Pelepasan Kortisol?", "id": "ACTH Singkatan (Adrenocorticotropic Hormone)." },
{ "en": "Kondisi Pembesaran Jantung Secara Abnormal Adalah?", "id": "Kardiomegali, Bukan Penyakit Tapi Tanda." },
{ "en": "Siapa Yang Merawat Pasien Sebelum Masuk RS?", "id": "Petugas Medis Gawat Darurat Atau Paramedis." },
{ "en": "Proses Penghancuran Sel Darah Merah Tua?", "id": "Eritoptosis, Terjadi Terutama Di Limpa." },
{ "en": "Hormon Apa Yang Merangsang Kelenjar Tiroid?", "id": "TSH Singkatan (Thyroid-Stimulating Hormone)." },
{ "en": "Kondisi Pembesaran Hati Secara Abnormal Adalah?", "id": "Hepatomegali, Tanda Penyakit Hati Mendasar." },
{ "en": "Ahli Yang Mengelola Program Imunisasi Publik?", "id": "Manajer Program Imunisasi Atau Immunization Program Manager." },
{ "en": "Apa Gerakan Menjauh Dari Garis Tengah Tubuh?", "id": "Abduksi, Contohnya Mengangkat Lengan Samping." },
{ "en": "Hormon Apa Yang Merangsang Perkembangan Seksual?", "id": "Gonadotropin (LH Dan FSH)." },
{ "en": "Kondisi Pembesaran Limpa Secara Abnormal Adalah?", "id": "Splenomegali, Tanda Infeksi Atau Penyakit." },
{ "en": "Spesialis Pengobatan Yang Fokus Pada Adiksi?", "id": "Dokter Pengobatan Adiksi Atau Addiction Medicine Physician." },
{ "en": "Apa Gerakan Menuju Garis Tengah Tubuh?", "id": "Aduksi, Contohnya Menurunkan Lengan Samping." },
{ "en": "Apa Hormon Pelepas Gonadotropin Dari Hipotalamus?", "id": "GnRH Singkatan (Gonadotropin-Releasing Hormone)." },
{ "en": "Kondisi Peradangan Pada Pembuluh Darah Adalah?", "id": "Vaskulitis, Dapat Mempengaruhi Berbagai Organ." },
{ "en": "Siapa Yang Menjalankan Tes Laboratorium Kompleks?", "id": "Ilmuwan Laboratorium Medis (Medical Laboratory Scientist)." },
{ "en": "Apa Gerakan Meluruskan Sendi Atau Ekstensi?", "id": "Ekstensi, Meningkatkan Sudut Antara Tulang." },
{ "en": "Apa Pusat Saraf Untuk Rasa Kenyang?", "id": "Nukleus Ventromedial Di Bagian Hipotalamus." },
{ "en": "Kondisi Peradangan Pada Otot Rangka Adalah?", "id": "Miositis, Bisa Disebabkan Infeksi, Autoimun." },
{ "en": "Spesialis Yang Menangani Masalah Kesehatan Remaja?", "id": "Dokter Kedokteran Remaja (Adolescent Medicine Specialist)." },
{ "en": "Apa Gerakan Menekuk Sendi Atau Fleksi?", "id": "Fleksi, Mengurangi Sudut Antara Tulang." },
{ "en": "Apa Pusat Saraf Untuk Rasa Lapar?", "id": "Hipotalamus Lateral, Merangsang Nafsu Makan." },
{ "en": "Kondisi Penumpukan Cairan Di Rongga Perut?", "id": "Asites, Sering Terkait Penyakit Hati." },
{ "en": "Dokter Yang Mengkhususkan Diri Perawatan Hospis?", "id": "Dokter Hospis Dan Perawatan Paliatif." },
{ "en": "Gerakan Memutar Telapak Tangan Ke Atas?", "id": "Supinasi, Seperti Memegang Mangkuk Sup." },
{ "en": "Apa Enzim Pengurai Hidrogen Peroksida Sel?", "id": "Katalase, Melindungi Sel Dari Kerusakan." },
{ "en": "Kondisi Penumpukan Cairan Di Sekitar Paru?", "id": "Efusi Pleura, Cairan Dalam Rongga Pleura." },
{ "en": "Ahli Yang Mempelajari Distribusi Penyakit Adalah?", "id": "Ahli Epidemiologi, Melacak Wabah Penyakit." },
{ "en": "Gerakan Memutar Telapak Tangan Ke Bawah?", "id": "Pronasi, Kebalikan Dari Gerakan Supinasi." },
{ "en": "Apa Protein Struktural Utama Jaringan Ikat?", "id": "Kolagen, Protein Paling Melimpah Tubuh." },
{ "en": "Kondisi Kolaps Sebagian Atau Seluruh Paru?", "id": "Atelektasis, Alveoli Mengempis Tidak Berfungsi." },
{ "en": "Spesialis Yang Mengelola Program Kesehatan Global?", "id": "Praktisi Kesehatan Global Atau Global Health Practitioner." },
{ "en": "Gerakan Mengangkat Bahu Ke Arah Atas?", "id": "Elevasi, Contohnya Mengangkat Tulang Belikat." },
{ "en": "Apa Protein Elastis Di Kulit Arteri?", "id": "Elastin, Memberikan Sifat Elastisitas Jaringan." },
{ "en": "Istilah Untuk Pengeluaran Dahak Dari Paru?", "id": "Ekspektorasi, Batuk Berdahak Atau Sputum." },
{ "en": "Ahli Yang Mempelajari Penyakit Zoonosis Adalah?", "id": "Dokter Hewan Kesehatan Masyarakat Veteriner." },
{ "en": "Gerakan Menurunkan Bahu Ke Arah Bawah?", "id": "Depresi, Kebalikan Dari Gerakan Elevasi." },
{ "en": "Apa Pigmen Pernapasan Di Jaringan Otot?", "id": "Mioglobin, Mengikat Oksigen Di Sel Otot." },
{ "en": "Istilah Medis Untuk Batuk Darah Adalah?", "id": "Hemoptisis, Tanda Kondisi Medis Serius." },
{ "en": "Spesialis Pengobatan Lingkungan Dan Kerja Adalah?", "id": "Dokter Kedokteran Okupasi Dan Lingkungan." },
{ "en": "Apa Lapisan Terluar Dari Pembuluh Arteri?", "id": "Tunika Adventitia, Jaringan Ikat Fibrosa." },
{ "en": "Apa Reseptor Panas Dingin Di Kulit?", "id": "Termoreseptor, Mendeteksi Perubahan Suhu Lingkungan." },
{ "en": "Istilah Medis Untuk Kesulitan Bernapas Berbaring?", "id": "Ortopnea, Gejala Gagal Jantung Kongestif." },
{ "en": "Ahli Fisioterapi Yang Fokus Pada Jantung?", "id": "Fisioterapis Kardiopulmoner Atau Cardiopulmonary Physiotherapist." },
{ "en": "Apa Proses Penguraian Lemak Menjadi Energi?", "id": "Lipolisis, Diikuti Dengan Proses Beta-Oksidasi." },
{ "en": "Apa Sel Yang Menghasilkan Cairan Serebrospinal?", "id": "Sel Ependimal, Melapisi Ventrikel Otak." },
{ "en": "Istilah Medis Untuk Kehilangan Nafsu Makan?", "id": "Anoreksia, Bukan Gangguan Makan Anoreksia Nervosa." },
{ "en": "Siapa Yang Merawat Pasien Lanjut Usia?", "id": "Perawat Gerontologi Atau Gerontological Nurse." },
{ "en": "Apa Proses Penguraian Protein Menjadi Energi?", "id": "Deaminasi, Melepaskan Gugus Amino Asam." },
{ "en": "Apa Enzim Pemecah Ikatan Peptida Protein?", "id": "Protease, Seperti Pepsin, Tripsin, Kimotripsin." },
{ "en": "Istilah Medis Untuk Suara Serak Parau?", "id": "Disfonia, Gangguan Pada Pita Suara." },
{ "en": "Ahli Yang Mempelajari Penyakit Lewat Epidemi?", "id": "Epidemiolog Lapangan Atau Field Epidemiologist." },
{ "en": "Apa Hormon Yang Dihasilkan Kelenjar Paratiroid?", "id": "Hormon Paratiroid (PTH), Mengatur Kalsium." },
{ "en": "Apa Enzim Pemecah Sukrosa Menjadi Glukosa?", "id": "Sukrase, Terdapat Di Dinding Usus." },
{ "en": "Istilah Medis Untuk Penglihatan Ganda Adalah?", "id": "Diplopia, Melihat Dua Gambar Objek." },
{ "en": "Dokter Gigi Spesialis Penyakit Mulut Adalah?", "id": "Spesialis Penyakit Mulut Atau Oral Medicine." },
{ "en": "Apa Hormon Yang Dihasilkan Sel Lemak?", "id": "Leptin, Mengatur Nafsu Makan, Energi." },
{ "en": "Apa Enzim Pemecah Maltosa Menjadi Glukosa?", "id": "Maltase, Memecah Gula Malt Menjadi Glukosa." },
{ "en": "Istilah Medis Untuk Mata Juling Adalah?", "id": "Strabismus, Ketidaksejajaran Posisi Kedua Mata." },
{ "en": "Ahli Yang Menangani Terapi Perilaku Hewan?", "id": "Dokter Hewan Perilaku Atau Veterinary Behaviorist." },
{ "en": "Struktur Sel Apa Yang Menghasilkan Energi?", "id": "Mitokondria, 'Pembangkit Tenaga' Seluler Manusia." },
{ "en": "Apa Enzim Pemecah DNA Asam Nukleat?", "id": "Deoksiribonuklease, Memecah Molekul DNA Kompleks." },
{ "en": "Istilah Medis Untuk Gerakan Mata Cepat?", "id": "Nistagmus, Gerakan Mata Involunter Berirama." },
{ "en": "Perawat Yang Membantu Proses Pembedahan Adalah?", "id": "Perawat Perioperatif Atau Perawat Kamar Bedah." },
{ "en": "Apa Struktur Sel Yang Mencerna Limbah?", "id": "Lisosom, 'Sistem Daur Ulang' Sel." },
{ "en": "Apa Enzim Pemecah RNA Asam Nukleat?", "id": "Ribonuklease, Memecah Molekul RNA Menjadi Nukleotida." },
{ "en": "Istilah Medis Untuk Keluarnya Air Liur?", "id": "Sialorea, Produksi Air Liur Berlebihan." },
{ "en": "Spesialis Pengobatan Yang Bekerja Di Pedalaman?", "id": "Dokter Daerah Terpencil Atau Remote Area Doctor." },
{ "en": "Apa Zat Yang Memberi Warna Pada Empedu?", "id": "Bilirubin, Produk Pemecahan Sel Darah." },
{ "en": "Apa Saraf Kranial Pendengaran Keseimbangan Manusia?", "id": "Saraf Vestibulokoklear (Saraf Kranial VIII)." },
{ "en": "Istilah Medis Untuk Rasa Haus Berlebihan?", "id": "Polidipsia, Seringkali Gejala Diabetes Mellitus." },
{ "en": "Ahli Patologi Yang Fokus Sistem Saraf?", "id": "Neuropatolog, Mendiagnosis Penyakit Otak Saraf." },
{ "en": "Apa Zat Yang Memberi Warna Pada Feses?", "id": "Sterkobilin, Turunan Dari Bilirubin Hati." },
{ "en": "Apa Saraf Kranial Pengecapan Belakang Lidah?", "id": "Saraf Glosofaringeal (Saraf Kranial IX)." },
{ "en": "Istilah Medis Untuk Rasa Lapar Berlebihan?", "id": "Polifagia, Nafsu Makan Meningkat Drastis." },
{ "en": "Perawat Yang Melakukan Kunjungan Rumah Pasien?", "id": "Perawat Kesehatan Rumah Atau Home Health Nurse." },
{ "en": "Apa Zat Yang Memberi Warna Urine?", "id": "Urokrom, Pigmen Kuning Dari Hemoglobin." },
{ "en": "Apa Saraf Kranial Mengontrol Otot Leher?", "id": "Saraf Asesori (Saraf Kranial XI)." },
{ "en": "Istilah Medis Produksi Urine Berlebih Adalah?", "id": "Poliuria, Sering Buang Air Kecil." },
{ "en": "Spesialis Penyakit Kulit Yang Menangani Anak?", "id": "Dermatolog Pediatrik Atau Pediatric Dermatologist." },
{ "en": "Apa Bagian Mata Yang Tidak Peka Cahaya?", "id": "Bintik Buta, Tempat Saraf Optik." },
{ "en": "Apa Bagian Otak Untuk Pemahaman Bahasa?", "id": "Area Wernicke, Di Lobus Temporal." },
{ "en": "Istilah Untuk Produksi Air Mata Berlebih?", "id": "Epifora, Mata Terus-Menerus Berair Keluar." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Kesuburan Pria?", "id": "Androlog, Ahli Sistem Reproduksi Pria." },
{ "en": "Apa Bagian Mata Untuk Penglihatan Sentral?", "id": "Makula, Bagian Tengah Dari Retina." },
{ "en": "Apa Bagian Otak Untuk Produksi Bahasa?", "id": "Area Broca, Di Lobus Frontal." },
{ "en": "Istilah Untuk Peningkatan Jumlah Sel Darah?", "id": "Polisitemia, Membuat Darah Menjadi Lebih Kental." },
{ "en": "Ahli Yang Mengelola Program Pencegahan Penyakit?", "id": "Spesialis Pencegahan Penyakit Atau Prevention Specialist." },
{ "en": "Apa Itu Kekebalan Yang Dibawa Sejak Lahir?", "id": "Kekebalan Bawaan Atau Imunitas Inate." },
{ "en": "Apa Bagian Dari Struktur DNA Manusia?", "id": "Nukleotida (Gula, Fosfat, Basa Nitrogen)." },
{ "en": "Istilah Untuk Penurunan Jumlah Trombosit Adalah?", "id": "Trombositopenia, Meningkatkan Risiko Perdarahan Abnormal." },
{ "en": "Siapa Yang Melakukan Terapi Kelompok Dukungan?", "id": "Fasilitator Kelompok Dukungan Atau Terapis." },
{ "en": "Apa Itu Kekebalan Yang Didapat Seumur Hidup?", "id": "Kekebalan Adaptif, Spesifik Terhadap Patogen." },
{ "en": "Apa Basa Nitrogen Purin Dalam DNA?", "id": "Adenin (A) Dan Guanin (G)." },
{ "en": "Istilah Untuk Peningkatan Jumlah Eosinofil Adalah?", "id": "Eosinofilia, Sering Terkait Alergi, Parasit." },
{ "en": "Ahli Yang Menangani Terapi Realitas Virtual?", "id": "Terapis Realitas Virtual Atau VR Therapist." },
{ "en": "Sel Apa Yang Menyerang Sel Kanker?", "id": "Sel Pembunuh Alami (Natural Killer Cell)." },
{ "en": "Apa Basa Nitrogen Pirimidin Dalam DNA?", "id": "Sitosin (C) Dan Timin (T)." },
{ "en": "Istilah Penurunan Jumlah Semua Sel Darah?", "id": "Pansitopenia, Kegagalan Fungsi Sumsum Tulang." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Remaja?", "id": "Dokter Kedokteran Remaja Atau Adolescent Medicine." },
{ "en": "Sel Imun Apa Yang Mengingat Infeksi Lalu?", "id": "Sel Memori, Memberikan Respons Cepat." },
{ "en": "Apa Basa Nitrogen Pirimidin Dalam RNA?", "id": "Sitosin (C) Dan Urasil (U)." },
{ "en": "Istilah Untuk Kehadiran Bakteri Dalam Darah?", "id": "Bakteremia, Dapat Menyebabkan Sepsis Parah." },
{ "en": "Siapa Yang Menulis Artikel Jurnal Ilmiah?", "id": "Penulis Ilmiah Atau Scientific Writer." },
{ "en": "Apa Proses Pindah Silang Materi Genetik?", "id": "Rekombinasi Genetik, Terjadi Saat Meiosis." },
{ "en": "Apa Enzim Yang Membuka Heliks Ganda DNA?", "id": "Helikase, Memisahkan Dua Untai DNA." },
{ "en": "Istilah Untuk Respon Peradangan Seluruh Tubuh?", "id": "Sepsis, Respon Imun Berbahaya Infeksi." },
{ "en": "Ahli Yang Mengelola Informasi Kesehatan Pasien?", "id": "Teknisi Informasi Kesehatan Atau Health Information Technician." },
{ "en": "Apa Itu Mutasi Satu Basa Nukleotida?", "id": "Mutasi Titik, Dapat Mengubah Protein." },
{ "en": "Apa Enzim Yang Membangun Untai DNA Baru?", "id": "DNA Polimerase, Mensintesis DNA Baru." },
{ "en": "Istilah Untuk Penumpukan Cairan Di Otak?", "id": "Hidrosefalus, Peningkatan Tekanan Intrakranial Otak." },
{ "en": "Spesialis Pengobatan Yang Bekerja Di Laut?", "id": "Dokter Kelautan Atau Dokter Kapal Pesiar." },
{ "en": "Apa Itu Gen Yang Gagal Memisah?", "id": "Nondisjunction, Menyebabkan Kelainan Jumlah Kromosom." },
{ "en": "Apa Enzim Yang Menggabungkan Fragmen DNA?", "id": "Ligase, 'Lem' Molekuler Untuk DNA." },
{ "en": "Istilah Untuk Pengangkatan Ovarium Secara Bedah?", "id": "Oophorektomi, Prosedur Bedah Pengangkatan Ovarium." },
{ "en": "Siapa Yang Merancang Ruang Perawatan Kesehatan?", "id": "Arsitek Perawatan Kesehatan Atau Healthcare Architect." },
{ "en": "Apa Proses Pembentukan Pembuluh Darah Baru?", "id": "Vaskulogenesis (Embrio), Angiogenesis (Dewasa)." },
{ "en": "Apa Sel Yang Menghasilkan Serat Jaringan?", "id": "Fibroblas, Mensintesis Kolagen, Jaringan Ikat." },
{ "en": "Istilah Pengangkatan Saluran Tuba Fallopi Bedah?", "id": "Salpingektomi, Prosedur Bedah Pengangkatan Tuba." },
{ "en": "Ahli Yang Mengelola Kualitas Rumah Sakit?", "id": "Manajer Peningkatan Kualitas Atau Quality Improvement Manager." },
{ "en": "Apa Proses Kematian Jaringan Akibat Dingin?", "id": "Frostbite, Pembekuan Jaringan Kulit Bawah." },
{ "en": "Apa Sel Yang Menghasilkan Histamin Alergi?", "id": "Sel Mast Dan Basofil Darah." },
{ "en": "Istilah Untuk Pengangkatan Prostat Secara Bedah?", "id": "Prostatektomi, Prosedur Bedah Pengangkatan Prostat." },
{ "en": "Dokter Yang Mengkhususkan Diri Kedokteran Luar Angkasa?", "id": "Dokter Penerbangan Atau Flight Surgeon." },
{ "en": "Apa Jaringan Yang Menopang Epitel Manusia?", "id": "Membran Basal Atau Lamina Basal." },
{ "en": "Apa Jenis Kelenjar Tanpa Saluran Khusus?", "id": "Kelenjar Endokrin, Melepaskan Hormon Darah." },
{ "en": "Istilah Untuk Pengangkatan Paru-Paru Secara Bedah?", "id": "Pneumonektomi, Prosedur Bedah Pengangkatan Paru." },
{ "en": "Ahli Yang Mempelajari Perilaku Kesehatan Masyarakat?", "id": "Ilmuwan Perilaku Kesehatan Atau Behavioral Scientist." },
{ "en": "Jaringan Apa Yang Memiliki Kemampuan Kontraksi?", "id": "Jaringan Otot (Polos, Lurik, Jantung)." },
{ "en": "Apa Jenis Kelenjar Dengan Saluran Khusus?", "id": "Kelenjar Eksokrin, Contohnya Kelenjar Keringat." },
{ "en": "Istilah Untuk Pengangkatan Ginjal Secara Bedah?", "id": "Nefrektomi, Prosedur Bedah Pengangkatan Ginjal." },
{ "en": "Spesialis Yang Menangani Masalah Kesehatan Wanita?", "id": "Ginekolog, Ahli Sistem Reproduksi Wanita." },
{ "en": "Mekanisme Ginjal Untuk Memekatkan Urine Adalah?", "id": "Mekanisme Penggandaan Berlawanan Arus (Countercurrent)." },
{ "en": "Apa Efek CO2 Pada Afinitas Oksigen Hemoglobin?", "id": "Efek Bohr, Menurunkan Afinitas Oksigen." },
{ "en": "Penyakit Akibat Perawatan Medis Disebut Apa?", "id": "Penyakit Iatrogenik, Disebabkan Oleh Intervensi." },
{ "en": "Siapa Spesialis Terapi Fisik Untuk Tangan?", "id": "Terapis Tangan Bersertifikat (Certified Hand Therapist)." },
{ "en": "Celah Pada Selubung Mielin Saraf Adalah?", "id": "Nodus Ranvier, Tempat Konduksi Saltatori." },
{ "en": "Apa Tekanan Cairan Yang Mendorong Keluar Kapiler?", "id": "Tekanan Hidrostatik, Bagian Dari Gaya Starling." },
{ "en": "Penyakit Yang Tidak Diketahui Penyebabnya Adalah?", "id": "Penyakit Idiopatik, Penyebabnya Spontan, Tidak Jelas." },
{ "en": "Petugas Yang Membantu Pasien Menavigasi Sistem?", "id": "Navigator Pasien Atau Patient Navigator Profesional." },
{ "en": "Mekanisme Jantung Memompa Sesuai Volume Darah?", "id": "Mekanisme Frank-Starling, Semakin Meregang Semakin Kuat." },
{ "en": "Apa Efek Oksigen Pada Afinitas CO2 Hemoglobin?", "id": "Efek Haldane, Meningkatkan Pelepasan Karbon Dioksida." },
{ "en": "Kumpulan Gejala Yang Khas Suatu Penyakit?", "id": "Sindrom, Bukan Penyakit Dengan Etiologi Tunggal." },
{ "en": "Ahli Terapi Yang Menggunakan Gerakan Tubuh?", "id": "Kinesioterapis, Fokus Pada Rehabilitasi Gerakan." },
{ "en": "Apa Tekanan Protein Yang Menarik Cairan Kapiler?", "id": "Tekanan Onkotik Atau Tekanan Osmotik Koloid." },
{ "en": "Apa Jalur Saraf Motorik Dari Otak?", "id": "Traktus Kortikospinalis, Mengontrol Gerakan Sadar." },
{ "en": "Istilah Angka Kesakitan Dalam Populasi Adalah?", "id": "Morbiditas, Mengukur Tingkat Prevalensi Penyakit." },
{ "en": "Siapa Yang Menjalankan Mesin MRI Singkatan (Magnetic Resonance Imaging)?", "id": "Teknolog MRI Atau MRI Technologist." },
{ "en": "Apa Proses Lompatan Impuls Saraf Mielin?", "id": "Konduksi Saltatori, Mempercepat Transmisi Sinyal." },
{ "en": "Apa Volume Rata-Rata Sel Darah Merah?", "id": "MCV Singkatan (Mean Corpuscular Volume)." },
{ "en": "Istilah Angka Kematian Dalam Populasi Adalah?", "id": "Mortalitas, Mengukur Jumlah Kematian Tertentu." },
{ "en": "Siapa Yang Menjalankan Mesin CT Scan?", "id": "Teknolog CT Atau CT Technologist." },
{ "en": "Persentase Volume Sel Darah Merah Adalah?", "id": "Hematokrit, Indikator Anemia Atau Dehidrasi." },
{ "en": "Apa Sel Yang Membentuk Tulang Rawan?", "id": "Kondroblas, Berkembang Menjadi Kondrosit Matang." },
{ "en": "Kanker Tahap Awal Belum Menyebar Adalah?", "id": "Karsinoma In Situ (CIS)." },
{ "en": "Siapa Yang Menjalankan Mesin USG Singkatan (Ultrasonography)?", "id": "Sonografer Medis Diagnostik Atau Sonographer." },
{ "en": "Apa Jumlah Trombosit Rata-Rata Dalam Darah?", "id": "Hitung Trombosit, Menilai Risiko Pendarahan." },
{ "en": "Apa Sel Yang Membangun Matriks Tulang?", "id": "Osteoblas, Berperan Dalam Proses Osifikasi." },
{ "en": "Tingkat Kematangan Sel Kanker Disebut Apa?", "id": "Diferensiasi, (Baik, Sedang, Atau Buruk)." },
{ "en": "Siapa Yang Memastikan Dokumentasi Medis Akurat?", "id": "Spesialis Dokumentasi Klinis Atau Clinical Documentation Specialist." },
{ "en": "Apa Hormon Yang Dihasilkan Jantung Manusia?", "id": "Peptida Natriuretik Atrium Atau ANP (Atrial Natriuretik Peptide)." },
{ "en": "Apa Sel Yang Menghancurkan Matriks Tulang?", "id": "Osteoklas, Untuk Proses Resorpsi Tulang." },
{ "en": "Obat Yang Meniru Aksi Neurotransmitter Adalah?", "id": "Agonis, Meningkatkan Aktivitas Reseptor Saraf." },
{ "en": "Dokter Pemeriksa Kematian Yang Ditunjuk Pemerintah?", "id": "Koroner, Seringkali Pejabat Terpilih Resmi." },
{ "en": "Apa Hormon Yang Dihasilkan Jaringan Adiposa?", "id": "Adiponektin Dan Leptin, Mengatur Metabolisme." },
{ "en": "Apa Sel Punca Yang Belum Berdiferensiasi?", "id": "Sel Punca Embrionik, Bersifat Pluripoten." },
{ "en": "Obat Yang Memblokir Aksi Neurotransmitter Adalah?", "id": "Antagonis, Menghambat Aktivitas Reseptor Saraf." },
{ "en": "Dokter Forensik Yang Melakukan Otopsi Adalah?", "id": "Pemeriksa Medis Atau Medical Examiner." },
{ "en": "Apa Hormon Yang Dihasilkan Oleh Usus?", "id": "Ghrelin, Kolesistokinin (CCK), Dan Sekretin." },
{ "en": "Apa Sel Punca Di Jaringan Dewasa?", "id": "Sel Punca Dewasa, Bersifat Multipotensial." },
{ "en": "Istilah Untuk Penghentian Aliran Darah Normal?", "id": "Hemostasis, Proses Alami Menghentikan Pendarahan." },
{ "en": "Spesialis Yang Mengelola Informasi Data Klinis?", "id": "Manajer Data Klinis Atau Clinical Data Manager." },
{ "en": "Apa Tahap Pertama Proses Hemostasis Darah?", "id": "Vasokonstriksi, Penyempitan Pembuluh Darah Cepat." },
{ "en": "Apa Ruang Antara Dua Neuron Saraf?", "id": "Celah Sinaptik, Tempat Neurotransmitter Dilepaskan." },
{ "en": "Istilah Untuk Pemecahan Gumpalan Darah Adalah?", "id": "Fibrinolisis, Melarutkan Bekuan Darah Fibrin." },
{ "en": "Siapa Yang Merancang Dan Mengelola Biobank?", "id": "Manajer Biobank Atau Biobank Manager." },
{ "en": "Apa Tahap Kedua Proses Hemostasis Darah?", "id": "Pembentukan Sumbat Trombosit Primer Darah." },
{ "en": "Apa Potensial Listrik Neuron Saat Istirahat?", "id": "Potensial Istirahat, Sekitar -70 Milivolt." },
{ "en": "Enzim Apa Yang Melarutkan Bekuan Fibrin?", "id": "Plasmin, Bentuk Aktif Dari Plasminogen." },
{ "en": "Siapa Yang Menyelaraskan Lensa Kontak Pasien?", "id": "Pemasang Lensa Kontak Atau Contact Lens Fitter." },
{ "en": "Apa Tahap Ketiga Proses Hemostasis Darah?", "id": "Pembentukan Bekuan Fibrin (Koagulasi Sekunder)." },
{ "en": "Apa Puncak Potensial Aksi Sebuah Neuron?", "id": "Depolarisasi, Saat Ion Natrium Masuk." },
{ "en": "Penyakit Keturunan Kekurangan Enzim Fenilalanina Hidroksilase?", "id": "Fenilketonuria (PKU), Menyebabkan Kerusakan Otak." },
{ "en": "Ahli Yang Mempelajari Penyakit Akibat Kerja?", "id": "Ahli Epidemiologi Okupasi Atau Occupational Epidemiologist." },
{ "en": "Apa Proses Kembalinya Neuron Ke Istirahat?", "id": "Repolarisasi, Saat Ion Kalium Keluar." },
{ "en": "Apa Dua Jalur Aktivasi Kaskade Koagulasi?", "id": "Jalur Intrinsik Dan Jalur Ekstrinsik." },
{ "en": "Penyakit Keturunan Kekurangan Enzim Heksosaminidase A?", "id": "Penyakit Tay-Sachs, Merusak Sel Saraf." },
{ "en": "Spesialis Yang Menangani Masalah Kesehatan Pelancong?", "id": "Dokter Kedokteran Perjalanan Atau Travel Medicine Doctor." },
{ "en": "Periode Singkat Neuron Tidak Bisa Dipicu?", "id": "Periode Refraktori, Mencegah Sinyal Mundur." },
{ "en": "Faktor Koagulasi Apa Yang Memulai Jalur Ekstrinsik?", "id": "Faktor Jaringan Atau Faktor III." },
{ "en": "Kondisi Peradangan Pada Lapisan Perikardium Adalah?", "id": "Perikarditis, Menyebabkan Nyeri Dada Tajam." },
{ "en": "Siapa Yang Mengoperasikan Mesin Ekokardiogram Jantung?", "id": "Teknisi Ekokardiografi Atau Cardiac Sonographer." },
{ "en": "Apa Prinsip 'Semua Atau Tidak Sama Sekali'?", "id": "Potensial Aksi Tercapai Penuh Atau Tidak." },
{ "en": "Faktor Koagulasi Apa Yang Krusial Kalsium?", "id": "Faktor IV Adalah Ion Kalsium." },
{ "en": "Kondisi Peradangan Pada Lapisan Miokardium Adalah?", "id": "Miokarditis, Sering Disebabkan Infeksi Virus." },
{ "en": "Ahli Yang Mempelajari Gerakan Dan Mekanikanya?", "id": "Biomekanis Atau Biomechanist Profesional Medis." },
{ "en": "Penjumlahan Sinyal Input Pada Neuron Adalah?", "id": "Sumasi, Bisa Spasial Maupun Temporal." },
{ "en": "Apa Vitamin Yang Penting Untuk Koagulasi?", "id": "Vitamin K, Untuk Sintesis Faktor Koagulasi." },
{ "en": "Kondisi Peradangan Pada Lapisan Endokardium Adalah?", "id": "Endokarditis, Berisiko Merusak Katup Jantung." },
{ "en": "Siapa Yang Memberikan Dukungan Spiritual Pasien?", "id": "Penyedia Perawatan Spiritual Atau Chaplain." },
{ "en": "Apa Zat Yang Melapisi Alveoli Paru?", "id": "Surfaktan, Mencegah Kolaps Kantung Udara." },
{ "en": "Apa Protein Plasma Yang Dibuat Limfosit?", "id": "Globulin Gama Atau Imunoglobulin (Antibodi)." },
{ "en": "Kondisi Gagal Jantung Sisi Kanan Adalah?", "id": "Cor Pulmonale, Sering Akibat Penyakit Paru." },
{ "en": "Ahli Yang Menangani Terapi Melalui Berkuda?", "id": "Terapis Kuda Atau Hippotherapist Profesional." },
{ "en": "Apa Volume Udara Pernapasan Normal Manusia?", "id": "Volume Tidal, Sekitar 500 Mililiter." },
{ "en": "Apa Protein Plasma Yang Menjaga Tekanan Onkotik?", "id": "Albumin, Paling Melimpah Di Plasma." },
{ "en": "Penumpukan Cairan Di Paru-Paru Disebut Apa?", "id": "Edema Paru, Gejala Gagal Jantung Kiri." },
{ "en": "Siapa Yang Mengelola Terapi Rekreasi Pasien?", "id": "Spesialis Rekreasi Terapeutik Atau Therapeutic Recreation Specialist." },
{ "en": "Volume Udara Maksimal Dikeluarkan Setelah Inspirasi?", "id": "Kapasitas Vital, Mengukur Fungsi Paru." },
{ "en": "Apa Protein Plasma Untuk Pembekuan Darah?", "id": "Fibrinogen, Diubah Menjadi Fibrin Oleh Trombin." },
{ "en": "Kematian Jaringan Otot Jantung Disebut Apa?", "id": "Infark Miokard Atau Serangan Jantung." },
{ "en": "Ahli Yang Mempelajari Penyakit Menular Hewan?", "id": "Ahli Epizootiologi Atau Veterinary Epidemiologist." },
{ "en": "Volume Udara Sisa Di Paru-Paru Adalah?", "id": "Volume Residu, Tidak Bisa Dikeluarkan." },
{ "en": "Apa Sel Darah Yang Tidak Memiliki Inti?", "id": "Eritrosit Dewasa Dan Trombosit Darah." },
{ "en": "Istilah Untuk Nyeri Dada Akibat Iskemia?", "id": "Angina Pektoris, Tanda Penyakit Arteri Koroner." },
{ "en": "Siapa Yang Melakukan Tes Latihan Stres?", "id": "Fisiolog Latihan Klinis Atau Clinical Exercise Physiologist." },
{ "en": "Area Mati Fisiologis Di Saluran Napas?", "id": "Ruang Mati Anatomis, Udara Tak Berpartisipasi." },
{ "en": "Apa Sel Darah Yang Berumur Paling Pendek?", "id": "Neutrofil, Bertahan Beberapa Jam Hingga Hari." },
{ "en": "Prosedur Bedah Bypass Arteri Jantung Adalah?", "id": "CABG Singkatan (Coronary Artery Bypass Grafting)." },
{ "en": "Ahli Yang Mengelola Program Kesehatan Internasional?", "id": "Konsultan Kesehatan Global Atau Global Health Consultant." },
{ "en": "Hukum Apa Yang Mengatur Difusi Gas?", "id": "Hukum Fick, Terkait Luas, Jarak, Gradien." },
{ "en": "Apa Sel Darah Yang Berumur Paling Panjang?", "id": "Limfosit Memori, Bisa Bertahan Bertahun-Tahun." },
{ "en": "Prosedur Memasukkan Kateter Ke Jantung Adalah?", "id": "Kateterisasi Jantung, Untuk Diagnosis, Terapi." },
{ "en": "Siapa Yang Merancang Intervensi Kesehatan Masyarakat?", "id": "Perencana Kesehatan Atau Health Planner." },
{ "en": "Apa Ujung Pelindung Kromosom Sel Manusia?", "id": "Telomer, Mencegah Kerusakan DNA Genetik." },
{ "en": "Apa Sistem Pembawa Pesan Kedua Sel?", "id": "Siklik AMP (cAMP) Atau Ion Kalsium." },
{ "en": "Akhiran '-pexy' Dalam Istilah Bedah Artinya?", "id": "Fiksasi Atau Penjangkaran Organ Bedah." },
{ "en": "Siapa Teknisi Yang Merawat Peralatan Medis?", "id": "Teknisi Peralatan Biomedis Atau BMET." },
{ "en": "Protein Apa Penampil Antigen Sel Imun?", "id": "MHC Singkatan (Major Histocompatibility Complex)." },
{ "en": "Apa Bagian Tengah Penyempitan Kromosom Manusia?", "id": "Sentromer, Tempat Pelekatan Benang Spindel." },
{ "en": "Sumbu Hormon Stres Hipotalamus-Pituitari-Adrenal Adalah?", "id": "Sumbu HPA Singkatan (Hypothalamic-Pituitary-Adrenal Axis)." },
{ "en": "Siapa Yang Meninjau Etika Penelitian Medis?", "id": "Anggota Komite Etik Penelitian (IRB)." },
{ "en": "Apa Bagian DNA Yang Mengkode Protein?", "id": "Ekson, Bagian Yang Diekspresikan Gen." },
{ "en": "Apa Reseptor Sel Paling Umum Manusia?", "id": "Reseptor Terkait Protein G (GPCR)." },
{ "en": "Akhiran '-rrhaphy' Dalam Istilah Bedah Artinya?", "id": "Penjahitan Atau Perbaikan Jaringan Bedah." },
{ "en": "Spesialis Yang Menangani Terapi Gen Adalah?", "id": "Ahli Terapi Gen Atau Gene Therapist." },
{ "en": "Apa Bagian DNA Yang Tidak Mengkode?", "id": "Intron, Dihilangkan Selama Proses Splicing." },
{ "en": "Apa Neurotransmitter Rangsang Utama Di Otak?", "id": "Glutamat, Penting Untuk Belajar, Memori." },
{ "en": "Istilah Peningkatan Jumlah Sel Jaringan Adalah?", "id": "Hiperplasia, Bukan Peningkatan Ukuran Sel." },
{ "en": "Dokter Spesialis Yang Menangani Penyakit Mitokondria?", "id": "Spesialis Penyakit Mitokondria Atau Mitochondrial Specialist." },
{ "en": "Apa Proses Penghilangan Intron Dari RNA?", "id": "Penyambungan RNA Atau RNA Splicing." },
{ "en": "Apa Neurotransmitter Hambat Utama Di Otak?", "id": "GABA Singkatan (Gamma-Aminobutyric Acid)." },
{ "en": "Istilah Kurangnya Diferensiasi Sel Kanker Adalah?", "id": "Anaplasia, Ciri Khas Tumor Ganas." },
{ "en": "Siapa Yang Mengoperasikan Mesin Selama Dialisis?", "id": "Teknisi Dialisis Atau Dialysis Technician." },
{ "en": "Apa Hormon Yang Merangsang Kelenjar Lain?", "id": "Hormon Tropik, Dilepaskan Oleh Kelenjar Pituitari." },
{ "en": "Neurotransmitter Apa Terkait Suasana Hati Tidur?", "id": "Serotonin, Diproduksi Di Batang Otak." },
{ "en": "Zat Apa Yang Menyebabkan Cacat Lahir?", "id": "Teratogen, Seperti Alkohol, Obat Tertentu." },
{ "en": "Ahli Yang Menggambar Anatomi Untuk Pendidikan?", "id": "Ilustrator Medis, Menggabungkan Seni, Sains." },
{ "en": "Apa Sambungan Khusus Antara Sel Otot Jantung?", "id": "Cakram Berinterkalar Atau Intercalated Disc." },
{ "en": "Neurotransmitter Apa Terkait Gerakan, Kesenangan, Motivasi?", "id": "Dopamin, Penting Dalam Sistem Penghargaan." },
{ "en": "Studi Pengaruh Genetik Terhadap Respon Obat?", "id": "Farmakogenomik, Dasar Pengobatan Personal Terkini." },
{ "en": "Spesialis Yang Mengelola Program Skrining Genetik?", "id": "Koordinator Skrining Genetik Atau Genetic Screening Coordinator." },
{ "en": "Apa Sel Di Usus Yang Sekresi Antimikroba?", "id": "Sel Paneth, Ditemukan Di Kriptus Usus." },
{ "en": "Penguatan Koneksi Sinaptik Jangka Panjang Adalah?", "id": "Potensiasi Jangka Panjang Atau Long-Term Potentiation (LTP)." },
{ "en": "Kondisi Berkurangnya Respon Terhadap Suatu Obat?", "id": "Toleransi Obat, Membutuhkan Dosis Lebih Tinggi." },
{ "en": "Ahli Yang Mempelajari Penyakit Jaringan Gigi?", "id": "Patolog Oral Dan Maksilofasial Profesional." },
{ "en": "Apa Kelenjar Penghasil Lendir Di Hidung?", "id": "Kelenjar Bowman, Di Epitel Olfaktori." },
{ "en": "Apa Teori Seleksi Sel Imun Spesifik?", "id": "Teori Seleksi Klon, Dasar Imunitas Adaptif." },
{ "en": "Ketergantungan Fisik Psikologis Pada Obat Adalah?", "id": "Ketergantungan Obat Atau Drug Dependence." },
{ "en": "Perawat Yang Fokus Pada Perawatan Luka Adalah?", "id": "Perawat Perawatan Luka Atau Wound Care Nurse." },
{ "en": "Molekul MHC Kelas I Ditemukan Dimana?", "id": "Di Semua Sel Tubuh Berinti." },
{ "en": "Apa Alat Bedah Untuk Membuat Sayatan?", "id": "Skalpel Atau Pisau Bedah Tajam." },
{ "en": "Awalan 'Supra-' Dalam Istilah Medis Artinya?", "id": "Di Atas Atau Superior Dari Sesuatu." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Pigmen Kulit?", "id": "Spesialis Gangguan Pigmentasi Atau Pigmentation Specialist." },
{ "en": "Molekul MHC Kelas II Ditemukan Dimana?", "id": "Hanya Di Sel Penyaji Antigen (APC)." },
{ "en": "Apa Alat Bedah Untuk Menjepit Jaringan?", "id": "Forseps Atau Pinset Bedah Medis." },
{ "en": "Awalan 'Infra-' Dalam Istilah Medis Artinya?", "id": "Di Bawah Atau Inferior Dari Sesuatu." },
{ "en": "Ahli Yang Mengelola Sistem Informasi Radiologi?", "id": "Administrator PACS Singkatan (Picture Archiving and Communication System)." },
{ "en": "Sel T Helper Mengenali Antigen Dimana?", "id": "Pada Molekul MHC Kelas II." },
{ "en": "Apa Alat Bedah Untuk Menahan Jaringan?", "id": "Klem, Menjepit Pembuluh Darah Jaringan." },
{ "en": "Istilah Untuk Pengerasan Jaringan Akibat Parut?", "id": "Sklerosis, Contohnya Aterosklerosis, Sklerosis Ganda." },
{ "en": "Dokter Yang Mengkhususkan Diri Perawatan Luka Kronis?", "id": "Dokter Spesialis Perawatan Luka (Wound Care Specialist)." },
{ "en": "Sel T Sitotoksik Mengenali Antigen Dimana?", "id": "Pada Molekul MHC Kelas I." },
{ "en": "Apa Alat Bedah Untuk Membuka Lapangan Operasi?", "id": "Retraktor, Menahan Jaringan Atau Organ." },
{ "en": "Istilah Untuk Pelembutan Tulang Secara Abnormal?", "id": "Osteomalasia, Akibat Kekurangan Vitamin D." },
{ "en": "Siapa Yang Menulis Teks Untuk Materi Kesehatan?", "id": "Penulis Medis Atau Medical Writer." },
{ "en": "Apa Hormon Yang Dihasilkan Kelenjar Timus?", "id": "Timosin, Berperan Pematangan Sel T." },
{ "en": "Apa Unit Penyusun Dasar Dari Protein?", "id": "Asam Amino, Dihubungkan Oleh Ikatan Peptida." },
{ "en": "Kondisi Peradangan Pada Sumsum Tulang Belakang?", "id": "Mielitis, Dapat Menyebabkan Kelemahan, Kelumpuhan." },
{ "en": "Spesialis Yang Mengelola Laboratorium Fertilitas Adalah?", "id": "Direktur Laboratorium Embriologi Atau IVF (In Vitro Fertilization)." },
{ "en": "Apa Hormon Yang Dihasilkan Oleh Ginjal?", "id": "Eritropoietin (EPO), Renin, Dan Kalsitriol." },
{ "en": "Apa Ikatan Kimia Yang Menghubungkan Asam Amino?", "id": "Ikatan Peptida, Membentuk Rantai Polipeptida." },
{ "en": "Kondisi Peradangan Kelenjar Ludah Parotis Adalah?", "id": "Parotitis, Contohnya Penyakit Gondongan (Mumps)." },
{ "en": "Ahli Yang Mempelajari Penyebaran Penyakit Kanker?", "id": "Ahli Epidemiologi Kanker Atau Cancer Epidemiologist." },
{ "en": "Apa Hormon Yang Dihasilkan Lapisan Perut?", "id": "Gastrin, Ghrelin, Dan Histamin Perut." },
{ "en": "Struktur Heliks Alfa Beta-Sheet Terdapat Dimana?", "id": "Struktur Sekunder Protein, Membentuk Lipatan." },
{ "en": "Kondisi Peradangan Pada Pankreas Secara Akut?", "id": "Pankreatitis Akut, Sering Akibat Batu Empedu." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Medis Penyelam?", "id": "Dokter Kedokteran Hiperbarik Dan Selam." },
{ "en": "Bagian Mana Dari Tulang Yang Terus Tumbuh?", "id": "Lempeng Epifisis Atau Lempeng Pertumbuhan." },
{ "en": "Struktur Protein Yang Mengalami Denaturasi Adalah?", "id": "Struktur Tersier Dan Kuartener Protein." },
{ "en": "Kondisi Peradangan Pada Otot Rangka Adalah?", "id": "Miositis, Dapat Disebabkan Oleh Infeksi." },
{ "en": "Ahli Yang Menangani Terapi Wicara Anak?", "id": "Patolog Wicara-Bahasa Pediatrik Profesional Medis." },
{ "en": "Di Mana Lokasi Pertukaran Gas Pernapasan?", "id": "Alveolus Di Ujung Saluran Bronkiolus." },
{ "en": "Apa Enzim Yang Mengkatalisis Reaksi Kimia?", "id": "Katalis Biologis, Sebagian Besar Berupa Protein." },
{ "en": "Kondisi Peradangan Pada Kelenjar Getah Bening?", "id": "Limfadenitis, Tanda Respon Imun Tubuh." },
{ "en": "Spesialis Yang Mengelola Program Kesehatan Sekolah?", "id": "Perawat Sekolah Atau Koordinator Kesehatan Sekolah." },
{ "en": "Apa Cairan Yang Mengurangi Tegangan Permukaan Alveoli?", "id": "Surfaktan Paru, Mencegah Alveoli Kolaps." },
{ "en": "Sisi Aktif Pada Enzim Berfungsi Untuk?", "id": "Mengikat Substrat, Mempercepat Reaksi Kimia." },
{ "en": "Kondisi Peradangan Pada Pembuluh Limfa Adalah?", "id": "Limfangitis, Sering Terlihat Garis Merah." },
{ "en": "Ahli Yang Mempelajari Penyakit Akibat Lingkungan?", "id": "Ahli Epidemiologi Lingkungan Atau Environmental Epidemiologist." },
{ "en": "Apa Jenis Respirasi Yang Tidak Butuh Oksigen?", "id": "Respirasi Anaerob, Menghasilkan Asam Laktat." },
{ "en": "Model Enzim Dan Substrat Yang Pas?", "id": "Model Kunci Dan Gembok (Lock-And-Key)." },
{ "en": "Kondisi Batu Di Dalam Kelenjar Ludah?", "id": "Sialolitiasis, Dapat Menyumbat Aliran Ludah." },
{ "en": "Dokter Spesialis Yang Menangani Gangguan Gerak?", "id": "Spesialis Gangguan Gerak (Movement Disorder Specialist)." },
{ "en": "Apa Jenis Respirasi Yang Membutuhkan Oksigen?", "id": "Respirasi Aerob, Menghasilkan Banyak ATP." },
{ "en": "Penghambat Yang Mengikat Sisi Aktif Enzim?", "id": "Inhibitor Kompetitif, Bersaing Dengan Substrat." },
{ "en": "Kondisi Batu Di Dalam Saluran Kemih?", "id": "Urolitiasis, Menyebabkan Nyeri Kolik Hebat." },
{ "en": "Spesialis Yang Menangani Masalah Kesehatan Transgender?", "id": "Dokter Perawatan Kesehatan Transgender Profesional Medis." },
{ "en": "Di Mana Glikolisis Terjadi Di Dalam Sel?", "id": "Di Sitoplasma Atau Sitosol Sel." },
{ "en": "Penghambat Yang Mengubah Bentuk Sisi Aktif?", "id": "Inhibitor Non-kompetitif, Mengikat Tempat Alosterik." },
{ "en": "Kondisi Batu Di Dalam Ginjal Manusia?", "id": "Nefrolitiasis, Dikenal Dengan Penyakit Batu Ginjal." },
{ "en": "Ahli Yang Merancang Studi Dan Uji Klinis?", "id": "Ahli Biostatistik Atau Biostatistician Profesional." },
{ "en": "Apa Siklus Pembuangan Amonia Dari Tubuh?", "id": "Siklus Urea, Terjadi Di Organ Hati." },
{ "en": "Apa Refleks Kaki Khas Pada Bayi?", "id": "Refleks Babinski, Jari Kaki Mekar." },
{ "en": "Istilah Cairan Radang Kaya Protein Sel?", "id": "Eksudat, Berbeda Dari Cairan Transudat." },
{ "en": "Siapa Fisikawan Yang Bekerja Di Radiologi?", "id": "Fisikawan Medis, Memastikan Keamanan Radiasi." },
{ "en": "Kondisi Kelebihan Hormon Kortisol Kronis Adalah?", "id": "Sindrom Cushing, Menyebabkan Wajah Bulan." },
{ "en": "Apa Badan Keton Utama Dalam Darah?", "id": "Asetoasetat Dan Beta-Hidroksibutirat, Sumber Energi." },
{ "en": "Sistem Saraf Intrinsik Saluran Pencernaan Adalah?", "id": "Sistem Saraf Enterik, 'Otak Kedua'." },
{ "en": "Pejabat Eksekutif Medis Tertinggi Rumah Sakit?", "id": "Chief Medical Officer Atau Direktur Medis." },
{ "en": "Apa Organel Sel Pendetoksifikasi Racun Alkohol?", "id": "Peroksisom, Mengandung Enzim Oksidatif Katalase." },
{ "en": "Apa Refleks Terkejut Khas Pada Bayi?", "id": "Refleks Moro, Tangan Terentang Tiba-Tiba." },
{ "en": "Istilah Cairan Bening Rendah Protein Sel?", "id": "Transudat, Akibat Perubahan Tekanan Hidrostatik." },
{ "en": "Pejabat Keperawatan Tertinggi Di Rumah Sakit?", "id": "Chief Nursing Officer Atau Direktur Keperawatan." },
{ "en": "Kondisi Kekurangan Hormon Kortisol Aldosteron Adalah?", "id": "Penyakit Addison, Menyebabkan Kelemahan, Hiperpigmentasi." },
{ "en": "Apa Rangka Internal Penopang Bentuk Sel?", "id": "Sitoskeleton (Mikrotubulus, Mikrofilamen, Filamen Intermediet)." },
{ "en": "Kumpulan Sel Radang Kronis Disebut Apa?", "id": "Granuloma, Khas Pada Penyakit Tuberkulosis." },
{ "en": "Siapa Yang Memantau Uji Coba Klinis?", "id": "Associate Riset Klinis Atau Clinical Research Associate (CRA)." },
{ "en": "Apa Kristal Kalsium Karbonat Di Telinga?", "id": "Otolit, Di Sistem Vestibular Keseimbangan." },
{ "en": "Apa Filamen Paling Tebal Di Sitoskeleton?", "id": "Mikrotubulus, Berperan Transportasi, Pembelahan Sel." },
{ "en": "Istilah Kegagalan Organ Membentuk Saluran Normal?", "id": "Atresia, Contohnya Atresia Esofagus Bawaan." },
{ "en": "Siapa Ahli Statistik Yang Bekerja Medis?", "id": "Ahli Biostatistik, Menganalisis Data Uji Klinis." },
{ "en": "Apa Saraf Kranial Mengontrol Gerakan Bahu?", "id": "Saraf Asesori (XI), Menginervasi Otot Trapezius." },
{ "en": "Apa Filamen Paling Tipis Di Sitoskeleton?", "id": "Mikrofilamen, Terbuat Dari Protein Aktin." },
{ "en": "Istilah Saluran Abnormal Antara Dua Organ?", "id": "Fistula, Bisa Terbentuk Akibat Cedera." },
{ "en": "Ahli Terapi Yang Menggunakan Air Adalah?", "id": "Terapis Akuatik Atau Aquatic Therapist." },
{ "en": "Apa Saraf Kranial Untuk Pengecapan Menelan?", "id": "Saraf Glosofaringeal (IX), Sensasi Faring." },
{ "en": "Dosis Awal Obat Yang Lebih Tinggi Adalah?", "id": "Dosis Muat Atau Loading Dose." },
{ "en": "Urutan Gambaran MRI Otak Berdasar Lemak?", "id": "Urutan T1-Weighted (T1), Lemak Terlihat Terang." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Kaki?", "id": "Podiatris, Merawat Kuku, Kapalan, Diabetes." },
{ "en": "Hormon Apa Yang Memicu Rasa Kantuk?", "id": "Melatonin, Diproduksi Oleh Kelenjar Pineal." },
{ "en": "Dosis Obat Untuk Menjaga Konsentrasi Stabil?", "id": "Dosis Pemeliharaan Atau Maintenance Dose." },
{ "en": "Urutan Gambaran MRI Otak Berdasar Air?", "id": "Urutan T2-Weighted (T2), Cairan Terlihat Terang." },
{ "en": "Terapis Yang Mengelola Oksigen Pasien Adalah?", "id": "Terapis Pernapasan Atau Respiratory Therapist." },
{ "en": "Apa Zat Yang Ditambahkan Memperjelas Gambaran?", "id": "Agen Kontras, Seperti Gadolinium (MRI)." },
{ "en": "Apa Jendela Terapeutik Suatu Obat Manusia?", "id": "Rentang Konsentrasi Obat Yang Efektif." },
{ "en": "Akar Kata 'Osteo-' Merujuk Pada Apa?", "id": "Tulang, Seperti Osteoporosis, Osteosit, Osteoma." },
{ "en": "Siapa Yang Membuat Alat Bantu Ortopedi?", "id": "Orthotist, Merancang Brace, Splint, Insole." },
{ "en": "Akar Kata 'Myo-' Merujuk Pada Apa?", "id": "Otot, Seperti Miokardium, Miositis, Mioma." },
{ "en": "Struktur Sel Apa Yang Menghasilkan Ribosom?", "id": "Nukleolus, Terletak Di Dalam Inti Sel." },
{ "en": "Penyakit Kelebihan Hormon Pertumbuhan Dewasa Adalah?", "id": "Akromegali, Pembesaran Tangan, Kaki, Wajah." },
{ "en": "Siapa Yang Membuat Anggota Badan Tiruan?", "id": "Prosthetist, Merancang Kaki, Tangan Prostetik." },
{ "en": "Akhiran '-tomi' Dalam Istilah Bedah Artinya?", "id": "Membuat Sayatan Atau Membuka Sesuatu." },
{ "en": "Apa Enzim Di Ujung Air Liur?", "id": "Amilase Saliva, Memecah Pati Menjadi Gula." },
{ "en": "Penyakit Kekurangan Hormon Antidiuretik (ADH) Adalah?", "id": "Diabetes Insipidus, Menyebabkan Haus, Poliuria." },
{ "en": "Ahli Yang Mempelajari Racun Dan Efeknya?", "id": "Toksikolog, Bisa Bekerja Forensik, Lingkungan." },
{ "en": "Akar Kata 'Hepato-' Merujuk Pada Apa?", "id": "Hati, Seperti Hepatitis, Hepatomegali, Hepatosit." },
{ "en": "Apa Lapisan Jaringan Ikat Di Bawah Kulit?", "id": "Fasia Superfisial, Mengandung Lemak, Saraf." },
{ "en": "Penyakit Autoimun Yang Menyerang Otot Adalah?", "id": "Polimiositis, Menyebabkan Kelemahan Otot Proksimal." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Medis Atlet?", "id": "Dokter Kedokteran Olahraga (Sports Medicine)." },
{ "en": "Akar Kata 'Nephro-' Merujuk Pada Apa?", "id": "Ginjal, Seperti Nefron, Nefrologi, Nefrektomi." },
{ "en": "Apa Lapisan Jaringan Ikat Pembungkus Otot?", "id": "Fasia Dalam, Memisahkan Kompartemen Otot." },
{ "en": "Penyakit Autoimun Yang Merusak Kelenjar Eksokrin?", "id": "Sindrom Sjogren, Menyebabkan Mata, Mulut Kering." },
{ "en": "Spesialis Pengobatan Yang Bekerja Daerah Bencana?", "id": "Dokter Medis Kedaruratan Atau Disaster Medicine Doctor." },
{ "en": "Akar Kata 'Kardio-' Merujuk Pada Apa?", "id": "Jantung, Seperti Kardiologi, Miokardium, EKG." },
{ "en": "Apa Sel Yang Menghasilkan Antibodi Imunoglobulin?", "id": "Sel Plasma, Bentuk Diferensiasi Sel B." },
{ "en": "Penyakit Genetik Yang Mempengaruhi Produksi Kolagen?", "id": "Sindrom Ehlers-Danlos, Sendi Hipermobil, Kulit Elastis." },
{ "en": "Ahli Yang Menangani Kesehatan Mental Tahanan?", "id": "Psikolog Forensik, Bekerja Sistem Hukum." },
{ "en": "Akar Kata 'Pulmo-' Merujuk Pada Apa?", "id": "Paru-Paru, Seperti Pulmonologi, Arteri Pulmonalis." },
{ "en": "Apa Sel Yang Mendegradasi Tulang Tua?", "id": "Osteoklas, Berperan Dalam Remodeling Tulang." },
{ "en": "Penyakit Genetik Tulang Rapuh Sejak Lahir?", "id": "Osteogenesis Imperfecta, Dikenal Penyakit Tulang Kaca." },
{ "en": "Dokter Spesialis Yang Menangani Penyakit Lansia?", "id": "Geriatris, Fokus Pada Kesehatan Geriatri." },
{ "en": "Akar Kata 'Gastro-' Merujuk Pada Apa?", "id": "Lambung, Seperti Gastritis, Gastroenterologi, Gastrin." },
{ "en": "Apa Sel Yang Melapisi Pembuluh Darah?", "id": "Sel Endotel, Membentuk Lapisan Tunika Intima." },
{ "en": "Istilah Untuk Kehadiran Udara Rongga Pleura?", "id": "Pneumotoraks, Dapat Menyebabkan Paru-Paru Kolaps." },
{ "en": "Ahli Yang Menangani Kesehatan Dan Keselamatan Kerja?", "id": "Spesialis K3 (Kesehatan dan Keselamatan Kerja)." },
{ "en": "Akar Kata 'Neuro-' Merujuk Pada Apa?", "id": "Saraf, Seperti Neurologi, Neuron, Neuropati." },
{ "en": "Apa Sel Yang Menghasilkan Asam Klorida?", "id": "Sel Parietal Di Dinding Lambung." },
{ "en": "Istilah Untuk Kehadiran Darah Rongga Pleura?", "id": "Hemotoraks, Akibat Cedera Trauma Dada." },
{ "en": "Spesialis Yang Mengelola Program Kesehatan Kampus?", "id": "Direktur Layanan Kesehatan Mahasiswa (University Health)." },
{ "en": "Akar Kata 'Hemo-' Merujuk Pada Apa?", "id": "Darah, Seperti Hemoglobin, Hemostasis, Hematologi." },
{ "en": "Apa Sel Yang Melapisi Rongga Peritoneum?", "id": "Sel Mesotelial, Membentuk Membran Serosa." },
{ "en": "Istilah Untuk Kehadiran Nanah Rongga Pleura?", "id": "Empiema Atau Piotoraks, Akibat Infeksi." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Medis Musisi?", "id": "Dokter Seni Pertunjukan (Performing Arts Medicine)." },
{ "en": "Akar Kata 'Dermato-' Merujuk Pada Apa?", "id": "Kulit, Seperti Dermatologi, Epidermis, Dermatitis." },
{ "en": "Apa Hormon Yang Membantu Penyerapan Kalsium?", "id": "Kalsitriol, Bentuk Aktif Vitamin D." },
{ "en": "Kondisi Peradangan Pada Kantung Perikardial Adalah?", "id": "Perikarditis, Menyebabkan Nyeri Dada Saat Bernapas." },
{ "en": "Ahli Yang Mengelola Bank Jaringan Manusia?", "id": "Spesialis Bank Jaringan Atau Tissue Bank Specialist." },
{ "en": "Akhiran '-plasti' Dalam Istilah Bedah Artinya?", "id": "Perbaikan Atau Rekonstruksi Bentuk Bedah." },
{ "en": "Apa Hormon Yang Meningkatkan Gula Darah?", "id": "Glukagon, Kortisol, Epinefrin, Hormon Pertumbuhan." },
{ "en": "Kondisi Peradangan Pada Selaput Otak-Tulang Belakang?", "id": "Meningitis, Bisa Disebabkan Bakteri, Virus." },
{ "en": "Dokter Spesialis Yang Menangani Kecanduan Adalah?", "id": "Psikiater Adiksi Atau Spesialis Kedokteran Adiksi." },
{ "en": "Akhiran '-skopi' Dalam Istilah Medis Artinya?", "id": "Melihat Ke Dalam Menggunakan Alat." },
{ "en": "Apa Reseptor Yang Merespon Regangan Otot?", "id": "Spindel Otot Atau Muscle Spindle." },
{ "en": "Kondisi Peradangan Pada Hati Manusia Adalah?", "id": "Hepatitis, Disebabkan Virus, Alkohol, Autoimun." },
{ "en": "Ahli Yang Merancang Program Latihan Rehabilitasi?", "id": "Fisiolog Latihan Klinis Atau Clinical Exercise Physiologist." },
{ "en": "Akhiran '-grafi' Dalam Istilah Medis Artinya?", "id": "Proses Merekam Atau Mengambil Gambar." },
{ "en": "Apa Reseptor Yang Merespon Tegangan Tendon?", "id": "Organ Tendon Golgi, Mencegah Cedera." },
{ "en": "Kondisi Peradangan Pada Sendi Manusia Adalah?", "id": "Artritis, Seperti Osteoartritis, Artritis Reumatoid." },
{ "en": "Spesialis Pengobatan Yang Bekerja Di Militer?", "id": "Dokter Militer Atau Military Doctor." },
{ "en": "Apa Sisa Duktus Arteriosus Setelah Lahir?", "id": "Ligamentum Arteriosum, Jaringan Ikat Fibrosa." },
{ "en": "Enzim Apa Yang Memicu Kematian Sel?", "id": "Kaspase, Eksekutor Utama Proses Apoptosis." },
{ "en": "Tanda Meningitis Saat Kaki Diluruskan Adalah?", "id": "Tanda Kernig Positif, Menyebabkan Nyeri." },
{ "en": "Siapa Ahli Gigi Yang Mengidentifikasi Jenazah?", "id": "Odontolog Forensik, Menggunakan Catatan Gigi." },
{ "en": "Obat Penurun Kolesterol Paling Umum Adalah?", "id": "Statin, Menghambat Enzim HMG-CoA Reduktase." },
{ "en": "Apa Sisa Foramen Ovale Setelah Lahir?", "id": "Fossa Ovalis, Cekungan Di Septum Atrium." },
{ "en": "Sel Ginjal Apa Yang Merasakan Kadar Garam?", "id": "Sel Makula Densa Di Aparatus Juxtaglomerular." },
{ "en": "Siapa Ahli Tulang Yang Mengidentifikasi Jenazah?", "id": "Antropolog Forensik, Menganalisis Sisa Rangka." },
{ "en": "Tanda Meningitis Saat Leher Ditekuk Adalah?", "id": "Tanda Brudzinski Positif, Kaki Ikut Menekuk." },
{ "en": "Obat Penurun Tekanan Darah Blokade Beta?", "id": "Beta Bloker, Mengurangi Beban Kerja Jantung." },
{ "en": "Jalur Apoptosis Yang Diinduksi Kerusakan Sel?", "id": "Jalur Intrinsik, Melibatkan Organel Mitokondria." },
{ "en": "Sel Ginjal Apa Yang Menghasilkan Hormon Renin?", "id": "Sel Juxtaglomerular, Merespons Tekanan Rendah." },
{ "en": "Siapa Spesialis Aplikasi Perangkat Lunak Medis?", "id": "Spesialis Aplikasi Klinis Atau Clinical Application Specialist." },
{ "en": "Tanda Kekurangan Kalsium Saat Wajah Diketuk?", "id": "Tanda Chvostek Positif, Otot Wajah Berkedut." },
{ "en": "Obat Penurun Tekanan Darah Blokade ACE?", "id": "ACE Inhibitor, Mencegah Penyempitan Pembuluh Darah." },
{ "en": "Jalur Apoptosis Yang Diinduksi Sinyal Eksternal?", "id": "Jalur Ekstrinsik, Melibatkan Reseptor Kematian." },
{ "en": "Apa Jaringan Ikat Pembentuk Rangka Limpa?", "id": "Jaringan Ikat Retikular, Serat Halus." },
{ "en": "Siapa Spesialis Implementasi Rekam Medis Elektronik?", "id": "Spesialis Implementasi EHR Singkatan (Electronic Health Record)." },
{ "en": "Istilah Untuk Jaringan Tumor Salah Tempat?", "id": "Koristoma, Jaringan Normal Di Lokasi Abnormal." },
{ "en": "Obat Penurun Asam Lambung Paling Kuat?", "id": "PPI Singkatan (Proton Pump Inhibitor)." },
{ "en": "Apa Pertumbuhan Jaringan Berlebih Tidak Teratur?", "id": "Hamartoma, Campuran Jaringan Normal Yang Berlebih." },
{ "en": "Apa Saraf Kranial Mengontrol Gerakan Lidah?", "id": "Saraf Hipoglosus (XII), Untuk Bicara Menelan." },
{ "en": "Teknisi Yang Menguji Jantung Pembuluh Darah?", "id": "Teknolog Kardiovaskular Atau Cardiovascular Technologist." },
{ "en": "Apa Istilah Bedah Untuk Membuka Perut?", "id": "Laparotomi, Sayatan Besar Dinding Abdomen." },
{ "en": "Kelas Obat Anti-inflamasi Non-steroid Adalah?", "id": "NSAID Singkatan (Nonsteroidal Anti-inflammatory Drug)." },
{ "en": "Apa Refleks Yang Menjaga Keseimbangan Berdiri?", "id": "Refleks Postural, Penyesuaian Otomatis Otot." },
{ "en": "Apa Saraf Kranial Sensasi Rasa Depan Lidah?", "id": "Saraf Wajah (VII), Melalui Korda Timpani." },
{ "en": "Teknisi Yang Menguji Vena Dan Arteri?", "id": "Teknolog Vaskular Atau Vascular Technologist." },
{ "en": "Apa Prosedur Medis Menggunakan Suhu Dingin?", "id": "Cryotherapy, Membekukan Jaringan Abnormal Dengan Dingin." },
{ "en": "Kelas Obat Untuk Mengencerkan Darah Adalah?", "id": "Antikoagulan, Mencegah Pembentukan Gumpalan Darah." },
{ "en": "Apa Refleks Pupil Yang Menyempit Terhadap Cahaya?", "id": "Refleks Miosis, Diperantarai Saraf Parasimpatik." },
{ "en": "Apa Sel Yang Melapisi Ventrikel Otak?", "id": "Sel Ependimal, Membantu Sirkulasi Cairan Serebrospinal." },
{ "en": "Direktur Program Pendidikan Dokter Spesialis Adalah?", "id": "Direktur Program Residensi (Residency Program Director)." },
{ "en": "Prosedur Medis Menggunakan Panas Listrik Adalah?", "id": "Elektrokauter, Menghentikan Pendarahan, Memotong Jaringan." },
{ "en": "Kelas Obat Untuk Menghilangkan Kecemasan Adalah?", "id": "Ansiolitik, Contohnya Benzodiazepin, SSRI." },
{ "en": "Apa Refleks Pupil Yang Melebar Kegelapan?", "id": "Refleks Midriasis, Diperantarai Saraf Simpatik." },
{ "en": "Apa Sel Yang Menghasilkan Cairan Pleura?", "id": "Sel Mesotelial Yang Melapisi Pleura." },
{ "en": "Petugas Penerimaan Mahasiswa Sekolah Kedokteran Adalah?", "id": "Petugas Penerimaan Medis (Medical Admissions Officer)." },
{ "en": "Gangguan Gerakan Berulang Tanpa Disengaja Adalah?", "id": "Sindrom Tourette, Tic Motorik Vokal." },
{ "en": "Kelas Obat Untuk Mengobati Depresi Adalah?", "id": "Antidepresan, Seperti SSRI, SNRI, TCA." },
{ "en": "Apa Pigmen Penglihatan Di Sel Batang?", "id": "Rodopsin, Peka Terhadap Cahaya Redup." },
{ "en": "Apa Sel Yang Menghasilkan Surfaktan Paru?", "id": "Pneumosit Tipe II Di Dinding Alveolus." },
{ "en": "Ahli Fisiologi Yang Bekerja Di Rumah Sakit?", "id": "Fisiolog Klinis, Melakukan Tes Diagnostik." },
{ "en": "Gangguan Genetik Dengan Tiga Salinan Kromosom 21?", "id": "Sindrom Down, Menyebabkan Keterlambatan Perkembangan." },
{ "en": "Kelas Obat Untuk Mengobati Psikosis Adalah?", "id": "Antipsikotik, Memblokir Reseptor Dopamin Di Otak." },
{ "en": "Apa Pigmen Penglihatan Di Sel Kerucut?", "id": "Fotopsin, Ada Tiga Jenis Warna." },
{ "en": "Apa Sel Yang Menyusun Kapsul Bowman?", "id": "Podosit, Sel Khusus Dengan Kaki Filtrasi." },
{ "en": "Siapa Penanggung Jawab Medis Tim Olahraga?", "id": "Dokter Tim Atau Team Physician." },
{ "en": "Penyakit Kelebihan Cairan Di Rongga Peritoneum?", "id": "Asites, Seringkali Akibat Sirosis Hati." },
{ "en": "Kelas Obat Untuk Mengontrol Kejang Epilepsi?", "id": "Antikonvulsan Atau Obat Anti-epilepsi." },
{ "en": "Apa Proses Adaptasi Mata Terhadap Kegelapan?", "id": "Adaptasi Gelap, Regenerasi Rodopsin Secara Penuh." },
{ "en": "Sel Apa Yang Menjadi Perancah Ginjal?", "id": "Sel Mesangial, Memberi Dukungan Struktural." },
{ "en": "Dokter Gigi Spesialis Kesehatan Gusi Adalah?", "id": "Periodontis, Menangani Gingivitis, Periodontitis Parah." },
{ "en": "Penyakit Pembesaran Vena Di Esofagus Adalah?", "id": "Varises Esofagus, Akibat Hipertensi Portal." },
{ "en": "Kelas Obat Untuk Meredakan Mual Muntah?", "id": "Antiemetik, Bekerja Pada Otak Usus." },
{ "en": "Apa Proses Adaptasi Mata Terhadap Cahaya?", "id": "Adaptasi Terang, Pigmen Penglihatan Pecah." },
{ "en": "Apa Bagian Dari Kelenjar Pituitari Anterior?", "id": "Pars Distalis, Pars Tuberalis, Pars Intermedia." },
{ "en": "Spesialis Pengobatan Yang Bekerja Di Penjara?", "id": "Dokter Lembaga Pemasyarakatan (Correctional Physician)." },
{ "en": "Penyakit Radang Pada Kantung Empedu Adalah?", "id": "Kolesistitis, Sering Akibat Batu Empedu." },
{ "en": "Kelas Obat Untuk Melebarkan Saluran Napas?", "id": "Bronkodilator, Digunakan Untuk Mengobati Asma." },
{ "en": "Apa Bagian Dari Kelenjar Pituitari Posterior?", "id": "Pars Nervosa Dan Batang Infundibulum." },
{ "en": "Apa Enzim Di Lisosom Yang Bersifat Merusak?", "id": "Hidrolase Asam, Bekerja Di pH Rendah." },
{ "en": "Ahli Yang Menangani Terapi Keluarga Sistemik?", "id": "Terapis Keluarga, Melihat Keluarga Sistem." },
{ "en": "Kondisi Kantung Udara Paru-Paru Rusak Permanen?", "id": "Emfisema, Menyebabkan Sesak Napas Berat." },
{ "en": "Kelas Obat Untuk Menekan Sistem Imun?", "id": "Imunosupresan, Digunakan Setelah Transplantasi Organ." },
{ "en": "Apa Struktur Terkecil Penyusun Otot Lurik?", "id": "Sarkomer, Unit Kontraktil Dari Miofibril." },
{ "en": "Enzim Apa Yang Ditemukan Di Peroksisom?", "id": "Katalase Dan Oksidase Asam Urat." },
{ "en": "Terapis Yang Menggunakan Pasir Untuk Bermain?", "id": "Terapis Sandplay, Metode Psikoterapi Ekspresif." },
{ "en": "Kondisi Radang Kronis Saluran Udara Paru?", "id": "Bronkitis Kronis, Batuk Produktif Jangka Panjang." },
{ "en": "Kelas Obat Untuk Mengobati Kanker Adalah?", "id": "Kemoterapi, Antineoplastik, Terapi Target Imunoterapi." },
{ "en": "Filamen Tebal Penyusun Sarkomer Otot Adalah?", "id": "Miosin, Memiliki Kepala Yang Mengikat Aktin." },
{ "en": "Zat Antara Dalam Produksi Bilirubin Adalah?", "id": "Biliverdin, Berwarna Hijau, Diubah Bilirubin." },
{ "en": "Dokter Spesialis Yang Menangani Masalah Gizi?", "id": "Dokter Gizi Klinis, Berbeda Ahli Gizi." },
{ "en": "Kondisi Genetik Darah Sulit Membeku Adalah?", "id": "Hemofilia, Kekurangan Faktor VIII Atau IX." },
{ "en": "Kelas Obat Untuk Melawan Infeksi Virus?", "id": "Antivirus, Menghambat Replikasi Virus Dalam Tubuh." },
{ "en": "Filamen Tipis Penyusun Sarkomer Otot Adalah?", "id": "Aktin, Tempat Miosin Mengikat Saat Kontraksi." },
{ "en": "Apa Bentuk Penyimpanan Besi Dalam Sel?", "id": "Feritin, Protein Penyimpan Besi Internal." },
{ "en": "Spesialis Pengobatan Yang Fokus Pencegahan Adalah?", "id": "Dokter Kedokteran Preventif (Preventive Medicine)." },
{ "en": "Kondisi Penumpukan Zat Besi Dalam Jaringan?", "id": "Hemokromatosis, Dapat Merusak Organ Vital." },
{ "en": "Kelas Obat Untuk Melawan Infeksi Jamur?", "id": "Antijamur, Menargetkan Dinding Sel Jamur." },
{ "en": "Protein Apa Yang Mengatur Kontraksi Otot?", "id": "Troponin Dan Tropomiosin, Mengatur Interaksi Aktin-Miosin." },
{ "en": "Apa Protein Transpor Besi Dalam Darah?", "id": "Transferin, Membawa Besi Antar Jaringan." },
{ "en": "Perawat Dengan Pelatihan Lanjutan Anestesi Adalah?", "id": "Perawat Anestesi Bersertifikat (CRNA)." },
{ "en": "Kondisi Kelainan Genetik Reseptor LDL Kolesterol?", "id": "Hiperkolesterolemia Familial, Kolesterol Sangat Tinggi." },
{ "en": "Kelas Obat Untuk Melawan Infeksi Parasit?", "id": "Antiparasit, Seperti Antelmintik, Antiprotozoa." },
{ "en": "Ion Apa Pemicu Utama Kontraksi Otot?", "id": "Ion Kalsium (Ca2+), Dilepaskan Retikulum Sarkoplasma." },
{ "en": "Apa Hormon Yang Merangsang Pembentukan Tulang?", "id": "Kalsitonin, Hormon Pertumbuhan, Estrogen, Testosteron." },
{ "en": "Ahli Yang Mengelola Program Kesehatan Internasional?", "id": "Spesialis Kesehatan Global Atau Global Health Specialist." },
{ "en": "Kondisi Genetik Gangguan Metabolisme Tembaga Adalah?", "id": "Penyakit Wilson, Penumpukan Tembaga Di Hati." },
{ "en": "Kelas Obat Untuk Menurunkan Asam Urat?", "id": "Obat Urikosurik Atau Inhibitor Xantin Oksidase." }


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
