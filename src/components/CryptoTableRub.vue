<template>
  <div class="crypto-table">
    <h2>Top Currency (RUB)</h2>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price (RUB)</th>
          <th>Change (24h)</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="crypto in cryptocurrencies" :key="crypto.id">
          <td>{{ crypto.name }}</td>
          <td>₽{{ crypto.current_price.toFixed(2) }}</td>
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
              vs_currency: 'rub',
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
}

table {
  width: 100%;
  border-collapse: collapse;
  align-items: center;
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

.positive {
  color: rgb(77, 204, 77);
}

.negative {
  color: rgb(226, 56, 56);
}
h2 {
  font-family: 'Tomorrow', sans-serif;
  font-weight: 500;
  font-style: normal;
  align-items: center;
  opacity: 0.5;
  caret-color: transparent;
}

@media (max-width: 768px) {
  .crypto-table {
    overflow-x: auto; /* Добавляем горизонтальную прокрутку для таблицы */
  }

  table {
    font-size: 0.8em; /* Уменьшаем размер шрифта для таблицы */
  }

  th,
  td {
    padding: 5px; /* Уменьшаем отступы в ячейках */
  }

  h2 {
    font-size: 1.5em; /* Уменьшаем размер шрифта для заголовков */
  }
}
</style>
