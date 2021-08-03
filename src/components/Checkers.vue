<template>
    <div class="checkers">
        <div class="checkers__title">шашки</div>
        <div class="checkers__field">
            <div class="checkers__row" v-for="(x,indexRow) in position_x" :key="indexRow">
                <div class="checkers__cell" v-for="(y,index) in position_y" :key="index"
                     :class="{'checkers__cell_black-color': ((indexRow + 1)%2 == 0 && (index+1) %2 !=0) || ((indexRow + 1)%2 != 0 && (index+1)% 2 ==0) }">
                    <div v-if="(indexRow == 5 && (index)%2 == 0) || (indexRow == 6 && (index)%2 != 0) || (indexRow == 7 && (index)%2 == 0) ||
                                    (indexRow == 0 && (index)%2 != 0) || (indexRow == 1 && (index)%2 == 0) || (indexRow == 2 && (index)%2 != 0)"
                         class="checkers__checker" :class="{'checkers__checker_red-color':indexRow>=5}"></div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Checkers",
        data: () => {
            return {
                check: [0, 0],
                blue_checkers_position: [{x: 8, y: 2}, {x: 8, y: 4}, {x: 8, y: 6}, {x: 8, y: 8}, {x: 7, y: 1}, {
                    x: 7,
                    y: 3
                }, {x: 7, y: 5}, {x: 7, y: 7},
                    {x: 6, y: 2}, {x: 6, y: 4}, {x: 6, y: 6}, {x: 6, y: 8}],
                red_checkers_position: [{x: 1, y: 2}, {x: 1, y: 4}, {x: 1, y: 6}, {x: 1, y: 8}, {x: 2, y: 1}, {
                    x: 2,
                    y: 3
                }, {x: 2, y: 5}, {x: 2, y: 7},
                    {x: 3, y: 2}, {x: 3, y: 4}, {x: 3, y: 6}, {x: 3, y: 8}],
                position_x: [1, 2, 3, 4, 5, 6, 7, 8],
                position_y: [1, 2, 3, 4, 5, 6, 7, 8],
                board: [
                    [0, 1, 0, 2]
                ]

            }
        }
    }
</script>

<style lang="scss" scoped>
    .checkers {
        display: flex;
        justify-content: center;
        flex-direction: column;
        margin-bottom: 50px;

        &__row {
            display: flex;
        }

        &__checker {
            background-color: cornflowerblue;
            border-radius: 50%;
            width: 60px;
            height: 60px;
            margin: auto;
            margin-top: 10px;
            cursor: pointer;

            &_red-color {
                background-color: #DC143C;
            }
        }

        &__title {
            display: block;
            font-size: 30px;
        }

        &__field {
            display: flex;
            flex-wrap: wrap;
            width: 640px;
            margin-left: auto;
            margin-right: auto;
            border: 2px solid black;
            align-items: flex-start;

        }

        &__cell {
            width: 80px;
            height: 80px;
            background-color: white;

            &_black-color {
                background-color: black;
            }
        }
    }
</style>