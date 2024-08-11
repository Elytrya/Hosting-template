<template>
    <div class="title">
        <img src="https://i.imgur.com/LJ1j8l7.jpg" alt="Логотип">
        <p>SpiderNodes</p>
    </div>
    <div class="nav-bar">
      <p>Главная</p>
      <p>Отзывы</p>
      <p>Тарифы</p>
      <p>Панель управления</p>
      <p>Билинг</p>
    </div>
    <div class="main">
      <h1>Закажите <span class="highlight">свой</span> сервер под <br><span class="highlight">ваши</span> нужды!</h1>
      <p>Закажите сервер у нас уже сегодня, мы предлагаем недорогие тарифы и услуги для вас, предоставляя лучшую поддержку.</p>
      <a href="">Подробнее</a>
    </div>
    <div class="console">
      <div class="left-top">
        <div class="red"></div>
        <div class="yellow"></div>
        <div class="green"></div>
      </div>
      <div class="center">
        <p>console.exe</p>
      </div>
      <div class="content">
        <div id="console-content" v-html="formattedText"></div>
      </div>
    </div>
  </template>
  
  <script>
    export default {
      data() {
        return {
          txt: [
            { text: "[Bot]: Starting up and connecting to Discord servers...", color: "green" },
            { text: "[Bot]: Loading configuration and modules...", color: "yellow" },
            { text: "[Bot]: Fetching server data and user info...", color: "blue" },
            { text: "[Bot]: Syncing with Discord API, please wait...", color: "purple" },
            { text: "[Bot]: Bot is <span class='highlight' data-color='yellow'>online</span> and <span class='highlight' data-color='yellow'>ready</span> to receive commands!", color: "red" },
          ],
          currentIndex: 0,
          typingInterval: 50,
          updateInterval: 1000,
          isTyping: false,
        };
      },
      methods: {
        typeText(text, callback) {
          const contentDiv = document.getElementById("console-content");
          const newLine = document.createElement("p");
          newLine.innerHTML = text; // Добавляем HTML сразу
          contentDiv.appendChild(newLine);
      
          // Сразу применяем стили после вставки HTML
          this.$nextTick(() => {
            this.applyStyles();
          });
  
          // Печать текста по символу (можно отключить, если текст добавляется сразу)
          this.isTyping = false;
          if (callback) callback();
        },
        updateScreen() {
          if (this.isTyping) return;
          const contentDiv = document.getElementById("console-content");
  
          if (contentDiv.childNodes.length === this.txt.length) {
            contentDiv.innerHTML = '';
            this.currentIndex = 0;
          }
  
          const text = this.txt[this.currentIndex].text;
          this.currentIndex = (this.currentIndex + 1) % this.txt.length;
          this.isTyping = true;
          this.typeText(text, () => {
            setTimeout(() => {
              this.updateScreen();
            }, this.updateInterval);
          });
        },
        applyStyles() {
          const contentDiv = document.getElementById("console-content");
          const spans = contentDiv.querySelectorAll('span.highlight');
          spans.forEach(span => {
            const color = span.getAttribute('data-color');
            span.style.backgroundColor = color; // Выставляем цвет фона
            span.style.color = '#000'; // Устанавливаем цвет текста
            span.style.fontWeight = 'bold'; // Жирный текст
          });
        }
      },
      mounted() {
        this.updateScreen();
      }
    }
  </script>
  
  
    
    
    
  <style scoped>
    .console {
      font-family: monospace;
      white-space: pre-wrap; /* Сохраняем пробелы и переводы строк */
      max-height: 335px; /* Пример максимальной высоты */
      overflow-y: auto; /* Скролл при переполнении */
    }
    
    .console p {
      margin: 0;
      word-wrap: break-word; /* Перенос слов, если они длинные */
    }
    
    .green {
      color: #00ff00; /* Зеленый цвет для текста */
    }
    
    .yellow {
      color: #ffff00; /* Желтый цвет для текста */
    }
    
    .blue {
      color: #0000ff; /* Синий цвет для текста */
    }
    
    .purple {
      color: #800080; /* Фиолетовый цвет для текста */
    }
    
    .red {
      color: #ff0000; /* Красный цвет для текста */
    }
    
    .highlight {
      background-color: #ffff00; /* Выделение фона желтым */
      color: #000; /* Черный текст для контраста */
      font-weight: bold; /* Жирный текст */
    }
</style>
