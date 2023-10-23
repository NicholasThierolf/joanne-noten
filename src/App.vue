<script setup>
	import { reactive } from "vue";

	const lookup = [
		{
			from: 0,
			upto: 0.01,
			grade: "1",
		},
		{
			from: 0.02,
			upto: 0.05,
			grade: "1-",
		},
		{
			from: 0.06,
			upto: 0.09,
			grade: "2+",
		},
		{
			from: 0.1,
			upto: 0.16,
			grade: "2",
		},
		{
			from: 0.17,
			upto: 0.2,
			grade: "2-",
		},
		{
			from: 0.21,
			upto: 0.24,
			grade: "3+",
		},
		{
			from: 0.25,
			upto: 0.31,
			grade: "3",
		},
		{
			from: 0.32,
			upto: 0.35,
			grade: "3-",
		},
		{
			from: 0.36,
			upto: 0.39,
			grade: "4+",
		},
		{
			from: 0.4,
			upto: 0.46,
			grade: "4",
		},
		{
			from: 0.47,
			upto: 0.5,
			grade: "4-",
		},
		{
			from: 0.51,
			upto: 0.56,
			grade: "5+",
		},
		{
			from: 0.57,
			upto: 0.69,
			grade: "5",
		},
		{
			from: 0.7,
			upto: 0.75,
			grade: "5-",
		},
		{
			from: 0.76,
			upto: 1,
			grade: "6",
		},
	];

	function calculateRows() {
		let lastMistakes = -0.5;
		let returnMap = lookup.map((row) => {
			let low = lastMistakes + 0.5;
			let high =
				Math.floor(
					((row.upto * state.textLength * 100) / 100) * 2 + 0.09
				) / 2;
			lastMistakes = high;
			if (row.upto == "∞") high = "∞";
			return {
				from: row.from,
				upto: row.upto,
				grade: row.grade,
				low: low,
				high: high,
			};
		});
		console.log(returnMap);
		return returnMap;
	}

	const state = reactive({
		textLength: 100,
		rows: [],
	});

	state.rows = calculateRows();
</script>

<template>
	<main>
		<h1>Notenrechner</h1>
		<label>
			<span>Gesamtpunktzahl:</span>
			<input
				type="number"
				v-model="state.textLength"
				@keyup="state.rows = calculateRows()"
				@change="state.rows = calculateRows()"
			/>
		</label>

		<table>
			<tr>
				<th>Prozent</th>
				<th>Note</th>
				<th>Punkte</th>
			</tr>
			<tr v-for="row in state.rows">
				<td>
					{{ Math.round((1 - row.from) * 100) }}% -
					{{ Math.round((1 - row.upto) * 100) }}%
				</td>
				<td>{{ row.grade }}</td>
				<td v-if="row.low == row.high">
					{{ state.textLength - row.low }}
				</td>
				<td v-else>
					{{ state.textLength - row.low }} -
					{{ state.textLength - row.high }}
				</td>
			</tr>
		</table>
	</main>
</template>

<style scoped lang="scss">
	table {
		border: solid 1px var(--vt-c-text-dark-2);
		border-spacing: 0;
		border-collapse: collapse;
		td,
		th {
			border: solid 1px var(--vt-c-text-dark-2);
			padding: 0 20px;
			text-align: left;
		}
		th {
			font-weight: bold;
		}
	}

	h1 {
		font-weight: bold;
		font-size: 32px;
	}

	label {
		display: flex;
		flex-direction: column;
		margin-bottom: 30px;
		span {
			font-size: 22px;
		}
		input {
			background-color: transparent;
			color: var(--color-text);
			border: none;
			border-bottom: solid 1px gray;
			width: 100px;
			font-size: 16px;
		}
	}
</style>

<style>
	#app {
		display: flex;
		justify-content: center;
	}
</style>
