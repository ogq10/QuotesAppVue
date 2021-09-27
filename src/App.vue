<template>
  <main>
    
      <input class="search-bar"
        type="search"
        placeholder="Search by Quote or Theme"
        v-model="filter"
      />
    

    
      <table>
        <thead>
          <th>Source</th>
          <th>Context</th>
          <th>Quote</th>
          <th>Theme</th>
        </thead>

        <tbody>
          <tr v-for="word in limitedView" :key="word">
            <td>{{ word.source }}</td>
            <td>{{ word.context }}</td>
            <td>{{ word.quote }}</td>
            <td>{{ word.theme }}</td>
          </tr>
        </tbody>
      </table>
    

    <div class="pagination">
      <button class="prev-button" @click="prevPage">Previous Page</button>
      <button class="next-button"  @click="nextPage">Next Page</button>
    </div>
  </main>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      allWords: [],
      filter: "",
      currentPage: 1,
      amountPerPage: 15,
    };
  },

  methods: {
    nextPage() {
      if((this.currentPage * this.amountPerPage) < this.filteredWords.length){
        return this.currentPage = this.currentPage + 1
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage = this.currentPage - 1;
      }
    },
  },
  mounted() {
    axios
      .get(
        "https://gist.githubusercontent.com/benchprep/dffc3bffa9704626aa8832a3b4de5b27/raw/quotes.json"
      )
      .then(({ data }) => {
        this.allWords = data;
        console.log(data);
        console.log(this.allWords);
      });
  },

  computed: {
    filteredWords() {
      return this.allWords.filter((quo) => {
        if (this.filter === "movies" || this.filter === "games") {
          return (
            quo.theme.toLowerCase().indexOf(this.filter.toLowerCase()) >= 0
          );
        }

        if (this.filter === "") {
          return true;
        } else {
          return (
            quo.quote.toLowerCase().indexOf(this.filter.toLowerCase()) >= 0
          );
        }
      });
    },

    limitedView() {
      return this.filteredWords.filter((row, index) => {
        const firstIndex = (this.currentPage - 1) * this.amountPerPage;
        const lastIndex = this.currentPage * this.amountPerPage;
        if (index >= firstIndex && index < lastIndex) {
          return true;
        }
      });
    },
  },
};
</script>

<style>
*{
  margin: 0; 
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
  font-size: 14px;

}

main{ 
  min-height: 100vh;
  background: linear-gradient(to right, #eecda3, #ef629f);
  display: flex; 
  align-items: center; 
  justify-content: center;
}

table{
  border-collapse: collapse;
  background: white;
  position: absolute;
  width: 90%;
  background: linear-gradient( to right bottom, rgba(255, 255, 255, 0.7),rgba(255, 255, 255, 0.3 ) );
  border-radius: .5rem;
}


th{
  padding: 15px 5px;
  position: sticky;
  top: 0;
  text-align: left;
}

td {
  padding: 10px 5px; 
  text-align: left;
}

.search-bar{
  background: linear-gradient(to right bottom, rgba(255, 255, 255, 0.7),rgba(255, 255, 255, 0.3 ));
  border: none; 
  padding: .5rem; 
  text-align: center;
  width: 20%;
  border-radius: 2rem;
  display: flex;
  position: absolute;
  top: 5%;
}

.pagination{
  border: none; 
  padding: .5rem; 
  text-align: center;
  text-decoration: none;
  position: absolute; 
  bottom: 5%;
}

.prev-button, .next-button { 
  background: linear-gradient(to right bottom, rgba(255, 255, 255, 0.7),rgba(255, 255, 255, 0.3 ));
  border-radius: .3rem;
  border: none; 
  margin: 20px;
  padding: 10px;
}
.next-button{
  width: 110px;
}

</style>
