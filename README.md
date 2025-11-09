<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>YouTube Proxy</title>
    <!-- Загрузка Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Шрифт Inter -->
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f7f9fb;
            color: #1e293b;
        }
        .app-container {
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            padding: 20px 10px;
        }
        .card {
            background-color: #ffffff;
            border-radius: 16px;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
    </style>
</head>
<body>
    <div class="app-container">
        <div class="card w-full max-w-lg p-6 sm:p-8">
            <h1 class="text-3xl font-bold text-center mb-6 text-indigo-700">
                YouTube Proxy 🚀
            </h1>
            <p class="text-center text-gray-500 mb-8">
                Вставьте ссылку на YouTube видео или используйте поиск.
            </p>

            <form id="urlForm" class="space-y-4">
                <div class="relative">
                    <input type="text" id="queryInput" placeholder="Введите ссылку YouTube или поисковый запрос" required
                           class="w-full p-3 pl-10 border border-gray-300 rounded-xl focus:ring-indigo-500 focus:border-indigo-500 transition duration-150">
                    <svg class="absolute left-3 top-1/2 transform -translate-y-1/2 h-5 w-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.881a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1"></path></svg>
                </div>

                <div class="flex flex-col sm:flex-row space-y-4 sm:space-y-0 sm:space-x-4">
                    <button type="submit" data-action="url"
                            class="flex-1 w-full bg-indigo-600 text-white font-semibold py-3 rounded-xl hover:bg-indigo-700 transition duration-150 shadow-md shadow-indigo-200">
                        🔗 Проксировать по ссылке
                    </button>
                    <button type="submit" data-action="search"
                            class="flex-1 w-full bg-green-500 text-white font-semibold py-3 rounded-xl hover:bg-green-600 transition duration-150 shadow-md shadow-green-200">
                        🔍 Найти видео
                    </button>
                </div>
            </form>

            <div id="messageBox" class="mt-6 p-4 text-sm rounded-xl hidden" role="alert"></div>
            
            <p class="text-xs text-center text-gray-400 mt-6">
                Mini App разработан для @LoaderYTbot.
            </p>
        </div>
    </div>

    <script>
        // Telegram WebApp Object: Должен быть доступен в Telegram WebApps
        const tg = window.Telegram.WebApp;
        const form = document.getElementById('urlForm');
        const queryInput = document.getElementById('queryInput');
        const messageBox = document.getElementById('messageBox');

        /**
         * Отображает временное сообщение пользователю.
         * @param {string} message - Текст сообщения.
         * @param {string} type - Тип сообщения ('success', 'error', 'info').
         */
        function showMessage(message, type = 'info') {
            messageBox.textContent = message;
            messageBox.classList.remove('hidden', 'bg-red-100', 'text-red-700', 'bg-green-100', 'text-green-700', 'bg-blue-100', 'text-blue-700');

ㅤ, [09.11.2025 21:52]
if (type === 'error') {
                messageBox.classList.add('bg-red-100', 'text-red-700');
            } else if (type === 'success') {
                messageBox.classList.add('bg-green-100', 'text-green-700');
            } else {
                messageBox.classList.add('bg-blue-100', 'text-blue-700');
            }
            // Скрываем через 5 секунд
            setTimeout(() => messageBox.classList.add('hidden'), 5000);
        }

        /**
         * Отправляет данные в Telegram-бота.
         * @param {string} query - Ссылка или поисковый запрос.
         * @param {string} type - Тип действия ('url' или 'search').
         */
        function sendDataToBot(query, type) {
            const data = JSON.stringify({ query: query, type: type });
            tg.sendData(data);
            showMessage('Запрос отправлен боту. Проверьте Telegram.', 'success');
            tg.close();
        }

        form.addEventListener('submit', function(e) {
            e.preventDefault();
            const query = queryInput.value.trim();
            const actionButton = e.submitter; 
            const actionType = actionButton.getAttribute('data-action'); 

            if (!query) {
                showMessage('Пожалуйста, введите ссылку или поисковый запрос.', 'error');
                return;
            }

            sendDataToBot(query, actionType);
        });

        // Инициализация: проверка параметров URL при открытии из инлайн-кнопки
        document.addEventListener('DOMContentLoaded', () => {
            const params = new URLSearchParams(window.location.search);
            const urlFromBot = params.get('url');
            
            if (urlFromBot) {
                // Если ссылка пришла от бота (после поиска), проксируем ее сразу
                sendDataToBot(urlFromBot, 'url');
            } else {
                // Плавное появление интерфейса
                tg.ready();
            }
        });

    </script>
</body>
</html>
