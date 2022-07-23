<script>
    export const loadStreak = () => {
		if (!window.localStorage.getItem('streak')) {
			window.localStorage.setItem('streak', '0');
			return 0;
		} else {
			return parseInt(window.localStorage.getItem('streak'));
		}
	}

	export const updateStreak = () => {
		window.localStorage.setItem('streak', 
			`${parseInt(window.localStorage.getItem('streak')) + 1}`
		);
		streak = loadStreak();
	}

    export const resetStreak = () => {
        window.localStorage.setItem('streak', 0);
        streak = loadStreak();
    }

    export const loadPoints = () => {
		if (!window.localStorage.getItem('points')) {
			window.localStorage.setItem('points', '0');
			return 0;
		} else {
			return parseInt(window.localStorage.getItem('points'));
		}
	}

	export const updatePoints = (amount) => {
		window.localStorage.setItem('points', 
			`${parseInt(window.localStorage.getItem('points')) + amount}`
		);
		points = loadPoints();
	}

    let points = loadPoints();
    let streak = loadStreak();
    export let showing;
</script>

<nav>
    <svg width="28" height="28" viewBox="0 0 28 28" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M3.73334 10.2667H24.2667M3.73334 17.7333H24.2667M12.1333 2.79999L8.4 25.2M19.6 2.79999L15.8667 25.2" stroke="white" stroke-width="2"/>
    </svg>
    <button on:click="{() => {showing = true;}}">
        <svg viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg" width="15" height="15"><path d="M7 4.5V5h1v-.5H7zm1-.01v-.5H7v.5h1zM8 11V7H7v4h1zm0-6.5v-.01H7v.01h1zM6 8h1.5V7H6v1zm0 3h3v-1H6v1zM7.5 1A6.5 6.5 0 0114 7.5h1A7.5 7.5 0 007.5 0v1zM1 7.5A6.5 6.5 0 017.5 1V0A7.5 7.5 0 000 7.5h1zM7.5 14A6.5 6.5 0 011 7.5H0A7.5 7.5 0 007.5 15v-1zm0 1A7.5 7.5 0 0015 7.5h-1A6.5 6.5 0 017.5 14v1z" fill="white"></path></svg>
    </button>
    <section>
        <div class="streak" title="Your streak">
            <svg width="10" height="10" viewBox="0 0 10 10" fill="none" xmlns="http://www.w3.org/2000/svg">
                <circle cx="5" cy="5" r="5" fill="#FF9500"/>
            </svg>
            <p>{streak}</p>
        </div>
        <div title="Your points">
            <svg width="10" height="10" viewBox="0 0 10 10" fill="none" xmlns="http://www.w3.org/2000/svg">
                <circle cx="5" cy="5" r="5" fill="#34C759"/>
            </svg>
            <p>{points}</p>
        </div>
    </section>
</nav>

<style>
    nav {
        background: #1C1C1E;
        width: 100vw;
        height: 60px;

        display: flex;
        flex-direction: row;
        align-items: center;
        padding: 14px;

        position: absolute;
        top: 0px;
        left: 0px;

        gap: 6px;
    }

    .streak {
        background: rgba(255, 149, 0, 0.3);
    }

    section {
        width: fit-content;
        height: fit-content;

        display: flex;
        flex-direction: row;
        gap: 12px;
        margin-left: auto;
    }

    div {
        background: rgba(52, 199, 89, 0.3);
        border-radius: 100px;
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        padding: 10px;
        gap: 10px;
        height: 28px;
        width: fit-content;
    }

    p {
        font-family: 'Source Code Pro';
        font-style: normal;
        font-weight: 600;
        font-size: 13px;
        line-height: 16px;
        text-align: center;

        color: #FFFFFF;
        user-select: none;
    }

    button {
		width: 40px;
		height: 40px;
		background: transparent;
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
</style>
