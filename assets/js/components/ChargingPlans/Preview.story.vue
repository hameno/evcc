<script setup>
import Preview from "./Preview.vue";

const now = new Date();

function createDate(hoursFromNow) {
	const result = new Date(now.getTime());
	result.setHours(result.getHours() + hoursFromNow);
	return result;
}

function createRate(price, hoursFromNow, durationHours = 1) {
	const start = new Date(now.getTime());
	start.setHours(start.getHours() + hoursFromNow);
	start.setMinutes(0);
	start.setSeconds(0);
	start.setMilliseconds(0);
	const end = new Date(start.getTime());
	end.setHours(start.getHours() + durationHours);
	end.setMinutes(0);
	end.setSeconds(0);
	end.setMilliseconds(0);
	return { start: start.toISOString(), end: end.toISOString(), price };
}

const co2 = {
	rates: [
		545, 518, 545, 518, 0, 545, 527, 527, 536, 518, 400, 336, 336, 339, 344, 336, 336, 336, 372,
		400, 555, 555, 545, 555, 564, 545, 555, 545, 536, 545, 527, 536, 518, 545, 509, 336, 336,
		336,
	].map((price, i) => createRate(price, i)),
	duration: 8695,
	plan: [createRate(213, 4), createRate(336, 11), createRate(336, 12)],
	smartCostType: "co2",
	targetTime: createDate(14),
};

const fixed = {
	rates: [createRate(0.442, 0, 50)],
	duration: 8695,
	plan: [createRate(0.442, 12, 3)],
	smartCostType: "price",
	currency: "EUR",
	targetTime: createDate(14),
};

const zoned = {
	rates: [
		createRate(3.72, 0, 4),
		createRate(2.39, 4, 12),
		createRate(3.72, 16, 12),
		createRate(2.39, 28, 12),
		createRate(3.72, 40, 12),
	],
	duration: 8695,
	plan: [createRate(2.39, 13, 3)],
	smartCostType: "price",
	currency: "DKK",
	targetTime: createDate(17),
};

const unknown = {
	rates: co2.rates.slice(0, 16),
	duration: 8695,
	plan: [createRate(213, 4), createRate(336, 11), createRate(336, 12)],
	smartCostType: "co2",
	targetTime: createDate(14),
};

const dynamic = {
	rates: [
		0.12, 0.15, 0, -0.05, -0.11, -0.24, -0.08, 0.12, 0.25, 0.29, 0.22, 0.31, 0.31, 0.33,
	].map((price, i) => createRate(price, i)),
	duration: 8695,
	plan: [createRate(0.23, 2, 5)],
	smartCostType: "price",
	currency: "EUR",
	targetTime: createDate(13),
};
</script>

<template>
	<Story title="ChargingPlanPreview">
		<Variant title="co2">
			<Preview v-bind="co2" />
		</Variant>
		<Variant title="fixed">
			<Preview v-bind="fixed" />
		</Variant>
		<Variant title="zoned">
			<Preview v-bind="zoned" />
		</Variant>
		<Variant title="unknown">
			<Preview v-bind="unknown" />
		</Variant>
		<Variant title="dynamic">
			<Preview v-bind="dynamic" />
		</Variant>
	</Story>
</template>
