<script lang="ts">
	import { data, type Data } from './data';
	import logo from '$lib/icons8-github-50.png';
	import calculadora from '$lib/icons8-calculadora-de-maçã-50 (1).png'
	import math from '$lib/icons8-matemática-50.png'
	import abc from '$lib/icons8-abc-30.png'
	import redacao from '$lib/icons8-documento-64.png'
	import question from '$lib/icons8-question-64.png'
	import problema from '$lib/icons8-problem-50.png'
	import fisica from '$lib/icons8-física-50.png'
	import quimica from '$lib/icons8-sódio-50.png'
	import duvida from '$lib/icons8-decisão-50 (1).png'
	import biologia from '$lib/icons8-pegada-de-cachorro-30.png'
	import { LightSwitch } from '@skeletonlabs/skeleton';
	import { Avatar } from '@skeletonlabs/skeleton';
	import { ProgressBar } from '@skeletonlabs/skeleton';
	import { ListBox, ListBoxItem } from '@skeletonlabs/skeleton';
	import { AppShell } from '@skeletonlabs/skeleton';
	import Navigation from '$lib/Navigation.svelte';
	import { page } from '$app/stores';
	import { TabGroup, Tab, TabAnchor } from '@skeletonlabs/skeleton';
	import { AppBar } from '@skeletonlabs/skeleton';
	import { initializeStores, Drawer, getDrawerStore } from '@skeletonlabs/skeleton';
	import pergunta from '$lib/icons8-pergunta-30.png'
	import pessoa from '$lib/icons8-pele-tipo-3-em-dúvida-48.png'
	import { Accordion, AccordionItem } from '@skeletonlabs/skeleton';
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
<svelte:head>
	<style>
		.merriweather-light {
  font-family: "Merriweather", serif;
  font-weight: 300;
  font-style: normal;
}

.merriweather-regular {
  font-family: "Merriweather", serif;
  font-weight: 400;
  font-style: normal;
}

.merriweather-bold {
  font-family: "Merriweather", serif;
  font-weight: 700;
  font-style: normal;
}

.merriweather-black {
  font-family: "Merriweather", serif;
  font-weight: 900;
  font-style: normal;
}

.merriweather-light-italic {
  font-family: "Merriweather", serif;
  font-weight: 300;
  font-style: italic;
}

.merriweather-regular-italic {
  font-family: "Merriweather", serif;
  font-weight: 400;
  font-style: italic;
}

.merriweather-bold-italic {
  font-family: "Merriweather", serif;
  font-weight: 700;
  font-style: italic;
}

.merriweather-black-italic {
  font-family: "Merriweather", serif;
  font-weight: 300;
  font-style: italic;
}

	</style>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com">
<link href="https://fonts.googleapis.com/css2?family=Merriweather:ital,wght@0,300;0,400;0,700;0,900;1,300;1,400;1,700;1,900&display=swap" rel="stylesheet">
</svelte:head>
<body class="bg-surface-50 dark:bg-gradient-to-r from-slate-900 to-surface-950 text-center text-surface-900 dark:text-tertiary-500 font-bold dark:font-normal text-sm md:max-2xl:text-lg flex-col space-between m-0 p-0 overflow-x-auto" data-theme="crimson">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<AppShell>
	<svelte:fragment slot="header">
		<AppBar gridColumns="grid-cols-3" class="bg-surface-100 shadow-xl dark:bg-surface-800" slotDefault="place-self-center" slotTrail="place-content-end">
	<svelte:fragment slot="lead"><button class="text-sm w-40 rounded-2xl border-surface-800 dark:border-tertiary-500 text-sm sm:max-lg:h-8 sm:py-2 border-2 bg-transparent text-surface-800 dark:text-tertiary-500" on:click={drawerOpen}><strong>Mostrar Resultados</strong></button></svelte:fragment>
<span><h1 class="text-center inline-block text-lg sm:text-2xl px-4 font-serif"><strong>Calculadora do ENEM</strong></h1></span>
	<svelte:fragment slot="trail"><div class="flex justify-center">
		<LightSwitch />
		</div><span><div class="flex flex-row"><a href="https://github.com/erickven" target="_blank" class="bg-transparent hover:bg-primary-400 shadow-2xl btn-icon variant-filled w-10 sm:max-lg:h-8"><img class="w-8 flex" src="{logo}" alt="" srcset=""></a></div></span>
</svelte:fragment>
</AppBar>
</svelte:fragment>
	<svelte:fragment slot="sidebarRight">
	<Drawer>
	<div id="sidebar-right" class="">
	<aside>
	<div id="resultado" class="w-full h-full variant-ghost-surface mr-0 pt-3 text-center text-surface-900 dark:text-tertiary-300 rounded-xl text-sm">
<h1 class="text-center mb-2"><strong>De acordo com a sua nota: </strong></h1>		
<h3>Com base nos dados entre 2020 e 2023</h3>
{#each mathGrade as m}
	<p>Caso você tivesse acertado {m[0]} questões em matemática sua nota seria:</p>
	Entre {calcularSimulation(arr, m[1]).toFixed(2)} e {calcularSimulation(arr, m[3]).toFixed(2)}
	<br>
	com média de {calcularSimulation(arr, m[2]).toFixed(2)}
	<ProgressBar />
{/each}
<hr>
<p class="text-center text-lg py-3 my-3 bg-primary-700 dark:bg-tertiary-500 text-tertiary-50 dark:text-tertiary-900"><strong>Nunca fez, quer fazer o enem esse ano ou não sabe se a universidade publica do seu estado aceita enem?</strong></p>
<Navigation />
</div>
</aside>
</div>
</Drawer>
</svelte:fragment>
<main>
<p class="py-3 px-4 mb-12 font-bold my-8">
	Esta calculadora serve para você entender que se tivesse aprendido matemática não teria tirado
	essa nota paia 😹
</p>
<div id="tabelas" class="sm:py-5 mb-12 inline-block">
<table class="border-2 shadow-xl dark:shadow-sm border-surface-700 border-collapse m-auto relative">
	<tr class="bg-primary-600 dark:bg-primary-800">
		<th>Prova do Enem</th><th>Nota mínima</th><th>Sua nota</th><th>Peso</th><th>Nota com peso</th>
	</tr>
	{#each arr as materia}
		<tr class="sm:max-xl:table-fixed text-center">
			<td class="">{materia.prova}</td>
			<td class=""><input type="number" class="w-16 rounded-lg pt-3 flex-col sm:m-1 bg-tertiary-800 dark:bg-surface-800 sm:w-32 md:w-44 text-center sm:max-xl:w-16" placeholder="0.01" bind:value={materia.minGrade} /></td>
			<td class=""><input type="number" class="w-16 rounded-lg pt-3 flex-col sm:m-1 bg-tertiary-800 dark:bg-surface-800 sm:w-32 md:w-44 text-center sm:max-xl:w-16" placeholder="780.00" bind:value={materia.nota} /></td>
			<td class=""><input type="number" class="w-16 rounded-lg pt-3 flex-col sm:m-1 bg-tertiary-800 dark:bg-surface-800 sm:w-32 md:w-44 text-center sm:max-xl:w-16" placeholder="1.50" bind:value={materia.peso} /></td>
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
<h3 class="h3 text-center font-bold mt-10 underline">Algumas Duvidas</h3>
<Accordion class="mt-6 bg-surface-900 dark:bg-surface-900 text-tertiary-500 text-left shadow-xl rounded-2xl">
	<AccordionItem>
		<svelte:fragment slot="lead"><img src="{duvida}" class="w-8" alt="" srcset=""></svelte:fragment>
		<svelte:fragment slot="summary"><strong>Como se usa a calculadora?</strong></svelte:fragment>
		<svelte:fragment slot="content">
			<p>Para usar a calculadora você deve consultar suas notas no site do inep, e inserir as suas notas no campo acima, ao final de tudo isso vá no canto superior esquerdo a tela onde tem um botão escrito "Mostrar Resultados", ao apertar nele lá estará o resultado da sua nota caso você tivesse acertado determinadas questões de matemática na prova.</p>
		</svelte:fragment>
	</AccordionItem>
	<AccordionItem>
		<svelte:fragment slot="lead"><img src="{question}" class="w-8" alt="" srcset=""></svelte:fragment>
		<svelte:fragment slot="summary"><strong>Oque fazer para tirar uma boa nota no Enem?</strong></svelte:fragment>
		<svelte:fragment slot="content">
			<p class="font-bold pl-2">Algumas dicas valiosas:</p>
			<ul class="list-decimal pl-6">
				<li>Não perca o foco, procure orientação e estude com mais afinco as matérias que realmente têm peso nas provas do Enem.</li>
				<li>Pratique.</li>
				<li>Leia tudo que passar por seus olhos.</li>
				<li>Não desperdice o seu tempo.</li>
				<li>Refaça os exercícios dos exames anteriores.</li>
				<li>Use a Internet a favor dos seus estudos.</li>
				<li>Durma bem.</li>
				<li>Alimente-se bem.</li>
			</ul>
		</svelte:fragment>
	</AccordionItem>
	<AccordionItem>
		<svelte:fragment slot="lead"><img src="{pessoa}" class="w-8" alt="" srcset=""></svelte:fragment>
		<svelte:fragment slot="summary"><strong>Porque é tão importante estudar e fazer o Enem?</strong></svelte:fragment>
		<svelte:fragment slot="content">
			<p> o Exame Nacional do Ensino Médio (Enem) tem o objetivo de avaliar o desempenho do estudante ao fim da escolaridade básica. Podem participar do exame alunos que estão concluindo ou que já concluíram o ensino médio em anos anteriores.Além de viabilizar o ingresso em instituições públicas, o Enem garante a permanência de estudantes em universidades particulares, através de bolsas de estudo integrais e parciais do Programa Universidade para Todos (ProUni). Para participar do programa é preciso ter média de 450 pontos no último Enem.</p>
		</svelte:fragment>
	</AccordionItem>
	<AccordionItem>
		<svelte:fragment slot="lead"><img src="{pergunta}" class="w-8" alt="" srcset=""></svelte:fragment>
		<svelte:fragment slot="summary"><strong>Tirei uma nota ruim, e agora o que fazer?</strong></svelte:fragment>
		<svelte:fragment slot="content">
			<p>Se com a sua atual nota você não conseguiu alcançar o que queria então estude mais caso queira fazer sua faculdade dos sonhos, principalmente se você busca fazer Direito, Medicina, Engenharia da Computação e etc..., pois são áreas meio complicadas de se entrar então se esforce bastante, garanto que tudo parece mais fácil quando se tem determinação.</p>
		</svelte:fragment>
	</AccordionItem>
	<AccordionItem>
		<svelte:fragment slot="lead"><img src="{problema}" class="w-8" alt="" srcset=""></svelte:fragment>
		<svelte:fragment slot="summary"><strong>Qual é o melhor jeito de estudar para o Enem?</strong></svelte:fragment>
		<svelte:fragment slot="content">
			<p>O melhor método de estudo para o Enem é criar um cronograma de estudos eficiente. Para isso, você deve estabelecer metas diárias, semanais e mensais para garantir que estará preparado para o dia da prova. Mas antes mesmo de criar o cronograma, é preciso levar em consideração o tempo disponível para estudo.</p>
		</svelte:fragment>
	</AccordionItem>
	<!-- ... -->
</Accordion>

</div>
</main>
	<slot />
	<svelte:fragment slot="pageFooter"><div class="mt-24"><h3 class="pl-4 mb-3">Após essa nota paia você pretende estudar: </h3>
	<TabGroup 
	justify="justify-center"
	active="variant-filled-primary"
	hover="hover:bg-primary-500"
	flex="flex-1 lg:flex-none"
	rounded=""
	border=""
	class="variant-ghost-surface w-full"
>
	<TabAnchor href="https://guiadoestudante.abril.com.br/enem/enem-o-que-mais-cai-em-matematica-e-dicas-de-como-estudar" target="_blank" class="border-2 border-primary-900" selected={$page.url.pathname === '/'}>
		<svelte:fragment slot="lead"><img src="{math}" class="w-9 m-auto" alt="matematica" srcset=""></svelte:fragment>
		<span class="text-center">Matemática</span>
	</TabAnchor>
	<TabAnchor href="https://www.todamateria.com.br/portugues-enem/"  target="_blank" class="border-2 border-primary-900" selected={$page.url.pathname === '/'}>
		<svelte:fragment slot="lead"><img src="{abc}" class="w-9 m-auto" alt="matematica" srcset=""></svelte:fragment>
		<span class="text-center">Português</span>
	</TabAnchor>
	<TabAnchor href="https://www.em.com.br/app/noticia/educacao/2023/10/06/internas_educacao,1572696/plano-de-estudos-7-macetes-para-tirar-uma-nota-alta-na-redacao-do-enem.shtml" target="_blank" class="border-2 border-primary-900" selected={$page.url.pathname === '/'}>
		<svelte:fragment slot="lead"><img src="{redacao}" class="w-9 m-auto" alt="matematica" srcset=""></svelte:fragment>
		<span class="text-center">Redação</span>
	</TabAnchor>
	<!-- <TabAnchor href="/" class="border-2 border-primary-900" selected={$page.url.pathname === '/'}>
		<svelte:fragment slot="lead"><img src="{fisica}" class="w-9 m-auto" alt="matematica" srcset=""></svelte:fragment>
		<span class="text-center">Física</span>
	</TabAnchor>
	<TabAnchor href="/" class="border-2 border-primary-900" selected={$page.url.pathname === '/'}>
		<svelte:fragment slot="lead"><img src="{quimica}" class="w-9 m-auto" alt="matematica" srcset=""></svelte:fragment>
		<span class="text-center">Quimíca</span>
	</TabAnchor>
	<TabAnchor href="/" class="border-2 border-primary-900" selected={$page.url.pathname === '/'}>
		<svelte:fragment slot="lead"><img src="{biologia}" class="w-9 m-auto" alt="matematica" srcset=""></svelte:fragment>
		<span class="text-center">Biologia</span>
	</TabAnchor> -->
</TabGroup>
</div>
</svelte:fragment>
</AppShell>
</body>
