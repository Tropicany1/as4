<script setup>
import axios from "axios";

const getData = async (url, params) => {
  try {
    return await axios.get(url, params);
  } catch (error) {
    console.log(error);
  }
};

const getMovies1 = async () => {
  let movieInfo= document.getElementById("movieInfo");
  movieInfo.innerHTML="";
  let selectedMovie = document.getElementById("movieselect")
  const movieData = await getData ("https://api.themoviedb.org/3/search/movie", {
  params: {
    api_key: "541f480956a206bb8529fb019433027e",
      query: selectedMovie.value,
    }
  });

  if (movieData.data.results.length < 1) {
    return;
  }
 
  let movie= movieData.data.results[0];
    const extraData = await getData(`https://api.themoviedb.org/3/movie/${movie.id}`, {
      params: {
        api_key: "541f480956a206bb8529fb019433027e",
        append_to_response: "videos",
      }
    });

    const trailer = extraData.data.videos.results.filter((video) => video.type === "Trailer").at(0).key;
    const p = document.createElement('p');
    const img = document.createElement('img');
    const iframe = document.createElement('iframe');

    p.innerHTML = `${movie.title} -- ${movie.release_date} -- ${movie.overview}-- ${movie.popularity}`;
    img.src = `https://image.tmdb.org/t/p/w500${movie.poster_path}`
    iframe.src = `https://www.youtube.com/embed/${trailer}`

    movieInfo.append(p);
    movieInfo.append(img);
    movieInfo.append(iframe);
  }

;

getMovies1();

</script>

<template>

<body>
  <h1> Pick a movie from my all time favourites! </h1>
  <div class="selector">
        <label for="movieselect">Choose a movie:</label>
        <select name="Movies" id="movieselect">
          <option value="despicable Me 3">Despicable Me 3</option>
          <option value="of saiki k">The Disastrous Life of Saiki K.</option>
          <option value="jujutsu kaisen">Jujutsu Kaisen</option>
          <option value="spiderman">Spiderman</option>
          <option value="attack on titan">Attack on Titan</option>
          <option value="panic in the Mailroom">Panic in the Mailroom</option>
          <option value="tokyo ghoul">Tokyo Ghoul</option>
          <option value="minions: The Rise Of Gru">Minions: The Rise Of Gru</option>
          <option value="a silent voice">A Silent Voice: The Movie</option>
          <option value="naruto">Boruto: Naruto the Movie</option>
         
        </select>
        <!-- <br><br> -->
        <button class="button" v-onClick="getMovies1()"> Get </button>
       <div id =movieInfo>
 
       </div>
    </div>
</body>


</template>


<style>
</style>