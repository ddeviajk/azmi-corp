<template>
<div class="container">
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
            <ul class="breadcrumb">
              <li>Home</li>
              <li><router-link :to="{name: 'IndexTransaksiKas'}">Transaksi Kas</router-link></li>
              <li class="active">Tambah Transaksi Kas</li>
            </ul>
            <div class="panel panel-default">
                <div class="panel-heading">Tambah Transaksi Kas</div>

                <div class="panel-body">
                    <form v-on:submit.prevent="saveForm()" class="form-horizontal" >
                      <div class="form-group">
                        <label for="name" class="col-md-2 control-label" >Dari Akun</label>
                        <div class="col-md-4">
                        <select
                          v-model="transaksiKas.akun_keluar" 
                          class="form-control" 
                          required="" >
                            <option value="">Pilih Akun</option>
                            <option v-for="akun in akuns" :value="akun.id">{{ akun.nama}}</option>
                          </select>
                        <span v-if="errors.akun_keluar" class="label label-danger"> {{ errors.akun_keluar[0]}}</span>
                        </div>
                      </div>
                      <div class="form-group">
                        <label for="name" class="col-md-2 control-label" >Ke Akun</label>
                        <div class="col-md-4">
                        <select v-model="transaksiKas.akun_masuk" class="form-control" required="" >
                            <option value="">Pilih Akun</option>
                            <option v-for="akun in akuns" :value="akun.id">{{ akun.nama}}</option>
                          </select>
                        <span v-if="errors.akun_masuk" class="label label-danger"> {{ errors.akun_masuk[0]}}</span>
                        </div>
                      </div>
                      <div class="form-group">
                        <label for="name" class="col-md-2 control-label" >Jumlah</label>
                        <div class="col-md-4">
                          <input type="text" class="form-control" required="" placeholder="Nama Akun" v-model="transaksiKas.jumlah" autofocus=""/>
                        <span v-if="errors.jumlah" class="label label-danger"> {{ errors.jumlah[0]}}</span>
                        </div>
                      </div>
                      <div class="form-group">
                        <div class="col-md-4 col-md-offset-2">
                          <button class="btn btn-primary" type="submit">Submit</button>
                        </div>
                      </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
</template>


<script>
  export default {
    data: function() {
      return {
        transaksiKas: {
          akun_masuk: '',
          akun_keluar: '',
          jumlah: ''
        },
        akuns: [],
        url: window.location.origin + (window.location.pathname).replace("home","akun"),
        errors: [],
        message: ''
      }
    },
    mounted()  {
     var app = this;
     app.getAkuns();
    },
    methods: {
      alert(pesan){
        this.$swal({
          title: "Berhasil Menambah Akun",
          text: pesan,
          icon: "success"
        });
      },
      saveForm(){
        var newAkun = this.akun;
        axios.post(this.url,newAkun)
        .then((resp) => {
          this.alert('Berhasil Menambah Akun ' + this.akun.nama );
          this.$router.replace('/akun/');
        })
        .catch((resp) =>{
          if(resp.response.status == 500) alert('Something Goes Wrong');
          this.errors = resp.response.data.errors;
          console.log(resp);
        });
      },
      getAkuns(){ 
        var app = this;
        axios.get(app.url.replace("transaksi-kas","akun") + '/all')
        .then(function(resp){
          app.akuns = resp.data;
        })
        .catch(function(resp){
          console.log(resp); 
        })
      }
    }
  }

</script>
