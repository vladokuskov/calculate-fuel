<script setup>
import { ref, watch, onMounted } from 'vue';

const cars = [
    { name: 'Renault Scenic II', cityFuelConsumption: 8, highwayFuelConsumption: 6.4 },
];

const selectedCar = ref(cars[0]);
const fuelPriceForLiter = ref(44) // UAH
const selectedRangeType = ref('city');
const range = ref(0);

const fuelConsumption = ref(0);
const totalFuelPrice = ref(0);

const updateFuelConsumption = () => {
    if (selectedRangeType.value === 'city') {
        fuelConsumption.value = selectedCar.value.cityFuelConsumption;
    } else {
        fuelConsumption.value = selectedCar.value.highwayFuelConsumption;
    }
    calculateTotalFuelPrice();
};

const calculateTotalFuelPrice = () => {
    if (range.value >= 0) {
        totalFuelPrice.value = ((range.value * fuelConsumption.value) / 100) * fuelPriceForLiter.value;
    }
};


onMounted(() => {
    const rangeType = localStorage.getItem('rangeType', selectedRangeType);
    const savedRange = localStorage.getItem('range', selectedRangeType);


    if (savedRange) {
        range.value = JSON.parse(savedRange)
    }

    if (rangeType) {
        selectedRangeType.value = JSON.parse(rangeType)
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
</script>

<template>
    <div class="form-wrapper">
        <h1>Fuel Price ⛽</h1>
        <section class="car-selection">
            <label for="car">Select Car:</label>
            <select v-model="selectedCar" @change="updateFuelConsumption" name="car">
                <option v-for="car in cars" :key="car.name" :value="car">
                    {{ car.name }}
                </option>
            </select>
        </section>
        <section class="range-type">
            <label for="range">Range Type:</label>
            <select v-model="selectedRangeType" name="range">
                <option value="city">City</option>
                <option value="highway">Highway</option>
            </select>
        </section>
        <section class="range-distance">
            <label for="distance">Range (km):</label>
            <input type="number" name="distance" v-model="range" @input="calculateTotalFuelPrice" />
        </section>
        <section class="fuel-consumption">
            <p>Fuel Consumption (L/100km): {{ fuelConsumption }}</p>
        </section>
        <section class="total-fuel-price">
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
    justify-content: center;
    gap: .5rem;
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