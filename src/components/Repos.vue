<template>
  <div>
    <div class="center d-flex m-4">
      <b-button class="m-2" variant="primary" @click="allRepo">All</b-button>
      <b-button class="m-2" variant="success" @click="forked">Forked</b-button>
      <b-button class="m-2" variant="info" @click="notForked">Not forked</b-button>
      <b-button class="m-2" variant="warning" @click="getOrderBycreated">Order by created</b-button>

      <div class="d-flex m-2">
        <b-form-input v-model="userId" debounce="500" placeholder="Enter your Github username"></b-form-input>
        <b-form-input v-model="searchWords" debounce="500" placeholder="Enter keyword to search"></b-form-input>
      </div>
    </div>
    <div v-if="validUsername">
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
    <div v-else>
      <div class="d-flex align-items-center">
        <strong>Loading...</strong>
        <b-spinner class="ml-auto"></b-spinner>
      </div>
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
      searchWords: "",
      validUsername: false,
      userId: "re4388"
    };
  },

  created() {
    // console.log(this.$route.params.id);
    this.fetchData();
  },
  computed: {
    filterSearch() {
      return this.repo.filter(i => i.name.match(this.searchWords));
    }
  },
  watch: {
    userId: function() {
      // this.$router.go()
      this.fetchData();
      this.getRemoteData();
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
        .get(
          `https://api.github.com/users/${this.userId}/repos?page=1&per_page=100`
        )
        .then(resp => {
          this.validUsername = true;
          this.savedData = resp.data;
          this.repo = resp.data;
          console.log(resp);
        })
        .catch(err => {
          this.validUsername = false;
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