<template>
  <div id="app">
    <nav class="navbar">
      <ul class="nav">
        <li class="nav-item">
          <a class="nav-link mr-5" href="#" @click="isBooksVisible = true"
            >Books</a
          >
        </li>
        <li>
          <div style="color: white">
            <span>{{ favourites.length }} </span
            ><span v-if="favourites.length != 1"> books</span
            ><span v-if="favourites.length == 1"> book</span
            ><span class="me-2"> in your favourites list</span>
          </div>
        </li>
        <li class="nav-item">
          <a class="nav-link ml-5" href="#" @click="isBooksVisible = false"
            >Favourites</a
          >
        </li>
      </ul>
    </nav>
    <div class="container">
      <div
        class="row justify-content-center mt-3"
        v-if="isBooksVisible == true"
      >
        <div
          v-for="book in books"
          :key="book.ISBN"
          class="card"
          style="width: 23rem; margin: 0.3em"
        >
          <img class="card-img-top" :src="book.image" :title="book.author" />
          <div class="card-body">
            <p class="card-title">{{ book.category }}</p>
            <p class="card-title">{{ book.author }}</p>
            <p :class="[book.pages >= 50 ? 'more' : 'less']">
              #Pages : {{ book.pages }}
            </p>
            <p class="card-title">
              Price : {{ currencyFormatter(book.price) }}
            </p>
            <p class="card-title">ISBN : {{ book.ISBN }}</p>
            <button
              class="btn"
              v-if="!checkexistence(book)"
              @click="addBook = book"
            >
              Add To Favourites
            </button>
            <p
              v-if="checkexistence(book)"
              style="
                background-color: rgb(172, 151, 190);
                display: block;
                width: 100%;
              "
            >
              this Book is in your Favourites
            </p>
          </div>
        </div>
      </div>
      <!-- 
      favourites
     -->

      <div v-show="isBooksVisible == false" class="row">
        <div id="noBooks" v-if="favourites.length == 0">
          You don' t have any books in your favourites
        </div>

        <!-- table to show favourites -->

        <table v-if="favourites.length >= 1" class="table">
          <thead>
            <th>ISBN</th>
            <th>Name</th>
            <th>Category</th>
            <th>Author</th>
            <th>Pages</th>
            <th>Remove</th>
            <th>Price</th>
          </thead>
          <tbody>
            <tr v-for="book in favourites" :key="book.ISBN">
              <td>{{ book.ISBN }}</td>
              <td>{{ book.name }}</td>
              <td>{{ book.category }}</td>
              <td>{{ book.author }}</td>
              <td>{{ book.pages }}</td>
              <td>
                <button class="btn-danger" @click="removebook = book">
                  Remove
                </button>
              </td>
              <td>{{ currencyFormatter(book.price) }}</td>
            </tr>
          </tbody>
          <tfoot>
            <tr>
              <th colspan="4" class="text-center fw-bolder">Total Price:</th>
              <th colspan="3" class="text-center fw-bolder">
                {{ currencyFormatter(getTotalPrice) }}
              </th>
            </tr>
            <tr>
              <th colspan="4" class="text-center fw-bolder">Taxes:</th>
              <th colspan="3" class="text-center fw-bolder">
                {{ currencyFormatter(getTotalPrice * 0.1) }}
              </th>
            </tr>
            <tr>
              <th colspan="4" class="text-center fw-bolder">Grand Total:</th>
              <th colspan="3" class="text-center fw-bolder">
                {{ currencyFormatter(getTotalPrice * 0.1 + getTotalPrice) }}
              </th>
            </tr>
          </tfoot>
        </table>
      </div>
    </div>
  </div>
</template>
<script>
import bookes from "./Books";

export default {
  data() {
    return {
      books: bookes,
      favourites: [],
      isBooksVisible: true,
    };
  },
  methods: {
    // addBook(book) {
    //   this.favourites.push(book);
    // },
    checkexistence(book) {
      for (let i = 0; i < this.favourites.length; i++) {
        if (book.ISBN == this.favourites[i].ISBN) {
          return true;
        }
      }
      return false;
    },
    // removebook(book) {
    //   let boook;
    //   for (let i = 0; i < this.favourites.length; i++) {
    //     if (book.ISBN == this.favourites[i].ISBN) {
    //       boook = i;
    //     }
    //   }
    //   this.favourites.splice(boook, 1);
    // },
    currencyFormatter(value) {
      let formatter = Intl.NumberFormat("ar-SA", {
        style: "currency",
        currency: "SAR",
        minimumFractionDigits: 0,
      });
      return formatter.format(value);
    },
    //   getTotalPrice() {
    //     let result = 0;
    //     for (let i = 0; i < this.favourites.length; i++) {
    //       result += this.favourites[i].price;
    //     }
    //     return result;
    //   },
  },
  computed: {
    addBook: {
      set(book) {
        this.favourites.push(book);
      },
      get() {},
    },
    removebook: {
      set(book) {
        let boook;
        for (let i = 0; i < this.favourites.length; i++) {
          if (book.ISBN == this.favourites[i].ISBN) {
            boook = i;
          }
        }
        this.favourites.splice(boook, 1);
      },
      get() {},
    },
    getTotalPrice() {
      let result = 0;
      for (let i = 0; i < this.favourites.length; i++) {
        result += this.favourites[i].price;
      }
      return result;
    },
  },
};
</script>
<style scoped>
img {
  height: 300px;
}
p {
  display: inline-block;
  width: 40%;
  padding: 10px;
  background-color: rgb(223, 205, 238);
  margin: 10px;
  border-radius: 5px;
}
nav {
  background-color: rgb(90, 73, 104);
}
a {
  text-decoration: none;
  color: black;
  font-weight: bold;
  font-size: 20px;
  border: 2px solid black;
  border-radius: 5px;
  background-color: rgb(223, 205, 238);
}
a:hover {
  color: white;
  border: 2px solid rgb(223, 205, 238);
  background-color: rgb(90, 73, 104) ack;
}
.btn {
  background-color: rgb(90, 73, 104);
}
.more {
  color: rgb(118, 44, 178);
}
.less {
  color: rgb(138, 45, 45);
}
.more,
.less {
  font-size: 18px;
}

#noBooks {
  margin: 20px;
  padding: 10px;
  text-align: center;
  width: 100%;
  background-color: rgb(223, 205, 238);
}
.btn-danger {
  border: none;
  border-radius: 5px;
  padding: 10px;
}

table {
  margin-top: 30px;
}
thead {
  background-color: rgb(90, 73, 104);
}
tbody {
  background-color: rgb(223, 205, 238);
}

tfoot {
  background-color: rgb(156, 138, 171);
  text-align: justify;
}
</style>
