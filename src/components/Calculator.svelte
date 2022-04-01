<script lang="ts">
	import { onMount } from 'svelte';
	onMount(() => {
		console.log('the component has mounted');
	});
	
	let value: string = '';
	let result: string = '';

	function isOperator(operator) {
		if(operator === '+' || operator === '-' || operator === 'x' || operator === '/')
			return true;
		else
			return false;
	}
	function isDoubleOperator() {
		let operators = ['+','-','x','/'];
		let returnVal = false;
		operators.forEach(op => {
			operators.forEach(op2 => {
				if(value.includes(op+op2))
					returnVal = true;
			});
		});
		return returnVal;
	}
	function isValid() {
		if(isOperator(value[0]) || isOperator(value[value.length-1]))
			return false;
		if(isDoubleOperator())
			return false;
		return true;
	}
	function convertStrToArr() {
		let arr = [];
		let lastOperator = 0;
		value.split('').forEach((element,index) => {
			if(isOperator(element)) {
				arr.push(parseInt(value.substring(lastOperator,index)))
				arr.push(value.substring(index,index+1))
				lastOperator = index+1;
			}
		});
		arr.push(parseInt(value.substring(lastOperator,value.length)))
		return arr;
	}

	const select = num => () => {
		value += num;
    }
	const clear  = () => {
        value = '';
        result = '';
    }
	function applyOperator(a,b,op) {
		switch(op) {
		case '+':
			return a+b;
		case '-':
			return a-b;
		case 'x':
			return a*b;
		case '/':
			return a/b;
	}
	} 
	function calculateArr(equation) {
		let i = 0, calculation = 0;
		while(equation.length >= 2) {
			if(equation.indexOf('/') > 0) {
				calculation = applyOperator(equation[equation.indexOf('/')-1],equation[equation.indexOf('/')+1],'/');
            	equation.splice(equation.indexOf('/')-1,3,calculation);
			}
			else if(equation.indexOf('x') > 0) {
				calculation = applyOperator(equation[equation.indexOf('x')-1],equation[equation.indexOf('x')+1],'x');
            	equation.splice(equation.indexOf('x')-1,3,calculation);
			}
			else{
				if(equation[i] == '+') {
					calculation = applyOperator(equation[i-1],equation[i+1],'+');
					equation.splice(i-1,3,calculation);
					i=-1;
				}
				else if(equation[i] == '-') {
					calculation = applyOperator(equation[i-1],equation[i+1],'-');
					equation.splice(i-1,3,calculation);
					i=-1;
				}
				i++;
			}
		}
		return equation[0];
	}
	const submit = () => { 
        try{
			if(isValid()) {
				let equation = convertStrToArr();
				result = calculateArr(equation);
            	// result = eval(value);
			}
			else {
				throw 'Equation not valid';
			}	
		}
        catch (err) {
            window.alert(err);
            value = '';
        }
    }
	let buttons = [
		{ id: '7', name: '7' },
		{ id: '8', name: '8' },
		{ id: '9', name: '9' },
		{ id: '4', name: '4' },
		{ id: '5', name: '5' },
		{ id: '6', name: '6' },
		{ id: '1', name: '1' },
		{ id: '2', name: '2' },
		{ id: '3', name: '3' },
		{ id: '0', name: '0' },
	];
</script>

<main>
    <div class="equation">
        Equation: {value} 
    </div>
    <div class="home">
        <div class="keypad">
            {#each buttons as { name }}
                <button on:click={select(name)}>{name}</button>
            {/each}
            <button disabled={!value} on:click={clear}>c</button>
            <button disabled={!value} on:click={submit}>=</button>
        </div>
        <div class="keypad2">
            <button on:click={select('+')}>+</button>
            <button on:click={select('-')}>-</button>
            <button on:click={select('x')}>x</button>
            <button on:click={select('/')}>/</button>
        </div>
    </div>
    Result: {result}
</main>

<style>
	.home {
		display: flex;
        flex-direction: row;
		align-items: center;
        justify-content: center;
        width: 100%;
        height: 100%;
	}
	.equation {
		display: flex;
        flex-direction: row;
        width: 100%;
	}
	.keypad {
		display: grid;
		grid-template-columns: repeat(3, 5em);
		grid-template-rows: repeat(4, 3em);
		grid-gap: 0.5em;
        margin-right: 8px;
	}
	.keypad2 {
		display: grid;
		grid-template-columns: repeat(1, 5em);
		grid-template-rows: repeat(4, 3em);
		grid-gap: 0.5em;
        margin-right: 8px;
	}

	button {
		margin: 0
	}
</style>