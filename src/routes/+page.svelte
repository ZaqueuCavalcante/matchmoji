<script lang="ts">
    import { emojis } from "./emojis"

    type State = 'idle' | 'playing' | 'paused' | 'won' | 'lost'

    let state: State = 'idle'
    let size: number = 20
    let grid: number[][] = []

    function createGrid(size: number) {
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
</script>
