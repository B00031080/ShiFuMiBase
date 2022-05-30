<script>
	let playerScore = 0;
	let computerScore = 0;
	let played = false;
	let computerChoice;
	let playerChoice;
	let winner;
	const possibleChoices = ["rock", "paper", "scissors"];
	let play = (choice) => () => {
		playerChoice = choice;
		computerChoice = possibleChoices[Math.floor(Math.random()*possibleChoices.length)];
		played = true;
		if(computerChoice === playerChoice){
			winner = "Nobody";
		} else {
			if(computerChoice === "rock") {
				if(playerChoice === "paper") {
					winner = "You";
					playerScore++;
				} else {
					winner = "The computer";
					computerScore++;
				}
			} else if(computerChoice === "paper") {
				if(playerChoice === "scissors") {
					winner = "You";
					playerScore++;
				} else {
					winner = "The computer";
					computerScore++;
				}
			} else {
				if(playerChoice === "rock") {
					winner = "You";
					playerScore++;
				} else {
					winner = "The computer";
					computerScore++;
				}
			}
		}
	}
	
	let playAgain = ()=>{
		played=false;
		playerChoice = null;
	}
	
</script>
<style>
	.choices,.scores{
		display:flex;
		
	}
	.disabled{
		pointer-events:none;
		opacity:0.8;
		background: #DFDFDF;
	}
	.choice{
		border:1px solid #DDDDDD;
		border-radius : 10px;
		padding:10px;
		margin:5px;
		cursor:pointer;
		background: white;
		transition: all 200ms;
	}	
	.choice:hover{
		background:#DDDDDD;
		border-color: #AAAAAA;
	}
	.active{
		color:red;
	}
	.scores>*{
		padding: 5px;
	}
</style>
<h1 class="uppercase text-gray-100 bg-gray-500">Let's play Rock paper Scissors</h1>
<div class="scores">
	<h4>Score</h4>
	<h5>{playerScore} for you</h5>
	<h5>{computerScore} for the computer</h5>
</div>

<div class="choices {played?"disabled":""}">
	<div class="choice {playerChoice==="rock"?"active":""}" on:click={play("rock")}>Rock</div>
	<div class="choice {playerChoice==="paper"?"active":""}" on:click={play("paper")}>Paper</div>
	<div class="choice {playerChoice==="scissors"?"active":""}"on:click={play("scissors")}>Scissors</div>
</div>
{#if played}
<div>
	<p>The computer played {computerChoice} </p>
	<p>{winner} won.</p>
	<button on:click={playAgain}>
		Play again
	</button>
</div>
{/if}