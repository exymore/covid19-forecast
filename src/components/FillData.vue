<template>
    <section class="section">
        <h2 class="subtitle">
            It seems data for today is missing yet. Do you want to enter it manually?
        </h2>

        <div class="group">
            <b-button class="btn" type="is-info">Yes</b-button>
            <b-button class="btn">No</b-button>
        </div>

        <div class="input-controls">
            <b-field label="😷 Infected">
                <b-numberinput v-model="infected" type="is-info" controls-position="compact"
                               controls-rounded/>
            </b-field>
            <b-field label="☠️ Dead">
                <b-numberinput v-model="dead" type="is-info" controls-position="compact"
                               controls-rounded/>
            </b-field>
            <b-field label="😊️ Recovered">
                <b-numberinput v-model="recovered" type="is-info" controls-position="compact"
                               controls-rounded/>
            </b-field>
        </div>
        <b-button type="is-primary" size="is-medium" class="submit">Get Forecast</b-button>
    </section>
</template>

<script>
  export default {
    name: 'FillData',
    data: () => ({
      infected: 0,
      dead: 0,
      recovered: 0,
    }),
  };
</script>

<style scoped>
    .input-controls {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: space-between;
        margin-top: 28px;
        margin-bottom: 2rem;
    }

    .submit {
        font-weight: 600;
    }

    .input-controls >>> .label {
        font-weight: 400;
        font-size: 1.125rem;
    }

    .group {
        display: flex;
        justify-content: space-between;
        width: 35%;
    }

    .btn {
        width: 150px;
        font-weight: 600;
    }

    @media (max-width: 960px) {
        .group {
            width: 100%;
        }

        .btn {
            width: 130px;
        }
    }

    @media (max-width: 360px) {
        .group {
            width: 100%;
        }

        .btn {
            width: 100px;
        }
    }
</style>
