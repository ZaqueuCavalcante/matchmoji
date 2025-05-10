<script lang="ts">
    import { emojis } from "./emojis"

    type State = 'idle' | 'playing' | 'paused' | 'won' | 'lost'

    let state: State = 'idle'
    let size: number = 20
    let grid = createGrid(size)
    var maxMatches = grid.length / 2

    let selectedCards: number[] = []
    let matches: string[] = []

    $: selectedCards.length === 2 && matchSelectedCards()
    $: matches.length === maxMatches && gameWon()



    function createGrid(size: number): string[] {
        let cards = new Set<string>()

        while (cards.size < size / 2) {
            const randomIndex = Math.floor(Math.random() * emojis.length)
            cards.add(emojis[randomIndex])
        }

        return shuffle([...cards, ...cards])
    }

    function shuffle<T>(array: T[]) {
        return array.sort(() => Math.random() - 0.5)
    }

    function selectCard(index: number) {
        selectedCards = selectedCards.concat(index)
    }

    function matchSelectedCards() {
        const [firstIndex, secondIndex] = selectedCards

        if (grid[firstIndex] === grid[secondIndex]) {
            matches = matches.concat(grid[firstIndex])
        }

        setTimeout(() => selectedCards = [], 300)
    }

    function gameWon() {
        state = 'won'
    }
</script>

{#if state === 'idle'}
    <h1>MatchMoji</h1>
    <button on:click={() => { state = 'playing' }}>Play</button>
{/if}

{#if state === 'playing'}
    <h2>MatchMoji</h2>

    <div class="matches">
        {#each matches as match}
            <div>{match}</div>
        {/each}
    </div>

    <div class="cards">
        {#each grid as card, index}
            {@const isSelected = selectedCards.includes(index)}
            {@const isMatched = matches.includes(card)}
            {@const isSelectedOrMatched = selectedCards.includes(index) || matches.includes(card)}

            <button
                class="card"
                class:selected={isSelected}
                disabled={isSelectedOrMatched}
                on:click={() => selectCard(index)}
            >
                <div class:isMatched>{card}</div>
            </button>
        {/each}
    </div>
{/if}

{#if state === 'lost'}
    <h2>MatchMoji</h2>
    <h1>You lost! 💩</h1>
    <button on:click={() => { state = 'playing' }}>Play again</button>
{/if}

{#if state === 'won'}
    <h2>MatchMoji</h2>
    <h1>You win! 🎉</h1>
    <button on:click={() => { state = 'playing' }}>Play again</button>
{/if}

<style>
    .cards {
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        gap: 0.4rem;
    }

    .matches {
        display: flex;
        gap: 0.5rem;
        margin-block: 2rem;
        font-size: 3rem;
    }

    .card {
        height: 140px;
        width: 140px;
        font-size: 4rem;
        background-color: var(--bg-2);

        &.selected {
            border: 4px solid var(--border);
        }

        & .isMatched {
            transition: opacity 0.3s ease-out;
            opacity: 0.4;
        }
    }
</style>
