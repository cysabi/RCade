<!-- DEV ONLY: remove this file and its three lines in routes/+page.svelte. -->
<script lang="ts">
    export interface KnobFeel {
        mass: number;
        tension: number;
        friction: number;
    }

    let { feel = $bindable() }: { feel: KnobFeel } = $props();

    const NAMES = ["mass", "tension", "friction"] as const;

    function float(node: HTMLElement) {
        document.body.appendChild(node);
        return { destroy: () => node.remove() };
    }
</script>

<div class="tuning" use:float>
    {#each NAMES as name}
        <label>
            <span>{name}</span>
            <input
                type="range"
                min="0"
                max="1"
                step="0.01"
                value={feel[name]}
                oninput={(event) => (feel = { ...feel, [name]: Number(event.currentTarget.value) })}
            />
            <span class="value">{feel[name].toFixed(2)}</span>
        </label>
    {/each}
</div>

<style>
    .tuning {
        position: fixed;
        right: 4px;
        bottom: 4px;
        z-index: 2147483647;
        padding: 3px 5px;
        background: #000;
        border: 1px solid rgba(255, 255, 255, 0.3);
        font: 7px monospace;
        color: #fff;
        pointer-events: auto;
    }

    label {
        display: flex;
        align-items: center;
        gap: 4px;
    }

    span {
        width: 7ch;
    }

    .value {
        width: 4ch;
        text-align: right;
    }

    input {
        width: 80px;
        height: 8px;
        margin: 1px 0;
    }
</style>
