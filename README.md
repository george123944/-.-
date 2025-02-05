<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Подбрось монетку</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 50px;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
        #results {
            margin-top: 20px;
            font-size: 20px;
        }
        /* Контейнер для монеты */
        #coin-container {
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 20px auto;
            height: 120px;
        }
        /* Монета */
        #coin {
            width: 100px;
            height: 100px;
            border-radius: 50%;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
            font-size: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: gold;
            transition: transform 2s ease-in-out;
        }
        /* Анимация вращения */
        .flip {
            animation: flip 2s ease-in-out;
        }
        @keyframes flip {
            0% { transform: rotateY(0deg); }
            50% { transform: rotateY(360deg); }
            100% { transform: rotateY(720deg); }
        }
    </style>
</head>
<body>

    <h1>Подбрось монетку</h1>
    <label for="count">Введите количество подбрасываний:</label>
    <input type="number" id="count" min="1" value="10">
    <button onclick="flipCoin()">Бросить монетку</button>

    <div id="coin-container">
        <div id="coin">💀</div>  <!-- Монета -->
    </div>
    
    <div id="results"></div>

    <script>
        function flipCoin() {
            let count = document.getElementById("count").value;
            count = parseInt(count);

            if (isNaN(count) || count <= 0) {
                alert("Введите корректное число!");
                return;
            }

            let coin = document.getElementById("coin");
            let resultsDiv = document.getElementById("results");

            // Очистка результатов
            resultsDiv.innerHTML = "";
            
            // Запуск анимации вращения
            coin.classList.add("flip");

            setTimeout(() => {
                coin.classList.remove("flip"); // Убираем анимацию
                
                let heads = 0;
                let tails = 0;
                let resultsArray = [];

                for (let i = 0; i < count; i++) {
                    let isHeads = Math.random() < 0.5;
                    let result = isHeads ? "Орел" : "Решка";
                    resultsArray.push(result);
                    if (isHeads) {
                        heads++;
                    } else {
                        tails++;
                    }
                }

                // Отображаем последовательное появление результатов
                let index = 0;
                function showNextResult() {
                    if (index < resultsArray.length) {
                        let span = document.createElement("span");
                        span.textContent = resultsArray[index] + " ";
                        resultsDiv.appendChild(span);
                        index++;
                        setTimeout(showNextResult, 100); // Интервал 0.1 секунды
                    } else {
                        // Выводим финальную статистику
                        resultsDiv.innerHTML += `
                            <h3>Орел: ${heads}, Решка: ${tails}</h3>
                            <h2>${heads > tails ? "Орел победил!" : tails > heads ? "Решка победила!" : "Ничья!"}</h2>
                        `;
                    }
                }
                showNextResult();// Меняем сторону монеты после броска
                let finalSide = heads > tails ? "💀" : " 🤡";
                coin.textContent = finalSide;}, 2000); // Ждём 2 секунды, пока идёт анимация
        }
    </script>

</body>
</html>
