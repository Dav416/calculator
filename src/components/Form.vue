<template>
    <div>
        <form action="">
            <div class="container">
                <h1>Calculadora</h1>
                <div class="selectGroup">
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
                    <label for="selectFigure">
                        Selecciona un tipo de cálculo
                    </label>
                    <select v-model="typeOfCalculation" name="selectTypeOfCalculation" id="selectTypeOfCalculation" @change="handleSelectedTypeOfCalculation">
                        <option value="" selected disabled> Selecciona una opción </option>
                        <option value="area">Área</option>
                        <option value="perimeter">Perímetro</option>
                        <option value="volume">Volumen</option>
                    </select>
                </div>
                <div v-if="selectedFigure !== '' && typeOfCalculation !== ''" class="figuresContainer">
                    <label>                        
                        {{ figuresLabelText }} {{ selectedFigureText }}
                    </label>
                    <div v-if="selectedFigure === 'triangle'">
                        <div v-if="typeOfCalculation === 'area'" class="doubleSideFigures">
                            <InputFile 
                                label="Base"
                                v-model="side1"
                                id="triangleBase" 
                                name="triangleBase" 
                            />
                            <InputFile 
                                label="Altura"
                                v-model="side2"
                                id="triangleHeight"
                                name="triangleHeight"
                            />
                        </div>
                        <div v-else-if="typeOfCalculation === 'perimeter'" class="tripleSideFigures">
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
                        <div v-else-if="typeOfCalculation === 'volume'">
                            <p style="color:greenyellow">La opción <strong>Volumen</strong> solo funciona con la figura <strong>Prisma.</strong></p> 
                        </div>
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
                <div v-if="perimiter !== 0 || volume !== 0">
                    <p>{{ calculateResultLabel }}</p>
                    <p><strong>{{ calculateResultValue }}</strong></p>
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
    .selectGroup {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    select {
        padding: 10px 0px;
        margin: 8px 0;
        border: 1px solid #ccc;
        border-radius: 4px;
    }

    option {
        text-align: center;
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

    p {
        font-size: 1.2rem;
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
import confetti from 'canvas-confetti';

const typeOfCalculation = ref('')
const perimiter = ref(0)
const volume = ref(0)
const area = ref(0)
const side1 = ref(0)
const side2 = ref(0)
const side3 = ref(0)
const radius = ref(0)
const pi = Math.PI
const selectedFigure = ref('')
const selectedFigureText = ref('')

const showConfetti = () => {
    confetti({
        particleCount: 100, // Número de partículas
        spread: 200,         // Ángulo de dispersión
        origin: { y: 0.6 }, // Posición del confeti en la pantalla
    })
}

// const figuresLabelText = computed(() => {
//     if (selectedFigure.value === 'circle') {
//         return 'Ingrese el radio de la figura'
//     } else if (selectedFigure.value === 'prisma') {
//         return 'Ingrese la base y la altura de la figura'
//     } else {
//         return 'Ingrese los lados de la figura'
//     }
// })

const figuresLabelText = computed(() => {
    if (typeOfCalculation.value === 'area' || selectedFigure.value === 'prisma') {
        return 'Ingrese la base y la altura de la figura'
    } else {
        return 'Ingrese los lados de la figura'
    } 
})

const calculateResultLabel = computed(() => {
    if (volume.value !== 0) {
        return `El volumen de la figura ${selectedFigureText.value} es de ` 
    }
    return `El perímetro de la figura ${selectedFigureText.value} es de `
})

const calculateResultValue = computed(() => {
    if (volume.value !== 0) {
        return `${volume.value} cm³`
    }
    return `${perimiter.value} cm`
    
})

function handleSelectedFigure(event) {
    selectedFigureText.value = event.target.selectedOptions[0].text
    resetInputs()
}

function handleSelectedTypeOfCalculation() {
    resetInputs()
}

function resetInputs() {
    perimiter.value = 0
    volume.value = 0
    area.value = 0
    side1.value = 0
    side2.value = 0
    side3.value = 0
    radius.value = 0
}

function Calculate() {
    let calculationCompleted = false
    switch (selectedFigure.value) {
        case 'triangle':
            perimiter.value = side1.value + side2.value + side3.value
            calculationCompleted = true
            break;
        case 'rectangle':
            perimiter.value = 2 * side1.value + 2 * side2.value
            calculationCompleted = true
            break;
        case 'square':
            perimiter.value = 4 * side1.value
            calculationCompleted = true
            break;
        case 'circle':
            perimiter.value = 2 * pi * radius.value
            calculationCompleted = true
            break;
        case 'prisma':
            volume.value = side1.value * side2.value
            calculationCompleted = true
            break;
        default:
            console.log('No se seleccionó ninguna figura')
    }
    if (calculationCompleted) {
        if (perimiter.value !== 0 || volume.value !== 0) {
            showConfetti()
        }
    }
}
</script>