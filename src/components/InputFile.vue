<script setup>
    import { defineProps } from 'vue';

    defineProps ({
        label: {
            type: String,
            required: true
        },
        modelValue: {
            type: [String, Number],
            required: true
        },
        id: {
            type: String,
            required: true
        },
        name: {
            type: String,
            required: true
        }
    })
</script>
<template>
    <label :for="id">
        {{ label }}
        <input 
        type="number"
        :id="id"
        :name="name"
        :value="modelValue"
        @input="$emit('update:modelValue', Number ($event.target.value))"    
        >
    </label>
</template>

<style scoped>
    input {
        display: block;
        padding: 10px 20px;
        margin: 8px 0;
        border: 1px solid #ccc;
        border-radius: 4px;
        text-align: center;
    }
</style>
