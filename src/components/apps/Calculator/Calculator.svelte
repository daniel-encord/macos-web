<script lang="ts">
  import Plus from '~icons/ic/outline-plus';
  import PlusMinusVariant from '~icons/ic/outline-plus-minus';
  import Equal from '~icons/ic/round-equals';
  import Minus from '~icons/ic/round-minus';
  import Modulo from '~icons/icon-park-outline/percentage';
  import Division from '~icons/ph/divide-bold';
  import Multiply from '~icons/uil/multiply';

  type Operation = '+' | '-' | '*' | '/' | '%';

  let currentInput = '';
  let previousInput = '';
  let operation: Operation | null = null;

  const handleNumber = (number: `${number}` | '.'): void => {
    if (currentInput === '0' && number !== '.') {
      currentInput = number;
    } else if (currentInput.includes('.') && number === '.') {
      return;
    } else {
      currentInput += number;
    }
  };

  const handleOperation = (op: Operation): void => {
    if (currentInput === '') return;
    if (previousInput !== '') {
      calculate();
    }
    operation = op;
    previousInput = currentInput;
    currentInput = '';
  };

  const formatResult = (result: number): string => {
    return Number(result.toFixed(10)).toString();
  };

  const calculate = (): void => {
    let result: number;
    const prev = parseFloat(previousInput);
    const current = parseFloat(currentInput);
    if (Number.isNaN(prev) || Number.isNaN(current)) return;

    switch (operation) {
      case '+':
        result = prev + current;
        break;
      case '-':
        result = prev - current;
        break;
      case '*':
        result = prev * current;
        break;
      case '/':
        if (current === 0) return;
        result = prev / current;
        break;
      case '%':
        result = prev % current;
        break;
      default:
        throw new Error(`Invalid operation: ${operation}`);
    }
    currentInput = formatResult(result);
    operation = null;
    previousInput = '';
  };

  const clear = () => {
    currentInput = '';
    previousInput = '';
    operation = null;
  };

  const handlePlusMinus = () => {
    if (currentInput) {
      currentInput = currentInput.startsWith('-') ? currentInput.slice(1) : '-' + currentInput;
    }
  };
</script>

<section class="container">
  <header class="app-window-drag-handle" />

  <section class="show-area">{currentInput || previousInput || '0'}</section>

  <section class="buttons-container">
    <button class="top-row-button" on:click={clear}> AC </button>
    <button class="top-row-button" on:click={handlePlusMinus}>
      <PlusMinusVariant />
    </button>
    <button class="top-row-button" on:click={() => handleOperation('%')}>
      <Modulo />
    </button>
    <button class="operation-button" on:click={() => handleOperation('/')}>
      <Division />
    </button>
    <button class="number-button" on:click={() => handleNumber('7')}> 7 </button>
    <button class="number-button" on:click={() => handleNumber('8')}> 8 </button>
    <button class="number-button" on:click={() => handleNumber('9')}> 9 </button>
    <button class="operation-button" on:click={() => handleOperation('*')}>
      <Multiply />
    </button>
    <button class="number-button" on:click={() => handleNumber('4')}> 4 </button>
    <button class="number-button" on:click={() => handleNumber('5')}> 5 </button>
    <button class="number-button" on:click={() => handleNumber('6')}> 6 </button>
    <button class="operation-button" on:click={() => handleOperation('-')}>
      <Minus />
    </button>
    <button class="number-button" on:click={() => handleNumber('1')}> 1 </button>
    <button class="number-button" on:click={() => handleNumber('2')}> 2 </button>
    <button class="number-button" on:click={() => handleNumber('3')}> 3 </button>
    <button class="operation-button" on:click={() => handleOperation('+')}>
      <Plus />
    </button>
    <button class="number-button curved-bottom-left-button" style:grid-column="1 / span 2" on:click={() => handleNumber('0')}>
      0
    </button>
    <button class="number-button" on:click={() => handleNumber('.')}> . </button>
    <button class="operation-button curved-bottom-right-button" on:click={calculate}>
      <Equal />
    </button>
  </section>
</section>

<style lang="scss">
  header {
    padding: 1rem;
  }

  .container {
    height: 100%;
    width: 100%;

    background-color: hsla(0, 0%, 27%, 0.7);
    backdrop-filter: blur(40px);

    border-radius: inherit;

    display: grid;
    grid-template-rows: auto auto 1fr;

    font-family: var(--system-font-family) !important;
  }

  .buttons-container {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(5, 1fr);
    gap: 0.9px;

    margin: 1.5px;

    & > button {
      font-size: 1.618rem;
      font-weight: 300 !important;
      color: white;
      fill: white;

      &:active { 
        background-color: hsla(0, 0%, 80%, 0.1);
      }
    }

    :global(svg) {
      font-size: 1.2rem;
    }
  }

  .top-row-button {
    background-color: hsla(0, 0%, 35%, 0.6);
  }

  .number-button {
    background-color: hsla(0, 0%, 45%, 0.6);
  }

  .operation-button {
    background-color: hsla(33, 87%, 59%, 1);
  }

  .curved-bottom-left-button {
    border-radius: 0 0 0 0.7rem;
  }

  .curved-bottom-right-button {
    border-radius: 0 0 0.7rem 0;
  }

  .show-area {
    font-size: 3rem;
    color: white;
    text-align: end;
    font-weight: 200;

    overflow: auto;

    padding: 0.5rem 1rem;
  }

  :global(.tl-container.calculator) {
    top: 0.7rem;
    left: 0.7rem;
  }
</style>
