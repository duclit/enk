<script>
	import { evaluate } from 'mathjs';
	import Dialog from './Dialog.svelte';
	import Navbar from './Navbar.svelte';
	import Selector from './Selector.svelte';

	let answerShown = false;
	let loadPoints, updatePoints, resetStreak, updateStreak, answer;
	let difficulty = "moderate";
	let dialogShowing = false;
	
	const error = () => {
		document.body.classList.add('shake');
		document.getElementById('text').style.borderColor = 'red';
		setTimeout(() => {
			document.body.classList.remove('shake');
			document.getElementById('text').style.borderColor = 'transparent';
		}, 100);
	};

	const correct = () => {
		document.getElementById('text').style.borderColor = 'green';
		document.getElementById('text').style.background = 'rgba(52, 199, 89, 0.3)';
		setTimeout(() => {
			document.getElementById('text').style.borderColor = 'transparent';
			document.getElementById('text').style.background = '#F2F2F7';
		}, 200);
	}

	const reloadSeries = () => {
		let expression = randomExpression();
		
		let i1 = evaluate(expression, {n: 1});
		let i2 = evaluate(expression, {n: 2});
		let i3 = evaluate(expression, {n: 3});

		while ((i3 === i2 && i2 === i1) || i3 < 0 || i2 < 0 || i1 < 0 || i1 > 343 || i2 > 343 || i3 > 343 || i3 % 1 !== 0 || i2 % 1 !== 0 || i1 % 1 !== 0) {
			expression = randomExpression();
		
			i1 = evaluate(expression, {n: 1});
			i2 = evaluate(expression, {n: 2});
			i3 = evaluate(expression, {n: 3});
		}

		document.getElementById('series').innerHTML = `${i1}, ${i2}, ${i3}`;
		document.getElementById('text').value = "";
		document.getElementById('text').focus();

		answer = expression;
	};

	const handleKeypress = (event) => {
		if (event.key == "Enter") {
			const expression = document.getElementById('text').value;

			try {
				let i1 = evaluate(expression, {n: 1});
				let i2 = evaluate(expression, {n: 2});
				let i3 = evaluate(expression, {n: 3});

				let series = document.getElementById('series').innerHTML.split(', ');

				if (JSON.stringify([i1.toString(), i2.toString(), i3.toString()]) == JSON.stringify(series)) {
					if (!answerShown) {
						updatePoints(difficulty == 'hard' ? 30 : difficulty == 'easy' ? 10 : 20)
						updateStreak();
					}

					correct();
					reloadSeries();
				} else {
					error();
				};
			} catch {
				error();
			}

			answerShown = false;
		} 
	};

	const randomNumberUpto = (range, start=0) => Math.floor(Math.random() * range) || 1 + start;
	const randomChoiceFrom = (array) => array[Math.floor(Math.random() * array.length)];
	
	const randomExpression = () => {
		const generators = {
			easy: [
				`n ${randomChoiceFrom(['*', '/', '^', '+', '-'])} ${randomChoiceFrom([randomNumberUpto(6), randomNumberUpto(4) + 'n'])}`
			],
			moderate: [
				`n ${randomChoiceFrom(['*', '^', '+', '-'])} ${randomChoiceFrom([randomNumberUpto(6), randomNumberUpto(4) + 'n'])} ${randomChoiceFrom(['+', '-'])} 1`, 
				`n ${randomChoiceFrom(['*', '^'])} ${randomNumberUpto(4, 1) + 'n'} ${randomChoiceFrom(['+', '-'])} ${randomNumberUpto(4)}`,
				`${randomNumberUpto(10)} - ${randomChoiceFrom(['n', '2n'])}`,
				`${randomNumberUpto(10)} - ${randomChoiceFrom(['n', '2n'])} ${randomChoiceFrom(['*', '^', '+', '-'])} ${randomChoiceFrom([randomNumberUpto(6), randomNumberUpto(4) + 'n'])}`,
			],
			hard: [
				`n ${randomChoiceFrom(['+', '-', '*', '^', '/'])} ${randomNumberUpto(4, 1) + 'n'} ${randomChoiceFrom(['+', '-', '*', '^', '/'])} ${randomNumberUpto(6)}`,

			],
		}

		return randomChoiceFrom(generators[difficulty]);
	}

	setTimeout(reloadSeries, 10);
</script>

<Navbar 
	bind:loadPoints={loadPoints} 
	bind:updatePoints={updatePoints} 
	bind:showing={dialogShowing} 
	bind:updateStreak={updateStreak} 
	bind:resetStreak={resetStreak}
/>

<main>
	<h1 id="series"> </h1>
	<div>
		<button on:click={() => {reloadSeries(); resetStreak();}}>
			<svg width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg">
				<path d="M6.5 2.499L6.146 2.145L5.793 2.499L6.146 2.852L6.5 2.5V2.499ZM7.5 1.999H7V2.999H7.5V1.999ZM2 8.495V7.995H1V8.495H2ZM8.145 0.146004L6.146 2.146L6.854 2.852L8.853 0.854004L8.145 0.146004ZM6.146 2.852L8.146 4.851L8.853 4.144L6.853 2.145L6.146 2.852V2.852ZM7.5 3C10.537 3 13 5.461 13 8.496H14C13.9992 6.77271 13.314 5.12028 12.0951 3.9021C10.8761 2.68392 9.2233 1.99974 7.5 2V3ZM13 8.495C12.9992 9.95308 12.4194 11.3512 11.388 12.3818C10.3566 13.4124 8.95808 13.9913 7.5 13.991V14.991C11.089 14.991 14 12.082 14 8.495H13ZM7.5 13.99C6.04209 13.9903 4.64375 13.4116 3.61239 12.3812C2.58102 11.3507 2.00106 9.95291 2 8.495H1C1.0008 10.2183 1.686 11.8707 2.90493 13.0889C4.12386 14.3071 5.7767 14.9913 7.5 14.991V13.991V13.99Z" fill="black"/>
			</svg>
		</button>
		<button on:click={() => {document.getElementById('text').value = answer; answerShown = true; document.getElementById('text').focus()}}>
			<svg width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg">
				<path d="M3.68842 4.3375L3.0638 4.25L3.68717 4.3375H3.68842ZM3.67583 4.425L4.2992 4.5125L3.67583 4.425ZM12.3123 4.3375L11.6889 4.425L12.3123 4.3375ZM12.3249 4.425L12.9482 4.3375L12.3249 4.425ZM10.519 11.875V12.5C10.686 12.5 10.8461 12.4342 10.9642 12.3169C11.0823 12.1997 11.1486 12.0408 11.1486 11.875H10.519ZM5.4817 11.875H4.85204C4.85204 12.0408 4.91838 12.1997 5.03646 12.3169C5.15455 12.4342 5.3147 12.5 5.4817 12.5V11.875ZM4.5435 7.69125L5.0422 7.30875L4.5435 7.69125ZM11.4572 7.69125L10.9585 7.30875L11.4572 7.69125ZM6.11136 15H9.88933V13.75H6.11136V15ZM3.0638 4.25L3.05121 4.3375L4.29794 4.5125L4.31053 4.425L3.0638 4.25ZM8.00034 6.95288e-08C6.79988 -0.000199574 5.63966 0.429544 4.73279 1.21029C3.82592 1.99104 3.2333 3.07036 3.0638 4.25L4.31053 4.425C4.43755 3.54357 4.88065 2.7372 5.55846 2.15396C6.23627 1.57072 7.1033 1.24975 8.00034 1.25V6.95288e-08ZM12.9369 4.25C12.7674 3.07036 12.1748 1.99104 11.2679 1.21029C10.361 0.429544 9.2008 -0.000199574 8.00034 6.95288e-08V1.25C8.89731 1.25002 9.7642 1.57107 10.442 2.15427C11.1197 2.73746 11.5629 3.54368 11.6902 4.425L12.9369 4.25ZM12.9495 4.3375L12.9369 4.25L11.6902 4.425L11.7027 4.5125L12.9495 4.3375ZM11.9546 8.07375C12.748 7.05625 13.1485 5.73 12.9495 4.33625L11.7027 4.51375C11.7749 5.00529 11.7462 5.50622 11.6186 5.9865C11.4909 6.46678 11.2668 6.91652 10.9598 7.30875L11.9546 8.07375ZM9.88933 9.875V11.875H11.1486V9.87625H9.88933V9.875ZM10.519 11.25H5.4817V12.5H10.519V11.25ZM6.11136 11.875V9.87625H4.85204V11.875H6.11136ZM3.05121 4.3375C2.95548 4.99467 2.99424 5.66426 3.16518 6.30622C3.33612 6.94819 3.63572 7.54934 4.04607 8.07375L5.04094 7.30875C4.73388 6.91652 4.5098 6.46678 4.38212 5.9865C4.25445 5.50622 4.22581 5.00529 4.29794 4.51375L3.05121 4.33625V4.3375ZM6.11136 9.875C6.11136 8.84625 5.55978 7.97125 5.04094 7.3075L4.04607 8.0725C4.52839 8.69125 4.85204 9.2825 4.85204 9.875H6.11136ZM10.9598 7.3075C10.4397 7.9725 9.88933 8.84625 9.88933 9.875H11.1486C11.1486 9.2825 11.4723 8.69125 11.9546 8.0725L10.9598 7.3075V7.3075Z" fill="black"/>
			</svg>				
		</button>
	</div>
</main>

<input type="text" on:keypress={handleKeypress} id="text" autocomplete="false" spellcheck="false" placeholder="2n + 1">
<Selector elements={['easy', 'moderate', 'hard']} bind:selected={difficulty} refresh={reloadSeries}/>
<Dialog bind:showing={dialogShowing}/>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Source+Code+Pro:wght@400;700;900&display=swap');

	h1 {
		font-family: 'Source Code Pro';
		font-style: normal;
		font-weight: 900;
		font-size: 40px;
		line-height: 50px;

		color: #000000;
		user-select: none;

		transition: background 0.1s ease-in;
		background: transparent;
		padding: 4px;
		border-radius: 8px;
	}

	button {
		width: 40px;
		height: 40px;
		background: white;
		border-radius: 8px;
		outline: none;
		cursor: pointer;
		border: none;
		transition: opacity 0.2s ease-in;
		opacity: 60%;
	}

	button:hover {
		opacity: 100%;
	}

	main {
		height: fit-content;
		width: fit-content;

		display: flex;
		flex-direction: row;
		gap: 10px;
		align-items: center;
	}

	div {
		height: fit-content;
		width: fit-content;

		display: flex;
		flex-direction: row;
		align-items: center;
	}

	input[type="text"] {
		width: 359px;
		height: 52px;

		background: #F2F2F7;
		border: 1px solid #C7C7CC;
		border-radius: 8px;

		font-family: 'Source Code Pro';
		font-style: normal;
		font-weight: 400;
		font-size: 16px;
		line-height: 20px;

		color: #8E8E93;
		padding-left: 12px;
		outline: none;
		transition: border 0.2s ease-in, background 0.1s ease-in;
	}

	input[type="text"]:hover {
		border: 1px solid #8E8E93;
	}

	input[type="text"]:focus {
		border: 1px solid #8E8E93;
	}

	input[type="text"]::placeholder {
		color: #D1D1D6;
	}

	@media only screen and (max-width: 400px) {
        input[type="text"] {
            width: 92vw;
        }

		h1 {
			font-size: 1.8em;
		}
    }
</style>
