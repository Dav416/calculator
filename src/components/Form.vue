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
                <div v-if="selectedFigure !== ''" class="figuresContainer">
                    <label>                        
                        {{ figuresLabelText }} {{ selectedFigureText }}
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
                    <div class="rectangleSides" v-if="selectedFigure === 'rectangle'">
                        <label for="rectangleSide1">
                            Lado 1 (n^2)
                            <input type="number" v-model="side1" name="rectangleSide1" id="rectangleSide1">
                        </label>
                        <label for="rectangleSide2">
                            Lado 2 (n^2)
                            <input type="number" v-model="side2" name="rectangleSide2" id="rectangleSide2">
                        </label>
                    </div>
                    <div class="otherFiguresSides" v-if="selectedFigure === 'square'">
                        <label for="squareSide1">
                            Lado (n^4)
                            <input type="number" v-model="side1" name="squareSide1" id="squareSide1">
                        </label>
                    </div>
                    <div class="otherFiguresSides" v-if="selectedFigure === 'circle'">
                        <label for="circleRadius">
                            Radio
                            <input type="number" v-model="radius" name="circleRadius" id="circleRadius">
                        </label>
                    </div>
                </div>
                <div class="results" v-if="perimiter !== 0">
                    <label for="perimeter">
                        El perímetro de la figura {{ selectedFigureText }} es:
                    </label>
                    <input type="number" v-model="perimiter" name="perimeter" id="perimeter" readonly>
                </div>
                <div class="buttonsGroup">                        
                    <button v-if="perimiter !== 0" @click="resetInputs" type="button">Limpiar</button>
                    <button @click="Calculate" type="button">Calcular</button>
                </div>
            </div>
         </form>
    </div>
</template>

<style scoped>
    .container * {
        text-align: center;
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
    }

    .rectangleSides {
        display: grid;
        grid-template-columns: 1fr 1fr;
        justify-items: center;
        gap: 1em;
    }

    .otherFiguresSides {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    button {
        padding: 8px 20px;
        margin: 8px 0;
        border: 1px solid #ccc;
        border-radius: 4px;
        cursor: pointer;
    }

    .results {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .buttonsGroup {
        display: flex;
        justify-content: space-evenly;
        align-items: center;
    }

</style>
<script setup>
import { ref, computed } from 'vue'

const selectedFigure = ref('')
const selectedFigureText = ref('')
const figuresLabelText = computed(() => {
    return selectedFigure.value === 'circle'
    ? 'Ingrese el radio de la figura'
    : 'Ingrese los lados de la figura'
})
const perimiter = ref(0)
const side1 = ref(0)
const side2 = ref(0)
const side3 = ref(0)
const radius = ref(0)
const pi = Math.PI

function handleSelectedFigure(event) {
    selectedFigureText.value = event.target.selectedOptions[0].text
    resetInputs()
}

function resetInputs() {
    side1.value = 0
    side2.value = 0
    side3.value = 0
    radius.value = 0
    perimiter.value = 0
}

function Calculate() {
    switch (selectedFigure.value) {
        case 'triangle':
            perimiter.value = side1.value + side2.value + side3.value
            break;
        case 'rectangle':
            perimiter.value = 2 * side1.value + 2 * side2.value
            break;
        case 'square':
            perimiter.value = 4 * side1.value
            break;
        case 'circle':
            perimiter.value = 2 * pi * radius.value
            console.log(pi)
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