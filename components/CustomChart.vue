<template>
    <div :id="props.id"></div>
</template>

<script setup>

const { $apexchart } = useNuxtApp();

const props = defineProps({
options: {
    type: Object,
    required: true,
},
id: {
    type: String,
    required: true
}
});

let chart = reactive({});

let selector = "#" + props.id;

const renderChart = (options) => {
if (chart && chart.destroy) {
    chart.destroy();
}

chart = new $apexchart(document.querySelector(selector), options);

chart.render();
}


watch(() => props.options, (newOptions) => {
renderChart(newOptions);
});

onUnmounted(() => { chart ? chart.destroy() : ""; });

</script>