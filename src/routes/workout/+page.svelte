<script>
    const images = import.meta.glob("$lib/variations/*.webp", { eager: true });

    let currentCircuit = $state(0);
    let currentSet = $state(0);
    let selectedVariations = $state([
        "1a",
        "2a",
        "3a",
        "4a",
        "5a",
        "6a",
        "7a",
        "8a",
        "9a",
        "10a",
    ]);
    const setInfo = [
        {
            name: "HS Pushup Progressions (6-12 reps)",
            variations: ["1a", "1b", "1c", "1d"],
        },
        {
            name: "Pistol Squat Progressions (6-12 reps)",
            variations: ["2a", "2b", "2c", "2d"],
        },
        {
            name: "Pushup Progressions (6-12 reps/side)",
            variations: ["3a", "3b", "3c", "3d", "3e"],
        },
        {
            name: "Chin-up Progressions (6-12 reps/side)",
            variations: ["4a", "4b", "4c", "4d"],
        },
        {
            name: "Bodyweight Hip Thrust Progressions (6-12 reps/side)",
            variations: ["5a", "5b", "5c"],
        },
        {
            name: "Plank / Awkward Plank (30s or 15s/side)",
            variations: ["6a", "6b", "6c"],
        },
        {
            name: "Russian Twists (12 reps/side)",
            variations: ["7a"],
        },
        {
            name: "Bicycle Crunches (20 reps/side)",
            variations: ["8a"],
        },
        {
            name: "Reverse Plank (30s)",
            variations: ["9a"],
        },
        {
            name: "Rest, Bridge or Full Wheel (15-20s)",
            variations: ["10a", "10b", "10c"],
        },
    ];

    // prevent screen lock and load variation data from localStorage
    $effect(async () => {
        await navigator.wakeLock.request("screen");

        document.addEventListener("visibilitychange", async () => {
            if (document.visibilityState === "visible") {
                await navigator.wakeLock.request("screen");
            }
        });

        if (localStorage.getItem("selectedVariations")) {
            selectedVariations = JSON.parse(
                localStorage.getItem("selectedVariations"),
            );
        }
    });

    function nextCircuit() {
        currentCircuit++;
        currentSet = 0;
    }

    function nextSet() {
        currentSet++;
    }
</script>

{#if currentCircuit > 3}
    <header>You may now close this page. <a href="/">Return </a> soon.</header>
    <h1>Workout Complete!</h1>
{:else}
    <header>Circuit {currentCircuit + 1}/4 • Set {currentSet + 1}/10</header>

    <h1>{setInfo[currentSet].name}</h1>

    <img
        src={images[
            `/src/lib/variations/${selectedVariations[currentSet]}.webp`
        ]["default"]}
        alt="Exercise {selectedVariations[currentSet]}"
        fetchpriority="high"
        width="640"
        height="360"
    />

    <div class="controls">
        <select
            aria-label="variation"
            bind:value={selectedVariations[currentSet]}
            onchange={() => {
                localStorage.setItem(
                    "selectedVariations",
                    JSON.stringify(selectedVariations),
                );
            }}
        >
            {#each setInfo[currentSet].variations as variation}
                <option value={variation}> {variation}</option>
            {/each}
        </select>

        {#if currentSet === setInfo.length - 1}
            <button onclick={() => nextCircuit()}>Next</button>
        {:else}
            <button onclick={() => nextSet()}>Next</button>
        {/if}
    </div>
{/if}

<style>
    .controls {
        display: flex;
        justify-content: space-between;
    }
</style>
