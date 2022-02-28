<template>
  <header>
            <h1>The&nbsp;<strong>{{listName}}</strong>&nbsp;List</h1>
            <form class="search-box" @submit.prevent="HandleSearch">
                <input 
                    type="search" 
                    class="search-field" 
                    :placeholder="'Search for an '+listName+'...'"
                    required
                    v-model="search_query" />
                <button type="button" class="search-button" @click="listName = listName === 'manga' ? 'anime' : 'manga'">
                    Search {{listName === 'manga' ? 'Anime' : 'Manga'}}
                </button>
            </form>
            
        </header>
  <main>
            <div class="cards" v-if="mangalist.length > 0">
                <CardTemplate 
                    v-for="manga in mangalist" 
                    :key="manga.mal_id"
                    :manga="manga" />
            </div>
            <div class="no-results" v-else>
                <h3>Sorry, we have no results found please search something else</h3>
            </div>
        </main>
</template>

<script>
import CardTemplate from './components/CardTemplate.vue'

const jikan_api = 'https://api.jikan.moe/v4/';

export default {
  components: {
    CardTemplate
  },
  data() {
    return {
      mangalist: [],
      search_query: '',
      listName: 'manga'
    }
  },
  methods: {
    HandleSearch() {
      this.GetmangaList()
    },
    GetmangaList() {
      fetch(`${jikan_api+this.listName}?q=${this.search_query}`)
        .then(async response => {
          this.mangalist = (await response.json()).data
        })
        .catch(error => {
          console.log(error)
        })
    }
  },
  mounted() {
    this.GetmangaList()
  }
}

</script>

<style lang="scss">
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Fira Sans', sans-serif;
}
a {
    text-decoration: none;
}
header {
    padding-top: 50px;
    padding-bottom: 50px;
    h1 {
        color: #888;
        font-size: 42px;
        font-weight: 400;
        text-align: center;
        text-transform: uppercase;
        margin-bottom: 30px;
        strong {
            color: #e62cf0;
        }
        &:hover {
            color: #313131;
        }
    }
    .search-box {
        display: flex;
        justify-content: center;
        padding-left: 30px;
        padding-right: 30px;
        .search-button {
            appearance: none;
            background: none;
            border: none;
            outline: none;
            background-color: #AAA;
            box-shadow: 0px 4px 8px rgba(29, 29, 29, 0.15);
            display: block;
            width: 80%;
            max-width: 120px;
            padding: 15px;
            margin-left: 16px;
            border-radius: 10px;
            font-size: 15px;
            transition: 0.4s;
        }
        .search-field {
            appearance: none;
            background: none;
            border: none;
            outline: none;
            background-color: #F3F3F3;
            box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);
            display: block;
            width: 100%;
            max-width: 600px;
            padding: 15px;
            border-radius: 8px;
            color: #313131;
            font-size: 20px;
            transition: 0.4s;
            &::placeholder {
                color: #AAA;
            }
            &:focus, &:valid {
                color: #FFF;
                background-color: #696969;
                box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
            }
        }
    }
}

main {
    max-width: 1200px;
    margin: 0 auto;
    padding-left: 30px;
    padding-right: 30px;
    .cards {
        display: flex;
        flex-wrap: wrap;
    justify-content: space-evenly;
        margin: 0 -8px;
    }
  .no-results {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    h3 {
      color: #888;
      font-size: 37px;
      font-weight: 350;
      text-align: center;
      text-transform: uppercase;
      margin-bottom: 30px;
      margin-top: 30px
      strong {
        color: #313131;
      }
    }
  }
}

</style>
