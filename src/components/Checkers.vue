<template>
  <div class="checkers">
    <div class="checkers__title">шашки</div>
    <div>
      <h1 class="checkers__info">{{ gameInfo }}</h1>
      <h1>счет: C - {{ check[0] }} K - {{ check[1] }}</h1>
    </div>
    <div class="checkers__field" :key="boardKey">

      <div class="checkers__row" v-for="(line, lineIndex) in board" :key="lineIndex">
        <div class="checkers__cell"
             :class="[{'checkers__cell_black-color': getCellColor(lineIndex, cellIndex) },{'checkers__cell_border-color' : cell === 3}]"
             v-for="(cell, cellIndex) in line"
             :key="cellIndex"
             :id="getCellColor(lineIndex, cellIndex)"
             @click="handleClickCell(lineIndex, cellIndex, cell)">
          <draggable :id="`${lineIndex}${cellIndex}${cell}`" :disable="false" group="test" @add="add"
                     style="width: 80px;height: 80px">
            <div v-if="cell !== 0 && cell!==3" class="checkers__checker"
                 :class="{'checkers__checker_red-color': cell === 2}"></div>
          </draggable>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";

export default {
  name: "Checkers",
  components: {
    draggable
  },
  data: () => {
    return {
      check: [0, 0],
      boardKey: 0,
      board: [
        [0, 1, 0, 1, 0, 1, 0, 1],
        [1, 0, 1, 0, 1, 0, 1, 0],
        [0, 1, 0, 1, 0, 1, 0, 1],
        [0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0],
        [2, 0, 2, 0, 2, 0, 2, 0],
        [0, 2, 0, 2, 0, 2, 0, 2],
        [2, 0, 2, 0, 2, 0, 2, 0],
      ],
      selectedCell: null,
      gameInfo: 'Ход синего игрока',
      gameFlag: 1,
      selectedMoves: null
    }
  },
  methods: {
    add: function (e) {
      console.log('add')
      console.log(e)
      //куда
      console.log(e.to.id[0])
      console.log(e.to.id[2])
      //от куда
      console.log(e.from.id[0])
      console.log(e.from.id[2])
      // находиться ли в черном квадрате
      console.log(e.path[1].id)
      if (e.path[1].id) {
        this.board[e.from.id[0]][e.from.id[1]] = 0
        this.board[e.to.id[0]][e.to.id[1]] = e.to.id[2]
      }
      ++this.boardKey

    },
    getCellColor: function (lineIndex, cellIndex) {
      return ((lineIndex + 1) % 2 == 0 && (cellIndex + 1) % 2 != 0) || ((lineIndex + 1) % 2 != 0 && (cellIndex + 1) % 2 == 0)
    },
    deletePossibleMoves: function () {
      //убираем предыдущие варианты ходов
      for (let row = 0; row < this.board.length; row++) {
        for (let item = 0; item < this.board[row].length; item++) {
          if (this.board[row][item] === 3) {
            this.board[row][item] = 0
            ++this.boardKey
          }
        }
      }
    },
    getPossibleMoves: function (selectedCell) {
      //если была выбрана синяя шашка
      if (selectedCell.cell === 1) {
        //проверяем диагональ синей шашки с правой и левой стороны
        if (selectedCell.lineIndex != 7) {
          if (this.board[selectedCell.lineIndex + 1][selectedCell.cellIndex - 1] === 0) {
            this.board[selectedCell.lineIndex + 1][selectedCell.cellIndex - 1] = 3
          }
          if (this.board[selectedCell.lineIndex + 1][selectedCell.cellIndex + 1] === 0) {
            this.board[selectedCell.lineIndex + 1][selectedCell.cellIndex + 1] = 3
          }
        }
        if (selectedCell.lineIndex < 6) {
          //если справа или слева есть красная шашка за синей
          if (this.board[selectedCell.lineIndex + 1][selectedCell.cellIndex + 1] === 2) {
            if (this.board[selectedCell.lineIndex + 2][selectedCell.cellIndex + 2] === 0) {
              this.board[selectedCell.lineIndex + 2][selectedCell.cellIndex + 2] = 3
              this.selectedCellMoves = {
                lineIndex: selectedCell.lineIndex + 1,
                cellIndex: selectedCell.cellIndex + 1,
                cell: 2
              }
            }
          }
          if (this.board[selectedCell.lineIndex + 1][selectedCell.cellIndex - 1] === 2) {
            if (this.board[selectedCell.lineIndex + 2][selectedCell.cellIndex - 2] === 0) {
              this.board[selectedCell.lineIndex + 2][selectedCell.cellIndex - 2] = 3
              this.selectedCellMoves = {
                lineIndex: selectedCell.lineIndex + 1,
                cellIndex: selectedCell.cellIndex - 1,
                cell: 2
              }
            }
          }
        }

        //если справа или слева есть красная шашка перед синй
        if (selectedCell.lineIndex > 1) {
          if (this.board[selectedCell.lineIndex - 1][selectedCell.cellIndex - 1] === 2) {
            if (this.board[selectedCell.lineIndex - 2][selectedCell.cellIndex - 2] === 0) {
              this.board[selectedCell.lineIndex - 2][selectedCell.cellIndex - 2] = 3
              this.selectedCellMoves = {
                lineIndex: selectedCell.lineIndex - 1,
                cellIndex: selectedCell.cellIndex - 1,
                cell: 2
              }
            }
          }
          if (this.board[selectedCell.lineIndex - 1][selectedCell.cellIndex + 1] === 2) {
            if (this.board[selectedCell.lineIndex - 2][selectedCell.cellIndex + 2] === 0) {
              this.board[selectedCell.lineIndex - 2][selectedCell.cellIndex + 2] = 3
              this.selectedCellMoves = {
                lineIndex: selectedCell.lineIndex - 1,
                cellIndex: selectedCell.cellIndex + 1,
                cell: 2
              }
            }
          }
        }

      } else {
        //если за красной шашкой есть клетка - предлагаем
        if (selectedCell.lineIndex !== 0) {
          if (this.board[selectedCell.lineIndex - 1][selectedCell.cellIndex - 1] === 0) {
            this.board[selectedCell.lineIndex - 1][selectedCell.cellIndex - 1] = 3
          }
          if (this.board[selectedCell.lineIndex - 1][selectedCell.cellIndex + 1] === 0) {
            this.board[selectedCell.lineIndex - 1][selectedCell.cellIndex + 1] = 3
          }
        }
        //если перед красной  шашкой есть синяя шашка и возможно ее побить
        if (selectedCell.lineIndex < 6) {
          if (this.board[selectedCell.lineIndex + 1][selectedCell.cellIndex - 1] === 1) {
            if (this.board[selectedCell.lineIndex + 2][selectedCell.cellIndex - 2] === 0) {
              this.board[selectedCell.lineIndex + 2][selectedCell.cellIndex - 2] = 3
              this.selectedCellMoves = {
                lineIndex: selectedCell.lineIndex + 1,
                cellIndex: selectedCell.cellIndex - 1,
                cell: 1
              }
            }
          }
          if (this.board[selectedCell.lineIndex + 1][selectedCell.cellIndex + 1] === 1) {
            if (this.board[selectedCell.lineIndex + 2][selectedCell.cellIndex + 2] === 0) {
              this.board[selectedCell.lineIndex + 2][selectedCell.cellIndex + 2] = 3
              this.selectedCellMoves = {
                lineIndex: selectedCell.lineIndex + 1,
                cellIndex: selectedCell.cellIndex + 1,
                cell: 1
              }
            }
          }
        }
        //если за красной  шашкой есть синяя шашка и возможно ее побить
        if (selectedCell.lineIndex > 1) {
          if (this.board[selectedCell.lineIndex - 1][selectedCell.cellIndex - 1] === 1) {
            if (this.board[selectedCell.lineIndex - 2][selectedCell.cellIndex - 2] === 0) {
              this.board[selectedCell.lineIndex - 2][selectedCell.cellIndex - 2] = 3
              this.selectedCellMoves = {
                lineIndex: selectedCell.lineIndex - 1,
                cellIndex: selectedCell.cellIndex - 1,
                cell: 1
              }
            }
          }
          if (this.board[selectedCell.lineIndex - 1][selectedCell.cellIndex + 1] === 1) {
            if (this.board[selectedCell.lineIndex - 2][selectedCell.cellIndex + 2] === 0) {
              this.board[selectedCell.lineIndex - 2][selectedCell.cellIndex + 2] = 3
              this.selectedCellMoves = {
                lineIndex: selectedCell.lineIndex - 1,
                cellIndex: selectedCell.cellIndex + 1,
                cell: 1
              }
            }
          }
        }
      }
      ++this.boardKey
    },
    handleClickCell: function (lineIndex, cellIndex, cell) {
      if (cell !== 0 && !this.selectedCell && cell !== 3) {
        this.selectedCell = {
          lineIndex: lineIndex,
          cellIndex: cellIndex,
          cell: cell
        }
        //убираем предыдущие подсказки
        this.deletePossibleMoves()
        //если выбрана шашка, делаем подсветку возможных ходов
        this.getPossibleMoves(this.selectedCell)
        console.log(this.selectedCell.cell)
      }
      //если уже выбрана шашка и выбрали клетку, а не шашку
      else if (cell === 3) {
        console.log('else if (this.selectedCell && cell === 3)')
        //проверяем нужную шашку
        if (this.selectedCell.cell === this.gameFlag) {
          console.log('this.selectedCell.cell === this.gameFlag')
          //если ,была выбрана синяя шашка и переход в разрешенную клетку
          if ((this.board[this.selectedCell.lineIndex][this.selectedCell.cellIndex] === 1) && cell === 3) {
            this.board[lineIndex][cellIndex] = 1
            this.board[this.selectedCell.lineIndex][this.selectedCell.cellIndex] = 0

            if (this.selectedCellMoves) {
              if (this.selectedCellMoves.lineIndex !== lineIndex && this.selectedCellMoves.cellIndex !== cellIndex) {
                this.board[this.selectedCellMoves.lineIndex][this.selectedCellMoves.cellIndex] = 0
                this.selectedCellMoves = null
                ++this.check[0]
              }
              //если был упущен ход побить шашку, убираем ее
              else {
                this.board[lineIndex][cellIndex] = 0
              }
            }

            this.gameFlag = 2
            this.gameInfo = 'Ход красного игрока'

            this.selectedCell = null
            //убираем предыдущие подсказки
            this.deletePossibleMoves()
          }
          //если ,была выбрана красная шашка и переход в разрешенную клетку
          else if ((this.board[this.selectedCell.lineIndex][this.selectedCell.cellIndex] === 2) && cell === 3) {
            this.board[lineIndex][cellIndex] = 2
            this.board[this.selectedCell.lineIndex][this.selectedCell.cellIndex] = 0

            if (this.selectedCellMoves) {
              if (this.selectedCellMoves.lineIndex !== lineIndex && this.selectedCellMoves.cellIndex !== cellIndex) {
                this.board[this.selectedCellMoves.lineIndex][this.selectedCellMoves.cellIndex] = 0
                ++this.check[1]
                this.selectedCellMoves = null
              }
              //если был упущен ход побить шашку, убираем ее
              else {
                this.board[lineIndex][cellIndex] = 0
              }
            }

            this.gameFlag = 1
            this.gameInfo = 'Ход синего игрока'

            //"сбрасываем нажатую кнопку"
            this.selectedCell = null
            //убираем предыдущие подсказки
            this.deletePossibleMoves()
          }
        }
      } else {
        //если выбрали шашку
        this.selectedCell = null
        this.deletePossibleMoves()
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

  &__info {
    margin: 15px;
  }

  &__row {
    display: flex;
  }

  &__cell_border-color {
    outline: 3px dashed green; /* Пунктирная рамка */
    outline-offset: -3px; /* Выводим рамку внутри элемента */
    transition: 0.3s;
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
