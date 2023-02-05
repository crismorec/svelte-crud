<script>
  import { v4 } from 'uuid'
  import Noty from 'noty'

  import 'noty/lib/noty.css'
  import 'noty/lib/themes/sunset.css'

  let products = [
    {
      id: 1,
      name: 'name Hp 1',
      description: 'descp Hp laptop1',
      category: 'laptop',
    },
    {
      id: 2,
      name: 'name Mouse Razer',
      description: 'descp gamin mouse',
      category: 'prefipherials',
    },
  ]

  let product = {
    id: '',
    name: '',
    description: '',
    category: '',
    imageURL: '',
  }

  let editStatus = false

  const cleanProduct = () => {
    let product = {
      id: '',
      name: '',
      description: '',
      category: '',
      imageURL: '',
    }
  }

  const addProduct = () => {
    const newProduct = {
      id: v4(),
      name: product.name,
      description: product.description,
      category: product.category,
      imageURL: product.imageURL,
    }
    products = products.concat(newProduct)
    cleanProduct()
  }

  const updateProduct = () => {
    let updatedProduct = {
      name: product.name,
      description: product.description,
      id: product.description,
      imageURL: product.imageURL,
      category: product.category,
    }

    const productIndex = products.findIndex((p) => p.id == product.id)
    products[productIndex] = updatedProduct
    editStatus = false
    new Noty({
      theme: 'sunset',
      type: 'success',
      timeout: 2000,
      text: 'Product Updated Successfully',
    }).show()
  }

  const onSubmitHandler = (e) => {
    if (!editStatus) {
      addProduct()
    } else {
      updateProduct()
    }
  }

  const deleteProduct = (id) => {
    products = products.filter((product) => product.id != id)
  }

  const editProduct = (productEdited) => {
    product = productEdited
    editStatus = true
  }
</script>

<style>

</style>

<main>
  <div class="container p-4">
    <div class="row">
      <div class="col-md-6">
        {#each products as product}
          <div class="card mt-2 p-2">
            <div class="row">
              <div class="col-md-4">
                {#if !product.imageURL}
                  <img
                    src="images/no-product.jpg"
                    alt=""
                    class="img-fluid p-2" />
                {:else}
                  <img src={product.imageURL} alt="" class="img-fluid p-2" />
                {/if}
              </div>
              <div class="col-md-8">
                <div class="car-body">
                  <h5>
                    <strong>{product.name}</strong>
                    <span>
                      <small>{product.category}</small>
                    </span>
                  </h5>
                  <p class="cart-text">{product.description}</p>
                  <button
                    class="btn btn-danger"
                    on:click={deleteProduct(product.id)}>
                    Delete
                  </button>
                  <button
                    class="btn btn-secondary"
                    on:click={editProduct(product)}>
                    Edit
                  </button>
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>
      <div class="col-md-6">
        <div class="card">
          <div class="card-body" />

          <form on:submit|preventDefault={onSubmitHandler}>
            <div class="form-group">
              <input
                bind:value={product.name}
                type="text"
                placeholder="Product Name"
                id="product-name"
                class="form-control" />
            </div>

            <div class="form-group">
              <textarea
                bind:value={product.description}
                id="product-description"
                rows="3"
                placeholder="Product Description"
                class="form-control" />
            </div>

            <div class="form-group">
              <input
                bind:value={product.imageURL}
                type="url"
                placeholder="https://random.com"
                id="product-image-url"
                class="form-control" />
            </div>

            <div class="form-group">
              <select
                bind:value={product.category}
                id="category"
                class="form-control">
                <option value="laptops">Laptops</option>
                <option value="prefipherial">Prefipherial</option>
                <option value="servers">Servers</option>
              </select>
            </div>

            <button class="btn btn-secondary">
              {#if !editStatus}Save product{:else}Update Product{/if}
            </button>
          </form>
        </div>
      </div>
    </div>

  </div>
</main>
