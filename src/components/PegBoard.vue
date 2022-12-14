<template>
    <div class="peg-board">

        <template v-for="(hole, index) in holes" :key="index">
            <div class="hole">
            {{ hole }}
            </div>
        </template>
        

    </div>
</template>

<script>
const MAX_HOLES = 1735;


export default {
    name: "PegBoard",
    data() {
        return {
            holes: Array.from(Array(MAX_HOLES))
        };
    },
    mounted() {

    }
}
</script>

<style lang="scss">
.peg-board {
    display: flex;
flex-wrap: wrap;
}

.peg-board .hole {
    background-color: #000;
    border-radius: 100%;
    width: 1rem;
    height: 1rem;
}
</style>