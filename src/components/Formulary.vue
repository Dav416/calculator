<template>
    <div>
        <form action="">
            <div class="container">
                <h1>Calculadora</h1>
                <label for="selectFigure">
                    Selecciona una figura
                </label>
                <select v-model="selectedFigure" @change=" handleSelectedFigure" name="selectFigure" id="selectFigure">
                    <option value="" selected disabled>Figuras Planas</option>
                    <option value="triangle">Triángulo</option>
                    <option value="rectangle">Rectángulo</option>
                    <option value="square">Cuadrado</option>
                    <option value="circle">Círculo</option>
                    <option disabled>Figuras Tridimensionales</option>
                    <option value="prisma">Prisma</option>
                    <option value="octohedron">Octaedro</option>
                </select>
                <div class="figuresContainer">
                    <label>
                            Ingrese los lados de la figura {{ selectedFigureText }}
                    </label>
                    <div class="triangleSides" v-if="selectedFigure === 'triangle'">
                        <label for="trianguleSide1">
                            Lado 1
                            <input type="number" v-model="side1" name="trianguleSide1" id="trianguleSide1">
                        </label>
                        <label for="trianguleSide2">
                            Lado 2
                            <input type="number" v-model="side2" name="trianguleSide2" id="trianguleSide2">
                        </label>
                        <label for="trianguleSide3">
                            Lado 3
                            <input type="number" v-model="side3" name="trianguleSide3" id="trianguleSide3">
                        </label>
                    </div>
                    <div class="results" v-if="perimiter !== ''">
                        <label for="perimeter">
                            El perímetro de la figura {{ selectedFigureText }} es:
                            <input type="number" v-model="perimiter" name="perimeter" id="perimeter" readonly>
                        </label>
                    </div>
                    <button @click="Calculate" type="button">Calcular</button>
                </div>
            </div>
         </form>
    </div>
</template>

<style scoped>
    .container {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    select {
        padding: 10px 20px;
        margin: 8px 0;
        border: 1px solid #ccc;
        border-radius: 4px;
    }

    label {
        display: block;
        margin: 8px 0;
    }

    input {
        display: block;
        padding: 10px 20px;
        margin: 8px 0;
        border: 1px solid #ccc;
        border-radius: 4px;
    }

    .triangleSides {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr;
        justify-items: center;
        gap: 1em;
    }
    .triangleSides input {
        max-width: 100px;
        text-align: center;
    }
    button {
        padding: 8px 20px;
        margin: 8px 0;
        border: 1px solid #ccc;
        border-radius: 4px;
        cursor: pointer;
    }

</style>
<script setup>
import { ref } from 'vue'

const selectedFigure = ref('')
const selectedFigureText = ref('')
const perimiter = ref('')
const side1 = ref('')
const side2 = ref('')
const side3 = ref('')

function handleSelectedFigure(event) {
    selectedFigureText.value = event.target.selectedOptions[0].text
    resetInputs()
}

function resetInputs() {
    side1.value = ''
    side2.value = ''
    side3.value = ''
    perimiter.value = ''
}

function Calculate() {
    console.log('Calcular')
    switch (selectedFigure.value) {
        case 'triangle':
        perimiter.value = side1.value + side2.value + side3.value
            console.log('Triángulo')
            console.log(side1.value)
            console.log(side2.value)
            console.log(side3.value)
            break;
        case 'square':
            console.log('Cuadrado')
            break;
        case 'rectangle':
            console.log('Rectángulo')
            break;
        case 'circle':
            console.log('Círculo')
            break;
        case 'prisma':
            console.log('Prisma')
            break;
        case 'octohedron':
            console.log('Octaedro')
            break;
        default:
            console.log('No se seleccionó ninguna figura')
    }
}
</script>