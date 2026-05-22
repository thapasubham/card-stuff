<script lang="ts">
    import Card from "./lib/Card.svelte";
    import { onMount } from "svelte";
    import { ranks, suits, type CardType } from "./lib/types";
    import { dndzone } from "svelte-dnd-action";
    import { flip } from "svelte/animate";

    let cards: CardType[] = [];
    let hands: CardType[] = [];
    let decks: CardType = { faceUp: false, id: "10", rank: "2", suit: "Club" };

    const MAX_FACE_UP = 3;
    const flipDurationMs = 300;

    function deckHand() {
        for (let i = 0; i < MAX_FACE_UP; i++) {
            hands[i] = cards[i];
        }
        decks = cards[MAX_FACE_UP + 1];
        console.log(decks);
    }

    function shuffle(cards: CardType[]) {
        const arr = [...cards];

        for (let i = arr.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));

            [arr[i], arr[j]] = [arr[j], arr[i]];
        }

        return arr;
    }

    function faceUp(cards: CardType[]) {
        const arr = [...cards];

        for (let i = 0; i < MAX_FACE_UP; i++) {
            arr[i].faceUp = true;
        }

        return arr;
    }

    function newCard() {
        let newCards = generateCard();
        cards = shuffle(newCards);
        cards = faceUp(cards);
        deckHand();
    }
    function generateCard() {
        let count = 0;

        const newCards: CardType[] = suits.flatMap((suit) =>
            ranks.map((rank) => ({
                id: (count++).toString(),
                rank,
                suit,
                faceUp: false,
            })),
        );

        return newCards;
    }

    onMount(() => {
        newCard();
    });

    function handleDndConsider(e: CustomEvent) {
        hands = e.detail.items;
    }

    function handleDndFinalize(e: CustomEvent) {
        hands = e.detail.items;
    }
</script>

<div class=" min-h-screen p-4">
    <button class="game-btn" on:click={newCard}> Shuffle </button>
    <div
        class="mt-1 grid gap-4 scale-100! w-auto p-4 translate-y-1! rounded-2xl transition-none bg-linear-to-b from-[#afdd0d] to-[#afddaf] shadow-[0_2px_4px_rgba(15,23,42,0.12),0_18px_36px_rgba(15,23,42,0.28)]!"
    >
        <div
            class="grid grid-cols-13 gap-1"
            use:dndzone={{
                items: hands,
                flipDurationMs,
                morphDisabled: true,
            }}
            on:consider={handleDndConsider}
            on:finalize={handleDndFinalize}
        >
            {#each hands as card (card.id)}
                <div
                    animate:flip={{ duration: flipDurationMs }}
                    class="w-24 h-32 my-2 rounded-2xl border-none"
                >
                    <Card {card} />
                </div>
            {/each}
        </div>
        <Card card={decks} />
    </div>
</div>
