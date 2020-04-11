<template>
    <section class="section">
        <h2 class="subtitle">
            Выберите Страну
        </h2>
        <b-field>
            <span class="icon">
                🌍
            </span>
            <v-select label="Country" v-model="country" :clearable="false" :options="countriesEnum"
                      class="select-component">
                <template #no-options="">
                    Такой страны не существует :(
                </template>
            </v-select>
        </b-field>
    </section>
</template>

<script>
  import countriesEnum from '../enums';
  import Vue from 'vue';
  import vSelect from 'vue-select';
  import 'vue-select/dist/vue-select.css';

  Vue.component('v-select', vSelect);

  export default {
    name: 'Select',
    data: () => ({
      country: '',
      countriesEnum,
    }),
    watch: {
      country() {
        this.$parent.country = this.country;
        if (!this.country)
          this.$parent.csvData = null;
      },
    },
  };
</script>

<style scoped>
    .select-component >>> .vs__selected {
        line-height: 1.5;
        font-weight: 600;
    }

    .select-component >>> .vs__dropdown-menu {
        line-height: 1.5;
        font-weight: 600;
    }

    .select-component {
        width: 100%;
    }

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
