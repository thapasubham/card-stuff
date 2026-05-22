<script lang="ts">
    import type { CardType } from "./types";
    import { IsRed, suitSymbol } from "./utils";

    export let card: CardType;

    $: isRed = IsRed(card);
</script>

{#if card.faceUp}
    <div
        class="w-24 h-36 rounded-xl bg-white border-2 border-black shadow-md
        flex flex-col justify-between p-2 select-none duration-200
        hover:-translate-y-2 hover:scale-105"
    >
        <div class="flex justify-between text-sm font-bold">
            <span class={isRed ? "text-red-500" : "text-black"}>
                {card.rank}
            </span>

            <span class={isRed ? "text-red-500" : "text-black"}>
                {suitSymbol[card.suit]}
            </span>
        </div>

        <div class="flex items-center justify-center text-3xl">
            <span class={isRed ? "text-red-500" : "text-black"}>
                {suitSymbol[card.suit]}
            </span>
        </div>

        <div class="flex justify-between text-sm font-bold rotate-180">
            <span class={isRed ? "text-red-500" : "text-black"}>
                {card.rank}
            </span>

            <span class={isRed ? "text-red-500" : "text-black"}>
                {suitSymbol[card.suit]}
            </span>
        </div>
    </div>
{:else}
    <div
        class="w-24 h-36 rounded-xl
    bg-linear-to-b from-rose-700 to-red-500
    border-2 border-black shadow-md
    flex items-center justify-center
    select-none duration-200
    hover:-translate-y-2 hover:scale-105"
    >
        <div
            class="w-16 h-24 rounded-lg border-2 border-red-950
        flex items-center justify-center"
        >
            <div class="grid grid-cols-2 gap-2 text-red-300">
                <span class="text-2xl [-webkit-text-stroke:1px_black] font-bold"
                    >♥</span
                >
                <span class="text-2xl [-webkit-text-stroke:1px_black] font-bold"
                    >♦</span
                >
                <span class="text-2xl [-webkit-text-stroke:1px_black] font-bold"
                    >♠</span
                >
                <span
                    class="text-2xl [-webkit-text-stroke:1px_black] *:font-bold"
                    >♣</span
                >
            </div>
        </div>
    </div>
{/if}
