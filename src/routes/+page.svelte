<script lang="ts">
	import Button from './CustomButton.svelte';
	import Undo from '$lib/images/undo-outline.svg';
	let grid = [
		['1', '2', '3', '/'],
		['4', '5', '6', '*'],
		['7', '8', '9', '+']
	];
	const placeholder: string = 'Введите что нибудь';
	let str: string = placeholder;
	let marks = ['.', '/', '*', '+', '-'];
	$: if (str == '') {
		str = placeholder;
	}

	function addToStr(key: string) {
		if (str == placeholder && '.+*/'.includes(key)) return;
		let dotCheck =
			key == '.'
				? !str
						.split(/[\/\*\+\-]/)
						.at(-1)
						?.includes('.')
				: true;
		let endIsNum =			str == placeholder || (marks.includes(str.at(-1)!) && !marks.includes(key))
				? true
				: !isNaN(parseInt(str.at(-1)!));
		if (grid.flat().includes(key) || key == '.' || key == '-') {
			if (
				(endIsNum && dotCheck) ||
				(str == '-' && !marks.includes(key)) ||
				(str == placeholder && key == '-')
			) {
				str != placeholder ? (str += key) : (str = key);
			}
		}
	}
	function keyPressHandler(event: KeyboardEvent) {
		addToStr(event.key);
	}
</script>

<svelte:body on:keypress={keyPressHandler} />
<div class="flex flex-col gap-4 justify-center items-center">
	<div class="flex flex-row justify-center items-center ">
		<h1 class="font-black text-xl">{str}</h1>
	</div>
	<div class="flex gap-4 flex-row justify-center items-center">
		<Button
			on:click={() => (str = placeholder)}
			on:keydown
			Class="w-52 justify-center text-3xl flex flex-col items-center h-24  rounded-xl cursor-pointer  bg-amber-300 "
		>
			<p class="flex  flex-row">c</p>
		</Button>
		<Button
			on:click={() => (str = str == placeholder ? str : str.substring(0, str.length - 1))}
			on:keydown={() => {}}
			Class="text-3xl w-24 justify-center flex flex-col items-center h-24  rounded-xl cursor-pointer  bg-amber-300 "
		>
			<img class="flex w-8 h-8 flex-row" src={Undo} alt="" />
		</Button>

		<Button
			on:keydown
			on:click={() => addToStr('-')}
			Class="w-24 justify-center flex flex-col items-center h-24 rounded-xl cursor-pointer  bg-amber-300 "
		>
			<p class="flex flex-row">-</p>
		</Button>
	</div>
	{#each grid as row}
		<div class="flex gap-4 flex-row justify-center items-center">
			{#each row as value}
				<Button on:click={() => addToStr(value)} {value} />
			{/each}
		</div>
	{/each}
	<div class="flex gap-4 flex-row justify-center items-center">
		<Button on:click={() => addToStr('0')} value="0" />
		<Button on:click={() => addToStr('.')} value="." />
		<Button
			on:keydown
			on:click={() => {
				try {
					str = eval(str);
					str = str.toString();
				} catch (error) {}
			}}
			Class="w-52 justify-center flex flex-col items-center h-24   rounded-xl cursor-pointer bg-red-900 text-white "
		>
			<p class="flex flex-row">=</p>
		</Button>
	</div>
</div>
