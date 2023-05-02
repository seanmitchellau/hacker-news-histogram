<script lang="ts">
import {
    Chart as ChartJS,
    Title,
    Tooltip,
    BarElement,
    CategoryScale,
    LinearScale
} from 'chart.js'
import {Bar} from 'vue-chartjs'

// https://github.com/apertureless/vue-chartjs
ChartJS.register(CategoryScale, LinearScale, BarElement, Title, Tooltip)

export default {
    name: 'App',
    props: {
        chartData: {
            type: Object,
            default: null
        },
    },
    components: {
        Bar
    },
    data() {
        const formatHour = (hour) => {
            const period = hour < 12 ? 'am' : 'pm';
            const formattedHour = hour % 12 || 12;
            return `${formattedHour}${period}`; // make hour index a bit more readable
        };

        return {
            data: {
                labels: Array.from(this.chartData.keys()).map(formatHour),
                datasets: [{data: this.chartData, backgroundColor: '#7c7474', borderWidth: 2}]
            },
            options: {
                responsive: true,
                scales: {
                    y: {
                        ticks: {
                            stepSize: 1,
                        },
                    },
                },
            },
        }
    }
}
</script>

<template>
    <div class="chart">
        <Bar :data="data" :options="options"/>
    </div>
</template>

<style scoped lang="scss">
.chart {
  margin-top: 32px;
}
</style>