<template>
	<div>
		<a @click="visible" title="Voir les résultats">Voir les résultats ( {{ total }} votes )</a>
		<div class="container-result" v-show="show">
			<div v-for="(data, index) in datas">
				<span>{{ index }}</span>
				<div class="result" :data-result="data" v-percentage="{data, total}"></div>
			</div>
		</div>
	</div>
</template>

<script>
import Vue from 'vue'
// Calcul les résultats en pourcentage pour le sondage en cours
Vue.directive('percentage', {
	update: function (el, binding) {
		// On détermine le pourcentage 
		let percentage =  ((binding.value.data * 100 ) / binding.value.total)
		// Avec le pourcentage, on va définir la propriété 'width' de l'élément 'div'
		el.style.width = Math.round(percentage) + '%'
		// On inscrit le pourcentage de la réponse
		el.innerHTML = Math.round(percentage) + '%'
	}
})
export default {
	name: 'Results',
	props: {
		datas: Object,
		total: Number
	},
	data () {
		return {
			show: false
		}
	},
	methods: {
		// Affiche ou masque les résultats du sondage
		visible () {
			this.show = this.show == true ? false : true
		}
	}
}
</script>

<style>
	span {
		font-weight: bold;
	}
	.container-result {
		margin-top: 10px;
	}
	.result {
		transition: width 2s;
		border: 1px solid black;
		margin-bottom: 10px;
		height: 30px;
		display: flex;
		justify-content: center;
		align-items: center;
		align-content: center;
		color: #FFF;
		background-color: #000;
		font-size: 11px;
		font-weight: bold;
	}
	

</style>
