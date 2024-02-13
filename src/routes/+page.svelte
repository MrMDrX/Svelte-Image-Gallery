<script>
  import { onMount } from "svelte";
  import { fade, fly } from "svelte/transition";
  import axios from "axios";

  let keyword = "";
  let photos = [];
  const fetchImages = async () => {
    const res = await axios.get(
      `https://api.unsplash.com/search/photos?page=1&query=${keyword || "nature"}&client_id=YourUnsplashAccessKey`
    );
    photos = res.data.results;
  };

  onMount(() => {
    fetchImages();
  });

  const handleSearch = async () => {
    if (!keyword) return;
    await fetchImages();
    keyword = "";
  };
</script>

<div class="container">
  <div class="header">
    <h1>Image Gallery</h1>
    <div class="input-container">
      <input
        bind:value={keyword}
        type="text"
        class="input"
        placeholder="keyword"
      />
      <button on:click={handleSearch} class="button">Search</button>
    </div>
  </div>
  <div class="photos">
    {#each photos as photo, i (photo.id)}
      <img
        src={photo.urls.regular}
        alt={photo.alt_description}
        class="image"
        in:fly={{ y: 200, duration: 2000, delay: i * 200 }}
        out:fade
      />
    {/each}
  </div>
</div>

<style>
  .image {
    width: 30%;
    height: 250px;
    margin: 5px;
    object-fit: cover;
  }
  .photos {
    display: flex;
    flex-wrap: wrap;
  }
  .container {
    width: 1230px;
    margin: 0 auto;
  }
  .header {
    text-align: center;
    font-size: 20px;
  }
  .input {
    padding: 10px;
    width: 400px;
    border-radius: 10px;
    outline: none;
    border: 1px solid gray;
    font-size: 20px;
  }
  .button {
    padding: 10px;
    font-size: 20px;
    background-color: deepskyblue;
    border-radius: 10px;
    border: none;
    color: white;
  }
  .input-container {
    margin-bottom: 40px;
  }
</style>
