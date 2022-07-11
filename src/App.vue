
<template>
<div class="wrap">
  <main id="main">

    <div class="row g-0">
      <div class="col-md-6">



        <div class="wdgt wdgt-main">
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




        </div>

      </div>
      <div class="col-md-6  wdgt-side">

        <div class="wdgt">
          <div class="wdgt-toolbar">Lens info</div>

          <div class="wdgt-body">

            <template v-if="curItem">
              <h5>Canon EF</h5>
              <h4>{{curItem.title}}</h4>

              <br>
              <strong>Center Sharpness:</strong> f/{{curItem.ff_center_sharpness}}<br>
              <strong>Corner Sharpness:</strong> f/{{curItem.ff_corner_sharpness}}<br>

              <strong>Sweet Spot:</strong> f/{{curItem.ff_corner_sharpness}}
</template>



          </div>

          <div class="wdgt-footer">


          </div>

        </div>


      </div>

      <div class="text-center" id="footer">Made by <a href="https://www.filmkit.net">Filmkit</a></div>

    </div>




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
  height: 400px;
  overflow: hidden;
  overflow-y: auto;
}

h5 {
  text-transform: uppercase;
  font-size: 19px;
  margin-bottom: 3px;
}


@media only screen and (max-width: 770px) {
  .wdgt-side {
    border-left: 0;
  }
}
</style>
