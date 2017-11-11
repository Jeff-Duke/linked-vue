<template>
  <main id="app">
    <section class="inputSection">
      <h1>Linked <span class="title-logo" /> List</h1>
      <input type="text" v-model="bookmarkTitle" placeholder="Website Title"/>
      <input type="text" v-model="bookmarkUrl" placeholder="Website URL"/>
      <button @click="validateBookmark">Enter</button>
      <h2 class="error" v-if="error">{{ error }}</h2>
    </section>
    <section class="bookmarkSection">
      <div v-for="bookmark in bookmarkArray" :class="[{read: bookmark.read}, 'bookmarkCard']" :key="bookmark.id">
        <h3>{{ bookmark.title }}</h3>
        <span class="divider" />
        <a :href="bookmark.url">{{ bookmark.url }}</a>
        <span class="divider" />
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
      error: "",
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

    validateBookmark() {
      if (!this.bookmarkTitle) {
        this.error = "Please enter a Website Title";
      } else if (!this.bookmarkUrl) {
        this.error = "Please enter a Website Address";
      } else {
        this.error = "";
        this.createBookmark();
      }
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
        if (bookmark.id === id) {
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

body {
  font-family: "Open Sans", sans-serif;
  margin: 0;
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
  display: flex;
  flex-direction: row;

  @media only screen and (max-width: 768px) {
    flex-direction: column;
  }
}

h1,
h2,
h3 {
  font-weight: normal;
}

button {
  border: none;
}

.inputSection {
  color: $color-white;
  display: inline-block;
  background-color: $color-dark-slate;
  margin: none;
  padding: 2rem;
  min-height: 100vh;

  @media only screen and (max-width: 768px) {
    height: 20rem;
    min-height: 0;
  }

  h1,
  h2,
  h3 {
    color: $color-white;
  }

  h1 {
    font-size: 2rem;
    font-weight: 800;
  }

  .title-logo {
    background: url('./assets/linked-list-logo.svg') center no-repeat ;
    content: '';
    display: inline-block;
    height: 1.75rem;
    width: 3rem;
  }

  .error {
    color: $color-red;
    margin-top: 2rem;
  }

  input[type="text"] {
    height: 2rem;
    padding: 0.25rem;
    font-size: 1.125rem;
  }

  button {
    cursor: pointer;
    margin: 1rem;
    height: 2rem;
    background-color: $color-yellow;
    color: $color-white;
    font-weight: 800;
    font-size: 0.8rem;

    &:hover {
      background-color: $color-light-yellow;
    }
  }

  input[type="text"],
  button {
    display: block;
    width: 100%;
    margin: 2rem auto;
  }
}

.inputSection {
  min-width: 50vw;
}

.bookmarkSection {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 2rem;
  width: 100%;

  @media only screen and (max-width: 768px) {
    width: auto;
  }

  .bookmarkCard {
    background-color: #fff;
    border: 1px solid $color-slate;
    min-height: 8rem;
    width: 100%;
    margin: 1rem 2rem;
    padding: 1rem;

    h3 {
      color: $color-dark-slate;
      margin-top: 0;
    }

    a {
      color: $color-dark-slate;
      font-size: 1rem;
      text-decoration: none;
      display: inline-block;
      margin-bottom: 1rem;
    }

    .divider {
      border-bottom: 2px solid $color-light-slate;
      display: block;
      height: 4px;
      width: 100%;
      margin-bottom: 1rem;
    }

    button {
      background-color: $color-white;
      border: none;
      color: $color-dark-slate;
      cursor: pointer;
      font-size: 1rem;
      padding-left: 0;
    }

    button ~ button {
      padding-left: 0.5rem;
      padding-right: 0;
      float: right;
    }

    button,
    a {
      border-bottom: 2px solid $color-light-gray;
    }

    &:hover {
      button, a {
        border-bottom: 2px solid $color-yellow;
      }
    }
  }

  .bookmarkCard.read {
    background-color: $color-light-gray;

    button {
      background-color: $color-light-gray;
    }

    button.read-button {
      color: $color-red;
    }
  }
}
</style>
