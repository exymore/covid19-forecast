<template>
    <div class="card" :style="{color, backgroundColor}">
        <h3>{{text}}</h3>
        <h1 class="title">{{value}}</h1>
    </div>
</template>

<script>
  export default {
    name: 'Card',
    props: {
      text: { type: String, default: '' },
      value: { type: String, default: '' },
      color: { type: String, default: '#4A4A4A' },
      backgroundColor: { type: String, default: '#FFFFFF' },
    },
  };
</script>

<style scoped>

    .card {
        border-radius: 8px;
        box-shadow: 0 1px 2px 0 rgba(60, 64, 67, .3), 0 1px 3px 1px rgba(60, 64, 67, .15);
        width: 240px;
        height: 100px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        margin: 10px;
    }

    h3 {
        font-weight: 500;
    }

    .title {
        font-weight: 700;
        color: inherit;
    }

    @media (max-width: 480px) {
        .card {
            text-align: center;
            justify-content: center;
            align-items: center;
        }

        h3 {
            font-size: 14px;
        }
    }
</style>
