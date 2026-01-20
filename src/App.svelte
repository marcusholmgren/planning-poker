<script>
  import { crossfade, scale, fade } from 'svelte/transition';
  import { quintOut } from 'svelte/easing';

  let selectedCard = null;
  let showCard = false;
  
  const cards = [
    '0', '0.5', '1', '2', '3', '5', '8', '13', '20', '40', '100', '?', '☕️'
  ];

  const [send, receive] = crossfade({
    duration: 500,
    easing: quintOut,
    fallback: scale
  });
  
  function selectCard(card) {
    selectedCard = card;
    showCard = true;
  }
  
  function goBack() {
    showCard = false;
  }
</script>

<div class="min-h-screen bg-gradient-to-br from-blue-500 to-purple-600 flex items-center justify-center p-4">
  {#if !showCard}
    <!-- Card Selection View -->
    <div class="w-full max-w-4xl">
      <h1 class="text-4xl md:text-5xl font-bold text-white text-center mb-8 md:mb-12" in:fade out:fade>
        Planning Poker
      </h1>
      
      <div class="grid grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-3 md:gap-4">
        {#each cards as card}
          <button
            onclick={() => selectCard(card)}
            in:receive={{key: card}}
            out:send={{key: card}}
            class="aspect-[2/3] bg-white rounded-xl shadow-lg hover:shadow-2xl transform hover:scale-105 transition-all duration-200 flex items-center justify-center text-4xl md:text-5xl font-bold text-gray-800 active:scale-95"
          >
            {card}
          </button>
        {/each}
      </div>
      
      <p class="text-white text-center mt-8 text-sm md:text-base opacity-90" in:fade out:fade>
        Select a card to display your estimation
      </p>
    </div>
  {:else}
    <!-- Card Display View -->
    <div class="w-full h-screen flex flex-col items-center justify-center p-4">
      <div
        in:receive={{key: selectedCard}}
        out:send={{key: selectedCard}}
        class="bg-white rounded-3xl shadow-2xl p-8 md:p-12 aspect-[2/3] flex items-center justify-center max-w-md w-full"
      >
        <div class="text-[120px] md:text-[180px] font-bold text-gray-800">
          {selectedCard}
        </div>
      </div>
      
      <button
        onclick={goBack}
        in:fade
        out:fade
        class="mt-8 bg-white text-purple-600 px-8 py-3 rounded-full font-semibold shadow-lg hover:shadow-xl transform hover:scale-105 transition-all duration-200 active:scale-95"
      >
        Choose Another Card
      </button>
    </div>
  {/if}
</div>
