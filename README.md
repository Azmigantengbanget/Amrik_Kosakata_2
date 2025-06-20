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


  { "en": "noble?", "id": "mulia." },
  { "en": "nod?", "id": "mengangguk." },
  { "en": "nominate?", "id": "mencalonkan." },
  { "en": "nomination?", "id": "nominasi." },
  { "en": "nominee?", "id": "calon." },
  { "en": "nonetheless?", "id": "namun." },
  { "en": "nonprofit?", "id": "nirlaba." },
  { "en": "nonsense?", "id": "omong kosong." },
  { "en": "noon?", "id": "tengah hari." },
  { "en": "notable?", "id": "terkenal." },
  { "en": "notably?", "id": "terutama." },
  { "en": "notify?", "id": "memberitahu." },
  { "en": "notorious?", "id": "terkenal buruk." },
  { "en": "novel?", "id": "baru." },
  { "en": "nursery?", "id": "tempat penitipan anak." },
  { "en": "objection?", "id": "keberatan." },
  { "en": "oblige?", "id": "mewajibkan." },
  { "en": "obsess?", "id": "terobsesi." },
  { "en": "obsession?", "id": "obsesi." },
  { "en": "occasional?", "id": "kadang-kadang." },
  { "en": "occurrence?", "id": "kejadian." },
  { "en": "odds?", "id": "kemungkinan." },
  { "en": "offering?", "id": "persembahan." },
  { "en": "offspring?", "id": "keturunan." },
  { "en": "operational?", "id": "operasional." },
  { "en": "opt?", "id": "memilih." },
  { "en": "optical?", "id": "optik." },
  { "en": "optimism?", "id": "optimisme." },
  { "en": "oral?", "id": "lisan." },
  { "en": "organizational?", "id": "organisasional." },
  { "en": "orientation?", "id": "orientasi." },
  { "en": "originate?", "id": "berasal." },
  { "en": "outbreak?", "id": "wabah." },
  { "en": "outing?", "id": "jalan-jalan." },
  { "en": "outlet?", "id": "gerai." },
  { "en": "outlook?", "id": "pandangan." },
  { "en": "outrage?", "id": "kemarahan, membuat marah." },
  { "en": "outsider?", "id": "orang luar." },
  { "en": "overlook?", "id": "mengabaikan." },
  { "en": "overly?", "id": "terlalu." },
  { "en": "oversee?", "id": "mengawasi." },
  { "en": "overturn?", "id": "membatalkan." },
  { "en": "overwhelm?", "id": "membanjiri." },
  { "en": "overwhelming?", "id": "luar biasa." },
  { "en": "pad?", "id": "bantalan." },
  { "en": "parameter?", "id": "parameter." },
  { "en": "parental?", "id": "orang tua." },
  { "en": "parliament?", "id": "parlemen." },
  { "en": "partial?", "id": "sebagian." },
  { "en": "partially?", "id": "sebagian." },
  { "en": "passing?", "id": "kematian." },
  { "en": "passive?", "id": "pasif." },
  { "en": "pastor?", "id": "pendeta." },
  { "en": "patent?", "id": "paten." },
  { "en": "pathway?", "id": "jalur." },
  { "en": "patrol?", "id": "patroli, berpatroli." },
  { "en": "patron?", "id": "pelindung." },
  { "en": "peak?", "id": "puncak." },
  { "en": "peasant?", "id": "petani." },
  { "en": "peculiar?", "id": "aneh." },
  { "en": "pension?", "id": "pensiun." },
  { "en": "persist?", "id": "bertahan." },
  { "en": "persistent?", "id": "gigih." },
  { "en": "personnel?", "id": "personel." },
  { "en": "petition?", "id": "petisi." },
  { "en": "philosopher?", "id": "filsuf." },
  { "en": "philosophical?", "id": "filosofis." },
  { "en": "pioneer?", "id": "perintis, merintis." },
  { "en": "pipeline?", "id": "pipa." },
  { "en": "pirate?", "id": "bajak laut." },
  { "en": "pit?", "id": "lubang." },
  { "en": "plea?", "id": "permohonan." },
  { "en": "plead?", "id": "memohon." },
  { "en": "pledge?", "id": "berjanji, janji." },
  { "en": "plug?", "id": "menyumbat, sumbat." },
  { "en": "plunge?", "id": "terjun." },
  { "en": "pole?", "id": "tiang." },
  { "en": "poll?", "id": "jajak pendapat." },
  { "en": "pond?", "id": "kolam." },
  { "en": "pop?", "id": "meletus." },
  { "en": "portfolio?", "id": "portofolio." },
  { "en": "portray?", "id": "menggambarkan." },
  { "en": "postpone?", "id": "menunda." },
  { "en": "postwar?", "id": "pascaperang." },
  { "en": "practitioner?", "id": "praktisi." },
  { "en": "preach?", "id": "berkhotbah." },
  { "en": "precedent?", "id": "preseden." },
  { "en": "precision?", "id": "ketepatan." },
  { "en": "predator?", "id": "predator." },
  { "en": "predecessor?", "id": "pendahulu." },
  { "en": "predominantly?", "id": "terutama." },
  { "en": "pregnancy?", "id": "kehamilan." },
  { "en": "prejudice?", "id": "prasangka." },
  { "en": "preliminary?", "id": "pendahuluan." },
  { "en": "premier?", "id": "perdana menteri." },
  { "en": "premise?", "id": "premis." },
  { "en": "premium?", "id": "premi." },
  { "en": "prescribe?", "id": "meresepkan." },
  { "en": "prescription?", "id": "resep." },
  { "en": "presently?", "id": "saat ini." },
  { "en": "preservation?", "id": "pelestarian." },
  { "en": "preside?", "id": "memimpin." },
  { "en": "presidency?", "id": "kepresidenan." },
  { "en": "prestigious?", "id": "bergengsi." },
  { "en": "presumably?", "id": "diduga." },
  { "en": "presume?", "id": "menduga." },
  { "en": "prevalence?", "id": "prevalensi." },
  { "en": "prevail?", "id": "menang." },
  { "en": "prevention?", "id": "pencegahan." },
  { "en": "prey?", "id": "mangsa." },
  { "en": "privatization?", "id": "privatisasi." },
  { "en": "privilege?", "id": "hak istimewa." },
  { "en": "probe?", "id": "penyelidikan, menyelidiki." },
  { "en": "problematic?", "id": "bermasalah." },
  { "en": "proceeding?", "id": "proses." },
  { "en": "proceeds?", "id": "hasil." },
  { "en": "processing?", "id": "pemrosesan." },
  { "en": "processor?", "id": "prosesor." },
  { "en": "proclaim?", "id": "memproklamasikan." },
  { "en": "productive?", "id": "produktif." },
  { "en": "productivity?", "id": "produktivitas." },
  { "en": "profitable?", "id": "menguntungkan." },
  { "en": "profound?", "id": "mendalam." },
  { "en": "projection?", "id": "proyeksi." },
  { "en": "prominent?", "id": "terkemuka." },
  { "en": "pronounced?", "id": "jelas." },
  { "en": "propaganda?", "id": "propaganda." },
  { "en": "proposition?", "id": "proposisi." },
  { "en": "prosecute?", "id": "menuntut." },
  { "en": "prosecution?", "id": "penuntutan." },
  { "en": "prosecutor?", "id": "jaksa." },
  { "en": "prospective?", "id": "prospektif." },
  { "en": "prosperity?", "id": "kemakmuran." },
  { "en": "protective?", "id": "protektif." },
  { "en": "protocol?", "id": "protokol." },
  { "en": "province?", "id": "provinsi." },
  { "en": "provincial?", "id": "provinsi." },
  { "en": "provision?", "id": "ketentuan." },
  { "en": "provoke?", "id": "memprovokasi." },
  { "en": "psychiatric?", "id": "psikiatri." },
  { "en": "pulse?", "id": "denyut nadi." },
  { "en": "pump?", "id": "memompa, pompa." },
  { "en": "punch?", "id": "pukulan, meninju." },
  { "en": "query?", "id": "pertanyaan." },
  { "en": "quest?", "id": "pencarian." },
  { "en": "quota?", "id": "kuota." },
  { "en": "radar?", "id": "radar." },
  { "en": "radical?", "id": "radikal." },
  { "en": "rage?", "id": "amarah." },
  { "en": "raid?", "id": "serangan, menyerbu." },
  { "en": "rally?", "id": "rapat umum, berkumpul." },
  { "en": "ranking?", "id": "peringkat." },
  { "en": "rape?", "id": "pemerkosaan, memperkosa." },
  { "en": "ratio?", "id": "rasio." },
  { "en": "rational?", "id": "rasional." },
  { "en": "ray?", "id": "sinar." },
  { "en": "readily?", "id": "dengan mudah." },
  { "en": "realization?", "id": "kesadaran." },
  { "en": "realm?", "id": "ranah." },
  { "en": "rear?", "id": "belakang." },
  { "en": "reasoning?", "id": "penalaran." },
  { "en": "reassure?", "id": "meyakinkan." },
  { "en": "rebel?", "id": "pemberontak." },
  { "en": "rebellion?", "id": "pemberontakan." },
  { "en": "recipient?", "id": "penerima." },
  { "en": "reconstruction?", "id": "rekonstruksi." },
  { "en": "recount'?", "id": "menceritakan kembali." },
  { "en": "recruitment?", "id": "prekrutmen." },
  { "en": "referendum?", "id": "referendum." },
  { "en": "reflection?", "id": "refleksi." },
  { "en": "reform?", "id": "reformasi, mereformasi." },
  { "en": "refuge?", "id": "perlindungan." },
  { "en": "refusal?", "id": "penolakan." },
  { "en": "regain?", "id": "mendapatkan kembali." },
  { "en": "regardless?", "id": "tanpa memandang." },
  { "en": "regime?", "id": "rezim." },
  { "en": "regulator?", "id": "regulator." },
  { "en": "regulatory?", "id": "pengaturan." },
  { "en": "rehabilitation?", "id": "rehabilitasi." },
  { "en": "reign?", "id": "pemerintahan, memerintah." },
  { "en": "rejection?", "id": "penolakan." },
  { "en": "relevance?", "id": "relevansi." },
  { "en": "reliability?", "id": "keandalan." },
  { "en": "reluctant?", "id": "enggan." },
  { "en": "remainder?", "id": "sisa." },
  { "en": "remains?", "id": "sisa-sisa." },
  { "en": "remedy?", "id": "obat." },
  { "en": "reminder?", "id": "pengingat." },
  { "en": "removal?", "id": "penghapusan." },
  { "en": "render?", "id": "memberikan." },
  { "en": "renew?", "id": "memperbarui." },
  { "en": "renowned?", "id": "terkenal." },
  { "en": "replacement?", "id": "penggantian." },
  { "en": "reportedly?", "id": "dilaporkan." },
  { "en": "representation?", "id": "representasi." },
  { "en": "reproduce?", "id": "bereproduksi." },
  { "en": "reproduction?", "id": "reproduksi." },
  { "en": "republic?", "id": "republik." },
  { "en": "resemble?", "id": "menyerupai." },
  { "en": "reside?", "id": "tinggal." },
  { "en": "residence?", "id": "tempat tinggal." },
  { "en": "residential?", "id": "perumahan." },
  { "en": "residue?", "id": "residu." },
  { "en": "resignation?", "id": "pengunduran diri." },
  { "en": "resistance?", "id": "perlawanan." },
  { "en": "respective?", "id": "masing-masing." },
  { "en": "respectively?", "id": "secara berturut-turut." },
  { "en": "restoration?", "id": "restorasi." },
  { "en": "restraint?", "id": "pengekangan." },
  { "en": "resume?", "id": "melanjutkan." },
  { "en": "retreat?", "id": "mundur, retret." },
  { "en": "retrieve?", "id": "mengambil kembali." },
  { "en": "revelation?", "id": "wahyu." },
  { "en": "revenge?", "id": "balas dendam." },
  { "en": "reverse?", "id": "membalik, kebalikan." },
  { "en": "revival?", "id": "kebangkitan kembali." },
  { "en": "revive?", "id": "menghidupkan kembali." },
  { "en": "revolutionary?", "id": "revolusioner." },
  { "en": "rhetoric?", "id": "retorika." },
  { "en": "rifle?", "id": "senapan." },
  { "en": "riot?", "id": "kerusuhan." },
  { "en": "rip?", "id": "merobek." },
  { "en": "ritual?", "id": "ritual." },
  { "en": "robust?", "id": "kuat." },
  { "en": "rock?", "id": "mengayun." },
  { "en": "rod?", "id": "batang." },
  { "en": "rookie?", "id": "pemula." },
  { "en": "roster?", "id": "daftar nama." },
  { "en": "rotate?", "id": "berputar." },
  { "en": "rotation?", "id": "rotasi." },
  { "en": "ruling?", "id": "keputusan." },
  { "en": "rumor?", "id": "rumor." },
  { "en": "sacred?", "id": "suci." },
  { "en": "sacrifice?", "id": "pengorbanan, berkorban." },
  { "en": "saint?", "id": "santo." },
  { "en": "sake?", "id": "demi." },
  { "en": "sanction?", "id": "sanksi." },
  { "en": "say?", "id": "pendapat." },
  { "en": "scattered?", "id": "tersebar." },
  { "en": "scope?", "id": "lingkup." },
  { "en": "screw?", "id": "sekrup, mengacaukan." },
  { "en": "scrutiny?", "id": "pengawasan ketat." },
  { "en": "seal?", "id": "menyegel, segel." },
  { "en": "secondly?", "id": "kedua." },
  { "en": "secular?", "id": "sekuler." },
  { "en": "seemingly?", "id": "kelihatannya." },
  { "en": "segment?", "id": "segmen." },
  { "en": "seize?", "id": "menyita." },
  { "en": "seldom?", "id": "jarang." },
  { "en": "selective?", "id": "selektif." },
  { "en": "sensation?", "id": "sensasi." },
  { "en": "sensitivity?", "id": "sensitivitas." },
  { "en": "sentiment?", "id": "sentimen." },
  { "en": "separation?", "id": "pemisahan." },
  { "en": "serial?", "id": "serial." },
  { "en": "settlement?", "id": "penyelesaian." },
  { "en": "setup?", "id": "pengaturan." },
  { "en": "sexuality?", "id": "seksualitas." },
  { "en": "shareholder?", "id": "pemegang saham." },
  { "en": "shatter?", "id": "menghancurkan." },
  { "en": "shed?", "id": "menumpahkan." },
  { "en": "sheer?", "id": "murni." },
  { "en": "shipping?", "id": "pengiriman." },
  { "en": "shoot?", "id": "tunas." },
  { "en": "shrink?", "id": "menyusut." },
  { "en": "shrug?", "id": "mengangkat bahu." },
  { "en": "sigh?", "id": "menghela napas." },
  { "en": "simulate?", "id": "mensimulasikan." },
  { "en": "simulation?", "id": "simulasi." },
  { "en": "simultaneously?", "id": "scr bersamaan." },
  { "en": "sin?", "id": "dosa." },
  { "en": "situated?", "id": "terletak." },
  { "en": "skeptical?", "id": "skeptis." },
  { "en": "sketch?", "id": "sketsa." },
  { "en": "skip?", "id": "melewatkan." },
  { "en": "slam?", "id": "membanting." },
  { "en": "slap?", "id": "menampar." },
  { "en": "slash?", "id": "memotong." },
  { "en": "slavery?", "id": "perbudakan." },
  { "en": "slot?", "id": "slot." },
  { "en": "smash?", "id": "menghancurkan." },
  { "en": "snap?", "id": "mematahkan." },
  { "en": "soak?", "id": "merendam." },
  { "en": "soar?", "id": "melambung." },
  { "en": "socialist?", "id": "sosialis." },
  { "en": "sole?", "id": "tunggal." },
  { "en": "solely?", "id": "semata-mata." },
  { "en": "solidarity?", "id": "solidaritas." },
  { "en": "solo?", "id": "solo." },
  { "en": "sophomore?", "id": "mahasiswa tingkat dua." },
  { "en": "sound?", "id": "sehat." },
  { "en": "sovereignty?", "id": "kedaulatan." },
  { "en": "spam?", "id": "spam." },
  { "en": "span?", "id": "menjangkau, rentang." },
  { "en": "spare (v)?", "id": "menghemat." },
  { "en": "spark?", "id": "memicu." },
  { "en": "specialized?", "id": "khusus." },
  { "en": "specification?", "id": "spesifikasi." },
  { "en": "specimen?", "id": "spesimen." },
  { "en": "spectacle?", "id": "tontonan." },
  { "en": "spectrum?", "id": "spektrum." },
  { "en": "spell?", "id": "mantra." },
  { "en": "sphere?", "id": "bola." },
  { "en": "spin?", "id": "berputar, putaran." },
  { "en": "spine?", "id": "tulang belakang." },
  { "en": "spotlight?", "id": "sorotan." },
  { "en": "spouse?", "id": "pasangan." },
  { "en": "spy?", "id": "mata-mata, memata-matai." },
  { "en": "squad?", "id": "regu." },
  { "en": "squeeze?", "id": "memeras." },
  { "en": "stab?", "id": "menusuk." },
  { "en": "stability?", "id": "stabilitas." },
  { "en": "stabilize?", "id": "menstabilkan." },
  { "en": "stake?", "id": "taruhan." },
  { "en": "standing?", "id": "berdiri." },
  { "en": "stark?", "id": "tajam." },
  { "en": "statistical?", "id": "statistik." },
  { "en": "steer?", "id": "mengarahkan." },
  { "en": "stem?", "id": "batang, berasal." },
  { "en": "stereotype?", "id": "stereotip." },
  { "en": "stimulus?", "id": "stimulus." },
  { "en": "stir?", "id": "mengaduk." },
  { "en": "storage?", "id": "penyimpanan." },
  { "en": "straightforward?", "id": "lugas." },
  { "en": "strain?", "id": "ketegangan." },
  { "en": "strand?", "id": "untaian." },
  { "en": "strategic?", "id": "strategis." },
  { "en": "striking?", "id": "mencolok." },
  { "en": "strip (long narrow piece)?", "id": "strip." },
  { "en": "strive?", "id": "berusaha keras." },
  { "en": "structural?", "id": "struktural." },
  { "en": "stumble?", "id": "tersandung." },
  { "en": "stun?", "id": "mengejutkan." },
  { "en": "submission?", "id": "pengajuan." },
  { "en": "subscriber?", "id": "pelanggan." },
  { "en": "subscription?", "id": "langganan." },
  { "en": "subsidy?", "id": "subsidi." },
  { "en": "substantial?", "id": "substansial." },
  { "en": "substantially?", "id": "secara substansial." },
  { "en": "substitute?", "id": "pengganti, menggantikan." },
  { "en": "substitution?", "id": "substitusi." },
  { "en": "subtle?", "id": "halus." },
  { "en": "suburban?", "id": "pinggiran kota." },
  { "en": "succession?", "id": "suksesi." },
  { "en": "successive?", "id": "berturut-turut." },
  { "en": "successor?", "id": "berturut-turut." },
  { "en": "suck?", "id": "mengisap." },
  { "en": "sue?", "id": "menggugat." },
  { "en": "suicide?", "id": "bunuh diri." },
  { "en": "suite?", "id": "suite." },
  { "en": "summit?", "id": "puncak." },
  { "en": "superb?", "id": "luar biasa." },
  { "en": "superintendent?", "id": "pengawas." },
  { "en": "superior?", "id": "unggul." },
  { "en": "supervise?", "id": "mengawasi." },
  { "en": "supervision?", "id": "pengawasan." },
  { "en": "supervisor?", "id": "penyelia." },
  { "en": "supplement?", "id": "suplemen, melengkapi." },
  { "en": "supportive?", "id": "mendukung." },
  { "en": "supposedly?", "id": "konon." },
  { "en": "suppress?", "id": "menekan." },
  { "en": "supreme?", "id": "tertinggi." },
  { "en": "surge?", "id": "lonjakan, melonjak." },
  { "en": "surgical?", "id": "bedah." },
  { "en": "surplus?", "id": "surplus." },
  { "en": "surrender?", "id": "menyerah." },
  { "en": "surveillance?", "id": "pengawasan." },
  { "en": "suspension?", "id": "penangguhan." },
  { "en": "suspicion?", "id": "kecurigaan." },
  { "en": "suspicious?", "id": "mencurigakan." },
  { "en": "sustain?", "id": "mempertahankan." },
  { "en": "swing?", "id": "berayun, ayunan." },
  { "en": "sword?", "id": "pedang." },
  { "en": "symbolic?", "id": "simbolis." },
  { "en": "syndrome?", "id": "sindrom." },
  { "en": "synthesis?", "id": "sintesis." },
  { "en": "systematic?", "id": "sistematis." },
  { "en": "tackle (n)?", "id": "menangani." },
  { "en": "tactic?", "id": "taktik." },
  { "en": "tactical?", "id": "taktis." },
  { "en": "taxpayer?", "id": "pembayar pajak." },
  { "en": "tempt?", "id": "menggoda." },
  { "en": "tenant?", "id": "penyewa." },
  { "en": "tender?", "id": "lembut." },
  { "en": "tenure?", "id": "masa jabatan." },
  { "en": "terminal (adj)?", "id": "terminal." },
  { "en": "terminate?", "id": "mengakhiri." },
  { "en": "terrain?", "id": "medan." },
  { "en": "terrific?", "id": "hebat." },
  { "en": "testify?", "id": "bersaksi." },
  { "en": "testimony?", "id": "kesaksian." },
  { "en": "texture?", "id": "tekstur." },
  { "en": "thankfully?", "id": "untungnya." },
  { "en": "theatrical?", "id": "teatrikal." },
  { "en": "theology?", "id": "teologi." },
  { "en": "theoretical?", "id": "teoretis." },
  { "en": "thereafter?", "id": "setelah itu." },
  { "en": "thereby?", "id": "dengan demikian." },
  { "en": "thoughtful?", "id": "bijaksana." },
  { "en": "thought-provoking?", "id": "memprovokasi pemikiran." },
  { "en": "thread?", "id": "benang." },
  { "en": "threshold?", "id": "ambang." },
  { "en": "thrilled?", "id": "sangat senang." },
  { "en": "thrive?", "id": "berkembang." },
  { "en": "tide?", "id": "pasang." },
  { "en": "tighten?", "id": "mengencangkan." },
  { "en": "timber?", "id": "kayu." },
  { "en": "timely?", "id": "tepat waktu." },
  { "en": "tobacco?", "id": "tembakau." },
  { "en": "tolerance?", "id": "toleransi." },
  { "en": "tolerate?", "id": "mentolerir." },
  { "en": "toll?", "id": "tol." },
  { "en": "top?", "id": "mengungguli." },
  { "en": "torture?", "id": "penyiksaan, menyiksa." },
  { "en": "toss?", "id": "melempar." },
  { "en": "total (v)?", "id": "total." },
  { "en": "toxic?", "id": "beracun." },
  { "en": "trace (n)?", "id": "jejak." },
  { "en": "trademark?", "id": "merek dagang." },
  { "en": "trail?", "id": "jejak, melacak." },
  { "en": "trailer?", "id": "trailer." },
  { "en": "transaction?", "id": "transaksi." },
  { "en": "transcript?", "id": "transkrip." },
  { "en": "transformation?", "id": "transformasi." },
  { "en": "transit?", "id": "transit." },
  { "en": "transmission?", "id": "transmisi." },
  { "en": "transparency?", "id": "transparansi." },
  { "en": "transparent?", "id": "transparan." },
  { "en": "trauma?", "id": "trauma." },
  { "en": "treaty?", "id": "perjanjian." },
  { "en": "tremendous?", "id": "luar biasa." },
  { "en": "tribal?", "id": "kesukuan." },
  { "en": "tribute?", "id": "penghormatan." },
  { "en": "trigger (n)?", "id": "pemicu." },
  { "en": "trio?", "id": "trio." },
  { "en": "triumph?", "id": "kemenangan." },
  { "en": "trophy?", "id": "trofi." },
  { "en": "troubled?", "id": "bermasalah." },
  { "en": "trustee?", "id": "wali." },
  { "en": "tuition?", "id": "biaya kuliah." },
  { "en": "tumor?", "id": "tumor." },
  { "en": "turnout?", "id": "kehadiran." },
  { "en": "turnover?", "id": "pergantian." },
  { "en": "twist?", "id": "memutar, putaran." },
  { "en": "unconstitutional?", "id": "inkonstitusional." },
  { "en": "undergraduate?", "id": "sarjana." },
  { "en": "underlying?", "id": "mendasari." },
  { "en": "undermine?", "id": "merusak." },
  { "en": "undoubtedly?", "id": "tidak diragukan." },
  { "en": "unify?", "id": "menyatukan." },
  { "en": "unprecedented?", "id": "belum pernah terjadi." },
  { "en": "unveil?", "id": "mengungkap." },
  { "en": "upcoming?", "id": "mendatang." },
  { "en": "upgrade?", "id": "meningkatkan, peningkatan." },
  { "en": "uphold?", "id": "menegakkan." },
  { "en": "utility?", "id": "utilitas." },
  { "en": "utilize?", "id": "memanfaatkan." },
  { "en": "utterly?", "id": "sepenuhnya." },
  { "en": "vacuum?", "id": "vakum." },
  { "en": "vague?", "id": "samar." },
  { "en": "validity?", "id": "validitas." },
  { "en": "vanish?", "id": "menghilang." },
  { "en": "variable?", "id": "variabel." },
  { "en": "varied?", "id": "beragam." },
  { "en": "vein?", "id": "pembuluh darah." },
  { "en": "venture?", "id": "usaha, berusaha." },
  { "en": "verbal?", "id": "lisan." },
  { "en": "verdict?", "id": "putusan." },
  { "en": "verify?", "id": "memverifikasi." },
  { "en": "verse?", "id": "ayat." },
  { "en": "versus?", "id": "versus." },
  { "en": "veteran?", "id": "veteran." },
  { "en": "viable?", "id": "layak." },
  { "en": "vibrant?", "id": "bersemangat." },
  { "en": "vice?", "id": "keburukan." },
  { "en": "vicious?", "id": "kejam." },
  { "en": "violate?", "id": "melanggar." },
  { "en": "violation?", "id": "pelanggaran." },
  { "en": "virtue?", "id": "kebajikan." },
  { "en": "vocal?", "id": "vokal." },
  { "en": "vow?", "id": "bersumpah." },
  { "en": "vulnerability?", "id": "kerentanan." },
  { "en": "vulnerable?", "id": "rentan." },
  { "en": "ward?", "id": "bangsal." },
  { "en": "warehouse?", "id": "gudang." },
  { "en": "warfare?", "id": "peperangan." },
  { "en": "warrant?", "id": "surat perintah, menjamin." },
  { "en": "warrior?", "id": "pejuang." },
  { "en": "weaken?", "id": "melemahkan." },
  { "en": "weave?", "id": "menenun." },
  { "en": "weed?", "id": "gulma." },
  { "en": "well?", "id": "sumur." },
  { "en": "well-being?", "id": "kesejahteraan." },
  { "en": "whatsoever?", "id": "sama sekali." },
  { "en": "whereby?", "id": "dengan mana." },
  { "en": "whip?", "id": "mencambuk." },
  { "en": "wholly?", "id": "sepenuhnya." },
  { "en": "widen?", "id": "melebarkan." },
  { "en": "widow?", "id": "janda." },
  { "en": "width?", "id": "lebar." },
  { "en": "willingness?", "id": "kesediaan." },
  { "en": "wipe?", "id": "menyeka." },
  { "en": "wit?", "id": "kecerdasan." },
  { "en": "withdrawal?", "id": "penarikan." },
  { "en": "workout?", "id": "latihan." },
  { "en": "worship?", "id": "pemujaan, memuja." },
  { "en": "worthwhile?", "id": "berharga." },
  { "en": "worthy?", "id": "layak." },
  { "en": "yell?", "id": "berteriak." },
  { "en": "yield?", "id": "hasil, menghasilkan." },



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
            }, 6000);
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
