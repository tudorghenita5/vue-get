<template>
  <div class="container">
    <h1>Vue GET Title List</h1>
    <div v-if="loader" class="loader"></div>
    <ul v-else>
      <li v-for="(item, index) in tableData" :key="index">{{item.title}}</li>
    </ul>
  </div>
</template>

<script>
  const axios = require('axios');
  export default {
    data () {
      return {
        tableData: [],
        loader: false,
      }
    },
    created() {
      this.loader = true;
      let self = this;
      axios.get('https://jsonplaceholder.typicode.com/posts')
        .then(function (response) {
          self.loader = false;
          self.tableData = response.data.map(element => new InfoModel(element));
        })
    }
  }

  class InfoModel {
    constructor(data) {
      this.title = data.title
    }
  }
</script>

<style lang="scss">
  h1 {
    text-align: center;
  }
  ul {
    margin: 0;
    padding: 0;
    list-style-type: none;
    li {
      padding: 5px 0;
      transition: all .2s ease;
      cursor: pointer;
      &:nth-child(even) {
        background-color: #f2f2f2;
      }
      &:hover {
        background-color: #f0f8fd;
      }
    }
  }

  .loader {
    position: fixed;
    z-index: 9999;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    &:before {
      content: "";
      display: block;
      border: 7px solid rgba(0, 0, 0, .1);
      border-right: 7px solid #5054dd;
      border-radius: 50%;
      animation: spinner 1s linear infinite;
      min-width: 70px;
      min-height: 70px;
      z-index: 1;
    }
    &:after {
      content: "";
      opacity: .6;
      background: #fff;
      position: absolute;
      left: 0;
      top: 0;
      height: 100%;
      width: 100%;
    }
  }

  @keyframes spinner {
    to {
      transform: rotate(360deg);
    }
  }

  @media (min-width: 720px) {
    .container {
      max-width: 1320px;
      margin: 0 auto;
    }
    ul {
      column-count: 2;
    }
  }
</style>
