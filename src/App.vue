<template>
  <main id="app">
    <section class="inputSection">
      <h1>Linked <span class="title-logo" /> List</h1>
      
      <input
        v-model="bookmarkTitle"
        @keyup="clearError"
        type="text"
        placeholder="Website Title"
      />
      
      <input
        v-model="bookmarkUrl"
        @keyup.enter="validateBookmark"
        type="text"
        placeholder="Website URL"
      />
      
      <button
        @click="validateBookmark"
        :disabled="isDisabled"
      >
        Enter
      </button>
      
      <button
        @click="clearRead"
        :disabled="!totalRead"
      >
        Clear Read
      </button>
      
      <h3>Total links: {{totalLinks}}</h3>
      <h3>Total read: {{totalRead}}</h3>
      <h3>Total unread: {{totalUnread}}</h3>

      <h2 v-if="error" class="error">{{ error }}</h2>
    </section>
    <section class="bookmarkSection">
      <div
        v-for="bookmark in bookmarks"
        :class="[{read: bookmark.read}, 'bookmarkCard']"
        :key="bookmark.id"
      >
        <h3>{{ bookmark.title }}</h3>
        <span class="divider" />
        <a :href="bookmark.url">{{ bookmark.url }}</a>
        <span class="divider" />
        
        <button
          class="read-button"
          @click="markAsRead(bookmark.id)"
        >
          Read
        </button>
        
        <button @click="deleteBookmark(bookmark.id)">Delete</button>
      </div>
    </section>
  </main>
</template>

<script>
const EMPTY_TITLE = 'Please enter a Website Title';
const EMPTY_URL = 'Please enter a Website Address';
const INVALID_URL = 'You did not enter a valid URL';

export default {
  name: 'app',
  data() {
    return {
      error: '',
      bookmarkTitle: '',
      bookmarkUrl: '',
      bookmarks: [],
    };
  },
  computed: {
    isDisabled() {
      return !this.bookmarkTitle || !this.bookmarkUrl;
    },
    totalLinks() {
      return this.bookmarks.length;
    },
    totalRead() {
      return this.bookmarks.filter(bookmark => bookmark.read).length;
    },
    totalUnread() {
      return this.totalLinks - this.totalRead;
    },
  },
  methods: {
    clearInputs() {
      this.bookmarkTitle = '';
      this.bookmarkUrl = '';
    },
    validateBookmark() {
      if (!this.bookmarkTitle) {
        this.error = EMPTY_TITLE;
      } else if (!this.bookmarkUrl) {
        this.error = EMPTY_URL;
      } else {
        this.error = INVALID_URL;
        this.clearInputs();
      }

      if (this.validateUrl()) {
        this.createBookmark();
      } 
    },
    validateUrl() {
      const testUrl = `http://${this.bookmarkUrl}`;

      return /^(http|https|ftp):\/\/[a-z0-9]+([\-\.]{1}[a-z0-9]+)*\.[a-z]{2,5}(:[0-9]{1,5})?(\/.*)?$/i.test(
        testUrl
      );
    },
    clearError() {
      this.error = '';
    },
    createBookmark() {
      const {
        bookmarkTitle: title,
      } = this;

      const url = `http://${this.bookmarkUrl}`;
      const read = false;
      const id = Date.now();

      const bookmark = {
        title,
        url,
        read,
        id,
      };
      
      this.bookmarks.push(bookmark);
      this.clearInputs();
    },
    markAsRead(bookmarkId) {
      const id = parseInt(bookmarkId);

      return this.bookmarks.find((bookmark) => {
        if (bookmark.id === id) {
          bookmark.read = !bookmark.read;
        }
      });
    },
    deleteBookmark(bookmarkId) {
      const id = parseInt(bookmarkId);

      this.bookmarks = bookmarks.filter((bookmark) => {
        return bookmark.id !== id;
      });
    },
    clearRead() {
      this.bookmarks = this.bookmarks.filter(bookmark => !bookmark.read);
    },
  },
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

  &:disabled {
    pointer-events: none;
  }
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
    background: url("./assets/linked-list-logo.svg") center no-repeat;
    content: "";
    display: inline-block;
    height: 1.75rem;
    width: 3rem;
  }

  .error {
    color: $color-red;
    margin-top: 2rem;
  }

  input[type="text"] {
    height: 2.25rem;
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

    &:disabled {
      background-color: $color-light-gray;
      color: $color-light-slate;
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
      button,
      a {
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
