<template>
    <b-navbar :style="{width: '100%'}" :mobile-burger="false">
        <template slot="brand">
            <b-navbar-item tag="div">
                <div class="app-title">
                    Прогноз Covid-19
                </div>
            </b-navbar-item>
        </template>
        <template slot="end">
            <b-navbar-item tag="div">
                <b-button type="is-light" class="external-button">
                    <a href="https://github.com/novelcovid/api" target="_blank" rel="noopener noreferrer"
                       class="external-link">
                        <div class="covid-emoji">🦠</div>
                    </a>
                </b-button>
            </b-navbar-item>
        </template>
    </b-navbar>
</template>

<script>
  export default {
    name: 'Toolbar',
  };
</script>

<style scoped>
    .app-title {
        font-weight: 600 !important;
        font-size: 24px !important;
    }

    .covid-emoji {
        font-size: 40px;
        animation: leaves 1s cubic-bezier(0.46, 0.03, 0.52, 0.96) infinite alternate;
    }

    .external-button {
        border-radius: 100%;
        height: 64px;
        width: 64px;
    }

    .external-link {
        background-color: white;
        text-decoration: none !important;
    }

    @keyframes leaves {
        0% {
            transform: scale(1.0);
        }
        100% {
            transform: scale(0.8);
        }
    }

    @media (max-width: 960px) {
        .app-title {
            font-weight: 600 !important;
            font-size: 20px !important;
        }
    }
</style>
