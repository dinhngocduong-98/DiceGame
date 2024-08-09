<template>
	 <div class="wrapper clearfix">

		<players
			v-bind:isWinner = "isWinner"
			v-bind:activePlayer = "activePlayer"
			v-bind:scoresPlayer = "scoresPlayer"
			v-bind:currentScore = "currentScore"
			
		/>
		
		<controls
			v-bind:isPlaying= "isPlaying"
			v-bind:finalScore = "finalScore"
			v-on:handleChangeFinalScore = "handleChangeFinalScore"
			v-on:handleNewGame = "handleNewGame"
			v-on:handleRollDice = "handleRollDice"
			v-on:handleHolpScore = "handleHolpScore"
		/>

		<dices 
			v-bind:dices = "dices"
		/>
		
		<popup-rule
			v-bind:titlePopup = "titlePopup"
			v-bind:contentPopup = "contentPopup"
			v-bind:isOpenPopup = "isOpenPopup"
			v-on:confirmRule = "confirmRule"
		/>

     </div>
</template>

<script>
import Players from './components/Players.vue'
import Controls from './components/Control.vue'
import Dices from './components/Dices.vue'
import PopupRule from './components/PopupRule.vue'

export default {
	name: 'app',
	data () {
		return {
			isPlaying: false,
			isOpenPopup: false,
			activePlayer: 1,
			scoresPlayer: [3, 50],
			dices: [4, 6],
			currentScore: 10,
			finalScore: 100,
			titlePopup: 'Luat choi',
			contentPopup: 'Lỗi bạn gặp phải liên quan đến cú pháp JavaScript trong Vue.js. Cụ thể, thông báo lỗi cho biết có sự không đúng kỳ vọng ở dòng 42 của tệp App.vue. Cụ thể, lỗi Unexpected token, expected ; cho thấy có vấn đề với cách bạn định nghĩa phương thức handleNewGame.'
		}
	},
	components: {
		Players,
		Controls,
		Dices,
		PopupRule
	},
	computed: {
		isWinner() {
			let { scoresPlayer, finalScore } = this;
			if (scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore) {
				// dung cuoc choi
				this.isPlaying = false;
				return true;
			}
			return false;
		}
	},
	methods: {
		handleNewGame() {
			// Hiển thị luật chơi --> show Popup
			this.isOpenPopup = true;
			this.contentPopup = "Lỗi bạn gặp phải liên quan đến cú pháp JavaScript trong Vue.js. Cụ thể, thông báo lỗi cho biết có sự không đúng kỳ vọng ở dòng 42 của tệp App.vue. Cụ thể, lỗi Unexpected token, expected ; cho thấy có vấn đề với cách bạn định nghĩa phương thức handleNewGame.";
			this.titlePopup = "Luat choi";
		},
		confirmRule() {
			if(this.titlePopup == "Luat choi") {
				this.isOpenPopup = false;
				this.isPlaying = true;
				this.activePlayer = Math.floor(Math.random() * 2);
				this.scoresPlayer = [0, 0];
				this.currentScore = 0;
				this.dices = [1, 1];
			} else {
				this.isOpenPopup = false;
			}
		},
		handleRollDice() {
			
			if(this.isPlaying) {
				// xoay xx
				var dice1 = Math.floor(Math.random() * 6) + 1;
				var dice2 = Math.floor(Math.random() * 6) + 1;
				this.dices = [dice1, dice2]
				// neu dice1 = 1 or dice2 = 1
				if(dice1 == 1 || dice2 == 1) {
					this.contentPopup = 'Nguoi choi Player'+ (this.activePlayer + 1) + ' da quay trung so 1. Rat tiec' ;
					this.titlePopup = "Canh bao";
					this.isOpenPopup = true;
					// doi nguoi choi
					this.activePlayer = this.activePlayer === 0 ? 1 : 0;
					// reset currentScore = 0
					this.currentScore = 0;
				} else {
					// cong don vao currentScore cho nguoi dang choi
					this.currentScore = this.currentScore + dice1 + dice2;
				}
			} else {
				this.contentPopup = "Vui long an nut NewGame";
				this.titlePopup = "Canh bao";
				this.isOpenPopup = true;
			}
		},
		handleHolpScore() {
			console.log('handleholeScore App.vue')
			if(this.isPlaying) {
				if(this.currentScore != 0) {
					// let cloneScorePlayer = [...this.scoresPlayer]
					// cloneScorePlayer[this.activePlayer] = cloneScorePlayer[this.activePlayer] + this.currentScore;
					// this.scoresPlayer = cloneScorePlayer;
					// co the dung cach khac de thay doi diem ma khong can tao mang moi.
					this.$set(this.scoresPlayer, this.activePlayer, this.scoresPlayer[this.activePlayer] + this.currentScore)
					
					if(!this.isWinner) {
						// doi nguoi choi
						this.activePlayer = this.activePlayer === 0 ? 1 : 0;
						// reset currentScore = 0
						this.currentScore = 0;
					}
	
				} else {
					this.contentPopup = "Nguoi choi chua co diem. Vui long Roll.";
					this.titlePopup = "Canh bao";
					this.isOpenPopup = true;
				}

			} else {
				this.contentPopup = "Vui long an nut NewGame";
				this.titlePopup = "Canh bao";
				this.isOpenPopup = true;
			}
		},
		handleChangeFinalScore(e) {
			var number = parseInt(e.target.value);
			if(isNaN(number)) {
				this.finalScore = '';
			} else {
				this.finalScore = number;
			}
		}
	}
}
</script>

<style>
	* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    
}

.clearfix::after {
    content: "";
    display: table;
    clear: both;
}

body {
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url('/public/assets/back.jpg');
    background-size: cover;
    background-position: center;
    font-family: Lato;
    font-weight: 300;
    position: relative;
    height: 100vh;
    color: #555;
}

.wrapper {
    width: 1000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
    overflow: hidden;
}
</style>
