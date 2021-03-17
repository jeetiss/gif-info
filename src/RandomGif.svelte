<script>
  const giphy = {
    baseURL: "https://api.giphy.com/v1/gifs/",
    apiKey: "0UTRbFtkMxAplrohufYco5IY74U8hOes",
    type: "random",
    rating: "pg-13",
  };

  let giphyURL = encodeURI(
    giphy.baseURL +
      giphy.type +
      "?api_key=" +
      giphy.apiKey +
      "&rating=" +
      giphy.rating
  );

  let promise = fetch(giphyURL)
    .then((res) => res.json())
    .then(({ data }) => data);
</script>

{#await promise then data}
  <img src={data.image_original_url} alt="" />
{/await}

<style>
  img {
    width: 30%;
    padding: 10px;
  }
</style>
