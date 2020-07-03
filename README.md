
<div align="center">

![input](https://raw.githubusercontent.com/keegandonley/input-deno/master/.github/input.png)

</div>

# Input-Deno
![Tests](https://github.com/keegandonley/input-deno/workflows/Tests/badge.svg)

Used to get command line input from the user

## Usage
```javascript
import InputLoop from 'https://raw.githubusercontent.com/Yazidn/input-deno/master/install/input.ts';
```

## Choose
Returns an array indicating which index was selected

```javascript
const input = new InputLoop();
const accepting = await input.choose(["Accepting node", "Non-accepting node"]);

// output:

// ------------------------------
// 0: Accepting node
// 1: Non-accepting node
// ------------------------------

// Return value:
// [false, true]
```

You can further customize the behavior of choose() by passing an object of perferences as a second argument.
All preferences are optional, as shows in the interface below.

```javascript
interface Preferences {
	lastOptionClose?: boolean,
	choice?: string | number,
	displayInline?: boolean, // if true, display all options in the same line
	inlineSpacing?: number, // number of spaces between inline options, default is 2
	indexStyle?: string[],
	dividerUp?: boolean,
	dividerBottom?: boolean
	dividerLength?: number
	dividerChar?: string,
	dividerPadding?: boolean
}

// Example
const input = new InputLoop();
await input.choose(['Option 1', 'Option 2', 'Option 3', 'Option 4', 'Option 5', 'Option 6', 'Quit'], {
	lastOptionClose: true,
	displayInline: true,
	indexStyle: [
		'( ', ' )'
	],
	inlineSpacing: 5
});

// Output
// ( 0 ) Option 1     ( 1 ) Option 2     ( 2 ) Option 3     ( 3 ) Option 4     ( 4 ) Option 5     ( 5 ) Option 6     ( 6 ) Quit
```

## Question
Ask a single question

```javascript
const input = new InputLoop();
const nodeName = await input.question('Enter the label for the node:');

// output:

// Enter the label for the node:

// Return Value:
// 'a'
```

Skip the newline after printing the question:
```javascript
const input = new InputLoop();
const nodeName = await input.question('Enter the label for the node:', false);

// output:

// Enter the label for the node:

// Return Value:
// 'a'
```

## Looping
Control an input loop which continues reprompting until terminated

#### Automatic Looping:
Passing `true` as the second argument will automatically end the iteration of the last option is selected.

```javascript
const input = new InputLoop();
const mainQuestions = ["Add a node", "Add an edge", "Set starting node", "Evaluate a string", "Quit"];

while (!input.done) {
	const result = await input.choose(mainQuestions, true);

	// Business logic...
}
```

#### Manual Looping:
```javascript
const input = new InputLoop();
const mainQuestions = ["Add a node", "Add an edge", "Set starting node", "Evaluate a string", "Quit"];

while (!input.done) {
	const result = await input.choose(mainQuestions);

	// Business logic...

	if (result[mainQuestions.length - 1]) {
		input.close();
	}
}
```

## Repeat
Repeat the previous question

```javascript
const input = new InputLoop();
const mainQuestions = ["Add a node", "Add an edge", "Set starting node", "Evaluate a string", "Quit"];

let result = await input.choose(mainQuestions);

while (!(result[0] || result[1])) {
	result = input.repeat();
}

```

## Testing
Deno tests can be run using:
```
make test
```
