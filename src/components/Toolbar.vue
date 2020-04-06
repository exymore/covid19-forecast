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
                <b-button class="external-button">
                    <a href="https://www.who.int/ru/emergencies/diseases/novel-coronavirus-2019/advice-for-public" target="_blank" rel="noopener noreferrer"
                       class="external-link">
                        <div class="covid-emoji">🦠</div>
                    </a>
                </b-button>
                <b-button class="external-button">
                    <a href="https://molamola.by/campaigns?sort=popular&category_id=10" target="_blank" rel="noopener noreferrer"
                       class="external-link">
                        <div class="covid-emoji">💵</div>
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
        font-weight: 700 !important;
        font-size: 24px !important;
    }

    .covid-emoji {
        font-size: 40px;
        animation: leaves 1s cubic-bezier(0.46, 0.03, 0.52, 0.96) infinite alternate;
    }

    .external-button {
        box-shadow: 0 1px 6px 0 rgba(60,64,67,.3);
        background-color: white;
        border: none;
        border-radius: 100%;
        height: 60px;
        width: 60px;
        margin-right: 1rem;
        transition: all 0.3s ease;
    }

    .external-button:hover {
        box-shadow: 0 4px 12px 0 rgba(60,64,67,.3);
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
