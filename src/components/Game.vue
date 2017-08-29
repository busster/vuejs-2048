<template>
  <div class="game">
    <div v-for="row in stringifyBoard" class="row">
      <div v-for="item in row" class="item">{{ item }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'game',
  data () {
    return {
      board: [
        [{merged:false, value:0},{merged:false, value:0},{merged:false, value:0},{merged:false, value:0}],
        [{merged:false, value:0},{merged:false, value:0},{merged:false, value:0},{merged:false, value:0}],
        [{merged:false, value:0},{merged:false, value:0},{merged:false, value:0},{merged:false, value:0}],
        [{merged:false, value:0},{merged:false, value:0},{merged:false, value:0},{merged:false, value:0}],
      ],
      boardChanged: false
    }
  },

  mounted() {
    const self = this
    self.seedTwo()
    self.seedTwo()
    document.addEventListener("keydown", function(event) {
      // debugger
      if (event.which === 39) {
        // right
        self.moveRight()
      } else if (event.which === 37) {
        // left
        self.moveLeft()
      } else if (event.which === 38) {
        // up
        self.moveUp()
      } else if (event.which === 40) {
        // down
        self.moveDown()
      } else {
        return
      }
      // self.resetMerged()
      // debugger
      if (self.boardChanged) {
        self.seedTwo()
        self.boardChanged = false
      }
    })
  },

  computed: {
    stringifyBoard(row) {
      const self = this
      return self.board.map(row => {
        return row.map(item => {
          return item.value
        })
      })
    },
  },

  methods: {
    changeFirstRow() {
      const self = this
      if (self.testToggle) {
        self.board.splice(0, 1, [1,1,1,1])
      } else {
        self.board.splice(0, 1, [0,0,0,0])
      }
      self.testToggle = !self.testToggle
    },

    printRow(num) {
      const self = this
      self.board.forEach(row => {
        console.log(row[num].value)
      })
    },

    seedTwo() {
      const self = this

      let getRandomItem = function() {
        let row = self.board[Math.floor(Math.random()*self.board.length)]
        return row[Math.floor(Math.random()*row.length)]
      }

      let initialRandomItem = getRandomItem()

      while (initialRandomItem.value != 0) {
        initialRandomItem = getRandomItem()
      }

      initialRandomItem.value = 2

    },

    resetMerged() {
      const self = this
      self.board.forEach(row => {
        row.forEach(item => {
          item.merged = false
        })
      })
    },

    // slideRight() {
    //   const self = this
    //   let changed = []
    //   for (var i = 0; i <= self.board.length - 1; i++) {
    //     let row = self.board[i]
    //     for (var x = row.length - 1; x >= 0; x--) {
    //       let iterTimes = (row.length - 1) - x
    //       for (var z = 0; z <= iterTimes; z++) {
    //         let currentIndex = x + z
    //         if (currentIndex != (row.length - 1)) {
    //           if (row[currentIndex].value === 0 && row[currentIndex + 1].value === 0) {
    //             changed.push(false)
    //           } else if (row[currentIndex].value === row[currentIndex + 1].value) {
    //             if (row[currentIndex].merged != true && row[currentIndex + 1].merged != true) {
    //               row.splice(currentIndex + 1, 1, {merged: true, value: row[currentIndex].value * 2})
    //               row.splice(currentIndex, 1, {merged: false, value: 0})
    //               changed.push(true)
    //             }
    //           } else if (row[currentIndex + 1].value === 0) {
    //             row.splice(currentIndex + 1, 1, {merged: false, value: row[currentIndex].value})
    //             row.splice(currentIndex, 1, {merged: false, value: 0})
    //             changed.push(true)
    //           }
    //         }
    //       }
    //     }
    //   }
    //   return changed.includes(true)
    // },

    // slideLeft() {
    //   const self = this
    //   let changed = []
    //   for (var i = 0; i <= self.board.length - 1; i++) {
    //     let row = self.board[i]
    //     for (var x = 0; x <= row.length - 1; x++) {
    //       let iterTimes = x
    //       for (var z = 0; z <= iterTimes; z++) {
    //         let currentIndex = x - z
    //         if (currentIndex != 0) {
    //           if (row[currentIndex].value === 0 && row[currentIndex - 1].value === 0) {
    //             changed.push(false)
    //           } else if (row[currentIndex].value === row[currentIndex - 1].value) {
    //             if (row[currentIndex].merged != true && row[currentIndex - 1].merged != true) {
    //               row.splice(currentIndex - 1, 1, {merged: true, value: row[currentIndex].value * 2})
    //               row.splice(currentIndex, 1, {merged: false, value: 0})
    //               changed.push(true)
    //             }
    //           } else if (row[currentIndex - 1].value === 0) {
    //             row.splice(currentIndex - 1, 1, {merged: false, value: row[currentIndex].value})
    //             row.splice(currentIndex, 1, {merged: false, value: 0})
    //             changed.push(true)
    //           }
    //         }
    //       }
    //     }
    //   }
    //   return changed.includes(true)
    // },

    // slideUp() {
    //   const self = this
    //   let changed = []
    //   for (var i = 0; i <= self.board[0].length - 1; i++) {
    //     let column = i
    //     for (var x = 0; x <= self.board.length - 1; x++) {
    //       let row = self.board[x]
    //       let iterTimes = x
    //       for (var z = 0; z <= iterTimes; z++) {
    //         let currentRow = x - z
    //         if (currentRow != 0) {
    //           if (self.board[currentRow - 1][column].value === 0 && self.board[currentRow][column].value === 0) {
    //             changed.push(false)
    //           } else if (self.board[currentRow - 1][column].value === self.board[currentRow][column].value) {
    //             if (self.board[currentRow - 1][column].merged != true && self.board[currentRow][column].merged != true) {
    //               self.board[currentRow - 1].splice(column, 1, {merged: true, value: self.board[currentRow - 1][column].value * 2})
    //               self.board[currentRow].splice(column, 1, {merged: false, value: 0})
    //               changed.push(true)
    //             }
    //           } else if (self.board[currentRow - 1][column].value === 0) {
    //             self.board[currentRow - 1].splice(column, 1, {merged: false, value: self.board[currentRow ][column].value})
    //             self.board[currentRow].splice(column, 1, {merged: false, value: 0})
    //             changed.push(true)
    //           }
    //         }
    //       }
    //     }
    //   }
    //   return changed.includes(true)
    // },

    // slideDown() {
    //   const self = this
    //   let changed = []
    //   for (var i = 0; i <= self.board[0].length - 1; i++) {
    //     let column = i
    //     for (var x = self.board.length - 1; x >= 0; x--) {
    //       let row = self.board[x]
    //       let iterTimes = (self.board.length - 1) - x
    //       for (var z = 0; z <= iterTimes; z++) {
    //         let currentRow = x + z
    //         if (currentRow != self.board.length - 1) {
    //           if (self.board[currentRow + 1][column].value === 0 && self.board[currentRow][column].value === 0) {
    //             changed.push(false)
    //           } else if (self.board[currentRow + 1][column].value === self.board[currentRow][column].value) {
    //             if (self.board[currentRow + 1][column].merged != true && self.board[currentRow][column].merged != true) {
    //               self.board[currentRow + 1].splice(column, 1, {merged: true, value: self.board[currentRow + 1][column].value * 2})
    //               self.board[currentRow].splice(column, 1, {merged: false, value: 0})
    //               changed.push(true)
    //             }
    //           } else if (self.board[currentRow + 1][column].value === 0) {
    //             self.board[currentRow + 1].splice(column, 1, {merged: false, value: self.board[currentRow ][column].value})
    //             self.board[currentRow].splice(column, 1, {merged: false, value: 0})
    //             changed.push(true)
    //           }
    //         }
    //       }
    //     }
    //   }
    //   return changed.includes(true)
    // },




    moveRight() {
      const self = this
      let board = self.board
      for (var a = 0; a < board.length; a++) {
        let i = self.board.length - 2
        let j = self.board.length - 1

        while (i >= 0) {
          if (board[a][i].value === board[a][j].value) {
            board[a][j].value = board[a][i].value + board[a][j].value
            board[a][i].value = 0
            self.boardDidChange()
            j--
            i--
          } else if ((board[a][j].value === 0 && board[a][j].value === 0) || 
            (board[a][j].value === 0 || 
            (board[a][i].value != 0 && board[a][j].value != 0 && (i + 1 == j)))) {
            j--
            i--
          } else if (board[a][i].value != 0 && board[a][j].value != 0) {
            j--
          } else if (board[a][i].value === 0) {
            i--
          }
        }
        for (var x = 0; x < board.length; x++) {
          for (var y = board.length - 1; y > 0; y--) {
            if (board[a][y].value === 0) {
              let temp = board[a][y - 1].value
              board[a][y - 1].value = 0
              board[a][y].value = temp
              self.boardDidChange()
            }
          }
        }
      }
    },

    moveLeft() {
      const self = this
      let board = self.board
      for (var a = 0; a < board.length; a++) {
        let i = 1
        let j = 0

        while (i < 4) {
          if (board[a][i].value === board[a][j].value) {
            board[a][j].value = board[a][i].value + board[a][j].value
            board[a][i].value = 0
            self.boardDidChange()
            j++
            i++
          } else if ((board[a][j].value === 0 && board[a][j].value === 0) || 
            (board[a][j].value === 0 || 
            (board[a][i].value != 0 && board[a][j].value != 0 && (i - 1 == j)))) {
            j++
            i++
          } else if (board[a][i].value != 0 && board[a][j].value != 0) {
            j++
          } else if (board[a][i].value === 0) {
            i++
          }
        }
        for (var x = 0; x < board.length; x++) {
          for (var y = 0; y < board.length - 1; y++) {
            if (board[a][y].value === 0) {
              let temp = board[a][y + 1].value
              board[a][y + 1].value = 0
              board[a][y].value = temp
              self.boardDidChange()
            }
          }
        }
      }
    },

    moveDown() {
      const self = this
      let board = self.board
      for (var a = 0; a < board.length; a++) {
        let i = self.board.length - 2
        let j = self.board.length - 1

        while (i >= 0) {
          if (board[i][a].value === board[j][a].value) {
            board[j][a].value = board[i][a].value + board[j][a].value
            board[i][a].value = 0
            self.boardDidChange()
            j--
            i--
          } else if ((board[j][a].value === 0 && board[j][a].value === 0) || 
            (board[j][a].value === 0 || 
            (board[i][a].value != 0 && board[j][a].value != 0 && (i + 1 == j)))) {
            j--
            i--
          } else if (board[i][a].value != 0 && board[j][a].value != 0) {
            j--
          } else if (board[i][a].value === 0) {
            i--
          }
        }
        for (var x = 0; x < board.length; x++) {
          for (var y = board.length - 1; y > 0; y--) {
            if (board[y][a].value === 0) {
              let temp = board[y - 1][a].value
              board[y - 1][a].value = 0
              board[y][a].value = temp
              self.boardDidChange()
            }
          }
        }
      }
    },

    moveUp() {
      const self = this
      let board = self.board
      for (var a = 0; a < board.length; a++) {
        let i = 1
        let j = 0

        while (i < 4) {
          if (board[i][a].value === board[j][a].value) {
            board[j][a].value = board[i][a].value + board[j][a].value
            board[i][a].value = 0
            self.boardDidChange()
            j++
            i++
          } else if ((board[j][a].value === 0 && board[j][a].value === 0) || 
            (board[j][a].value === 0 || 
            (board[i][a].value != 0 && board[j][a].value != 0 && (i - 1 == j)))) {
            j++
            i++
          } else if (board[i][a].value != 0 && board[j][a].value != 0) {
            j++
          } else if (board[i][a].value === 0) {
            i++
          }
        }
        for (var x = 0; x < board.length; x++) {
          for (var y = 0; y < board.length - 1; y++) {
            if (board[y][a].value === 0) {
              let temp = board[y + 1][a].value
              board[y + 1][a].value = 0
              board[y][a].value = temp
              self.boardDidChange()
            }
          }
        }
      }
    },

    boardDidChange() {
      const self = this
      self.boardChanged = true
    }




  }


}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.game {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
}
.row {
  margin: 0 auto;
  max-width: 10em;
  display: flex;
  justify-content: space-around;
  font-size: 5em;
}
.item {
  width: 2em;
  height: 2em;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
