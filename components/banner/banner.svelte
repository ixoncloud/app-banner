<script lang="ts">
  import type { ComponentContext } from '@ixon-cdk/types';
  import tinycolor, { type MostReadableArgs } from 'tinycolor2';

  export let context: ComponentContext;

  let bgColor: string;
  let text: string;
  let textColor: string;
  let fontSize: string;

  $: bgColor = context?.inputs?.color || '#021752';
  $: text = context?.inputs?.text || '';
  $: fontSize = context?.inputs?.fontSize || 'auto';
  $: _setTextColor(bgColor);

  function _setTextColor(bgColor: string) {
    const color = tinycolor(bgColor);
    const wcag2: MostReadableArgs = { level: 'AA', size: 'small' };
    const readable = tinycolor.isReadable(color, 'black', wcag2);
    textColor = readable
      ? 'black'
      : tinycolor.mostReadable(color, ['white', '#f0f0f0', '#e0e0e0'], wcag2);
  }

  function getFontSizeStyle(fontSize: string): string {
    return fontSize === 'auto' ? '1.9em' : `${fontSize}px`;
  }
</script>

<main style={`background-color: ${bgColor};`}>
  <div class="wrapper">
    <div class="item">
      <p
        style={`color: ${textColor}; font-size: ${getFontSizeStyle(fontSize)};`}
      >
        {text}
      </p>
    </div>
  </div>
</main>

<style>
  main {
    box-sizing: border-box;
    height: 100%;
    padding: 0em;
  }
  p {
    margin: 0;
    font-family: 'Merienda', Helvetica, Arial;
    line-height: 1.5;
    text-align: center;
  }
  .item {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
  }
  .wrapper {
    display: flex;
    height: 100%;
    justify-content: center;
  }
</style>
