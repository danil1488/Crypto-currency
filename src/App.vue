<template>
  <h1>
    <span style="--i: 1">C</span>
    <span style="--i: 2">R</span>
    <span style="--i: 3">Y</span>
    <span style="--i: 4">P</span>
    <span style="--i: 5">T</span>
    <span style="--i: 6">O</span>
  </h1>
  <Input :changeAmount="changeAmount" :convert="convert" />
  <p v-if="error !== ''">{{ error }}</p>
  <p v-if="result !== ''" class="result-text">{{ result }}</p>
  <div class="selectors">
    <Selector :setCrypto="setCryptoFirst" />
    <Selector :setCrypto="setCryptoSecond" />
  </div>
  <CryptoTableRub />
  <CryptoTable />
</template>

<script>
import Input from './components/Input.vue'
import Selector from './components/Selector.vue'
import CryptoConvert from 'crypto-convert'
import CryptoTable from './components/CryptoTable.vue'
import CryptoTableRub from './components/CryptoTableRub.vue'

const convert = new CryptoConvert()

export default {
  components: { Input, Selector, CryptoTable, CryptoTableRub },
  data() {
    return {
      amount: 0,
      crypoFirst: '',
      cryptoSecond: '',
      error: '',
      result: 0,
    }
  },
  methods: {
    changeAmount(val) {
      this.amount = val
    },
    setCryptoFirst(val) {
      this.crypoFirst = val
    },
    setCryptoSecond(val) {
      this.cryptoSecond = val
    },
    async convert() {
      if (this.amount <= 0) {
        this.error = 'Введите число больше нуля'
        return
      } else if (this.crypoFirst === '' || this.cryptoSecond === '') {
        this.error = 'Выберите валюту'
        return
      } else if (this.crypoFirst === this.cryptoSecond) {
        this.error = 'Выберите разные валюты'
        return
      }
      this.error = ''
      await convert.ready()

      if (this.crypoFirst === 'BTC' && this.cryptoSecond === 'ETH')
        this.result = convert.BTC.ETH(this.amount)
      else if (this.crypoFirst === 'BTC' && this.cryptoSecond === 'USDT') {
        this.result = convert.BTC.USDT(this.amount)
      } else if (this.crypoFirst === 'ETH' && this.cryptoSecond === 'BTC') {
        this.result = convert.ETH.BTC(this.amount)
      } else if (this.crypoFirst === 'ETH' && this.cryptoSecond === 'USDT') {
        this.result = convert.ETH.USDT(this.amount)
      } else if (this.crypoFirst === 'USDT' && this.cryptoSecond === 'BTC') {
        this.result = convert.USDT.BTC(this.amount)
      } else if (this.crypoFirst === 'USDT' && this.cryptoSecond === 'ETH') {
        this.result = convert.USDT.ETH(this.amount)
      }
    },
  },
}
</script>

<style scoped>
p {
  font-family: 'Tomorrow', sans-serif;
  font-weight: 500;
  font-size: 30px;
  color: #fff;
  caret-color: transparent;
}
.selectors {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin: 20px auto;
  gap: 20px;
  width: 100%;
  caret-color: transparent;
}

/* Анимация волны */
@keyframes wave {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

h1 span {
  display: inline-block;
  animation: wave 1s infinite;
  animation-delay: calc(0.1s * var(--i));
  caret-color: transparent;
}

/* Адаптивность для мобильных устройств */
@media (max-width: 768px) {
  .selectors {
    flex-direction: column;
    align-items: center;
  }

  .result-text {
    font-size: 1.2em;
    text-align: center;
  }
}
</style>
