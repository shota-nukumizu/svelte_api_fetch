# Svelteでfetch APIを使ったAPIデータ表示(Svelte✕JavaScript)

# 実装方法

`src/App.svelte`

```svelte
<script>
	import { onMount } from 'svelte'
	import { apiData, drinkNames } from './store.js'

	onMount(async () => {
		fetch('https://www.thecocktaildb.com/api/json/v1/1/filter.php?i=Bourbon')
		.then(response => response.json())
		.then(data => {
			apiData.set(data)
		}).catch(error => {
			console.log(error)
			return []
		})
	})
</script>

<main>
	<h1>Whiskey Drinks Menu</h1>
	<ul>
	{#each $drinkNames as drinkName}
		<li>{drinkName}</li>
	{/each}
	</ul>
</main>

<style>
</style>
```

`src/store.js`

```js
import { writable, derived } from 'svelte/store'

// APIデータを格納する場所
export const apiData = writable([])

// APIデータを表示する。データがない場合は空リストを出力する
export const drinkNames = derived(apiData, ($apiData) => {
    if ($apiData.drinks) {
        return $apiData.drinks.map(drink => drink.strDrink)
    }
    return []
})
```

# 開発環境

* Visual Studio Code
* Svelte
* JavaScript