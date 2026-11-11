# AccessBoard-Klaviatura-Navigatsiyasi-WebSocket-fokusni-saqlash-bilan-
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AccessBoard - Kanban</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <!-- Semantik asosiy qism -->
    <main class="board-container">
        <h1>AccessBoard Taxtasi</h1>
        
        <!-- Taxta qismi dinamik ravishda JS orqali to'ldiriladi yoki quyidagi tuzilmada bo'ladi -->
        <div id="board" class="board">
            <!-- Ustunlar va kartalar shu yerga render qilinadi -->
        </div>
    </main>

    <script src="script.js"></script>
</body>
</html>
2. CSS Kodi (style.css)
CSS
/* Asosiy uslublar */
body {
    font-family: sans-serif;
    background-color: #f4f5f7;
    color: #333;
    padding: 20px;
}

.board {
    display: flex;
    gap: 20px;
}

.column {
    background: #ebecf0;
    padding: 15px;
    border-radius: 8px;
    width: 300px;
}

.column h2 {
    font-size: 16px;
    margin-bottom: 10px;
}

.task-list {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-direction: column;
    gap: 10px;
}

/* Vazifa kartasi (nativ button) */
.card {
    width: 100%;
    text-align: left;
    background: #ffffff;
    border: 1px solid #dfe1e6;
    padding: 10px 12px;
    border-radius: 6px;
    cursor: pointer;
    font-size: 14px;
    transition: background 0.2s, border-color 0.2s;
}

.card:hover {
    background: #f4f5f7;
}

/* outline: none ISHLatilmagan! :focus-visible orqali chiroyli fokus indikatori */
.card:focus-visible {
    outline: 3px solid #0052cc;
    outline-offset: 2px;
    border-color: #0052cc;
}
3. JavaScript Kodi (script.js)
JavaScript
// Taxtaning boshlang'ich ma'lumotlari
let boardData = [
    {
        id: 'todo',
        title: 'Bajarilishi kerak',
        cards: [
            { id: 'card-1', title: 'Dizayn maketini tuzish' },
            { id: 'card-2', title: 'Semantik HTML yozish' }
        ]
    },
    {
        id: 'in-progress',
        title: 'Jarayonda',
        cards: [
            { id: 'card-3', title: 'WebSocket ulanishini sozlash' }
        ]
    }
];

// Taxtani render qilish va fokusni saqlash funksiyasi
function renderBoard(data) {
    const boardContainer = document.getElementById('board');

    // 1. Hozir fokusda turgan elementning ID sini aniqlash (agar card bo'lsa)
    const activeElement = document.activeElement;
    const activeCardId = activeElement && activeElement.classList.contains('card') 
        ? activeElement.dataset.cardId 
        : null;

    // 2. HTML generatsiya qilish va DOM'ga yozish
    boardContainer.innerHTML = data.map(column => `
        <section class="column" aria-labelledby="${column.id}-title">
            <h2 id="${column.id}-title">${column.title}</h2>
            <ul class="task-list">
                ${column.cards.map(card => `
                    <li>
                        <button type="button" class="card" data-card-id="${card.id}">
                            ${card.title}
                        </button>
                    </li>
                `).join('')}
            </ul>
        </section>
    `).join('');

    // 3. Agar avval kartada fokus bo'lgan bo'lsa, uni yangi DOM'dan topib qaytadan tiklash
    if (activeCardId) {
        const cardToFocus = boardContainer.querySelector(`[data-card-id="${activeCardId}"]`);
        if (cardToFocus) {
            cardToFocus.focus();
        }
    }
}
