<template>
	<div class="container">
		<span><b>Парсер JS и CSS</b></span>
		<div>
			<input class="url" ref="url" value="https://yandex.ru" type="text" />
			<input class="url-button" v-on:click="send" type="submit" />
		</div>
		<span>Styles</span>
		<ul>
			<li v-for="style in styles" :key="style">
				{{ style }}
			</li>
		</ul>
		<span>Scripts</span>
		<ul>
			<li v-for="script in scripts" :key="script">
				{{ script }}
			</li>
		</ul>
	</div>
</template>

<script>
export default {
	name: 'ParserComponent',
	props: {},
	data: () => {
		return {
			styles: [],
			scripts: [],
		}
	},
	methods: {
		send() {
			console.log('ASD')
			let url = this.$refs.url.value
			if (url.trim() == '') {
				alert('Вы не ввели ссылку')
				return
			}
			fetch(`http://241a3a7ab2d9.vps.myjino.ru:49162/get-files?url=${url}`)
				.then(async d => {
					if (d.status != 200) {
						throw new Error('Запрашиваемый ресурс недоступен')
					}
					let json = await d.json()
					this.styles = json.styles
					this.scripts = json.scripts
				})
				.catch(e => {
					alert(e.message)
				})
		},
	},
}
</script>

<style scoped>
* {
	margin: 0;
	padding: 0;
}
.container {
	display: flex;
	flex-direction: column;
	gap: 10px;
}
.url {
	width: 295px;
	padding: 5px;
	margin-right: 5px;
}
.url-button {
	width: 100px;
	padding: 5px;
}
ul {
	border: 1px solid black;
	border-radius: 3px;
	width: 390px;
	padding: 10px;
	margin: auto;
}
ul > li {
	list-style-type: '';
	padding: 5px;
	margin-bottom: 5px;
	border-radius: 3px;
	background-color: antiquewhite;
	text-align: left;
}
</style>
