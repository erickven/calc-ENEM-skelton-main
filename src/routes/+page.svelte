<script lang="ts">
	import { data, type Data } from './data';
	import logo from '$lib/icons8-github-50.png';
	import calculadora from '$lib/icons8-calculadora-de-maçã-50 (1).png'
	import { LightSwitch } from '@skeletonlabs/skeleton';
	import { Avatar } from '@skeletonlabs/skeleton';
	import { ProgressBar } from '@skeletonlabs/skeleton';
	import { ListBox, ListBoxItem } from '@skeletonlabs/skeleton';
	import { AppShell } from '@skeletonlabs/skeleton';
	import Navigation from '$lib/Navigation.svelte';
	import { initializeStores, Drawer, getDrawerStore } from '@skeletonlabs/skeleton';
	initializeStores();

	const drawerStore = getDrawerStore();
	function drawerOpen(): void {
	drawerStore.open({});
}

	let valueMultiple: string[] = ['books', 'movies'];

	let mathGrade: number[][] = [
		[30, 714, 800, 879],
		[35, 760, 850, 932],
		[40, 820, 900, 978],
		[45, 950, 970, 986]
	];

	let arr: Data[] = data;

	function calcularNota(materia: Data) {
		return (materia.nota * materia.peso).toFixed(2);
	}

	function calcularScore(arr: Data[]) {
		return arr.reduce((total, subject) => {
			return (total + subject.peso * subject.nota);
		}, 0);
	}

	function calcularSomaDosPesos(arr: Data[]) {
		return arr.reduce((sum, subject) => {
			return Math.round(sum + subject.peso);
		}, 0);
	}

	function calcularScoreFinal(arr: Data[]) {
		return (calcularScore(arr) / calcularSomaDosPesos(arr));
	}

	function calcularSimulation(arr: Data[], math: number) {
		const simuArr = JSON.parse(JSON.stringify(arr)).map((n: Data) =>
			n.prova === 'Matemática e suas tecnologias' ? { ...n, nota: math } : n
		);

		return calcularScoreFinal(simuArr);
	}
</script>
<body class="bg-gradient-to-r from-slate-900 to-surface-950 text-tertiary-500 text-xl flex-col space-between m-0 p-0 overflow-x-auto" data-theme="crimson">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<AppShell>
	<!-- <svelte:fragment slot="sidebarLeft">
    <div class="flex items-center">
        <button class="lg:hidden btn btn-sm mr-4">
            <span>
                <svg viewBox="0 0 100 80" class="fill-token w-4 h-4">
                    <rect width="100" height="20" />
                    <rect y="30" width="100" height="20" />
                    <rect y="60" width="100" height="20" />
                </svg>
            </span>
        </button>
        <strong class="text-xl uppercase">Skeleton</strong>
    </div>
</svelte:fragment> -->
	<svelte:fragment slot="header"><header><div class="bg-surface-100-800-token py-5 w-screen shadow-xl shadow-surface-950 flex flex-row justify-between"><button class="w-40 rounded-2xl ml-4 border-tertiary-500 text-sm sm:max-lg:h-8 border-2 bg-transparent text-tertiary-500" on:click={drawerOpen}><strong>Mostrar Resultados</strong></button><h1 class="text-center inline-block text-2xl px-4 font-serif"><strong>Calculadora do ENEM</strong></h1><div class="flex flex-row"><a href="https://github.com/erickven" target="_blank" class="btn-icon variant-filled w-10 mr-2 sm:max-lg:h-8"><img class="w-8 flex" src="{logo}" alt="" srcset=""></a></div></div></header>
</svelte:fragment>
	<svelte:fragment slot="sidebarRight">
	<Drawer>
	<div id="sidebar-right" class="">
	<aside>
	<div id="resultado" class="w-screen h-full variant-ghost-surface mr-0 pt-3 text-center text-tertiary-300 rounded-xl text-xl">
<h1 class="text-center mb-2"><strong>De Acordo Com A Sua Nota: </strong></h1>		
<h3>Com base nos dados entre 2020 e 2023</h3>
{#each mathGrade as m}
	<p>Caso você tivesse acertado {m[0]} questões em matemática sua nota seria:</p>
	Entre {calcularSimulation(arr, m[1]).toFixed(2)} e {calcularSimulation(arr, m[3]).toFixed(2)}
	<br>
	com média de {calcularSimulation(arr, m[2]).toFixed(2)}
	<ProgressBar />
{/each}
<hr>
<p class="text-center text-lg py-3 my-3 bg-tertiary-500 text-tertiary-900"><strong>Nunca Fez, Quer Fazer O Enem Esse Ano ou Não Sabe Se A Universidade Publica Do Seu Estado Aceita Enem?</strong></p>
<Navigation />
</div>
</aside>
</div>
</Drawer>
</svelte:fragment>
<main>
<p class="py-3 px-4 mb-12">
	Esta calculadora serve para você entender que se tivesse aprendido matemática não teria tirado
	essa nota paia 😹
</p>
<div id="tabelas" class="pl-7 mb-12 inline-block">
<table class="border-2 border-surface-700 border-collapse m-auto relative">
	<tr class="bg-primary-800">
		<th>Prova do Enem</th><th>Nota mínima</th><th>Sua nota</th><th>Peso</th><th>Nota com peso</th>
	</tr>
	{#each arr as materia}
		<tr class="sm:max-xl:table-fixed text-center">
			<td class="">{materia.prova}</td>
			<td class=""><input type="number" class="w-16 rounded-lg pt-3 flex-col bg-surface-800 m-1 sm:w-32 md:w-44 text-center sm:max-xl:w-16" placeholder="0.01" bind:value={materia.minGrade} /></td>
			<td class=""><input type="number" class="w-16 rounded-lg pt-3 flex-col m-1 bg-surface-800 sm:w-32 md:w-44 text-center sm:max-xl:w-16" placeholder="780.00" bind:value={materia.nota} /></td>
			<td class=""><input type="number" class="w-16 rounded-lg pt-3 flex-col m-1 bg-surface-800 sm:w-32 md:w-44 text-center sm:max-xl:w-16" placeholder="1.50" bind:value={materia.peso} /></td>
			<td class="">{calcularNota(materia)}</td>
		</tr>
	{/each}

	<tr class="text-primary-700">
		<td /><td /><td>Total</td><td>Total</td>
	</tr>
	<tr>
		<td /><td /><td>{calcularScore(arr).toFixed(2)}(A)</td><td>{calcularSomaDosPesos(arr).toFixed(2)}(B)</td>
	</tr>
	<td><i>Nota do estudante (B/A) = {calcularScoreFinal(arr).toFixed(2)}</i></td>
</table>
</div>
</main>
	<slot />
	<svelte:fragment slot="pageFooter"><h3 class="pl-4 mb-3">Após Essa Nota Paia Vc Pretende Estudar: </h3>
<ListBox multiple class="gap-5 inline">
	<ListBoxItem bind:group={valueMultiple} name="medium" value="Math">Matematica</ListBoxItem>
	<ListBoxItem bind:group={valueMultiple} name="medium" value="Portuguese">Português</ListBoxItem>
	<ListBoxItem bind:group={valueMultiple} name="medium" value="Redacao">Redação</ListBoxItem>
</ListBox></svelte:fragment>
</AppShell>
</body>
