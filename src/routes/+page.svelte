<script lang="ts">
	const OYA = '오야';
	const KO = '코';
	const TSUMO = '쯔모';
	const RON = '론';

	function createSet() {
		let fu;
		const fur = Math.random();
		if (fur <= 0.4) {
			fu = 30;
		} else if (fur <= 0.8) {
			fu = 40;
		} else if (fur <= 0.9) {
			fu = 20;
		} else {
			fu = Math.floor(Math.random() * 8 + 5) * 10;
		}

		let han;
		const hanr = Math.random();
		if (hanr < 0.05) {
			han = 13;
		} else if (hanr < 0.1) {
			han = Math.floor(Math.random() * 2) + 11;
		} else if (hanr < 0.15) {
			han = Math.floor(Math.random() * 3) + 8;
		} else if (hanr < 0.3) {
			han = Math.floor(Math.random() * 2) + 6;
		} else if (hanr < 0.5) {
			han = 5;
		} else {
			han = Math.floor(Math.random() * 4) + 1;
		}

		let oya = Math.random() < 0.25 ? OYA : KO;

		let ron = Math.random() < 0.33 ? TSUMO : RON;

		return [fu, han, oya, ron];
	}

	let [fu, han, oya, agari] = createSet();

	let userInput: string;

	function ok() {
		const score = getScore(fu, han, oya, agari);
		records = [
			{
				fu,
				han,
				oya,
				agari,
				score,
				correct: userInput === score
			},
			...records
		];

		if (userInput === score) corrects++;
		total++;

		[fu, han, oya, agari] = createSet();
		userInput = '';
	}

	let records = [];

	function getScore(f, h, o, a) {
		let base;
		if (h <= 4) {
			base = f * Math.pow(2, 2 + h);
			if (base > 2000) {
				base = 2000;
			}
		} else if (h >= 13) {
			base = 8000;
		} else if (h >= 11) {
			base = 6000;
		} else if (h >= 8) {
			base = 4000;
		} else if (h >= 6) {
			base = 3000;
		} else {
			base = 2000;
		}

		if (o === OYA && a === TSUMO) {
			const score = Math.ceil((base * 2) / 100) * 100;
			return `${score} all`;
		} else if (o === OYA && a === RON) {
			const score = Math.ceil((base * 6) / 100) * 100;
			return `${score}`;
		} else if (o === KO && a === TSUMO) {
			const oya = Math.ceil((base * 2) / 100) * 100;
			const ko = Math.ceil(base / 100) * 100;
			return `${ko}/${oya}`;
		} else if (o === KO && a === RON) {
			const score = Math.ceil((base * 4) / 100) * 100;
			return `${score}`;
		}
	}

	let total = 0;
	let corrects = 0;
</script>

<div class="main-container">
	<div class="container">
		<div>{han}판 {fu}부 {oya} {agari}</div>
		<form on:submit|preventDefault>
			<input type="text" bind:value={userInput} placeholder="4000/6000" />
			<button on:click={ok}>OK</button>
		</form>
		<div class="record">
			{#each records as record}
				<div class:correct={record.correct} class:incorrect={!record.correct}>
					{record.han}판 {record.fu}부 {record.oya}
					{record.agari}: {record.score}
				</div>
			{/each}
		</div>
		<div>정답률: {Math.round((corrects / total) * 10000) / 100}%</div>
	</div>
</div>

<style>
	.main-container {
		min-height: 100vh;
		min-height: 100dvh;
	}

	.main-container {
		display: flex;
		align-items: center;
	}

	.container {
		text-align: center;
		margin: auto;
	}

	.container div,
	.container form {
		margin-bottom: 10px;
	}

	input,
	button {
		border: 1px solid #ccc;
		padding: 8px;
		border-radius: 8px;
		background-color: #fff;
		transition: ease-out 0.05s background-color;
	}

	button:hover {
		background-color: #eee;
	}

	button:active {
		background-color: #ccc;
	}

	.record {
		font-size: 8px;
		height: 100px;
		border: 1px solid #ccc;
		border-radius: 8px;
		overflow-y: auto;
	}

	.correct {
		color: green;
	}

	.incorrect {
		color: red;
	}
</style>
