<template>
	<div class="calc">
<!--		<p>{{inputBefore}} / {{input}}</p>-->
		<div class="input-form">
			<div class="text-after">= {{inputBefore}}</div>
			<input disabled type="text"
				   class="output-input"
				   v-model="input"
			/>
		</div>
		<div class="keys">
			<div class="row">
				<button @click="clearInput"
						class="key key-operator">
					C
				</button>
				<button @click="reverseNum"
						class="key key-operator">
					+/-
				</button>
				<button @click="percentNum"
						class="key key-operator">
					%
				</button>
				<button @click="getOperator('divide', $event)"
						class="key key-operator main-operator">&#247
				</button>
			</div>
			<div class="row">
				<button @click="num" class="key key-number">7</button>
				<button @click="num" class="key key-number">8</button>
				<button @click="num" class="key key-number">9</button>
				<button @click="getOperator('increase', $event)"
						class="key key-operator main-operator">
					&times
				</button>
			</div>
			<div class="row">
				<button @click="num" class="key key-number" value="4">4</button>
				<button @click="num" class="key key-number">5</button>
				<button @click="num" class="key key-number">6</button>
				<button @click="getOperator('subtraction', $event)"
						class="key key-operator main-operator">
					-
				</button>
			</div>
			<div class="row">
				<button @click="num" class="key key-number">1</button>
				<button @click="num" class="key key-number">2</button>
				<button @click="num" class="key key-number">3</button>
				<button @click="getOperator('addition', $event)"
						class="key key-operator main-operator">
					+
				</button>
			</div>
			<div class="row">
				<button @click="num" class="key key-number">0</button>
				<button @click="input = input + $event.target.innerText" class="key key-operator">.</button>
				<button @click="equalNum"
						class="key key-operator">
					=
				</button>
			</div>
		</div>
	</div>
</template>

<script>
    export default {
        data() {
            return {
                input: '',
                inputBefore: '',
                operator: '',
                clearAfter: false,
                firstOperator: true,
                afterOperator: '',

            }
        },
        methods: {
            getOperator(value, event) {
                if (this.firstOperator === true) { // при изменении оператора не изменяет модель inputBefore
                    this.inputBefore = +this.input
                }
                this.operator = value
                this.clearAfter = true
                this.afterOperator = value // используется для процентов
                console.log(this.afterOperator)
                this.firstOperator = false
                this.removeActiveClass()
                event.target.classList.add('main-operator_chose')
            },
            equalNum() {
                this.input = Number(this.input)
                if (this.operator === 'divide') {
                    this.inputBefore = this.inputBefore / this.input
                }
                if (this.operator === 'increase') {
                    this.inputBefore = this.inputBefore * this.input
                }
                if (this.operator === 'subtraction') {
                    this.inputBefore = this.inputBefore - this.input
                }
                if (this.operator === 'addition') {
                    this.inputBefore = this.inputBefore + this.input
                }
                if (this.operator === 'percent') {
                    if (this.afterOperator === 'increase') {
                        this.inputBefore = this.input * this.inputBefore
                    }
                    if (this.afterOperator === 'addition') {
                        this.inputBefore += this.input
                    }
                }

                console.log(this.input, this.operator, this.inputBefore)
                this.clearAfter = true
            },
            clearInput() {
                this.input = ''
                this.inputBefore = ''
                this.operator = ''
                this.clearAfter = false
                this.firstOperator = true
                this.removeActiveClass()
            },
            reverseNum() {
                this.input -= +this.input + +this.input
            },
            percentNum() {
                this.operator = 'percent'
                console.log(this.afterOperator)
                if (this.afterOperator === 'increase') {
                    this.input = (this.inputBefore * 100)
                }
                if (this.afterOperator === 'addition') {
                    console.log('qwe')
                    this.input = (this.inputBefore / 100) * this.input
                }
            },
            num(event) {
                if (this.clearAfter === true) {
                    this.input = ''
                    this.clearAfter = false
                    this.input += Number(event.target.innerText)
                } else {
                    this.input += Number(event.target.innerText)
                }
            },
            removeActiveClass() {
                for (const elem of document.querySelectorAll('.main-operator')) {
                    elem.classList.remove('main-operator_chose')
                }
            }
        },
        watch: {
            // 'operator': {
            //     handler: function (val, oldVal) {
            //         this.operator = val
            //         this.inputBefore = this.input
            //         this.input = ''
            //         console.log(this.operator, this.inputBefore, this.input)
            //     },
            //     deep: true
            // },
            // 'input': {
            //     handler: function (val, oldVal) {
            //         console.log(val,'watcher')
            //     },
            //     deep: true
            // },
        }
    }
</script>

<style lang="scss">

	$orange: #FA8E12;

	* {
		box-sizing: border-box;
	}

	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
	}

	.calc {
		margin: 50px auto 0;
		width: 640px;

		.input-form {
			position: relative;

			.text-after {
				position: absolute;
				top: 50%;
				left: 15px;
				color: rgba(255, 255, 255, 0.7);
				font-size: 32px;
				transform: translateY(-50%);
			}

			.output-input {
				background-color: #202020;
				width: 100%;
				height: 100px;
				padding: 0;
				border: 0;
				color: white;
				font-size: 32px;
				text-align: right;
				padding-right: 20px;
			}
		}

		.row {
			width: 100%;

			&:last-child {
				& > button {
					&:first-child {
						width: 50%;
					}

					&:last-child {
						color: white;
						background-color: $orange;
					}
				}
			}
		}

		.keys {
			display: flex;
			justify-content: flex-start;
			flex-wrap: wrap;

			.key:nth-child(-n+3) {
				color: #212225;
				background-color: #D2D3D7;
			}

			.key {
				color: #212225;
				border: 0;
				width: 25%;
				height: 100px;
				font-size: 38px;

				&:hover {
					cursor: pointer;
				}

				&:active {
					color: white;
					background-color: rgba(0, 0, 0, .8) !important;
				}
			}


			.key-number {
			}

			.key-operator {
				background-color: $orange;
				color: white;
			}

			.main-operator_chose {
				background-color: black;
			}
		}
	}
</style>
