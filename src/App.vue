<template>
  <div id="app">
    <!-- Добавляю Loader, если loading = true -->
    <div class="loader" v-if="loading">
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
    </div>
    <div v-if="!loading && !errored">
      <div class="header">
        <div v-for="headerName in info.header" :key="headerName">
          <a href="#">{{ headerName }}</a>
        </div>
      </div>
      <div class="about-us">
        <div class="about-us-desc">
          <h1 class="about-us-h1">{{ info.title }}</h1>
          <p class="info-description">{{ info.description }}</p>
          <p class="info-description short-desc">
            {{ info.short_description }}
          </p>
        </div>
        <button type="button" @click="switchLang" class="switch-button">
          {{ lang }}
        </button>
      </div>

      <div class="blocks">
        <div class="mission">
          <h1>{{ info.about_area.mission.title }}</h1>
          <p>{{ info.about_area.mission.description }}</p>
        </div>
        <div class="principles">
          <h1>{{ info.about_area.principles.title }}</h1>

          <ul v-html="info.about_area.principles.description"></ul>
        </div>
        <div class="team">
          <h1>{{ info.about_area.team.title }}</h1>
          <h3>{{ info.about_area.team.description }}</h3>
        </div>
      </div>
      <form action="https://finakademiya.ru/" target="_blank">
        <button class="meet-us">
          {{ info.about_area.team.button }}
        </button>
      </form>
      <div class="additional-info">
        <h1 class="about-us-h1 info-h1">{{ partners }}</h1>
        <div class="partners">
          <div
            class="partner"
            v-bind="partnership"
            v-for="partners in info.partners"
            :key="partners.name"
            v-html="
              '<h1>' +
              partners.name +
              '</h1>' +
              '<br>' +
              '<p>' +
              partners.description +
              '</p>'
            "
          >
            >
          </div>
        </div>
      </div>
      <form action="#" target="_blank" class="products">
        <button
          class="product"
          v-for="products in info.products"
          :key="products"
        >
          {{ products.title }} . {{ products.description }}
        </button>
      </form>
      <div class="news" v-if="show">
        <p class="new" v-for="news in info.news" :key="news">
          {{ news.title }} <br />
          {{ news.description }}
        </p>
      </div>
      <button type="button" @click="showNews" class="show-news">
        {{ showButton }}
      </button>
      <div class="footer">
        <a href="https://www.instagram.com/" target="_blank"
          ><img src="/img/instagram.png"
        /></a>
        <a href="https://www.facebook.com/" target="_blank"
          ><img src="/img/facebook.png"
        /></a>
        <div class="phone">
          <a href="tel:+74951282028" class="info-h1">74951282028</a>
        </div>
        <div class="mail">
          <p>
            <a href="mailto:info@finakademiya.ru" class="info-h1"
              >info@finakademiya.ru</a
            >
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const axios = require("axios").default;
export default {
  data() {
    return {
      lang: "en",
      url: "https://api.ybershop.ru/finakademiya/ru",
      partners: "Наши партнеры",
      info: null,
      loading: false,
      errored: false,
      showButton: "Последние новости",
      show: false,
    };
  },
  name: "App",
  mounted() {
    //Забираю содержимое из предоставленного API
    this.loading = true;
    axios
      .get(this.url)
      .then((response) => {
        this.info = response.data;
      })
      .catch((error) => {
        console.log(error);
        this.errored = true;
      })
      .finally(() => (this.loading = false));
  },
  methods: {
    //Метод для переключения языка

    switchLang: function () {
      if (this.lang != "en") {
        this.lang = "en";
        this.partners = "Наши партнеры";
        this.showButton = "Последние новости";
        axios
          .get(this.url)
          .then((response) => {
            this.info = response.data;
          })
          .catch((error) => {
            console.log(error);
            this.errored = true;
          })
          .finally(() => (this.loading = false));
      } else {
        this.lang = "ru";
        this.partners = "Our partners";
        this.showButton = "Latest news";
        axios
          .get("https://api.ybershop.ru/finakademiya/en")
          .then((response) => {
            this.info = response.data;
          })
          .catch((error) => {
            console.log(error);
            this.errored = true;
          })
          .finally(() => (this.loading = false));
      }
    },
    // Метод который показывает\скрывает новости
    showNews: function () {
      if (!this.show) {
        this.show = true;
      } else {
        this.show = false;
      }
    },
  },
};
</script>

<style>
/* header */
.header,
.footer {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  font-family: "Montserrat", sans-serif;
  font-weight: 900;
  margin: 10px auto;
  font-size: 20px;
  flex-wrap: wrap;
}

.header a {
  margin: 10px;
  display: block;
}

.header a:hover {
  color: skyblue;
}

/* about-us */

.about-us {
  width: 100%;
  padding: 50px 10px;
  background: url(/img/financies.jpg);
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  position: relative;
}
.about-us-h1 {
  color: black;
  margin-bottom: 30px;
  font-size: 3em;
  border-left: 3px solid black;
  padding-left: 10px;
  text-transform: uppercase;
}
.about-us-desc,
.new {
  display: flex;
  justify-content: center;
  flex-direction: column;
  max-width: 820px;
  background: rgba(247, 245, 245, 0.7);
  padding: 50px;
}

.info-description {
  color: black;
  font-size: 2em;
}

.short-desc {
  padding-top: 20px;
}

.blocks,
.partners {
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-wrap: wrap;
  padding: 20px 0;
}

/* mission  */

.mission {
  background: url(/img/mission.png);
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
.principles {
  background: url(/img/principles.png);
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.team {
  background: url(/img/team.png);
  background-position-x: 50%;
  background-repeat: no-repeat;
  background-size: cover;
}
.mission,
.principles,
.team,
.partner {
  max-width: 300px;
  min-width: 250px;
  max-height: 300px;
  min-height: 250px;
  text-align: center;
  box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
  background-color: lightcyan;
  opacity: 0.8;
  margin: 20px;
  display: flex;
  justify-content: center;
  flex-direction: column;
  border-radius: 20px;
  padding: 0 10px;
}

.meet-us,
.show-news {
  margin: 20px auto;
  display: block;
  font-size: 24px;
  user-select: none; /* убирать выделение текста */
  background: lightcyan; /* фон кнопки */
  padding: 0.7em 1.5em; /* отступ от текста */
  outline: none; /* убирать контур в Mozilla */
  border-radius: 20px;
  box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
  transition: box-shadow 0.1s, transform 0.1s;
  min-width: 250px;
}
.meet-us:hover,
.show-news:hover {
  background: lightblue;
}
.meet-us:active {
  background: skyblue;
  box-shadow: none;
  transform: scale(0.9);
}

.additional-info {
  background: url(/img/partners.jpg);
  background-position-x: center;
  background-repeat: no-repeat;
  background-size: cover;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  text-align: center;
  padding-top: 50px;
}

/* products  */

.products {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  min-width: 90vw;
  margin: 50px 0;
}

.product {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  flex-direction: column;
  background: rgba(111, 188, 233, 0.2);
  border-radius: 20px;
  margin: 30px 10px;
  font-size: 2em;
  max-width: 700px;
  padding: 15px;
  color: black;
  font-family: "Montserrat", sans-serif;
  font-weight: 900;
}

.product:hover {
  transform: scale(1.1);
}

.info-h1 {
  color: black;
}

/* news  */

.news {
  background: url(/img/news.jpg);
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  font-family: "Montserrat", sans-serif;
  font-weight: 900;
  font-size: 2em;
  margin: 30px 0;
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  align-items: flex-end;
}
.new {
  margin: 30px;
}

/* footer  */

.footer {
  margin-top: 50px;
  background: black;
  padding: 20px;
  align-items: center;
  margin-bottom: 0px;
}

.footer img {
  max-width: 100%;
  width: 50px;
}

.footer a {
  margin: 10px;
}

.footer a:hover {
  transform: scale(1.1);
}

.phone a,
.mail a {
  color: white;
}

.switch-button {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 24px;
  border: none;
  padding: 5px;
  background: white;
  opacity: 0.6;
}

.switch-button:hover {
  background: lightgray;
  opacity: 1;
}

@media (max-width: 1024px) {
  h1 {
    font-size: 20px;
  }
  p {
    font-size: 16px;
  }
  .product {
    font-size: 20px;
  }

  .info-description {
    font-size: 16px;
  }
  .about-us-h1 {
    font-size: 2em;
  }
  .about-us-desc,
  .new {
    padding: 10px;
    margin: 10px;
  }
  .header a {
    font-size: 16px;
  }
}

@media (max-width: 850px) {
  .footer {
    flex-direction: column;
  }
}
</style>
