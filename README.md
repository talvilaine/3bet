<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Калькулятор букмекерской маржи</title>
    <link href="https://3bet.pro/resources/css/bootstrap.min.css" rel="stylesheet">
  </head>
  <body>
	<div class="container" id="app3bet">
		<div class="page-header">
			<h1>Калькулятор букмекерской маржи</h1>
		</div>
		<p>
			<toggle-variants :values="resultVariants" :selected="resultsCount" @update:selected="resultsCount = $event" :def="resultsCountDef"></toggle-variants>
		</p>
		<div class="table-responsive">
			<table class="table table-bordered">
				<thead>
					<tr>
						<td></td>
						<th>Исход 1</th>
						<th>Исход 2</th>
						<th v-if="resultsCount > 2">Исход 3</th>
					</tr>
				</thead>
				<tbody>
					<tr>
						<th scope="row">Коэффициенты</th>
						<!--<div v-for="n in resultsCount">
							<input v-model="koef[n]">
						</div>-->

						<td>
							<label><input type="text" name="koef1" class="form-control" value="1,926"></label>
						</td>
						<td>
							<label><input type="text" name="koef2" class="form-control" value="2,02"></label>
						</td>
						<td v-if="resultsCount > 2">
							<label><input type="text" name="koef3" class="form-control" value="2,65"></label>
						</td>
					</tr>
					<tr>
						<th scope="row">Маржа БК</th>
						<td v-if="resultsCount > 2" :colspan="resultsCount" class="text-center">
							(1/koef1+1/koef2+1/koef3)-1
						</td>
						<td v-else :colspan="resultsCount" class="text-center">
							(1/koef1+1/koef2)-1
						</td>
					</tr>
					<tr>
						<th scope="row">Вероятность наступления исхода</th>
						<template v-if="resultsCount > 2">
							<td>
								C33 = 1/koef1/(1/koef1+1/koef2+1/koef3)
							</td>
							<td>
								D33 = 1/koef2/(1/koef1+1/koef2+1/koef3)
							</td>
							<td>
								E31 = 1/koef3/(1/koef1+1/koef2+1/koef3)
							</td>
						</template>
						<template v-else>
							<td>
								C33 = 1/koef1/(1/koef1+1/koef2)
							</td>
							<td>
								D33 = 1/koef2/(1/koef1+1/koef2)
							</td>
						</template>
					</tr>
					<tr>
						<th scope="row">Вероятно с учётом маржи</th>
						<td>
							1/koef1
						</td>
						<td>
							1/koef2
						</td>
						<td v-if="resultsCount > 2">
							1/koef3
						</td>
					</tr>
					<tr>
						<th scope="row">Реальные коэффициенты</th>
						<td>
							1/C33
						</td>
						<td>
							1/D33
						</td>
						<td v-if="resultsCount > 2">
							1/E31
						</td>
					</tr>
				</tbody>
			</table>

		</div>
	</div>
    <script src="https://3bet.pro/resources/lib/jquery/jquery.min.js"></script>
    <script src="https://3bet.pro/resources/lib/bootstrap/bootstrap.min.js"></script>
	<script src="https://unpkg.com/vue"></script>
	<script type="text/x-template" id="toggle-variants-tpl">
		<div class="btn-group">
			<button type="button"
				v-for="(key, val) in values"
				@click="changeSelectVal(val)"
				:class="[ 'btn', selected == val ? 'btn-success' : 'btn-default' ]"
			>{{ key }}</button>
		</div>
	</script>
  	<script>
		Vue.component('toggle-variants', {
			props: [ 'values', 'selected', 'def' ],
			template: '#toggle-variants-tpl',
			ready: function (){
				this.selected = this.def;
			},
			methods: {
				changeSelectVal: function(val){
					this.$emit('update:selected', val)
				}
			}
		});

		var app3bet = new Vue({
			el: '#app3bet',
			data: {
				resultVariants: { 2 : 'Два исхода', 3 : 'Три исхода' },
				resultsCountDef: 2,
				resultsCount: 2,
				results: {

				},
				koef: []
			},
			computed: {
			}
		})
	</script>
  </body>
</html>


