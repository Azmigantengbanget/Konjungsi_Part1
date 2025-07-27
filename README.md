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


  { "en": "Konjungsi penambahan dalam kalimat?", "id": "Dan, serta, lagipula." },
  { "en": "Lengkapi: Dia datang ... pergi lagi.", "id": "Lalu." },
  { "en": "Konjungsi untuk menyatakan pilihan?", "id": "Atau." },
  { "en": "Lengkapi: Kamu mau teh ... kopi?", "id": "Atau." },
  { "en": "Konjungsi pertentangan paling umum?", "id": "Tetapi, namun, melainkan." },
  { "en": "Lengkapi: Rumahnya besar ... sempit.", "id": "Tetapi." },
  { "en": "Konjungsi yang menyatakan waktu?", "id": "Ketika, saat, sesudah, sebelum." },
  { "en": "Lengkapi: Aku tidur ... dia datang.", "id": "Ketika." },
  { "en": "Konjungsi untuk tujuan atau maksud?", "id": "Agar, supaya, untuk, guna." },
  { "en": "Lengkapi: Belajar ... menjadi pintar.", "id": "Agar." },
  { "en": "Konjungsi yang menyatakan sebab?", "id": "Sebab, karena, oleh karena." },
  { "en": "Lengkapi: Dia sakit ... kehujanan.", "id": "Karena." },
  { "en": "Konjungsi untuk akibat atau dampak?", "id": "Sehingga, sampai, akibatnya." },
  { "en": "Lengkapi: Dia malas ... dia gagal.", "id": "Sehingga." },
  { "en": "Konjungsi untuk syarat?", "id": "Jika, jikalau, asalkan, bila." },
  { "en": "Lengkapi: Aku ikut ... kamu janji.", "id": "Jika." },
  { "en": "Konjungsi tak bersyarat?", "id": "Walaupun, meskipun, biarpun." },
  { "en": "Lengkapi: ... sibuk, dia tetap membantu.", "id": "Meskipun." },
  { "en": "Konjungsi untuk perbandingan?", "id": "Seperti, bagai, laksana, ibarat." },
  { "en": "Lengkapi: Wajahnya bersinar ... rembulan.", "id": "Seperti." },
  { "en": "Konjungsi yang menguatkan (korelatif)?", "id": "Baik... maupun..., tidak hanya..." },
  { "en": "Lengkapi: ... Budi ... Ani suka apel.", "id": "Baik... maupun..." },
  { "en": "Konjungsi penjelas atau pemerinci?", "id": "Bahwa." },
  { "en": "Lengkapi: Dia sadar ... dia salah.", "id": "Bahwa." },
  { "en": "Konjungsi urutan waktu?", "id": "Lalu, kemudian, selanjutnya." },
  { "en": "Lengkapi: Dia makan, ... dia minum.", "id": "Kemudian." },
  { "en": "Konjungsi pembatas suatu hal?", "id": "Kecuali, selain." },
  { "en": "Lengkapi: Semua hadir ... Budi.", "id": "Kecuali." },
  { "en": "Konjungsi antar kalimat untuk kesimpulan?", "id": "Jadi, dengan demikian." },
  { "en": "Lengkapi: ... dapat disimpulkan begitu.", "id": "Jadi." },
  { "en": "Konjungsi pengandaian?", "id": "Andaikan, seandainya, sekiranya." },
  { "en": "Lengkapi: ... aku kaya, aku sedekah.", "id": "Seandainya." },
  { "en": "Lengkapi: Dia tidak makan ... minum.", "id": "Melainkan." },
  { "en": "Lengkapi: Aku akan pergi ... hujan reda.", "id": "Setelah." },
  { "en": "Konjungsi 'dan' termasuk jenis apa?", "id": "Konjungsi koordinatif (penambahan)." },
  { "en": "Konjungsi 'atau' termasuk jenis apa?", "id": "Konjungsi koordinatif (pemilihan)." },
  { "en": "Konjungsi 'tetapi' termasuk jenis apa?", "id": "Konjungsi koordinatif (perlawanan)." },
  { "en": "Konjungsi 'karena' termasuk jenis apa?", "id": "Konjungsi subordinatif (sebab)." },
  { "en": "Konjungsi 'jika' termasuk jenis apa?", "id": "Konjungsi subordinatif (syarat)." },
  { "en": "Konjungsi 'meskipun' termasuk jenis apa?", "id": "Konjungsi subordinatif (konsesif)." },
  { "en": "Lengkapi: ... hanya cantik, ... juga pintar.", "id": "Tidak hanya... tetapi juga..." },
  { "en": "Lengkapi: Jangankan uang, waktu ... tak ada.", "id": "Pun." },
  { "en": "Konjungsi 'lalu' menghubungkan apa?", "id": "Dua klausa setara." },
  { "en": "Konjungsi 'bahwa' memulai klausa apa?", "id": "Anak kalimat pengisi subjek/objek." },
  { "en": "Lengkapi: Aku di sini ... kau di sana.", "id": "Sedangkan." },
  { "en": "Konjungsi 'sedangkan' menyatakan apa?", "id": "Hubungan pertentangan dua hal." },
  { "en": "Lengkapi: Cuci tanganmu ... makan.", "id": "Sebelum." },
  { "en": "Konjungsi 'sebelum' menyatakan apa?", "id": "Keterangan waktu." },
  { "en": "Lengkapi: Dia belajar ... ujian.", "id": "Untuk." },
  { "en": "Konjungsi 'untuk' menyatakan apa?", "id": "Tujuan atau peruntukan." },
  { "en": "Lengkapi: Dia menangis ... sedih.", "id": "Sebab." },
  { "en": "Lengkapi: Dia begitu lelah ... tertidur.", "id": "Sampai." },
  { "en": "Konjungsi 'sampai' menyatakan apa?", "id": "Batasan atau akibat." },
  { "en": "Lengkapi: Dia diam ... orang lain ribut.", "id": "Padahal." },
  { "en": "Konjungsi 'padahal' menyatakan apa?", "id": "Pertentangan dengan kenyataan." },
  { "en": "Lengkapi: Aku menurut ... itu benar.", "id": "Asalkan." },
  { "en": "Konjungsi 'asalkan' menyatakan apa?", "id": "Syarat yang harus dipenuhi." },
  { "en": "Lengkapi: Seolah-olah dia tidak tahu apa-apa.", "id": "Seolah-olah." },
  { "en": "Konjungsi 'seolah-olah' menyatakan apa?", "id": "Perbandingan atau kemiripan." },
  { "en": "Lengkapi: ... hujan deras, kami berangkat.", "id": "Biarpun." },
  { "en": "Konjungsi 'biarpun' semakna dengan?", "id": "Meskipun, walaupun." },
  { "en": "Konjungsi antarkalimat untuk hal lain?", "id": "Selain itu, tambahan pula." },
  { "en": "Lengkapi: ... , ada hal lain.", "id": "Selain itu." },
  { "en": "Konjungsi antarkalimat menyatakan kebalikan?", "id": "Sebaliknya." },
  { "en": "Lengkapi: Dia rajin. ..., adiknya malas.", "id": "Sebaliknya." },
  { "en": "Konjungsi antarkalimat menyatakan akibat?", "id": "Oleh karena itu, akibatnya." },
  { "en": "Lengkapi: ... , dia dihukum.", "id": "Oleh karena itu." },
  { "en": "Konjungsi antarkalimat keadaan sebenarnya?", "id": "Sesungguhnya, bahwasanya." },
  { "en": "Lengkapi: ..., dia tidak bersalah.", "id": "Sesungguhnya." },
  { "en": "Lengkapi: Dia bekerja ... membiayai sekolah.", "id": "Guna." },
  { "en": "Konjungsi 'guna' menyatakan apa?", "id": "Tujuan atau manfaat." },
  { "en": "Lengkapi: Dia marah ... aku bohong.", "id": "Sebab." },
  { "en": "Lengkapi: Dia tertawa ... mendengar lelucon.", "id": "Setelah." },
  { "en": "Lengkapi: Kamu boleh main ... PR selesai.", "id": "Asal." },
  { "en": "Konjungsi 'asal' menyatakan apa?", "id": "Syarat." },
  { "en": "Lengkapi: Dia pura-pura tidur ... ditanya.", "id": "Supaya tidak." },
  { "en": "Lengkapi: Hujan turun, ... jalanan becek.", "id": "Akibatnya." },
  { "en": "Lengkapi: Dia bukan pemalas ... rajin.", "id": "Melainkan." },
  { "en": "Lengkapi: Entah benar ... salah.", "id": "Entah." },
  { "en": "Konjungsi 'entah' menyatakan apa?", "id": "Ketidakpastian atau pilihan." },
  { "en": "Lengkapi: Dia tetap diam ... disindir.", "id": "Walaupun." },
  { "en": "Lengkapi: Semakin belajar ... semakin bisa.", "id": "Semakin." },
  { "en": "Konjungsi 'semakin' menyatakan apa?", "id": "Hubungan gradasi (tingkatan)." },
  { "en": "Lengkapi: Ayah ... ibu pergi ke pasar.", "id": "Dan." },
  { "en": "Lengkapi: Kamu mau ... tidak?", "id": "Atau." },
  { "en": "Lengkapi: Dia kaya ... tidak sombong.", "id": "Tetapi." },
  { "en": "Lengkapi: Dia datang ... semua selesai.", "id": "Ketika." },
  { "en": "Lengkapi: Kita harus hemat ... boros.", "id": "Bukannya." },
  { "en": "Lengkapi: Dia berlari cepat ... kilat.", "id": "Bagai." },
  { "en": "Lengkapi: Dengan ini, rapat ... selesai.", "id": "Dinyatakan." },
  { "en": "Lengkapi: Dia terlambat, ... dia macet.", "id": "Oleh sebab itu." },
  { "en": "Lengkapi: Saya percaya ... dia jujur.", "id": "Bahwa." },
  { "en": "Lengkapi: Dia membaca buku ... menunggu.", "id": "Sambil." },
  { "en": "Konjungsi 'sambil' menyatakan apa?", "id": "Dua pekerjaan bersamaan." },
  { "en": "Lengkapi: Dia akan datang ... diundang.", "id": "Bila." },
  { "en": "Lengkapi: Dia diam seribu bahasa.", "id": "Seribu bahasa." },
  { "en": "Lengkapi: Dia tetap nekat ... dilarang.", "id": "Meskipun." },
  { "en": "Lengkapi: Dia belajar keras ... lulus.", "id": "Supaya." },
  { "en": "Lengkapi: Jangan pergi ... aku kembali.", "id": "Sebelum." },
  { "en": "Konjungsi 'yakni' berfungsi sebagai?", "id": "Penjelas atau perincian." },
  { "en": "Lengkapi: Dia membawa banyak barang, ... buku.", "id": "Yakni." },
  { "en": "Konjungsi 'yaitu' semakna dengan?", "id": "Yakni, ialah." },
  { "en": "Lengkapi: Ada 2 pahlawan, ... Diponegoro dan Pattimura.", "id": "Yaitu." },
  { "en": "Konjungsi 'malah' menyatakan apa?", "id": "Pertentangan yang menguatkan." },
  { "en": "Lengkapi: Bukannya diam, dia ... berteriak.", "id": "Malah." },
  { "en": "Konjungsi 'bahkan' menyatakan apa?", "id": "Penguatan atau penegasan." },
  { "en": "Lengkapi: Dia sangat kuat, ... bisa mengangkat mobil.", "id": "Bahkan." },
  { "en": "Konjungsi 'daripada' digunakan untuk?", "id": "Perbandingan." },
  { "en": "Lengkapi: Lebih baik belajar ... bermain.", "id": "Daripada." },
  { "en": "Konjungsi 'alih-alih' menyatakan apa?", "id": "Penggantian suatu tindakan." },
  { "en": "Lengkapi: ... tidur, ia memilih membaca.", "id": "Alih-alih." },
  { "en": "Konjungsi 'ibarat' menyatakan apa?", "id": "Perbandingan atau perumpamaan." },
  { "en": "Lengkapi: Mereka berdua ... pinang dibelah dua.", "id": "Ibarat." },
  { "en": "Konjungsi 'umpama' semakna dengan?", "id": "Seandainya, misal." },
  { "en": "Lengkapi: ... kamu jadi presiden, apa janjimu?", "id": "Umpama." },
  { "en": "Konjungsi 'sementara' menyatakan apa?", "id": "Waktu atau pertentangan." },
  { "en": "Lengkapi: Kakak menyapu, ... adik mengepel.", "id": "Sementara." },
  { "en": "Konjungsi 'seraya' menyatakan apa?", "id": "Dua pekerjaan bersamaan." },
  { "en": "Lengkapi: Dia bernyanyi ... menari.", "id": "Seraya." },
  { "en": "Konjungsi 'serta' semakna dengan?", "id": "Dan, bersama dengan." },
  { "en": "Lengkapi: Ayah, ibu, ... kakak pergi.", "id": "Serta." },
  { "en": "Konjungsi 'kendatipun' semakna dengan?", "id": "Meskipun, walaupun." },
  { "en": "Lengkapi: ... miskin, dia tetap dermawan.", "id": "Kendatipun." },
  { "en": "Konjungsi 'sekalipun' menyatakan apa?", "id": "Penguatan konsesif (tak bersyarat)." },
  { "en": "Lengkapi: Aku takkan menyerah, ... harus mati.", "id": "Sekalipun." },
  { "en": "Konjungsi 'biarpun' sering diikuti?", "id": "Begitu." },
  { "en": "Lengkapi: ... begitu, dia tetap tegar.", "id": "Biarpun." },
  { "en": "Konjungsi 'apalagi' menyatakan apa?", "id": "Penguatan atau penambahan." },
  { "en": "Lengkapi: Dia malas, ... jika disuruh.", "id": "Apalagi." },
  { "en": "Konjungsi 'sebab itu' termasuk jenis?", "id": "Konjungsi antarkalimat (akibat)." },
  { "en": "Lengkapi: Dia curang. ..., dia didiskualifikasi.", "id": "Sebab itu." },
  { "en": "Konjungsi 'meski begitu' menyatakan?", "id": "Pertentangan antarkalimat." },
  { "en": "Lengkapi: Soalnya sulit. ..., dia bisa.", "id": "Meski begitu." },
  { "en": "Lengkapi: Dia tidak hanya kaya, ... dermawan.", "id": "Tetapi juga." },
  { "en": "Lengkapi: Bukan saya yang mengambil, ... dia.", "id": "Melainkan." },
  { "en": "Lengkapi: Dia bekerja keras ... dapat bonus.", "id": "Supaya." },
  { "en": "Lengkapi: Saya akan datang ... tidak hujan.", "id": "Asalkan." },
  { "en": "Lengkapi: Kamu boleh pinjam, ... jangan rusak.", "id": "Asal." },
  { "en": "Lengkapi: Dia terus berjalan ... lelah.", "id": "Walau." },
  { "en": "Konjungsi 'walau' versi singkat dari?", "id": "Walaupun." },
  { "en": "Lengkapi: Dia mematung ... melihat hantu.", "id": "Seakan-akan." },
  { "en": "Konjungsi 'seakan-akan' menyatakan apa?", "id": "Pengandaian atau perbandingan." },
  { "en": "Lengkapi: Dia menjelaskan ... terperinci.", "id": "Secara." },
  { "en": "Lengkapi: Dia diam, ... dia tahu jawabannya.", "id": "Padahal." },
  { "en": "Lengkapi: Dia menangis ... bahagia.", "id": "Karena." },
  { "en": "Lengkapi: Dia terlalu kenyang ... tidak makan.", "id": "Sehingga." },
  { "en": "Lengkapi: Kerjakan ... kamu bisa.", "id": "Selagi." },
  { "en": "Konjungsi 'selagi' menyatakan apa?", "id": "Hubungan waktu (selama)." },
  { "en": "Lengkapi: Dia menurut ... ayahnya ada.", "id": "Selama." },
  { "en": "Konjungsi 'selama' menyatakan apa?", "id": "Durasi waktu atau syarat." },
  { "en": "Lengkapi: Dia pulang ... pekerjaan selesai.", "id": "Seusai." },
  { "en": "Konjungsi 'seusai' semakna dengan?", "id": "Setelah." },
  { "en": "Lengkapi: Dia menunggu ... bus datang.", "id": "Hingga." },
  { "en": "Konjungsi 'hingga' menyatakan apa?", "id": "Batasan waktu atau tempat." },
  { "en": "Lengkapi: Dia belajar dari pagi ... sore.", "id": "Sampai." },
  { "en": "Lengkapi: Jangan menebang pohon ... sembarangan.", "id": "Dengan." },
  { "en": "Lengkapi: Buku itu ada ... saya.", "id": "Pada." },
  { "en": "Lengkapi: Dia marah ... adiknya.", "id": "Kepada." },
  { "en": "Lengkapi: Hadiah ini ... kamu.", "id": "Untuk." },
  { "en": "Lengkapi: ... demikian, acara kita selesai.", "id": "Dengan." },
  { "en": "Konjungsi 'namun' sering di awal kalimat?", "id": "Ya, sebagai konjungsi antarkalimat." },
  { "en": "Lengkapi: Dia gagal. ..., dia tak menyerah.", "id": "Namun." },
  { "en": "Lengkapi: Dia membeli sayur ... buah.", "id": "Dan." },
  { "en": "Lengkapi: Dia tidak suka teh, ... kopi.", "id": "Melainkan." },
  { "en": "Lengkapi: ... hari sudah malam, dia pulang.", "id": "Karena." },
  { "en": "Lengkapi: Dia akan berhasil ... berusaha.", "id": "Jikalau." },
  { "en": "Lengkapi: Dia tetap tersenyum ... sedih.", "id": "Meskipun." },
  { "en": "Lengkapi: Suaranya merdu ... buluh perindu.", "id": "Laksana." },
  { "en": "Lengkapi: Baik pria ... wanita hadir.", "id": "Maupun." },
  { "en": "Lengkapi: Aku berpikir ... dia jujur.", "id": "Bahwa." },
  { "en": "Lengkapi: Dia sarapan, ... berangkat kerja.", "id": "Lalu." },
  { "en": "Lengkapi: Semua buah aku suka, ... durian.", "id": "Kecuali." },
  { "en": "Lengkapi: ... , kita harus tetap optimis.", "id": "Jadi." },
  { "en": "Lengkapi: ... aku punya sayap, aku terbang.", "id": "Andaikan." },
  { "en": "Lengkapi: Dia tidak membaca ... menulis.", "id": "Ataupun." },
  { "en": "Lengkapi: Dia akan pergi ... diizinkan.", "id": "Bila." },
  { "en": "Konjungsi 'bila' semakna dengan?", "id": "Jika, kalau." },
  { "en": "Lengkapi: Dia berlatih agar ... menang.", "id": "Bisa." },
  { "en": "Lengkapi: Api membesar ... membakar rumah.", "id": "Hingga." },
  { "en": "Lengkapi: Jangankan seribu, seratus ... tak ada.", "id": "Pun." },
  { "en": "Konjungsi 'pun' menyatakan apa?", "id": "Penegasan atau penguatan." },
  { "en": "Lengkapi: ... dia tidak datang, kita tunggu.", "id": "Apabila." },
  { "en": "Konjungsi 'apabila' menyatakan apa?", "id": "Syarat atau pengandaian." },
  { "en": "Lengkapi: Dia bukan hanya malas, ... bodoh.", "id": "Melainkan juga." },
  { "en": "Lengkapi: Dia belajar ... ayahnya mengajar.", "id": "Sambil." },
  { "en": "Lengkapi: Dengan ini, saya ... terima.", "id": "Nyatakan." },
  { "en": "Lengkapi: Dia berjanji, ... dia ingkar.", "id": "Tetapi." },
  { "en": "Lengkapi: Saya suka sate ... gado-gado.", "id": "Serta." },
  { "en": "Lengkapi: Dia bekerja ... fajar menyingsing.", "id": "Sejak." },
  { "en": "Konjungsi 'sejak' menyatakan apa?", "id": "Permulaan waktu." },
  { "en": "Lengkapi: Dia terdiam ... mendengar berita itu.", "id": "Seketika." },
  { "en": "Lengkapi: Oleh ... itu, kita harus waspada.", "id": "Sebab." },
  { "en": "Lengkapi: Kamu harus pilih: aku ... dia?", "id": "Atau." },
  { "en": "Lengkapi: Dia kelelahan, ... dia istirahat.", "id": "Kemudian." },
  { "en": "Lengkapi: Dia gembira ... mendapat hadiah.", "id": "Sebab." },
  { "en": "Lengkapi: Air meluap ... hujan deras.", "id": "Akibat." },
  { "en": "Lengkapi: Dia akan memaafkanmu ... kamu jujur.", "id": "Asalkan." },
  { "en": "Fungsi konjungsi 'bak'?", "id": "Menyatakan perbandingan atau kemiripan." },
  { "en": "Lengkapi: Gaya bicaranya ... seorang orator.", "id": "Bak." },
  { "en": "Fungsi konjungsi 'manakala'?", "id": "Menyatakan waktu (sama dengan ketika)." },
  { "en": "Lengkapi: ... bel pulang berbunyi, siswa bersorak.", "id": "Manakala." },
  { "en": "Fungsi konjungsi 'padahal'?", "id": "Menyatakan pertentangan dengan fakta." },
  { "en": "Lengkapi: Dia mengaku miskin, ... punya mobil.", "id": "Padahal." },
  { "en": "Konjungsi apa yang memulai klausa tujuan?", "id": "Agar, supaya, biar, untuk." },
  { "en": "Lengkapi: Dia menabung ... bisa membeli rumah.", "id": "Supaya." },
  { "en": "Konjungsi apa yang menyatakan akibat?", "id": "Sehingga, sampai, akibatnya." },
  { "en": "Lengkapi: Dia belajar giat ... lulus ujian.", "id": "Sehingga." },
  { "en": "Selesaikan: Bukannya belajar, dia ... bermain.", "id": "Malah." },
  { "en": "Fungsi konjungsi 'malah'?", "id": "Menyatakan kebalikan yang tak terduga." },
  { "en": "Selesaikan: Dia tidak hanya malas, ... curang.", "id": "Bahkan." },
  { "en": "Fungsi konjungsi 'bahkan'?", "id": "Memberi penekanan atau penguatan." },
  { "en": "Selesaikan: Jangankan mobil, sepeda ... tak punya.", "id": "Pun." },
  { "en": "Fungsi partikel 'pun' dalam kalimat?", "id": "Penegas atau penguat." },
  { "en": "Selesaikan: Kamu boleh pergi ... izin dulu.", "id": "Asalkan." },
  { "en": "Beda 'asal' dan 'asalkan'?", "id": "Hampir sama, 'asalkan' lebih formal." },
  { "en": "Konjungsi untuk memulai contoh?", "id": "Misalnya, contohnya." },
  { "en": "Lengkapi: Hewan karnivora, ... singa dan harimau.", "id": "Contohnya." },
  { "en": "Selesaikan: Dia diam ... menyembunyikan sesuatu.", "id": "Seakan-akan." },
  { "en": "Fungsi konjungsi 'seakan-akan'?", "id": "Menyatakan perumpamaan atau pengandaian." },
  { "en": "Selesaikan: Dia datang ... semua sudah pergi.", "id": "Setelah." },
  { "en": "Selesaikan: Kunci pintunya ... kamu pergi.", "id": "Sebelum." },
  { "en": "Konjungsi 'lalu' dan 'kemudian'?", "id": "Menyatakan urutan kejadian." },
  { "en": "Selesaikan: Dia makan, ... langsung tidur.", "id": "Lalu." },
  { "en": "Selesaikan: Dia tidak suka ikan ... daging.", "id": "Maupun." },
  { "en": "Pasangan 'baik' dalam konjungsi korelatif?", "id": "Maupun." },
  { "en": "Pasangan 'tidak hanya'?", "id": "Tetapi juga." },
  { "en": "Selesaikan: Dia ... tampan, ... juga baik hati.", "id": "Tidak hanya... tetapi juga..." },
  { "en": "Konjungsi untuk dua kegiatan bersamaan?", "id": "Sambil, seraya." },
  { "en": "Selesaikan: Dia membaca koran ... minum kopi.", "id": "Sambil." },
  { "en": "Selesaikan: ... kaya, ia sangat sombong.", "id": "Karena." },
  { "en": "Selesaikan: ... kamu tidak datang, aku kecewa.", "id": "Jika." },
  { "en": "Selesaikan: Dia tetap tersenyum ... hatinya sedih.", "id": "Meskipun." },
  { "en": "Selesaikan: Dia bukan marah, ... kecewa.", "id": "Melainkan." },
  { "en": "Fungsi konjungsi 'melainkan'?", "id": "Menyatakan pertentangan korektif." },
  { "en": "Selesaikan: Pilihlah satu: apel ... jeruk?", "id": "Atau." },
  { "en": "Konjungsi penambahan selain 'dan'?", "id": "Serta, lagipula." },
  { "en": "Selesaikan: Ibu, ayah, ... adik pergi.", "id": "Serta." },
  { "en": "Konjungsi 'sebaliknya' menghubungkan apa?", "id": "Antarkalimat yang berlawanan." },
  { "en": "Selesaikan: Kakak rajin. ..., adiknya pemalas.", "id": "Sebaliknya." },
  { "en": "Selesaikan: Dia gagal. ..., dia tidak putus asa.", "id": "Namun." },
  { "en": "Fungsi konjungsi 'namun'?", "id": "Menyatakan pertentangan antarkalimat." },
  { "en": "Konjungsi apa untuk menyimpulkan?", "id": "Jadi, dengan demikian." },
  { "en": "Selesaikan: ..., kita harus selalu waspada.", "id": "Jadi." },
  { "en": "Konjungsi 'yakni' memulai perincian?", "id": "Ya, benar." },
  { "en": "Lengkapi: Tiga warna primer, ... merah, kuning, biru.", "id": "Yakni." },
  { "en": "Konjungsi 'yaitu' sinonim dari?", "id": "Yakni, ialah." },
  { "en": "Selesaikan: Dia bekerja keras ... keluarganya.", "id": "Demi." },
  { "en": "Konjungsi 'demi' menyatakan apa?", "id": "Tujuan atau kepentingan." },
  { "en": "Selesaikan: Dia menunggu ... malam tiba.", "id": "Hingga." },
  { "en": "Konjungsi 'hingga' menyatakan batas apa?", "id": "Batas waktu atau tingkatan." },
  { "en": "Selesaikan: Dia belajar ... ujian besok.", "id": "Untuk." },
  { "en": "Konjungsi 'untuk' menyatakan apa?", "id": "Tujuan atau peruntukan." },
  { "en": "Selesaikan: Dia terlambat ... bangun kesiangan.", "id": "Sebab." },
  { "en": "Selesaikan: Dia berlari secepat mungkin ... menang.", "id": "Agar." },
  { "en": "Selesaikan: Dia tidak akan pergi ... dijemput.", "id": "Kecuali." },
  { "en": "Konjungsi 'kecuali' menyatakan apa?", "id": "Pengecualian atau pembatasan." },
  { "en": "Selesaikan: Dia mirip ayahnya, ... dari cara bicaranya.", "id": "Terutama." },
  { "en": "Selesaikan: Dia bekerja ... matahari terbenam.", "id": "Sampai." },
  { "en": "Konjungsi 'dan' menghubungkan klausa setara?", "id": "Ya, benar." },
  { "en": "Selesaikan: Aku menyapu ... dia mengepel.", "id": "Dan." },
  { "en": "Selesaikan: Dia tidak mau makan ... minum.", "id": "Ataupun." },
  { "en": "Selesaikan: ... dia datang, masalah selesai.", "id": "Ketika." },
  { "en": "Selesaikan: ... hujan, pertandingan tetap berjalan.", "id": "Walaupun." },
  { "en": "Selesaikan: Dia memotong kue ... pisau.", "id": "Dengan." },
  { "en": "Fungsi konjungsi 'dengan'?", "id": "Menyatakan cara, alat, kesertaan." },
  { "en": "Selesaikan: ... kamu berusaha, pasti bisa.", "id": "Selama." },
  { "en": "Fungsi konjungsi 'selama'?", "id": "Menyatakan durasi atau syarat." },
  { "en": "Selesaikan: Aku tahu ... dia berbohong.", "id": "Bahwa." },
  { "en": "Fungsi konjungsi 'bahwa'?", "id": "Memulai anak kalimat penjelas." },
  { "en": "Konjungsi korelatif untuk perbandingan?", "id": "Seperti...demikian pula." },
  { "en": "Selesaikan: Daripada diam, ... kita bertanya.", "id": "Lebih baik." },
  { "en": "Selesaikan: Dia berbicara ... tak ada jeda.", "id": "Sehingga." },
  { "en": "Selesaikan: Aku akan ikut ... kamu traktir.", "id": "Kalau." },
  { "en": "Konjungsi 'kalau' semakna dengan?", "id": "Jika, bila." },
  { "en": "Selesaikan: Dia tidak bersalah, ... difitnah.", "id": "Melainkan." },
  { "en": "Selesaikan: Dia tetap ramah ... dihina.", "id": "Sekalipun." },
  { "en": "Fungsi konjungsi 'sekalipun'?", "id": "Menyatakan hal tak bersyarat." },
  { "en": "Selesaikan: Dia pandai dalam ... hal akademis.", "id": "Segala." },
  { "en": "Selesaikan: ..., dia memutuskan untuk pergi.", "id": "Akhirnya." },
  { "en": "Konjungsi 'akhirnya' menyatakan apa?", "id": "Kesimpulan atau akhir cerita." },
  { "en": "Selesaikan: Entah setuju ... tidak, dia pasrah.", "id": "Atau." },
  { "en": "Fungsi konjungsi 'lantas'?", "id": "Menyatakan kelanjutan (sinonim lalu)." },
  { "en": "Selesaikan: Dia terjatuh, ... menangis.", "id": "Lantas." },
  { "en": "Fungsi konjungsi 'biarpun'?", "id": "Menyatakan hubungan konsesif (pertentangan)." },
  { "en": "Selesaikan: ... panas, dia tetap memakai jaket.", "id": "Biarpun." },
  { "en": "Konjungsi penjelas selain 'bahwa'?", "id": "Yakni, yaitu, ialah." },
  { "en": "Selesaikan: Dia juara kelas, ... anak terpintar.", "id": "Yakni." },
  { "en": "Lengkapi: Dia belajar ... pagi hingga malam.", "id": "Dari." },
  { "en": "Fungsi konjungsi 'dari'?", "id": "Menyatakan asal atau permulaan." },
  { "en": "Lengkapi: ... begitu, jangan menyerah.", "id": "Meskipun." },
  { "en": "Lengkapi: Baik kaya ... miskin sama di mata hukum.", "id": "Maupun." },
  { "en": "Lengkapi: Dia tidak hanya diam, ... ikut membantu.", "id": "Tetapi juga." },
  { "en": "Konjungsi 'adapun' berfungsi untuk?", "id": "Memulai paragraf atau gagasan baru." },
  { "en": "Lengkapi: ..., mengenai biaya, kita bicarakan nanti.", "id": "Adapun." },
  { "en": "Konjungsi 'alkisah' memulai apa?", "id": "Cerita lama atau hikayat." },
  { "en": "Lengkapi: ..., tersebutlah seorang putri cantik.", "id": "Alkisah." },
  { "en": "Konjungsi 'syahdan' semakna dengan?", "id": "Kemudian, lalu (dalam sastra lama)." },
  { "en": "Lengkapi: ..., maka raja pun murka.", "id": "Syahdan." },
  { "en": "Konjungsi 'maka dari itu' menyatakan?", "id": "Kesimpulan atau akibat." },
  { "en": "Lengkapi: Dia malas. ..., nilainya jelek.", "id": "Maka dari itu." },
  { "en": "Konjungsi 'oleh sebab itu'?", "id": "Sama dengan 'oleh karena itu'." },
  { "en": "Lengkapi: Dia begadang, ... dia mengantuk.", "id": "Oleh sebab itu." },
  { "en": "Selesaikan: Dia bukan hanya pintar, ... rajin.", "id": "Melainkan juga." },
  { "en": "Fungsi konjungsi 'melainkan juga'?", "id": "Menambahkan penguatan positif." },
  { "en": "Selesaikan: Sedemikian rupa cantiknya ... bidadari.", "id": "Seperti." },
  { "en": "Fungsi 'sedemikian rupa'?", "id": "Menekankan tingkat atau cara." },
  { "en": "Konjungsi untuk urutan selain 'lalu'?", "id": "Kemudian, selanjutnya, setelah itu." },
  { "en": "Lengkapi: Rebus mi, ... tiriskan.", "id": "Kemudian." },
  { "en": "Konjungsi 'tatkala' lebih sering di?", "id": "Karya sastra atau tulisan formal." },
  { "en": "Lengkapi: ... senja tiba, suasana hening.", "id": "Tatkala." },
  { "en": "Selesaikan: Dia tetap bekerja ... sakit.", "id": "Meskipun." },
  { "en": "Selesaikan: Aku memaafkanmu ... kamu tidak ulangi.", "id": "Asalkan." },
  { "en": "Konjungsi apa untuk pilihan eksklusif?", "id": "Atau." },
  { "en": "Lengkapi: Kamu pilih dia ... aku?", "id": "Atau." },
  { "en": "Konjungsi penambahan selain 'dan'?", "id": "Serta, lagi pula." },
  { "en": "Lengkapi: Dia membawa buku, pulpen, ... penggaris.", "id": "Serta." },
  { "en": "Selesaikan: Dia gagal, ... dia mencoba lagi.", "id": "Tetapi." },
  { "en": "Selesaikan: Dia tahu itu salah, ... tetap melakukannya.", "id": "Namun." },
  { "en": "Beda 'tetapi' dan 'namun'?", "id": "'Namun' lebih kuat, sering antarkalimat." },
  { "en": "Selesaikan: Dia tidak mau ... disuruh ibunya.", "id": "Walaupun." },
  { "en": "Selesaikan: Dia menangis ... menonton film sedih.", "id": "Karena." },
  { "en": "Selesaikan: Dia rajin menabung ... bisa naik haji.", "id": "Agar." },
  { "en": "Selesaikan: Dia terdiam ... mendengar kabar itu.", "id": "Seketika." },
  { "en": "Selesaikan: Dia bekerja ... fajar hingga senja.", "id": "Dari." },
  { "en": "Konjungsi 'dari...hingga' menyatakan?", "id": "Rentang waktu atau tempat." },
  { "en": "Selesaikan: Dia membaca ... menunggu kereta.", "id": "Sambil." },
  { "en": "Selesaikan: Hujan turun sangat deras ... banjir.", "id": "Sehingga." },
  { "en": "Selesaikan: Dia akan lulus ... dia rajin.", "id": "Jika." },
  { "en": "Konjungsi 'kalau' bisa menyatakan apa?", "id": "Syarat atau waktu (di masa lampau)." },
  { "en": "Lengkapi: ... dulu, dia tidak begitu.", "id": "Kalau." },
  { "en": "Selesaikan: Dia bukan pencurinya, ... saksinya.", "id": "Melainkan." },
  { "en": "Selesaikan: Suaranya indah ... suara seruling.", "id": "Bagaikan." },
  { "en": "Konjungsi 'bagaikan' menyatakan apa?", "id": "Perumpamaan atau perbandingan." },
  { "en": "Lengkapi: ... lebih baik kita diam.", "id": "Mungkin." },
  { "en": "Selesaikan: Dia pasti datang, ... sedikit terlambat.", "id": "Meskipun." },
  { "en": "Selesaikan: Semua orang setuju, ... dia menolak.", "id": "Kecuali." },
  { "en": "Selesaikan: Dia menjelaskan ... sangat detail.", "id": "Secara." },
  { "en": "Selesaikan: Dia kaya, ... dia tidak bahagia.", "id": "Akan tetapi." },
  { "en": "Fungsi 'akan tetapi'?", "id": "Menyatakan pertentangan yang kuat." },
  { "en": "Selesaikan: Dia memaafkannya, ... tidak melupakannya.", "id": "Namun." },
  { "en": "Konjungsi penjelas untuk daftar?", "id": "Yaitu, yakni." },
  { "en": "Lengkapi: Bahan-bahannya, ...: tepung, gula, telur.", "id": "Yaitu." },
  { "en": "Selesaikan: Dia tidak makan apa pun ... pagi.", "id": "Sejak." },
  { "en": "Konjungsi 'sejak' menyatakan apa?", "id": "Titik awal waktu." },
  { "en": "Selesaikan: Dia bekerja ... lelah.", "id": "Tanpa." },
  { "en": "Selesaikan: Dia akan membantumu ... kamu sopan.", "id": "Asal." },
  { "en": "Selesaikan: Jangan hanya bicara, ... bertindaklah.", "id": "Tetapi." },
  { "en": "Selesaikan: Dia berteriak ... memanggil nama temannya.", "id": "Sambil." },
  { "en": "Selesaikan: Dia begitu terkejut ... tidak bisa bicara.", "id": "Sampai." },
  { "en": "Konjungsi 'sampai' bisa menyatakan apa?", "id": "Akibat atau batas akhir." },
  { "en": "Selesaikan: ... kamu tahu, dia sudah pindah.", "id": "Asal." },
  { "en": "Selesaikan: Dia tidak ingin pergi, ... terpaksa.", "id": "Namun." },
  { "en": "Selesaikan: Dia datang tepat ... acara dimulai.", "id": "Ketika." },
  { "en": "Selesaikan: Dia akan tetap pergi ... dilarang.", "id": "Sekalipun." },
  { "en": "Fungsi 'sekalipun' dalam kalimat?", "id": "Menyatakan penguatan hal tak bersyarat." },
  { "en": "Selesaikan: Entah besok ... lusa, dia pasti datang.", "id": "Atau." },
  { "en": "Selesaikan: Dia tidak hanya kaya, ... juga rendah hati.", "id": "Tetapi." },
  { "en": "Selesaikan: Saya akan menunggu ... dia kembali.", "id": "Sampai." },
  { "en": "Selesaikan: Dia menangis ... tahu kebenarannya.", "id": "Setelah." },
  { "en": "Selesaikan: Dia bersembunyi ... tidak terlihat.", "id": "Supaya." },
  { "en": "Selesaikan: Rumah itu besar ... tidak terawat.", "id": "Tetapi." },
  { "en": "Selesaikan: Dia bicara terus ... tidak ada yang dengar.", "id": "Padahal." },
  { "en": "Selesaikan: Dia sangat lelah, ... dia tidur pulas.", "id": "Alhasil." },
  { "en": "Fungsi konjungsi 'alhasil'?", "id": "Menyatakan hasil akhir cerita." },
  { "en": "Selesaikan: Dia tidak punya uang, ... teman.", "id": "Apalagi." },
  { "en": "Selesaikan: ... tidak setuju, katakan saja.", "id": "Jika." },
  { "en": "Selesaikan: Dia bekerja ... mendapat upah.", "id": "Untuk." },
  { "en": "Selesaikan: Dia tidak menjawab, ... hanya tersenyum.", "id": "Melainkan." },
  { "en": "Selesaikan: ... sering gagal, dia tidak menyerah.", "id": "Walaupun." },
  { "en": "Selesaikan: Dia diam ... menyimpan sebuah rahasia.", "id": "Seolah-olah." },
  { "en": "Selesaikan: Dia membeli baju ... celana.", "id": "Dan." },
  { "en": "Selesaikan: Dia tidak akan berhasil ... bantuanmu.", "id": "Tanpa." },
  { "en": "Selesaikan: ... kita berusaha, pasti ada jalan.", "id": "Selama." },
  { "en": "Selesaikan: Dia memotong kertas ... gunting.", "id": "Dengan." },
  { "en": "Selesaikan: Dia akan ikut ... pestanya malam hari.", "id": "Asalkan." },
  { "en": "Selesaikan: Dia tetap tenang ... situasi genting.", "id": "Meskipun." },
  { "en": "Selesaikan: Aku tidak percaya ... dia berkata jujur.", "id": "Bahwa." },
  { "en": "Selesaikan: Dia pulang ... ayahnya menelepon.", "id": "Setelah." },
  { "en": "Selesaikan: Dia melukis ... mendengarkan musik.", "id": "Sambil." },
  { "en": "Selesaikan: Dia memarahi adiknya ... tidak sengaja.", "id": "Padahal." },
  { "en": "Selesaikan: Baik siswa ... siswi wajib ikut.", "id": "Maupun." },
  { "en": "Selesaikan: Dia tidak hanya datang, ... membawa hadiah.", "id": "Tetapi juga." },
  { "en": "Selesaikan: Dia akan berlibur ... pekerjaannya selesai.", "id": "Apabila." },
  { "en": "Fungsi konjungsi 'apabila'?", "id": "Menyatakan syarat (sama dengan jika)." },
  { "en": "Selesaikan: Dia tidak makan nasi, ... makan bubur.", "id": "Melainkan." },
  { "en": "Selesaikan: Dia memaafkan kesalahan temannya ... tulus.", "id": "Dengan." },
  { "en": "Konjungsi 'bukannya... melainkan' menyatakan?", "id": "Hubungan pertentangan korektif." },
  { "en": "Lengkapi: Dia ... marah, ... hanya kecewa.", "id": "Bukannya... melainkan..." },
  { "en": "Konjungsi 'demikian... sehingga' menyatakan?", "id": "Hubungan akibat yang ditekankan." },
  { "en": "Lengkapi: ... hebatnya, ... semua terdiam.", "id": "Demikian... sehingga..." },
  { "en": "Konjungsi 'kian... kian' menyatakan?", "id": "Hubungan gradasi atau tingkatan." },
  { "en": "Lengkapi: ... hari ... malam, hujan makin deras.", "id": "Kian... kian..." },
  { "en": "Konjungsi 'biarpun' termasuk jenis apa?", "id": "Konjungsi subordinatif konsesif." },
  { "en": "Lengkapi: Dia tetap berangkat ... dilarang.", "id": "Biarpun." },
  { "en": "Benarkah 'sedangkan' konjungsi antarkalimat?", "id": "Salah, itu konjungsi intrakalimat." },
  { "en": "Lengkapi: Ayah membaca koran, ... ibu memasak.", "id": "Sedangkan." },
  { "en": "Benarkah 'oleh karena itu' konjungsi intrakalimat?", "id": "Salah, itu konjungsi antarkalimat." },
  { "en": "Lengkapi: Dia terlambat. ..., dia ketinggalan kereta.", "id": "Oleh karena itu." },
  { "en": "Sebutkan 2 konjungsi tujuan.", "id": "Agar, supaya." },
  { "en": "Sebutkan 2 konjungsi sebab.", "id": "Karena, sebab." },
  { "en": "Sebutkan 2 konjungsi syarat.", "id": "Jika, asalkan." },
  { "en": "Sebutkan 2 konjungsi waktu.", "id": "Ketika, setelah." },
  { "en": "Sebutkan 2 konjungsi pertentangan.", "id": "Tetapi, namun." },
  { "en": "Fungsi konjungsi 'lantas'?", "id": "Menyatakan kelanjutan (sinonim lalu)." },
  { "en": "Lengkapi: Dia melihat kecelakaan, ... menolongnya.", "id": "Lantas." },
  { "en": "Konjungsi 'seusai' semakna dengan?", "id": "Setelah, sehabis." },
  { "en": "Lengkapi: Mereka berdoa ... makan.", "id": "Seusai." },
  { "en": "Fungsi konjungsi 'guna'?", "id": "Menyatakan tujuan atau manfaat." },
  { "en": "Lengkapi: Menabunglah ... masa depan.", "id": "Guna." },
  { "en": "Konjungsi 'selain' menyatakan apa?", "id": "Pengecualian atau penambahan." },
  { "en": "Lengkapi: ... cantik, dia juga pintar.", "id": "Selain." },
  { "en": "Konjungsi 'daripada' biasa digunakan untuk?", "id": "Membandingkan dua hal." },
  { "en": "Lengkapi: Saya lebih suka teh ... kopi.", "id": "Daripada." },
  { "en": "Fungsi konjungsi 'semenjak'?", "id": "Menandai titik awal waktu." },
  { "en": "Lengkapi: Dia menjadi pendiam ... ayahnya wafat.", "id": "Semenjak." },
  { "en": "Fungsi konjungsi 'sewaktu-waktu'?", "id": "Menyatakan waktu yang tidak tentu." },
  { "en": "Lengkapi: Bawalah payung, ... hujan turun.", "id": "Sewaktu-waktu." },
  { "en": "Lengkapi: Dia ... diam ... berkata.", "id": "Antara." },
  { "en": "Selesaikan: Dia tidak berkata sepatah kata ...", "id": "Pun." },
  { "en": "Selesaikan: Apa ... yang kamu lakukan?", "id": "Gerangan." },
  { "en": "Fungsi konjungsi 'akibatnya'?", "id": "Menghubungkan sebab dengan akibatnya." },
  { "en": "Lengkapi: Dia boros. ..., dia kehabisan uang.", "id": "Akibatnya." },
  { "en": "Fungsi konjungsi 'akhirulkalam'?", "id": "Menutup pidato atau tulisan." },
  { "en": "Lengkapi: ..., wassalamualaikum warahmatullahi wabarakatuh.", "id": "Akhirulkalam." },
  { "en": "Lengkapi: Dia bukan temanku, ... musuhku.", "id": "Melainkan." },
  { "en": "Lengkapi: ... dia tidak datang, acara tetap jalan.", "id": "Meskipun." },
  { "en": "Lengkapi: Aku akan pergi ... kauizinkan.", "id": "Jika." },
  { "en": "Lengkapi: Dia belajar keras ... menjadi juara.", "id": "Agar." },
  { "en": "Lengkapi: Dia datang ... semua sudah selesai.", "id": "Ketika." },
  { "en": "Lengkapi: Pertama, siapkan alat. ..., siapkan bahan.", "id": "Kemudian." },
  { "en": "Lengkapi: Dia tidak hanya kaya, ... juga dermawan.", "id": "Tetapi juga." },
  { "en": "Lengkapi: Kamu boleh bermain ... sudah belajar.", "id": "Setelah." },
  { "en": "Lengkapi: Dia menunggu dari pagi ... sore.", "id": "Sampai." },
  { "en": "Lengkapi: Dia diam saja ... dihina orang.", "id": "Walaupun." },
  { "en": "Lengkapi: Dia berlatih setiap hari ... menang.", "id": "Supaya." },
  { "en": "Lengkapi: Dia berbicara ... tidak ada yang mendengarkan.", "id": "Padahal." },
  { "en": "Lengkapi: Dia kaya raya, ... hidupnya kesepian.", "id": "Akan tetapi." },
  { "en": "Lengkapi: Dia memotong sayur ... pisau tajam.", "id": "Dengan." },
  { "en": "Lengkapi: Saya percaya ... kamu bisa melakukannya.", "id": "Bahwa." },
  { "en": "Lengkapi: Dia tidak makan nasi, ... hanya makan buah.", "id": "Melainkan." },
  { "en": "Lengkapi: Dia membaca buku ... menunggu temannya.", "id": "Sambil." },
  { "en": "Lengkapi: Baik laki-laki ... perempuan boleh ikut.", "id": "Maupun." },
  { "en": "Lengkapi: Aku akan membantumu ... tidak merepotkan.", "id": "Asalkan." },
  { "en": "Lengkapi: Api itu padam ... disiram air.", "id": "Setelah." },
  { "en": "Lengkapi: Jangan keluar ... hujan reda.", "id": "Sebelum." },
  { "en": "Lengkapi: Dia begitu lelah ... langsung tertidur.", "id": "Sehingga." },
  { "en": "Lengkapi: ... kamu jujur, semua akan baik.", "id": "Selama." },
  { "en": "Lengkapi: Dia menangis tersedu-sedu ... menonton film.", "id": "Sebab." },
  { "en": "Lengkapi: Jangan berbicara ... makan.", "id": "Ketika." },
  { "en": "Lengkapi: Dia akan memaafkanmu ... kamu meminta maaf.", "id": "Jika." },
  { "en": "Lengkapi: ... jauh di mata, ... dekat di hati.", "id": "Walaupun... namun..." },
  { "en": "Lengkapi: Dia tidak takut ... siapapun.", "id": "Pada." },
  { "en": "Lengkapi: Dia bekerja ... tidak kenal lelah.", "id": "Seakan-akan." },
  { "en": "Lengkapi: Dia tetap berangkat ... kabut tebal.", "id": "Meskipun." },
  { "en": "Lengkapi: Dia diam ... menyetujui usul itu.", "id": "Tanda." },
  { "en": "Lengkapi: Dia tidak marah, ... hanya menasihati.", "id": "Tetapi." },
  { "en": "Lengkapi: Dia tertawa ... mendengar cerita lucu.", "id": "Sebab." },
  { "en": "Lengkapi: Belajarlah dengan tekun ... cita-citamu tercapai.", "id": "Agar." },
  { "en": "Lengkapi: Dia tidak hanya pandai, ... juga rendah hati.", "id": "Bahkan." },
  { "en": "Lengkapi: Semua murid hadir, ... Ali yang sakit.", "id": "Kecuali." },
  { "en": "Lengkapi: Dia akan datang ... tidak ada halangan.", "id": "Apabila." },
  { "en": "Lengkapi: Dia lebih tinggi ... adiknya.", "id": "Daripada." },
  { "en": "Lengkapi: Dia bekerja keras ... menafkahi keluarga.", "id": "Untuk." },
  { "en": "Lengkapi: Dia menyelesaikan tugasnya, ... dia beristirahat.", "id": "Lalu." },
  { "en": "Lengkapi: Dia tidak tahu ... harus berbuat apa.", "id": "Lagi." },
  { "en": "Lengkapi: ... hari mulai gelap, mereka pulang.", "id": "Tatkala." },
  { "en": "Lengkapi: Jangankan berjalan, berdiri ... dia tak sanggup.", "id": "Pun." },
  { "en": "Lengkapi: ... tidak ada lagi pertanyaan, rapat ditutup.", "id": "Karena." },
  { "en": "Lengkapi: Dia bukan malas, ... sedang tidak sehat.", "id": "Melainkan." },
  { "en": "Lengkapi: Dia belajar ... ujiannya lancar.", "id": "Supaya." },
  { "en": "Lengkapi: Dia menunggu ... temannya datang menjemput.", "id": "Hingga." },
  { "en": "Lengkapi: Saya akan ikut ... kamu yang pimpin.", "id": "Asalkan." },
  { "en": "Lengkapi: Dia sangat mengantuk, ... tetap menonton.", "id": "Namun." },
  { "en": "Lengkapi: Ayah sedang bekerja, ... jangan diganggu.", "id": "Maka." },
  { "en": "Lengkapi: Dia pandai berbicara ... seorang diplomat.", "id": "Laksana." },
  { "en": "Lengkapi: Dia akan tetap di sini ... kau kembali.", "id": "Sampai." },
  { "en": "Lengkapi: Semua orang tahu ... dia pembohong.", "id": "Bahwa." },
  { "en": "Lengkapi: Dia gagal dalam ujian, ... dia malas.", "id": "Akibat." },
  { "en": "Lengkapi: Jangan membuat keputusan ... sedang marah.", "id": "Ketika." },
  { "en": "Lengkapi: Dia akan pergi ke London ... Paris.", "id": "Atau." },
  { "en": "Lengkapi: ... dia kaya, dia tidak pernah sombong.", "id": "Walaupun." },
  { "en": "Lengkapi: Dia menulis surat ... mengirimkannya.", "id": "Dan." },
  { "en": "Lengkapi: Dia akan membelinya ... punya cukup uang.", "id": "Jika." },
  { "en": "Lengkapi: Dia makan dengan lahap ... sangat lapar.", "id": "Karena." },
  { "en": "Fungsi konjungsi 'di samping itu'?", "id": "Menambahkan informasi atau hal lain." },
  { "en": "Lengkapi: Dia pintar. ..., dia juga atlet.", "id": "Di samping itu." },
  { "en": "Fungsi konjungsi 'dengan kata lain'?", "id": "Menjelaskan dengan kalimat berbeda." },
  { "en": "Lengkapi: Dia hemat, ..., tidak boros.", "id": "Dengan kata lain." },
  { "en": "Fungsi 'meskipun demikian/begitu'?", "id": "Menyatakan pertentangan antarkalimat." },
  { "en": "Lengkapi: Ujiannya sulit. ..., dia lulus.", "id": "Meskipun demikian." },
  { "en": "Konjungsi 'tambahan lagi' semakna dengan?", "id": "Lagi pula, selain itu." },
  { "en": "Lengkapi: Harganya mahal, ..., kualitasnya buruk.", "id": "Tambahan lagi." },
  { "en": "Beda 'yakni' dan 'yaitu'?", "id": "Hampir tidak ada, sama-sama formal." },
  { "en": "Beda 'lalu' dan 'kemudian'?", "id": "'Lalu' lebih cepat jedanya." },
  { "en": "Konjungsi 'arkian' dan 'kalakian'?", "id": "Sastra lama, artinya 'setelah itu'." },
  { "en": "Sebutkan 3 konjungsi antarkalimat.", "id": "Namun, jadi, oleh karena itu." },
  { "en": "Selesaikan: Dia tidak suka sayur ... buah.", "id": "Ataupun." },
  { "en": "Selesaikan: Dia bekerja keras ... cita-citanya.", "id": "Demi." },
  { "en": "Selesaikan: ... kamu belajar, ... kamu bisa.", "id": "Makin... makin..." },
  { "en": "Konjungsi 'makin... makin' menyatakan?", "id": "Hubungan gradasi (kian... kian...)." },
  { "en": "Selesaikan: Dia tidak menyerah, ... terus mencoba.", "id": "Sebaliknya." },
  { "en": "Selesaikan: Dia datang terlambat ... ada kecelakaan.", "id": "Karena." },
  { "en": "Selesaikan: Bacalah buku petunjuk ... menggunakan alat ini.", "id": "Sebelum." },
  { "en": "Selesaikan: Dia akan ikut ... pestanya seru.", "id": "Asalkan." },
  { "en": "Selesaikan: Dia tidak hanya diam, ... bertanya.", "id": "Tetapi juga." },
  { "en": "Selesaikan: Jangan menebang pohon ... kapak tumpul.", "id": "Dengan." },
  { "en": "Selesaikan: Dia menunggu ... bus terakhir datang.", "id": "Hingga." },
  { "en": "Selesaikan: Dia pura-pura tidak tahu, ... dia tahu segalanya.", "id": "Padahal." },
  { "en": "Selesaikan: Dia akan kembali ... semua urusan selesai.", "id": "Setelah." },
  { "en": "Selesaikan: Dia sangat marah ... tidak mau bicara.", "id": "Sehingga." },
  { "en": "Selesaikan: Kamu harus memilih, sekarang ... tidak sama sekali?", "id": "Atau." },
  { "en": "Selesaikan: Dia akan memaafkan ... kamu jujur.", "id": "Apabila." },
  { "en": "Selesaikan: Dia terlihat lelah ... kurang tidur.", "id": "Sebab." },
  { "en": "Selesaikan: Dia belajar bahasa Inggris ... bisa kuliah di luar negeri.", "id": "Agar." },
  { "en": "Selesaikan: Dia bukan pelukis, ... seorang penyanyi.", "id": "Melainkan." },
  { "en": "Selesaikan: Dia tetap tersenyum ... sedang ada masalah.", "id": "Meskipun." },
  { "en": "Selesaikan: Aku percaya ... semua akan indah pada waktunya.", "id": "Bahwa." },
  { "en": "Selesaikan: Baik pagi ... siang, toko itu selalu ramai.", "id": "Maupun." },
  { "en": "Selesaikan: Dia memancing ... menunggu senja.", "id": "Sambil." },
  { "en": "Selesaikan: Dia lebih suka di rumah ... pergi keluar.", "id": "Daripada." },
  { "en": "Selesaikan: Dia menjadi pendiam ... peristiwa itu.", "id": "Sejak." },
  { "en": "Selesaikan: Dia bekerja ... tidak mengharapkan imbalan.", "id": "Tanpa." },
  { "en": "Selesaikan: Dia terus berjalan ... kakinya sakit.", "id": "Walaupun." },
  { "en": "Selesaikan: Beristirahatlah sejenak ... melanjutkan perjalanan.", "id": "Sebelum." },
  { "en": "Selesaikan: Dia akan datang ... tidak ada urusan mendadak.", "id": "Jika." },
  { "en": "Selesaikan: Dia tidak membalas pesanku, ... hanya membacanya.", "id": "Tetapi." },
  { "en": "Selesaikan: Dia membersihkan rumah, ... adiknya menyiram tanaman.", "id": "Sementara." },
  { "en": "Selesaikan: Dia berbicara dengan jelas ... semua orang paham.", "id": "Supaya." },
  { "en": "Selesaikan: Dia membeli banyak makanan, ...: roti, keju, susu.", "id": "Yaitu." },
  { "en": "Selesaikan: Dia memasak, ... menyiapkan meja makan.", "id": "Lalu." },
  { "en": "Selesaikan: Dia terkejut ... melihat temannya datang.", "id": "Ketika." },
  { "en": "Selesaikan: Dia tidak akan berhasil ... usahanya sendiri.", "id": "Tanpa." },
  { "en": "Selesaikan: Dia akan menunggumu ... kamu datang.", "id": "Sampai." },
  { "en": "Selesaikan: Dia mirip sekali dengan ibunya, ... senyumnya.", "id": "Terutama." },
  { "en": "Selesaikan: Dia tidak hanya cantik, ... juga cerdas.", "id": "Bahkan." },
  { "en": "Selesaikan: Semua diundang ke pesta, ... mantan pacarnya.", "id": "Kecuali." },
  { "en": "Selesaikan: Dia berjanji akan datang, ... dia tidak muncul.", "id": "Namun." },
  { "en": "Selesaikan: Suasana menjadi ramai ... dia datang.", "id": "Semenjak." },
  { "en": "Selesaikan: Dia berlatih keras ... pertandingan.", "id": "Untuk." },
  { "en": "Selesaikan: Dia tidak mau makan, ... dibujuk.", "id": "Sekalipun." },
  { "en": "Selesaikan: Dia menjelaskan masalahnya ... singkat.", "id": "Secara." },
  { "en": "Selesaikan: ... kamu tidak keberatan, aku akan bertanya.", "id": "Jikalau." },
  { "en": "Selesaikan: Dia tidak suka pedas, ... dia makan sambal.", "id": "Akan tetapi." },
  { "en": "Selesaikan: Dia tersenyum ... melambaikan tangan.", "id": "Seraya." },
  { "en": "Selesaikan: Ayah, ibu, ... anak-anaknya berlibur bersama.", "id": "Serta." },
  { "en": "Selesaikan: Dia bukan pembohong, ... orang jujur.", "id": "Melainkan." },
  { "en": "Selesaikan: Dia bergegas pulang ... hari sudah sore.", "id": "Karena." },
  { "en": "Selesaikan: Dia akan meminjamkan bukunya ... kamu jaga baik-baik.", "id": "Asal." },
  { "en": "Selesaikan: Dia tetap optimis ... banyak rintangan.", "id": "Kendatipun." },
  { "en": "Selesaikan: Dia diam ... sedang berpikir keras.", "id": "Seolah-olah." },
  { "en": "Selesaikan: Dia sangat kaya, ... tidak pernah pamer.", "id": "Namun." },
  { "en": "Selesaikan: Cuci piringnya ... selesai makan.", "id": "Setelah." },
  { "en": "Selesaikan: Dia marah ... terus diganggu.", "id": "Akibat." },
  { "en": "Selesaikan: ... kamu menjadi aku, apa yang kau lakukan?", "id": "Andaikata." },
  { "en": "Selesaikan: Dia tidak hanya lupa, ... tidak peduli.", "id": "Malah." },
  { "en": "Selesaikan: Dia bekerja dengan giat ... mendapat bonus.", "id": "Agar." },
  { "en": "Selesaikan: Dia pergi ke sekolah ... berjalan kaki.", "id": "Dengan." },
  { "en": "Selesaikan: Dia tidak akan mengganggumu ... kamu sedang sibuk.", "id": "Selama." },
  { "en": "Selesaikan: Dia sangat gembira ... menerima kabar itu.", "id": "Waktu." },
  { "en": "Selesaikan: Dia harus memilih ... melanjutkan kuliah ... bekerja.", "id": "Antara... atau..." },
  { "en": "Selesaikan: Dia tidak menjawab, ... langsung pergi.", "id": "Lantas." },
  { "en": "Selesaikan: ... gagal, itu bukan akhir segalanya.", "id": "Kalaupun." },
  { "en": "Selesaikan: Dia belajar dari kesalahan, ... menjadi lebih baik.", "id": "Kemudian." },
  { "en": "Selesaikan: Dia tidak akan berubah ... siapapun.", "id": "Demi." },
  { "en": "Selesaikan: ... dia tidak datang, kita tetap mulai.", "id": "Biarpun." },
  { "en": "Selesaikan: Dia memandangku ... belum pernah bertemu.", "id": "Seakan." },
  { "en": "Selesaikan: Dia tidak hanya membantah, ... juga menuduh.", "id": "Bahkan." },
  { "en": "Selesaikan: Semua tampak normal, ... ada yang aneh.", "id": "Padahal." },
  { "en": "Selesaikan: ... kamu di sini, aku merasa tenang.", "id": "Selama." },
  { "en": "Selesaikan: Dia tidak akan menyerah ... titik darah penghabisan.", "id": "Sampai." },
  { "en": "Selesaikan: Baik miskin ... kaya, semua sama.", "id": "Maupun." },
  { "en": "Selesaikan: Dia tidak makan daging, ... ikan.", "id": "Ataupun." },
  { "en": "Selesaikan: Dia akan sukses ... dia disiplin.", "id": "Bila." },
  { "en": "Selesaikan: Dia menjelaskan teorinya ... memberikan contoh.", "id": "Serta." },
  { "en": "Selesaikan: Dia tidak pandai matematika, ... dia jago seni.", "id": "Tetapi." },
  { "en": "Selesaikan: Dia menyanyi ... menari di panggung.", "id": "Sambil." },
  { "en": "Selesaikan: ... kamu sudah tahu, jangan pura-pura.", "id": "Kalau." },
  { "en": "Selesaikan: Dia bekerja keras, ... hasilnya memuaskan.", "id": "Alhasil." },
  { "en": "Selesaikan: Dia tidak suka keramaian, ... lebih suka menyendiri.", "id": "Sebaliknya." },
  { "en": "Selesaikan: ... , mari kita tutup acara ini.", "id": "Akhirnya." },
  { "en": "Klasifikasi konjungsi 'karena'?", "id": "Subordinatif kausal (sebab)." },
  { "en": "Klasifikasi konjungsi 'jika'?", "id": "Subordinatif kondisional (syarat)." },
  { "en": "Klasifikasi konjungsi 'ketika'?", "id": "Subordinatif temporal (waktu)." },
  { "en": "Klasifikasi konjungsi 'agar'?", "id": "Subordinatif final (tujuan)." },
  { "en": "Klasifikasi konjungsi 'meskipun'?", "id": "Subordinatif konsesif (pertentangan)." },
  { "en": "Klasifikasi konjungsi 'sehingga'?", "id": "Subordinatif konsekutif (akibat)." },
  { "en": "Klasifikasi konjungsi 'seperti'?", "id": "Subordinatif komparatif (perbandingan)." },
  { "en": "Klasifikasi konjungsi 'dan'?", "id": "Koordinatif aditif (penambahan)." },
  { "en": "Klasifikasi konjungsi 'atau'?", "id": "Koordinatif disjungtif (pilihan)." },
  { "en": "Klasifikasi konjungsi 'tetapi'?", "id": "Koordinatif adversatif (perlawanan)." },
  { "en": "Hubungkan: 'Dia rajin.' 'Dia berhasil.'", "id": "Karena... maka..." },
  { "en": "Hubungkan: 'Hujan turun.' 'Saya bawa payung.'", "id": "Jika... maka..." },
  { "en": "Hubungkan: 'Dia kaya.' 'Dia sombong.'", "id": "Meskipun... tetapi..." },
  { "en": "Konjungsi syarat selain 'jika'?", "id": "Bila, apabila, asalkan, kalau." },
  { "en": "Konjungsi sebab selain 'karena'?", "id": "Sebab, lantaran." },
  { "en": "Konjungsi akibat selain 'sehingga'?", "id": "Hingga, sampai, akibatnya." },
  { "en": "Konjungsi tujuan selain 'agar'?", "id": "Supaya, untuk, guna, biar." },
  { "en": "Konjungsi waktu selain 'ketika'?", "id": "Saat, waktu, tatkala, manakala." },
  { "en": "Sinonim konjungsi 'walaupun'?", "id": "Meskipun, biarpun, kendatipun." },
  { "en": "Selesaikan: Dia tidak menyerah ... gagal berkali-kali.", "id": "Meskipun." },
  { "en": "Selesaikan: Dia akan datang ... tidak ada urusan.", "id": "Bila." },
  { "en": "Selesaikan: Dia dimarahi ... membuat kesalahan.", "id": "Lantaran." },
  { "en": "Selesaikan: Dia berteriak ... suaranya serak.", "id": "Hingga." },
  { "en": "Selesaikan: Dia berbisik ... tidak terdengar.", "id": "Supaya." },
  { "en": "Selesaikan: ... bel berbunyi, semua murid masuk.", "id": "Tatkala." },
  { "en": "Selesaikan: Dia tetap tersenyum ... diejek.", "id": "Biarpun." },
  { "en": "Selesaikan: Dia membeli buku, ..., dia membacanya.", "id": "Kemudian." },
  { "en": "Selesaikan: Dia tidak suka kopi, ... teh.", "id": "Melainkan." },
  { "en": "Selesaikan: Jangan pergi ... aku belum datang.", "id": "Sebelum." },
  { "en": "Selesaikan: Aku akan menunggumu ... kamu siap.", "id": "Sampai." },
  { "en": "Selesaikan: Dia tidak hanya pandai, ... juga rendah hati.", "id": "Tetapi juga." },
  { "en": "Selesaikan: Dia bekerja keras ... bisa berlibur.", "id": "Agar." },
  { "en": "Selesaikan: Dia gagal ujian ... tidak belajar.", "id": "Karena." },
  { "en": "Selesaikan: Dia akan memaafkanmu ... kamu mengaku.", "id": "Asalkan." },
  { "en": "Selesaikan: ... dia tidak datang, kita tetap lanjut.", "id": "Walaupun." },
  { "en": "Selesaikan: Dia menyapu lantai, ... kakaknya mencuci piring.", "id": "Sementara." },
  { "en": "Selesaikan: Dia tidak marah, ... hanya sedikit kecewa.", "id": "Tetapi." },
  { "en": "Selesaikan: Baik kamu ... dia, keduanya harus hadir.", "id": "Maupun." },
  { "en": "Selesaikan: Dia makan dengan lahap ... sangat lapar.", "id": "Sebab." },
  { "en": "Selesaikan: Dia bergegas ... tidak terlambat.", "id": "Supaya." },
  { "en": "Selesaikan: ... hari sudah larut, dia belum pulang.", "id": "Padahal." },
  { "en": "Selesaikan: Dia tidak punya uang ... membeli buku.", "id": "Untuk." },
  { "en": "Selesaikan: Dia sangat lelah, ... dia istirahat.", "id": "Oleh karena itu." },
  { "en": "Selesaikan: Dia tidak hanya lupa, ... tidak peduli.", "id": "Bahkan." },
  { "en": "Selesaikan: Dia akan pergi ... hujan reda.", "id": "Setelah." },
  { "en": "Selesaikan: Dia belajar sambil ... musik.", "id": "Mendengarkan." },
  { "en": "Selesaikan: Dia akan membantumu ... tidak sibuk.", "id": "Jika." },
  { "en": "Selesaikan: Dia bukan malas, ... hanya butuh istirahat.", "id": "Melainkan." },
  { "en": "Selesaikan: Dia lebih memilih diam ... berdebat.", "id": "Daripada." },
  { "en": "Selesaikan: Dia menjadi terkenal ... memenangkan lomba.", "id": "Sejak." },
  { "en": "Selesaikan: Dia menyelesaikan pekerjaannya ... cepat.", "id": "Dengan." },
  { "en": "Selesaikan: Aku yakin ... dia orang yang baik.", "id": "Bahwa." },
  { "en": "Selesaikan: Dia terus berlari ... mencapai garis finis.", "id": "Hingga." },
  { "en": "Selesaikan: Dia tidak akan ikut ... dipaksa.", "id": "Sekalipun." },
  { "en": "Selesaikan: Dia tidak suka durian ... nangka.", "id": "Ataupun." },
  { "en": "Selesaikan: Dia menjelaskan rencananya ... detail.", "id": "Secara." },
  { "en": "Selesaikan: Dia pandai seperti ayahnya, ... ibunya juga pandai.", "id": "Dan." },
  { "en": "Selesaikan: Dia tidak mau makan, ... diajak jalan-jalan.", "id": "Kecuali." },
  { "en": "Selesaikan: Dia sangat senang, ... dia tersenyum terus.", "id": "Sehingga." },
  { "en": "Selesaikan: Dia akan membelikanmu mainan ... kamu ranking satu.", "id": "Bila." },
  { "en": "Selesaikan: Dia tidak hanya kaya, ... sombong.", "id": "Malah." },
  { "en": "Selesaikan: Dia datang ke pesta ... tidak diundang.", "id": "Walaupun." },
  { "en": "Selesaikan: Pertama, baca soalnya. ..., jawab pertanyaannya.", "id": "Lalu." },
  { "en": "Selesaikan: Dia akan pergi ke Bali ... Lombok.", "id": "Atau." },
  { "en": "Selesaikan: Dia tidak akan menyerah ... gagal lagi.", "id": "Meskipun." },
  { "en": "Selesaikan: Dia menabung ... masa tuanya.", "id": "Untuk." },
  { "en": "Selesaikan: ... dia tidak jujur, aku tidak akan percaya lagi.", "id": "Kalau." },
  { "en": "Selesaikan: Dia tidak hanya guru, ... juga seorang penulis.", "id": "Tetapi." },
  { "en": "Selesaikan: Dia sangat lelah. ..., dia tetap bekerja.", "id": "Namun." },
  { "en": "Selesaikan: Dia bekerja ... menghasilkan uang.", "id": "Guna." },
  { "en": "Selesaikan: Dia pulang ... pekerjaan selesai.", "id": "Seusai." },
  { "en": "Selesaikan: Dia marah ... dibohongi temannya.", "id": "Karena." },
  { "en": "Selesaikan: Dia akan membantumu ... kamu juga membantunya.", "id": "Asalkan." },
  { "en": "Selesaikan: Dia tetap tenang ... suasana panik.", "id": "Biarpun." },
  { "en": "Selesaikan: Dia berbicara seperti ... tahu segalanya.", "id": "Seolah-olah." },
  { "en": "Selesaikan: Dia tidak punya waktu, ... untuk istirahat.", "id": "Apalagi." },
  { "en": "Selesaikan: Dia miskin, ... dia sangat dermawan.", "id": "Akan tetapi." },
  { "en": "Selesaikan: Dia menyelesaikan tugasnya, ... dia bermain.", "id": "Kemudian." },
  { "en": "Selesaikan: Dia tidak akan pergi ... cuaca membaik.", "id": "Sebelum." },
  { "en": "Selesaikan: Dia akan menunggumu di sini ... kau datang.", "id": "Sampai." },
  { "en": "Selesaikan: Dia tidak hanya pintar, ... sangat kreatif.", "id": "Bahkan." },
  { "en": "Selesaikan: ... dia tidak ada, rapat dibatalkan.", "id": "Berhubung." },
  { "en": "Fungsi konjungsi 'berhubung'?", "id": "Menyatakan alasan (sebab)." },
  { "en": "Selesaikan: Dia tidak mau sekolah, ... ingin bekerja.", "id": "Melainkan." },
  { "en": "Selesaikan: Dia akan membelinya ... harganya murah.", "id": "Jika." },
  { "en": "Selesaikan: Dia tidak akan menyerah ... ada harapan.", "id": "Selama." },
  { "en": "Selesaikan: Dia belajar dengan giat ... lulus dengan baik.", "id": "Agar." },
  { "en": "Selesaikan: Dia tidak hanya datang terlambat, ... tidak membawa tugas.", "id": "Tetapi juga." },
  { "en": "Selesaikan: Dia akan berlibur ... semua tugasnya selesai.", "id": "Setelah." },
  { "en": "Selesaikan: Dia tidak akan berhasil ... kerja keras.", "id": "Tanpa." },
  { "en": "Selesaikan: Dia menangis ... mendengar kabar duka.", "id": "Ketika." },
  { "en": "Selesaikan: Dia tidak suka pedas, ... dia mencoba sambal itu.", "id": "Namun." },
  { "en": "Selesaikan: Baik di darat ... di laut, tim SAR terus mencari.", "id": "Maupun." },
  { "en": "Selesaikan: Dia tidak akan pergi ... kamu ikut.", "id": "Kecuali." },
  { "en": "Selesaikan: Dia sangat gembira ... mendapat beasiswa.", "id": "Sebab." },
  { "en": "Selesaikan: Dia akan meminjamkanmu uang ... kamu kembalikan tepat waktu.", "id": "Asalkan." },
  { "en": "Selesaikan: Dia tetap bersemangat ... usianya sudah tua.", "id": "Meskipun." },
  { "en": "Konjungsi 'dan', 'serta', 'lagipula' termasuk?", "id": "Kelompok penambahan (aditif)." },
  { "en": "Konjungsi 'atau', 'ataupun' termasuk?", "id": "Kelompok pilihan (disjungtif)." },
  { "en": "Konjungsi 'tetapi', 'namun', 'melainkan'?", "id": "Kelompok perlawanan (adversatif)." },
  { "en": "Konjungsi 'lalu', 'kemudian', 'setelah itu'?", "id": "Kelompok urutan (temporal)." },
  { "en": "Konjungsi 'jika', 'bila', 'asalkan'?", "id": "Kelompok syarat (kondisional)." },
  { "en": "Konjungsi 'karena', 'sebab', 'lantaran'?", "id": "Kelompok sebab (kausal)." },
  { "en": "Konjungsi 'sehingga', 'sampai', 'akibatnya'?", "id": "Kelompok akibat (konsekutif)." },
  { "en": "Konjungsi 'agar', 'supaya', 'biar'?", "id": "Kelompok tujuan (final)." },
  { "en": "Konjungsi 'walau', 'meski', 'biarpun'?", "id": "Kelompok konsesif." },
  { "en": "Konjungsi 'seperti', 'bagai', 'laksana'?", "id": "Kelompok perbandingan (komparatif)." },
  { "en": "Selesaikan: Dia tidak hanya datang, ... juga membawa oleh-oleh.", "id": "Tetapi." },
  { "en": "Selesaikan: Baik itu benar ... salah, kita harus periksa.", "id": "Maupun." },
  { "en": "Selesaikan: ... dia tidak belajar, ... dia bisa menjawab.", "id": "Meskipun... tetapi..." },
  { "en": "Selesaikan: Bukannya diam, ... dia malah ikut campur.", "id": "Melainkan." },
  { "en": "Selesaikan: Dia bekerja begitu keras ... jatuh sakit.", "id": "Hingga." },
  { "en": "Selesaikan: Aku akan pergi ... kau mengizinkanku.", "id": "Jikalau." },
  { "en": "Selesaikan: Dia dihukum ... melanggar aturan.", "id": "Sebab." },
  { "en": "Selesaikan: Tutup jendela ... nyamuk tidak masuk.", "id": "Agar." },
  { "en": "Selesaikan: Dia tetap tersenyum ... hatinya menangis.", "id": "Walaupun." },
  { "en": "Selesaikan: Wajahnya pucat ... mayat.", "id": "Seperti." },
  { "en": "Konjungsi antarkalimat untuk pertentangan?", "id": "Namun, akan tetapi, sebaliknya." },
  { "en": "Konjungsi antarkalimat untuk kesimpulan?", "id": "Jadi, dengan demikian." },
  { "en": "Konjungsi antarkalimat untuk akibat?", "id": "Oleh karena itu, akibatnya." },
  { "en": "Konjungsi antarkalimat untuk penambahan?", "id": "Selain itu, di samping itu." },
  { "en": "Selesaikan: Dia malas. ..., dia tidak naik kelas.", "id": "Akibatnya." },
  { "en": "Selesaikan: Dia rajin. ..., adiknya pemalas.", "id": "Sebaliknya." },
  { "en": "Selesaikan: Pelatihannya berat. ..., banyak yang menyerah.", "id": "Maka." },
  { "en": "Selesaikan: Kita harus hemat. ..., pengeluaran harus ditekan.", "id": "Dengan demikian." },
  { "en": "Benarkah 'dengan' selalu menjadi konjungsi?", "id": "Tidak, bisa juga preposisi." },
  { "en": "Benarkah 'karena' bisa di awal kalimat?", "id": "Ya, sebagai awal anak kalimat." },
  { "en": "Selesaikan: ... kamu sudah sampai, telepon aku.", "id": "Apabila." },
  { "en": "Selesaikan: Dia tidak makan nasi, ... hanya lauknya.", "id": "Tetapi." },
  { "en": "Selesaikan: Dia tidak berkata-kata ... pergi begitu saja.", "id": "Lantas." },
  { "en": "Selesaikan: Dia akan lulus ... Tuhan menghendaki.", "id": "Jika." },
  { "en": "Selesaikan: Dia tidak akan memaafkanmu ... kamu berbohong lagi.", "id": "Kalau." },
  { "en": "Selesaikan: Aku tidak akan pergi ... ada kamu.", "id": "Kecuali." },
  { "en": "Selesaikan: Dia bekerja keras ... dapat membayar utang.", "id": "Supaya." },
  { "en": "Selesaikan: Dia tetap berjuang ... kemungkinannya kecil.", "id": "Meskipun." },
  { "en": "Selesaikan: Dia tidak makan ... minum seharian.", "id": "Dan." },
  { "en": "Selesaikan: Dia akan kembali ... satu jam lagi.", "id": "Dalam." },
  { "en": "Selesaikan: Dia datang tepat ... rapat dimulai.", "id": "Saat." },
  { "en": "Selesaikan: Dia begitu terkejut ... tidak bisa bergerak.", "id": "Sehingga." },
  { "en": "Selesaikan: Dia akan selalu mendukungmu ... kamu benar.", "id": "Selama." },
  { "en": "Selesaikan: Saya tahu ... kamu tidak bersalah.", "id": "Bahwa." },
  { "en": "Selesaikan: Dia tidak akan pergi ... dipaksa sekalipun.", "id": "Walau." },
  { "en": "Selesaikan: Dia menangis ... mengingat masa lalunya.", "id": "Sambil." },
  { "en": "Selesaikan: Dia tidak hanya pintar, ... juga sangat rendah hati.", "id": "Bahkan." },
  { "en": "Selesaikan: Kamu mau ikut ... tinggal di rumah?", "id": "Atau." },
  { "en": "Selesaikan: Dia akan meminjamkanmu buku ... kamu kembalikan.", "id": "Asalkan." },
  { "en": "Selesaikan: Dia bukan marah, ... sedang menasihati.", "id": "Melainkan." },
  { "en": "Selesaikan: Dia belajar dari pengalaman ... menjadi bijak.", "id": "Lalu." },
  { "en": "Selesaikan: Dia sangat lapar, ... dia makan sangat banyak.", "id": "Oleh sebab itu." },
  { "en": "Selesaikan: Dia akan menyelesaikan laporannya ... malam ini.", "id": "Sebelum." },
  { "en": "Selesaikan: Dia menunggu ... busnya tiba.", "id": "Hingga." },
  { "en": "Selesaikan: Dia tidak akan berhasil ... bantuan dari orang lain.", "id": "Tanpa." },
  { "en": "Selesaikan: Dia akan pergi ke mana ... kamu pergi.", "id": "Pun." },
  { "en": "Selesaikan: Dia tidak hanya lupa janji, ... juga tidak merasa bersalah.", "id": "Malah." },
  { "en": "Selesaikan: Dia akan berangkat ... cuaca cerah.", "id": "Bila." },
  { "en": "Selesaikan: Dia lebih memilih membaca ... menonton TV.", "id": "Daripada." },
  { "en": "Selesaikan: Dia menjadi atlet profesional ... usia muda.", "id": "Sejak." },
  { "en": "Selesaikan: Dia akan membantumu ... tidak merepotkannya.", "id": "Selama." },
  { "en": "Selesaikan: Dia tidak hanya gagal, ... juga berhutang banyak.", "id": "Bahkan." },
  { "en": "Selesaikan: Dia akan tetap datang ... hujan badai sekalipun.", "id": "Sekalipun." },
  { "en": "Selesaikan: Dia tidak mau makan nasi ... roti.", "id": "Maupun." },
  { "en": "Selesaikan: Dia akan memaafkanmu ... kamu tidak mengulanginya lagi.", "id": "Asalkan." },
  { "en": "Selesaikan: Dia tidak hanya guru, ... juga seorang musisi.", "id": "Tetapi." },
  { "en": "Selesaikan: Dia sangat kecewa, ... dia tetap tegar.", "id": "Namun." },
  { "en": "Selesaikan: Dia belajar dengan giat ... bisa meraih cita-citanya.", "id": "Guna." },
  { "en": "Selesaikan: Dia akan pergi ... semua pekerjaannya selesai.", "id": "Setelah." },
  { "en": "Selesaikan: Dia menangis ... kebahagiaan.", "id": "Karena." },
  { "en": "Selesaikan: Dia akan tetap di sini ... kamu kembali.", "id": "Sampai." },
  { "en": "Selesaikan: Dia akan membeli mobil baru ... punya cukup uang.", "id": "Apabila." },
  { "en": "Selesaikan: Dia tidak marah, ... hanya kecewa dengan sikapmu.", "id": "Tetapi." },
  { "en": "Selesaikan: Dia akan datang ke pestamu ... diundang.", "id": "Jika." },
  { "en": "Selesaikan: Dia tidak hanya malas, ... juga tidak bertanggung jawab.", "id": "Melainkan." },
  { "en": "Selesaikan: Dia akan berangkat besok ... tidak ada halangan.", "id": "Kalau." },
  { "en": "Selesaikan: Dia akan mengerjakan tugasnya ... selesai.", "id": "Hingga." },
  { "en": "Selesaikan: Dia tidak akan pergi ... kamu ikut serta.", "id": "Kecuali." },
  { "en": "Selesaikan: Dia akan membantumu ... ada imbalannya.", "id": "Asal." },
  { "en": "Selesaikan: Dia tetap bekerja ... merasa tidak enak badan.", "id": "Meskipun." },
  { "en": "Selesaikan: Dia tidak hanya diam, ... dia juga berpikir keras.", "id": "Tetapi." },
  { "en": "Selesaikan: Dia akan pergi ke toko buku ... perpustakaan.", "id": "Atau." },
  { "en": "Selesaikan: Dia akan selalu mengingatmu ... kamu pergi jauh.", "id": "Walaupun." },
  { "en": "Selesaikan: Dia akan meminjamkanmu payung ... hujan.", "id": "Jika." },
  { "en": "Selesaikan: Dia tidak hanya kaya, ... juga sangat dermawan.", "id": "Bahkan." },
  { "en": "Selesaikan: Dia tidak akan menyerah ... masih ada waktu.", "id": "Selama." },
  { "en": "Selesaikan: Dia akan membantumu ... kamu memintanya dengan baik.", "id": "Asalkan." },
  { "en": "Selesaikan: Dia tidak hanya datang, ... dia juga membawa teman-temannya.", "id": "Tetapi." },
  { "en": "Selesaikan: Dia akan pergi ke pantai ... gunung untuk liburan.", "id": "Atau." },
  { "en": "Selesaikan: Dia akan tetap tersenyum ... apapun yang terjadi.", "id": "Meskipun." },
  { "en": "Selesaikan: Dia akan membantumu ... kamu berjanji untuk tidak memberitahu siapa pun.", "id": "Jika." },
  { "en": "Selesaikan: Dia tidak hanya seorang dokter, ... juga seorang relawan.", "id": "Tetapi." },
  { "en": "Selesaikan: Dia akan pergi sekarang ... nanti malam.", "id": "Atau." },
  { "en": "Selesaikan: Dia akan tetap mencintaimu ... kamu tidak sempurna.", "id": "Walaupun." },
  { "en": "Selesaikan: Dia akan membelikanmu hadiah ... kamu berulang tahun.", "id": "Jika." },
  { "en": "Selesaikan: Dia tidak hanya teman baikku, ... juga seperti saudaraku sendiri.", "id": "Tetapi." },
  { "en": "Selesaikan: Dia akan pergi ke bioskop ... taman hiburan akhir pekan ini.", "id": "Atau." }



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
