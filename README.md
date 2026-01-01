<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Boycott Check Pro 🇵🇸</title>
    
    <script src="https://cdn.jsdelivr.net/npm/@ericblade/quagga2@1.4.2/dist/quagga.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap" rel="stylesheet">
    
    <style>
        :root { --bg: #f8fafc; --card: #ffffff; --text: #0f172a; --accent: #059669; --danger: #dc2626; --border: #e2e8f0; --input: #ffffff; }
        .dark-mode { --bg: #000000; --card: #121212; --text: #f1f5f9; --accent: #10b981; --danger: #ef4444; --border: #262626; --input: #121212; }

        body { font-family: 'Inter', sans-serif; background: var(--bg); color: var(--text); margin: 0; transition: background 0.3s; padding-bottom: 50px; min-height: 100dvh; }
        .container { max-width: 550px; margin: 0 auto; padding: 15px; }

        .top-nav { display: flex; justify-content: space-between; align-items: center; padding: 10px 0 20px; }
        select, .theme-btn { background: var(--card); border: 1px solid var(--border); color: var(--text); padding: 12px; border-radius: 14px; font-weight: 600; cursor: pointer; }

        header { text-align: center; margin-bottom: 25px; }
        h1 { font-size: 1.8rem; font-weight: 900; margin: 0; letter-spacing: -1px; }

        .scan-btn { background: var(--text); color: var(--bg); border: none; padding: 20px; border-radius: 20px; width: 100%; font-weight: 800; font-size: 1rem; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 10px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        #scanner-container { width: 100%; height: 250px; background: #000; border-radius: 24px; overflow: hidden; display: none; margin-top: 15px; border: 3px solid var(--accent); }
        #scanner-container video { width: 100%; height: 100%; object-fit: cover; }

        input { width: 100%; padding: 18px; border-radius: 18px; border: 2px solid var(--border); background: var(--input); color: var(--text); font-size: 1rem; box-sizing: border-box; outline: none; margin: 20px 0 10px; font-weight: 500; }

        #no-result { display: none; text-align: center; padding: 30px 20px; background: var(--card); border-radius: 20px; border: 2px dashed var(--border); margin-top: 20px; }
        .success-icon { font-size: 2rem; display: block; margin-bottom: 5px; }

        .cat-title { font-size: 0.75rem; font-weight: 800; color: #94a3b8; text-transform: uppercase; letter-spacing: 1.5px; margin: 30px 0 12px; display: flex; align-items: center; gap: 10px; }
        .cat-title::after { content: ""; flex: 1; height: 1px; background: var(--border); }
        
        .brand-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(100px, 1fr)); gap: 10px; }
        .brand-card { background: var(--card); border: 1px solid var(--border); padding: 12px 8px; border-radius: 14px; text-align: center; font-size: 0.75rem; font-weight: 700; cursor: pointer; transition: 0.2s; min-height: 45px; display: flex; align-items: center; justify-content: center; line-height: 1.2; }
        .brand-card:active { background: var(--border); transform: scale(0.96); }

        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.7); backdrop-filter: blur(10px); z-index: 2000; align-items: flex-end; }
        .modal-content { background: var(--card); width: 100%; padding: 40px 25px; border-top-left-radius: 35px; border-top-right-radius: 35px; animation: slideUp 0.3s ease-out; box-sizing: border-box; max-height: 85vh; overflow-y: auto; }
        @keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
        
        .modal-title { font-size: 2.2rem; font-weight: 900; color: var(--danger); margin: 0; }
        .cause-box { background: rgba(220, 38, 38, 0.05); border-left: 4px solid var(--danger); padding: 15px; margin: 20px 0; font-size: 0.9rem; line-height: 1.5; color: var(--text); }
        .alt-tag { background: rgba(5, 150, 105, 0.1); color: var(--accent); padding: 15px; border-radius: 15px; font-weight: 800; text-align: center; border: 2px dashed var(--accent); margin-top: 10px; }
        .close-btn { width: 100%; padding: 18px; background: var(--text); color: var(--bg); border: none; border-radius: 18px; font-weight: 900; margin-top: 20px; cursor: pointer; }
        .rtl { direction: rtl; text-align: right; }
    </style>
</head>
<body>

<div class="container">
    <div class="top-nav">
        <select id="langSelect" onchange="updateLang()">
            <option value="fr">🇫🇷 Français</option>
            <option value="en">🇺🇸 English</option>
            <option value="ar">🇵🇸 العربية</option>
        </select>
        <button class="theme-btn" onclick="toggleTheme()" id="themeIcon">🌙</button>
    </div>

    <header><h1 id="t-title">BOYCOTT CHECK</h1></header>

    <div class="scan-wrapper">
        <button class="scan-btn" onclick="toggleScanner()" id="t-scan-btn">📷 SCANNER LE PRODUIT</button>
        <div id="scanner-container"></div>
    </div>

    <input type="text" id="marqueInput" placeholder="Recherche une marque, catégorie..." onkeyup="handleSearch()">

    <div id="no-result">
        <span class="success-icon">✅</span>
        <strong id="t-notfound">Marque non trouvée ou à ne pas boycotter</strong>
    </div>

    <div id="brandsList"></div>
</div>

<div id="modal" class="modal" onclick="closeModal()">
    <div class="modal-content" onclick="event.stopPropagation()">
        <div id="m-title" class="modal-title"></div>
        <div id="m-reason" class="cause-box"></div>
        <div style="font-weight:800; font-size:0.7rem; color:#94a3b8; text-transform: uppercase;">Alternatives conseillées</div>
        <div id="m-alt" class="alt-tag"></div>
        <button class="close-btn" onclick="closeModal()" id="t-close">FERMER</button>
    </div>
</div>

<script>
    let lang = 'fr';
    
    // Raisons génériques par groupe pour éviter les répétitions dans le code
    const r = {
        coca: "Propriété de Coca-Cola, qui exploite des usines dans les colonies illégales d'Atarot.",
        pepsi: "Propriété de PepsiCo, détenteur de SodaStream et investisseur majeur en territoire occupé.",
        nestle: "Le groupe Nestlé détient plus de 50% d'Osem, une entreprise pilier de l'économie israélienne.",
        unilever: "Unilever opère massivement en Israël et a maintenu ses activités dans les colonies malgré les alertes.",
        pg: "Procter & Gamble finance des centres de recherche et développement majeurs à Tel-Aviv.",
        danone: "Danone détient des parts directes dans Strauss Group, géant alimentaire de l'occupation.",
        mondelez: "Mondelez (LU, Oreo) investit dans des fonds technologiques soutenant l'économie de l'occupation.",
        loreal: "L'Oréal possède des usines sur place et utilise des ressources naturelles de la Mer Morte sous contrôle israélien.",
        mars: "Mars Inc finance des incubateurs FoodTech en partenariat avec l'État d'Israël.",
        ferrero: "Ferrero maintient des liens financiers via des fonds d'investissement pro-occupation."
    };

    const brands = [
        // BOISSONS
        { name: "Coca-Cola", cat: "drink", alt: "Selecto, Hamoud", reason: r.coca },
        { name: "Fanta", cat: "drink", alt: "Jus de fruits", reason: r.coca },
        { name: "Sprite", cat: "drink", alt: "Limonade artisanale", reason: r.coca },
        { name: "Minute Maid", cat: "drink", alt: "Jus pressés", reason: r.coca },
        { name: "Fuze Tea", cat: "drink", alt: "Ifri Ice Tea", reason: r.coca },
        { name: "Tropico", cat: "drink", alt: "Boissons locales", reason: r.coca },
        { name: "Pepsi", cat: "drink", alt: "Slim, Vimto", reason: r.pepsi },
        { name: "Seven Up", cat: "drink", alt: "Limonades", reason: r.pepsi },
        { name: "Mirinda", cat: "drink", alt: "Jus gazeux", reason: r.pepsi },
        { name: "Lipton", cat: "drink", alt: "Thé local", reason: r.pepsi },
        { name: "SodaStream", cat: "drink", alt: "Eau gazeuse", reason: r.pepsi },
        { name: "Evian", cat: "drink", alt: "Ifri, Saida", reason: r.danone },
        { name: "Volvic", cat: "drink", alt: "Eaux minérales", reason: r.danone },
        { name: "Badoit", cat: "drink", alt: "Eaux gazeuses locales", reason: r.danone },
        { name: "Perrier", cat: "drink", alt: "Eau pétillante", reason: r.nestle },
        { name: "Vittel", cat: "drink", alt: "Eaux de source", reason: r.nestle },
        { name: "Nespresso", cat: "drink", alt: "Café en grain", reason: r.nestle },
        { name: "Nescafé", cat: "drink", alt: "Café soluble local", reason: r.nestle },
        { name: "Nesquik", cat: "drink", alt: "Cacao local", reason: r.nestle },
        
        // ALIMENTATION
        { name: "Nutella", cat: "food", alt: "El Mordjene, Maxon", reason: r.ferrero },
        { name: "Kinder", cat: "food", alt: "Chocolat local", reason: r.ferrero },
        { name: "Ferrero Rocher", cat: "food", alt: "Artisanat", reason: r.ferrero },
        { name: "Oreo", cat: "food", alt: "Biscuits locaux", reason: r.mondelez },
        { name: "Milka", cat: "food", alt: "Chocolat Bio", reason: r.mondelez },
        { name: "LU", cat: "food", alt: "Bifa, Biscuits locaux", reason: r.mondelez },
        { name: "Prince", cat: "food", alt: "Biscuits fourrés", reason: r.mondelez },
        { name: "Mikado", cat: "food", alt: "Biscuits", reason: r.mondelez },
        { name: "Toblerone", cat: "food", alt: "Chocolat suisse", reason: r.mondelez },
        { name: "KitKat", cat: "food", alt: "Biscuits", reason: r.nestle },
        { name: "Lion", cat: "food", alt: "Céréales locales", reason: r.nestle },
        { name: "Smarties", cat: "food", alt: "Confiserie", reason: r.nestle },
        { name: "Maggi", cat: "food", alt: "Épices locales", reason: r.nestle },
        { name: "Knorr", cat: "food", alt: "Bouillons locaux", reason: r.unilever },
        { name: "Amora", cat: "food", alt: "Condiments", reason: r.unilever },
        { name: "Maille", cat: "food", alt: "Moutarde", reason: r.unilever },
        { name: "Lay's", cat: "food", alt: "Chips locales", reason: r.pepsi },
        { name: "Doritos", cat: "food", alt: "Snacks", reason: r.pepsi },
        { name: "Pringles", cat: "food", alt: "Chips", reason: "Groupe Kellogg's, investisseur technologique en Israël." },
        { name: "Kellogg's", cat: "food", alt: "Céréales", reason: "Soutien technologique direct à l'économie de l'occupation." },
        { name: "M&Ms", cat: "food", alt: "Dragées", reason: r.mars },
        { name: "Snickers", cat: "food", alt: "Barres chocolatées", reason: r.mars },
        { name: "Twix", cat: "food", alt: "Barres", reason: r.mars },
        { name: "Danone", cat: "food", alt: "Soummam, Délice", reason: r.danone },
        { name: "Activia", cat: "food", alt: "Yaourt local", reason: r.danone },
        
        // HYGIENE
        { name: "Dove", cat: "beauty", alt: "Savons locaux, Bio", reason: r.unilever },
        { name: "Axe", cat: "beauty", alt: "Déodorants Bio", reason: r.unilever },
        { name: "Rexona", cat: "beauty", alt: "Hygiène", reason: r.unilever },
        { name: "Signal", cat: "beauty", alt: "Miswak", reason: r.unilever },
        { name: "Ariel", cat: "beauty", alt: "Lessives locales", reason: r.pg },
        { name: "Dash", cat: "beauty", alt: "Lessive", reason: r.pg },
        { name: "Always", cat: "beauty", alt: "Hygiène féminine", reason: r.pg },
        { name: "Pampers", cat: "beauty", alt: "Couches locales", reason: r.pg },
        { name: "Gillette", cat: "beauty", alt: "Rasoirs", reason: r.pg },
        { name: "Oral-B", cat: "beauty", alt: "Brosses à dents", reason: r.pg },
        { name: "Head & Shoulders", cat: "beauty", alt: "Shampooing local", reason: r.pg },
        { name: "Pantene", cat: "beauty", alt: "Soins cheveux", reason: r.pg },
        { name: "Garnier", cat: "beauty", alt: "Cosmétique Bio", reason: r.loreal },
        { name: "Maybelline", cat: "beauty", alt: "Maquillage local", reason: r.loreal },
        { name: "Vichy", cat: "beauty", alt: "Soins pharmacie", reason: r.loreal },
        { name: "La Roche Posay", cat: "beauty", alt: "Soins", reason: r.loreal },
        { name: "Johnson's", cat: "beauty", alt: "Soins bébé", reason: "Soutien historique et financier de longue date à Israël." },
        { name: "Neutrogena", cat: "beauty", alt: "Nettoyants", reason: "Groupe Johnson & Johnson." },
        
        // TECH & MODE & RESTO
        { name: "McDonald's", cat: "tech", alt: "Restaurants locaux", reason: "Repas gratuits à l'armée d'occupation." },
        { name: "Burger King", cat: "tech", alt: "Fast-food", reason: "Soutien logistique aux unités militaires." },
        { name: "Starbucks", cat: "tech", alt: "Cafés indépendants", reason: "Hostilité envers les syndicats pro-Palestine." },
        { name: "Carrefour", cat: "tech", alt: "Épiceries locales", reason: "Magasins ouverts dans les colonies illégales." },
        { name: "Zara", cat: "tech", alt: "Mode locale", reason: "Soutien politique et campagnes offensantes." },
        { name: "Puma", cat: "tech", alt: "Adidas, Decathlon", reason: "Sponsor officiel de la Fédération israélienne." },
        { name: "HP", cat: "tech", alt: "Asus, Lenovo", reason: "Systèmes biométriques aux checkpoints de sécurité." },
        { name: "Apple", cat: "tech", alt: "Android", reason: "Investissements technologiques massifs en Israël (Apple Silicon)." },
        { name: "Intel", cat: "tech", alt: "AMD", reason: "Usines géantes situées sur des terres occupées." },
        { name: "Bershka", cat: "tech", alt: "Vêtements", reason: "Groupe Inditex (Zara)." },
        { name: "Stradivarius", cat: "tech", alt: "Mode", reason: "Groupe Inditex." },
        { name: "Pull & Bear", cat: "tech", alt: "Mode", reason: "Groupe Inditex." },
        { name: "Airbnb", cat: "tech", alt: "Hôtels", reason: "Location de logements dans les colonies." },
        { name: "Booking.com", cat: "tech", alt: "Hôtels directs", reason: "Référencement des colonies illégales." },
        { name: "Amazon", cat: "tech", alt: "Commerces locaux", reason: "Contrat Nimbus (Cloud) avec le gouvernement israélien." },
        { name: "Google", cat: "tech", alt: "Alternatives open-source", reason: "Projet Nimbus fournissant des services à l'armée." },
        { name: "Wix", cat: "tech", alt: "WordPress", reason: "Entreprise 100% israélienne soutenant l'effort de guerre." },
        { name: "Disney", cat: "tech", alt: "Cinéma indépendant", reason: "Dons massifs aux services d'urgence israéliens." }
        // Note: La liste peut être étendue jusqu'à 166 marques ici de la même manière
    ];

    const texts = {
        fr: { title: "BOYCOTT CHECK 🇵🇸", scan: "📷 SCANNER LE PRODUIT", search: "Recherche une marque, catégorie...", cats: { drink: "Boissons", food: "Alimentation", beauty: "Hygiène", tech: "Restauration, Mode & Tech" }, close: "FERMER", notfound: "Marque non trouvée ou à ne pas boycotter" },
        en: { title: "BOYCOTT CHECK 🇵🇸", scan: "📷 SCAN PRODUCT", search: "Search brand, category...", cats: { drink: "Drinks", food: "Food", beauty: "Hygiene", tech: "Dining & Tech" }, close: "CLOSE", notfound: "Brand not found or not for boycott" },
        ar: { title: "مدقق المقاطعة 🇵🇸", scan: "📷 مسح المنتج", search: "ابحث عن علامة، فئة...", cats: { drink: "مشروبات", food: "أغذية", beauty: "نظافة", tech: "تكنولوجيا ومطاعم" }, close: "إغلاق", notfound: "الماركة غير موجودة أو ليست للمقاطعة" }
    };

    function updateLang() {
        lang = document.getElementById('langSelect').value;
        document.getElementById('t-title').innerText = texts[lang].title;
        document.getElementById('t-scan-btn').innerText = texts[lang].scan;
        document.getElementById('marqueInput').placeholder = texts[lang].search;
        document.getElementById('t-close').innerText = texts[lang].close;
        document.getElementById('t-notfound').innerText = texts[lang].notfound;
        document.body.classList.toggle('rtl', lang === 'ar');
        renderBrands();
    }

    function renderBrands(filter = "") {
        const container = document.getElementById('brandsList');
        const noResult = document.getElementById('no-result');
        container.innerHTML = '';
        const query = filter.toLowerCase().trim().normalize("NFD").replace(/[\u0300-\u036f]/g, "");
        
        let foundAny = false;

        ['drink', 'food', 'beauty', 'tech'].forEach(c => {
            const catLabel = texts[lang].cats[c].toLowerCase();
            const filtered = brands.filter(b => {
                if (b.cat !== c) return false;
                const bName = b.name.toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g, "");
                if (catLabel.includes(query) && query.length > 3) return true;
                return bName.includes(query);
            });

            if (filtered.length > 0) {
                foundAny = true;
                let html = `<div class="cat-title">${texts[lang].cats[c]}</div><div class="brand-grid">`;
                filtered.sort((x,y)=>x.name.localeCompare(y.name)).forEach(b => {
                    html += `<div class="brand-card" onclick="openModal('${b.name.replace(/'/g, "\\'")}')">${b.name}</div>`;
                });
                container.innerHTML += html + `</div>`;
            }
        });

        noResult.style.display = (!foundAny && query.length > 0) ? 'block' : 'none';
    }

    function handleSearch() { renderBrands(document.getElementById('marqueInput').value); }

    function openModal(name) {
        if(navigator.vibrate) navigator.vibrate(20);
        const b = brands.find(x => x.name === name);
        document.getElementById('m-title').innerText = b.name;
        document.getElementById('m-reason').innerText = b.reason;
        document.getElementById('m-alt').innerText = b.alt;
        document.getElementById('modal').style.display = 'flex';
    }

    function closeModal() { document.getElementById('modal').style.display = 'none'; }
    function toggleTheme() { document.body.classList.toggle('dark-mode'); }

    let scannerOn = false;
    function toggleScanner() {
        const box = document.getElementById('scanner-container');
        if (!scannerOn) {
            box.style.display = 'block';
            Quagga.init({ 
                inputStream: { name: "Live", type: "LiveStream", target: box, constraints: { facingMode: "environment" } },
                decoder: { readers: ["ean_reader"] }
            }, (err) => { if(!err) { Quagga.start(); scannerOn = true; } });
        } else { Quagga.stop(); box.style.display = 'none'; scannerOn = false; }
    }

    Quagga.onDetected((data) => {
        toggleScanner();
        checkProduct(data.codeResult.code);
    });

    async function checkProduct(code) {
        if (code.startsWith("729")) { alert("🛑 ORIGINE ISRAËL (729)"); return; }
        try {
            const res = await fetch(`https://world.openfoodfacts.org/api/v0/product/${code}.json`);
            const data = await res.json();
            if (data.status === 1) {
                const bName = (data.product.brands || "").toLowerCase();
                const match = brands.find(b => bName.includes(b.name.toLowerCase()));
                if (match) openModal(match.name);
                else alert(texts[lang].notfound + " ✅");
            } else { alert(texts[lang].notfound); }
        } catch (e) { alert("Erreur réseau"); }
    }

    updateLang();
</script>
</body>
</html>
