<template>
    <div class="inc-exp-container">
        <div>
            <h4>Income</h4>
            <p id="money-plus" class="money plus">+${{props.income}}</p>
        </div>
        <div>
            <h4>Expense</h4>
            <p id="money-plus" class="money minus">${{props.expenses}}</p>
        </div>
    </div>
</template>

<script setup>
import  {defineProps} from 'vue';

const props = defineProps({
    income: {
        type: Number,
        require: true
    },
    expenses: {
        type: Number,
        require: true
    }
})
</script>