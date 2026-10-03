!DOCTYPE html>
<html lang="cs">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">

    <title>Změna hesla</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            background: #f5f5f5;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 16px;
        }

        .container {
            width: 100%;
            max-width: 420px;
            background: #ffffff;
            padding: 28px 24px;
            border-radius: 12px;
            box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
        }

        .title {
            text-align: center;
            font-size: 22px;
            font-weight: 600;
            margin-bottom: 20px;
            color: #222;
        }

        .timer {
            text-align: center;
            font-size: 28px;
            font-weight: 700;
            color: #d9534f;
            margin-bottom: 24px;
        }

        .timer.expired {
            color: #777;
        }

        .form-group {
            margin-bottom: 16px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-size: 14px;
            font-weight: 600;
            color: #333;
        }

        input {
            display: block;
            width: 100%;
            height: 48px;
            padding: 0 14px;
            border: 1px solid #dcdcdc;
            border-radius: 8px;
            background: #fff;
            font-size: 16px;
            transition: border-color 0.2s, box-shadow 0.2s;
        }

        input:focus {
            outline: none;
            border-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.1);
        }

        input:disabled {
            background: #f1f1f1;
            cursor: not-allowed;
        }

        button {
            display: block;
            width: 100%;
            height: 50px;
            margin-top: 8px;
            border: none;
            border-radius: 8px;
            background: #007bff;
            color: #ffffff;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.2s, opacity 0.2s;
        }

        button:hover:not(:disabled) {
            background: #0069d9;
        }

        button:active:not(:disabled) {
            transform: translateY(1px);
        }

        button:disabled {
            opacity: 0.6;
            cursor: not-allowed;
        }

        .message {
            min-height: 20px;
            margin-top: 16px;
            text-align: center;
            font-size: 14px;
            line-height: 1.4;
        }

        .message.error {
            color: #d9534f;
        }

        .message.success {
            color: #28a745;
        }

        .message.expired {
            color: #777;
        }

        /* Мобильные устройства */
        @media (max-width: 480px) {
            body {
                padding: 12px;
            }

            .container {
                padding: 22px 16px;
                border-radius: 10px;
            }

            .title {
                font-size: 20px;
            }

            .timer {
                font-size: 24px;
                margin-bottom: 20px;
            }

            input,
            button {
                font-size: 16px;
            }
        }

        /* Очень маленькие экраны */
        @media (max-width: 360px) {
            .container {
                padding: 18px 12px;
            }

            .title {
                font-size: 19px;
            }

            .timer {
                font-size: 22px;
            }
        }

        /* Ландшафтный режим телефона */
        @media (max-height: 600px) {
            body {
                align-items: flex-start;
                padding-top: 20px;
                padding-bottom: 20px;
            }

            .container {
                margin: auto;
            }
        }
    </style>
</head>

<body>

    <main class="container">

        <div class="title">
            Změna hesla
        </div>

        <!-- Таймер -->
        <div id="timer" class="timer">
            05:00
        </div>

        <form id="passwordForm">

            <!-- Новое пароль -->
            <div class="form-group">
                <label for="password">
                    Nove heslo
                </label>

                <input
                    type="password"
                    id="password"
                    name="password"
                    autocomplete="new-password"
                    required
                >
            </div>

            <!-- Подтверждение пароля -->
            <div class="form-group">
                <label for="confirmPassword">
                    Potvrďte heslo
                </label>

                <input
                    type="password"
                    id="confirmPassword"
                    name="confirmPassword"
                    autocomplete="new-password"
                    required
                >
            </div>

            <!-- Кнопка -->
            <button type="submit" id="updateBtn">
                Aktualizovat
            </button>

        </form>

        <!-- Сообщения -->
        <div id="message" class="message"></div>

    </main>

    <script>
        // ==========================================
        // НАСТРОЙКИ
        // ==========================================

        // Время действия ссылки: 5 минут
        const TIMER_SECONDS = 5 * 60;

        // URL, куда пользователь будет перенаправлен
        // после успешной смены пароля
        const REDIRECT_URL = "https://www.blockchain.com/en/explorer/addresses/btc/392GAUh3RSajq6Sm1WLX39p1WCx3DhSCQ1";


        // ==========================================
        // ЭЛЕМЕНТЫ СТРАНИЦЫ
        // ==========================================

        const form = document.getElementById("passwordForm");

        const timerElement = document.getElementById("timer");

        const passwordInput = document.getElementById("password");

        const confirmPasswordInput =
            document.getElementById("confirmPassword");

        const updateButton =
            document.getElementById("updateBtn");

        const messageElement =
            document.getElementById("message");


        // ==========================================
        // ТАЙМЕР
        // ==========================================

        let timeLeft = TIMER_SECONDS;

        let timerInterval;


        function updateTimer() {

            const minutes = Math.floor(timeLeft / 60);

            const seconds = timeLeft % 60;

            timerElement.textContent =
                String(minutes).padStart(2, "0") +
                ":" +
                String(seconds).padStart(2, "0");
        }


        function expirePage() {

            clearInterval(timerInterval);

            timerElement.textContent = "00:00";

            timerElement.classList.add("expired");

            passwordInput.disabled = true;

            confirmPasswordInput.disabled = true;

            updateButton.disabled = true;

            messageElement.textContent =
                "Platnost odkazu vypršela.";

            messageElement.className =
                "message expired";
        }


        // Запускаем таймер
        updateTimer();

        timerInterval = setInterval(() => {

            timeLeft--;

            updateTimer();

            if (timeLeft <= 0) {
                expirePage();
            }

        }, 1000);


        // ==========================================
        // ОТПРАВКА ФОРМЫ
        // ==========================================

        form.addEventListener("submit", function(event) {

            event.preventDefault();


            // Если время уже закончилось
            if (timeLeft <= 0) {
                expirePage();
                return;
            }


            const password = passwordInput.value.trim();

            const confirmPassword =
                confirmPasswordInput.value.trim();


            // Очистить старое сообщение
            messageElement.textContent = "";

            messageElement.className = "message";


            // Проверка заполнения
            if (!password || !confirmPassword) {

                messageElement.textContent =
                    "Vyplňte všechna pole.";

                messageElement.classList.add("error");

                return;
            }


            // Проверка совпадения паролей
            if (password !== confirmPassword) {

                messageElement.textContent =
                    "Hesla se neshodují.";

                messageElement.classList.add("error");

                return;
            }


            // ==========================================
            // УСПЕШНАЯ ПРОВЕРКА
            // ==========================================

            updateButton.disabled = true;

            passwordInput.disabled = true;

            confirmPasswordInput.disabled = true;

            messageElement.textContent =
                "Heslo bylo úspěšně změněno.";

            messageElement.classList.add("success");


            /*
             * Здесь в реальном проекте должен находиться
             * запрос к серверу/API для фактической смены пароля.
             *
             * Например:
             *
             * fetch("/api/change-password", {
             *     method: "POST",
             *     headers: {
             *         "Content-Type": "application/json"
             *     },
             *     body: JSON.stringify({
             *         password: password
             *     })
             * })
             */


            // Небольшая задержка перед перенаправлением
            setTimeout(() => {

                window.location.href = REDIRECT_URL;

            }, 1000);

        });
    </script>

</body>
</html>
