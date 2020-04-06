<template>
    <section class="section">
        <h2 class="subtitle">
            Выберите Страну
        </h2>
        <b-field>
            <span class="icon">
                🌍
            </span>
            <b-select placeholder="Country" v-model="country" rounded expanded>
                <option v-for="country in countriesEnum" :key="country" value="country">{{country}}</option>
            </b-select>
        </b-field>
    </section>
</template>

<script>
  import countriesEnum from '../enums';

  export default {
    name: 'Select',
    data: () => ({
      country: '',
      countriesEnum
    }),
  };
</script>

<style scoped>
    .field {
        flex-direction: row;
        display: flex;
        align-items: center;
        justify-content: space-between;
        width: 40%;
    }

    .icon {
        font-size: 30px;
        margin-right: 1rem;
    }

    .field >>> .control {
        width: 70%;
    }

    @media (max-width: 960px) {
        .field {
            width: 100%;
        }
    }
</style>
