<script>
    export let elements;
    export let selected;
    export let refresh;

    const setDifficulty = (difficulty) => {
        if (difficulty === selected) return;
        
        selected = difficulty;
        refresh();
    };
</script>

<div>
    {#each elements as element}
        <button on:click={setDifficulty(element)} id={element} class:active={selected === element}>
            {element}
        </button>
    {/each}
</div>

<style>
    div {
        display: flex;
        flex-direction: row;
        padding: 2px;

        width: 343px;
        height: 32px;

        background: #F2F2F7;
        border-radius: 8.91px;
    }

    button {
        background: none;
        border: 1px solid transparent;
        border-radius: 6.93px;
        cursor: pointer;
        transition: box-shadow 0.2s ease-in;

        height: 100%;
        flex-grow: 1;

        font-family: 'Source Code Pro';
		font-style: normal;
		font-weight: 400;
		font-size: 16px;
		line-height: 20px;

		color: black;
        user-select: none;
        overflow: none;
    }

    button:hover {
        border: 0.5px solid #C7C7CC;
    }

    .active {
        background: #FFFFFF;
        box-shadow: 0px 3px 8px rgba(0, 0, 0, 0.12), 0px 3px 1px rgba(0, 0, 0, 0.04);
        border: 0.5px solid #C7C7CC;
    }

    @media only screen and (max-width: 400px) {
        div {
            width: 88vw;
        }
    }
</style>
