<template>
  <b-container class="pt-10" >
    <b-row>
      <b-col sm="4" >
        <b-card header="Cari Zodiak" header-tag="header" >
              <b-alert v-model="alert.showDismissibleAlert" variant="danger" dismissible>
                {{alert.message}}
              </b-alert>
              <form>
              <div class="form-group">
                <label for="exampleInputEmail1">Nama Kamu</label>
                <input type="text" class="form-control"  v-model="user.nama" id="name" placeholder="Nama">
              </div>
              <div class="form-group">
                <label for="exampleInputPassword1">Tanggal Lahir Kamu</label>
                <input type="date" class="form-control" v-model="user.tanggal"  id="date" placeholder="Tanggal Lahir">
              </div>
              <button type="button" @click="getZodiak" class="btn btn-primary">Kirim</button>
            </form>
        </b-card>
      </b-col>

      <b-col sm="8" >

        <b-card header="Zodiak Kamu" v-if="resultShowed" header-tag="header" >

          <div class="img" style="display: flex; justify-content: center;">
            <b-img-lazy :src="gambarzodiak" :alt="result.zodiak" rounded class="mb-5 text-center" ></b-img-lazy>
          </div>
          
          <b-list-group>
            <b-list-group-item>
              <font-awesome-icon icon="user" style="color: #017c89;" /> {{result.nama}}
            </b-list-group-item>
            <b-list-group-item>
              <font-awesome-icon icon="baby-carriage" style="color: #017c89;" /> {{result.lahir}}
            </b-list-group-item>
            <b-list-group-item>
              <font-awesome-icon icon="user-clock" style="color: #017c89;" /> {{result.usia}}
            </b-list-group-item>
            <b-list-group-item>
              <font-awesome-icon icon="birthday-cake" style="color: #017c89;" /> {{result.ultah}}
            </b-list-group-item>
            <b-list-group-item>
              <font-awesome-icon icon="horse-head" style="color: #017c89;" /> {{result.zodiak}}
            </b-list-group-item>
          </b-list-group>
          
        </b-card>
        
      </b-col>

    </b-row>
  </b-container>
</template>

<script>

export default {
  data(){
    return {
      user : {
        nama: "",
        tanggal: ''
      },
      alert: {
        showDismissibleAlert: false,
        message: ''
      },
      resultShowed: false,
      result: {},
      gambarzodiak: ''
    }
  },
  methods:{
    getZodiak(e) {
      e.preventDefault()
      if( this.user.nama.trim().length < 1 ){
        this.showAlert("Input Nama Kamu dahulu");
        return false;
      }
      if( this.user.tanggal.trim().length < 1 ){
        this.showAlert("Tanggal Harus Di isi");
        return false;
      }
      const url = process.env.ZODIAKURL + "?service=" + process.env.APIKEY
      this.$axios.get(url + "&nama=" + this.user.nama + "&tanggal=" + this.user.tanggal  )
        .then(resp => {
          if(resp.data.status == "success"){
            this.showResult(resp.data.data)
            this.showImage(resp.data.data.zodiak)
            return true;
          }
          this.showAlert("Terjadi Kesalahan")
        })
    },
    showAlert(message){
      this.alert = {
        showDismissibleAlert: true,
        message: message
      }
    },
    showResult(data){
      this.result = data
      this.resultShowed = true
    },
    showImage(search){
      this.$axios.get(`https://app.zenserp.com/api/v2/search?q=${search}&hl=id&gl=ID&location=United%20States&search_engine=google.com&tbm=isch&num=10&apikey=b5365de0-170e-11ea-b9f4-a139c25d4256`).then(resp => {
          this.gambarzodiak = resp.data.image_results[0].thumbnail
        })
    }
  }
}
</script>

<style>

</style>
