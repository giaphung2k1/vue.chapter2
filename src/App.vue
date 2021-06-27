<template>
	<div id="app">
		<div class="wrapper clearfix">
            <players 
			v-bind:isWinner='isWinner'
			v-bind:scorePlayer='scorePlayer'
			v-bind:activePlayer='activePlayer'
			v-bind:currentScore='currentScore'
			/>
            <controls
			v-bind:isPlaying='isPlaying'
			v-on:handleChangeFineScore='handleChangeFineScore'
			v-bind:finalScore='finalScore' 
			v-on:handleHoldScore='handleHoldScore'
			v-on:handleNewGame='handleNewGame'
			v-on:handleRollDice='handleRollDice'
			/>
            <dices 
			v-bind:dices='dices'
			/>
			<popup-rule 
			v-bind:isOpenPopup='isOpenPopup'
			v-on:handleClosePopup='handleClosePopup'
			
			/>
            
            
        </div>

	</div>
</template>

<script>
import PopupRule from './components/PopupRule';
import Dices from './components/Dices';
import Controls from './components/Controls';
import Players from './components/Players';
export default {
	name: 'app',
	data () {
		return {
			isOpenPopup:false,
			isPlaying:false,
			activePlayer:0, 
			scorePlayer:[0,0],
			currentScore:0,
			dices:[1,5],
			finalScore:100
		}
	},
	methods:{
		nextPlayer(){
			this.activePlayer = (this.activePlayer === 0)? 1 : 0;
			this.currentScore = 0;

		},
		handleNewGame(){
			// Hiển thị luật chơi
			this.isOpenPopup = true;
		},
		handleClosePopup(){
			this.activePlayer = 0;
			this.scorePlayer = [0,0];
			this.currentScore = 0;
			this.isPlaying = true;
			this.isOpenPopup = false;
			this.dices = [0,0];
		},
		handleRollDice(){
			if(this.isPlaying){
				var dice1 = Math.floor(Math.random() * 6) + 1;
				var dice2 = Math.floor(Math.random() * 6) + 1;
				this.dices = [dice1,dice2];
				if(dice1 === 1 || dice2 === 1){
					// Thông báo mất lượt chơi
					let activePlayer = this.activePlayer;
					setTimeout(function(){
						alert(`Người chơi Player ${activePlayer+1} đã quay trúng số 1. Rất tiếc`);
					},1000);
					// Đổi lượt chơi
					this.nextPlayer();

				}else{
					this.currentScore = this.currentScore + dice1 +dice2;
				}
			}else{
				alert('Vui lòng nhấn vào New Game');
			}
		},
		handleHoldScore(){
			if(this.isPlaying){
				// Thay đổi địa chỉ của scorePlayer

				// Cách 1:
				// let cloneScorePlayer = [...this.scorePlayer];
				// cloneScorePlayer[this.activePlayer] += this.currentScore;
				// this.scorePlayer = cloneScorePlayer;


				// Cách 2:
				let {scorePlayer,currentScore,activePlayer} = this;
				this.$set(scorePlayer,activePlayer,scorePlayer[activePlayer]+currentScore);
				if(!this.isWinner()){
					this.nextPlayer();
				}
				
			}
			else{
				alert('Vui lòng nhấn vào New Game');
			}
		},
		handleChangeFineScore(e){
			var number = parseInt(e.target.value);
			if(isNaN(number)){
				this.finalScore = '';
			}else{
				this.finalScore = number;
			}
			
		}
	},
	computed:{
		isWinner(){
			let {scorePlayer,finalScore} = this;
			if(scorePlayer[0] >= finalScore || scorePlayer[1] >= finalScore){
				// Dừng cuộc chơi
				this.isPlaying = false;
				return true;
			}
			return false;
		}
	},
	components: {
		Players,
		Controls,
		Dices,
		PopupRule
	}
}
</script>

<style>
	/**********************************************
*** GENERAL
**********************************************/

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
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url('/public/assets/images/back.jpg');
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
