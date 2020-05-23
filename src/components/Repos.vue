<template>
  <div>
    <div class="center d-flex m-4">
      <b-button class="m-2" variant="primary" @click="allRepo">All</b-button>
      <b-button class="m-2" variant="success" @click="forked">Forked</b-button>
      <b-button class="m-2" variant="info" @click="notForked">Not forked</b-button>
      <b-button class="m-2" variant="warning" @click="getOrderBycreated">Order by created</b-button>
      <div class="m-2">
          <b-form-input v-model="searchWords" placeholder="請輸入要搜尋的字詞"></b-form-input>
          <!-- <input class="input" type="text" placeholder="請輸入要搜尋的字詞" v-model="searchWords" /> -->
      </div>
    </div>
    <div v-if="searchWords" class="center d-flex flex-wrap">
      <ul v-for="i in filterSearch" :key="i.id">
        <li>
          <a :href="`${i.html_url}`" target="_blank">{{ i.name }} {{ i.fork?'/ forked':'' }}</a>
        </li>
      </ul>
    </div>
    <div v-if="searchWords === ''" class="center d-flex flex-wrap">
      <ul v-for="i in repo" :key="i.id">
        <li>
          <a :href="`${i.html_url}`" target="_blank">{{ i.name }} {{ i.fork?'/ forked':'' }}</a>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "Repos",

  data() {
    return {
      savedData: {},
      repo: {},
      searchWords: ""
    };
  },

  created() {
    this.fetchData();
    // this.getRemoteData();
  },
  computed: {
    filterSearch() {
      return this.repo.filter(i => i.name.match(this.searchWords));
    }
  },

  methods: {
    getRemoteData() {
      this.repo = this.savedData;
    },
    // "created_at": "2019-05-31T01:41:12Z",
    getOrderBycreated() {
      this.getRemoteData();
      this.repo = this.repo.sort(function(a, b) {
        return new Date(b.created_at) - new Date(a.created_at);
      });
    },
    allRepo() {
      this.getRemoteData();
    },
    forked() {
      this.getRemoteData();
      this.repo = this.repo.filter(i => {
        return i.fork === true;
      });
    },
    notForked() {
      this.getRemoteData();
      this.repo = this.repo.filter(i => {
        return i.fork === false;
      });
    },
    fetchData() {
      axios
        .get("https://api.github.com/users/re4388/repos?page=1&per_page=100")
        .then(resp => {
          this.savedData = resp.data;
          this.repo = resp.data;
          console.log(resp);
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>

<style scoped>
ol,
ul {
  list-style: none;
}

/* .news p {
  font-size: 10px;
} */
</style>