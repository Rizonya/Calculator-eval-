<script lang="ts">
	export let value: string = '';
	export let Class: string =
		'text-3xl hover:bg-amber-200  w-24  justify-center flex flex-col items-center h-24  rounded-xl cursor-pointer  bg-amber-300 ';
</script>
<div on:click on:keydown={() => {}} class={Class + 'hover:bg-amber-200 duration-200'}>
	<p class="flex flex-row">{value}</p>
	<slot />
</div>
