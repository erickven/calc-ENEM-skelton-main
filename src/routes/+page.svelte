<script lang="ts">
	import { data, type Data } from './data';
	import logo from '$lib/icons8-github-50.png';
	import calculadora from '$lib/icons8-calculadora-de-maçã-50 (1).png';
	import math from '$lib/icons8-matemática-50.png';
	import abc from '$lib/icons8-abc-30.png';
	import redacao from '$lib/icons8-documento-64.png';
	import question from '$lib/icons8-question-64.png';
	import problema from '$lib/icons8-problem-50.png';
	import fisica from '$lib/icons8-física-50.png';
	import quimica from '$lib/icons8-sódio-50.png';
	import duvida from '$lib/icons8-decisão-50 (1).png';
	import biologia from '$lib/icons8-pegada-de-cachorro-30.png';
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
	import pergunta from '$lib/icons8-pergunta-30.png';
	import pessoa from '$lib/icons8-pele-tipo-3-em-dúvida-48.png';
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
			return total + subject.peso * subject.nota;
		}, 0);
	}

	function calcularSomaDosPesos(arr: Data[]) {
		return arr.reduce((sum, subject) => {
			return Math.round(sum + subject.peso);
		}, 0);
	}

	function calcularScoreFinal(arr: Data[]) {
		return calcularScore(arr) / calcularSomaDosPesos(arr);
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
			font-family: 'Merriweather', serif;
			font-weight: 300;
			font-style: normal;
		}

		.merriweather-regular {
			font-family: 'Merriweather', serif;
			font-weight: 400;
			font-style: normal;
		}

		.merriweather-bold {
			font-family: 'Merriweather', serif;
			font-weight: 700;
			font-style: normal;
		}

		.merriweather-black {
			font-family: 'Merriweather', serif;
			font-weight: 900;
			font-style: normal;
		}

		.merriweather-light-italic {
			font-family: 'Merriweather', serif;
			font-weight: 300;
			font-style: italic;
		}

		.merriweather-regular-italic {
			font-family: 'Merriweather', serif;
			font-weight: 400;
			font-style: italic;
		}

		.merriweather-bold-italic {
			font-family: 'Merriweather', serif;
			font-weight: 700;
			font-style: italic;
		}

		.merriweather-black-italic {
			font-family: 'Merriweather', serif;
			font-weight: 300;
			font-style: italic;
		}
	</style>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" />
	<link
		href="https://fonts.googleapis.com/css2?family=Merriweather:ital,wght@0,300;0,400;0,700;0,900;1,300;1,400;1,700;1,900&display=swap"
		rel="stylesheet"
	/>
</svelte:head>
<body
	class="merriweather-black bg-surface-50 dark:bg-gradient-to-r from-slate-900 to-surface-950 text-center text-surface-900 dark:text-tertiary-500 font-bold dark:font-normal text-sm md:max-2xl:text-lg flex-col space-between m-0 p-0 overflow-x-auto"
	data-theme="crimson"
>
	<meta name="viewport" content="width=device-width, initial-scale=1.0" />
	<AppShell>
		<svelte:fragment slot="header">
			<AppBar
				gridColumns="grid-cols-3"
				class="bg-surface-100 shadow-xl dark:bg-surface-800"
				slotDefault="place-self-center"
				slotTrail="place-content-end"
			>
				<svelte:fragment slot="lead"
					><button
						class="text-sm w-40 rounded-2xl border-surface-800 dark:border-tertiary-500 text-sm sm:max-lg:h-8 sm:py-2 border-2 bg-transparent text-surface-800 dark:text-tertiary-500"
						on:click={drawerOpen}><strong>Mostrar Resultados</strong></button
					></svelte:fragment
				>
				<span
					><h1 class="text-center inline-block text-lg sm:text-2xl px-4 font-serif">
						<strong>Calculadora do ENEM</strong>
					</h1></span
				>
				<svelte:fragment slot="trail"
					><div class="flex justify-center">
						<LightSwitch />
					</div>
					<span
						><div class="flex flex-row">
							<a
								href="https://github.com/erickven"
								target="_blank"
								class="bg-transparent hover:bg-primary-400 shadow-2xl btn-icon variant-filled w-10 sm:max-lg:h-8"
								><img class="w-8 flex" src={logo} alt="" srcset="" /></a
							>
						</div></span
					>
				</svelte:fragment>
			</AppBar>
		</svelte:fragment>
		<svelte:fragment slot="sidebarRight">
			<Drawer>
				<div id="sidebar-right" class="">
					<aside>
						<div
							id="resultado"
							class="w-full h-full variant-ghost-surface mr-0 pt-3 text-center text-surface-900 dark:text-tertiary-300 rounded-xl text-sm"
						>
							<h1 class="text-center mb-2"><strong>De acordo com a sua nota: </strong></h1>
							<h3>Com base nos dados entre 2020 e 2023</h3>
							{#each mathGrade as m}
								<p>Caso você tivesse acertado {m[0]} questões em matemática sua nota seria:</p>
								Entre {calcularSimulation(arr, m[1]).toFixed(2)} e {calcularSimulation(
									arr,
									m[3]
								).toFixed(2)}
								<br />
								com média de {calcularSimulation(arr, m[2]).toFixed(2)}
								<ProgressBar />
							{/each}
							<hr />
							<p
								class="text-center text-lg py-3 my-3 bg-primary-700 dark:bg-tertiary-500 text-tertiary-50 dark:text-tertiary-900"
							>
								<strong
									>Nunca fez, quer fazer o enem esse ano ou não sabe se a universidade publica do
									seu estado aceita enem?</strong
								>
							</p>
							<Navigation />
					</div></aside>
				</div>
				<h3 class="my-10 h3 font-bold underline underline-offset-8">
					Caso queira praticar aqui estão alguns simulados para você resolver.
				</h3>
				<div class="text-left ml-6 md:mx-40 my-16">
					<h5 class="underline underline-offset-4 font-bold text-center mb-3">Questão 1</h5>
					<h5 class="">
						Um construtor precisa revestir o piso de uma sala retangular. Para essa tarefa, ele
						dispõe de dois tipos de cerâmicas:
						<br />
						a) cerâmica em forma de quadrado de lado 20 cm, que custa R$ 8,00 por unidade;
						<br />
						b) cerâmica em forma de triângulo retângulo isósceles de catetos com 20 cm, que custa R$
						6,00 por unidade.
						<br />
						A sala tem largura de 5 m e comprimento de 6 m.
						<br />
						O construtor deseja gastar a menor quantia possível com a compra de cerâmica. Sejam x o número
						de peças de cerâmica de forma quadrada e y o número de peças de cerâmica de forma triangular.
						<br />
						Isso significa, então, encontrar valores para x e y tais que 0,04x + 0,02y > 30 e que tornem
						o menor possível valor de
					</h5>
					<br />
					<ol class="my-4">
						<li>a)8x + 6y</li>
						<li>b)6x + 8y</li>
						<li>c)0,32x + 0,12y</li>
						<li>d)0,32x + 0,02y</li>
						<li>e)0,04x + 0,12y</li>
					</ol>
				</div>
				<div class="text-left ml-6 md:mx-40 my-16">
					<h5 class="font-bold text-center underline underline-offset-4 mb-3">Questão 2</h5>
					<h5 class="">
						Um grupo sanguíneo, ou tipo sanguíneo, baseia-se na presença ou ausência de dois
						antígenos, A e B, na superfície das células vermelhas do sangue. Como dois antígenos
						estão envolvidos, os quatro tipos sanguíneos distintos são: <br />
					</h5>
					<ol class="my-4">
						<li>• Tipo A: apenas o antígeno A está presente;</li>
						<li>• Tipo B: apenas o antígeno B está presente;</li>
						<li>• Tipo AB: ambos os antígenos estão presentes;</li>
						<li>• Tipo O: nenhum dos antígenos está presente.</li>
					</ol>
					<h5>
						Foram coletadas amostras de sangue de 200 pessoas e, após análise laboratorial, foi
						identificado que em 100 amostras está presente o antígeno A, em 110 amostras há presença
						do antígeno B e em 20 amostras nenhum dos antígenos está presente. Dessas pessoas que
						foram submetidas à coleta de sangue, o número das que possuem o tipo sanguíneo A é igual
						a:
					</h5>
					<ol class="my-4">
						<li>a)30</li>
						<li>b)60</li>
						<li>c)70</li>
						<li>d)90</li>
						<li>e)100</li>
					</ol>
				</div>
				<div class="text-left ml-6 md:mx-40 my-16">
					<h5 class="font-bold text-center underline underline-offset-4 mb-3">Questão 3</h5>
					<h5 class="">
						Um vaso decorativo quebrou e os donos vão encomendar outro para ser pintado com as
						mesmas características. Eles enviam uma foto do vaso na escala 1 : 5 (em relação ao
						objeto original) para um artista. Para ver melhor os detalhes do vaso o artista solicita
						uma cópia impressa da foto com dimensões triplicadas em relação às dimensões da foto
						original. Na cópia impressa, o vaso quebrado tem uma altura de 30 centímetros.
					</h5>
					<ol class="my-4">
						<li>a)2</li>
						<li>b)18</li>
						<li>c)56</li>
						<li>d)60</li>
						<li>e)90</li>
					</ol>
				</div>
				<div class="text-left ml-6 md:mx-40 my-16">
					<h5 class="font-bold text-center underline underline-offset-4 mb-3">Questão 4</h5>
					<h5 class="">
						Após o término das inscrições de um concurso, cujo número de vagas é fixo, foi divulgado
						que a razão entre o número de candidatos e o número de vagas, nesta ordem, era igual a
						300. Entretanto, as inscrições foram prorrogadas, inscrevendo-se mais 4 000 candidatos,
						fazendo com que a razão anteriormente referida passasse a ser igual a 400. Todos os
						candidatos inscritos fizeram a prova, e o total de candidatos aprovados foi igual à
						quantidade de vagas. Os demais candidatos foram reprovados.
					</h5>
					<h5>Nessas condições, quantos foram os candidatos reprovados?</h5>
					<ol class="my-4">
						<li>a)11 950</li>
						<li>b)11 970</li>
						<li>c)15 960</li>
						<li>d)15 970</li>
						<li>e)19 960</li>
					</ol>
				</div>
				<div class="text-left ml-6 md:mx-40 my-16">
					<h5 class="font-bold text-center underline underline-offset-4 mb-3">Questão 5</h5>
					<h5 class="">
						O Índice de Desenvolvimento Humano (IDH) mede a qualidade de vida dos países para além
						dos indicadores econômicos. O IDH do Brasil tem crescido ano a ano e atingiu os
						seguintes patamares: 0,600 em 1990; 0,665 em 2000; 0,715 em 2010. Quanto mais perto de
						1,00, maior é o desenvolvimento do país.
						<br />
						O Globo. Caderno Economia, 3 nov. 2011 (adaptado).
						<br />
						Observando o comportamento do IDH nos períodos citados, constata-se que, ao longo do período
						1990-2010, o IDH brasileiro
					</h5>
					<br />
					<ol class="my-4">
						<li>a)diminuiu com variações decenais crescentes.</li>
						<li>b)diminuiu em proporção direta com o tempo.</li>
						<li>c)aumentou com variações decenais decrescentes.</li>
						<li>d)aumentou em proporções direta com o tempo</li>
						<li>e)aumento em proporção inversa com o tempo</li>
					</ol>
				</div>
				<div class="text-left ml-6 md:mx-40 my-16">
					<h5 class="font-bold text-center underline underline-offset-4 mb-3">Questão 6</h5>
					<h5 class="">
						O prefeito de uma cidade deseja promover uma festa popular no parque municipal para
						comemorar o aniversário de fundação do município. Sabe-se que esse parque possui formato
						retangular, com 120 m de comprimento por 150 m de largura. Além disso, para segurança
						das pessoas presentes no local, a polícia recomenda que a densidade média, num evento
						dessa natureza, não supere quatro pessoas por metro quadrado.
						<br />
						Seguindo as recomendações de segurança estabelecidas pela polícia, qual é o número máximo
						de pessoas que poderão estar presentes na festa?
						<br />
						<ol class="my-4">
							<li>a)1.000</li>
							<li>b)4.500</li>
							<li>c)18.000</li>
							<li>d)72.000</li>
							<li>e)120.000</li>
						</ol>
					</h5>
				</div>
				<div class="text-left ml-6 md:mx-40 my-16">
					<h5 class="font-bold text-center underline underline-offset-4 mb-3">Questão 7</h5>
					<h5 class="">
						A probabilidade de um empregado permanecer em uma dada empresa particular por 10 anos ou
						mais é de 1/6.
						<br />
						Um homem e uma mulher começam a trabalhar nessa companhia no mesmo dia. Suponha que não haja
						nenhuma relação entre o trabalho dele e o dela, de modo que seus tempos de permanência na
						firma são independentes entre si.
						<br />
						A probabilidade de ambos, homem e mulher, permanecerem nessa empresa por menos de 10 anos
						é de:
						<br />
						<ol class="my-4">
							<li>a)60/36</li>
							<li>b)25/36</li>
							<li>c)24/36</li>
							<li>d)12/36</li>
							<li>e)1/36</li>
						</ol>
					</h5>
				</div>
				<div class="text-left ml-6 md:mx-40 my-16">
					<h5 class="font-bold text-center underline underline-offset-4 mb-3">Questão 8</h5>
					<h5 class="">
						Um técnico gráfico constrói uma nova folha a partir das medidas de uma folha A0. As
						medidas de uma folha A0 são 595 mm de largura e 840 mm de comprimento. <br /> A nova
						folha foi construída do seguinte modo: acrescenta uma polegada na medida da largura e 16
						polegadas na medida do comprimento. Esse técnico precisa saber a razão entre as medidas
						da largura e do comprimento, respectivamente, dessa nova folha. <br />Considere 2,5 cm
						como valor aproximado para uma polegada. <br /> Qual é a razão entre as medidas da
						largura e do comprimento da nova folha? <br />
					</h5>
					<ol class="my-4">
						<li>a)1/16</li>
						<li>b)620/1240</li>
						<li>c)596/650</li>
						<li>d)598/880</li>
						<li>e)845/4840</li>
					</ol>
				</div>
				<div class="text-left ml-6 md:mx-40 my-16">
					<h5 class="font-bold text-center underline underline-offset-4 mb-3">Questão 9</h5>
					<h5 class="">
						Na construção de um conjunto habitacional de casas populares, todas serão feitas num
						mesmo modelo, ocupando, cada uma delas, terrenos cujas dimensões são iguais a 20 m de
						comprimento por 8 m de largura. Visando a comercialização dessas casas, antes do início
						das obras, a empresa resolveu apresentá-las por meio de maquetes construídas numa escala
						de 1 : 200.
						<br />
						As medidas do comprimento e da largura dos terrenos, respectivamente, em centímetros, na
						maquete construída, foram de
					</h5>
					<ol class="my-4">
						<li>a)4 e 10.</li>
						<li>b)5 e 2.</li>
						<li>c)10 e 4.</li>
						<li>d)20 e 8</li>
						<li>e)50 e 20</li>
					</ol>
				</div>
				<div class="text-left ml-6 md:mx-40 my-16">
					<h5 class="font-bold text-center underline underline-offset-4 mb-3">Questão 10</h5>
					<h5 class="">
						Um confeiteiro deseja fazer um bolo cuja receita indica a utilização de açúcar e farinha
						de trigo em quantidades fornecidas em gramas. Ele sabe que uma determinada xícara
						utilizada para medir os ingredientes comporta 120 gramas de farinha de trigo e que três
						dessas xícaras de açúcar correspondem, em gramas, a quatro de farinha de trigo.
						<br />
						Quantos gramas de açúcar cabem em uma dessas xícaras?
					</h5>
					<ol class="my-4">
						<li>a)30</li>
						<li>b)40</li>
						<li>c)90</li>
						<li>d)160</li>
						<li>e)360</li>
					</ol>
				</div>
			</Drawer>
		</svelte:fragment>
		<main>
			<p class="py-3 px-4 mb-12 font-bold my-8">
				Esta calculadora serve para você calcular a sua nota obtida no ENEM e saber o quanto você teria tirado se tivesse acertado determinadas questões de matemática.
			</p>
			<div id="tabelas" class="sm:py-5 mb-12 inline-block">
				<table
					class="border-2 shadow-xl dark:shadow-sm border-surface-700 border-collapse m-auto relative"
				>
					<tr class="bg-primary-600 dark:bg-primary-800">
						<th>Prova do Enem</th><th>Nota mínima</th><th>Sua nota</th><th>Peso</th><th
							>Nota com peso</th
						>
					</tr>
					{#each arr as materia}
						<tr class="sm:max-xl:table-fixed text-center">
							<td class="">{materia.prova}</td>
							<td class=""
								><input
									type="number"
									class="w-16 rounded-lg pt-3 flex-col sm:m-1 bg-tertiary-800 dark:bg-surface-800 sm:w-32 md:w-44 text-center sm:max-xl:w-16"
									placeholder="0.01"
									bind:value={materia.minGrade}
								/></td
							>
							<td class=""
								><input
									type="number"
									class="w-16 rounded-lg pt-3 flex-col sm:m-1 bg-tertiary-800 dark:bg-surface-800 sm:w-32 md:w-44 text-center sm:max-xl:w-16"
									placeholder="780.00"
									bind:value={materia.nota}
								/></td
							>
							<td class=""
								><input
									type="number"
									class="w-16 rounded-lg pt-3 flex-col sm:m-1 bg-tertiary-800 dark:bg-surface-800 sm:w-32 md:w-44 text-center sm:max-xl:w-16"
									placeholder="1.50"
									bind:value={materia.peso}
								/></td
							>
							<td class="">{calcularNota(materia)}</td>
						</tr>
					{/each}

					<tr class="text-primary-700">
						<td /><td /><td>Total</td><td>Total</td>
					</tr>
					<tr>
						<td /><td /><td>{calcularScore(arr).toFixed(2)}(A)</td><td
							>{calcularSomaDosPesos(arr).toFixed(2)}(B)</td
						>
					</tr>
					<td><i>Nota do estudante (B/A) = {calcularScoreFinal(arr).toFixed(2)}</i></td>
				</table>
				<h3 class="h3 text-center font-bold mt-10 underline">Algumas Duvidas</h3>
				<Accordion
					class="mt-6 bg-surface-900 dark:bg-surface-900 text-tertiary-500 text-left shadow-xl rounded-2xl"
				>
					<AccordionItem>
						<svelte:fragment slot="lead"
							><img src={duvida} class="w-8" alt="" srcset="" /></svelte:fragment
						>
						<svelte:fragment slot="summary"
							><strong>Como se usa a calculadora?</strong></svelte:fragment
						>
						<svelte:fragment slot="content">
							<p>
								Para usar a calculadora você deve consultar suas notas no site do MEC, e inserir as
								suas notas no campo acima, ao final de tudo isso vá no canto superior esquerdo da
								tela onde tem um botão escrito "Mostrar Resultados", ao apertar nele lá estará o
								resultado da sua nota caso você tivesse acertado determinadas questões de matemática
								na prova.
							</p>
						</svelte:fragment>
					</AccordionItem>
					<AccordionItem>
						<svelte:fragment slot="lead"
							><img src={question} class="w-8" alt="" srcset="" /></svelte:fragment
						>
						<svelte:fragment slot="summary"
							><strong>Oque fazer para tirar uma boa nota no Enem?</strong></svelte:fragment
						>
						<svelte:fragment slot="content">
							<p class="font-bold pl-2">Algumas dicas valiosas:</p>
							<ul class="list-decimal pl-6">
								<li>
									Não perca o foco, procure orientação e estude com mais afinco as matérias que
									realmente têm peso nas provas do Enem.
								</li>
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
						<svelte:fragment slot="lead"
							><img src={pessoa} class="w-8" alt="" srcset="" /></svelte:fragment
						>
						<svelte:fragment slot="summary"
							><strong>Porque é tão importante estudar e fazer o Enem?</strong></svelte:fragment
						>
						<svelte:fragment slot="content">
							<p>
								o Exame Nacional do Ensino Médio (Enem) tem o objetivo de avaliar o desempenho do
								estudante ao fim da escolaridade básica. Podem participar do exame alunos que estão
								concluindo ou que já concluíram o ensino médio em anos anteriores.Além de viabilizar
								o ingresso em instituições públicas, o Enem garante a permanência de estudantes em
								universidades particulares, através de bolsas de estudo integrais e parciais do
								Programa Universidade para Todos (ProUni). Para participar do programa é preciso ter
								média de 450 pontos no último Enem.
							</p>
						</svelte:fragment>
					</AccordionItem>
					<AccordionItem>
						<svelte:fragment slot="lead"
							><img src={pergunta} class="w-8" alt="" srcset="" /></svelte:fragment
						>
						<svelte:fragment slot="summary"
							><strong>Tirei uma nota ruim, e agora o que fazer?</strong></svelte:fragment
						>
						<svelte:fragment slot="content">
							<p>
								Se com a sua atual nota você não conseguiu alcançar o que queria então estude mais
								caso queira fazer sua faculdade dos sonhos, principalmente se você busca fazer
								Direito, Medicina, Engenharia da Computação e etc..., pois são áreas meio
								complicadas de se entrar então se esforce bastante, garanto que tudo parece mais
								fácil quando se tem determinação.
							</p>
						</svelte:fragment>
					</AccordionItem>
					<AccordionItem>
						<svelte:fragment slot="lead"
							><img src={problema} class="w-8" alt="" srcset="" /></svelte:fragment
						>
						<svelte:fragment slot="summary"
							><strong>Qual é o melhor jeito de estudar para o Enem?</strong></svelte:fragment
						>
						<svelte:fragment slot="content">
							<p>
								O melhor método de estudo para o Enem é criar um cronograma de estudos eficiente.
								Para isso, você deve estabelecer metas diárias, semanais e mensais para garantir que
								estará preparado para o dia da prova. Mas antes mesmo de criar o cronograma, é
								preciso levar em consideração o tempo disponível para estudo.
							</p>
						</svelte:fragment>
					</AccordionItem>
					<!-- ... -->
				</Accordion>
		</main>
		<slot />
		<svelte:fragment slot="pageFooter"
			><div class="mt-24">
				<h3 class="pl-4 mb-3">Não está satisfeito com a sua nota?aqui estão alguns links para você poder estudar um pouco mais:</h3>
				<TabGroup
					justify="justify-center"
					active="variant-filled-primary"
					hover="hover:bg-primary-500"
					flex="flex-1 lg:flex-none"
					rounded=""
					border=""
					class="variant-ghost-surface w-full"
				>
					<TabAnchor
						href="https://guiadoestudante.abril.com.br/enem/enem-o-que-mais-cai-em-matematica-e-dicas-de-como-estudar"
						target="_blank"
						class="border-2 border-primary-900"
						selected={$page.url.pathname === '/'}
					>
						<svelte:fragment slot="lead"
							><img src={math} class="w-9 m-auto" alt="matematica" srcset="" /></svelte:fragment
						>
						<span class="text-center">Matemática</span>
					</TabAnchor>
					<TabAnchor
						href="https://www.todamateria.com.br/portugues-enem/"
						target="_blank"
						class="border-2 border-primary-900"
						selected={$page.url.pathname === '/'}
					>
						<svelte:fragment slot="lead"
							><img src={abc} class="w-9 m-auto" alt="matematica" srcset="" /></svelte:fragment
						>
						<span class="text-center">Português</span>
					</TabAnchor>
					<TabAnchor
						href="https://www.em.com.br/app/noticia/educacao/2023/10/06/internas_educacao,1572696/plano-de-estudos-7-macetes-para-tirar-uma-nota-alta-na-redacao-do-enem.shtml"
						target="_blank"
						class="border-2 border-primary-900"
						selected={$page.url.pathname === '/'}
					>
						<svelte:fragment slot="lead"
							><img src={redacao} class="w-9 m-auto" alt="matematica" srcset="" /></svelte:fragment
						>
						<span class="text-center">Redação</span>
					</TabAnchor>
				</TabGroup>
			</div>
		</svelte:fragment>
	</AppShell>
</body>
