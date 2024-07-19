<template>
  <div>
    <div class="main">
      <div
        id="board"
        class="board max-w-96 max-h-84 mt-14 border-4 border-black flex flex-wrap m-0"
      >
        <!-- <div class="tile"></div> -->
      </div>
      <div

        id="digits"
        class="digits max-w-96 max-h-12 mt-14  m-0-auto border-2 border-black flex flex-wrap board"
      ></div>
    </div>
    <div id="errors" class="errors">0</div>
  </div>
</template>





<script setup>
  import { onMounted } from 'vue';

let numSelected = null;
let errors = 0;

const baseNumber = 3;  // Should be 3 for a 9x9 board
const boardSize = Math.pow(baseNumber, 2);

const board = [
    "--74916-5",
    "2---6-3-9",
    "-----7-1-",
    "-586----4",
    "--3----9-",
    "--62--187",
    "9-4-7---2",
    "67-83----",
    "81--45---",


];

const solution = [
  "387491625",
    "241568379",
    "569327418",
    "758619234",
    "123784596",
    "496253187",
    "934176852",
    "675832941",
    "812945763"

   
];

onMounted(() => {
    setGame();
});

function setGame() {
    // Digits 1-9
    for (let i = 1; i <= boardSize; i++) {
        const number = document.createElement('div');
        number.id = i.toString();
        number.innerText = i.toString();
        number.addEventListener('click', selectNumber);
        number.classList.add('number');
        document.getElementById('digits').appendChild(number);
    }

    // Board 9x9
    for (let r = 0; r < boardSize; r++) {
        for (let c = 0; c < boardSize; c++) {
            const tile = document.createElement('div');
            tile.id = r.toString() + '-' + c.toString();
            if (board[r][c] !== '-') {
                tile.innerText = board[r][c];
                tile.classList.add('tile-start');
            }
            if (r % baseNumber === baseNumber - 1 && r !== boardSize - 1) {
                tile.classList.add('horizontal-line');
            }
            if (c % baseNumber === baseNumber - 1 && c !== boardSize - 1) {
                tile.classList.add('vertical-line');
            }
            tile.addEventListener('click', selectTile);
            tile.classList.add('tile');
            document.getElementById('board').append(tile);
        }
    }
}

function selectNumber(event) {
    if (numSelected !== null) {
        numSelected.classList.remove('number-selected');
    }
    numSelected = event.target;
    numSelected.classList.add('number-selected');
}

function selectTile(event) {
    if (numSelected) {
        if (event.target.innerText !== '') {
            return;
        }

        const coords = event.target.id.split('-');
        const r = parseInt(coords[0]);
        const c = parseInt(coords[1]);

        if (solution[r][c] === numSelected.id) {
            event.target.innerText = numSelected.id;
        } else {
            errors += 1;
            document.getElementById('errors').innerText = errors.toString();
        }
    }
}
</script>
