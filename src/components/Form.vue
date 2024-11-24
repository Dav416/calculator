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
                </select>
                <div v-if="selectedFigure !== ''" class="figuresContainer">
                    <label>                        
                        {{ figuresLabelText }} {{ selectedFigureText }}
                    </label>
                    <div class="tripleSideFigures" v-if="selectedFigure === 'triangle'">
                        <InputFile 
                            label="Lado 1"
                            v-model="side1"
                            id="trianguleSide1"
                            name="trianguleSide1"
                        />
                        <InputFile 
                            label="Lado 2"
                            v-model="side2"
                            id="trianguleSide2"
                            name="trianguleSide2"
                        />
                        <InputFile 
                            label="Lado 3"
                            v-model="side3"
                            id="trianguleSide3"
                            name="trianguleSide3"
                        />
                    </div>
                    <div class="doubleSideFigures" v-if="selectedFigure === 'rectangle'">
                        <InputFile 
                            label="Lado 1 (2L1)"
                            v-model="side1"
                            id="rectangleSide1"
                            name="rectangleSide1"
                        />
                        <InputFile 
                            label="Lado 2 (2L2)"
                            v-model="side2"
                            id="rectangleSide2"
                            name="rectangleSide2"
                        />
                    </div>
                    <div class="oneSideFigures" v-if="selectedFigure === 'square'">
                        <InputFile 
                            label="Lado (4L)"
                            v-model="side1"
                            id="squareSide1"
                            name="squareSide1"
                        />
                    </div>
                    <div class="oneSideFigures" v-if="selectedFigure === 'circle'">
                        <InputFile 
                            label="Radio"
                            v-model="radius"
                            id="circleRadius"
                            name="circleRadius"
                        />
                    </div>
                    <div class="doubleSideFigures" v-if="selectedFigure === 'prisma'">
                        <InputFile 
                            label="Base"
                            v-model="side1"
                            id="prismaSide1"
                            name="prismaSide1"
                        />
                        <InputFile 
                            label="Altura"
                            v-model="side2"
                            id="prismaSide2"
                            name="prismaSide2"
                        />
                    </div>
                </div>
                <div v-if="perimiter !== 0">
                    <InputFile 
                        :label="`El perímetro de la figura ${selectedFigureText} es:`" 
                        v-model="perimiter"
                        id="perimeter"
                        name="perimeter"
                        class="results"
                        readonly
                    />
                </div>
                <div v-if="volume !== 0">
                    <InputFile 
                        :label="`El volumen de la figura ${selectedFigureText} es:`" 
                        v-model="volume"
                        id="volume"
                        name="volume"
                        class="results"
                        readonly
                    />
                </div>
                <div class="buttonsGroup">                        
                    <button v-if="perimiter !== 0 || volume !== 0" @click="resetInputs" type="button">Limpiar</button>
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

    .tripleSideFigures {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr;
        justify-items: center;
        gap: 1em;
    }
    .tripleSideFigures input {
        max-width: 100px;
    }

    .doubleSideFigures {
        display: grid;
        grid-template-columns: 1fr 1fr;
        justify-items: center;
        gap: 1em;
    }

    .oneSideFigures {
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
import InputFile from './InputFile.vue'

const perimiter = ref(0)
const volume = ref(0)
const side1 = ref(0)
const side2 = ref(0)
const side3 = ref(0)
const radius = ref(0)
const pi = Math.PI
const selectedFigure = ref('')
const selectedFigureText = ref('')

const figuresLabelText = computed(() => {
    if (selectedFigure.value === 'circle') {
        return 'Ingrese el radio de la figura'
    } else if (selectedFigure.value === 'prisma') {
        return 'Ingrese la base y la altura de la figura'
    } else {
        return 'Ingrese los lados de la figura'
    }
    return 'No se seleccionó ninguna figura'
})

function handleSelectedFigure(event) {
    selectedFigureText.value = event.target.selectedOptions[0].text
    resetInputs()
}

function resetInputs() {
    perimiter.value = 0
    volume.value = 0
    side1.value = 0
    side2.value = 0
    side3.value = 0
    radius.value = 0
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
            break;
        case 'prisma':
            volume.value = side1.value * side2.value
            break;
        default:
            console.log('No se seleccionó ninguna figura')
    }
}
</script>