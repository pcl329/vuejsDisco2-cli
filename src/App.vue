<template>
  <div id="app">
    <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
      <a class="navbar-brand" href="#">{{ appTitre }}</a>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarCollapse"
        aria-controls="navbarCollapse"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarCollapse">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <a class="nav-link" href="#">
              Home
              <span class="sr-only">(current)</span>
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Link</a>
          </li>
        </ul>
      </div>
    </nav>
    <main role="main" class="fluid-container m-4">
      <div class="row">
        <div class="col-8">
          <div class="card mb-3" v-for="record in records" :key="record.id" @click="handleFavAlbum(record.id)">
            <div class="row no-gutters">
              <div class="col-m-4 m-2">
                <img height="150px" :src="hasCoverUrl(record)" alt="..." />
              </div>
              <div class="col-md-8">
                <div class="card-body">
                  <h5 class="card-title">{{ record.artist }}</h5>
                  <p class="card-text">{{ record.title }}</p>
                  <p class="card-text">{{ record.year }}</p>
                  <p class="card-text">
                    <small class="text-muted">{{ record.pitchforkPos }}</small>
                  </p>
                </div>
              </div>
              <div class="col-m-4 m-2">
                <p v-if="favList.includes(record.id)" class="card-text">💖</p>
              </div>
            </div>
            <!-- <div class="card-footer bg-dark">
                      <small class="text-white">{{record.stock}} en Stock </small>
            </div>-->

            <div class="card-footer" :class="record.stock <= 0 ? 'bg-warning' : 'bg-dark'">
              <!-- <button type="button" class="btn btn-warning">[+]</button>
              <button type="button" class="btn btn-warning">[-]</button>-->
              <button type="button" class="btn btn-outline-danger" @click.stop="stockPlus(record)">[+]</button>
              <button type="button" class="btn btn-outline-danger" @click.stop="stockMoins(record)">[-]</button>
              <small class="text-white">{{ record.stock }} en Stock</small>
            </div>
          </div>
        </div>
        <div class="col-4">
          <div class="card bg-light mb-3" style="max-width: 18rem;">
            <div class="card-header">Infos albums{{ records.length }}</div>
            <div class="card-body">
              <h5 class="card-title">Option selec: {{ sortingOption }}</h5>
              <select class="custom-select" v-model="sortingOption" @change="sortBy">
                <option value="default">default</option>
                <option value="1">Pitchfork Pos ⬆</option>
                <option value="2">Pitchfork Pos ⬇</option>
                <option value="3">Année de sortie</option>
              </select>
              <p class="card-text">Some sorting options</p>
              <h5 class="card-title">Filter</h5>

              <div class="custom-control custom-switch">
                <input
                  @change="activeFilter"
                  v-model="isFilterActive"
                  type="checkbox"
                  class="custom-control-input"
                  id="customSwitch1"
                />
                <label class="custom-control-label" for="customSwitch1">Que ceux disponible</label>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import { records } from './assets/static/js/allRecords'
console.log(records)

export default {
  name: 'App',
  components: {},
  data: () => ({
    records,  //👈si le même nom, on peut mettre que records, origin(records: records)     
    appTitre: 'Cci discography',
    sortingOption: 'default',
    isFilterActive: false,
    favList: []
  }),
  methods: {
    hasCoverUrl(record) {
      //ici on va vérifier si la proprièté coverUrl existe
      if (record?.coverUrl) {
        return record.coverUrl
      } else {
        return 'https://via.placeholder.com/100'
      }
    },
    stockPlus(record) {
      record.stock++
    },
    stockMoins(record) {
      record.stock--
    },
    sortBy() {
      console.log('Option de tri > ', this.sortingOption)

      if (this.sortingOption === 'default') {
        return this.records.sort((a, b) => a.pitchforkPos - b.pitchforkPos)
      }

      if (this.sortingOption === '1') {
        return this.records.sort((a, b) => a.pitchforkPos - b.pitchforkPos)
      }

      if (this.sortingOption === '2') {
        return this.records.sort((a, b) => b.pitchforkPos - a.pitchforkPos)
      }

      if (this.sortingOption === '3') {
        return this.records.sort((a, b) => a.year - b.year)
      }
      /*
            if (this.sortingOption === 'posAsc') {
              this.records.sort((a, b) => a.pitchforkPos-b.pitchforkPos)
            }
            if (this.sortingOption === 'posDesc') {
              this.records.sort((a, b) => b.pitchforkPos-a.pitchforkPos)
            }
            if (this.sortingOption === 'by year') {
              this.records.sort((a, b) => a.year-b.year)
            }
            */
    },
    activeFilter() {
      console.log(this.isFilterActive)
      if (this.isFilterActive) {//si la variable data est sur true > on filtre
        this.records = this.records.filter((record) => record.stock >= 1)
      } else { //sinon on recharge le tableau originel
        this.records = records
      }
    },
    handleFavAlbum(recordId) {
      console.log('click sur', recordId)
     // this.favList.push(recordId)
     if(this.favList.includes(recordId)){
       const pos =this.favList.indexOf(recordId)
       this.favList.splice(pos, 1)
     }else{
       this.favList.push(recordId)
     }
    },
  }
}     
</script>

<style lang="scss">
@import "./assets/navbar-top-fixed.css";
@import "../node_modules/bootstrap/scss/bootstrap.scss";
</style>
