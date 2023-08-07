<template>
  <dialog id="dial" >
    <form>
      <p>{{ dialogMsg }}</p>
      <button class="green" formmethod="dialog">OK</button>
    </form>
  </dialog>
  <div class="cloud">
    <p><span class="extra euro">{{ difference }}</span> still needed for this project</p>
  </div>
  <progress :max="moneyNeeded" :value="moneyDonated"/>
  <div class="box">
    <p><span class="extra orange">Only {{ daysLeft }} days left</span> to fund this project.</p>
    <p>Join the <span class="extra">{{ nrDonors }}</span> other donors who have already supported this project. Every euro helps.</p>
    <div class="buttons">
      <span class="currencyInput">{{ currency }}<input id="currency" type="number" @change="checkNumber" :value="donation" placeholder="50"/></span>
      <button class="green" @click="donateNow" :disabled="isReached">Give Now</button>
    </div>
  </div>
  <div class="buttons">
    <button @click="setModal('Saved!')">Save for later</button>
    <button @click="setModal('Yeah! I just donated.')">Tell your friends</button>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue"

const currency = '€'
const moneyNeeded = 1000
const daysLeft = 3

const nrDonors = ref<number>(42)
const moneyDonated = ref<number>(100)

const donation = ref<number>(50)
const dialogMsg = ref<string>('')
const difference = computed(() => moneyNeeded - moneyDonated.value )
const isReached = computed(() => moneyDonated.value >= moneyNeeded ) 

function setModal (msg: string) {
  dialogMsg.value = msg
  const dialogModal = document.getElementById("dial") as HTMLDialogElement
  dialogModal.showModal()
}

function checkNumber (val: Event) {
  const target = val.target as HTMLInputElement
  const value: number = parseFloat(target?.value)

  if ( value < 1 ) {
    setModal(`Minimum donation alowed: ${currency}1`)
    return donation.value = 1
  }
  if ( value > 1000 ) {
    setModal(`Maximum donation alowed: ${currency}1000`)
    return donation.value = 1000
  }
  if ( !Number.isInteger(value)) setModal("We accept round amounts only.\nYour donation will be set to " + currency + Math.floor(value) + ".")
  donation.value = Math.floor(value)
}

function donateNow () {
  if (donation.value > difference.value) {
    setModal("Your donation is bigger than we need. We will reduce your donation to " + currency + difference.value)
    return donation.value = difference.value
  }
  moneyDonated.value += donation.value
  nrDonors.value++
}
</script>

<style scoped>
dialog { border: none }
dialog form{ 
  display: flex;
  flex-direction: column;
  min-width: 300px;
}
dialog form p { text-align: center; }
.cloud {
  border-radius: 8px;
  background-color: rgb(72, 67, 67);
  color: white;
  padding: 4px;
  margin-bottom: 8px;
  position: relative;
  text-align: center;
}
.cloud::after {
  content: '';
  position: absolute;
  width: 12px;
  height: 12px;
  transform: rotate(45deg);
  bottom: -6px;
  right: 20px;
  background-color: rgb(72, 67, 67);
}
.box {
  padding: 1em;
  border: 1px solid lightgrey;
  margin-bottom: 16px;
}
.euro::before { content: '€'; }
.extra.orange { color: orangered; }
.extra { font-weight: bold; }
progress { 
  width: 100%;
  vertical-align: bottom;
  border: 1px solid lightgray;
  border-bottom: none;
  -webkit-appearance: none;
  appearance: none;
}
progress::-webkit-progress-bar { background-color: rgb(249, 240, 240); }
progress::-webkit-progress-value { background-color: orangered; }
.currencyInput {
  display: inline-flex;
  padding: 8px;
  border: 1px solid lightgrey;
  border-radius: 4px;
  align-items: center;
}
.currencyInput input { 
  border: none;
  outline: none;
  height: 100%;
  font-weight: bolder;
}
.buttons {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 24px;
}
button.green { 
  color: white; 
  background-color: rgb(20, 161, 20);
  border-bottom: 2px solid rgb(20, 161, 20);
}
button.green:disabled,
button.green[disabled] {
  background-color: rgb(72, 67, 67);
  border-bottom: 2px solid rgb(72, 67, 67);
}
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  appearance: textfield;
}
</style>