<template>
    <div @scroll="handleScroll" class="scroll-container">
        <div v-for="(message, index) in messages" :key="index" class="message">
            <p>{{ formatDate(message.date) }} / {{ message.authorName }} / {{ message.authorUrl }}</p>
            <br />
            <p v-html="colorizeText(message.content, message.contentPostTones)" class="message-content"></p>
            <hr />
        </div>
        <div v-if="loading">Загрузка...</div>
        <div v-if="!loading">
            <button @click="loadMore">Загрузить еще</button>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                messages: [],
                loadedMessages: 0,
                loading: false,
                hasMoreData: true,
            }
        },
        methods: {
            fetchMessages: async function(){
                if (this.loading || !this.hasMoreData) return;
                this.loading = true;

                const response = await fetch(`http://localhost:3000/messages?_limit=10&_start=${this.loadedMessages}`);
                const data = await response.json();
                if (data.length) {
                    this.messages.push(...data);
                    this.loadedMessages += data.length;
                } else {
                    this.hasMoreData = false; // Нет больше данных для загрузки
                }

                this.loading = false;
            },
            colorizeText: function(content, contentPostTones) {
                function getColor(tone) { // Ф-ция добавлена, т.к. у Николая Носова есть градиент
                    const red = Math.max(0, Math.min(255, Math.round((1 - tone) * 255)));
                    const green = Math.max(0, Math.min(255, Math.round((tone + 1) * 127.5)));
                    return `rgb(${red}, ${green}, 0)`;
                }

                let coloredContent = '';
                let lastIndex = 0;

                contentPostTones.forEach(({ position, tone, length }) => {
                    coloredContent += content.slice(lastIndex, position); // Добавляем неокрашенную часть текста
                    const color = getColor(tone);  // Получаем цвет для текущего tone
                    coloredContent += `<span style="color: ${color}">${content.slice(position, position + length)}</span>`;    // Добавляем окрашенную часть текста
                    lastIndex = position + length; // Обновляем последний индекс
                });

                coloredContent += content.slice(lastIndex); // Добавляем оставшуюся часть текста

                return coloredContent;
            },
            formatDate: function(isoDate) {
                const date = new Date(isoDate);
                const months = ["января", "февраля", "марта", "апреля", "мая", "июня","июля", "августа", "сентября", "октября", "ноября", "декабря"];
                const hours = String(date.getUTCHours()).padStart(2, '0');
                const minutes = String(date.getUTCMinutes()).padStart(2, '0');
                const day = date.getUTCDate();
                const month = months[date.getUTCMonth()];
                const year = date.getUTCFullYear();

                return `${hours}:${minutes} ${day} ${month} ${year} г`;
            },  
            handleScroll: function() {
                const container = document.querySelector('.scroll-container');
  
                if (container.scrollTop + container.clientHeight >= container.scrollHeight) {
                    this.fetchMessages();
                }
            },
            loadMore: function() {
                this.fetchMessages();
            },
        },
        mounted() {
            this.fetchMessages();
        },
	}
</script>


<style scoped>
.message {
    margin-bottom: 20px;
}
.scroll-container {
    height: 85vh; /* Задайте высоту контейнера */
    overflow-y: auto; /* Включаем прокрутку */
}
</style>