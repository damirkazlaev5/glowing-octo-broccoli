<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Прогноз погоды</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 500px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f0f8ff;
        }
        .container {
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            padding: 20px;
        }
        h1 {
            text-align: center;
            color: #333;
        }
        .form {
            margin-bottom: 20px;
        }
        input[type="text"] {
            width: 70%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            padding: 10px 15px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        .weather {
            margin-top: 20px;
            padding: 15px;
            background-color: #e8f4f8;
            border-radius: 6px;
            display: none;
        }
        .error {
            color: red;
            text-align: center;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Прогноз погоды</h1>
        <div class="form">
            <input type="text" id="city" placeholder="Введите город">
            <button onclick="getWeather()">Получить погоду</button>
        </div>
        <div id="error" class="error"></div>
        <div id="weather" class="weather">
            <h2 id="city-name"></h2>
            <p><strong>Температура:</strong> <span id="temp"></span> °C</p>
            <p><strong>Ощущается как:</strong> <span id="feels"></span> °C</p>
            <p><strong>Влажность:</strong> <span id="humidity"></span> %</p>
            <p><strong>Описание:</strong> <span id="desc"></span></p>
            <p><strong>Ветер:</strong> <span id="wind"></span> м/с</p>
        </div>
    </div>

    <script>
        // Ваш API-ключ OpenWeatherMap (замените на реальный!)
        const API_KEY = 'ВАШ_API_КЛЮЧ';

        function getWeather() {
            const city = document.getElementById('city').value.trim();
            const errorDiv = document.getElementById('error');
            const weatherDiv = document.getElementById('weather');

            if (!city) {
                errorDiv.textContent = 'Введите название города!';
                weatherDiv.style.display = 'none';
                return;
            }

            // Скрываем ошибки и показываем загрузчик
            errorDiv.textContent = '';
            weatherDiv.style.display = 'none';

            // Формируем URL запроса
            const url = `https://api.openweathermap.org/data/2.5/weather?q=${encodeURIComponent(city)}&appid=${API_KEY}&units=metric&lang=ru`;

            fetch(url)
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Город не найден или ошибка API');
                    }
                    return response.json();
                })
                .then(data => {
                    // Заполняем данные
                    document.getElementById('city-name').textContent = data.name;
                    document.getElementById('temp').textContent = data.main.temp;
                    document.getElementById('feels').textContent = data.main.feels_like;
                    document.getElementById('humidity').textContent = data.main.humidity;
                    document.getElementById('desc').textContent = data.weather[0].description;
                    document.getElementById('wind').textContent = data.wind.speed;

                    // Показываем блок с погодой
                    weatherDiv.style.display = 'block';
                })
                .catch(error => {
                    errorDiv.textContent = error.message;
                    weatherDiv.style.display = 'none';
                });
        }
    </script>
</html>
