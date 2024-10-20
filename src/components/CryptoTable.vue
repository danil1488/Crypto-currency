<template>
  <div class="crypto-table">
    <h2>Top Currency (USD)</h2>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price (USD)</th>
          <th>Change (24h)</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="crypto in cryptocurrencies" :key="crypto.id">
          <td>{{ crypto.name }}</td>
          <td>${{ crypto.current_price.toFixed(2) }}</td>
          <td
            :class="{
              positive: crypto.price_change_percentage_24h > 0,
              negative: crypto.price_change_percentage_24h < 0,
            }"
          >
            {{ crypto.price_change_percentage_24h.toFixed(2) }}%
          </td>
        </tr>
      </tbody>
    </table>
    <p v-if="error">{{ error }}</p>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      cryptocurrencies: [],
      error: '',
    }
  },
  mounted() {
    this.fetchCryptocurrencies()
  },
  methods: {
    async fetchCryptocurrencies() {
      try {
        const response = await axios.get(
          'https://api.coingecko.com/api/v3/coins/markets',
          {
            params: {
              vs_currency: 'usd',
              order: 'market_cap_desc',
              per_page: 10,
              page: 1,
              sparkline: false,
            },
          }
        )
        this.cryptocurrencies = response.data
      } catch (error) {
        this.error = 'Не удалось загрузить данные о криптовалютах.'
      }
    },
  },
}
</script>

<style scoped>
.crypto-table {
  width: 100%;
  margin: 20px 0;
  text-align: center;
  overflow-x: auto; /* Горизонтальная прокрутка для маленьких экранов */
}

table {
  width: 100%;
  border-collapse: collapse;
  font-size: 1em;
}

th,
td {
  padding: 10px;
  border: 1px solid #1a032d;
  font-family: 'Tomorrow', sans-serif;
  font-weight: 500;
  font-style: normal;
  align-items: center;
}
th {
  background: #1a032d;
  color: #fff;
  align-items: center;
  opacity: 0.5;
}
h2 {
  font-family: 'Tomorrow', sans-serif;
  font-weight: 500;
  font-style: normal;
  align-items: center;
  opacity: 0.5;
  caret-color: transparent;
}

/* Адаптивность для мобильных устройств */
@media (max-width: 768px) {
  .crypto-table {
    width: 100%;
  }

  table {
    font-size: 0.9em;
  }
}
</style>
