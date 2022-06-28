<script>
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
</script>

<nav>
    <svg width="28" height="28" viewBox="0 0 28 28" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M3.73334 10.2667H24.2667M3.73334 17.7333H24.2667M12.1333 2.79999L8.4 25.2M19.6 2.79999L15.8667 25.2" stroke="white" stroke-width="2"/>
    </svg>
    
    <div>
        <svg width="10" height="10" viewBox="0 0 10 10" fill="none" xmlns="http://www.w3.org/2000/svg">
            <circle cx="5" cy="5" r="5" fill="#34C759"/>
        </svg>
        <p>{points}</p>
    </div>
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
    }

    div {
        margin-left: auto;
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
    }
</style>
