<script>
  import characters from './data/characters'
  import Picker from './Picker.svelte'
  import Advantage from './Advantage.svelte'

  export let currentStep
  export let goToNextStep
  export let updateSelectedCharacter
  export let updatePreviewCharacter
  export let p1Character
  export let p1Preview
  export let p2Character
  export let p2Preview
  export let matchupData
</script>

<h1 class={currentStep === 'player1Choses' ? 'p1' : 'p2'}>
  Player {#if currentStep === 'player1Choses'}1{:else}2{/if}: select your
  fighter
</h1>

<div class="preview">
  <div class="player-preview">
    {#if p1Character}
      <img
        src={p1Character.image}
        alt={p1Character.displayName || p1Character.name}
        class="preview-image"
      />
      <h2>{p1Character.displayName || p1Character.name}</h2>
    {:else if p1Preview}
      <img
        src={p1Preview.image}
        alt={p1Preview.displayName || p1Preview.name}
        class="preview-image"
      />
      <h2>{p1Preview.displayName || p1Preview.name}</h2>
    {:else}
      <div class="character-preview-placeholder">?</div>
    {/if}
  </div>
  <div class="middle-section">
    <img src="/img/vs.svg" alt="versus" class="versus" />
    <Advantage
      {matchupData}
      {p1Character}
      p2Character={p2Character || p2Preview}
    />
  </div>
  <div class="player-preview">
    {#if p2Character}
      <img
        src={p2Character.image}
        alt={p2Character.displayName || p2Character.name}
        class="preview-image"
      />
      <h2>{p2Character.displayName || p2Character.name}</h2>
    {:else if p2Preview}
      <img
        src={p2Preview.image}
        alt={p2Preview.displayName || p2Preview.name}
        class="preview-image"
      />
      <h2>{p2Preview.displayName || p2Preview.name}</h2>
    {:else}
      <div class="character-preview-placeholder">?</div>
    {/if}
  </div>
</div>

<div class="confirm-selection-wrapper">
  {#if (currentStep === 'player1Choses' && p1Character) || (currentStep === 'player2Choses' && p2Character)}
    <button class="button-primary" on:click={() => goToNextStep()}
      >Confirm selection</button
    >
  {/if}
</div>

<Picker
  items={characters}
  {currentStep}
  {goToNextStep}
  updateSelectedItem={updateSelectedCharacter}
  updatePreviewItem={updatePreviewCharacter}
  excludeFromRandomPool={p1Character}
/>

<style>
  h1.p1 {
    color: var(--c-red);
  }

  h1.p2 {
    color: var(--c-blue);
  }

  .preview {
    width: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    gap: 20px;
  }

  .preview-image {
    margin: 0 auto;
    max-width: 100%;
    max-height: 300px;
    border-radius: 4px;
  }

  h2 {
    margin: 0;
  }

  .character-preview-placeholder {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 100px;
  }

  .player-preview {
    width: calc(50% - 60px);
    text-align: center;
  }

  @media (min-width: 600px) {
    .player-preview {
      min-height: 390px;
    }
  }

  .middle-section {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  @media (min-width: 600px) {
    .middle-section {
      min-width: 250px;
    }
  }

  .versus {
    width: 60px;
  }

  @media (min-width: 600px) {
    .versus {
      width: 120px;
    }
  }

  .confirm-selection-wrapper {
    min-height: 60px;
    display: flex;
    align-items: start;
  }
</style>
