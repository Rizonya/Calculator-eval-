<script lang="ts">
	import { parse } from 'cookie';
	import { each } from 'svelte/internal';
 import Button from './CustomButton.svelte'
 import Undo from '$lib/images/undo-outline.svg'
 let grid = [['1','2','3','/'],['4','5','6','*'],['7','8','9','+']]
 let str:string = "Введите что нибудь";
 let sus = ['.','/','*','+','-']
 let nums = '0123456789'
 $:if(str==''){
	str = "Введите что нибудь"
 }
 
 function addToStr(key:string){
	if(str =="Введите что нибудь"&& '.+*/'.includes(key)) return
	console.log(str);
	let dotCheck = key =='.'? !str.split(/[\/\*\+\-]/).at(-1)?.includes('.'):true;
	let endIsNum = (str=='Введите что нибудь'||(sus.includes(str.at(-1)!)&&!sus.includes(key)))?true:!isNaN(parseInt(str.at(-1)!))
	console.log(dotCheck);
	
	if(grid.flat().includes(key) || key =='.'||key=='-'){
		console.log(1);
		
		if((endIsNum&&dotCheck)||(str =='-'&& !sus.includes(key)) || ( str == "Введите что нибудь"&&key =='-' )){
			console.log(dotCheck);
			
			str!='Введите что нибудь' ? str += key : str = key
		}
		
	}
 }
 function keyPressHandler(event:KeyboardEvent){
	
	addToStr(event.key);
 };
</script>
<svelte:body on:keypress={keyPressHandler}></svelte:body>
<div  class="flex flex-col gap-4 justify-center items-center">
	<div class="flex flex-row justify-center items-center ">
		
			<h1 class='font-black text-xl'>{str}</h1>
		
		
	</div>
	<div class="flex gap-4 flex-row justify-center items-center">
		<Button on:click={()=>str='Введите что нибудь'} on:keydown Class="w-52 justify-center text-3xl flex flex-col items-center h-24  rounded-xl cursor-pointer  bg-amber-300 ">
			<p class="flex  flex-row">c</p>
		</Button>
		<Button on:click={()=>str=str=='Введите что нибудь'?str:str.substring(0,str.length-1)
		} on:keydown={()=>{}} Class="text-3xl w-24 justify-center flex flex-col items-center h-24  rounded-xl cursor-pointer  bg-amber-300 ">
			<img class="flex w-8 h-8 flex-row" src={Undo} alt=''/>
		</Button>
		
		<Button on:keydown on:click={()=>addToStr('-')} Class="w-24 justify-center flex flex-col items-center h-24 rounded-xl cursor-pointer  bg-amber-300 ">
			<p class="flex flex-row">-</p>
		</Button>
	</div>
	{#each grid as row}
	<div class="flex gap-4 flex-row justify-center items-center" >
		{#each row as value }
			<Button on:click={()=>addToStr(value)} {value}></Button>
		{/each}
	</div>
	{/each}
	<div class="flex gap-4 flex-row justify-center items-center">
		
		<Button on:click={()=>addToStr('0')} value='0'/>
		<Button on:click={()=>addToStr('.')} value='.'/>
		<Button on:keydown on:click={()=>{try {
			str = eval(str)
			str = str.toString()
			
		} catch (error) {

		}
			}} Class="w-52 justify-center flex flex-col items-center h-24   rounded-xl cursor-pointer  bg-amber-300 ">
			<p class="flex flex-row">=</p>
		</Button>
	</div>

</div>




<style>

</style>
