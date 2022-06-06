<script lang="ts" context="module">
    import { API_AUTH, ROUTE_HOME } from '$lib/constants'

    function response(user) {
        if (user && !user.guest) {
            return {
                props: {
                    user
                }
            };
        } else {
            return {
                redirect:  ROUTE_HOME,
                status: 302
            }
        }
    }

    export async function load({ fetch, session }) {
        // Approach #1 - Call the session getter API
		const res = await fetch(API_AUTH);
		if (res.ok) {
            const { user } = await res.json();
            return response(user)
        } else {
            return {
                status: res.status,
                error: new Error(`Could not load ${API_AUTH}`)
            };
        }

        // Approach #2 - Use the session parameter (refer hooks/index.ts to see how it got populated)
        // const { user } = session
        // return response(user)
    }
</script>

<script>
	import { onMount } from 'svelte';
	import {  getCurrUserProfile, updCurrUserProfile} from '$lib/user'


 	
	export let user_score = 0;
	export let computer_score = 0;
	onMount(async ()=>{
		let  data = await getCurrUserProfile();
		user_score = data.data.user_score;
		computer_score = data.data.computer_score;
	});
	let played = false;
	let computerChoice;
	let playerChoice;
	let winner;
	const possibleChoices = ["rock", "paper", "scissors"];
	let play = (choice) => async () => {
		playerChoice = choice;
		computerChoice = possibleChoices[Math.floor(Math.random()*possibleChoices.length)];
		played = true;
		if(computerChoice === playerChoice){
			winner = "Nobody";
		} else {
			if(computerChoice === "rock") {
				if(playerChoice === "paper") {
					winner = "You";
					user_score++;
				} else {
					winner = "The computer";
					computer_score++;
				}
			} else if(computerChoice === "paper") {
				if(playerChoice === "scissors") {
					winner = "You";
					user_score++;
				} else {
					winner = "The computer";
					computer_score++;
				}
			} else {
				if(playerChoice === "rock") {
					winner = "You";
					user_score++;
				} else {
					winner = "The computer";
					computer_score++;
				}
			}
		}
		await updCurrUserProfile({computer_score:computer_score,
			user_score:user_score});
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
	<h5>{user_score} for you</h5>
	<h5>{computer_score} for the computer</h5>
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