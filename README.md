<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>EmoLink — Твой эмоциональный помощник</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    body {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      color: #333;
    }

    .container {
      width: 90%;
      max-width: 600px;
      padding: 2rem;
      margin: 2rem auto;
      background: rgba(255, 255, 255, 0.95);
      border-radius: 20px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
      text-align: center;
    }

    h1 {
      color: #4a3a87;
      margin-bottom: 1.5rem;
    }

    .emotion-buttons {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 1rem;
      margin: 1.5rem 0;
    }

    .emotion-btn {
      padding: 1rem 1.5rem;
      border: none;
      border-radius: 15px;
      font-size: 1rem;
      cursor: pointer;
      transition: transform 0.2s, box-shadow 0.2s;
      background: #e0e0ff;
      color: #4a3a87;
      font-weight: 600;
    }

    .emotion-btn:hover {
      transform: translateY(-3px);
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
    }

    .emotion-btn.active {
      background: #667eea;
      color: white;
    }

    .response {
      margin: 2rem 0;
      padding: 1.5rem;
      background: #f0f7ff;
      border-radius: 15px;
      min-height: 100px;
      display: none;
      font-size: 1.1rem;
      line-height: 1.6;
      color: #2c3e50;
    }

    .video-feed {
      margin-top: 2rem;
      width: 100%;
      display: none;
    }

    .video-item {
      margin-bottom: 1.5rem;
      border-radius: 15px;
      overflow: hidden;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
    }

    .video-item iframe {
      width: 100%;
      height: 200px;
      border: none;
    }

    .loading {
      color: #667eea;
      font-style: italic;
      margin: 1rem 0;
    }

    .footer {
      margin-top: 3rem;
      font-size: 0.9rem;
      color: rgba(255,255,255,0.8);
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Как ты себя чувствуешь сегодня? 💬</h1>

    <div class="emotion-buttons">
      <button class="emotion-btn" data-emotion="sad">Грустно 😢</button>
      <button class="emotion-btn" data-emotion="anxious">Тревожно 😰</button>
      <button class="emotion-btn" data-emotion="lonely">Одиночество 🥺</button>
      <button class="emotion-btn" data-emotion="stressed">Перегружен 😫</button>
      <button class="emotion-btn" data-emotion="happy">Хорошо 😊</button>
      <button class="emotion-btn" data-emotion="confused">Сбился с пути 🤔</button>
    </div>

    <div class="response" id="response">
      <div class="loading">Подбираем твоего собеседника...</div>
    </div>

    <div class="video-feed" id="videoFeed">
      <h2>Милые видео для поддержки 🐶🐱</h2>
      <!-- Динамически заполняется JS -->
    </div>

    <div class="footer">
      EmoLink — твой безопасный уголок для эмоций 🌸
    </div>
  </div>

  <script>
    // Данные: эмоции → текст поддержки
    const supportMessages = {
      sad: "Ты не один. Грусть — это нормально. Позволь себе чувствовать. Иногда просто быть — уже подвиг. Я здесь, чтобы выслушать.",
      anxious: "Твоя тревога не говорит о твоей слабости — она говорит о твоей заботе. Дыши глубоко. Всё будет. Ты справляешься, даже если не чувствуешь этого.",
      lonely: "Ты не один, даже если сейчас кажется иначе. Ты важен. Твоё присутствие в мире имеет значение. Я с тобой.",
      stressed: "Ты не обязан всё успеть. Отдохни. Сделай паузу. Ты не машина. Ты — человек, и твоё благополучие важнее любой задачи.",
      happy: "Как здорово, что ты счастлив! 🌞 Позволь этому чувству расти. Иногда самые простые моменты — самые ценные. Наслаждайся этим!",
      confused: "Не знаешь, куда идти? Это нормально. Даже самые мудрые люди теряются. Пусть шаг будет маленьким — главное, чтобы он был твой."
    };

    // Данные: эмоции → список YouTube-видео (милые животные)
    const videoLinks = {
      sad: [
        "https://www.youtube.com/embed/dQw4w9WgXcQ?autoplay=0", // Заменить на реальные
        "https://www.youtube.com/embed/3j9xQ9QqY0k?autoplay=0",
        "https://www.youtube.com/embed/5qap5aO4i9A?autoplay=0"
      ],
      anxious: [
        "https://www.youtube.com/embed/1uZq2J7g1oM?autoplay=0",
        "https://www.youtube.com/embed/2145s0k3w2M?autoplay=0",
        "https://www.youtube.com/embed/6u89w3ZvQ0Y?autoplay=0"
      ],
      lonely: [
        "https://www.youtube.com/embed/8h29uZk9j4I?autoplay=0",
        "https://www.youtube.com/embed/7h7hZ7Y5k9g?autoplay=0",
        "https://www.youtube.com/embed/3j9xQ9QqY0k?autoplay=0"
      ],
      stressed: [
        "https://www.youtube.com/embed/3j9xQ9QqY0k?autoplay=0",
        "https://www.youtube.com/embed/6u89w3ZvQ0Y?autoplay=0",
        "https://www.youtube.com/embed/1uZq2J7g1oM?autoplay=0"
      ],
      happy: [
        "https://www.youtube.com/embed/5qap5aO4i9A?autoplay=0",
        "https://www.youtube.com/embed/6u89w3ZvQ0Y?autoplay=0",
        "https://www.youtube.com/embed/3j9xQ9QqY0k?autoplay=0"
      ],
      confused: [
        "https://www.youtube.com/embed/7h7hZ7Y5k9g?autoplay=0",
        "https://www.youtube.com/embed/8h29uZk9j4I?autoplay=0",
        "https://www.youtube.com/embed/2145s0k3w2M?autoplay=0"
      ]
    };

    // Список реальных видео (можно заменить на YouTube-ссылки с милыми животными)
    // ⚠️ В реальности используйте только разрешённые YouTube-видео с embed-доступом
    const realVideoIds = [
      "3j9xQ9QqY0k", // Котёнок в коробке
      "5qap5aO4i9A", // Щенок лает на бабочку
      "1uZq2J7g1oM", // Утка с котёнком
      "6u89w3ZvQ0Y", // Собака играет с котом
      "7h7hZ7Y5k9g", // Пингвин и пёс
      "8h29uZk9j4I", // Лиса и кот
      "2145s0k3w2M", // Кролик в снегу
      "dQw4w9WgXcQ"  // Замена: можно заменить на другое
    ];

    // Генерация URL для YouTube
    function getYoutubeUrl(id) {
      return `https://www.youtube.com/embed/${id}?autoplay=0&rel=0&modestbranding=1`;
    }

    // Функция: отобразить поддержку и видео
    function showSupport(emotion) {
      const responseEl = document.getElementById('response');
      const videoFeedEl = document.getElementById('videoFeed');

      // Показать сообщение
      responseEl.innerHTML = `<p>${supportMessages[emotion]}</p>`;
      responseEl.style.display = 'block';

      // Случайно выбрать 3 видео
      const shuffled = [...realVideoIds].sort(() => 0.5 - Math.random()).slice(0, 3);
      videoFeedEl.innerHTML = `
        <h2>Милые видео для поддержки 🐶🐱</h2>
        ${shuffled.map(id => `
          <div class="video-item">
            <iframe src="${getYoutubeUrl(id)}" allowfullscreen></iframe>
          </div>
        `).join('')}
      `;
      videoFeedEl.style.display = 'block';
    }

    // Обработчики кнопок
    document.querySelectorAll('.emotion-btn').forEach(button => {
      button.addEventListener('click', () => {
        // Снять активный класс со всех
        document.querySelectorAll('.emotion-btn').forEach(btn => {
          btn.classList.remove('active');
        });

        // Добавить активный класс
        button.classList.add('active');

        // Получить эмоцию
        const emotion = button.getAttribute('data-emotion');

        // Показать поддержку и видео
        showSupport(emotion);
      });
    });
  </script>
</body>
</html>

