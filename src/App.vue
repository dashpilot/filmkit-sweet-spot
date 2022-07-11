
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

          <template v-if="loading">
            <div class="loading pt-3">
              <div class="fas fa-spinner fa-spin"></div>
            </div>
          </template>

          <ul class="list-group">
            <template v-for="item in selection">
              <a class="list-group-item" @click="curItem = item">Canon EF {{item.title}}</a>
            </template>
          </ul>
        </div>


        <div class="detail" :class="{'show' : curItem}">

          <template v-if="curItem">

            <div class="wdgt-toolbar">

              <i class="fas fa-chevron-left" @click="curItem = null"></i>

              <span>Canon EF {{curItem.title}}</span>

            </div>


            <div class="wdgt-content p-3">

              <template v-if="curItem.main_image">
                <div class="img-wrap">
                  <img :src="`https://lensdata.b-cdn.net/img/${curItem.main_image}`" />
                </div>
              </template>



              <table class="table table-striped table-hover">
                <tbody>
                  <tr>
                    <td class="w-50">
                      <strong>Center Sharpness<template v-if="curItem.tele"> @{{curItem.wide}}mm</template>:</strong>
                    </td>
                    <td class="w-50">
                      f/{{curItem.wide_center_sharpness}}
                    </td>
                  </tr>
                  <tr>
                    <td>
                      <strong>Corner Sharpness<template v-if="curItem.tele"> @{{curItem.wide}}mm</template>:</strong>
                    </td>
                    <td>
                      f/{{curItem.wide_corner_sharpness}}
                    </td>
                  </tr>
                  <template v-if="curItem.tele">
                    <tr>
                      <td>
                        <strong>Center sharpness @{{curItem.tele}}mm:</strong>
                      </td>
                      <td>
                        f/{{curItem.tele_center_sharpness}}
                      </td>
                    </tr>
                    <tr>
                      <td>
                        <strong>Corner sharpness @{{curItem.tele}}mm:</strong>
                      </td>
                      <td>
                        f/{{curItem.tele_corner_sharpness}}
                      </td>
                    </tr>
                  </template>
                  <template v-if="curItem.diffraction">
                    <tr>
                      <td>
                        <strong>Diffraction:</strong>
                      </td>
                      <td>
                        f/{{curItem.diffraction}}
                      </td>
                    </tr>
                  </template>
                  <template v-if="curItem.filter_size">
                    <tr>
                      <td>
                        <strong>Filter Size:</strong>
                      </td>
                      <td>
                        {{curItem.filter_size}}mm
                      </td>
                    </tr>
                  </template>
                </tbody>
              </table>

              <template v-if="curItem.notes">
                <div class="alert alert-warning">
                  <strong>Note:</strong> {{curItem.notes}}
                </div>
              </template>


            </div>


          </template>

        </div>


      </div>

    </div>


    <div class="text-center wdgt-footer">Made by <a href="https://www.filmkit.net">Filmkit</a></div>




  </main>
</div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      loading: true,
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
        this.loading = false;

        let myitems = data.categories.filter(x => x.slug == 'canon-ef')[0]
        localStorage.setItem('items', JSON.stringify(myitems.posts));

        // sort numerically
        myitems.posts.sort((a, b) => a.wide - b.wide)

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
main {
  overflow: hidden;
}

.wdgt-toolbar {
  padding: 10px;
  border-bottom: 2px solid black;
  text-align: center;
  background-color: #F8F8F8;
}

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

.detail .wdgt-content {
  height: 565px;
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
  user-select: none;
}

.wdgt-footer {
  padding: 10px;
  border-top: 2px solid black;
}

.loading {
  text-align: center;
}
</style>
