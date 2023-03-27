<script lang="ts">
  const numbers = ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0', '.'];
  const operations = ['/', 'X', '+', '-', '='];

  let selectedOperation = '';
  let display = '';
  let firstNumber = '';
  let secondNumber = '';
  let isDisplayResults = false;

  function handleClear() {
    selectedOperation = '';
    display = '';
    firstNumber = '';
    secondNumber = '';
    isDisplayResults = false;
  }

  function handleSelectedOperation(operation: string) {
    if (!firstNumber) return;
    if (operation === '=') {
      if (!secondNumber) return;
      let results = '';
      switch (selectedOperation) {
        case '/':
          results = (parseInt(firstNumber) / parseInt(secondNumber)).toFixed(2);
          break;
        case '+':
          results = (parseInt(firstNumber) + parseInt(secondNumber)).toFixed(2);
          break;
        case '-':
          results = (parseInt(firstNumber) - parseInt(secondNumber)).toFixed(2);
          break;
        case 'X':
          results = (parseInt(firstNumber) * parseInt(secondNumber)).toFixed(2);
          break;
      }
      display = results;
      isDisplayResults = true;
    }
    selectedOperation = operation;
  }

  function handleDisplayClick(num: string) {
    if (isDisplayResults) {
      return handleClear();
    }
    if (!display && num === '0') return;
    if (num === '.' && display.includes('.')) return;
    if (!selectedOperation) {
      if (display === '' && num === '.') {
        firstNumber = `${'0'}${num}`;
        return (display = firstNumber);
      }
      firstNumber = `${firstNumber}${num}`;
      return (display = firstNumber);
    } else {
      if (display === '' && num === '.') {
        secondNumber = `${'0'}${num}`;
        return (display = secondNumber);
      }
      secondNumber = `${secondNumber}${num}`;
      return (display = secondNumber);
    }
  }
</script>

<main>
  <div class="calculator">
    <div class="results">
      {display}
    </div>
    <div class="digits">
      <div class="numbers">
        <button class="btn btn-xlg" on:click={handleClear}>C</button>
        {#each numbers as number (number)}
          <button
            class={`btn ${number === '0' && 'btn-lg'}`}
            on:click={() => handleDisplayClick(number)}>{number}</button
          >
        {/each}
      </div>
      <div class="operations">
        {#each operations as operation (operation)}
          <button
            class={`btn btn-orange ${
              selectedOperation === operation && 'btn-silver'
            }`}
            on:click={() => handleSelectedOperation(operation)}
            >{operation}</button
          >
        {/each}
      </div>
    </div>
  </div>
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .calculator {
    background-color: rgb(28, 28, 28);
    width: 240px;
    padding: 15px;
    border-radius: 7px;
  }
  .digits {
    display: flex;
  }
  .operations {
    display: flex;
    flex-direction: column;
  }
  .numbers {
    display: flex;
    flex-wrap: wrap;
    width: 200px;
  }
  .btn {
    width: 50px;
    height: 50px;
    border-radius: 100px;
    background-color: rgb(114, 113, 113);
    font-size: 20px;
    font-weight: bold;
    color: white;
    margin: 5px;
    border: none;
  }
  .btn-lg {
    width: 110px;
  }
  .btn-orange {
    background-color: orange;
  }
  .btn-silver {
    background-color: silver;
  }
  .btn-xlg {
    width: 170px;
  }
  .results {
    height: 60px;
    color: white;
    font-size: 50px;
    display: flex;
    flex-direction: row-reverse;
    margin-right: 10px;
  }
</style>
