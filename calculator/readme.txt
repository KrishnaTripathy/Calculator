Sure! Let's go through the code line by line to understand what each part does.

### Class Definition

```javascript
class Calculator {
```
- Defines a new class named `Calculator`.

```javascript
    constructor(previousOperandTextElement, currentOperandTextElement){
        this.previousOperandTextElement = previousOperandTextElement
        this.currentOperandTextElement = currentOperandTextElement
        this.clear()
    }
```
- The constructor initializes the `Calculator` object with `previousOperandTextElement` and `currentOperandTextElement` (HTML elements where the operands will be displayed).
- It then calls the `clear` method to initialize the calculator's state.

### Clear Method

```javascript
    clear() {
       this.currentOperand = ''
       this.previousOperand = ''
       this.operation = undefined
    }
```
- The `clear` method resets the current and previous operands to empty strings and the operation to `undefined`.

### Delete Method

```javascript
    delete() {
     this.currentOperand = this.currentOperand.toString().slice(0, -1)
    }
```
- The `delete` method removes the last character from the `currentOperand`.

### AppendNumber Method

```javascript
    appendNumber(number){
        if(number === '.' && this.currentOperand.includes('.')) return;
        this.currentOperand = this.currentOperand.toString() + number.toString()
    }
```
- The `appendNumber` method adds a number to the `currentOperand`. It prevents adding multiple decimal points by checking if a `.` is already included.

### ChooseOperation Method

```javascript
    chooseOperation(operation) {
        if(this.currentOperand === '') return;
        if(this.previousOperand !== ''){
            this.compute()
        }
        this.operation = operation
        this.previousOperand = this.currentOperand
        this.currentOperand = ''
    }
```
- The `chooseOperation` method sets the chosen operation.
- If the `currentOperand` is empty, it returns early.
- If there is already a `previousOperand`, it calls `compute` to perform the existing operation before setting a new one.
- It then sets the operation and moves the `currentOperand` to `previousOperand`, and clears the `currentOperand`.

### Compute Method

```javascript
    compute(){
        let computation
        const prev = parseFloat(this.previousOperand)
        const current = parseFloat(this.currentOperand)
        if(isNaN(prev) || isNaN(current)) return 
        switch(this.operation){
            case '+' :
                computation = prev + current
                break
            case '-' :
                computation = prev - current
                break
            case '*' :
                computation = prev * current
                break
            case '÷' :
                computation = prev / current
                break
            default:
                return
        }
        this.currentOperand = computation
        this.operation = undefined
        this.previousOperand = ''
    }
```
- The `compute` method performs the chosen operation.
- It converts `previousOperand` and `currentOperand` to numbers.
- If either operand is not a number, it returns early.
- It performs the operation using a `switch` statement and stores the result in `currentOperand`.
- It then resets `operation` to `undefined` and clears `previousOperand`.

### GetDisplayNumber Method

```javascript
    getDisplayNumber(number){
        const stringNumber = number.toString()
        const integerDigits = parseFloat(stringNumber.split('.')[0])
        const decimalDigits = stringNumber.split('.')[1]
        let integerDisplay
        if(isNaN(integerDigits)){
            integerDisplay = ''
        }else {
            integerDisplay = integerDigits.toLocaleString('en', {
                maximumFractionDigits: 0 
            })
        }
        if(decimalDigits != null){
            return `${integerDisplay}.${decimalDigits}`
        }else{
            return integerDisplay
        }
    }
```
- The `getDisplayNumber` method formats the number for display.
- It splits the number into integer and decimal parts.
- It formats the integer part with commas for thousands and retains the decimal part if it exists.
- It returns the formatted number as a string.

### UpdateDisplay Method

```javascript
    updateDisplay() {
          this.currentOperandTextElement.innerText = this.getDisplayNumber(this.currentOperand)
          if(this.operation != null){
            this.previousOperandTextElement.innerText =
            `${this.getDisplayNumber(this.previousOperand)} ${this.operation}`
          }
          else{
            this.previousOperandTextElement.innerText = ''
          }
    }
```
- The `updateDisplay` method updates the text content of the operand display elements.
- It sets the `currentOperandTextElement` to the formatted `currentOperand`.
- If an operation is chosen, it sets the `previousOperandTextElement` to the formatted `previousOperand` and the operation symbol.
- Otherwise, it clears the `previousOperandTextElement`.

### HTML Element Selection

```javascript
const numberButtons = document.querySelectorAll('[data-number]')
const operationButtons = document.querySelectorAll('[data-operation]')
const equalsButton = document.querySelector('[data-equals]')
const deleteButton = document.querySelector('[data-delete]')
const allClearButton = document.querySelector('[data-all-clear]')
const previousOperandTextElement = document.querySelector('[data-previous-operand]')
const currentOperandTextElement = document.querySelector('[data-current-operand]')
```
- These lines select the HTML elements based on data attributes and store them in variables.

### Calculator Instance

```javascript
const calculator = new Calculator(previousOperandTextElement, currentOperandTextElement)
```
- This line creates a new instance of the `Calculator` class, passing in the selected operand text elements.

### Event Listeners

```javascript
numberButtons.forEach(button => {
    button.addEventListener('click', () => {
        calculator.appendNumber(button.innerText)
        calculator.updateDisplay()
    })
})
```
- Adds click event listeners to all number buttons.
- When a number button is clicked, it appends the number to the current operand and updates the display.

```javascript
operationButtons.forEach(button => {
    button.addEventListener('click', () => {
        calculator.chooseOperation(button.innerText)
        calculator.updateDisplay()
    })
})
```
- Adds click event listeners to all operation buttons.
- When an operation button is clicked, it sets the chosen operation and updates the display.

```javascript
equalsButton.addEventListener('click', button => {
    calculator.compute()
    calculator.updateDisplay()
})
```
- Adds a click event listener to the equals button.
- When clicked, it performs the computation and updates the display.

```javascript
allClearButton.addEventListener('click', button => {
    calculator.clear()
    calculator.updateDisplay()
})
```
- Adds a click event listener to the all-clear button.
- When clicked, it clears the calculator's state and updates the display.

```javascript
deleteButton.addEventListener('click', button => {
    calculator.delete()
    calculator.updateDisplay()
})
```
- Adds a click event listener to the delete button.
- When clicked, it deletes the last character from the current operand and updates the display.