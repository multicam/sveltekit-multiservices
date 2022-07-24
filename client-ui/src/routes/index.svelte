<script context="module">
  import {fetchProducts, pingService} from '$lib/inventory.js'

  export async function load() {
    const products = await pingService()
    return { props: { initialProducts: products } }
  }
</script>

<script>
  import Modal from '$components/Modal.svelte'
  import AddPostForm from '$components/AddPostForm.svelte'
  import ProductsTable from '$components/ProductsTable.svelte'
  import { isModalOpen, mutated } from '$lib/stores.js'
  import { onMount } from 'svelte'

  export let initialProducts
  let products

  onMount(() => (products = initialProducts))

  $: {
    ;(async () => {
      if ($mutated) {
        products = await pingService()
        $mutated = false
      }
    })()
  }
</script>

<div class="pt-10">
  <button
    on:click={() => ($isModalOpen = true)}
    class="rounded-lg bg-gradient-to-r from-blue-500 to-purple-500 px-8 py-2 font-semibold text-white hover:opacity-90"
  >
    Add
  </button>

  <ProductsTable {products} />
</div>

<Modal title="Add new product">
  <AddPostForm />
</Modal>
