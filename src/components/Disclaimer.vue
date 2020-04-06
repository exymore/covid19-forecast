<template>
    <div class="message">
        <b-message title="Предупреждение" type="is-warning" has-icon aria-close-label="Close message">
            Данные, полученные приложением, являются прогнозом и не отражают действительность. День ото дня данные могут
            кардинально отличаться. Приложение предназначено для примерной оценки распространения пандемии по странам.
        </b-message>
    </div>
</template>

<script>
  export default {
    name: 'Disclaimer',
  };
</script>

<style scoped>
    .message >>> .message-body {
        background-color: white;
    }

    .message {
        position: relative;
        z-index: 30;
        display: flex;
        top: 60px;
        flex-direction: column;
        margin-left: auto;
        margin-right: auto;
        max-width: 960px;
        border-radius: 8px;
        background: none;
    }

    @media (max-width: 960px) {
        .message {
            z-index: 5;
            position: fixed;
            bottom: 0;
            top: auto;
            width: 100%;
        }
    }
</style>
