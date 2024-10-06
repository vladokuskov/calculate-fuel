<script setup>
import { ref, watch, onMounted } from 'vue';


const selectedRangeType = ref('city');
const range = ref(0);

const priceForLiter = ref(0)
const fuelConsumption = ref(0);
const totalFuelPrice = ref(0);

const updateFuelConsumption = () => {
    calculateTotalFuelPrice();
};

const calculateTotalFuelPrice = () => {
    if (range.value >= 0) {
        totalFuelPrice.value = ((range.value * fuelConsumption.value) / 100) * priceForLiter.value;
    }
};

onMounted(() => {
    const rangeType = localStorage.getItem('rangeType');
    const savedRange = localStorage.getItem('range');
    const savedPriceForLiter = localStorage.getItem('priceForLiter');

    if (savedRange) {
        range.value = JSON.parse(savedRange);
    }
    if (rangeType) {
        selectedRangeType.value = JSON.parse(rangeType);
    }
    if (savedPriceForLiter) {
        priceForLiter.value = JSON.parse(savedPriceForLiter);
    }


    updateFuelConsumption();
})

watch(selectedRangeType, () => {
    localStorage.setItem('rangeType', JSON.stringify(selectedRangeType.value));
    updateFuelConsumption();
});

watch(range, () => {
    localStorage.setItem('range', JSON.stringify(range.value));
});

watch(priceForLiter, () => {
    localStorage.setItem('priceForLiter', JSON.stringify(priceForLiter.value));
    calculateTotalFuelPrice();
});
</script>

<template>
    <div class="form-wrapper">
        <h1>Fuel Price ⛽</h1>

        <section>
            <label for="price">Price for 1 L:</label>
            <input type="number" name="price" v-model="priceForLiter" @input="updateFuelConsumption" />
        </section>
        <section>
            <label for="city">Fuel Consumption AVG:</label>
            <input type="number" name="city" v-model="fuelConsumption" @input="updateFuelConsumption" />
        </section>
        <section>
            <label for="distance">Range (km):</label>
            <input type="number" name="distance" v-model="range" @input="calculateTotalFuelPrice" />
        </section>
        <section class="result-section">
            <p>Fuel Consumption (L/100km): {{ fuelConsumption }}</p>
            <p>Total Fuel Price: {{ totalFuelPrice.toFixed(0) }} UAH</p>
        </section>
    </div>
</template>

<style scoped>
.form-wrapper {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    gap: 1rem;
    border: none;
    border-radius: 12px;
    background-color: #dce1cb;
    padding: .5rem;
    color: #114971;

}

h1 {
    font: optional;
    font-family: Roboto, sans-serif;
    color: #114971;
    font-weight: 500;
    letter-spacing: -0.04rem;
    margin: 0 auto;
}

p,
label {
    font: optional;
    font-family: Roboto, sans-serif;
    letter-spacing: -0.01rem;
    font-size: .95rem;
}

section {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: .5rem;
    width: 100%;
}

.result-section {
    display: flex;
    align-items: flex-start;
    flex-direction: column;
    justify-content: center;
    gap: .5rem;
    width: 100%;
    padding-top: 1rem;
    border-top: 1px solid #114971;
}

select {
    padding: 0.25rem;
    border: none;
    border-radius: 8px;
    font-family: Roboto, sans-serif;
    font-size: 0.95rem;
    color: #114971;
    font-weight: 500;
    background-color: #c6d498;
    cursor: pointer;
    transition: all 0.2s;
}

select:hover {
    background-color: #cfdba5;
}

select:focus {
    background-color: #cfdba5;
}

select option {
    font-family: Roboto, sans-serif;
    background-color: #dde9b7;
}

select option:hover {
    font-family: Roboto, sans-serif;
    background-color: #edf5d2;
}

select:focus {
    outline: none;
}

select option:hover {
    background-color: #f2f5e7;
}

input {
    padding: 0.25rem;
    border: none;
    border-radius: 8px;
    background-color: #d7c896;
    font-family: Roboto, sans-serif;
    font-size: 0.95rem;
    color: #114971;
    font-weight: 500;
    transition: all 0.2s;
}

input:focus {
    outline: none;
    background-color: #e1d4a9;
}

input:hover {
    background-color: #e1d4a9;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

input[type=number] {
    -moz-appearance: textfield;
}
</style>