<script setup>
import { ref } from "vue";
import axios from "axios";
const headTitle = ref(false);
const inputText = ref("");
const title = ref("");
const poster = ref("");
const ratings = ref("");
const stars = ref([]);
const storage = ref([]);
const removeData = ref("");
const searchMoviesApi = async () => {
  title.value = "";
  poster.value = "";
  ratings.value = "";
  stars.value = [];
  try {
    const res = await axios.get(
      `https://www.omdbapi.com/?i=tt3896198&apikey=a5a02f05&t=${inputText.value}`
    );
    // console.log(res);
    if (res.data.Poster !== "N/A") {
      poster.value = res.data.Poster;
    } else {
      poster.value =
        "https://upload.wikimedia.org/wikipedia/commons/thumb/6/65/No-Image-Placeholder.svg/1665px-No-Image-Placeholder.svg.png";
    }
    // console.log(res.data.Title);
    if (res.data.Title !== undefined) {
      title.value = res.data.Title;
    } else {
      title.value = "Movie not found!";
    }
    ratings.value = res.data.Ratings[0].Value.split("/")[0];
    // console.log(Math.round(ratings.value));
    if (ratings !== "") {
      for (let i = 0; i < Math.round(ratings.value); i++) {
        stars.value.push(i);
      }
      // console.log(stars.value);
    }
  } catch (err) {
    console.log(err);
  }
};
const searchMovies = () => {
  if (inputText.value !== "") {
    headTitle.value = true;
    searchMoviesApi();
  } else {
    headTitle.value = false;
  }
};
const saveToStorage = () => {
  const newMovie = {
    title: title.value,
    poster: poster.value,
    ratings: ratings.value,
  };
  storage.value.push(newMovie);
  localStorage.setItem("movies", JSON.stringify(storage.value));
  alert("新增成功");
};
const getStorage = () => {
  // console.log(JSON.parse(localStorage.getItem("movies")));
  const storedMovies = JSON.parse(localStorage.getItem("movies")) || [];
  if (storedMovies.length > 0) {
    storage.value = storedMovies;
    console.log(storage.value);
  } else {
    console.log("local storage是空的");
  }
};
const remove = (data) => {
  // console.log(data.srcElement.__vnode.key);
  removeData.value = data.srcElement.__vnode.key;
  storage.value = storage.value.filter((i) => i.title !== removeData.value);
  console.log(storage.value);
  localStorage.setItem("movies", JSON.stringify(storage.value));
  alert("刪除成功");
};
getStorage();
</script>

<template>
  <div class="container">
    <!-- 主題,查詢框 -->
    <div class="title">
      <h1>
        {{ headTitle == true ? "ADD A MOVIE" : "Movie List" }}
      </h1>
      <input
        type="text"
        placeholder="Movie Title"
        @blur="searchMovies"
        v-model="inputText"
        @focus="clearInputText"
      />
    </div>
    <!-- 變更內容 -->
    <div class="main">
      <div
        v-if="inputText !== '' && title !== 'Movie not found!'"
        class="movie"
      >
        <div class="poster">
          <img :src="poster" />
        </div>
        <div class="content">
          <h3>{{ title }}</h3>
          <div class="stars">
            <p>{{ ratings }}</p>
            <div v-for="star in stars">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                height="1em"
                viewBox="0 0 576 512"
              >
                <path
                  d="M316.9 18C311.6 7 300.4 0 288.1 0s-23.4 7-28.8 18L195 150.3 51.4 171.5c-12 1.8-22 10.2-25.7 21.7s-.7 24.2 7.9 32.7L137.8 329 113.2 474.7c-2 12 3 24.2 12.9 31.3s23 8 33.8 2.3l128.3-68.5 128.3 68.5c10.8 5.7 23.9 4.9 33.8-2.3s14.9-19.3 12.9-31.3L438.5 329 542.7 225.9c8.6-8.5 11.7-21.2 7.9-32.7s-13.7-19.9-25.7-21.7L381.2 150.3 316.9 18z"
                />
              </svg>
            </div>
          </div>
          <button @click="saveToStorage" class="btn">Add to list</button>
        </div>
      </div>
      <div
        v-else-if="inputText !== '' && title == 'Movie not found!'"
        class="movie"
      >
        <div class="content">
          <h3>{{ title }}</h3>
        </div>
      </div>
      <div v-else class="main-2">
        <div v-if="storage.length < 0">
          <h3>Your movie list is empty lets try to fill it up...</h3>
          <h3>
            Write your favorite movie name then click the add button and magic
            will happen...
          </h3>
        </div>
        <div v-else>
          <h3>收藏清單</h3>
          <br />
          <div
            v-for="item in storage"
            style="display: flex; justify-content: center"
          >
            <div
              style="
                background-color: white;
                width: 90%;
                display: flex;
                margin-bottom: 15px;
                justify-content: space-around;
              "
            >
              <img :src="item.poster" style="width: 30%; padding: 10px 0px" />
              <div style="display: flex; align-items: center; width: 60%">
                <h3 style="margin-right: 50px; width: 70%">{{ item.title }}</h3>
                <button @click="remove" :key="item.title" class="removeBtn">
                  x
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.container {
  width: 1200px;
}
.title {
  text-align: center;
  position: fixed;
  width: 77%;
  z-index: 10;
  background-color: beige;
  padding: 10px;
}
.title > h1 {
  font-weight: bold;
  margin-bottom: 5px;
}
.title > input {
  width: 70%;
  padding: 5px 10px;
  font-size: 1.1rem;
}
.main {
  display: flex;
  justify-content: center;
  margin-top: 150px;
}
.movie {
  background-color: white;
  display: flex;
  justify-content: center;
  width: 60%;
  gap: 100px;
}
.poster {
  width: 30%;
  top: -40px;
  left: 50px;
}
.poster > img {
  width: 180px;
  box-shadow: 5px 15px 15px gray;
}
.content {
  width: 70%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 10%;
  font-size: 1.2rem;
}
.stars {
  display: flex;
  gap: 10px;
}
.btn {
  width: 120px;
  padding: 8px;
  font-size: 1.2rem;
  font-weight: bold;
  background-color: black;
  color: white;
  border: none;
  cursor: pointer;
}
.main-2 {
  text-align: center;
}
.main-2 > h3 {
  font-weight: bold;
  margin: 15px;
}
svg {
  fill: #fff700;
}
.removeBtn {
  position: absolute;
  top: 10px;
  right: -10px;
  border: none;
  background-color: transparent;
  font-size: 1.2rem;
  cursor: pointer;
}
</style>
