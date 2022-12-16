<template>
    <!-- Main wrapper -->
    <div class="light-pegs">
        <!-- This div contains all the holes we can drop pins on -->
        <div class="peg-board">
            <!-- Render the holes -->
            <template v-for="(row, rowIndex) in ROWS" :key="row">
                <template v-for="col in (rowIndex % 2 === 0 ? EVEN_COLUMNS : ODD_COLUMNS)" :key="col">
                    <div class="hole">
                        <div class="peg peg-green"></div>
                    </div>

                </template>
                <!-- Force holes to next row -->
                <div class="separater"></div>

            </template>


        </div>

        <div class="pin-tray">
            <div class="pin-box">
                <div class="peg peg-green"></div>
            </div>
            <div class="pin-box">

            </div>
            <div class="pin-box">

            </div>
            <div class="pin-box">

            </div>
            <div class="pin-box">

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
            holes: Array.from(Array(MAX_HOLES))
        };
    },
    mounted() {

    },
    methods: {
        // Gets the current row index based on the hole index
        getCurrentRowIndex(holeIndex) {
            // Based on the hole index, we can calculate which row we are on.
            let even = true;
            let counter = 0;
            let rowIndex = 0;
            while (counter <= holeIndex) {
                // Count the number of time we can alternately fit 44 and 45 in the holeIndex
                counter += even ? 44 : 45;
                if (counter > holeIndex) {
                    // Since we are still on the first row, break
                    break;
                }

                // flip even and odd
                even = !even;

                // Count the row if counter is less than the current hole index, meaning we
                // have at least one row already
                rowIndex++;
            }
            console.log(rowIndex)
            return rowIndex;
        }
    }
}
</script>

<style lang="scss">
$peg-shadow: 0 0 0.25rem 0.075rem;
$green-on: rgb(0, 255, 00);
$green-off: rgba(0, 127, 00, .75);

.light-pegs {
    display: flex;
    flex-direction: column;
}

.light-pegs .peg-board {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-bottom: 2rem;
}

.light-pegs .peg-board .hole {
    background-color: #000;
    border-radius: 100%;
    width: 1.25rem;
    height: 1.25rem;
    margin: .075rem;
}

.light-pegs .peg-board .separater {
    width: 100%;
}

.light-pegs .pin-tray {
    display: flex;
    margin-top: auto;
    justify-content: space-around;
}

.light-pegs .pin-tray .pin-box {
    background-color: #000;
    width: 10rem;
    padding: 1rem;

}

.light-pegs .peg {
    border-radius: 100%;
    width: 1.25rem;
    height: 1.25rem;
}

.light-pegs .peg.peg-green {
    background-color: $green-off;
}

.light-pegs .peg-board .hole .peg.peg-green {
    background-color: $green-on;
    box-shadow: $peg-shadow $green-off;
}
</style>