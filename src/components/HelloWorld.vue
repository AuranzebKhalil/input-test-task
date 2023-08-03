<template>
  <div id="app">
    <div class="input-container">
      <div class="chip-tags">
        <span
          v-for="(chip, index) in chips"
          :key="index"
          :class="{ 'chip-tag': true, 'valid-chip': isValidChip(chip), arithmetic: isArithmeticOperator(chip), 'typed-chip': !isArithmeticOperator(chip) }"
        >
          {{ chip }}
          <button v-if="!isArithmeticOperator(chip) && isValidChip(chip)" @click="deleteChip(index)">[X]</button>
        </span>
      </div>

      <div class="auto">
        <input
          ref="inputFieldRef"
          v-model="inputValue"
          @input="handleInput"
          @keydown.enter.prevent="handleEnterKey"
          @keydown.backspace="handleBackspace"
          @keydown.up="navigateAutocomplete('up')"
          @keydown.down="navigateAutocomplete('down')"
          @click="showAutocomplete = true"
          class="input-text"
    
        />
        <div
          ref="autocompleteContainerRef"
          class="autocomplete"
          :class="{ show: showAutocomplete && filteredSentences.length > 0 }"
          :style="{ top: inputFieldTop, left: inputFieldLeft }"
        >
          <ul>
            <li
              v-for="(sentence, index) in filteredSentences"
              :key="index"
              :class="{ active: index === selectedAutocompleteIndex }"
              @click="selectSentence(sentence)"
            >
              {{ sentence }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, watch, computed } from 'vue';

const data = ['payment Processing fees', 'Payroll Bonus OOGS', 'Payroll Bonus G&A', 'Payroll Bonus R&D', 'Payroll Bonus S&M', 'Payroll COGS','Payroll G&A', 'Payroll R&D' ,'Payroll S&M', 'Payroll Tax' ];
const inputValue = ref('');
const filteredSentences = ref([]);
const chips = reactive([]);
const selectedAutocompleteIndex = ref(-1);
const inputFieldRef = ref(null);
const autocompleteContainerRef = ref(null);
const showAutocomplete = ref(false);
const inputFieldTop = ref(0);
const inputFieldLeft = ref(0);

const handleInput = () => {
  selectedAutocompleteIndex.value = -1;
  const input = inputValue.value.trim().toLowerCase();

  const operator = findArithmeticOperator(input);
  if (operator) {
  chips.push(operator);
    inputValue.value = input.replace(operator, '').trim();
  } else {
    filteredSentences.value = input
      ? data.filter(sentence => sentence.toLowerCase().includes(input))
      : [];
    showAutocomplete.value = input.length > 0 && filteredSentences.value.length > 0;
  }
};

const selectSentence = (sentence) => {
  const inputSentence = inputValue.value.trim();
  const operator = findArithmeticOperator(inputSentence);

  if (operator) {
    chips.push(operator);
  }

  if (!containsForbiddenCharacters(sentence) && sentence !== operator) {
    chips.push(sentence);
  }

  inputValue.value = '';
  showAutocomplete.value = false;
};

const deleteChip = (index) => {
  chips.splice(index, 1);
};

const handleEnterKey = (event) => {
  if (selectedAutocompleteIndex.value !== -1) {
    const selectedSentence = filteredSentences.value[selectedAutocompleteIndex.value];
    selectSentence(selectedSentence);
    event.preventDefault();
  } else {
    const inputSentence = inputValue.value.trim();
    if (inputSentence) {
      const operator = findArithmeticOperator(inputSentence);
      if (operator) {
        chips.push(operator);
      }

      if (!containsForbiddenCharacters(inputSentence) && inputSentence !== operator) {
        chips.push(inputSentence);
      }

      inputValue.value = '';
      showAutocomplete.value = false;
    }
  }
};

const handleBackspace = () => {
  const inputField = inputFieldRef.value;
  const cursorPosition = inputField.selectionStart;

  if (cursorPosition === 0 && chips.length > 0) {
    // If the cursor is at the beginning and there are chips, delete the last chip
    deleteChip(chips.length - 1);
    return;
  }

  // Check if the cursor is just before an arithmetic operator (+, -, *, or /)
  if (isArithmeticOperator(inputValue.value[cursorPosition - 1])) {
    // Remove the arithmetic operator by updating the inputValue
    inputValue.value = inputValue.value.slice(0, cursorPosition - 1) + inputValue.value.slice(cursorPosition);
  }
};

const navigateAutocomplete = (direction) => {
  if (direction === 'up') {
    selectedAutocompleteIndex.value = Math.max(selectedAutocompleteIndex.value - 1, -1);
  } else if (direction === 'down') {
    selectedAutocompleteIndex.value = Math.min(selectedAutocompleteIndex.value + 1, filteredSentences.value.length - 1);
  }
};

const containsForbiddenCharacters = (input) => {
  const forbiddenCharacters = ['+', '=' , '-', '*', '/','.',',','?',';',':','>','<','{}','{','}','"','()','(',')','*','&','^','%','$','#','@','!'];
  return forbiddenCharacters.includes(input);
};

const isArithmeticOperator = (input) => {
  const arithmeticOperators = ['+','=' , '-', '*', '/','.',',','?',';',':','>','<','{}','{','}','"','()','(',')','*','&','^','%','$','#','@','!'];
  return arithmeticOperators.includes(input);
};

const findArithmeticOperator = (input) => {
  const arithmeticOperators = ['+','=' , '-', '*', '/','.',',','?',';',':','>','<','{}','{','}','"','()','(',')','*','&','^','%','$','#','@','!'];
  return arithmeticOperators.find(operator => input.includes(operator));
};

const isValidChip = (input) => {
  return data.includes(input);
};

watch(inputValue, handleInput);

const hasArithmeticOperator = computed(() => isArithmeticOperator(findArithmeticOperator(inputValue.value)));
const updateAutocompletePosition = () => {
  const inputField = inputFieldRef.value;
  const autocompleteContainer = autocompleteContainerRef.value;
  const inputRect = inputField.getBoundingClientRect();
  const autocompleteRect = autocompleteContainer.getBoundingClientRect();
  const leftOffset = inputRect.left + window.pageXOffset + inputField.offsetWidth - autocompleteRect.width - -130;
  const topOffset = inputRect.top + window.pageYOffset + inputField.offsetHeight;
  inputFieldLeft.value = `${leftOffset}px`;
  inputFieldTop.value = `${topOffset}px`;
};

// Update the position of the autocomplete container when the input field is clicked or the input value changes
watch([showAutocomplete, inputValue], () => {
  if (showAutocomplete.value) {
    updateAutocompletePosition();
  }
});

// Update the position of the autocomplete container when the window is resized
window.addEventListener('resize', () => {
  if (showAutocomplete.value) {
    updateAutocompletePosition();
  }
});
</script>

<style>
/* Main container styles */
#app {
  font-family: Arial, sans-serif;
  width: 100%;
}

.input-container {
  position: relative;
  border: 1px solid #ccc;
  display: flex;
  align-items: center;
  margin: auto;
  width: 97%;
  min-height: 60px;
  border-radius: 5px;
  margin-top: 10px;
  height: auto;
  flex-wrap: wrap;
  padding: 8px;
  box-sizing: border-box;
}

.chip-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 4px;
}

.chip-tag {
  padding: 4px;
  border-radius: 4px;
  display: flex;
  align-items: center;
}

.auto{
  margin-left: 5px;
}

.chip-tag.typed-chip {
  margin-left: -3px;
  margin-right: -1px;
  background-color: rgb(231, 227, 227);
  border: 1px solid rgb(175, 175, 175);
  /* Pink background for typed word chips */
}

.chip-tag.arithmetic {
  background-color: transparent; /* Remove background color from arithmetic operators */
  padding: 0px;
}

.chip-tag button {
  margin-left: 4px;
  border: none;
  background-color: transparent;
  color: rgb(134, 133, 133);
  font-size: 16px;
  cursor: pointer;
  display: inline;
}

/* Autocomplete styles */
.input-text {
  border: none;
  flex: 1;
  outline: none;
  resize: none;
  overflow: hidden;
  font-size: 16px;
  line-height: 1.4;
  position: relative; /* Add position relative to the input field */
}

.autocomplete {
  position: absolute;
  z-index: 1;
  width: 100%;
  max-width: 405px;
  overflow-y: auto;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  /* Initially hidden */
  visibility: hidden;
  opacity: 0;
  transition: opacity 0.2s ease;
}

.autocomplete.show {
  visibility: visible;
  opacity: 1;
  top: 50px !important;
  /* The left and top positions will be updated dynamically in JavaScript */
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  padding: 4px;
    cursor: pointer;
    height: 45px;
    display: flex;
    align-items: center;
    border-radius: 7px;
    font-size: 18px;
    font-weight: 500;
}

li.active {
  background-color: rgb(62, 124, 230); /* Light blue background for active sentence in autocomplete */
}
</style>
