<template>
  <div>
    <div class="home">
      <p>ici le home</p>
    </div>
    <vue-p5
            @setup="setup"
            @draw="draw">
    </vue-p5>

    <v-btn :color="this.running ? 'green' : 'orange'" dark @click="pause">
      Pause
    </v-btn>
  </div>
</template>

<script>
    import VueP5 from 'vue-p5'

    export default {
        name: 'Home',
        components: {
            "vue-p5": VueP5
        },
        data: () => ({
            running: true,
            cellsize: 10,
            width: 800,
            height: 600,
            grid: null,
            singlestep: false,
        }),
        computed: {
            cols () {
                return this.width / this.cellsize
            },
            rows () {
                return this.height / this.cellsize
            }
        },
        methods: {
            setup (sketch) {
                  sketch.createCanvas(800, 600);

                    this.grid = this.createGrid(this.cols, this.rows);
                    for (let i = 0; i < this.cols; i++)
                        for (let j = 0; j < this.rows; j++)
                            this.grid[i][j] = sketch.floor(sketch.random(2));
            },
            draw (sketch) {
                sketch.background(0);

                for (let i = 0; i < this.cols; i++) {
                    for (let j = 0; j < this.rows; j++) {
                        let x = i * this.cellsize;
                        let y = j * this.cellsize;
                        if (this.grid[i][j] === 1) {
                            sketch.fill(255);
                            sketch.stroke(0);
                            sketch.rect(x, y, this.cellsize - 1, this.cellsize - 1);
                        }
                    }
                }
                let next = this.createGrid(this.cols, this.rows);

                for (let i = 0; i < this.cols; i++) {
                    for (let j = 0; j < this.rows; j++) {
                        let state = this.grid[i][j];
                        let neighbors = this.countNeighbors(this.grid, i, j);

                        if (state === 0 && neighbors === 3) {
                            next[i][j] = 1;
                        } else if (state === 1 && (neighbors < 2 || neighbors > 3)) {
                            next[i][j] = 0;
                        } else {
                            next[i][j] = state;
                        }
                    }
                }

                this.grid = next;
                this.singlestep = false
            },
            createGrid() {
                let arr = new Array(this.cols);
                for (let i = 0; i< arr.length; i++) {
                    arr[i] = new Array(this.rows);
                }
                return arr;
            },
            countNeighbors(grid, x, y) {
                let sum = 0;
                for (let i = -1; i < 2; i++) {
                    for (let j = -1; j < 2; j++) {
                        let col = (x + i + this.cols) % this.cols;
                        let row = (y + j + this.rows) % this.rows;
                        sum += grid[col][row];
                    }
                }
                sum -= grid[x][y];
                return sum;
            },
            pause () {
                this.running = !this.running;
            }
        }
    }
</script>
