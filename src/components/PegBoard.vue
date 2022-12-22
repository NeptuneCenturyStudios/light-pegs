<template>
    <!-- Main wrapper -->
    <div class="light-pegs">
        <!-- This div contains all the holes we can drop pins on -->
        <div class="peg-board">
            <!-- Render the holes -->
            <template v-for="(row, rowIndex) in ROWS" :key="row">
                <template v-for="col in (rowIndex % 2 === 0 ? EVEN_COLUMNS : ODD_COLUMNS)" :key="col">
                    <div class="hole" draggable="false" @dragover="dragPegOverHole" @drop="droppedPegOnHole">

                    </div>

                </template>
                <!-- Force holes to next row -->
                <div class="separater"></div>

            </template>


        </div>

        <div class="pin-tray">
            <div class="pin-box" >
                <div v-for="pegColor in pegColors" :key="pegColor" class="peg" :class="pegColor" draggable="true" @dragstart="startDraggingPeg"></div>
            </div>

        </div>
    </div>

</template>

<script>

const MAX_HOLES = 1735;
const ROWS = 39;
const EVEN_COLUMNS = 44;
const ODD_COLUMNS = 45;

export default {
    name: "PegBoard",
    data() {
        return {
            ROWS,
            EVEN_COLUMNS,
            ODD_COLUMNS,
            holes: Array.from(Array(MAX_HOLES)),
            holeIndex: 0,
            draggingPeg: null,
            pegColors: [
                "peg-blue",
                "peg-green",
                "peg-violet",
                "peg-red",
                "peg-orange",
                "peg-pink",
                "peg-yellow",
                "peg-clear"
            ]
        };
    },
    mounted() {

    },
    methods: {
        startDraggingPeg(ev) {
            this.draggingPeg = ev.target;

            // Set drag image
            ev.dataTransfer.setDragImage(this.draggingPeg, 10, 10);
            // Set data to work with when peg is dropped. In this case, it will be the peg
            // because we are going to clone it.
            ev.dataTransfer.setData("peg-class", this.draggingPeg.classList);

        },
        dragPegOverHole(ev) {
            // Allows drop
            ev.preventDefault();

            // If the peg is being dragged from a hole, we need to remove it
            // from the hole.
            this.removePegFromHole(this.draggingPeg);

        },
        droppedPegOnHole(ev) {
            // Get class list of peg we started dragging
            const classList = ev.dataTransfer.getData("peg-class");
            // Create a peg div and make it a child of the hole
            const peg = this.createPeg(classList);
            // Get the hole we dropped the peg on
            const hole = ev.target;
            // Place the peg in the hole
            hole.appendChild(peg);
        },
        removePegFromHole(peg) {
            // If the peg is being dragged from a hole, we need to remove it
            // from the hole.
            if (peg.parentNode && !peg.parentNode.classList.contains("pin-box")) {
                peg.remove();
            }
        },
        createPeg(classList) {
            // Create a peg div and make it a child of the hole
            const peg = document.createElement("div");
            // Get class list of peg we started dragging
            peg.classList = classList;
            peg.draggable = true;
            peg.ondragstart = this.startDraggingPeg;
            return peg;
        }
    }
}
</script>

<style lang="scss">
$peg-shadow: 0 0 0.25rem 0.075rem;
// blue, green, violet, red, orange, pink, yellow, clear
$blue-on: rgb(0, 0, 255);
$blue-off: rgba(0, 0, 127, .75);
$green-on: rgb(0, 255, 0);
$green-off: rgba(0, 127, 0, .75);
$violet-on: rgb(127, 0, 255);
$violet-off: rgba(64, 0, 127, .75);
$red-on: rgb(255, 0, 0);
$red-off: rgba(127, 0, 0, .75);
$orange-on: rgb(255, 127, 0);
$orange-off: rgba(127, 64, 0, .75);
$pink-on: rgb(255, 0, 127);
$pink-off: rgba(127, 0, 127, .75);
$yellow-on: rgb(255, 255, 0);
$yellow-off: rgba(127, 127, 0, .75);
$clear-on: rgb(255, 255, 255);
$clear-off: rgba(127, 127, 127, .75);

.light-pegs {
    display: flex;
    flex-direction: column;
    width: 75rem;
}

.light-pegs .peg-board {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-bottom: 2rem;
    background-color: #0a0a0a;
    padding: 1rem 0;
}

.light-pegs .peg-board .hole {
    background-color: #000;
    border-radius: 100%;
    width: 1.25rem;
    height: 1.25rem;
    margin: .04rem .2rem;
}

.light-pegs .peg-board .separater {
    width: 100%;
}

.light-pegs .pin-tray {
    margin-top: auto;
}

.light-pegs .pin-tray .pin-box {
    background-color: #000;
    width: 100%;
    padding: 1rem;
    display: flex;
    justify-content: space-around;
}

.light-pegs .peg {
    border-radius: 100%;
    width: 1.25rem;
    height: 1.25rem;
}

// Blue
.light-pegs .peg.peg-blue {
    background-color: $blue-off;
}

.light-pegs .peg-board .hole .peg.peg-blue {
    background-color: $blue-on;
    box-shadow: $peg-shadow $blue-off;
}

// Green
.light-pegs .peg.peg-green {
    background-color: $green-off;
}

.light-pegs .peg-board .hole .peg.peg-green {
    background-color: $green-on;
    box-shadow: $peg-shadow $green-off;
}

// Violet
.light-pegs .peg.peg-violet {
    background-color: $violet-off;
}

.light-pegs .peg-board .hole .peg.peg-violet {
    background-color: $violet-on;
    box-shadow: $peg-shadow $violet-off;
}

// Red
.light-pegs .peg.peg-red {
    background-color: $red-off;
}

.light-pegs .peg-board .hole .peg.peg-red {
    background-color: $red-on;
    box-shadow: $peg-shadow $red-off;
}

// Orange
.light-pegs .peg.peg-orange {
    background-color: $orange-off;
}

.light-pegs .peg-board .hole .peg.peg-orange {
    background-color: $orange-on;
    box-shadow: $peg-shadow $orange-off;
}

// Pink
.light-pegs .peg.peg-pink {
    background-color: $pink-off;
}

.light-pegs .peg-board .hole .peg.peg-pink {
    background-color: $pink-on;
    box-shadow: $peg-shadow $pink-off;
}

// Yellow
.light-pegs .peg.peg-yellow {
    background-color: $yellow-off;
}

.light-pegs .peg-board .hole .peg.peg-yellow {
    background-color: $yellow-on;
    box-shadow: $peg-shadow $yellow-off;
}

// Clear
.light-pegs .peg.peg-clear {
    background-color: $clear-off;
}

.light-pegs .peg-board .hole .peg.peg-clear {
    background-color: $clear-on;
    box-shadow: $peg-shadow $clear-off;
}
</style>