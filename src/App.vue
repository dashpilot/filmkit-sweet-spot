
<template>
<div class="wrap">
  <main id="main">


    <div class="wdgt wdgt-main">

      <div class="wdgt-screen">

        <div class="wdgt-toolbar">
          Filmkit Lens Sweet Spot
        </div>





        <div class="wdgt-searchbar">
          <input type="text" class="form-control" placeholder="Search" v-model="searchTerm" @keyup="search()">
        </div>


        <div class="wdgt-content p-3">
          <ul class="list-group">
            <template v-for="item in selection">
              <a class="list-group-item" @click="curItem = item">{{item.title}}</a>
            </template>
          </ul>
        </div>


        <div class="detail" :class="{'show' : curItem}">

          <template v-if="curItem">

            <div class="wdgt-toolbar">

              <i class="fas fa-chevron-left" @click="curItem = null"></i>

              <span>Canon EF {{curItem.title}}</span>

            </div>

            <template v-if="curItem.main_image">
              <div class="img-wrap">
                <img :src="`https://lensdata.b-cdn.net/img/${curItem.main_image}`" />
              </div>
            </template>

            <div class="wdgt-content p-3">



              <strong>Center Sharpness:</strong> f/{{curItem.ff_center_sharpness}}<br>
              <strong>Corner Sharpness:</strong> f/{{curItem.ff_corner_sharpness}}<br>

              <strong>Sweet Spot:</strong> f/{{curItem.ff_corner_sharpness}}
            </div>


          </template>

        </div>


      </div>

    </div>


    <div class="text-center" id="footer">Made by <a href="https://www.filmkit.net">Filmkit</a></div>




  </main>
</div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      items: [],
      selection: [],
      searchTerm: null,
      curItem: null,
    }
  },
  created: function() {
    fetch('https://lensdata.b-cdn.net/data.json', {
        cache: "no-store"
      })
      .then(response => response.json())
      .then(data => {
        console.log(data)
        let myitems = data.categories.filter(x => x.slug == 'canon-ef')[0]
        localStorage.setItem('items', JSON.stringify(myitems.posts));

        myitems.posts.sort((a, b) => a.title.localeCompare(b.title)).reverse()

        this.items = myitems.posts;
        this.selection = myitems.posts;
      });
  },
  methods: {
    search() {
      let result = this.items.filter(x => x.title.includes(this.searchTerm))
      this.selection = result;
    }
  },
}
</script>

<style>
.wdgt-searchbar {
  padding: 15px;
  padding-bottom: 0;
  border-bottom: 2px solid black;
  height: 69px;
}

.wdgt-side {
  border-left: 2px solid black;
}

a.list-group-item {
  cursor: pointer;
}

a.list-group-item:hover {
  background-color: #F8F8F8;
  color: black;
}

.wdgt-content {
  height: 500px;
  overflow: hidden;
  overflow-y: auto;
}

.wdgt-toolbar span {

  text-align: center;
}

.wdgt-screen {
  position: relative;
}

.detail {
  background-color: white;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  right: -100%;
}

.show {
  transition: .7s;
  right: 0%;
}

.fa-chevron-left {
  float: left;

  margin-top: 4px;
}

.fa-chevron-left:hover {
  color: blue;
  cursor: pointer;
}

.img-wrap {
  clear: both;
  border-bottom: 2px solid black;
  padding-top: 15px;
}

.img-wrap img {
  display: block;
  width: 300px;
  margin: 0 auto;
  margin-bottom: 20px;
}

.form-control {
  border: 2px solid black;
}

.list-group-item {
  border: 2px solid black;
}

@media only screen and (max-width: 770px) {
  .wdgt-side {
    border-left: 0;
  }
}
</style>
