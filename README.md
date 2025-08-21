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



  { "en": "Apa itu jaringan komputer?", "id": "Dua atau lebih komputer saling terhubung." },
  { "en": "Tujuan utama jaringan komputer?", "id": "Berbagi sumber daya dan informasi." },
  { "en": "Apa itu Model OSI?", "id": "Model referensi tujuh lapisan untuk jaringan." },
  { "en": "Siapa yang mengembangkan Model OSI?", "id": "International Organization for Standardization (ISO)." },
  { "en": "Berapa lapisan pada Model OSI?", "id": "Ada tujuh lapisan (layer)." },
  { "en": "Sebutkan lapisan ke-7 OSI?", "id": "Application Layer." },
  { "en": "Sebutkan lapisan ke-6 OSI?", "id": "Presentation Layer." },
  { "en": "Sebutkan lapisan ke-5 OSI?", "id": "Session Layer." },
  { "en": "Sebutkan lapisan ke-4 OSI?", "id": "Transport Layer." },
  { "en": "Sebutkan lapisan ke-3 OSI?", "id": "Network Layer." },
  { "en": "Sebutkan lapisan ke-2 OSI?", "id": "Data Link Layer." },
  { "en": "Sebutkan lapisan ke-1 OSI?", "id": "Physical Layer." },
  { "en": "Akronim untuk mengingat lapisan OSI?", "id": "Please Do Not Throw Sausage Pizza Away." },
  { "en": "Fungsi utama Application Layer?", "id": "Antarmuka pengguna ke layanan jaringan." },
  { "en": "Contoh protokol di Application Layer?", "id": "HTTP, FTP, SMTP, DNS." },
  { "en": "Fungsi utama Presentation Layer?", "id": "Translasi, enkripsi, dan kompresi data." },
  { "en": "Fungsi utama Session Layer?", "id": "Membangun, mengelola, dan mengakhiri sesi." },
  { "en": "Fungsi utama Transport Layer?", "id": "Menyediakan koneksi antar host yang andal." },
  { "en": "Protokol utama di Transport Layer?", "id": "TCP dan UDP." },
  { "en": "Fungsi utama Network Layer?", "id": "Pengalamatan logis dan penentuan rute." },
  { "en": "Perangkat apa yang bekerja di Network Layer?", "id": "Router." },
  { "en": "Fungsi utama Data Link Layer?", "id": "Pengalamatan fisik dan deteksi eror." },
  { "en": "Perangkat apa yang bekerja di Data Link Layer?", "id": "Switch dan Bridge." },
  { "en": "Fungsi utama Physical Layer?", "id": "Transmisi bit melalui media fisik." },
  { "en": "Contoh dari Physical Layer?", "id": "Kabel, konektor, sinyal listrik." },
  { "en": "Apa itu PDU?", "id": "Protocol Data Unit." },
  { "en": "Apa nama PDU di Application Layer?", "id": "Data." },
  { "en": "Apa nama PDU di Transport Layer?", "id": "Segment (TCP) atau Datagram (UDP)." },
  { "en": "Apa nama PDU di Network Layer?", "id": "Packet." },
  { "en": "Apa nama PDU di Data Link Layer?", "id": "Frame." },
  { "en": "Apa nama PDU di Physical Layer?", "id": "Bits." },
  { "en": "Apa itu proses enkapsulasi?", "id": "Menambahkan header pada data di setiap lapisan." },
  { "en": "Apa itu proses dekapsulasi?", "id": "Melepas header pada data di setiap lapisan." },
  { "en": "Apa itu Model TCP/IP?", "id": "Model praktis yang digunakan oleh internet." },
  { "en": "Berapa lapisan pada Model TCP/IP?", "id": "Ada empat lapisan." },
  { "en": "Sebutkan lapisan TCP/IP dari atas?", "id": "Application, Transport, Internet, Network Access." },
  { "en": "Lapisan Application TCP/IP setara dengan apa di OSI?", "id": "Setara dengan lapisan 5, 6, 7 OSI." },
  { "en": "Lapisan Transport TCP/IP setara dengan apa di OSI?", "id": "Setara dengan lapisan 4 OSI." },
  { "en": "Lapisan Internet TCP/IP setara dengan apa di OSI?", "id": "Setara dengan lapisan 3 OSI." },
  { "en": "Lapisan Network Access TCP/IP setara dengan apa di OSI?", "id": "Setara dengan lapisan 1 dan 2 OSI." },
  { "en": "Apa itu topologi jaringan?", "id": "Tata letak fisik atau logis dari jaringan." },
  { "en": "Apa itu topologi bus?", "id": "Semua perangkat terhubung ke satu kabel pusat." },
  { "en": "Kelemahan topologi bus?", "id": "Jika kabel pusat rusak, jaringan mati." },
  { "en": "Apa itu terminator pada topologi bus?", "id": "Menyerap sinyal di ujung kabel." },
  { "en": "Apa itu topologi bintang (star)?", "id": "Semua perangkat terhubung ke hub/switch pusat." },
  { "en": "Topologi apa yang paling umum saat ini?", "id": "Topologi Bintang (Star)." },
  { "en": "Kelebihan topologi bintang?", "id": "Mudah dikelola, kerusakan satu node tidak mengganggu." },
  { "en": "Kelemahan topologi bintang?", "id": "Bergantung pada perangkat pusat (hub/switch)." },
  { "en": "Apa itu topologi cincin (ring)?", "id": "Setiap perangkat terhubung ke dua tetangganya." },
  { "en": "Apa itu 'token' pada topologi cincin?", "id": "Sinyal khusus untuk hak transmisi data." },
  { "en": "Apa itu topologi mesh?", "id": "Setiap perangkat terhubung ke semua perangkat lain." },
  { "en": "Kelebihan utama topologi mesh?", "id": "Sangat andal dan punya banyak jalur." },
  { "en": "Kelemahan utama topologi mesh?", "id": "Sangat mahal dan rumit untuk diimplementasikan." },
  { "en": "Apa itu topologi pohon (tree)?", "id": "Gabungan dari topologi bintang dan bus." },
  { "en": "Apa itu topologi hibrida?", "id": "Gabungan dari dua atau lebih topologi berbeda." },
  { "en": "Apa itu 'node' dalam jaringan?", "id": "Setiap perangkat yang terhubung ke jaringan." },
  { "en": "Apa itu LAN?", "id": "Local Area Network." },
  { "en": "Apa cakupan dari LAN?", "id": "Area kecil seperti gedung atau kampus." },
  { "en": "Apa itu WAN?", "id": "Wide Area Network." },
  { "en": "Apa cakupan dari WAN?", "id": "Area geografis yang sangat luas." },
  { "en": "Contoh WAN terbesar?", "id": "Internet adalah contoh WAN terbesar." },
  { "en": "Apa itu MAN?", "id": "Metropolitan Area Network." },
  { "en": "Apa cakupan dari MAN?", "id": "Mencakup area sebuah kota." },
  { "en": "Apa itu PAN?", "id": "Personal Area Network." },
  { "en": "Contoh teknologi PAN?", "id": "Bluetooth dan Wi-Fi Direct." },
  { "en": "Apa itu Alamat IP?", "id": "Alamat logis unik untuk identifikasi perangkat." },
  { "en": "Apa kepanjangan dari IP?", "id": "Internet Protocol." },
  { "en": "Versi IP yang paling umum saat ini?", "id": "IPv4 (Internet Protocol version 4)." },
  { "en": "Berapa bit panjang Alamat IPv4?", "id": "Terdiri dari 32 bit." },
  { "en": "Bagaimana format penulisan IPv4?", "id": "Empat blok desimal dipisahkan titik." },
  { "en": "Setiap blok IPv4 disebut apa?", "id": "Disebut oktet (octet)." },
  { "en": "Berapa nilai minimum satu oktet?", "id": "0." },
  { "en": "Berapa nilai maksimum satu oktet?", "id": "255." },
  { "en": "Alamat IP terdiri dari dua bagian, apa saja?", "id": "Network ID dan Host ID." },
  { "en": "Apa itu Network ID?", "id": "Bagian yang mengidentifikasi jaringan." },
  { "en": "Apa itu Host ID?", "id": "Bagian yang mengidentifikasi perangkat di jaringan." },
  { "en": "Apa itu subnet mask?", "id": "Menentukan bagian Network dan Host ID." },
  { "en": "Bagaimana cara kerja subnet mask?", "id": "Bit 1 untuk Network, bit 0 untuk Host." },
  { "en": "Contoh subnet mask kelas C?", "id": "255.255.255.0." },
  { "en": "Apa itu kelas Alamat IP?", "id": "Metode lama untuk mengklasifikasikan alamat." },
  { "en": "Rentang oktet pertama Kelas A?", "id": "1 sampai 126." },
  { "en": "Subnet mask default Kelas A?", "id": "255.0.0.0." },
  { "en": "Rentang oktet pertama Kelas B?", "id": "128 sampai 191." },
  { "en": "Subnet mask default Kelas B?", "id": "255.255.0.0." },
  { "en": "Rentang oktet pertama Kelas C?", "id": "192 sampai 223." },
  { "en": "Subnet mask default Kelas C?", "id": "255.255.255.0." },
  { "en": "Apa itu alamat IP privat?", "id": "Alamat yang digunakan dalam jaringan lokal." },
  { "en": "Apakah alamat IP privat bisa di-routing di internet?", "id": "Tidak, alamat privat tidak bisa di-routing." },
  { "en": "Rentang IP privat Kelas A?", "id": "10.0.0.0 sampai 10.255.255.255." },
  { "en": "Apa itu alamat IP publik?", "id": "Alamat unik global yang terhubung ke internet." },
  { "en": "Siapa yang mengalokasikan alamat IP publik?", "id": "IANA dan RIR (Regional Internet Registries)." },
  { "en": "Apa itu 'loopback address'?", "id": "127.0.0.1, merujuk ke perangkat itu sendiri." },
  { "en": "Apa itu alamat broadcast?", "id": "Alamat untuk mengirim data ke semua host." },
  { "en": "Apa itu alamat network?", "id": "Alamat pertama dalam sebuah subnet." },
  { "en": "Apa itu CIDR?", "id": "Classless Inter-Domain Routing." },
  { "en": "Apa tujuan CIDR?", "id": "Menggantikan sistem kelas IP yang tidak efisien." },
  { "en": "Bagaimana notasi CIDR ditulis?", "id": "Alamat IP diikuti garis miring dan angka." },
  { "en": "Contoh notasi CIDR?", "id": "192.168.1.0/24." },
  { "en": "Apa arti /24 dalam CIDR?", "id": "24 bit pertama adalah network ID." },
  { "en": "Apa itu 'subnetting'?", "id": "Membagi satu jaringan besar menjadi sub-jaringan." },
  { "en": "Tujuan utama 'subnetting'?", "id": "Mengurangi lalu lintas broadcast dan efisiensi IP." },
  { "en": "Apa itu 'VLSM'?", "id": "Variable Length Subnet Masking." },
  { "en": "Apa keuntungan VLSM?", "id": "Alokasi alamat IP menjadi jauh lebih efisien." },
  { "en": "Apa itu 'supernetting'?", "id": "Menggabungkan beberapa jaringan kecil menjadi satu." },
  { "en": "Apa itu IPv6?", "id": "Internet Protocol version 6." },
  { "en": "Mengapa IPv6 diciptakan?", "id": "Karena alamat IPv4 sudah hampir habis." },
  { "en": "Berapa bit panjang Alamat IPv6?", "id": "Terdiri dari 128 bit." },
  { "en": "Bagaimana format penulisan IPv6?", "id": "Delapan blok heksadesimal dipisahkan titik dua." },
  { "en": "Apa itu 'heksadesimal'?", "id": "Sistem bilangan berbasis 16 (0-9, A-F)." },
  { "en": "Bagaimana cara menyingkat alamat IPv6?", "id": "Blok nol dihilangkan dengan '::'." },
  { "en": "Apa itu Alamat MAC?", "id": "Alamat fisik unik dari kartu jaringan." },
  { "en": "Apa kepanjangan dari MAC?", "id": "Media Access Control." },
  { "en": "Berapa bit panjang Alamat MAC?", "id": "Terdiri dari 48 bit." },
  { "en": "Bagaimana format penulisan Alamat MAC?", "id": "Enam blok heksadesimal dipisahkan titik dua/strip." },
  { "en": "Siapa yang menetapkan Alamat MAC?", "id": "Produsen perangkat keras (pabrikan)." },
  { "en": "Apakah Alamat MAC bisa diubah?", "id": "Bisa diubah (spoofing), tapi aslinya permanen." },
  { "en": "Di lapisan OSI mana Alamat MAC bekerja?", "id": "Bekerja di Data Link Layer (Lapisan 2)." },
  { "en": "Apa itu ARP?", "id": "Address Resolution Protocol." },
  { "en": "Apa fungsi dari ARP?", "id": "Memetakan Alamat IP ke Alamat MAC." },
  { "en": "Bagaimana cara kerja ARP?", "id": "Mengirim broadcast 'siapa punya IP ini?'." },
  { "en": "Apa itu 'ARP cache'?", "id": "Tabel sementara berisi pemetaan IP ke MAC." },
  { "en": "Apa itu RARP?", "id": "Reverse Address Resolution Protocol." },
  { "en": "Fungsi RARP?", "id": "Memetakan Alamat MAC ke Alamat IP." },
  { "en": "Apa itu TCP?", "id": "Transmission Control Protocol." },
  { "en": "Apa sifat utama TCP?", "id": "Andal (reliable) dan berorientasi koneksi." },
  { "en": "Apa maksud 'berorientasi koneksi'?", "id": "Harus membangun koneksi sebelum transfer data." },
  { "en": "Apa itu 'three-way handshake'?", "id": "Proses membangun koneksi TCP (SYN, SYN-ACK, ACK)." },
  { "en": "Apa itu 'acknowledgment' (ACK)?", "id": "Pesan konfirmasi bahwa data telah diterima." },
  { "en": "Bagaimana TCP memastikan keandalan?", "id": "Dengan ACK dan pengiriman ulang (retransmission)." },
  { "en": "Apa itu UDP?", "id": "User Datagram Protocol." },
  { "en": "Apa sifat utama UDP?", "id": "Cepat, tidak andal, tanpa koneksi." },
  { "en": "Kapan UDP lebih baik digunakan?", "id": "Streaming video, game online, VoIP." },
  { "en": "Apa itu port jaringan?", "id": "Nomor untuk mengidentifikasi aplikasi atau layanan." },
  { "en": "Berapa rentang 'well-known ports'?", "id": "0 sampai 1023." },
  { "en": "Port 80 untuk apa?", "id": "HTTP (Hypertext Transfer Protocol)." },
  { "en": "Port 443 untuk apa?", "id": "HTTPS (HTTP Secure)." },
  { "en": "Port 21 untuk apa?", "id": "FTP (File Transfer Protocol)." },
  { "en": "Port 22 untuk apa?", "id": "SSH (Secure Shell)." },
  { "en": "Port 25 untuk apa?", "id": "SMTP (Simple Mail Transfer Protocol)." },
  { "en": "Port 53 untuk apa?", "id": "DNS (Domain Name System)." },
  { "en": "Apa itu 'socket'?", "id": "Kombinasi dari alamat IP dan nomor port." },
  { "en": "Apa itu enkapsulasi?", "id": "Proses membungkus data dengan header." },
  { "en": "Data di Transport Layer disebut apa?", "id": "Segment (untuk TCP)." },
  { "en": "Data di Network Layer disebut apa?", "id": "Packet." },
  { "en": "Data di Data Link Layer disebut apa?", "id": "Frame." },
  { "en": "Apa itu 'header'?", "id": "Informasi kontrol di awal data." },
  { "en": "Apa itu 'payload'?", "id": "Data inti yang sebenarnya dikirim." },
  { "en": "Apa itu HTTP?", "id": "Hypertext Transfer Protocol." },
  { "en": "Apa itu HTTPS?", "id": "Versi aman dari HTTP dengan enkripsi." },
  { "en": "Apa itu FTP?", "id": "Protokol untuk mentransfer file." },
  { "en": "Apa itu SMTP?", "id": "Protokol untuk mengirim email." },
  { "en": "Apa itu POP3 dan IMAP?", "id": "Protokol untuk menerima email." },
  { "en": "Apa itu DNS?", "id": "Domain Name System." },
  { "en": "Fungsi utama DNS?", "id": "Menerjemahkan nama domain menjadi alamat IP." },
  { "en": "Apa itu 'domain name'?", "id": "Nama yang mudah diingat untuk sebuah situs." },
  { "en": "Contoh 'domain name'?", "id": "google.com, wikipedia.org." },
  { "en": "Apa itu DHCP?", "id": "Dynamic Host Configuration Protocol." },
  { "en": "Fungsi utama DHCP?", "id": "Memberikan alamat IP secara otomatis ke perangkat." },
  { "en": "Apa itu 'default gateway'?", "id": "Router untuk mengakses jaringan luar." },
  { "en": "Apa itu 'server DNS'?", "id": "Server yang menangani permintaan DNS." },
  { "en": "Apa itu hub?", "id": "Perangkat usang yang mengirim data ke semua port." },
  { "en": "Apa itu switch?", "id": "Perangkat cerdas yang mengirim data ke port tujuan." },
  { "en": "Apa itu router?", "id": "Perangkat yang menghubungkan jaringan berbeda." },
  { "en": "Fungsi utama router?", "id": "Menentukan rute terbaik untuk pengiriman paket." },
  { "en": "Apa itu 'routing table'?", "id": "Tabel yang digunakan router untuk menentukan rute." },
  { "en": "Apa itu 'bridge'?", "id": "Menghubungkan dua segmen LAN di lapisan Data Link." },
  { "en": "Apa itu 'repeater'?", "id": "Memperkuat sinyal untuk memperluas jangkauan." },
  { "en": "Perangkat apa yang bekerja di Physical Layer?", "id": "Hub, repeater, dan kabel." },
  { "en": "Perangkat apa yang bekerja di Data Link Layer?", "id": "Switch dan bridge." },
  { "en": "Perangkat apa yang bekerja di Network Layer?", "id": "Router." },
  { "en": "Apa itu kabel UTP?", "id": "Unshielded Twisted Pair." },
  { "en": "Mengapa kabelnya dipilin (twisted)?", "id": "Untuk mengurangi interferensi elektromagnetik." },
  { "en": "Apa itu kabel STP?", "id": "Shielded Twisted Pair." },
  { "en": "Apa itu kabel koaksial?", "id": "Kabel dengan konduktor pusat dan pelindung." },
  { "en": "Apa itu kabel serat optik?", "id": "Mengirim data menggunakan pulsa cahaya." },
  { "en": "Kelebihan utama serat optik?", "id": "Kecepatan sangat tinggi, tahan interferensi." },
  { "en": "Apa itu NIC?", "id": "Network Interface Card." },
  { "en": "Fungsi NIC?", "id": "Menghubungkan komputer ke jaringan." },
  { "en": "Apa itu 'collision domain'?", "id": "Area di mana tabrakan data bisa terjadi." },
  { "en": "Perangkat apa yang bisa memisahkan 'collision domain'?", "id": "Switch dan bridge." },
  { "en": "Apa itu 'broadcast domain'?", "id": "Area di mana broadcast frame diterima." },
  { "en": "Perangkat apa yang bisa memisahkan 'broadcast domain'?", "id": "Router." },
  { "en": "Apa itu CSMA/CD?", "id": "Carrier Sense Multiple Access/Collision Detection." },
  { "en": "Di mana CSMA/CD digunakan?", "id": "Pada jaringan Ethernet klasik." },
  { "en": "Apa itu CSMA/CA?", "id": "Carrier Sense Multiple Access/Collision Avoidance." },
  { "en": "Di mana CSMA/CA digunakan?", "id": "Pada jaringan nirkabel (Wi-Fi)." },
  { "en": "Apa itu 'ping'?", "id": "Perintah untuk menguji konektivitas jaringan." },
  { "en": "Apa itu 'traceroute' atau 'tracert'?", "id": "Perintah untuk melacak rute paket." },
  { "en": "Apa itu 'ipconfig' atau 'ifconfig'?", "id": "Perintah untuk melihat konfigurasi IP." },
  { "en": "Apa itu 'localhost'?", "id": "Nama lain untuk alamat loopback (127.0.0.1)." },
  { "en": "Apa itu 'firewall'?", "id": "Sistem keamanan yang menyaring lalu lintas jaringan." },
  { "en": "Apa itu 'proxy server'?", "id": "Server perantara antara pengguna dan internet." },
  { "en": "Fungsi utama proxy server?", "id": "Menyembunyikan IP asli, caching, dan filtering." },
  { "en": "Apa itu 'caching'?", "id": "Menyimpan data sementara untuk akses lebih cepat." },
  { "en": "Apa itu 'port forwarding'?", "id": "Meneruskan lalu lintas dari satu port ke IP lain." },
  { "en": "Apa itu 'VPN'?", "id": "Virtual Private Network." },
  { "en": "Fungsi utama VPN?", "id": "Membuat koneksi aman melalui jaringan publik." },
  { "en": "Apa itu 'tunneling' pada VPN?", "id": "Membungkus paket data di dalam paket lain." },
  { "en": "Apa itu 'enkripsi'?", "id": "Mengubah data menjadi kode rahasia." },
  { "en": "Apa itu 'dekripsi'?", "id": "Mengembalikan data terenkripsi ke bentuk aslinya." },
  { "en": "Apa itu 'kunci' (key) dalam enkripsi?", "id": "Informasi rahasia untuk enkripsi/dekripsi." },
  { "en": "Apa itu 'enkripsi simetris'?", "id": "Kunci yang sama untuk enkripsi dan dekripsi." },
  { "en": "Apa itu 'enkripsi asimetris'?", "id": "Menggunakan kunci publik dan kunci privat." },
  { "en": "Apa itu 'kunci publik'?", "id": "Kunci yang dapat dibagikan secara bebas." },
  { "en": "Apa itu 'kunci privat'?", "id": "Kunci yang harus dijaga kerahasiaannya." },
  { "en": "Apa itu 'SSL/TLS'?", "id": "Protokol untuk mengamankan koneksi internet." },
  { "en": "Apa kepanjangan dari SSL?", "id": "Secure Sockets Layer." },
  { "en": "Apa kepanjangan dari TLS?", "id": "Transport Layer Security." },
  { "en": "Apa itu 'sertifikat digital'?", "id": "File untuk memverifikasi identitas di internet." },
  { "en": "Siapa yang mengeluarkan sertifikat digital?", "id": "Otoritas Sertifikat (Certificate Authority)." },
  { "en": "Apa itu 'client-server model'?", "id": "Model jaringan dengan klien dan server." },
  { "en": "Apa itu 'peer-to-peer' (P2P) model?", "id": "Setiap perangkat bisa jadi klien dan server." },
  { "en": "Apa itu 'bandwidth'?", "id": "Kapasitas maksimum transfer data." },
  { "en": "Apa itu 'throughput'?", "id": "Laju transfer data aktual yang tercapai." },
  { "en": "Apa itu 'latensi'?", "id": "Waktu tunda dalam pengiriman data." },
  { "en": "Apa itu 'ping'?", "id": "Waktu bolak-balik (round-trip time) ke host." },
  { "en": "Apa itu 'jitter'?", "id": "Variasi dalam latensi paket." },
  { "en": "Mengapa 'jitter' buruk untuk VoIP?", "id": "Menyebabkan suara terputus-putus atau kacau." },
  { "en": "Apa itu 'packet loss'?", "id": "Paket data yang gagal sampai tujuan." },
  { "en": "Apa itu 'protocol stack'?", "id": "Implementasi dari lapisan-lapisan protokol." },
  { "en": "Di lapisan mana TCP beroperasi?", "id": "Transport Layer (Lapisan 4)." },
  { "en": "Di lapisan mana IP beroperasi?", "id": "Internet Layer (Lapisan 3)." },
  { "en": "Di lapisan mana Ethernet beroperasi?", "id": "Network Access Layer (Lapisan 1 & 2)." },
  { "en": "Apa itu 'header' frame Ethernet?", "id": "Berisi alamat MAC sumber dan tujuan." },
  { "en": "Apa itu 'FCS (Frame Check Sequence)'?", "id": "Kode untuk memeriksa eror pada frame." },
  { "en": "Apa itu 'CRC (Cyclic Redundancy Check)'?", "id": "Metode umum untuk menghitung FCS." },
  { "en": "Apa itu 'MTU'?", "id": "Maximum Transmission Unit." },
  { "en": "Apa arti MTU?", "id": "Ukuran paket maksimum yang bisa dikirim." },
  { "en": "Apa itu 'fragmentasi' paket?", "id": "Memecah paket besar menjadi paket lebih kecil." },
  { "en": "Apa itu 'flow control'?", "id": "Mekanisme untuk mencegah pengirim membanjiri penerima." },
  { "en": "Apa itu 'sliding window'?", "id": "Metode TCP untuk 'flow control'." },
  { "en": "Apa itu 'congestion control'?", "id": "Mekanisme untuk mencegah kepadatan di jaringan." },
  { "en": "Apa itu 'unicast'?", "id": "Pengiriman data dari satu ke satu." },
  { "en": "Apa itu 'multicast'?", "id": "Pengiriman data dari satu ke banyak (grup)." },
  { "en": "Apa itu 'broadcast'?", "id": "Pengiriman data dari satu ke semua." },
  { "en": "Apa itu 'anycast'?", "id": "Pengiriman data dari satu ke terdekat." },
  { "en": "Alamat IP 224.0.0.1 untuk apa?", "id": "Alamat multicast untuk semua host." },
  { "en": "Alamat IP 255.255.255.255 untuk apa?", "id": "Alamat broadcast terbatas." },
  { "en": "Apa itu NAT?", "id": "Network Address Translation." },
  { "en": "Fungsi utama NAT?", "id": "Memetakan IP privat ke satu IP publik." },
  { "en": "Apa itu PAT (Port Address Translation)?", "id": "Bentuk NAT yang paling umum (NAT Overload)." },
  { "en": "Apa itu 'demilitarized zone' (DMZ)?", "id": "Sub-jaringan terisolasi untuk server publik." },
  { "en": "Apa itu 'Intrusion Detection System' (IDS)?", "id": "Sistem yang mendeteksi aktivitas mencurigakan." },
  { "en": "Apa itu 'Intrusion Prevention System' (IPS)?", "id": "IDS yang juga bisa memblokir serangan." },
  { "en": "Apa itu 'MAC flooding'?", "id": "Serangan yang membanjiri tabel MAC switch." },
  { "en": "Apa itu 'ARP spoofing'?", "id": "Serangan 'man-in-the-middle' menggunakan ARP." },
  { "en": "Apa itu 'DHCP starvation'?", "id": "Serangan yang menghabiskan semua alokasi IP." },
  { "en": "Apa itu 'VLAN'?", "id": "Virtual Local Area Network." },
  { "en": "Apa fungsi VLAN?", "id": "Membuat beberapa broadcast domain di satu switch." },
  { "en": "Apa itu 'trunking'?", "id": "Membawa lalu lintas beberapa VLAN antar switch." },
  { "en": "Protokol 'trunking' yang umum?", "id": "IEEE 802.1Q." },
  { "en": "Apa itu 'native VLAN'?", "id": "VLAN yang tidak diberi tag di trunk." },
  { "en": "Apa itu 'Spanning Tree Protocol' (STP)?", "id": "Protokol untuk mencegah 'loop' di jaringan." },
  { "en": "Apa itu 'routing loop'?", "id": "Paket berputar-putar tanpa henti di jaringan." },
  { "en": "Apa itu 'TTL (Time to Live)'?", "id": "Batas hop paket untuk mencegah loop." },
  { "en": "Apa itu 'routing statis'?", "id": "Rute yang dikonfigurasi secara manual." },
  { "en": "Apa itu 'routing dinamis'?", "id": "Router saling bertukar informasi rute." },
  { "en": "Contoh protokol routing dinamis?", "id": "RIP, OSPF, EIGRP, BGP." },
  { "en": "Apa itu 'administrative distance'?", "id": "Ukuran kepercayaan terhadap sumber rute." },
  { "en": "Apa itu 'metric' dalam routing?", "id": "Ukuran untuk menentukan rute terbaik." },
  { "en": "Metric yang digunakan RIP?", "id": "Hop count (jumlah router yang dilewati)." },
  { "en": "Apa itu 'hop count'?", "id": "Jumlah router yang dilewati paket." },
  { "en": "Apa itu 'interior gateway protocol' (IGP)?", "id": "Protokol routing di dalam satu sistem otonom." },
  { "en": "Contoh IGP?", "id": "OSPF dan EIGRP." },
  { "en": "Apa itu 'exterior gateway protocol' (EGP)?", "id": "Protokol routing antar sistem otonom." },
  { "en": "Contoh EGP?", "id": "BGP (Border Gateway Protocol)." },
  { "en": "Apa itu 'autonomous system' (AS)?", "id": "Sekelompok jaringan di bawah satu administrasi." },
  { "en": "Apa itu 'link-state protocol'?", "id": "Router membangun peta topologi jaringan (contoh: OSPF)." },
  { "en": "Apa itu 'distance-vector protocol'?", "id": "Router hanya tahu tentang tetangganya (contoh: RIP)." },
  { "en": "Apa itu 'Quality of Service' (QoS)?", "id": "Memprioritaskan lalu lintas data penting." },
  { "en": "Mengapa QoS penting?", "id": "Untuk aplikasi sensitif seperti suara dan video." },
  { "en": "Apa itu 'ICMP'?", "id": "Internet Control Message Protocol." },
  { "en": "Fungsi ICMP?", "id": "Untuk pesan eror dan diagnostik (contoh: Ping)." },
  { "en": "Apa itu 'API'?", "id": "Application Programming Interface." },
  { "en": "Apa itu 'client'?", "id": "Perangkat yang meminta layanan." },
  { "en": "Apa itu 'server'?", "id": "Perangkat yang menyediakan layanan." },
  { "en": "Apa itu 'URL'?", "id": "Uniform Resource Locator." },
  { "en": "Bagian-bagian dari URL?", "id": "Protokol, domain, dan path." },
  { "en": "Apa itu 'browser' web?", "id": "Aplikasi untuk mengakses World Wide Web." },
  { "en": "Apa itu 'World Wide Web'?", "id": "Sistem informasi dokumen hypertext di internet." },
  { "en": "Apa itu 'Internet'?", "id": "Jaringan global dari jaringan komputer." },
  { "en": "Apa itu 'Intranet'?", "id": "Jaringan privat di dalam sebuah organisasi." },
  { "en": "Apa itu 'Extranet'?", "id": "Intranet yang bisa diakses oleh pihak luar." },
  { "en": "Apa itu 'cloud computing'?", "id": "Layanan komputasi melalui internet." },
  { "en": "Apa itu 'IaaS'?", "id": "Infrastructure as a Service." },
  { "en": "Apa itu 'PaaS'?", "id": "Platform as a Service." },
  { "en": "Apa itu 'SaaS'?", "id": "Software as a Service." },
  { "en": "Apa itu 'media transmisi'?", "id": "Jalur fisik untuk mengirim data." },
  { "en": "Contoh media transmisi 'terpandu'?", "id": "Kabel tembaga, serat optik." },
  { "en": "Contoh media transmisi 'tak terpandu'?", "id": "Udara (gelombang radio)." },
  { "en": "Apa itu 'noise' dalam jaringan?", "id": "Sinyal acak yang tidak diinginkan." },
  { "en": "Apa itu 'atenuasi' (attenuation)?", "id": "Pelemahan sinyal seiring jarak." },
  { "en": "Apa itu 'distorsi'?", "id": "Perubahan bentuk sinyal saat transmisi." },
  { "en": "Apa itu 'sinyal analog'?", "id": "Sinyal kontinu yang nilainya bervariasi." },
  { "en": "Apa itu 'sinyal digital'?", "id": "Sinyal diskrit dengan level tegangan tertentu." },
  { "en": "Apa itu 'modem'?", "id": "Modulator-Demodulator." },
  { "en": "Fungsi modem?", "id": "Mengubah sinyal digital ke analog dan sebaliknya." },
  { "en": "Apa itu 'bandwidth'?", "id": "Rentang frekuensi yang bisa dilewatkan." },
  { "en": "Satuan bandwidth?", "id": "Hertz (Hz)." },
  { "en": "Apa itu 'laju data' (data rate)?", "id": "Kecepatan transfer data (bit per detik)." },
  { "en": "Satuan laju data?", "id": "bps (bits per second)." },
  { "en": "Apa itu 'encoding'?", "id": "Proses mengubah data menjadi sinyal." },
  { "en": "Apa itu 'Manchester encoding'?", "id": "Transisi di tengah bit untuk sinkronisasi." },
  { "en": "Apa itu 'multiplexing'?", "id": "Mengirim banyak sinyal melalui satu media." },
  { "en": "Apa itu FDM?", "id": "Frequency-Division Multiplexing." },
  { "en": "Apa itu TDM?", "id": "Time-Division Multiplexing." },
  { "en": "Apa itu 'error detection'?", "id": "Proses mendeteksi kesalahan dalam data." },
  { "en": "Contoh metode deteksi eror?", "id": "Parity check, checksum, CRC." },
  { "en": "Apa itu 'parity bit'?", "id": "Satu bit tambahan untuk deteksi eror." },
  { "en": "Apa itu 'error correction'?", "id": "Proses memperbaiki kesalahan dalam data." },
  { "en": "Contoh metode koreksi eror?", "id": "Hamming code." },
  { "en": "Apa itu 'flow control'?", "id": "Mengatur laju pengiriman data." },
  { "en": "Apa itu 'congestion control'?", "id": "Mengatasi kepadatan lalu lintas di jaringan." },
  { "en": "Apa itu 'OSI Layer 1'?", "id": "Physical Layer." },
  { "en": "Apa itu 'OSI Layer 2'?", "id": "Data Link Layer." },
  { "en": "Apa itu 'OSI Layer 3'?", "id": "Network Layer." },
  { "en": "Apa itu 'OSI Layer 4'?", "id": "Transport Layer." },
  { "en": "Apa itu 'OSI Layer 5'?", "id": "Session Layer." },
  { "en": "Apa itu 'OSI Layer 6'?", "id": "Presentation Layer." },
  { "en": "Apa itu 'OSI Layer 7'?", "id": "Application Layer." },
  { "en": "Tugas utama Physical Layer?", "id": "Mengirim bit melalui media fisik." },
  { "en": "Tugas utama Data Link Layer?", "id": "Mengatur akses ke media dan pengalamatan fisik." },
  { "en": "Tugas utama Network Layer?", "id": "Menentukan rute dan pengalamatan logis." },
  { "en": "Tugas utama Transport Layer?", "id": "Menyediakan koneksi end-to-end yang andal." },
  { "en": "Tugas utama Session Layer?", "id": "Membuat, mengelola, dan mengakhiri sesi." },
  { "en": "Tugas utama Presentation Layer?", "id": "Menangani format dan enkripsi data." },
  { "en": "Tugas utama Application Layer?", "id": "Menyediakan layanan jaringan ke aplikasi." },
  { "en": "Apa itu 'header'?", "id": "Informasi tambahan yang ditambahkan ke data." },
  { "en": "Apa itu 'trailer'?", "id": "Informasi tambahan di akhir data." },
  { "en": "Apa itu 'segment'?", "id": "PDU dari Transport Layer." },
  { "en": "Apa itu 'packet'?", "id": "PDU dari Network Layer." },
  { "en": "Apa itu 'frame'?", "id": "PDU dari Data Link Layer." },
  { "en": "Apa itu 'bit'?", "id": "PDU dari Physical Layer." },
  { "en": "Apa itu 'logical address'?", "id": "Alamat IP." },
  { "en": "Apa itu 'physical address'?", "id": "Alamat MAC." },
  { "en": "Apa itu 'port address'?", "id": "Alamat layanan atau aplikasi." },
  { "en": "Apa itu 'routing'?", "id": "Proses menemukan jalur terbaik di jaringan." },
  { "en": "Apa itu 'static routing'?", "id": "Rute ditentukan secara manual oleh admin." },
  { "en": "Apa itu 'dynamic routing'?", "id": "Router belajar rute secara otomatis." },
  { "en": "Apa itu 'routing protocol'?", "id": "Aturan yang digunakan untuk routing dinamis." },
  { "en": "Contoh 'routing protocol'?", "id": "RIP, OSPF, BGP." },
  { "en": "Apa itu 'hop count'?", "id": "Jumlah router yang dilewati paket." },
  { "en": "Apa itu 'metric'?", "id": "Nilai yang digunakan untuk mengukur rute." },
  { "en": "Apa itu 'default route'?", "id": "Rute yang digunakan jika tidak ada rute lain." },
  { "en": "Apa itu 'loopback address'?", "id": "Alamat IP untuk menguji tumpukan protokol." },
  { "en": "Alamat loopback IPv4?", "id": "127.0.0.1." },
  { "en": "Apa itu 'ping of death'?", "id": "Serangan menggunakan paket ping yang salah." },
  { "en": "Apa itu 'IP spoofing'?", "id": "Memalsukan alamat IP sumber." },
  { "en": "Apa itu 'denial-of-service' (DoS)?", "id": "Serangan untuk membuat layanan tidak tersedia." },
  { "en": "Apa itu 'distributed DoS' (DDoS)?", "id": "Serangan DoS dari banyak sumber." },
  { "en": "Apa itu 'botnet'?", "id": "Jaringan komputer yang terinfeksi malware." },
  { "en": "Apa itu 'malware'?", "id": "Perangkat lunak berbahaya." },
  { "en": "Apa itu 'virus'?", "id": "Malware yang mereplikasi diri." },
  { "en": "Apa itu 'worm'?", "id": "Virus yang menyebar melalui jaringan." },
  { "en": "Apa itu 'trojan horse'?", "id": "Malware yang menyamar sebagai program sah." },
  { "en": "Apa itu 'spyware'?", "id": "Malware yang memata-matai aktivitas pengguna." },
  { "en": "Apa itu 'ransomware'?", "id": "Malware yang mengenkripsi file dan meminta tebusan." },
  { "en": "Apa itu 'phishing'?", "id": "Upaya menipu untuk mendapatkan informasi sensitif." },
  { "en": "Apa itu 'social engineering'?", "id": "Memanipulasi orang untuk mendapatkan informasi." },
  { "en": "Apa itu 'man-in-the-middle' attack?", "id": "Menyadap komunikasi antara dua pihak." },
  { "en": "Apa itu 'Wi-Fi'?", "id": "Teknologi jaringan nirkabel." },
  { "en": "Standar Wi-Fi?", "id": "IEEE 802.11." },
  { "en": "Apa itu 'access point' (AP)?", "id": "Perangkat pusat untuk jaringan Wi-Fi." },
  { "en": "Apa itu 'SSID'?", "id": "Nama dari sebuah jaringan Wi-Fi." },
  { "en": "Apa itu 'authentication'?", "id": "Proses memverifikasi identitas pengguna." },
  { "en": "Apa itu 'WEP'?", "id": "Enkripsi Wi-Fi lama yang tidak aman." },
  { "en": "Apa itu 'WPA/WPA2'?", "id": "Enkripsi Wi-Fi yang lebih aman." },
  { "en": "Apa itu 'MAC filtering'?", "id": "Membatasi akses berdasarkan alamat MAC." },
  { "en": "Apa itu 'hotspot'?", "id": "Area publik yang menyediakan akses Wi-Fi." },
  { "en": "Apa itu 'Bluetooth'?", "id": "Teknologi nirkabel untuk jarak pendek." },
  { "en": "Apa itu 'pairing'?", "id": "Proses menghubungkan dua perangkat Bluetooth." },
  { "en": "Apa itu 'Ethernet'?", "id": "Teknologi dominan untuk jaringan LAN kabel." },
  { "en": "Kecepatan Fast Ethernet?", "id": "100 Mbps." },
  { "en": "Kecepatan Gigabit Ethernet?", "id": "1000 Mbps atau 1 Gbps." },
  { "en": "Apa itu 'Power over Ethernet' (PoE)?", "id": "Mengirimkan daya listrik melalui kabel Ethernet." },
  { "en": "Apa itu 'DNS server'?", "id": "Server yang menerjemahkan nama domain." },
  { "en": "Apa itu 'DHCP server'?", "id": "Server yang memberikan alamat IP otomatis." },
  { "en": "Apa itu 'file server'?", "id": "Server untuk menyimpan dan berbagi file." },
  { "en": "Apa itu 'web server'?", "id": "Server yang menghosting situs web." },
  { "en": "Apa itu 'mail server'?", "id": "Server untuk menangani email." },
  { "en": "Apa itu 'port scanning'?", "id": "Mencari port terbuka pada sebuah host." },
  { "en": "Apa itu 'packet sniffing'?", "id": "Menangkap dan menganalisis lalu lintas data." },
  { "en": "Apa itu 'network topology'?", "id": "Tata letak fisik atau logis dari jaringan." },
  { "en": "Topologi apa yang menggunakan satu kabel pusat?", "id": "Topologi bus." },
  { "en": "Topologi apa yang semua nodenya terhubung ke pusat?", "id": "Topologi bintang (star)." },
  { "en": "Topologi apa yang paling andal (reliable)?", "id": "Topologi mesh." },
  { "en": "Apa itu 'terminator'?", "id": "Menyerap sinyal di ujung kabel bus." },
  { "en": "Apa itu 'Model OSI'?", "id": "Model konseptual tujuh lapisan untuk jaringan." },
  { "en": "Lapisan mana yang bertanggung jawab atas routing?", "id": "Network Layer (Lapisan 3)." },
  { "en": "Lapisan mana yang berurusan dengan alamat MAC?", "id": "Data Link Layer (Lapisan 2)." },
  { "en": "Lapisan mana yang menangani transmisi bit?", "id": "Physical Layer (Lapisan 1)." },
  { "en": "Lapisan mana yang menyediakan koneksi andal?", "id": "Transport Layer (Lapisan 4)." },
  { "en": "Apa itu 'enkapsulasi'?", "id": "Proses menambahkan header ke data." },
  { "en": "Apa itu 'dekapsulasi'?", "id": "Proses melepaskan header dari data." },
  { "en": "Apa itu 'Alamat IP'?", "id": "Alamat logis unik dari sebuah perangkat." },
  { "en": "Berapa bit panjang alamat IPv4?", "id": "32 bit." },
  { "en": "Berapa bit panjang alamat IPv6?", "id": "128 bit." },
  { "en": "Apa itu 'subnet mask'?", "id": "Membedakan Network ID dan Host ID." },
  { "en": "Apa itu 'CIDR'?", "id": "Classless Inter-Domain Routing." },
  { "en": "Apa arti '/16' dalam notasi CIDR?", "id": "16 bit pertama adalah Network ID." },
  { "en": "Apa itu 'alamat IP privat'?", "id": "Alamat untuk digunakan dalam jaringan lokal." },
  { "en": "Rentang IP privat kelas B?", "id": "172.16.0.0 sampai 172.31.255.255." },
  { "en": "Apa itu 'NAT'?", "id": "Network Address Translation." },
  { "en": "Fungsi utama NAT?", "id": "Memetakan banyak IP privat ke satu IP publik." },
  { "en": "Apa itu 'Alamat MAC'?", "id": "Alamat fisik unik dari kartu jaringan." },
  { "en": "Di lapisan mana switch beroperasi?", "id": "Data Link Layer (Lapisan 2)." },
  { "en": "Di lapisan mana router beroperasi?", "id": "Network Layer (Lapisan 3)." },
  { "en": "Di lapisan mana hub beroperasi?", "id": "Physical Layer (Lapisan 1)." },
  { "en": "Apa perbedaan utama switch dan hub?", "id": "Switch cerdas, hub hanya meneruskan sinyal." },
  { "en": "Apa itu 'ARP'?", "id": "Address Resolution Protocol." },
  { "en": "Fungsi ARP?", "id": "Menemukan alamat MAC dari alamat IP." },
  { "en": "Apa itu 'default gateway'?", "id": "Jalan keluar dari jaringan lokal." },
  { "en": "Biasanya apa alamat IP 'default gateway'?", "id": "Alamat IP dari router." },
  { "en": "Apa itu 'TCP'?", "id": "Transmission Control Protocol." },
  { "en": "Sifat utama TCP?", "id": "Andal (reliable) dan berorientasi koneksi." },
  { "en": "Apa itu 'UDP'?", "id": "User Datagram Protocol." },
  { "en": "Sifat utama UDP?", "id": "Cepat tapi tidak andal." },
  { "en": "Apa itu 'three-way handshake'?", "id": "Proses memulai koneksi TCP." },
  { "en": "Paket apa saja dalam 'three-way handshake'?", "id": "SYN, SYN-ACK, ACK." },
  { "en": "Apa itu 'port'?", "id": "Nomor yang mengidentifikasi sebuah aplikasi." },
  { "en": "Port 23 untuk apa?", "id": "Telnet (koneksi terminal jarak jauh)." },
  { "en": "Port 110 untuk apa?", "id": "POP3 (menerima email)." },
  { "en": "Port 143 untuk apa?", "id": "IMAP (menerima email)." },
  { "en": "Apa itu 'socket'?", "id": "Kombinasi dari alamat IP dan port." },
  { "en": "Apa itu 'DNS'?", "id": "Domain Name System." },
  { "en": "Fungsi utama DNS?", "id": "Menerjemahkan nama domain ke alamat IP." },
  { "en": "Apa itu 'DHCP'?", "id": "Dynamic Host Configuration Protocol." },
  { "en": "Fungsi utama DHCP?", "id": "Memberikan konfigurasi IP secara otomatis." },
  { "en": "Apa itu 'server'?", "id": "Komputer yang menyediakan layanan." },
  { "en": "Apa itu 'klien'?", "id": "Komputer yang meminta layanan." },
  { "en": "Apa itu 'peer-to-peer'?", "id": "Setiap komputer bisa jadi klien dan server." },
  { "en": "Apa itu 'kabel UTP'?", "id": "Unshielded Twisted Pair." },
  { "en": "Mengapa kabelnya dipilin (twisted)?", "id": "Untuk mengurangi interferensi (crosstalk)." },
  { "en": "Apa itu 'crosstalk'?", "id": "Gangguan sinyal dari kabel tetangga." },
  { "en": "Apa itu 'kabel straight-through'?", "id": "Urutan pin di kedua ujung sama." },
  { "en": "Kapan kabel 'straight-through' digunakan?", "id": "Menghubungkan perangkat yang berbeda (PC ke switch)." },
  { "en": "Apa itu 'kabel cross-over'?", "id": "Urutan pin pengirim dan penerima disilangkan." },
  { "en": "Kapan kabel 'cross-over' digunakan?", "id": "Menghubungkan perangkat yang sama (PC ke PC)." },
  { "en": "Apa itu 'serat optik'?", "id": "Media transmisi menggunakan pulsa cahaya." },
  { "en": "Apa itu 'single-mode fiber'?", "id": "Untuk jarak jauh dengan bandwidth tinggi." },
  { "en": "Apa itu 'multi-mode fiber'?", "id": "Untuk jarak lebih pendek." },
  { "en": "Apa itu 'sinyal radio'?", "id": "Media transmisi nirkabel." },
  { "en": "Apa itu 'Wi-Fi'?", "id": "Jaringan nirkabel lokal (WLAN)." },
  { "en": "Apa itu 'SSID'?", "id": "Nama dari sebuah jaringan Wi-Fi." },
  { "en": "Apa itu 'enkripsi WPA2'?", "id": "Standar keamanan yang kuat untuk Wi-Fi." },
  { "en": "Apa itu 'Bluetooth'?", "id": "Teknologi nirkabel untuk jarak pendek." },
  { "en": "Apa itu 'broadcast domain'?", "id": "Area di mana frame broadcast diterima." },
  { "en": "Perangkat apa yang memisahkan 'broadcast domain'?", "id": "Router." },
  { "en": "Apa itu 'collision domain'?", "id": "Area di mana tabrakan data bisa terjadi." },
  { "en": "Perangkat apa yang memisahkan 'collision domain'?", "id": "Switch, bridge, dan router." },
  { "en": "Apa itu 'unicast'?", "id": "Komunikasi satu-ke-satu." },
  { "en": "Apa itu 'multicast'?", "id": "Komunikasi satu-ke-grup." },
  { "en": "Apa itu 'broadcast'?", "id": "Komunikasi satu-ke-semua." },
  { "en": "Apa itu 'VLAN'?", "id": "Virtual Local Area Network." },
  { "en": "Apa fungsi VLAN?", "id": "Membagi satu switch menjadi beberapa broadcast domain." },
  { "en": "Apa itu 'trunk port'?", "id": "Port yang membawa lalu lintas beberapa VLAN." },
  { "en": "Apa itu 'access port'?", "id": "Port yang hanya menjadi anggota satu VLAN." },
  { "en": "Apa itu 'STP (Spanning Tree Protocol)'?", "id": "Mencegah terjadinya 'loop' pada jaringan switch." },
  { "en": "Apa itu 'routing loop'?", "id": "Paket berputar-putar tanpa henti antar router." },
  { "en": "Apa itu 'TTL (Time To Live)'?", "id": "Membatasi umur paket untuk mencegah loop." },
  { "en": "Apa itu 'ping'?", "id": "Utilitas untuk menguji konektivitas." },
  { "en": "Protokol apa yang digunakan 'ping'?", "id": "ICMP (Internet Control Message Protocol)." },
  { "en": "Apa itu 'traceroute'?", "id": "Menampilkan rute yang dilewati paket." },
  { "en": "Apa itu 'netstat'?", "id": "Menampilkan koneksi jaringan yang aktif." },
  { "en": "Apa itu 'nslookup'?", "id": "Utilitas untuk melakukan query ke DNS server." },
  { "en": "Apa itu 'firewall'?", "id": "Sistem keamanan yang memfilter lalu lintas." },
  { "en": "Apa itu 'ACL (Access Control List)'?", "id": "Daftar aturan untuk mengizinkan/memblokir lalu lintas." },
  { "en": "Apa itu 'proxy server'?", "id": "Server perantara antara klien dan internet." },
  { "en": "Apa itu 'reverse proxy'?", "id": "Proxy yang bertindak atas nama server." },
  { "en": "Apa itu 'load balancer'?", "id": "Mendistribusikan beban ke beberapa server." },
  { "en": "Apa itu 'caching'?", "id": "Menyimpan data sementara untuk akses cepat." },
  { "en": "Apa itu 'cookie'?", "id": "Data kecil yang disimpan oleh browser." },
  { "en": "Apa itu 'URL'?", "id": "Uniform Resource Locator, alamat web." },
  { "en": "Apa itu 'HTTP'?", "id": "Hypertext Transfer Protocol." },
  { "en": "Apa itu 'HTTPS'?", "id": "Versi aman dari HTTP." },
  { "en": "Apa itu 'FTP'?", "id": "File Transfer Protocol." },
  { "en": "Apa itu 'SMTP'?", "id": "Simple Mail Transfer Protocol." },
  { "en": "Apa itu 'SSH'?", "id": "Secure Shell, untuk akses remote yang aman." },
  { "en": "Apa itu 'Telnet'?", "id": "Akses remote yang tidak aman." },
  { "en": "Apa itu 'API'?", "id": "Application Programming Interface." },
  { "en": "Apa itu 'JSON'?", "id": "JavaScript Object Notation." },
  { "en": "Apa itu 'XML'?", "id": "Extensible Markup Language." },
  { "en": "Apa itu 'server DHCP'?", "id": "Server yang memberikan alamat IP secara otomatis." },
  { "en": "Apa itu 'DNS server'?", "id": "Server yang menerjemahkan nama domain ke IP." },
  { "en": "Apa itu 'web server'?", "id": "Server yang menyimpan dan mengirimkan halaman web." },
  { "en": "Apa itu 'file server'?", "id": "Server untuk menyimpan dan berbagi file." },
  { "en": "Apa itu 'mail server'?", "id": "Server yang menangani pengiriman dan penerimaan email." },
  { "en": "Apa itu 'proxy server'?", "id": "Server perantara antara pengguna dan internet." },
  { "en": "Model jaringan dengan server dan klien?", "id": "Model Client-Server." },
  { "en": "Model jaringan tanpa server terpusat?", "id": "Model Peer-to-Peer (P2P)." },
  { "en": "Apa itu 'topologi' jaringan?", "id": "Tata letak fisik atau logis dari jaringan." },
  { "en": "Topologi dengan satu kabel utama?", "id": "Topologi Bus." },
  { "en": "Topologi terhubung ke perangkat pusat?", "id": "Topologi Bintang (Star)." },
  { "en": "Topologi di mana semua node terhubung?", "id": "Topologi Mesh." },
  { "en": "Apa itu 'repeater'?", "id": "Memperkuat sinyal untuk memperpanjang jangkauan." },
  { "en": "Di lapisan OSI mana repeater bekerja?", "id": "Physical Layer (Lapisan 1)." },
  { "en": "Apa itu 'hub'?", "id": "Membagi koneksi ke beberapa perangkat (usang)." },
  { "en": "Di lapisan OSI mana hub bekerja?", "id": "Physical Layer (Lapisan 1)." },
  { "en": "Apa itu 'bridge'?", "id": "Menghubungkan dua segmen LAN." },
  { "en": "Di lapisan OSI mana bridge bekerja?", "id": "Data Link Layer (Lapisan 2)." },
  { "en": "Apa itu 'switch'?", "id": "Menghubungkan perangkat di LAN secara cerdas." },
  { "en": "Di lapisan OSI mana switch bekerja?", "id": "Data Link Layer (Lapisan 2)." },
  { "en": "Apa itu 'router'?", "id": "Menghubungkan jaringan yang berbeda." },
  { "en": "Di lapisan OSI mana router bekerja?", "id": "Network Layer (Lapisan 3)." },
  { "en": "Apa itu 'gateway'?", "id": "Pintu gerbang antara dua jaringan berbeda." },
  { "en": "Apa itu 'firewall'?", "id": "Sistem keamanan untuk memfilter lalu lintas." },
  { "en": "Apa itu 'Alamat IP'?", "id": "Alamat logis unik dari sebuah perangkat." },
  { "en": "Apa itu 'subnetting'?", "id": "Membagi sebuah jaringan menjadi sub-jaringan." },
  { "en": "Apa itu 'VLSM'?", "id": "Variable Length Subnet Masking." },
  { "en": "Apa itu 'supernetting'?", "id": "Menggabungkan beberapa jaringan menjadi satu rute." },
  { "en": "Apa itu 'CIDR'?", "id": "Classless Inter-Domain Routing." },
  { "en": "Notasi CIDR untuk 255.255.255.0?", "id": "Notasinya adalah /24." },
  { "en": "Apa itu 'IPv4'?", "id": "Internet Protocol versi 4 (32-bit)." },
  { "en": "Apa itu 'IPv6'?", "id": "Internet Protocol versi 6 (128-bit)." },
  { "en": "Mengapa IPv6 dibuat?", "id": "Karena alamat IPv4 sudah hampir habis." },
  { "en": "Apa itu 'dual stack'?", "id": "Menjalankan IPv4 dan IPv6 secara bersamaan." },
  { "en": "Apa itu 'tunneling' IPv6?", "id": "Membungkus paket IPv6 di dalam paket IPv4." },
  { "en": "Apa itu 'alamat unicast'?", "id": "Alamat untuk satu antarmuka tunggal." },
  { "en": "Apa itu 'alamat multicast'?", "id": "Alamat untuk sekelompok antarmuka." },
  { "en": "Apa itu 'alamat anycast'?", "id": "Alamat untuk sekelompok, dikirim ke yang terdekat." },
  { "en": "Alamat loopback IPv6?", "id": "::1." },
  { "en": "Apa itu 'scope' alamat IPv6?", "id": "Menentukan jangkauan alamat (link-local, global)." },
  { "en": "Apa itu 'link-local address'?", "id": "Alamat yang hanya valid di link lokal." },
  { "en": "Apa itu 'global unicast address'?", "id": "Alamat IPv6 publik yang bisa di-routing." },
  { "en": "Apa itu 'Alamat MAC'?", "id": "Alamat fisik unik dari kartu jaringan." },
  { "en": "Berapa panjang alamat MAC?", "id": "48 bit (6 oktet)." },
  { "en": "Apa itu OUI?", "id": "Organizationally Unique Identifier." },
  { "en": "Apa fungsi OUI pada alamat MAC?", "id": "Mengidentifikasi produsen perangkat keras." },
  { "en": "Apa itu 'ARP'?", "id": "Address Resolution Protocol." },
  { "en": "Fungsi ARP?", "id": "Memetakan Alamat IP ke Alamat MAC." },
  { "en": "Apa itu 'NDP (Neighbor Discovery Protocol)'?", "id": "Protokol di IPv6, menggantikan ARP." },
  { "en": "Apa itu 'SLAAC'?", "id": "Stateless Address Autoconfiguration." },
  { "en": "Fungsi SLAAC?", "id": "Perangkat membuat alamat IPv6 sendiri." },
  { "en": "Apa itu 'DHCPv6'?", "id": "Versi DHCP untuk jaringan IPv6." },
  { "en": "Apa itu 'ICMPv6'?", "id": "Versi ICMP untuk jaringan IPv6." },
  { "en": "Apa itu 'flow control'?", "id": "Mengatur laju transfer data." },
  { "en": "Apa itu 'congestion'?", "id": "Kepadatan lalu lintas yang berlebihan di jaringan." },
  { "en": "Apa itu 'Quality of Service' (QoS)?", "id": "Memprioritaskan jenis lalu lintas tertentu." },
  { "en": "Lalu lintas apa yang butuh QoS tinggi?", "id": "Suara (VoIP) dan video streaming." },
  { "en": "Apa itu 'VLAN'?", "id": "Virtual Local Area Network." },
  { "en": "Apa manfaat utama VLAN?", "id": "Segmentasi jaringan, keamanan, mengurangi broadcast." },
  { "en": "Apa itu 'trunking' (802.1Q)?", "id": "Membawa lalu lintas beberapa VLAN antar switch." },
  { "en": "Apa itu 'Spanning Tree Protocol' (STP)?", "id": "Mencegah 'loop' di antara switch." },
  { "en": "Apa itu 'loop' jaringan?", "id": "Koneksi redundan yang menyebabkan broadcast storm." },
  { "en": "Apa itu 'portfast'?", "id": "Fitur STP untuk mempercepat koneksi port akses." },
  { "en": "Apa itu 'etherchannel'?", "id": "Menggabungkan beberapa link fisik menjadi satu." },
  { "en": "Apa itu 'routing protocol'?", "id": "Aturan yang digunakan router untuk bertukar rute." },
  { "en": "Apa itu 'RIP'?", "id": "Routing Information Protocol." },
  { "en": "Apa itu 'OSPF'?", "id": "Open Shortest Path First." },
  { "en": "Apa itu 'EIGRP'?", "id": "Enhanced Interior Gateway Routing Protocol." },
  { "en": "Apa itu 'BGP'?", "id": "Border Gateway Protocol." },
  { "en": "Protokol apa yang digunakan di internet?", "id": "BGP (Border Gateway Protocol)." },
  { "en": "Apa itu 'autonomous system' (AS)?", "id": "Sekelompok jaringan di bawah satu administrasi." },
  { "en": "Apa itu 'metric'?", "id": "Nilai untuk menentukan rute terbaik." },
  { "en": "Apa itu 'administrative distance'?", "id": "Tingkat kepercayaan terhadap sumber rute." },
  { "en": "Apa itu 'hotspot'?", "id": "Titik akses Wi-Fi publik." },
  { "en": "Apa itu 'tethering'?", "id": "Berbagi koneksi internet dari ponsel." },
  { "en": "Apa itu 'repeater' Wi-Fi?", "id": "Memperluas jangkauan sinyal Wi-Fi." },
  { "en": "Apa itu 'mesh network'?", "id": "Jaringan di mana semua node saling terhubung." },
  { "en": "Apa itu 'firmware'?", "id": "Perangkat lunak yang tertanam di perangkat keras." },
  { "en": "Apa itu 'patch panel'?", "id": "Panel untuk mengorganisir koneksi kabel." },
  { "en": "Apa itu 'kabel straight-through'?", "id": "Untuk menghubungkan perangkat yang berbeda." },
  { "en": "Apa itu 'kabel crossover'?", "id": "Untuk menghubungkan perangkat yang sama." },
  { "en": "Apa itu 'Power over Ethernet' (PoE)?", "id": "Menyalurkan daya melalui kabel Ethernet." },
  { "en": "Perangkat apa yang menggunakan PoE?", "id": "IP Camera, Access Point, telepon VoIP." },
  { "en": "Apa itu 'collision'?", "id": "Tabrakan data saat dua perangkat mengirim bersamaan." },
  { "en": "Apa itu 'broadcast storm'?", "id": "Lalu lintas broadcast berlebihan yang melumpuhkan jaringan." },
  { "en": "Apa itu 'packet sniffer'?", "id": "Alat untuk menangkap dan menganalisis paket." },
  { "en": "Contoh 'packet sniffer'?", "id": "Wireshark." },
  { "en": "Apa itu 'port mirroring'?", "id": "Menyalin lalu lintas satu port ke port lain." },
  { "en": "Apa itu 'checksum'?", "id": "Nilai untuk memverifikasi integritas data." },
  { "en": "Apa itu 'segmentation'?", "id": "Proses memecah data besar di Transport Layer." },
  { "en": "Apa itu 'reassembly'?", "id": "Proses menggabungkan kembali segmen di penerima." },
  { "en": "Apa itu 'pdu (protocol data unit)'?", "id": "Bentuk data pada lapisan tertentu." },
  { "en": "Apa itu 'URL'?", "id": "Uniform Resource Locator." },
  { "en": "Apa itu 'URI'?", "id": "Uniform Resource Identifier." },
  { "en": "Apa itu 'HTML'?", "id": "Hypertext Markup Language." },
  { "en": "Apa itu 'CSS'?", "id": "Cascading Style Sheets." },
  { "en": "Apa itu 'JavaScript'?", "id": "Bahasa pemrograman untuk halaman web interaktif." },
  { "en": "Apa dua sublapisan dari Data Link Layer?", "id": "LLC (Logical Link Control) dan MAC." },
  { "en": "Apa fungsi sublapisan LLC?", "id": "Berkomunikasi dengan lapisan Network di atasnya." },
  { "en": "Apa fungsi sublapisan MAC?", "id": "Mengontrol akses ke media fisik." },
  { "en": "Apa itu 'preamble' dalam frame Ethernet?", "id": "Pola bit untuk sinkronisasi clock." },
  { "en": "Apa itu 'SFD (Start Frame Delimiter)'?", "id": "Menandai akhir dari preamble." },
  { "en": "Standar kabel TIA/EIA 568A dimulai warna apa?", "id": "Dimulai dengan warna hijau-putih." },
  { "en": "Standar kabel TIA/EIA 568B dimulai warna apa?", "id": "Dimulai dengan warna oranye-putih." },
  { "en": "Standar mana yang lebih umum di Indonesia?", "id": "Standar TIA/EIA 568B." },
  { "en": "Apa itu 'split horizon'?", "id": "Mekanisme pencegahan routing loop." },
  { "en": "Bagaimana cara kerja 'split horizon'?", "id": "Jangan kirim rute kembali ke sumbernya." },
  { "en": "Apa itu 'route poisoning'?", "id": "Mengiklankan rute yang rusak dengan metrik tak hingga." },
  { "en": "Apa itu 'poison reverse'?", "id": "Mengirim kembali rute rusak ke sumbernya." },
  { "en": "Protokol routing apa yang menggunakan 'hop count'?", "id": "RIP (Routing Information Protocol)." },
  { "en": "Berapa 'hop count' maksimum pada RIP?", "id": "Maksimum adalah 15 hop." },
  { "en": "Metrik apa yang digunakan OSPF?", "id": "Cost, berdasarkan bandwidth." },
  { "en": "Apa itu 'area' dalam OSPF?", "id": "Sekelompok router untuk membatasi penyebaran rute." },
  { "en": "Apa itu 'backbone area' (Area 0)?", "id": "Area pusat dalam jaringan OSPF." },
  { "en": "Apa itu 'DR (Designated Router)'?", "id": "Router utama dalam segmen multi-akses OSPF." },
  { "en": "Apa itu 'BDR (Backup Designated Router)'?", "id": "Cadangan jika DR gagal." },
  { "en": "Protokol apa yang proprietary Cisco?", "id": "EIGRP (Enhanced Interior Gateway Routing Protocol)." },
  { "en": "Metrik apa yang digunakan EIGRP?", "id": "Metrik komposit (bandwidth, delay, dll)." },
  { "en": "Apa itu 'autonomous system number' (ASN)?", "id": "Nomor unik untuk sebuah sistem otonom." },
  { "en": "Apa itu 'port security' pada switch?", "id": "Membatasi akses port ke alamat MAC tertentu." },
  { "en": "Apa yang terjadi jika terjadi pelanggaran 'port security'?", "id": "Port bisa dimatikan (shutdown)." },
  { "en": "Apa itu 'sticky MAC address'?", "id": "Alamat MAC pertama yang dipelajari akan disimpan." },
  { "en": "Apa itu 'DHCP snooping'?", "id": "Fitur keamanan untuk memblokir server DHCP palsu." },
  { "en": "Apa itu 'dynamic ARP inspection' (DAI)?", "id": "Memvalidasi paket ARP dalam jaringan." },
  { "en": "Apa itu 'network virtualization'?", "id": "Membuat representasi virtual dari sumber daya fisik." },
  { "en": "Apa itu 'hypervisor'?", "id": "Perangkat lunak untuk membuat dan menjalankan mesin virtual." },
  { "en": "Apa itu 'virtual machine' (VM)?", "id": "Emulasi perangkat keras dari sebuah sistem komputer." },
  { "en": "Apa itu 'virtual switch' (vSwitch)?", "id": "Switch perangkat lunak untuk komunikasi antar VM." },
  { "en": "Apa itu 'SDN (Software-Defined Networking)'?", "id": "Memisahkan control plane dan data plane." },
  { "en": "Apa itu 'control plane'?", "id": "Bagian yang membuat keputusan (routing, policy)." },
  { "en": "Apa itu 'data plane'?", "id": "Bagian yang meneruskan lalu lintas data." },
  { "en": "Apa itu 'SDN controller'?", "id": "Otak terpusat dari jaringan SDN." },
  { "en": "Apa itu 'OpenFlow'?", "id": "Protokol komunikasi antara controller dan switch SDN." },
  { "en": "Apa itu 'network automation'?", "id": "Mengotomatiskan konfigurasi dan manajemen jaringan." },
  { "en": "Apa itu 'scripting' dalam jaringan?", "id": "Menggunakan skrip (misal Python) untuk otomasi." },
  { "en": "Apa itu 'API (Application Programming Interface)'?", "id": "Antarmuka untuk interaksi antar perangkat lunak." },
  { "en": "Apa itu 'REST API'?", "id": "Gaya arsitektur API yang umum digunakan." },
  { "en": "Apa itu 'JSON (JavaScript Object Notation)'?", "id": "Format pertukaran data yang ringan." },
  { "en": "Apa itu 'YAML'?", "id": "YAML Ain't Markup Language." },
  { "en": "Apa itu 'Ansible'?", "id": "Alat otomasi berbasis agen." },
  { "en": "Standar Wi-Fi 802.11b beroperasi di frekuensi mana?", "id": "Hanya di 2.4 GHz." },
  { "en": "Standar Wi-Fi 802.11a beroperasi di frekuensi mana?", "id": "Hanya di 5 GHz." },
  { "en": "Standar Wi-Fi 802.11n bisa beroperasi di frekuensi mana?", "id": "Bisa di 2.4 GHz dan 5 GHz." },
  { "en": "Apa itu 'channel bonding'?", "id": "Menggabungkan dua kanal untuk bandwidth lebih besar." },
  { "en": "Apa itu 'MIMO'?", "id": "Multiple-Input Multiple-Output." },
  { "en": "Fungsi MIMO pada Wi-Fi?", "id": "Menggunakan banyak antena untuk kecepatan lebih tinggi." },
  { "en": "Apa itu 'beamforming'?", "id": "Memfokuskan sinyal Wi-Fi ke arah perangkat." },
  { "en": "Apa itu 'WEP'?", "id": "Wired Equivalent Privacy (enkripsi usang)." },
  { "en": "Mengapa WEP tidak aman?", "id": "Punya banyak kelemahan kriptografi yang fatal." },
  { "en": "Apa itu 'WPA'?", "id": "Wi-Fi Protected Access." },
  { "en": "Apa itu 'TKIP'?", "id": "Temporal Key Integrity Protocol (digunakan WPA)." },
  { "en": "Apa itu 'WPA2'?", "id": "Standar keamanan Wi-Fi yang sangat umum." },
  { "en": "Apa itu 'AES'?", "id": "Advanced Encryption Standard (digunakan WPA2)." },
  { "en": "Apa itu 'WPA3'?", "id": "Standar keamanan Wi-Fi terbaru." },
  { "en": "Apa itu 'WPS (Wi-Fi Protected Setup)'?", "id": "Metode mudah menghubungkan perangkat, tapi rentan." },
  { "en": "Apa itu 'evil twin' attack?", "id": "Access point palsu untuk mencuri data." },
  { "en": "Apa itu 'deauthentication attack'?", "id": "Memutuskan paksa koneksi klien dari AP." },
  { "en": "Apa itu 'IGMP'?", "id": "Internet Group Management Protocol." },
  { "en": "Fungsi IGMP?", "id": "Mengelola keanggotaan dalam grup multicast." },
  { "en": "Apa itu 'NTP'?", "id": "Network Time Protocol." },
  { "en": "Fungsi NTP?", "id": "Menyinkronkan waktu jam di seluruh jaringan." },
  { "en": "Apa itu 'SNMP'?", "id": "Simple Network Management Protocol." },
  { "en": "Fungsi SNMP?", "id": "Untuk memonitor dan mengelola perangkat jaringan." },
  { "en": "Apa itu 'MIB (Management Information Base)'?", "id": "Database informasi yang dikelola oleh SNMP." },
  { "en": "Apa itu 'syslog'?", "id": "Protokol standar untuk pengiriman pesan log." },
  { "en": "Apa itu 'NetFlow'?", "id": "Protokol Cisco untuk mengumpulkan statistik lalu lintas IP." },
  { "en": "Apa itu 'hypertext'?", "id": "Teks yang berisi tautan ke teks lain." },
  { "en": "Apa itu 'web socket'?", "id": "Koneksi komunikasi dua arah yang persisten." },
  { "en": "Apa itu 'LDAP'?", "id": "Lightweight Directory Access Protocol." },
  { "en": "Fungsi LDAP?", "id": "Untuk mengakses dan mengelola layanan direktori." },
  { "en": "Apa itu 'Kerberos'?", "id": "Protokol otentikasi jaringan yang kuat." },
  { "en": "Apa itu 'RADIUS'?", "id": "Remote Authentication Dial-In User Service." },
  { "en": "Apa itu 'TACACS+'?", "id": "Protokol otentikasi, otorisasi, dan akuntansi." },
  { "en": "Apa itu 'single-mode fiber'?", "id": "Inti sangat kecil, untuk jarak jauh." },
  { "en": "Apa itu 'multi-mode fiber'?", "id": "Inti lebih besar, untuk jarak pendek." },
  { "en": "Konektor serat optik yang umum?", "id": "SC, ST, dan LC." },
  { "en": "Apa itu 'optical time-domain reflectometer' (OTDR)?", "id": "Alat untuk menguji kabel serat optik." },
  { "en": "Apa itu 'attenuation'?", "id": "Pelemahan sinyal saat melewati media." },
  { "en": "Apa itu 'dispersion'?", "id": "Penyebaran pulsa sinyal saat melewati media." },
  { "en": "Apa itu 'latency'?", "id": "Waktu tunda total dari sumber ke tujuan." },
  { "en": "Apa itu 'throughput'?", "id": "Laju data aktual yang berhasil ditransfer." },
  { "en": "Apa itu 'goodput'?", "id": "Throughput pada lapisan aplikasi (tanpa overhead)." },
  { "en": "Apa itu 'jumbo frame'?", "id": "Frame Ethernet yang lebih besar dari 1500 byte." },
  { "en": "Apa itu 'bit'?", "id": "Unit data terkecil, bernilai 0 atau 1." },
  { "en": "Apa itu 'byte'?", "id": "Sekelompok 8 bit." },
  { "en": "Apa itu 'octet'?", "id": "Istilah lain untuk 8 bit." },
  { "en": "Berapa nilai 'administrative distance' untuk rute statis?", "id": "Nilainya 1." },
  { "en": "Berapa nilai 'administrative distance' untuk OSPF?", "id": "Nilainya 110." },
  { "en": "Berapa nilai 'administrative distance' untuk EIGRP internal?", "id": "Nilainya 90." },
  { "en": "Apa itu 'floating static route'?", "id": "Rute statis dengan AD tinggi sebagai cadangan." },
  { "en": "Apa itu 'network baseline'?", "id": "Ukuran performa normal dari sebuah jaringan." },
  { "en": "Mengapa 'network baseline' penting?", "id": "Untuk mendeteksi anomali atau masalah performa." },
  { "en": "Apa itu 'syslog'?", "id": "Protokol standar untuk mengirim pesan log." },
  { "en": "Apa itu 'syslog server'?", "id": "Server terpusat untuk mengumpulkan pesan log." },
  { "en": "Apa itu 'NTP'?", "id": "Network Time Protocol." },
  { "en": "Fungsi NTP?", "id": "Menyinkronkan waktu jam di seluruh jaringan." },
  { "en": "Mengapa sinkronisasi waktu penting?", "id": "Untuk analisis log dan keamanan yang akurat." },
  { "en": "Apa itu 'SNMP'?", "id": "Simple Network Management Protocol." },
  { "en": "Fungsi SNMP?", "id": "Untuk memonitor dan mengelola perangkat jaringan." },
  { "en": "Apa itu 'SNMP manager' (NMS)?", "id": "Sistem yang memonitor agen SNMP." },
  { "en": "Apa itu 'SNMP agent'?", "id": "Perangkat lunak di perangkat yang dimonitor." },
  { "en": "Apa itu 'MIB (Management Information Base)'?", "id": "Database informasi yang dikelola agen SNMP." },
  { "en": "Apa itu 'SNMP trap'?", "id": "Notifikasi yang dikirim oleh agen ke manajer." },
  { "en": "Apa itu 'NetFlow'?", "id": "Protokol Cisco untuk analisis lalu lintas." },
  { "en": "Apa itu 'port mirroring'?", "id": "Menyalin lalu lintas satu port ke port lain." },
  { "en": "Tujuan 'port mirroring'?", "id": "Untuk analisis dan monitoring lalu lintas." },
  { "en": "Apa itu 'SPAN'?", "id": "Switched Port Analyzer (istilah Cisco)." },
  { "en": "Apa itu 'cloud computing'?", "id": "Layanan komputasi yang diakses via internet." },
  { "en": "Apa itu 'SaaS'?", "id": "Software as a Service." },
  { "en": "Contoh SaaS?", "id": "Google Workspace, Microsoft 365." },
  { "en": "Apa itu 'PaaS'?", "id": "Platform as a Service." },
  { "en": "Contoh PaaS?", "id": "Heroku, Google App Engine." },
  { "en": "Apa itu 'IaaS'?", "id": "Infrastructure as a Service." },
  { "en": "Contoh IaaS?", "id": "Amazon Web Services (AWS), Microsoft Azure." },
  { "en": "Apa itu 'public cloud'?", "id": "Layanan cloud yang tersedia untuk umum." },
  { "en": "Apa itu 'private cloud'?", "id": "Infrastruktur cloud untuk satu organisasi." },
  { "en": "Apa itu 'hybrid cloud'?", "id": "Gabungan dari public dan private cloud." },
  { "en": "Apa itu 'virtualisasi'?", "id": "Membuat versi virtual dari sesuatu." },
  { "en": "Apa itu 'hypervisor'?", "id": "Perangkat lunak untuk menjalankan mesin virtual." },
  { "en": "Apa itu 'containerization'?", "id": "Metode virtualisasi di tingkat sistem operasi." },
  { "en": "Contoh platform container?", "id": "Docker dan Kubernetes." },
  { "en": "Apa itu 'SDN (Software-Defined Networking)'?", "id": "Memisahkan control plane dari data plane." },
  { "en": "Apa itu 'control plane'?", "id": "Bagian jaringan yang membuat keputusan." },
  { "en": "Apa itu 'data plane'?", "id": "Bagian jaringan yang meneruskan data." },
  { "en": "Apa itu 'network automation'?", "id": "Menggunakan skrip untuk mengelola jaringan." },
  { "en": "Apa itu 'API (Application Programming Interface)'?", "id": "Antarmuka untuk interaksi antar perangkat lunak." },
  { "en": "Apa itu 'RESTful API'?", "id": "Gaya arsitektur API yang umum." },
  { "en": "Apa itu 'JSON'?", "id": "JavaScript Object Notation." },
  { "en": "Tujuan JSON?", "id": "Format pertukaran data yang ringan." },
  { "en": "Apa itu 'XML'?", "id": "Extensible Markup Language." },
  { "en": "Apa itu 'YAML'?", "id": "Format serialisasi data yang mudah dibaca." },
  { "en": "Apa itu 'ping of death'?", "id": "Serangan DoS menggunakan paket ping ilegal." },
  { "en": "Apa itu 'smurf attack'?", "id": "Serangan DDoS menggunakan broadcast address." },
  { "en": "Apa itu 'brute force attack'?", "id": "Mencoba semua kemungkinan kata sandi." },
  { "en": "Apa itu 'dictionary attack'?", "id": "Brute force menggunakan daftar kata umum." },
  { "en": "Apa itu 'SQL injection'?", "id": "Serangan yang menyisipkan kode SQL berbahaya." },
  { "en": "Apa itu 'cross-site scripting' (XSS)?", "id": "Menyisipkan skrip berbahaya ke situs web." },
  { "en": "Apa itu 'zero-day exploit'?", "id": "Serangan yang memanfaatkan kerentanan baru." },
  { "en": "Apa itu 'honeypot'?", "id": "Sistem umpan untuk menjebak penyerang." },
  { "en": "Apa itu 'penetration testing'?", "id": "Uji coba peretasan etis untuk menemukan celah." },
  { "en": "Apa itu 'ethical hacking'?", "id": "Peretasan yang sah untuk tujuan keamanan." },
  { "en": "Apa itu 'white hat hacker'?", "id": "Peretas etis." },
  { "en": "Apa itu 'black hat hacker'?", "id": "Peretas dengan niat jahat." },
  { "en": "Apa itu 'grey hat hacker'?", "id": "Peretas di antara white dan black hat." },
  { "en": "Apa itu 'dua-faktor otentikasi' (2FA)?", "id": "Membutuhkan dua metode verifikasi." },
  { "en": "Apa itu 'biometrik'?", "id": "Otentikasi menggunakan ciri fisik." },
  { "en": "Apa itu 'checksum'?", "id": "Nilai untuk memeriksa integritas data." },
  { "en": "Apa itu 'hashing'?", "id": "Mengubah data menjadi string berukuran tetap." },
  { "en": "Sifat utama fungsi hash?", "id": "Satu arah, tidak bisa dibalikkan." },
  { "en": "Contoh algoritma hash?", "id": "MD5, SHA-256." },
  { "en": "Apakah MD5 masih aman?", "id": "Tidak, MD5 sudah dianggap tidak aman." },
  { "en": "Apa itu 'digital signature'?", "id": "Tanda tangan elektronik menggunakan kriptografi." },
  { "en": "Apa itu 'public key infrastructure' (PKI)?", "id": "Sistem untuk mengelola sertifikat digital." },
  { "en": "Apa itu 'certificate authority' (CA)?", "id": "Entitas yang menerbitkan sertifikat digital." },
  { "en": "Apa itu 'root certificate'?", "id": "Sertifikat paling atas yang dipercaya." },
  { "en": "Apa itu 'certificate chain'?", "id": "Rantai kepercayaan dari sertifikat root." },
  { "en": "Apa itu 'domain hijacking'?", "id": "Mengambil alih nama domain tanpa izin." },
  { "en": "Apa itu 'typosquatting'?", "id": "Mendaftarkan domain salah ketik dari situs populer." },
  { "en": "Apa itu 'Cat 5e'?", "id": "Kategori kabel UTP untuk Gigabit Ethernet." },
  { "en": "Apa itu 'Cat 6'?", "id": "Kabel UTP dengan performa lebih baik." },
  { "en": "Apa itu 'plenum-rated cable'?", "id": "Kabel tahan api untuk ruang plenum." },
  { "en": "Apa itu 'wiring closet'?", "id": "Ruangan untuk menempatkan perangkat jaringan." },
  { "en": "Apa itu 'rack unit' (U)?", "id": "Satuan tinggi untuk perangkat di rak." },
  { "en": "Apa itu 'patch panel'?", "id": "Panel untuk mengorganisir dan menghubungkan kabel." },
  { "en": "Apa itu 'keystone jack'?", "id": "Konektor modular untuk wall plate/patch panel." },
  { "en": "Apa itu 'RJ11'?", "id": "Konektor yang biasa digunakan untuk telepon." },
  { "en": "Apa itu 'RJ45'?", "id": "Konektor yang biasa digunakan untuk Ethernet." },
  { "en": "Apa itu 'SFP (Small Form-factor Pluggable)'?", "id": "Modul transceiver optik atau tembaga." },
  { "en": "Apa itu 'GBIC'?", "id": "Gigabit Interface Converter (versi SFP lama)." },
  { "en": "Apa itu 'link aggregation'?", "id": "Menggabungkan beberapa link menjadi satu (EtherChannel)." },
  { "en": "Protokol untuk 'link aggregation'?", "id": "LACP (Link Aggregation Control Protocol)." },
  { "en": "Apa itu 'First Hop Redundancy Protocol' (FHRP)?", "id": "Menyediakan gateway default yang redundan." },
  { "en": "Contoh FHRP?", "id": "HSRP, VRRP, GLBP." },
  { "en": "Apa itu 'HSRP'?", "id": "Hot Standby Router Protocol (Cisco)." },
  { "en": "Apa itu 'VRRP'?", "id": "Virtual Router Redundancy Protocol (standar)." },
  { "en": "Apa itu 'GLBP'?", "id": "Gateway Load Balancing Protocol (Cisco)." },
  { "en": "Apa itu 'routing on a stick'?", "id": "Metode routing antar-VLAN menggunakan satu link." },
  { "en": "Apa itu 'switch multilayer'?", "id": "Switch yang bisa melakukan fungsi routing." },
  { "en": "Apa itu 'ASIC'?", "id": "Application-Specific Integrated Circuit." },
  { "en": "Mengapa switch lebih cepat dari router?", "id": "Karena menggunakan ASIC untuk penerusan frame." },
  { "en": "Apa itu 'process switching'?", "id": "Metode routing paling lambat (oleh CPU)." },
  { "en": "Apa itu 'fast switching'?", "id": "Metode routing lebih cepat menggunakan cache." },
  { "en": "Apa itu 'CEF (Cisco Express Forwarding)'?", "id": "Metode switching lapisan 3 paling cepat." },
  { "en": "Apa itu 'jumbo frame'?", "id": "Frame Ethernet dengan payload lebih dari 1500 byte." },
  { "en": "Apa keuntungan menggunakan 'jumbo frame'?", "id": "Mengurangi overhead, meningkatkan throughput." },
  { "en": "Apa itu 'network tap'?", "id": "Perangkat keras untuk memonitor lalu lintas jaringan." },
  { "en": "Apa itu 'out-of-band management'?", "id": "Mengelola perangkat jaringan melalui koneksi terpisah." },
  { "en": "Apa keuntungan 'out-of-band management'?", "id": "Masih bisa diakses saat jaringan utama mati." },
  { "en": "Apa itu 'console port'?", "id": "Port serial untuk manajemen perangkat." },
  { "en": "Apa itu 'rollover cable'?", "id": "Kabel khusus untuk koneksi console." },
  { "en": "Apa itu 'TFTP'?", "id": "Trivial File Transfer Protocol." },
  { "en": "Kapan TFTP digunakan?", "id": "Untuk transfer konfigurasi dan OS perangkat jaringan." },
  { "en": "Apa itu 'secure FTP' (SFTP)?", "id": "Versi aman dari FTP menggunakan SSH." },
  { "en": "Apa itu 'FTP passive mode'?", "id": "Mode koneksi di mana klien memulai koneksi data." },
  { "en": "Apa itu 'FTP active mode'?", "id": "Mode koneksi di mana server memulai koneksi data." },
  { "en": "Apa itu 'well-known port'?", "id": "Port 0-1023 yang dicadangkan untuk layanan standar." },
  { "en": "Apa itu 'registered port'?", "id": "Port 1024-49151 untuk aplikasi spesifik." },
  { "en": "Apa itu 'dynamic/private port'?", "id": "Port 49152-65535 untuk koneksi sementara." },
  { "en": "Apa itu 'UDP hole punching'?", "id": "Teknik untuk membangun koneksi UDP di belakang NAT." },
  { "en": "Apa itu 'stateful firewall'?", "id": "Firewall yang melacak status koneksi." },
  { "en": "Apa itu 'stateless firewall'?", "id": "Firewall yang hanya memeriksa header paket." },
  { "en": "Apa itu 'next-generation firewall' (NGFW)?", "id": "Firewall dengan fitur inspeksi aplikasi dan IPS." },
  { "en": "Apa itu 'deep packet inspection' (DPI)?", "id": "Memeriksa konten payload dari sebuah paket." },
  { "en": "Apa itu 'sandboxing'?", "id": "Menjalankan file di lingkungan terisolasi." },
  { "en": "Apa itu 'honeypot'?", "id": "Sistem umpan untuk menarik dan menganalisis serangan." },
  { "en": "Apa itu 'DNS poisoning' atau 'spoofing'?", "id": "Meracuni cache DNS dengan informasi palsu." },
  { "en": "Apa itu 'DNSSEC'?", "id": "Domain Name System Security Extensions." },
  { "en": "Fungsi DNSSEC?", "id": "Memastikan respons DNS otentik dan tidak diubah." },
  { "en": "Apa itu 'DANE'?", "id": "DNS-based Authentication of Named Entities." },
  { "en": "Apa itu 'DNS over TLS' (DoT)?", "id": "Mengenkripsi lalu lintas DNS menggunakan TLS." },
  { "en": "Apa itu 'DNS over HTTPS' (DoH)?", "id": "Mengenkripsi DNS melalui koneksi HTTPS." },
  { "en": "Apa itu 'root hints'?", "id": "Daftar alamat IP dari root DNS server." },
  { "en": "Apa itu 'recursive DNS query'?", "id": "Server DNS mencari jawaban atas nama klien." },
  { "en": "Apa itu 'iterative DNS query'?", "id": "Server DNS memberikan rujukan ke server lain." },
  { "en": "Apa itu 'Time to Live' (TTL) pada DNS?", "id": "Berapa lama sebuah record DNS di-cache." },
  { "en": "Apa itu 'A record'?", "id": "Memetakan nama domain ke alamat IPv4." },
  { "en": "Apa itu 'AAAA record' ('quad-A')?", "id": "Memetakan nama domain ke alamat IPv6." },
  { "en": "Apa itu 'MX record'?", "id": "Menentukan mail server untuk sebuah domain." },
  { "en": "Apa itu 'CNAME record'?", "id": "Membuat alias dari satu nama domain ke nama lain." },
  { "en": "Apa itu 'PTR record'?", "id": "Memetakan alamat IP ke nama domain (reverse DNS)." },
  { "en": "Apa itu 'SPF (Sender Policy Framework)'?", "id": "Mekanisme otentikasi email." },
  { "en": "Apa itu 'DKIM (DomainKeys Identified Mail)'?", "id": "Menambahkan tanda tangan digital ke email." },
  { "en": "Apa itu 'DMARC'?", "id": "Membangun di atas SPF dan DKIM." },
  { "en": "Apa itu 'link-state advertisement' (LSA)?", "id": "Paket yang digunakan OSPF untuk bertukar info." },
  { "en": "Apa itu 'shortest path first' (SPF) algorithm?", "id": "Algoritma yang digunakan oleh OSPF." },
  { "en": "Apa itu 'DUAL algorithm'?", "id": "Algoritma yang digunakan oleh EIGRP." },
  { "en": "Apa itu 'feasible successor'?", "id": "Rute cadangan bebas loop dalam EIGRP." },
  { "en": "Apa itu 'route summarization'?", "id": "Menggabungkan beberapa rute menjadi satu (supernetting)." },
  { "en": "Apa itu 'BGP peer' atau 'neighbor'?", "id": "Dua router BGP yang bertukar informasi." },
  { "en": "Apa itu 'iBGP (Internal BGP)'?", "id": "BGP yang berjalan di dalam satu AS." },
  { "en": "Apa itu 'eBGP (External BGP)'?", "id": "BGP yang berjalan di antara AS yang berbeda." },
  { "en": "Apa itu 'BGP path attributes'?", "id": "Parameter yang digunakan BGP untuk memilih rute." },
  { "en": "Apa itu 'AS-Path'?", "id": "Daftar AS yang dilewati sebuah rute." },
  { "en": "Apa itu 'route reflector'?", "id": "Menyederhanakan konfigurasi iBGP dalam skala besar." },
  { "en": "Apa itu 'multihoming'?", "id": "Terhubung ke lebih dari satu ISP." },
  { "en": "Apa itu 'peering'?", "id": "Interkoneksi langsung antara dua jaringan." },
  { "en": "Apa itu 'Internet Exchange Point' (IXP)?", "id": "Infrastruktur fisik untuk peering." },
  { "en": "Apa itu 'content delivery network' (CDN)?", "id": "Mendistribusikan konten lebih dekat ke pengguna." },
  { "en": "Bagaimana cara kerja CDN?", "id": "Menyimpan salinan (cache) konten di banyak lokasi." },
  { "en": "Apa itu 'latency'?", "id": "Waktu tunda dalam transmisi data." },
  { "en": "Apa itu 'round-trip time' (RTT)?", "id": "Waktu total untuk sinyal bolak-balik." },
  { "en": "Apa itu 'jitter'?", "id": "Variasi dalam penundaan paket." },
  { "en": "Apa itu 'packet loss'?", "id": "Paket yang hilang selama transmisi." },
  { "en": "Apa itu 'bit stuffing'?", "id": "Menyisipkan bit untuk mencegah pola tertentu." },
  { "en": "Apa itu 'flow control'?", "id": "Mengatur laju data antara pengirim dan penerima." },
  { "en": "Apa itu 'congestion control'?", "id": "Mekanisme untuk mengatasi kepadatan di jaringan." },
  { "en": "Apa itu 'TCP window size'?", "id": "Jumlah data yang bisa dikirim sebelum menunggu ACK." },
  { "en": "Apa itu 'slow start'?", "id": "Algoritma TCP untuk memulai transmisi." },
  { "en": "Apa itu 'TCP Reno'?", "id": "Salah satu versi algoritma congestion control TCP." },
  { "en": "Apa itu 'TCP Cubic'?", "id": "Algoritma congestion control default di Linux." },
  { "en": "Apa itu 'BBR'?", "id": "Algoritma congestion control yang dikembangkan Google." },
  { "en": "Apa itu 'head-of-line blocking'?", "id": "Paket yang tertunda memblokir paket di belakangnya." },
  { "en": "Apa itu 'QUIC'?", "id": "Protokol transport baru di atas UDP." },
  { "en": "Apa itu 'HTTP/3'?", "id": "Versi HTTP terbaru yang menggunakan QUIC." },
  { "en": "Apa itu 'idempotent method' di HTTP?", "id": "Metode yang hasilnya sama jika diulang." },
  { "en": "Apa itu 'safe method' di HTTP?", "id": "Metode yang tidak mengubah state server." },
  { "en": "Contoh 'safe method'?", "id": "GET, HEAD, OPTIONS." },
  { "en": "Apa itu 'status code' HTTP 200?", "id": "OK, permintaan berhasil." },
  { "en": "Apa itu 'status code' HTTP 404?", "id": "Not Found, sumber daya tidak ditemukan." },
  { "en": "Apa itu 'status code' HTTP 500?", "id": "Internal Server Error." },
  { "en": "Apa itu 'HTTP cookie'?", "id": "Data kecil yang disimpan oleh server di browser." },
  { "en": "Apa itu 'web cache'?", "id": "Menyimpan salinan sumber daya web." },
  { "en": "Apa itu 'conditional GET'?", "id": "Permintaan GET yang hanya mengambil jika ada perubahan." },
  { "en": "Apa itu 'ETag'?", "id": "Header untuk validasi cache." },
  { "en": "Apa itu 'CORS'?", "id": "Cross-Origin Resource Sharing." },
  { "en": "Apa itu 'JSONP'?", "id": "Teknik lama untuk permintaan data lintas domain." },
  { "en": "Apa itu 'long polling'?", "id": "Teknik untuk mendapatkan pembaruan dari server." },
  { "en": "Apa itu 'server-sent events' (SSE)?", "id": "Server mengirim pembaruan ke klien secara otomatis." },
  { "en": "Apa itu 'WebSocket'?", "id": "Protokol komunikasi dua arah full-duplex." },
  { "en": "Apa itu 'physical topology'?", "id": "Tata letak fisik kabel dan perangkat." },
  { "en": "Apa itu 'logical topology'?", "id": "Bagaimana data mengalir dalam jaringan." },
  { "en": "Topologi Star punya 'logical topology' apa?", "id": "Logical bus (jika menggunakan hub)." },
  { "en": "Apa itu 'token' pada Token Ring?", "id": "Frame khusus yang memberikan hak transmisi." },
  { "en": "Apa itu 'beaconing'?", "id": "Proses untuk melaporkan kesalahan serius." },
  { "en": "Apa itu 'promiscuous mode'?", "id": "Mode di mana NIC menerima semua frame." },
  { "en": "Apa itu 'autonegotiation'?", "id": "Proses otomatis menentukan kecepatan dan dupleks." },
  { "en": "Apa itu 'half-duplex'?", "id": "Komunikasi dua arah, tapi bergantian." },
  { "en": "Apa itu 'full-duplex'?", "id": "Komunikasi dua arah, bisa bersamaan." },
  { "en": "Apa itu 'simplex'?", "id": "Komunikasi hanya satu arah." },
  { "en": "Apa itu 'multihoming'?", "id": "Terhubung ke internet melalui lebih dari satu ISP." },
  { "en": "Apa keuntungan 'multihoming'?", "id": "Meningkatkan keandalan (redundansi) koneksi internet." },
  { "en": "Apa itu 'peering'?", "id": "Interkoneksi langsung antara dua jaringan (AS)." },
  { "en": "Apa itu 'Internet Exchange Point' (IXP)?", "id": "Tempat di mana banyak jaringan saling 'peering'." },
  { "en": "Apa itu 'content delivery network' (CDN)?", "id": "Jaringan server untuk mendistribusikan konten." },
  { "en": "Bagaimana cara kerja CDN?", "id": "Menyimpan cache konten lebih dekat ke pengguna." },
  { "en": "Apa itu 'anycast'?", "id": "Menggunakan satu IP untuk banyak server." },
  { "en": "Bagaimana anycast memilih server?", "id": "Routing memilih server terdekat secara geografis." },
  { "en": "Apa itu 'geolocation'?", "id": "Menentukan lokasi geografis dari alamat IP." },
  { "en": "Apa itu 'RFC (Request for Comments)'?", "id": "Dokumen yang mendefinisikan standar internet." },
  { "en": "Siapa yang menerbitkan RFC?", "id": "Internet Engineering Task Force (IETF)." },
  { "en": "Apa itu 'IANA'?", "id": "Internet Assigned Numbers Authority." },
  { "en": "Apa tugas IANA?", "id": "Mengelola alokasi alamat IP global." },
  { "en": "Apa itu 'RIR (Regional Internet Registry)'?", "id": "Organisasi yang mengelola IP di suatu wilayah." },
  { "en": "Apa RIR untuk wilayah Asia Pasifik?", "id": "APNIC (Asia-Pacific Network Information Centre)." },
  { "en": "Apa itu 'WHOIS'?", "id": "Protokol untuk mencari info pendaftaran domain/IP." },
  { "en": "Apa itu 'dark fiber'?", "id": "Kabel serat optik yang belum digunakan." },
  { "en": "Apa itu 'last mile'?", "id": "Segmen terakhir jaringan ke pelanggan." },
  { "en": "Apa itu 'backbone' internet?", "id": "Infrastruktur inti berkecepatan sangat tinggi." },
  { "en": "Apa itu 'network convergence'?", "id": "Satu jaringan untuk data, suara, dan video." },
  { "en": "Apa itu 'unified communications' (UC)?", "id": "Integrasi berbagai alat komunikasi." },
  { "en": "Apa itu 'IP telephony'?", "id": "Teknologi untuk layanan telepon melalui IP." },
  { "en": "Apa itu 'SIP (Session Initiation Protocol)'?", "id": "Protokol untuk memulai sesi multimedia (VoIP)." },
  { "en": "Apa itu 'codec'?", "id": "Coder-Decoder, untuk kompresi data." },
  { "en": "Contoh codec audio?", "id": "MP3, AAC, G.711." },
  { "en": "Contoh codec video?", "id": "H.264, H.265 (HEVC), VP9." },
  { "en": "Apa itu 'mean opinion score' (MOS)?", "id": "Ukuran kualitas subjektif dari suara/video." },
  { "en": "Apa itu 'class of service' (CoS)?", "id": "Metode QoS di Lapisan 2 (802.1p)." },
  { "en": "Apa itu 'differentiated services' (DiffServ)?", "id": "Metode QoS di Lapisan 3 (DSCP)." },
  { "en": "Apa itu 'traffic shaping'?", "id": "Mengatur laju lalu lintas data keluar." },
  { "en": "Apa itu 'traffic policing'?", "id": "Menegakkan batas laju, membuang paket berlebih." },
  { "en": "Apa itu 'queuing'?", "id": "Manajemen antrean paket saat terjadi kepadatan." },
  { "en": "Contoh algoritma 'queuing'?", "id": "FIFO, WFQ, CQ." },
  { "en": "Apa itu 'FIFO (First-In, First-Out)'?", "id": "Antrean sederhana, siapa datang pertama dilayani." },
  { "en": "Apa itu 'network forensics'?", "id": "Investigasi kejahatan siber menggunakan data jaringan." },
  { "en": "Apa itu 'chain of custody'?", "id": "Dokumentasi penanganan barang bukti digital." },
  { "en": "Apa itu 'hashing'?", "id": "Membuat sidik jari digital unik dari data." },
  { "en": "Contoh algoritma hashing?", "id": "SHA-256, SHA-3." },
  { "en": "Apa itu 'data loss prevention' (DLP)?", "id": "Sistem untuk mencegah kebocoran data sensitif." },
  { "en": "Apa itu 'least privilege principle'?", "id": "Memberikan hak akses minimum yang diperlukan." },
  { "en": "Apa itu 'defense in depth'?", "id": "Menerapkan keamanan berlapis-lapis." },
  { "en": "Apa itu 'attack surface'?", "id": "Total area di mana penyerang bisa masuk." },
  { "en": "Apa itu 'threat modeling'?", "id": "Proses mengidentifikasi dan menilai ancaman." },
  { "en": "Apa itu 'air gap'?", "id": "Mengisolasi jaringan secara fisik dari jaringan lain." },
  { "en": "Apa itu 'SCADA (Supervisory Control and Data Acquisition)'?", "id": "Sistem kontrol untuk infrastruktur industri." },
  { "en": "Mengapa keamanan jaringan SCADA penting?", "id": "Karena mengontrol infrastruktur kritis." },
  { "en": "Apa itu 'Post Office Protocol' (POP3)?", "id": "Protokol untuk mengunduh email dari server." },
  { "en": "Apa itu 'Internet Message Access Protocol' (IMAP)?", "id": "Protokol untuk mengakses email di server." },
  { "en": "Perbedaan utama POP3 dan IMAP?", "id": "IMAP menyinkronkan email, POP3 mengunduh." },
  { "en": "Apa itu 'MIME (Multipurpose Internet Mail Extensions)'?", "id": "Standar untuk mengirim lampiran di email." },
  { "en": "Apa itu 'bastion host'?", "id": "Server yang diperkuat di luar firewall utama." },
  { "en": "Apa itu 'honeynet'?", "id": "Jaringan umpan yang terdiri dari beberapa honeypot." },
  { "en": "Apa itu 'war dialing'?", "id": "Mencari modem yang terhubung ke jaringan." },
  { "en": "Apa itu 'war driving'?", "id": "Mencari jaringan Wi-Fi saat berkendara." },
  { "en": "Apa itu 'session hijacking'?", "id": "Mengambil alih sesi pengguna yang valid." },
  { "en": "Apa itu 'cookie poisoning'?", "id": "Memodifikasi cookie untuk mendapatkan akses." },
  { "en": "Apa itu 'URL redirection'?", "id": "Mengarahkan pengguna dari satu URL ke URL lain." },
  { "en": "Apa itu 'OSI model'?", "id": "Open Systems Interconnection model." },
  { "en": "Apa itu 'TCP/IP model'?", "id": "Model yang menjadi dasar internet." },
  { "en": "Apa itu 'application layer'?", "id": "Lapisan teratas, antarmuka ke pengguna." },
  { "en": "Apa itu 'transport layer'?", "id": "Menyediakan koneksi end-to-end." },
  { "en": "Apa itu 'internet layer'?", "id": "Menangani pengalamatan dan routing paket." },
  { "en": "Apa itu 'network access layer'?", "id": "Menangani transmisi data melalui media fisik." },
  { "en": "Apa itu 'simplex'?", "id": "Komunikasi hanya satu arah." },
  { "en": "Apa itu 'half-duplex'?", "id": "Komunikasi dua arah, tapi bergantian." },
  { "en": "Apa itu 'full-duplex'?", "id": "Komunikasi dua arah secara bersamaan." },
  { "en": "Apa itu 'IEEE'?", "id": "Institute of Electrical and Electronics Engineers." },
  { "en": "Apa itu 'IETF'?", "id": "Internet Engineering Task Force." },
  { "en": "Apa itu 'W3C'?", "id": "World Wide Web Consortium." },
  { "en": "Apa itu 'ISO'?", "id": "International Organization for Standardization." },
  { "en": "Apa itu 'ANSI'?", "id": "American National Standards Institute." },
  { "en": "Apa itu 'TIA/EIA'?", "id": "Asosiasi industri yang membuat standar kabel." },
  { "en": "Apa itu 'structured cabling'?", "id": "Sistem pengkabelan terorganisir untuk gedung." },
  { "en": "Apa itu 'horizontal cabling'?", "id": "Kabel dari wiring closet ke area kerja." },
  { "en": "Apa itu 'vertical cabling' (backbone)?", "id": "Kabel antar lantai atau antar gedung." },
  { "en": "Apa itu 'work area'?", "id": "Tempat di mana pengguna terhubung ke jaringan." },
  { "en": "Apa itu 'telecommunications room'?", "id": "Ruangan untuk menempatkan perangkat jaringan." },
  { "en": "Apa itu 'ping flooding'?", "id": "Serangan DoS menggunakan banyak paket ping." },
  { "en": "Apa itu 'SYN flooding'?", "id": "Serangan DoS menggunakan banyak permintaan koneksi." },
  { "en": "Apa itu 'UDP flooding'?", "id": "Serangan DoS menggunakan banyak paket UDP." },
  { "en": "Apa itu 'bot'?", "id": "Program otomatis yang menjalankan tugas." },
  { "en": "Apa itu 'CAPTCHA'?", "id": "Tes untuk membedakan manusia dan bot." },
  { "en": "Apa itu 'two-tier architecture'?", "id": "Arsitektur client-server." },
  { "en": "Apa itu 'three-tier architecture'?", "id": "Client, application server, dan database server." },
  { "en": "Apa itu 'n-tier architecture'?", "id": "Arsitektur dengan lebih dari tiga tingkatan." },
  { "en": "Apa itu 'microservices architecture'?", "id": "Membangun aplikasi sebagai kumpulan layanan kecil." },
  { "en": "Apa itu 'monolithic architecture'?", "id": "Membangun aplikasi sebagai satu unit tunggal." },
  { "en": "Apa itu 'serverless computing'?", "id": "Menjalankan kode tanpa mengelola server." },
  { "en": "Apa itu 'edge computing'?", "id": "Memproses data lebih dekat ke sumbernya." },
  { "en": "Apa itu 'fog computing'?", "id": "Lapisan antara edge dan cloud." },
  { "en": "Apa itu 'network latency'?", "id": "Waktu yang dibutuhkan paket untuk bepergian." },
  { "en": "Apa itu 'bandwidth-delay product'?", "id": "Ukuran kapasitas data sebuah link." },
  { "en": "Apa itu 'packet switching'?", "id": "Metode pengiriman data dalam bentuk paket." },
  { "en": "Apa itu 'circuit switching'?", "id": "Metode dengan jalur dedicated selama koneksi." },
  { "en": "Apa itu 'virtual circuit'?", "id": "Jalur logis yang dibuat di jaringan paket." },
  { "en": "Apa itu 'datagram'?", "id": "Unit transfer data dalam jaringan paket." },
  { "en": "Apa itu 'golden sample' dalam pengujian?", "id": "Perangkat referensi yang diketahui berfungsi baik." },
  { "en": "Apa itu 'network baseline'?", "id": "Ukuran performa normal dari sebuah jaringan." },
  { "en": "Apa itu 'root bridge' dalam STP?", "id": "Switch referensi utama dalam topologi STP." },
  { 'en': 'Apa itu "portfast" pada switch?', "id": "Mempercepat transisi port ke status forwarding." },
  { "en": "Apa itu 'BPDU Guard'?", "id": "Melindungi port PortFast dari BPDU yang tidak sah." },
  { "en": "Apa itu 'BPDU Filter'?", "id": "Mencegah port mengirim atau menerima BPDU." },
  { "en": "Apa itu 'root guard'?", "id": "Mencegah switch lain menjadi root bridge." },
  { "en": "Apa itu 'loop guard'?", "id": "Mencegah loop akibat masalah link searah." },
  { "en": "Apa itu 'UDLD (UniDirectional Link Detection)'?", "id": "Mendeteksi link yang hanya berfungsi satu arah." },
  { "en": "Apa itu 'flex links'?", "id": "Alternatif sederhana untuk Spanning Tree Protocol." },
  { "en": "Apa itu 'private VLAN'?", "id": "Mengisolasi port dalam VLAN yang sama." },
  { "en": "Apa itu 'community port'?", "id": "Port PVLAN yang bisa berkomunikasi satu sama lain." },
  { "en": "Apa itu 'isolated port'?", "id": "Port PVLAN yang hanya bisa ke port promiscuous." },
  { "en": "Apa itu 'promiscuous port'?", "id": "Port PVLAN yang bisa berkomunikasi ke semua." },
  { "en": "Apa itu 'VTP (VLAN Trunking Protocol)'?", "id": "Protokol Cisco untuk menyinkronkan database VLAN." },
  { "en": "Apa itu 'VTP pruning'?", "id": "Mencegah lalu lintas VLAN yang tidak perlu." },
  { "en": "Apa itu 'DTP (Dynamic Trunking Protocol)'?", "id": "Protokol Cisco untuk negosiasi trunk otomatis." },
  { "en": "Apa itu 'CDP (Cisco Discovery Protocol)'?", "id": "Menemukan informasi tentang perangkat Cisco tetangga." },
  { "en": "Apa itu 'LLDP (Link Layer Discovery Protocol)'?", "id": "Standar terbuka yang mirip dengan CDP." },
  { "en": "Apa itu 'PoE (Power over Ethernet)'?", "id": "Mengirimkan daya listrik melalui kabel Ethernet." },
  { "en": "Apa itu 'PSE (Power Sourcing Equipment)'?", "id": "Perangkat yang menyediakan daya PoE (misal, switch)." },
  { "en": "Apa itu 'PD (Powered Device)'?", "id": "Perangkat yang menerima daya dari PoE." },
  { "en": "Apa itu 'link aggregation'?", "id": "Menggabungkan beberapa link fisik menjadi satu." },
  { "en": "Protokol standar untuk link aggregation?", "id": "LACP (Link Aggregation Control Protocol)." },
  { "en": "Apa itu 'PAgP'?", "id": "Port Aggregation Protocol (proprietary Cisco)." },
  { "en": "Apa itu 'SD-WAN'?", "id": "Software-Defined Wide Area Network." },
  { "en": "Keuntungan SD-WAN?", "id": "Manajemen WAN yang terpusat dan fleksibel." },
  { "en": "Apa itu 'overlay network'?", "id": "Jaringan virtual di atas jaringan fisik." },
  { "en": "Apa itu 'underlay network'?", "id": "Infrastruktur jaringan fisik yang mendasarinya." },
  { "en": "Apa itu 'VXLAN'?", "id": "Virtual Extensible LAN." },
  { "en": "Fungsi VXLAN?", "id": "Membuat jaringan Lapisan 2 di atas Lapisan 3." },
  { "en": "Apa itu 'network function virtualization' (NFV)?", "id": "Menjalankan fungsi jaringan di mesin virtual." },
  { "en": "Contoh fungsi jaringan tervirtualisasi?", "id": "Router virtual, firewall virtual." },
  { "en": "Apa itu 'intent-based networking' (IBN)?", "id": "Jaringan yang mengotomatiskan berdasarkan tujuan bisnis." },
  { "en": "Apa itu 'saltstack'?", "id": "Alat otomasi jaringan dan infrastruktur." },
  { "en": "Apa itu 'puppet'?", "id": "Alat manajemen konfigurasi." },
  { "en": "Apa itu 'chef'?", "id": "Kerangka kerja otomasi infrastruktur." },
  { "en": 'Apa itu "graceful restart" pada routing?', "id": "Restart proses routing tanpa mengganggu penerusan." },
  { "en": "Apa itu 'non-stop forwarding' (NSF)?", "id": "Terus meneruskan paket saat control plane restart." },
  { "en": "Apa itu 'route dampening'?", "id": "Mengurangi dampak rute yang tidak stabil." },
  { "en": "Apa itu 'route reflector' di BGP?", "id": "Menyederhanakan konfigurasi iBGP." },
  { "en": "Apa itu 'BGP confederation'?", "id": "Membagi satu AS besar menjadi sub-AS." },
  { "en": "Apa itu 'equal-cost multi-path' (ECMP)?", "id": "Load balancing melalui beberapa rute dengan metrik sama." },
  { "en": "Apa itu 'policy-based routing' (PBR)?", "id": "Routing berdasarkan kebijakan selain alamat tujuan." },
  { "en": "Apa itu 'IP SLA'?", "id": "Fitur Cisco untuk mengukur performa jaringan." },
  { "en": "Apa itu 'object tracking'?", "id": "Melacak status objek (misal, IP SLA)." },
  { "en": "Apa itu 'VRF (Virtual Routing and Forwarding)'?", "id": "Membuat beberapa tabel routing virtual di satu router." },
  { "en": "Apa itu 'MPLS (Multi-protocol Label Switching)'?", "id": "Teknik penerusan paket menggunakan label." },
  { "en": "Apa itu 'label' dalam MPLS?", "id": "Header kecil yang disisipkan antar lapisan 2 dan 3." },
  { "en": "Apa itu 'LSP (Label Switched Path)'?", "id": "Jalur yang dilewati paket berlabel." },
  { "en": "Apa itu 'LER (Label Edge Router)'?", "id": "Router di tepi jaringan MPLS." },
  { "en": "Apa itu 'LSR (Label Switching Router)'?", "id": "Router di inti jaringan MPLS." },
  { "en": "Apa itu 'VPN MPLS Layer 3'?", "id": "Membuat VPN privat di atas jaringan MPLS." },
  { "en": "Apa itu 'DMVPN'?", "id": "Dynamic Multipoint VPN." },
  { "en": "Apa itu 'IPsec'?", "id": "Internet Protocol Security." },
  { "en": "Fungsi utama IPsec?", "id": "Menyediakan enkripsi dan otentikasi di lapisan IP." },
  { "en": "Apa itu 'tunnel mode' IPsec?", "id": "Mengenkripsi seluruh paket IP asli." },
  { "en": "Apa itu 'transport mode' IPsec?", "id": "Hanya mengenkripsi payload IP." },
  { "en": "Apa itu 'IKE (Internet Key Exchange)'?", "id": "Protokol untuk negosiasi kunci IPsec." },
  { "en": "Apa itu 'security association' (SA)?", "id": "Koneksi IPsec searah." },
  { "en": "Apa itu 'Authentication Header' (AH)?", "id": "Protokol IPsec untuk integritas dan otentikasi." },
  { "en": "Apa itu 'Encapsulating Security Payload' (ESP)?", "id": "Protokol IPsec untuk kerahasiaan." },
  { "en": "Apa itu 'GETVPN'?", "id": "Group Encrypted Transport VPN." },
  { "en": "Apa itu 'wireless LAN controller' (WLC)?", "id": "Manajemen terpusat untuk banyak access point." },
  { "en": "Apa itu 'lightweight access point' (LWAP)?", "id": "AP yang dikelola oleh WLC." },
  { "en": "Apa itu 'CAPWAP'?", "id": "Control and Provisioning of Wireless Access Points." },
  { "en": "Apa itu 'rogue access point'?", "id": "AP tidak sah yang terhubung ke jaringan." },
  { "en": "Apa itu 'wireless intrusion prevention system' (WIPS)?", "id": "Mendeteksi dan mencegah serangan nirkabel." },
  { "en": "Apa itu 'site survey' nirkabel?", "id": "Proses merencanakan penempatan AP." },
  { "en": "Apa itu 'heat map'?", "id": "Visualisasi cakupan sinyal Wi-Fi." },
  { "en": "Apa itu 'channel overlap'?", "id": "Interferensi dari AP yang menggunakan kanal berdekatan." },
  { "en": "Kanal 2.4 GHz mana yang tidak tumpang tindih?", "id": "Kanal 1, 6, dan 11." },
  { "en": "Apa itu 'signal-to-noise ratio' (SNR)?", "id": "Rasio kekuatan sinyal terhadap noise." },
  { "en": "Apa itu 'received signal strength indicator' (RSSI)?", "id": "Ukuran kekuatan sinyal yang diterima." },
  { "en": "Apa itu 'Wi-Fi 6'?", "id": "Standar IEEE 802.11ax." },
  { "en": "Fitur utama Wi-Fi 6?", "id": "OFDMA, efisiensi lebih tinggi di lingkungan padat." },
  { "en": "Apa itu 'OFDMA'?", "id": "Orthogonal Frequency-Division Multiple Access." },
  { "en": "Apa itu 'Wi-Fi 6E'?", "id": "Wi-Fi 6 yang beroperasi di pita 6 GHz." },
  { "en": "Apa itu 'Wi-Fi Direct'?", "id": "Menghubungkan perangkat Wi-Fi tanpa AP." },
  { "en": "Apa itu 'Miracast'?", "id": "Standar untuk streaming layar nirkabel." },
  { "en": "Apa itu 'zeroconf'?", "id": "Teknologi jaringan tanpa konfigurasi (APIPA)." },
  { "en": "Apa itu 'APIPA'?", "id": "Automatic Private IP Addressing." },
  { "en": "Rentang alamat APIPA?", "id": "169.254.0.0/16." },
  { "en": "Apa itu 'multicast DNS' (mDNS)?", "id": "DNS tanpa server terpusat." },
  { "en": "Apa itu 'jaringan optik pasif' (PON)?", "id": "Teknologi serat optik ke pelanggan." },
  { "en": "Apa itu 'GPON'?", "id": "Gigabit-capable Passive Optical Network." },
  { "en": "Apa itu 'OLT (Optical Line Terminal)'?", "id": "Perangkat pusat di jaringan PON." },
  { "en": "Apa itu 'ONT (Optical Network Terminal)'?", "id": "Perangkat di sisi pelanggan." },
  { "en": "Apa itu 'optical splitter'?", "id": "Membagi sinyal optik ke banyak pengguna." },
  { "en": "Apa itu 'dark web'?", "id": "Bagian web yang butuh software khusus." },
  { "en": "Apa itu 'Tor (The Onion Router)'?", "id": "Jaringan untuk komunikasi anonim." },
  { "en": "Apa itu 'blockchain'?", "id": "Buku besar digital yang terdistribusi." },
  { "en": "Apa itu 'cryptocurrency'?", "id": "Mata uang digital menggunakan kriptografi." },
  { "en": "Apa itu 'Bitcoin'?", "id": "Cryptocurrency pertama dan paling terkenal." },
  { "en": "Apa itu 'Ethereum'?", "id": "Platform blockchain dengan fungsionalitas smart contract." },
  { "en": "Apa itu 'smart contract'?", "id": "Kontrak yang dieksekusi secara otomatis." },
  { "en": "Apa itu 'hash'?", "id": "Fungsi yang mengubah input menjadi output tetap." },
  { "en": "Apa itu 'golden sample' dalam pengujian?", "id": "Perangkat referensi yang diketahui berfungsi baik." },
  { "en": "Apa itu 'network baseline'?", "id": "Ukuran performa normal dari sebuah jaringan." },
  { "en": "Apa itu 'root bridge' dalam STP?", "id": "Switch referensi utama dalam topologi STP." },
  { 'en': 'Apa itu "portfast" pada switch?', "id": "Mempercepat transisi port ke status forwarding." },
  { "en": "Apa itu 'BPDU Guard'?", "id": "Melindungi port PortFast dari BPDU yang tidak sah." },
  { "en": "Apa itu 'BPDU Filter'?", "id": "Mencegah port mengirim atau menerima BPDU." },
  { "en": "Apa itu 'root guard'?", "id": "Mencegah switch lain menjadi root bridge." },
  { "en": "Apa itu 'loop guard'?", "id": "Mencegah loop akibat masalah link searah." },
  { "en": "Apa itu 'UDLD (UniDirectional Link Detection)'?", "id": "Mendeteksi link yang hanya berfungsi satu arah." },
  { "en": "Apa itu 'flex links'?", "id": "Alternatif sederhana untuk Spanning Tree Protocol." },
  { "en": "Apa itu 'private VLAN'?", "id": "Mengisolasi port dalam VLAN yang sama." },
  { "en": "Apa itu 'community port'?", "id": "Port PVLAN yang bisa berkomunikasi satu sama lain." },
  { "en": "Apa itu 'isolated port'?", "id": "Port PVLAN yang hanya bisa ke port promiscuous." },
  { "en": "Apa itu 'promiscuous port'?", "id": "Port PVLAN yang bisa berkomunikasi ke semua." },
  { "en": "Apa itu 'VTP (VLAN Trunking Protocol)'?", "id": "Protokol Cisco untuk menyinkronkan database VLAN." },
  { "en": "Apa itu 'VTP pruning'?", "id": "Mencegah lalu lintas VLAN yang tidak perlu." },
  { "en": "Apa itu 'DTP (Dynamic Trunking Protocol)'?", "id": "Protokol Cisco untuk negosiasi trunk otomatis." },
  { "en": "Apa itu 'CDP (Cisco Discovery Protocol)'?", "id": "Menemukan informasi tentang perangkat Cisco tetangga." },
  { "en": "Apa itu 'LLDP (Link Layer Discovery Protocol)'?", "id": "Standar terbuka yang mirip dengan CDP." },
  { "en": "Apa itu 'PoE (Power over Ethernet)'?", "id": "Mengirimkan daya listrik melalui kabel Ethernet." },
  { "en": "Apa itu 'PSE (Power Sourcing Equipment)'?", "id": "Perangkat yang menyediakan daya PoE (misal, switch)." },
  { "en": "Apa itu 'PD (Powered Device)'?", "id": "Perangkat yang menerima daya dari PoE." },
  { "en": "Apa itu 'link aggregation'?", "id": "Menggabungkan beberapa link fisik menjadi satu." },
  { "en": "Protokol standar untuk link aggregation?", "id": "LACP (Link Aggregation Control Protocol)." },
  { "en": "Apa itu 'PAgP'?", "id": "Port Aggregation Protocol (proprietary Cisco)." },
  { "en": "Apa itu 'SD-WAN'?", "id": "Software-Defined Wide Area Network." },
  { "en": "Keuntungan SD-WAN?", "id": "Manajemen WAN yang terpusat dan fleksibel." },
  { "en": "Apa itu 'overlay network'?", "id": "Jaringan virtual di atas jaringan fisik." },
  { "en": "Apa itu 'underlay network'?", "id": "Infrastruktur jaringan fisik yang mendasarinya." },
  { "en": "Apa itu 'VXLAN'?", "id": "Virtual Extensible LAN." },
  { "en": "Fungsi VXLAN?", "id": "Membuat jaringan Lapisan 2 di atas Lapisan 3." },
  { "en": "Apa itu 'network function virtualization' (NFV)?", "id": "Menjalankan fungsi jaringan di mesin virtual." },
  { "en": "Contoh fungsi jaringan tervirtualisasi?", "id": "Router virtual, firewall virtual." },
  { "en": "Apa itu 'intent-based networking' (IBN)?", "id": "Jaringan yang mengotomatiskan berdasarkan tujuan bisnis." },
  { "en": "Apa itu 'saltstack'?", "id": "Alat otomasi jaringan dan infrastruktur." },
  { "en": "Apa itu 'puppet'?", "id": "Alat manajemen konfigurasi." },
  { "en": "Apa itu 'chef'?", "id": "Kerangka kerja otomasi infrastruktur." },
  { 'en': 'Apa itu "graceful restart" pada routing?', "id": "Restart proses routing tanpa mengganggu penerusan." },
  { "en": "Apa itu 'non-stop forwarding' (NSF)?", "id": "Terus meneruskan paket saat control plane restart." },
  { "en": "Apa itu 'route dampening'?", "id": "Mengurangi dampak rute yang tidak stabil." },
  { "en": "Apa itu 'route reflector' di BGP?", "id": "Menyederhanakan konfigurasi iBGP." },
  { "en": "Apa itu 'BGP confederation'?", "id": "Membagi satu AS besar menjadi sub-AS." },
  { "en": "Apa itu 'equal-cost multi-path' (ECMP)?", "id": "Load balancing melalui beberapa rute dengan metrik sama." },
  { "en": "Apa itu 'policy-based routing' (PBR)?", "id": "Routing berdasarkan kebijakan selain alamat tujuan." },
  { "en": "Apa itu 'IP SLA'?", "id": "Fitur Cisco untuk mengukur performa jaringan." },
  { "en": "Apa itu 'object tracking'?", "id": "Melacak status objek (misal, IP SLA)." },
  { "en": "Apa itu 'VRF (Virtual Routing and Forwarding)'?", "id": "Membuat beberapa tabel routing virtual di satu router." },
  { "en": "Apa itu 'MPLS (Multi-protocol Label Switching)'?", "id": "Teknik penerusan paket menggunakan label." },
  { "en": "Apa itu 'label' dalam MPLS?", "id": "Header kecil yang disisipkan antar lapisan 2 dan 3." },
  { "en": "Apa itu 'LSP (Label Switched Path)'?", "id": "Jalur yang dilewati paket berlabel." },
  { "en": "Apa itu 'LER (Label Edge Router)'?", "id": "Router di tepi jaringan MPLS." },
  { "en": "Apa itu 'LSR (Label Switching Router)'?", "id": "Router di inti jaringan MPLS." },
  { "en": "Apa itu 'VPN MPLS Layer 3'?", "id": "Membuat VPN privat di atas jaringan MPLS." },
  { "en": "Apa itu 'DMVPN'?", "id": "Dynamic Multipoint VPN." },
  { "en": "Apa itu 'IPsec'?", "id": "Internet Protocol Security." },
  { "en": "Fungsi utama IPsec?", "id": "Menyediakan enkripsi dan otentikasi di lapisan IP." },
  { "en": "Apa itu 'tunnel mode' IPsec?", "id": "Mengenkripsi seluruh paket IP asli." },
  { "en": "Apa itu 'transport mode' IPsec?", "id": "Hanya mengenkripsi payload IP." },
  { "en": "Apa itu 'IKE (Internet Key Exchange)'?", "id": "Protokol untuk negosiasi kunci IPsec." },
  { "en": "Apa itu 'security association' (SA)?", "id": "Koneksi IPsec searah." },
  { "en": "Apa itu 'Authentication Header' (AH)?", "id": "Protokol IPsec untuk integritas dan otentikasi." },
  { "en": "Apa itu 'Encapsulating Security Payload' (ESP)?", "id": "Protokol IPsec untuk kerahasiaan." },
  { "en": "Apa itu 'GETVPN'?", "id": "Group Encrypted Transport VPN." },
  { "en": "Apa itu 'wireless LAN controller' (WLC)?", "id": "Manajemen terpusat untuk banyak access point." },
  { "en": "Apa itu 'lightweight access point' (LWAP)?", "id": "AP yang dikelola oleh WLC." },
  { "en": "Apa itu 'CAPWAP'?", "id": "Control and Provisioning of Wireless Access Points." },
  { "en": "Apa itu 'rogue access point'?", "id": "AP tidak sah yang terhubung ke jaringan." },
  { "en": "Apa itu 'wireless intrusion prevention system' (WIPS)?", "id": "Mendeteksi dan mencegah serangan nirkabel." },
  { "en": "Apa itu 'site survey' nirkabel?", "id": "Proses merencanakan penempatan AP." },
  { "en": "Apa itu 'heat map'?", "id": "Visualisasi cakupan sinyal Wi-Fi." },
  { "en": "Apa itu 'channel overlap'?", "id": "Interferensi dari AP yang menggunakan kanal berdekatan." },
  { "en": "Kanal 2.4 GHz mana yang tidak tumpang tindih?", "id": "Kanal 1, 6, dan 11." },
  { "en": "Apa itu 'signal-to-noise ratio' (SNR)?", "id": "Rasio kekuatan sinyal terhadap noise." },
  { "en": "Apa itu 'received signal strength indicator' (RSSI)?", "id": "Ukuran kekuatan sinyal yang diterima." },
  { "en": "Apa itu 'Wi-Fi 6'?", "id": "Standar IEEE 802.11ax." },
  { "en": "Fitur utama Wi-Fi 6?", "id": "OFDMA, efisiensi lebih tinggi di lingkungan padat." },
  { "en": "Apa itu 'OFDMA'?", "id": "Orthogonal Frequency-Division Multiple Access." },
  { "en": "Apa itu 'Wi-Fi 6E'?", "id": "Wi-Fi 6 yang beroperasi di pita 6 GHz." },
  { "en": "Apa itu 'Wi-Fi Direct'?", "id": "Menghubungkan perangkat Wi-Fi tanpa AP." },
  { "en": "Apa itu 'Miracast'?", "id": "Standar untuk streaming layar nirkabel." },
  { "en": "Apa itu 'zeroconf'?", "id": "Teknologi jaringan tanpa konfigurasi (APIPA)." },
  { "en": "Apa itu 'APIPA'?", "id": "Automatic Private IP Addressing." },
  { "en": "Rentang alamat APIPA?", "id": "169.254.0.0/16." },
  { "en": "Apa itu 'multicast DNS' (mDNS)?", "id": "DNS tanpa server terpusat." },
  { "en": "Apa itu 'jaringan optik pasif' (PON)?", "id": "Teknologi serat optik ke pelanggan." },
  { "en": "Apa itu 'GPON'?", "id": "Gigabit-capable Passive Optical Network." },
  { "en": "Apa itu 'OLT (Optical Line Terminal)'?", "id": "Perangkat pusat di jaringan PON." },
  { "en": "Apa itu 'ONT (Optical Network Terminal)'?", "id": "Perangkat di sisi pelanggan." },
  { "en": "Apa itu 'optical splitter'?", "id": "Membagi sinyal optik ke banyak pengguna." },
  { "en": "Apa itu 'dark web'?", "id": "Bagian web yang butuh software khusus." },
  { "en": "Apa itu 'Tor (The Onion Router)'?", "id": "Jaringan untuk komunikasi anonim." },
  { "en": "Apa itu 'blockchain'?", "id": "Buku besar digital yang terdistribusi." },
  { "en": "Apa itu 'cryptocurrency'?", "id": "Mata uang digital menggunakan kriptografi." },
  { "en": "Apa itu 'Bitcoin'?", "id": "Cryptocurrency pertama dan paling terkenal." },
  { "en": "Apa itu 'Ethereum'?", "id": "Platform blockchain dengan fungsionalitas smart contract." },
  { "en": "Apa itu 'smart contract'?", "id": "Kontrak yang dieksekusi secara otomatis." },
  { "en": "Apa itu 'hash'?", "id": "Fungsi yang mengubah input menjadi output tetap." },
  { "en": "Apa perbedaan utama HTTP/1.1 dan HTTP/2?", "id": "HTTP/2 menggunakan multiplexing pada satu koneksi." },
  { "en": "Apa itu 'multiplexing' di HTTP/2?", "id": "Mengirim banyak request/response secara bersamaan." },
  { "en": "Apa itu 'head-of-line blocking'?", "id": "Satu paket lambat menahan semua paket di belakangnya." },
  { "en": "Bagaimana HTTP/2 mengatasi 'head-of-line blocking'?", "id": "Dengan stream multiplexing." },
  { "en": "Apa itu 'server push'?", "id": "Server mengirim sumber daya sebelum browser meminta." },
  { "en": "Apa itu 'HTTP/3'?", "id": "Versi HTTP terbaru yang menggunakan QUIC." },
  { "en": "Apa itu 'QUIC'?", "id": "Protokol transport baru di atas UDP." },
  { "en": "Apa keuntungan QUIC dibanding TCP?", "id": "Koneksi lebih cepat, mengatasi head-of-line blocking." },
  { "en": "Apa itu 'REST (Representational State Transfer)'?", "id": "Gaya arsitektur untuk membangun layanan web." },
  { "en": "Metode HTTP apa yang digunakan untuk membaca data?", "id": "Metode GET." },
  { "en": "Metode HTTP apa yang digunakan untuk membuat data baru?", "id": "Metode POST." },
  { "en": "Metode HTTP apa yang digunakan untuk memperbarui data?", "id": "Metode PUT atau PATCH." },
  { "en": "Metode HTTP apa yang digunakan untuk menghapus data?", "id": "Metode DELETE." },
  { "en": "Apa itu 'idempotent'?", "id": "Operasi yang hasilnya sama jika diulang." },
  { "en": "Metode HTTP mana yang 'idempotent'?", "id": "GET, PUT, DELETE." },
  { "en": "Apa itu 'WebSocket'?", "id": "Protokol komunikasi dua arah full-duplex." },
  { "en": "Kapan WebSocket ideal digunakan?", "id": "Untuk aplikasi real-time seperti chat atau game." },
  { "en": "Apa itu 'server-sent events' (SSE)?", "id": "Server mengirim pembaruan ke klien secara otomatis." },
  { "en": "Apa beda SSE dan WebSocket?", "id": "SSE hanya satu arah (server ke klien)." },
  { "en": "Apa itu 'long polling'?", "id": "Klien menahan koneksi terbuka menunggu respons." },
  { "en": "Apa itu 'Remote Procedure Call' (RPC)?", "id": "Memanggil fungsi di mesin lain seolah-olah lokal." },
  { "en": "Apa itu 'gRPC'?", "id": "Framework RPC modern yang dikembangkan Google." },
  { "en": "Apa itu 'Protocol Buffers'?", "id": "Mekanisme serialisasi data yang efisien." },
  { "en": "Apa itu 'serialisasi' data?", "id": "Mengubah struktur data menjadi format untuk transmisi." },
  { "en": "Apa itu 'deserialisasi' data?", "id": "Mengembalikan data dari format transmisi." },
  { "en": "Apa itu 'MIME (Multipurpose Internet Mail Extensions)'?", "id": "Standar untuk format lampiran email." },
  { "en": "Apa itu 'base64 encoding'?", "id": "Mengubah data biner menjadi teks." },
  { "en": "Apa itu 'POP3 (Post Office Protocol 3)'?", "id": "Protokol untuk mengunduh email ke klien." },
  { "en": "Apa itu 'IMAP (Internet Message Access Protocol)'?", "id": "Protokol untuk mengakses email di server." },
  { "en": "Perbedaan utama POP3 dan IMAP?", "id": "IMAP menyinkronkan email, POP3 mengunduh." },
  { "en": "Port default untuk POP3S (aman)?", "id": "Port 995." },
  { "en": "Port default untuk IMAPS (aman)?", "id": "Port 993." },
  { "en": "Port default untuk SMTPS (aman)?", "id": "Port 465 atau 587 (STARTTLS)." },
  { "en": "Apa itu 'STARTTLS'?", "id": "Memulai enkripsi TLS pada koneksi yang ada." },
  { "en": "Apa itu 'SPF (Sender Policy Framework)'?", "id": "Mekanisme untuk memverifikasi server pengirim email." },
  { "en": "Apa itu 'DKIM (DomainKeys Identified Mail)'?", "id": "Menambahkan tanda tangan digital ke email." },
  { "en": "Apa itu 'DMARC'?", "id": "Kebijakan untuk menangani email yang gagal SPF/DKIM." },
  { "en": "Apa itu 'XMPP'?", "id": "Extensible Messaging and Presence Protocol." },
  { "en": "Di mana XMPP sering digunakan?", "id": "Untuk aplikasi instant messaging (chat)." },
  { "en": "Apa itu 'MQTT'?", "id": "Message Queuing Telemetry Transport." },
  { "en": "Kapan MQTT ideal digunakan?", "id": "Untuk komunikasi perangkat IoT." },
  { "en": "Model komunikasi MQTT?", "id": "Publish-subscribe (pub/sub)." },
  { "en": "Apa itu 'broker' dalam MQTT?", "id": "Server pusat yang menerima dan meneruskan pesan." },
  { "en": "Apa itu 'topic' dalam MQTT?", "id": "Label untuk mengkategorikan pesan." },
  { "en": "Apa itu 'CoAP'?", "id": "Constrained Application Protocol." },
  { "en": "Di mana CoAP digunakan?", "id": "Untuk perangkat IoT dengan sumber daya terbatas." },
  { "en": "Apa itu 'root name server'?", "id": "Server DNS paling atas dalam hierarki." },
  { "en": "Ada berapa 'root name server' di dunia?", "id": "Ada 13 klaster root server logis." },
  { "en": "Apa itu 'Top-Level Domain' (TLD)?", "id": "Bagian akhir dari nama domain (.com, .id)." },
  { "en": "Contoh 'generic TLD' (gTLD)?", "id": ".com, .org, .net." },
  { "en": "Contoh 'country-code TLD' (ccTLD)?", "id": ".id (Indonesia), .jp (Jepang)." },
  { "en": "Apa itu 'authoritative DNS server'?", "id": "Server yang memegang record DNS asli." },
  { "en": "Apa itu 'caching DNS server'?", "id": "Menyimpan sementara hasil query DNS." },
  { "en": "Apa itu 'round-robin DNS'?", "id": "Teknik load balancing sederhana menggunakan DNS." },
  { "en": "Apa itu 'DNS zone transfer'?", "id": "Menyalin data zona antara server DNS." },
  { "en": "Apa itu 'zone file'?", "id": "File teks yang berisi record DNS." },
  { "en": "Apa itu 'SOA (Start of Authority) record'?", "id": "Menyimpan info administratif tentang zona DNS." },
  { "en": "Apa itu 'NS (Name Server) record'?", "id": "Menentukan server DNS otoritatif untuk domain." },
  { "en": "Apa itu 'TXT record'?", "id": "Menyimpan informasi teks (untuk SPF, DKIM)." },
  { "en": "Apa itu 'SRV (Service) record'?", "id": "Menentukan host dan port untuk layanan." },
  { "en": "Apa itu 'wildcard DNS record'?", "id": "Record yang cocok untuk semua subdomain." },
  { "en": "Apa itu 'Dynamic DNS' (DDNS)?", "id": "Memperbarui record DNS secara otomatis." },
  { "en": "Apa itu 'split-horizon DNS'?", "id": "Memberikan jawaban DNS berbeda (internal/eksternal)." },
  { "en": "Apa itu 'time synchronization'?", "id": "Menjaga agar waktu di semua perangkat akurat." },
  { "en": "Apa itu 'stratum' dalam NTP?", "id": "Tingkat hierarki dari sebuah server waktu." },
  { "en": "Stratum 0 adalah apa?", "id": "Perangkat waktu presisi tinggi (jam atom)." },
  { "en": "Apa itu 'SNMP Get'?", "id": "Permintaan manajer untuk mengambil nilai dari agen." },
  { "en": "Apa itu 'SNMP Set'?", "id": "Permintaan manajer untuk mengubah nilai di agen." },
  { "en": "Apa itu 'SNMP Walk'?", "id": "Mengambil seluruh pohon MIB dari agen." },
  { "en": "Apa itu 'community string'?", "id": "Kata sandi sederhana dalam SNMPv1/v2c." },
  { "en": "Apakah SNMPv1/v2c aman?", "id": "Tidak, karena community string dikirim tanpa enkripsi." },
  { "en": "Versi SNMP mana yang paling aman?", "id": "SNMPv3, dengan enkripsi dan otentikasi." },
  { "en": "Apa itu 'RMON'?", "id": "Remote Monitoring." },
  { "en": "Apa itu 'file sharing protocol'?", "id": "Protokol untuk berbagi file di jaringan." },
  { "en": "Contoh file sharing protocol?", "id": "SMB, NFS, AFP." },
  { "en": "Apa itu 'SMB (Server Message Block)'?", "id": "Protokol file sharing yang digunakan Windows." },
  { "en": "Apa itu 'NFS (Network File System)'?", "id": "Protokol file sharing yang digunakan Unix/Linux." },
  { "en": "Apa itu 'AFP (Apple Filing Protocol)'?", "id":"Protokol file sharing yang digunakan Apple." },
  { "en": "Apa itu 'BitTorrent'?", "id": "Protokol file sharing peer-to-peer." },
  { "en": "Apa itu 'tracker' di BitTorrent?", "id": "Server yang mengoordinasikan peer." },
  { "en": "Apa itu 'seed' di BitTorrent?", "id": "Peer yang memiliki file 100% lengkap." },
  { "en": "Apa itu 'leech' di BitTorrent?", "id": "Peer yang sedang mengunduh file." },
  { "en": "Apa itu 'remote desktop protocol'?", "id": "Protokol untuk mengakses desktop dari jarak jauh." },
  { "en": "Contoh remote desktop protocol?", "id": "RDP, VNC, TeamViewer." },
  { "en": "Apa itu 'RDP (Remote Desktop Protocol)'?", "id": "Protokol remote desktop milik Microsoft." },
  { "en": "Apa itu 'VNC (Virtual Network Computing)'?", "id": "Sistem remote desktop lintas platform." },
  { "en": "Apa itu 'Telnet'?", "id": "Protokol remote terminal yang tidak terenkripsi." },
  { "en": "Apa itu 'SSH (Secure Shell)'?", "id": "Protokol remote terminal yang aman." },
  { "en": "Apa itu 'port knocking'?", "id": "Mekanisme keamanan untuk membuka port." },
  { "en": "Apa itu 'directory service'?", "id": "Layanan untuk menyimpan dan mengelola informasi jaringan." },
  { "en": "Contoh directory service?", "id": "Active Directory, OpenLDAP." },
  { "en": "Apa itu 'Active Directory'?", "id": "Layanan direktori milik Microsoft." },
  { "en": "Apa itu 'Kerberos'?", "id": "Protokol otentikasi default di Active Directory." },
  { "en": "Apa itu 'ticket' dalam Kerberos?", "id": "Bukti terenkripsi untuk mengakses layanan." },
  { "en": "Apa itu 'IEEE 802.1X'?", "id": "Standar untuk otentikasi akses jaringan berbasis port." },
  { "en": "Apa tiga peran dalam 802.1X?", "id": "Supplicant, Authenticator, dan Authentication Server." },
  { "en": "Apa itu 'supplicant'?", "id": "Perangkat klien yang meminta akses." },
  { "en": "Apa itu 'authenticator'?", "id": "Switch atau Access Point." },
  { "en": "Apa itu 'authentication server'?", "id": "Server yang memvalidasi kredensial (misal, RADIUS)." },
  { "en": "Apa itu 'EAP (Extensible Authentication Protocol)'?", "id": "Kerangka kerja otentikasi yang digunakan 802.1X." },
  { "en": "Apa itu 'EAP-TLS'?", "id": "EAP yang menggunakan sertifikat digital." },
  { "en": "Apa itu 'PEAP'?", "id": "Protected EAP." },
  { "en": "Apa itu 'RADIUS'?", "id": "Remote Authentication Dial-In User Service." },
  { "en": "Apa itu 'TACACS+'?", "id": "Protokol otentikasi, otorisasi, dan akuntansi Cisco." },
  { "en": "Perbedaan utama RADIUS dan TACACS+?", "id": "TACACS+ mengenkripsi seluruh paket." },
  { "en": "Apa itu 'authorization'?", "id": "Menentukan hak akses setelah otentikasi." },
  { "en": "Apa itu 'accounting'?", "id": "Mencatat aktivitas dan penggunaan sumber daya." },
  { "en": "Apa itu 'single point of failure'?", "id": "Satu komponen yang jika gagal, sistem mati." },
  { "en": "Apa itu 'redundansi'?", "id": "Menyediakan komponen cadangan untuk keandalan." },
  { "en": "Apa itu 'high availability' (HA)?", "id": "Sistem yang dirancang untuk beroperasi terus-menerus." },
  { "en": "Apa itu 'failover'?", "id": "Proses perpindahan otomatis ke sistem cadangan." },
  { "en": "Apa itu 'load balancing'?", "id": "Mendistribusikan beban kerja ke beberapa server." },
  { "en": "Apa itu 'First Hop Redundancy Protocol' (FHRP)?", "id": "Menyediakan gateway default yang redundan." },
  { "en": "Apa itu 'HSRP (Hot Standby Router Protocol)'?", "id": "FHRP proprietary dari Cisco." },
  { "en": "Apa itu 'VRRP (Virtual Router Redundancy Protocol)'?", "id": "FHRP standar terbuka (open standard)." },
  { "en": "Apa itu 'GLBP (Gateway Load Balancing Protocol)'?", "id": "FHRP Cisco yang juga melakukan load balancing." },
  { "en": "Apa itu 'virtual IP address'?", "id": "Alamat IP yang dibagi oleh beberapa router." },
  { "en": "Apa itu 'active router' di HSRP?", "id": "Router yang sedang aktif meneruskan lalu lintas." },
  { "en": "Apa itu 'standby router' di HSRP?", "id": "Router cadangan yang akan mengambil alih." },
  { "en": "Apa itu 'preemption'?", "id": "Router prioritas lebih tinggi merebut kembali peran aktif." },
  { "en": "Apa itu 'syslog'?", "id": "Protokol standar untuk mengirim pesan log." },
  { "en": "Apa itu 'severity level' di syslog?", "id": "Tingkat kepentingan pesan log (0-7)." },
  { "en": "Severity level mana yang paling kritis?", "id": "Level 0 (Emergency)." },
  { "en": "Apa itu 'facility' di syslog?", "id": "Sumber dari pesan log (misal, kernel)." },
  { "en": "Apa itu 'NTP (Network Time Protocol)'?", "id": "Protokol untuk menyinkronkan waktu jam." },
  { "en": "Apa itu 'stratum' di NTP?", "id": "Tingkat hierarki dari sebuah server waktu." },
  { "en": "Stratum 0 adalah apa?", "id": "Jam referensi presisi tinggi (misal, atom)." },
  { "en": "Apa itu 'SNMP (Simple Network Management Protocol)'?", "id": "Protokol untuk mengelola perangkat jaringan." },
  { "en": "Apa itu 'NMS (Network Management Station)'?", "id": "Sistem yang memonitor perangkat." },
  { "en": "Apa itu 'SNMP agent'?", "id": "Perangkat lunak di perangkat yang dimonitor." },
  { "en": "Apa itu 'MIB (Management Information Base)'?", "id": "Database informasi terstruktur pada agen." },
  { "en": "Apa itu 'OID (Object Identifier)'?", "id": "Alamat unik untuk variabel di MIB." },
  { "en": "Apa itu 'SNMP Get'?", "id": "Operasi untuk mengambil nilai dari agen." },
  { "en": "Apa itu 'SNMP Set'?", "id": "Operasi untuk mengubah nilai di agen." },
  { "en": "Apa itu 'SNMP Trap'?", "id": "Notifikasi spontan yang dikirim oleh agen." },
  { "en": "Versi SNMP mana yang paling aman?", "id": "SNMPv3, karena mendukung enkripsi." },
  { "en": "Apa itu 'NetFlow'?", "id": "Protokol Cisco untuk mengumpulkan metadata lalu lintas." },
  { "en": "Apa itu 'IPFIX'?", "id": "Versi standar terbuka dari NetFlow." },
  { "en": "Apa itu 'SPAN (Switched Port Analyzer)'?", "id": "Fitur Cisco untuk port mirroring." },
  { "en": "Apa itu 'RSPAN (Remote SPAN)'?", "id": "SPAN yang mengirim lalu lintas ke VLAN khusus." },
  { "en": "Apa itu 'ERSPAN (Encapsulated RSPAN)'?", "id": "RSPAN yang merutekan lalu lintas." },
  { "en": "Apa itu 'network tap'?", "id": "Perangkat keras pasif untuk menyalin lalu lintas." },
  { "en": "Apa itu 'IP SLA'?", "id": "Fitur Cisco untuk mengukur performa jaringan." },
  { "en": "Apa itu 'CDP (Cisco Discovery Protocol)'?", "id": "Protokol untuk menemukan perangkat Cisco tetangga." },
  { "en": "Apa itu 'LLDP (Link Layer Discovery Protocol)'?", "id": "Standar terbuka yang mirip CDP." },
  { "en": "Apa itu 'jumbo frame'?", "id": "Frame Ethernet yang ukurannya lebih dari 1500 byte." },
  { "en": "Apa itu 'baby giant frame'?", "id": "Frame yang sedikit lebih besar dari 1500 byte." },
  { "en": "Apa itu 'MTU (Maximum Transmission Unit)'?", "id": "Ukuran paket maksimum di Lapisan 3." },
  { "en": "Apa itu 'MSS (Maximum Segment Size)'?", "id": "Ukuran data maksimum dalam segmen TCP." },
  { "en": "Apa itu 'path MTU discovery'?", "id": "Proses menemukan MTU terkecil di sepanjang jalur." },
  { "en": "Apa itu 'ICMP'?", "id": "Internet Control Message Protocol." },
  { "en": "Pesan ICMP apa yang digunakan ping?", "id": "Echo Request dan Echo Reply." },
  { "en": "Pesan ICMP apa yang digunakan traceroute?", "id": "Echo Request dengan TTL yang meningkat." },
  { "en": "Apa itu 'ICMP unreachable'?", "id": "Pesan bahwa tujuan tidak bisa dijangkau." },
  { "en": "Apa itu 'ICMP redirect'?", "id": "Pesan untuk menggunakan router yang lebih baik." },
  { "en": "Apa itu 'broadcast address'?", "id": "Alamat IP untuk mengirim ke semua host." },
  { "en": "Apa itu 'network address'?", "id": "Alamat IP pertama yang mengidentifikasi jaringan." },
  { "en": "Alamat IP mana yang tidak bisa dipakai host?", "id": "Network address dan broadcast address." },
  { "en": "Apa itu 'classful addressing'?", "id": "Sistem pengalamatan IP lama (Kelas A, B, C)." },
  { "en": "Apa itu 'classless addressing'?", "id": "Sistem pengalamatan IP modern (CIDR)." },
  { "en": "Apa itu 'address pool' di DHCP?", "id": "Kumpulan alamat IP yang bisa disewakan." },
  { "en": "Apa itu 'lease time' di DHCP?", "id": "Durasi penyewaan sebuah alamat IP." },
  { "en": "Apa itu 'DHCP relay agent'?", "id": "Meneruskan permintaan DHCP antar jaringan." },
  { "en": "Apa itu 'DHCP starvation attack'?", "id": "Serangan yang menghabiskan semua IP di pool." },
  { "en": "Apa itu 'rogue DHCP server'?", "id": "Server DHCP tidak sah di jaringan." },
  { "en": "Apa itu 'DHCP snooping'?", "id": "Fitur keamanan untuk memblokir server DHCP palsu." },
  { "en": "Apa itu 'unicast flooding'?", "id": "Saat switch mengirim frame ke semua port." },
  { "en": "Apa itu 'CAM table'?", "id": "Tabel alamat MAC pada switch." },
  { "en": "Apa itu 'MAC address spoofing'?", "id": "Memalsukan alamat MAC dari sebuah perangkat." },
  { "en": "Apa itu 'broadcast storm'?", "id": "Lalu lintas broadcast berlebihan yang melumpuhkan jaringan." },
  { "en": "Apa itu 'loop prevention'?", "id": "Mekanisme seperti STP untuk mencegah loop." },
  { "en": "Apa itu 'BPDU'?", "id": "Bridge Protocol Data Unit (paket STP)." },
  { "en": "Apa itu 'root bridge'?", "id": "Switch pusat dalam topologi STP." },
  { "en": "Bagaimana 'root bridge' dipilih?", "id": "Berdasarkan Bridge ID terendah." },
  { "en": "Apa itu 'root port'?", "id": "Port dengan jalur terbaik ke root bridge." },
  { "en": "Apa itu 'designated port'?", "id": "Port yang meneruskan lalu lintas di segmen." },
  { "en": "Apa itu 'blocked port'?", "id": "Port yang diblokir STP untuk mencegah loop." },
  { "en": "Status port STP apa saja?", "id": "Blocking, Listening, Learning, Forwarding." },
  { "en": "Apa itu 'Rapid STP' (RSTP)?", "id": "Versi STP yang konvergensinya lebih cepat." },
  { "en": "Apa itu 'port state' di RSTP?", "id": "Discarding, Learning, atau Forwarding." },
  { "en": "Apa itu 'port role' di RSTP?", "id": "Root, Designated, Alternate, atau Backup." },
  { "en": "Apa itu 'alternate port'?", "id": "Port cadangan menuju root bridge." },
  { "en": "Apa itu 'backup port'?", "id": "Port cadangan untuk segmen yang sama." },
  { "en": "Apa itu 'edge port'?", "id": "Istilah RSTP untuk port yang terhubung ke host." },
  { "en": "Apa itu 'PVRST+'?", "id": "Per-VLAN Rapid Spanning Tree Plus (Cisco)." },
  { "en": "Apa itu 'MSTP (Multiple STP)'?", "id": "Memetakan beberapa VLAN ke satu instance STP." },
  { "en": "Apa itu 'split horizon'?", "id": "Aturan routing untuk mencegah loop." },
  { "en": "Apa itu 'poison reverse'?", "id": "Teknik pencegahan loop routing." },
  { "en": "Apa itu 'holddown timer'?", "id": "Timer untuk mengabaikan info rute yang buruk." },
  { "en": "Apa itu 'route summarization'?", "id": "Menggabungkan banyak rute menjadi satu." },
  { "en": "Apa keuntungan 'route summarization'?", "id": "Mengurangi ukuran tabel routing." },
  { "en": "Apa itu 'TCP sequence number'?", "id": "Nomor urut untuk menyusun kembali data." },
  { "en": "Apa itu 'TCP acknowledgment number'?", "id": "Nomor urut byte berikutnya yang diharapkan." },
  { "en": "Apa fungsi flag TCP 'SYN'?", "id": "Memulai dan menyinkronkan sebuah koneksi." },
  { "en": "Apa fungsi flag TCP 'ACK'?", "id": "Memberikan konfirmasi penerimaan data." },
  { "en": "Apa fungsi flag TCP 'FIN'?", "id": "Mengakhiri koneksi secara normal." },
  { "en": "Apa fungsi flag TCP 'RST'?", "id": "Me-reset atau menolak sebuah koneksi." },
  { "en": "Apa fungsi flag TCP 'PSH'?", "id": "Memerintahkan pengiriman data tanpa menunggu buffer." },
  { "en": "Apa fungsi flag TCP 'URG'?", "id": "Menandakan data yang harus diproses segera." },
  { "en": "Apa itu 'TCP window size'?", "id": "Jumlah data yang bisa dikirim sebelum ACK." },
  { "en": "Apa itu 'window scaling'?", "id": "Opsi untuk memperbesar ukuran window TCP." },
  { "en": "Apa itu 'Nagle's algorithm'?", "id": "Menggabungkan data kecil untuk efisiensi TCP." },
  { "en": "Apa itu 'silly window syndrome'?", "id": "Masalah efisiensi akibat pengiriman data kecil." },
  { "en": "Apa itu 'selective acknowledgment' (SACK)?", "id": "Memberi tahu segmen mana yang hilang." },
  { "en": "Apa itu 'TCP keepalive'?", "id": "Mengecek apakah koneksi masih aktif." },
  { "en": "Apa itu 'TIME_WAIT state'?", "id": "Status socket setelah koneksi ditutup." },
  { "en": "Apa itu 'ephemeral port'?", "id": "Port sementara yang digunakan oleh klien." },
  { "en": "Apa 'Type' field di frame Ethernet?", "id": "Menunjukkan protokol Lapisan 3 yang digunakan." },
  { "en": "Apa itu 'preamble'?", "id": "Tujuh byte untuk sinkronisasi di frame Ethernet." },
  { "en": "Apa itu 'SFD (Start Frame Delimiter)'?", "id": "Satu byte yang menandai awal frame." },
  { "en": "Apa itu 'MTU (Maximum Transmission Unit)'?", "id": "Ukuran paket maksimum di Lapisan 3." },
  { "en": "Apa itu 'fragmentation'?", "id": "Memecah paket agar sesuai dengan MTU." },
  { "en": "Field apa di header IP yang mencegah loop?", "id": "TTL (Time To Live)." },
  { "en": "Field apa di header IP yang menandai QoS?", "id": "DSCP (Differentiated Services Code Point)." },
  { "en": "Apa itu 'protocol field' di header IP?", "id": "Menunjukkan protokol Lapisan 4 (TCP/UDP)." },
  { "en": "Nilai 'protocol field' untuk TCP?", "id": "Nilainya adalah 6." },
  { "en": "Nilai 'protocol field' untuk UDP?", "id": "Nilainya adalah 17." },
  { "en": "Nilai 'protocol field' untuk ICMP?", "id": "Nilainya adalah 1." },
  { "en": "Apa itu 'BSS (Basic Service Set)'?", "id": "Satu access point dan semua kliennya." },
  { "en": "Apa itu 'ESS (Extended Service Set)'?", "id": "Dua atau lebih BSS yang terhubung." },
  { "en": "Apa itu 'IBSS (Independent BSS)'?", "id": "Jaringan ad-hoc tanpa access point." },
  { "en": "Apa itu 'beacon frame'?", "id": "Frame yang diiklankan oleh access point." },
  { "en": "Apa isi dari 'beacon frame'?", "id": "SSID, kanal, dan info keamanan." },
  { "en": "Apa itu 'probe request'?", "id": "Frame yang dikirim klien untuk mencari jaringan." },
  { "en": "Apa itu 'probe response'?", "id": "Jawaban dari access point atas probe request." },
  { "en": "Apa itu 'association request'?", "id": "Permintaan klien untuk bergabung ke BSS." },
  { "en": "Apa itu 'association response'?", "id": "Jawaban dari AP atas permintaan asosiasi." },
  { "en": "Apa itu 'CSMA/CA'?", "id": "Carrier Sense Multiple Access/Collision Avoidance." },
  { "en": "Apa itu 'DIFS (DCF Interframe Space)'?", "id": "Waktu tunggu awal sebelum mengirim frame." },
  { "en": "Apa itu 'SIFS (Short Interframe Space)'?", "id": "Waktu tunggu terpendek antar frame." },
  { "en": "Apa itu 'RTS/CTS'?", "id": "Request to Send / Clear to Send." },
  { "en": "Fungsi RTS/CTS?", "id": "Mengatasi masalah 'hidden node'." },
  { "en": "Apa itu 'hidden node problem'?", "id": "Dua klien tidak bisa saling mendengar." },
  { "en": "Apa itu 'exposed node problem'?", "id": "Node salah mengira media sedang sibuk." },
  { "en": "Apa itu '802.11a'?", "id": "Standar Wi-Fi di pita 5 GHz." },
  { "en": "Apa itu '802.11g'?", "id": "Standar Wi-Fi di pita 2.4 GHz (54 Mbps)." },
  { "en": "Apa itu '802.11n' (Wi-Fi 4)?", "id": "Memperkenalkan MIMO dan channel bonding." },
  { "en": "Apa itu '802.11ac' (Wi-Fi 5)?", "id": "Hanya di 5 GHz, kecepatan lebih tinggi." },
  { "en": "Apa itu '802.11ax' (Wi-Fi 6)?", "id": "Fokus pada efisiensi di lingkungan padat." },
  { "en": "Fitur kunci Wi-Fi 6?", "id": "OFDMA, BSS Coloring, TWT." },
  { "en": "Apa itu 'BSS coloring'?", "id": "Mekanisme untuk mengurangi interferensi co-channel." },
  { "en": "Apa itu 'TWT (Target Wake Time)'?", "id": "Fitur hemat daya untuk perangkat IoT." },
  { "en": "Apa itu 'WPA-Enterprise'?", "id": "Menggunakan otentikasi 802.1X dan RADIUS." },
  { "en": "Apa itu 'WPA-Personal'?", "id": "Menggunakan Pre-Shared Key (PSK)." },
  { "en": "Apa itu 'KRACK attack'?", "id": "Serangan terhadap protokol keamanan WPA2." },
  { "en": "Apa itu 'path selection'?", "id": "Proses pemilihan rute terbaik oleh router." },
  { "en": "Apa itu 'longest match rule'?", "id": "Router memilih rute yang paling spesifik." },
  { "en": "Apa itu 'route redistribution'?", "id": "Berbagi rute antara protokol routing berbeda." },
  { "en": "Apa masalah 'route redistribution'?", "id": "Dapat menyebabkan loop dan inkonsistensi." },
  { "en": "Apa itu 'seed metric'?", "id": "Metrik awal saat melakukan redistribusi." },
  { "en": "Apa itu 'route map'?", "id": "Mekanisme untuk memfilter dan memodifikasi rute." },
  { "en": "Apa itu 'prefix list'?", "id": "Cara fleksibel untuk mencocokkan prefix jaringan." },
  { "en": "Apa itu 'distribute list'?", "id": "Memfilter rute menggunakan access list." },
  { "en": "Apa itu 'passive interface'?", "id": "Mencegah router mengirim update routing." },
  { "en": "Apa itu 'area border router' (ABR)?", "id": "Router OSPF yang menghubungkan area ke backbone." },
  { "en": "Apa itu 'ASBR (Autonomous System Boundary Router)'?", "id": "Router OSPF yang terhubung ke jaringan luar." },
  { "en": "Apa itu 'LSA Type 1'?", "id": "Router LSA, mendeskripsikan link sebuah router." },
  { "en": "Apa itu 'LSA Type 2'?", "id": "Network LSA, dibuat oleh Designated Router." },
  { "en": "Apa itu 'LSA Type 3'?", "id": "Summary LSA, dibuat oleh ABR." },
  { "en": "Apa itu 'LSA Type 5'?", "id": "External LSA, dibuat oleh ASBR." },
  { "en": "Apa itu 'OSPF neighborship states'?", "id": "Tahapan pembentukan hubungan tetangga OSPF." },
  { "en": "Apa itu '2-Way state'?", "id": "Kedua router saling melihat satu sama lain." },
  { "en": "Apa itu 'Full state'?", "id": "Database link-state sudah tersinkronisasi." },
  { "en": "Apa itu 'hello packet'?", "id": "Paket untuk menemukan dan menjaga hubungan." },
  { "en": "Apa itu 'dead interval'?", "id": "Waktu tunggu sebelum tetangga dianggap mati." },
  { "en": "Atribut BGP mana yang diperiksa pertama?", "id": "Atribut Weight (proprietary Cisco)." },
  { "en": "Apa itu 'BGP Weight'?", "id": "Atribut Cisco untuk mempengaruhi rute keluar." },
  { "en": "Apa itu 'Local Preference'?", "id": "Atribut untuk memilih rute keluar dari AS." },
  { "en": "Apa itu 'MED (Multi-Exit Discriminator)'?", "id": "Atribut untuk mempengaruhi rute masuk." },
  { "en": "Apa itu 'community' di BGP?", "id": "Tag untuk menandai sekelompok rute." },
  { "en": "Apa itu 'well-known mandatory' attribute?", "id": "Atribut BGP yang harus ada." },
  { "en": "Apa itu 'well-known discretionary' attribute?", "id": "Atribut BGP yang bisa ada atau tidak." },
  { "en": "Apa itu 'optional transitive' attribute?", "id": "Atribut BGP yang diteruskan ke peer lain." },
  { "en": "Apa itu 'optional non-transitive' attribute?", "id": "Atribut BGP yang tidak diteruskan." },
  { "en": "Apa itu 'route aggregation'?", "id": "Nama lain untuk route summarization." },
  { "en": "Apa itu 'black hole route'?", "id": "Rute yang membuang lalu lintas." },
  { "en": "Kapan 'black hole route' digunakan?", "id": "Untuk mitigasi serangan DDoS." },
  { "en": "Apa itu 'null interface'?", "id": "Interface virtual untuk membuang paket." },
  { "en": "Apa itu 'DNS amplification attack'?", "id": "Serangan DDoS menggunakan server DNS terbuka." },
  { "en": "Apa itu 'NTP amplification attack'?", "id": "Serangan DDoS menggunakan server NTP." },
  { "en": "Apa itu 'MAC address table'?", "id": "Tabel pada switch yang memetakan MAC ke port." },
  { "en": "Bagaimana switch mempelajari alamat MAC?", "id": "Dari alamat MAC sumber frame yang masuk." },
  { "en": "Apa itu 'aging time'?", "id": "Waktu sebelum entri di tabel MAC dihapus." },
  { "en": "Apa itu 'unicast flooding'?", "id": "Saat switch tidak tahu tujuan MAC address." },
  { "en": "Apa itu 'store-and-forward' switching?", "id": "Switch menerima seluruh frame sebelum meneruskan." },
  { "en": "Apa itu 'cut-through' switching?", "id": "Switch meneruskan frame setelah membaca alamat." },
  { "en": "Apa itu 'fragment-free' switching?", "id": "Varian cut-through, menunggu 64 byte pertama." },
  { "en": "Apa itu 'OSI model troubleshooting approach'?", "id": "Mengisolasi masalah berdasarkan lapisan." },
  { "en": "Apa itu pendekatan 'bottom-up'?", "id": "Troubleshooting dari Physical Layer ke atas." },
  { "en": "Apa itu pendekatan 'top-down'?", "id": "Troubleshooting dari Application Layer ke bawah." },
  { "en": "Apa itu pendekatan 'divide-and-conquer'?", "id": "Memulai dari tengah (misal, Network Layer)." },
  { "en": "Apa itu 'baseline' jaringan?", "id": "Dokumentasi performa jaringan dalam kondisi normal." },
  { "en": "Apa itu 'uptime'?", "id": "Waktu operasional sebuah sistem tanpa henti." },
  { "en": "Apa itu 'MTBF (Mean Time Between Failures)'?", "id": "Waktu rata-rata antar kegagalan." },
  { "en": "Apa itu 'MTTR (Mean Time To Repair)'?", "id": "Waktu rata-rata untuk memperbaiki kegagalan." },
  { "en": "Apa itu 'service-level agreement' (SLA)?", "id": "Kontrak layanan yang mendefinisikan level performa." },
  { "en": "Apa itu 'post-mortem'?", "id": "Analisis setelah terjadi insiden atau kegagalan." },
  { "en": "Apa itu 'root cause analysis' (RCA)?", "id": "Proses menemukan penyebab utama suatu masalah." },
  { "en": "Apa itu 'fallback mechanism'?", "id": "Mekanisme cadangan jika sistem utama gagal." },
  { "en": "Apa itu 'cold site'?", "id": "Situs pemulihan bencana tanpa peralatan." },
  { "en": "Apa itu 'hot site'?", "id": "Situs pemulihan bencana yang siap beroperasi." },
  { "en": "Apa itu 'warm site'?", "id": "Di antara cold dan hot site." },
  { "en": "Apa itu 'disaster recovery plan' (DRP)?", "id": "Rencana untuk memulihkan operasi setelah bencana." },
  { "en": "Apa itu 'business continuity plan' (BCP)?", "id": "Rencana untuk menjaga fungsi bisnis tetap berjalan." },
  { "en": "Apa itu 'single point of failure'?", "id": "Satu komponen yang kegagalannya melumpuhkan sistem." },
  { "en": "Apa itu 'fault tolerance'?", "id": "Kemampuan sistem tetap beroperasi meski ada gagal." },
  { "en": "Apa itu 'RAID'?", "id": "Redundant Array of Independent Disks." },
  { "en": "Apa itu 'RAID 0'?", "id": "Striping, untuk kecepatan, tanpa redundansi." },
  { "en": "Apa itu 'RAID 1'?", "id": "Mirroring, untuk redundansi." },
  { "en": "Apa itu 'RAID 5'?", "id": "Striping dengan paritas terdistribusi." },
  { "en": "Apa itu 'hot swapping'?", "id": "Mengganti komponen tanpa mematikan sistem." },
  { "en": "Apa itu 'power supply unit' (PSU) redundan?", "id": "Dua atau lebih PSU untuk keandalan." },
  { "en": "Apa itu 'Thicknet' (10BASE5)?", "id": "Standar Ethernet lama menggunakan kabel koaksial tebal." },
  { "en": "Apa itu 'Thinnet' (10BASE2)?", "id": "Standar Ethernet lama menggunakan kabel koaksial tipis." },
  { "en": "Apa itu 'vampire tap'?", "id": "Konektor yang menusuk kabel Thicknet." },
  { "en": "Apa itu 'BNC connector'?", "id": "Konektor yang digunakan pada Thinnet." },
  { "en": "Apa itu 'Token Ring'?", "id": "Teknologi LAN lama dari IBM." },
  { "en": "Bagaimana Token Ring menghindari tabrakan?", "id": "Menggunakan token untuk mengatur hak transmisi." },
  { "en": "Apa itu 'FDDI'?", "id": "Fiber Distributed Data Interface." },
  { "en": "Apa itu 'ATM (Asynchronous Transfer Mode)'?", "id": "Teknologi jaringan berbasis sel berukuran tetap." },
  { "en": "Berapa ukuran sel ATM?", "id": "53 byte." },
  { "en": "Apa itu 'Frame Relay'?", "id": "Teknologi WAN packet-switched." },
  { "en": "Apa itu 'X.25'?", "id": "Standar jaringan WAN packet-switched awal." },
  { "en": "Apa itu 'ISDN'?", "id": "Integrated Services Digital Network." },
  { "en": "Apa itu 'dial-up'?", "id": "Koneksi internet menggunakan saluran telepon." },
  { "en": "Apa itu 'serial communication'?", "id": "Mengirim data satu bit pada satu waktu." },
  { "en": "Contoh port serial?", "id": "RS-232." },
  { "en": "Apa itu 'baud rate'?", "id": "Jumlah perubahan sinyal per detik." },
  { "en": "Beda 'baud rate' dan 'bit rate'?", "id": "Tidak selalu sama, tergantung skema modulasi." },
  { "en": "Apa itu 'IEEE 1394'?", "id": "FireWire." },
  { "en": "Apa itu 'SCSI'?", "id": "Small Computer System Interface." },
  { "en": "Apa itu 'Global Unicast Address'?", "id": "Alamat IPv6 yang unik secara global." },
  { "en": "Apa itu 'Link-Local Address'?", "id": "Alamat IPv6 yang hanya valid di link lokal." },
  { "en": "Prefix untuk Link-Local Address?", "id": "FE80::/10." },
  { "en": "Apa itu 'Unique Local Address'?", "id": "Alamat IPv6 privat." },
  { "en": "Apa itu 'EUI-64'?", "id": "Metode membuat Interface ID dari alamat MAC." },
  { "en": "Apa itu 'solicited-node multicast address'?", "id": "Alamat multicast yang digunakan oleh NDP." },
  { "en": "Apa itu 'Neighbor Discovery Protocol' (NDP)?", "id": "Protokol di IPv6 pengganti ARP." },
  { "en": "Pesan NDP apa yang setara ARP request?", "id": "Neighbor Solicitation (NS)." },
  { "en": "Pesan NDP apa yang setara ARP reply?", "id": "Neighbor Advertisement (NA)." },
  { "en": "Apa itu 'Router Solicitation' (RS)?", "id": "Pesan dari host untuk menemukan router." },
  { "en": "Apa itu 'Router Advertisement' (RA)?", "id": "Pesan dari router untuk mengiklankan diri." },
  { "en": "Apa itu 'Duplicate Address Detection' (DAD)?", "id": "Proses mengecek apakah alamat IPv6 sudah dipakai." },
  { "en": "Apa itu 'TCP three-way handshake'?", "id": "Proses SYN, SYN-ACK, ACK untuk memulai koneksi." },
  { "en": "Apa itu 'TCP four-way handshake'?", "id": "Proses FIN, ACK, FIN, ACK untuk menutup koneksi." },
  { "en": "Apa itu 'graceful close'?", "id": "Menutup koneksi TCP dengan four-way handshake." },
  { "en": "Apa itu 'TCP reset' (RST)?", "id": "Menutup koneksi secara paksa dan segera." },
  { "en": "Apa itu 'TCP delayed acknowledgment'?", "id": "Menunda ACK untuk efisiensi." },
  { "en": "Apa itu 'selective acknowledgment' (SACK)?", "id": "Memberi tahu segmen spesifik yang hilang." },
  { "en": "Apa itu 'TCP fast retransmit'?", "id": "Mengirim ulang segmen setelah tiga duplicate ACK." },
  { "en": "Apa itu 'TCP fast recovery'?", "id": "Memulihkan koneksi dengan cepat setelah paket hilang." },
  { "en": "Apa itu 'TCP keepalive'?", "id": "Mengecek apakah koneksi yang diam masih aktif." },
  { "en": "Apa itu 'silly window syndrome'?", "id": "Masalah efisiensi akibat pengiriman data kecil." },
  { "en": "Bagaimana 'Nagle's algorithm' bekerja?", "id": "Menggabungkan data kecil sebelum dikirim." },
  { "en": "Apa itu 'path MTU discovery'?", "id": "Menemukan MTU terkecil di sepanjang jalur." },
  { "en": "Apa itu 'ICMP fragmentation needed'?", "id": "Pesan eror saat paket terlalu besar." },
  { "en": "Apa itu 'don't fragment' (DF) bit?", "id": "Flag di header IP untuk mencegah fragmentasi." },
  { "en": "Apa itu 'more fragments' (MF) bit?", "id": "Flag yang menandakan ada fragmen lain." },
  { "en": "Apa itu 'fragment offset'?", "id": "Menunjukkan posisi fragmen dalam paket asli." },
  { "en": "Apa itu 'NEXT (Near-End Crosstalk)'?", "id": "Interferensi di ujung kabel yang sama." },
  { "en": "Apa itu 'FEXT (Far-End Crosstalk)'?", "id": "Interferensi yang diukur di ujung seberang." },
  { "en": "Apa itu 'return loss'?", "id": "Ukuran sinyal yang dipantulkan kembali." },
  { "en": "Apa itu 'insertion loss'?", "id": "Pelemahan sinyal akibat penyisipan komponen." },
  { "en": "Apa itu 'propagation delay'?", "id": "Waktu yang dibutuhkan sinyal untuk merambat." },
  { "en": "Apa itu 'delay skew'?", "id": "Perbedaan propagation delay antar pasangan kawat." },
  { "en": "Apa itu 'wiring map'?", "id": "Tes untuk memastikan pin terhubung dengan benar." },
  { "en": "Apa itu 'split pair'?", "id": "Kesalahan pengkabelan di mana dua pasang tertukar." },
  { "en": "Apa itu 'TDR (Time-Domain Reflectometer)'?", "id": "Alat untuk menemukan lokasi kerusakan kabel." },
  { "en": "Apa itu 'OTDR (Optical TDR)'?", "id": "TDR untuk kabel serat optik." },
  { "en": "Apa itu 'bit error rate tester' (BERT)?", "id": "Alat untuk mengukur BER sebuah link." },
  { "en": "Apa itu 'cable certifier'?", "id": "Alat untuk menguji kepatuhan kabel terhadap standar." },
  { "en": "Apa itu 'tone generator and probe'?", "id": "Alat untuk melacak kabel di dinding." },
  { "en": "Apa itu 'punch-down tool'?", "id": "Alat untuk memasang kabel ke patch panel." },
  { "en": "Apa itu 'cable stripper'?", "id": "Alat untuk mengupas isolasi kabel." },
  { "en": "Apa itu 'crimper'?", "id": "Alat untuk memasang konektor ke kabel." },
  { "en": "Apa itu 'zero-trust security model'?", "id": "Model keamanan 'jangan percaya, selalu verifikasi'." },
  { "en": "Apa itu 'microsegmentation'?", "id": "Membagi jaringan menjadi segmen-segmen sangat kecil." },
  { "en": "Apa itu 'SASE (Secure Access Service Edge)'?", "id": "Arsitektur cloud untuk keamanan dan jaringan." },
  { "en": "Apa itu 'CASB (Cloud Access Security Broker)'?", "id": "Titik penegakan kebijakan keamanan cloud." },
  { "en": "Apa itu 'security orchestration' (SOAR)?", "id": "Mengotomatiskan respons terhadap insiden keamanan." },
  { "en": "Apa itu 'threat intelligence'?", "id": "Informasi tentang ancaman siber saat ini." },
  { "en": "Apa itu 'indicator of compromise' (IoC)?", "id": "Bukti forensik adanya intrusi." },
  { "en": "Apa itu 'security information and event management' (SIEM)?", "id": "Sistem untuk analisis log keamanan." },
  { "en": "Apa itu 'user and entity behavior analytics' (UEBA)?", "id": "Mendeteksi ancaman berdasarkan anomali perilaku." }


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
