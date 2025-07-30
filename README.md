
<html lang="kk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Қыз Жібек жыры - Оқиғаларды ретке келтіру</title>
    <style>
        :root {
            --primary: #3498db; /* Небесно-голубой */
            --secondary: #f1c40f; /* Солнечно-желтый */
            --correct: #2ecc71; /* Зеленый */
            --incorrect: #e74c3c; /* Красный */
            --background: #ecf0f1; /* Светлый фон */
            --text: #2c3e50; /* Темный текст */
            --card: #ffffff; /* Белый */
            --key: #9b59b6; /* Фиолетовый для кнопки ключа */
        }

        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            background-color: var(--background);
            color: var(--text);
            background-image: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            background-attachment: fixed;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
            padding: 20px;
        }

        header {
            background-color: rgba(255, 255, 255, 0.9);
            color: var(--text);
            padding: 20px;
            text-align: center;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            margin-bottom: 30px;
        }

        h1 {
            margin: 0;
            font-size: 2.2rem;
            color: var(--primary);
        }

        h2 {
            color: var(--secondary);
            text-shadow: 1px 1px 2px rgba(0,0,0,0.2);
            text-align: center;
            margin-bottom: 30px;
        }

        .game-container {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .columns-container {
            display: flex;
            justify-content: space-between;
            gap: 20px;
            min-height: 600px;
        }

        .column {
            flex: 1;
            min-width: 300px;
            display: flex;
            flex-direction: column;
        }

        .column-title {
            background-color: var(--primary);
            color: white;
            padding: 10px;
            border-radius: 5px;
            text-align: center;
            margin-bottom: 10px;
            font-weight: bold;
        }

        .items-container {
            flex: 1;
            background-color: rgba(255,255,255,0.7);
            border-radius: 8px;
            padding: 10px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            border: 2px dashed var(--primary);
            min-height: 500px;
        }

        .item {
            background-color: var(--card);
            border: 2px solid var(--primary);
            border-radius: 8px;
            padding: 15px;
            cursor: grab;
            transition: all 0.3s ease;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            position: relative;
            user-select: none;
            touch-action: none;
        }

        .item:hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
        }

        .item.correct {
            background-color: var(--correct);
            color: white;
            border-color: var(--correct);
        }

        .item.incorrect {
            background-color: var(--incorrect);
            color: white;
            border-color: var(--incorrect);
        }

        .item.dragging {
            opacity: 0.8;
            transform: scale(1.05);
            box-shadow: 0 8px 16px rgba(0,0,0,0.3);
            z-index: 100;
        }

        .item.show-answer {
            background-color: var(--key);
            color: white;
            border-color: var(--key);
            box-shadow: 0 0 0 3px rgba(155, 89, 182, 0.5);
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 20px;
        }

        .btn {
            padding: 12px 25px;
            background-color: var(--primary);
            color: white;
            border: none;
            border-radius: 30px;
            cursor: pointer;
            font-size: 1.1rem;
            transition: all 0.3s;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            font-weight: bold;
        }

        .btn:hover {
            background-color: var(--secondary);
            color: var(--text);
            transform: translateY(-2px);
            box-shadow: 0 6px 10px rgba(0,0,0,0.15);
        }

        .btn:active {
            transform: translateY(0);
        }

        .btn-check {
            background-color: var(--correct);
        }

        .btn-reset {
            background-color: var(--incorrect);
        }

        .btn-key {
            background-color: var(--key);
        }

        .feedback {
            text-align: center;
            margin-top: 20px;
            padding: 15px;
            border-radius: 8px;
            font-weight: bold;
            display: none;
        }

        .feedback.correct {
            background-color: rgba(46, 204, 113, 0.2);
            color: var(--correct);
            display: block;
        }

        .feedback.incorrect {
            background-color: rgba(231, 76, 60, 0.2);
            color: var(--incorrect);
            display: block;
        }

        @media (max-width: 768px) {
            .columns-container {
                flex-direction: column;
            }
            
            .column {
                width: 100%;
            }
            
            h1 {
                font-size: 1.8rem;
            }
            
            .controls {
                flex-direction: column;
                align-items: center;
            }
            
            .btn {
                width: 100%;
                max-width: 250px;
            }
        }

        /* Анимации */
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .pulse {
            animation: pulse 1.5s infinite;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            10%, 30%, 50%, 70%, 90% { transform: translateX(-5px); }
            20%, 40%, 60%, 80% { transform: translateX(5px); }
        }

        .shake {
            animation: shake 0.5s;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .fade-in {
            animation: fadeIn 0.5s ease-out;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>«Қыз Жібек» жырының оқиғаларын ретке келтір</h1>
            <p>Оқиғаларды дұрыс ретпен орналастырыңыз</p>
        </header>

        <div class="game-container">
            <h2>Сәйкестендіру ойыны</h2>
            
            <div class="columns-container">
                <div class="column">
                    <div class="column-title">Оқиғалар</div>
                    <div class="items-container" id="events-column">
                        <!-- События будут добавлены через JavaScript -->
                    </div>
                </div>
                
                <div class="column">
                    <div class="column-title">Сипаттамалар</div>
                    <div class="items-container" id="descriptions-column">
                        <!-- Описания будут добавлены через JavaScript -->
                    </div>
                </div>
            </div>
            
            <div class="controls">
                <button class="btn btn-check" id="check-btn">Тексеру</button>
                <button class="btn btn-reset" id="reset-btn">Қалпына келтіру</button>
                <button class="btn btn-key" id="key-btn">Кілт</button>
            </div>
            
            <div class="feedback" id="feedback"></div>
        </div>
    </div>

    <script>
        // Данные для игры
        const gameData = [
            {
                event: "Төлеген түс көріп, өзі үшін жаратылған қыз бар екенін біледі",
                description: "Төлегеннің түс көруі мен ел аралап қыз іздеуі"
            },
            {
                event: "Ел аралап, қыз іздейді, бірақ ешқайсысын ұнатпайды",
                description: "Төлегеннің жорыққа шығуы"
            },
            {
                event: "Саудагер шал Жайық бойында сұлулар барын айтады",
                description: "Саудагер шалдың Жайықтағы қыз туралы айтуы"
            },
            {
                event: "Төлеген шешесіне қарамай жорыққа шығады",
                description: "Анасының қарсы болуы, қоштасу сәті"
            },
            {
                event: "Жүздеген қызды көреді, ешқайсысын таңдамаған соң Қаршыға кездеседі",
                description: "Қыз таңдауы (жүздеген қыздарды көруі)"
            },
            {
                event: "Қаршыға Төлегенді Жібекпен таныстырады",
                description: "Қаршыға арқылы Қыз Жібекті көруі"
            },
            {
                event: "Төлеген мен Жібек өлеңмен сөйлесіп, тіл табысады",
                description: "Жібек пен Төлегеннің танысуы, сөз алмасуы"
            },
            {
                event: "Төлеген Сырлыбай ханға барып құда түседі",
                description: "Құда түсу, той"
            },
            {
                event: "Той болады, Төлеген еліне қайтады",
                description: "Бекежан мен басқа жігіттердің қызғануы"
            },
            {
                event: "Бекежан мен шекті жігіттері Төлегенге қастандық жасайды",
                description: "Қастандық пен Төлегеннің өлімі"
            },
            {
                event: "Төлеген мерт болады, Жібек жоқтау айтады",
                description: "Жібектің зары"
            }
        ];

        // Перемешиваем данные для игры
        function shuffleArray(array) {
            const shuffled = [...array];
            for (let i = shuffled.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
            }
            return shuffled;
        }

        // Инициализация игры
        function initGame() {
            const eventsColumn = document.getElementById('events-column');
            const descriptionsColumn = document.getElementById('descriptions-column');
            
            // Очищаем колонки
            eventsColumn.innerHTML = '';
            descriptionsColumn.innerHTML = '';
            
            // Перемешиваем данные
            const shuffledEvents = shuffleArray([...gameData]);
            const shuffledDescriptions = shuffleArray([...gameData]);
            
            // Добавляем события
            shuffledEvents.forEach((item, index) => {
                const eventElement = createItemElement(item.event, 'event', index);
                eventsColumn.appendChild(eventElement);
            });
            
            // Добавляем описания
            shuffledDescriptions.forEach((item, index) => {
                const descElement = createItemElement(item.description, 'description', index);
                descriptionsColumn.appendChild(descElement);
            });
            
            // Скрываем feedback
            document.getElementById('feedback').style.display = 'none';
            
            // Добавляем обработчики перетаскивания
            setupDragAndDrop();
        }

        // Создание элемента для перетаскивания
        function createItemElement(text, type, index) {
            const element = document.createElement('div');
            element.className = 'item';
            element.textContent = text;
            element.dataset.type = type;
            element.dataset.index = index;
            element.draggable = true;
            return element;
        }

        // Настройка перетаскивания (улучшенная версия)
        function setupDragAndDrop() {
            const items = document.querySelectorAll('.item');
            const containers = document.querySelectorAll('.items-container');
            
            let draggedItem = null;
            let draggedItemParent = null;
            let draggedItemIndex = null;

            items.forEach(item => {
                item.addEventListener('dragstart', (e) => {
                    draggedItem = item;
                    draggedItemParent = item.parentElement;
                    draggedItemIndex = Array.from(item.parentElement.children).indexOf(item);
                    setTimeout(() => {
                        item.classList.add('dragging');
                    }, 0);
                    
                    // Для мобильных устройств
                    e.dataTransfer.setData('text/plain', '');
                });

                item.addEventListener('dragend', () => {
                    item.classList.remove('dragging');
                    draggedItem = null;
                    draggedItemParent = null;
                    draggedItemIndex = null;
                });
            });

            containers.forEach(container => {
                container.addEventListener('dragover', (e) => {
                    e.preventDefault();
                    const afterElement = getDragAfterElement(container, e.clientY);
                    const dragging = document.querySelector('.dragging');
                    
                    if (afterElement == null) {
                        container.appendChild(dragging);
                    } else {
                        container.insertBefore(dragging, afterElement);
                    }
                });
            });
        }

        // Функция для определения позиции при перетаскивании
        function getDragAfterElement(container, y) {
            const draggableElements = [...container.querySelectorAll('.item:not(.dragging)')];
            
            return draggableElements.reduce((closest, child) => {
                const box = child.getBoundingClientRect();
                const offset = y - box.top - box.height / 2;
                
                if (offset < 0 && offset > closest.offset) {
                    return { offset: offset, element: child };
                } else {
                    return closest;
                }
            }, { offset: Number.NEGATIVE_INFINITY }).element;
        }

        // Проверка ответов
        function checkAnswers() {
            const eventsColumn = document.getElementById('events-column');
            const descriptionsColumn = document.getElementById('descriptions-column');
            const feedback = document.getElementById('feedback');
            
            const eventItems = Array.from(eventsColumn.children);
            const descItems = Array.from(descriptionsColumn.children);
            
            let allCorrect = true;
            
            // Проверяем соответствие
            eventItems.forEach((eventItem, index) => {
                const eventText = eventItem.textContent;
                const descText = descItems[index]?.textContent || '';
                
                // Находим оригинальное соответствие
                const originalPair = gameData.find(item => 
                    item.event === eventText || item.description === eventText
                );
                
                let isCorrect = false;
                
                if (originalPair) {
                    if (eventText === originalPair.event && descText === originalPair.description) {
                        isCorrect = true;
                    } else if (eventText === originalPair.description && descText === originalPair.event) {
                        isCorrect = true;
                    }
                }
                
                if (isCorrect) {
                    eventItem.classList.add('correct');
                    eventItem.classList.remove('incorrect');
                    if (descItems[index]) {
                        descItems[index].classList.add('correct');
                        descItems[index].classList.remove('incorrect');
                    }
                } else {
                    eventItem.classList.add('incorrect');
                    eventItem.classList.remove('correct');
                    if (descItems[index]) {
                        descItems[index].classList.add('incorrect');
                        descItems[index].classList.remove('correct');
                    }
                    allCorrect = false;
                }
            });
            
            // Показываем feedback
            if (allCorrect) {
                feedback.textContent = 'Керемет! Барлық жауаптар дұрыс!';
                feedback.className = 'feedback correct pulse';
            } else {
                feedback.textContent = 'Кейбір жауаптар қате. Қайталап көріңіз!';
                feedback.className = 'feedback incorrect shake';
            }
            
            feedback.style.display = 'block';
        }

        // Показать правильные ответы
        function showAnswers() {
            const eventsColumn = document.getElementById('events-column');
            const descriptionsColumn = document.getElementById('descriptions-column');
            const feedback = document.getElementById('feedback');
            
            // Удаляем все классы перед показом ответов
            document.querySelectorAll('.item').forEach(item => {
                item.classList.remove('correct', 'incorrect', 'show-answer');
            });
            
            // Создаем временные массивы для элементов
            const eventItems = Array.from(eventsColumn.children);
            const descItems = Array.from(descriptionsColumn.children);
            
            // Для каждого элемента в левой колонке
            eventItems.forEach(eventItem => {
                const eventText = eventItem.textContent;
                
                // Находим правильное описание
                const correctPair = gameData.find(item => 
                    item.event === eventText || item.description === eventText
                );
                
                if (correctPair) {
                    // Находим индекс правильного описания в правой колонке
                    const correctDescIndex = descItems.findIndex(descItem => 
                        descItem.textContent === correctPair.description || 
                        descItem.textContent === correctPair.event
                    );
                    
                    // Если нашли правильное описание в правой колонке
                    if (correctDescIndex !== -1) {
                        // Подсвечиваем пару
                        eventItem.classList.add('show-answer', 'fade-in');
                        descItems[correctDescIndex].classList.add('show-answer', 'fade-in');
                        
                        // Перемещаем описание напротив события
                        if (Array.from(eventsColumn.children).indexOf(eventItem) !== correctDescIndex) {
                            const correctDesc = descItems[correctDescIndex];
                            const currentIndex = Array.from(descriptionsColumn.children).indexOf(correctDesc);
                            const targetIndex = Array.from(eventsColumn.children).indexOf(eventItem);
                            
                            if (targetIndex < descriptionsColumn.children.length) {
                                descriptionsColumn.insertBefore(
                                    correctDesc, 
                                    descriptionsColumn.children[targetIndex]
                                );
                            } else {
                                descriptionsColumn.appendChild(correctDesc);
                            }
                        }
                    }
                }
            });
            
            // Показываем сообщение
            feedback.textContent = 'Дұрыс жауаптар көрсетілді!';
            feedback.className = 'feedback correct';
            feedback.style.display = 'block';
        }

        // Сброс игры
        function resetGame() {
            initGame();
        }

        // Инициализация при загрузке
        document.addEventListener('DOMContentLoaded', () => {
            initGame();
            
            // Кнопки
            document.getElementById('check-btn').addEventListener('click', checkAnswers);
            document.getElementById('reset-btn').addEventListener('click', resetGame);
            document.getElementById('key-btn').addEventListener('click', showAnswers);
        });
    </script>
</body>
</html>
