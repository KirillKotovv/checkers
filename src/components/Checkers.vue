<template>
  <div class="checkers">
    <div class="checkers__title">шашки</div>
    <div>
      <div v-for="(line, index) in board" :key="index">{{line}}</div>
    </div>
    <br>
    {{selectedCell}}
    <br>
    <div class="checkers__field" :key="boardKey">
      <div class="checkers__row" v-for="(line, lineIndex) in board" :key="lineIndex + Math.random()">
        <div class="checkers__cell"
             :class="{'checkers__cell_black-color': getCellColor(lineIndex, cellIndex) }"
             v-for="(cell, cellIndex) in line"
             :key="cellIndex + Math.random()"
             @click="handleClickCell(lineIndex, cellIndex, cell)">
          <div v-if="cell !== 0" class="checkers__checker" :class="{'checkers__checker_red-color': cell === 2}"></div>
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
      boardKey: 0,
      board: [
        [0, 1, 0, 1, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 1, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 2, 0, 0, 0, 0],
        [0, 0, 0, 0, 2, 0, 0, 0]
      ],
      selectedCell: null
    }
  },
  methods: {
    getCellColor: function (lineIndex, cellIndex) {
      return ((lineIndex + 1)%2 == 0 && (cellIndex+1) %2 !=0) || ((lineIndex + 1)%2 != 0 && (cellIndex+1)% 2 ==0)
    },
    handleClickCell: function (lineIndex, cellIndex, cell) {
      // console.log(lineIndex)
      // console.log(cellIndex)
      // console.log(cell)
      console.log(this.selectedCell)
      if (cell !== 0 && !this.selectedCell) {
        this.selectedCell = {
          lineIndex: lineIndex,
          cellIndex: cellIndex,
          cell: cell
        }
      } else if (this.selectedCell) {
        this.board[lineIndex][cellIndex] = this.selectedCell.cell
        this.board[this.selectedCell.lineIndex][this.selectedCell.cellIndex] = 0
        this.selectedCell = null
        ++this.boardKey
      }
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
