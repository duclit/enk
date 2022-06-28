<script>
	import { evaluate } from 'mathjs';
	import Selector from './Selector.svelte';

	let difficulty = "easy";
	
	const error = () => {
		document.body.classList.add('shake');
		document.getElementById('text').style.borderColor = 'red';
		setTimeout(() => {
			document.body.classList.remove('shake');
			document.getElementById('text').style.borderColor = 'transparent';
		}, 100);
	};

	const reloadSeries = () => {
		const expression = randomExpression();
		
		let i1 = evaluate(expression, {n: 1});
		let i2 = evaluate(expression, {n: 2});
		let i3 = evaluate(expression, {n: 3});

		document.getElementById('series').innerHTML = `${i1}, ${i2}, ${i3}`;
		document.getElementById('text').value = "";

		console.log(expression);
	};

	const handleKeypress = (event) => {
		if (event.key == "Enter") {
			const expression = document.getElementById('text').value;

			try {
				let i1 = evaluate(expression, {n: 1});
				let i2 = evaluate(expression, {n: 2});
				let i3 = evaluate(expression, {n: 3});

				let series = document.getElementById('series').innerHTML.split(', ');

				if (JSON.stringify([i1.toString(), i2.toString(), i3.toString()]) === JSON.stringify(series)) {
					reloadSeries();
				} else {
					error()
				};
			} catch {
				error()
			}
		} 
	};

	const randomNumberUpto = (range, start=0) => Math.floor(Math.random() * range) || 1 + start;
	const randomChoiceFrom = (array) => array[Math.floor(Math.random() * array.length)];
	const randomNumberFactorial = (range) => `${randomNumberUpto(range)}!`;
	
	const randomExpression = () => {
		const generators = {
			easy: [
				`n ${randomChoiceFrom(['*', '/', '^', '+', '-'])} ${randomChoiceFrom([randomNumberUpto(6), 'n', '2n'])}`
			],
			moderate: [
				`n ${randomChoiceFrom(['*', '^', '+', '-'])} ${randomChoiceFrom([randomNumberUpto(6), randomNumberUpto(4) + 'n'])} ${randomChoiceFrom(['+', '-'])} 1`, 
				`n ${randomChoiceFrom(['*', '^'])} ${randomNumberUpto(4, 1) + 'n'} ${randomChoiceFrom(['+', '-'])} ${randomNumberUpto(4)}`
			],
			hard: [
				`n ${randomChoiceFrom(['*', '^'])} ${randomNumberUpto(4, 1) + 'n'} ${randomChoiceFrom(['+', '-', '*'])} ${randomNumberUpto(14)}`, 
				`n ${randomChoiceFrom(['+', '-', '*'])} ${randomNumberUpto(4, 1) + 'n'} ${randomChoiceFrom(['+', '-', '*'])} ${randomNumberUpto(14)}`
			],
		}

		return randomChoiceFrom(
			generators[difficulty]

			// [`n ${randomChoiceFrom(['*', '/', '^'])} ${randomChoiceFrom([randomNumberUpto(5), 'n', '2n'])} ${randomChoiceFrom(['+', '-'])} 1`, 'moderate'],
			// [`n ${randomChoiceFrom(['*', '/', '^'])} ${randomChoiceFrom([randomNumberUpto(4), 'n', '2n'])}`, 'easy'],
			// [`n ${randomChoiceFrom(['*', '/', '^'])} ${randomChoiceFrom([randomNumberFactorial(6), 'n', '2n', 'n!', '2n!'])}`, 'hard'],
			// [`n ${randomChoiceFrom(['*', '/', '^'])} ${randomChoiceFrom([randomNumberUpto(6), 'n', '2n'])} ${randomChoiceFrom(['+', '-', '*'])} ${randomNumberUpto(4)}`, 'hard'],
		);
	}

	// const randomTerm = () => {
	// 		return randomChoiceFrom([
	// 			`${randomNumberUpto(3)}n`, randomNumberUpto(6),
	// 		])
	// }
	// 
	// const randomSeries = () => {
	// 		let operators = ['+', '-', '*', '^'];
	// 		let equation = `${randomNumberUpto(4)}n` + randomChoiceFrom(operators);
	// 		let strength = difficulty == 'hard' ? 4 : difficulty == 'easy' ? 2 : 3;
	// 	
	// 		for (let i = 0; i < strength - 2; i++) {
	// 			equation += randomTerm();
	// 			equation += randomChoiceFrom(operators);
	// 		}
	//
	// 		equation += randomTerm();
	// 		console.log(equation);
	// 		return equation;
	// };
</script>

<div>
	<h1 id="series"> </h1>
	<button on:click={reloadSeries}>
		<svg width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg">
			<path d="M6.5 2.499L6.146 2.145L5.793 2.499L6.146 2.852L6.5 2.5V2.499ZM7.5 1.999H7V2.999H7.5V1.999ZM2 8.495V7.995H1V8.495H2ZM8.145 0.146004L6.146 2.146L6.854 2.852L8.853 0.854004L8.145 0.146004ZM6.146 2.852L8.146 4.851L8.853 4.144L6.853 2.145L6.146 2.852V2.852ZM7.5 3C10.537 3 13 5.461 13 8.496H14C13.9992 6.77271 13.314 5.12028 12.0951 3.9021C10.8761 2.68392 9.2233 1.99974 7.5 2V3ZM13 8.495C12.9992 9.95308 12.4194 11.3512 11.388 12.3818C10.3566 13.4124 8.95808 13.9913 7.5 13.991V14.991C11.089 14.991 14 12.082 14 8.495H13ZM7.5 13.99C6.04209 13.9903 4.64375 13.4116 3.61239 12.3812C2.58102 11.3507 2.00106 9.95291 2 8.495H1C1.0008 10.2183 1.686 11.8707 2.90493 13.0889C4.12386 14.3071 5.7767 14.9913 7.5 14.991V13.991V13.99Z" fill="black"/>
		</svg>
	</button>
</div>

<input type="text" on:keypress={handleKeypress} id="text" autocomplete="false" spellcheck="false">
<Selector elements={['easy', 'moderate', 'hard']} bind:selected={difficulty} refresh={reloadSeries}/>

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

	div {
		height: fit-content;
		width: fit-content;

		display: flex;
		flex-direction: row;
		gap: 10px;
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
		transition: border 0.04s ease-in;
	}

	input[type="text"]:focus {
		border: 1px solid #8E8E93;
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
