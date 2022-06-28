<script>
    let showing = !localStorage.getItem('visited');

    const hide = () => {
        showing = false;
        localStorage.setItem('visited', 'yes')
    }
</script>

<main class:hidden={!showing} on:click|self={hide}>
    <div>
        <svg width="34" height="34" viewBox="0 0 34 34" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect width="34" height="34" rx="17" fill="#EFF1F6"/>
            <path d="M10.4 14.6H23.6M10.4 19.4H23.6M15.8 9.8L13.4 24.2M20.6 9.8L18.2 24.2" stroke="#2B67E9" stroke-width="2"/>
        </svg>
        <h6>How to play</h6>
        <p>You're given a series of numbers, and you have to come up with an equation that can predict the value of each number, where <span>n</span> is 1 for the first item, 2 for the second, and so on. For example, the equation for <span>1, 4, 9</span> would be <span>n^2</span></p>
        <button on:click={hide}>Continue</button>
    </div>
</main>

<style>
    .hidden {
        display: none;
    }

    main {
        width: 100vw;
        height: 100vh;
        background: rgba(0, 0, 0, 0.2);;
        position: absolute;
        z-index: 100;
        top: 0px;
        left: 0px;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    div {
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        padding: 16px;
        gap: 16px;

        background: #1C1C1E;
        border-radius: 24px;

        width: 328px;
        height: fit-content;
    }

    h6 {
        font-family: 'Source Code Pro';
        font-style: normal;
        font-weight: 700;
        font-size: 18px;
        line-height: 28px;

        color: #FFFFFF;
    }

    p {
        font-family: 'Source Code Pro';
        font-style: normal;
        font-weight: 400;
        font-size: 14px;
        line-height: 20px;

        color: #FFFFFF;

        width: 296px;
        word-wrap: break-word;
    }

    span {
        width: fit-content;
        height: fit-content;
        border-radius: 4px;
        padding: 2px;
        background-color: #48484A;
        color: white;
    }

    button {
        width: 296px;
        height: 40px;
        background: #2B67E9;
        border-radius: 8px;
        transition: background 0.2s ease-in;
        font-family: 'Source Code Pro';
        font-style: normal;
        font-weight: 500;
        font-size: 16px;
        line-height: 24px;

        text-align: center;
        color: #FFFFFF;
        border: none;
        cursor: pointer;
    }

    button:hover {
        background: #1d4db3;
    }
</style>
