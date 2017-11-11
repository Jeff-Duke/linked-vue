<template>
  <main id="app">
    <aside class="inputSection">
      <h1>{{ title }}</h1>
      <input type="text" v-model="bookmarkTitle" placeholder="Website Title"/>
      <input type="text" v-model="bookmarkUrl" placeholder="Website URL"/>
      <button v-on:click="createBookmark">Submit</button>
    </aside>
    <section class="bookmarkSection">
      <div v-for="bookmark in bookmarkArray" :class="[{read: bookmark.read}, 'bookmarkCard']" :key="bookmark.id">
        <h3>{{ bookmark.title }}</h3>
        <a :href="bookmark.url">{{ bookmark.url }}</a>
        <button class="read-button" @click="markAsRead(bookmark.id)">Read</button>
        <button @click="deleteBookmark(bookmark.id)">Delete</button>
      </div>
    </section>
  </main>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      title: "Linked List",
      bookmarkTitle: "",
      bookmarkUrl: "",
      bookmarkArray: [
        { id: 231, title: "google", url: "http://www.google.com", read: false }
      ]
    };
  },
  methods: {
    clearInputs() {
      (this.bookmarkTitle = ""), (this.bookmarkUrl = "");
    },
    createBookmark() {
      const bookmark = {
        title: this.bookmarkTitle,
        url: `http://${this.bookmarkUrl}`,
        read: false,
        id: Date.now() * Math.random()
      };
      this.bookmarkArray.push(bookmark);
      this.clearInputs();
      console.log(this.bookmarkArray);
    },
    markAsRead(bookmarkId) {
      const id = parseInt(bookmarkId);
      return this.bookmarkArray.find(function(bookmark) {
        if(bookmark.id === id) {
          bookmark.read = !bookmark.read;
        }
      });
    },
    deleteBookmark(bookmarkId) {
      const id = parseInt(bookmarkId);
      this.bookmarkArray = this.bookmarkArray.filter(function(i) {
        return i.id !== id;
      });
    }
  }
};
</script>

<style lang="scss">
html {
  box-sizing: border-box;
}
*,
*:before,
*:after {
  box-sizing: inherit;
}

$color-white: #fff;
$color-light-gray: #f2f4f4;
$color-dark-slate: #455a64;
$color-slate: #607d8b;
$color-light-slate: #cfd8dc;
$color-yellow: #ffc107;
$color-light-yellow: #ffd773;
$color-red: #f05a28;

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: row;

  @media only screen and (max-width: 600px) {
    flex-direction: column;
  }
}

h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.inputSection {
  color: $color-white;
  display: inline-block;

  input[type="text"] {
    height: 3rem;
    width: 100%;
    margin: 1rem;
    padding: 0.5rem;
    font-size: 1.125rem;
  }

  button {
    display: block;
    width: 100%;
    margin: 1rem;
    height: 2rem;
  }
}

.inputSection,
.bookmarkSection {
  min-width: 48vw;
}

.bookmarkSection {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: 2rem;

  .bookmarkCard {
    background-color: #fff;
    border: 1px solid $color-slate;
    min-height: 8rem;
    width: 100%;
    margin: 1rem 0;
    margin-left: 2rem;
    padding: 1rem;

    button {
      border: none;
      color: $color-dark-slate;
      cursor: pointer;
      border-bottom: 1px solid $color-light-gray;
    }
  }

  .bookmarkCard.read {
    background-color: $color-light-gray;

    button.read-button {
      color: $color-red;
    }
  }
}
</style>
