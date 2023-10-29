<template>
<div :class="$style.wrapper">
    <div :class="$style.title">
        BMI Calculator
    </div>
    <div :class="$style.intro">
        Welcome to our super friendly BMI calculator. Don't wonder if you're fat anymore,
        just plug in those big numbers below :)
    </div>
    <div :class="$style.chooseUnit">
        <p>Please choose your preferred unit system below:</p>
        <label><input type="radio" value="metric" :class="$style.metric" v-model="unitSystem" />Metric</label>
        <label><input type="radio" value="imperial" :class="$style.imperial" v-model="unitSystem" />Imperial</label>
    </div>
    <div :class="$style.weightContainer">
        <label>Enter your weight in {{ weightUnit }}:
            <input type="text" v-model="weight" />
        </label>
        <label>Enter your height in {{ heightUnit }}:
            <input type="text" v-model="height" />
        </label>
        <button :class="$style.submitButton" @click="submitButtonClicked">Submit</button>
    </div>
    <div v-if="hasSubmitted">
        Your BMI is {{ bmi.toFixed(2) }}.
        <p v-if="bmi < 18.5">
            You are underweight. Please seek medical advice.
        </p>
        <p v-else-if="bmi < 25">
            Congratulations, you have a healthy weight.
        </p>
        <p v-else-if="bmi < 40">
            You are overweight. Please seek medical advice.
        </p>
        <p v-else>
            You are obese (extremely fat). Please seek medical advice immediately.
        </p>
    </div>
</div>
</template>

<script>

export default {
    name: 'BmiCalculator',
    data() {
        return {
            weight: '',
            height: '',
            hasSubmitted: false,
            bmi: 0,
            unitSystem: 'metric',
        }
    },
    watch: {
        unitSystem(newValue, oldValue) {
            const weightNum = Number(this.weight)
            const heightNum = Number(this.height)
            if (newValue === 'imperial') {
                this.weight = (weightNum * 2.20462).toFixed(2)
                this.height = (heightNum * 39.3701).toFixed(2)
            } else if (newValue === 'metric') {
                this.weight = (weightNum / 2.20462).toFixed(2)
                this.height = (heightNum / 39.3701).toFixed(2)
            }
        }
    },
    computed: {
        weightUnit() {
            if (this.unitSystem === 'imperial') {
                return 'pounds'
            } 
            return 'kilograms'
        },
        heightUnit() {
            if (this.unitSystem === 'imperial') {
                return 'inches'
            }
            return 'meters'
        },
    },
    methods: {
        submitButtonClicked() {
            this.hasSubmitted = true
            if (this.unitSystem === 'imperial') {
                this.bmi = this.calculateBmi(703)
            } else {
                this.bmi = this.calculateBmi(1)
            }
        },
        calculateBmi(factor) {
            let bmi = factor * this.weight / (this.height * this.height) 
            return bmi
        },
    },
}
</script>

<style module src="./styles.module.css"/>