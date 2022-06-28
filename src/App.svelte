<script>
	import { evaluate } from 'mathjs';
	import Dialog from './Dialog.svelte';
	import Navbar from './Navbar.svelte';
	import Selector from './Selector.svelte';

	let answerShown = false;
	let loadPoints, updatePoints, answer;
	let difficulty = "moderate";
	
	const error = () => {
		document.body.classList.add('shake');
		document.getElementById('text').style.borderColor = 'red';
		setTimeout(() => {
			document.body.classList.remove('shake');
			document.getElementById('text').style.borderColor = 'transparent';
		}, 100);
	};

	const reloadSeries = () => {
		let expression = randomExpression();
		
		let i1 = evaluate(expression, {n: 1});
		let i2 = evaluate(expression, {n: 2});
		let i3 = evaluate(expression, {n: 3});

		while ((i3 === i2 && i2 === i1) || i3 < 0 || i2 < 0 || i1 < 0 ) {
			expression = randomExpression();
		
			i1 = evaluate(expression, {n: 1});
			i2 = evaluate(expression, {n: 2});
			i3 = evaluate(expression, {n: 3});
		}

		document.getElementById('series').innerHTML = `${i1}, ${i2}, ${i3}`;
		document.getElementById('text').value = "";

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
					if (!answerShown) updatePoints(difficulty == 'hard' ? 30 : difficulty == 'easy' ? 10 : 20);
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
	const randomNumberFactorial = (range) => `${randomNumberUpto(range)}!`;
	
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
				`n ${randomChoiceFrom(['+', '-', '*'])} ${randomNumberUpto(4, 1) + 'n'} ${randomChoiceFrom(['+', '-', '*'])} ${randomNumberUpto(14)}`
			],
		}

		return randomChoiceFrom(generators[difficulty]);
	}

	setTimeout(reloadSeries, 10);
</script>

<Navbar bind:loadPoints={loadPoints} bind:updatePoints={updatePoints}/>

<main>
	<h1 id="series"> </h1>
	<div>
		<button on:click={reloadSeries}>
			<svg width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg">
				<path d="M6.5 2.499L6.146 2.145L5.793 2.499L6.146 2.852L6.5 2.5V2.499ZM7.5 1.999H7V2.999H7.5V1.999ZM2 8.495V7.995H1V8.495H2ZM8.145 0.146004L6.146 2.146L6.854 2.852L8.853 0.854004L8.145 0.146004ZM6.146 2.852L8.146 4.851L8.853 4.144L6.853 2.145L6.146 2.852V2.852ZM7.5 3C10.537 3 13 5.461 13 8.496H14C13.9992 6.77271 13.314 5.12028 12.0951 3.9021C10.8761 2.68392 9.2233 1.99974 7.5 2V3ZM13 8.495C12.9992 9.95308 12.4194 11.3512 11.388 12.3818C10.3566 13.4124 8.95808 13.9913 7.5 13.991V14.991C11.089 14.991 14 12.082 14 8.495H13ZM7.5 13.99C6.04209 13.9903 4.64375 13.4116 3.61239 12.3812C2.58102 11.3507 2.00106 9.95291 2 8.495H1C1.0008 10.2183 1.686 11.8707 2.90493 13.0889C4.12386 14.3071 5.7767 14.9913 7.5 14.991V13.991V13.99Z" fill="black"/>
			</svg>
		</button>
		<button on:click={() => {document.getElementById('text').value = answer; answerShown = true;}}>
			<svg viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg" width="15" height="15"><path d="M4.076 6.47L3.58 6.4l.495.07zm-.01.07l.495.07-.495-.07zm6.858-.07l-.495.07.495-.07zm.01.07l.495-.07-.495.07zM9.5 12.5v.5a.5.5 0 00.5-.5h-.5zm-4 0H5a.5.5 0 00.5.5v-.5zm-.745-3.347l.396-.306-.396.306zm5.49 0l-.396-.306.396.306zM6 15h3v-1H6v1zM3.58 6.4l-.01.07.99.14.01-.07-.99-.14zM7.5 3a3.959 3.959 0 00-3.92 3.4l.99.14A2.959 2.959 0 017.5 4V3zm3.92 3.4A3.959 3.959 0 007.5 3v1a2.96 2.96 0 012.93 2.54l.99-.14zm.01.07l-.01-.07-.99.14.01.07.99-.14zm-.79 2.989c.63-.814.948-1.875.79-2.99l-.99.142a2.951 2.951 0 01-.59 2.236l.79.612zM9 10.9v1.6h1v-1.599H9zm.5 1.1h-4v1h4v-1zm-3.5.5v-1.599H5V12.5h1zM3.57 6.47a3.951 3.951 0 00.79 2.989l.79-.612a2.951 2.951 0 01-.59-2.236l-.99-.142zM6 10.9c0-.823-.438-1.523-.85-2.054l-.79.612c.383.495.64.968.64 1.442h1zm3.85-2.054C9.437 9.378 9 10.077 9 10.9h1c0-.474.257-.947.64-1.442l-.79-.612z" fill="currentColor"></path></svg>
		</button>
	</div>
</main>

<input type="text" on:keypress={handleKeypress} id="text" autocomplete="false" spellcheck="false" placeholder="2n + 1">
<Selector elements={['easy', 'moderate', 'hard']} bind:selected={difficulty} refresh={reloadSeries}/>
<Dialog/>

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
		transition: border 0.2s ease-in;
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
