<template>
  <div>
    <header>
      <img src="../assets/logo.png">
    </header>

    <div class="container h-100" style="margin-top: 5%;">
      <div class="d-flex justify-content-center h-100">
        <div v-show="barcodeShow" style="margin-right: 5%">
          <div class="searchbar specs" @mouseover="qrcodeShow=false" @mouseout="outBarCode">
          <input class="search_input" maxlength="45" v-model="barcodeValue" type="text" name="" placeholder="Enter the code">
          <a href="#" class="search_icon"><i class="fas fa-barcode"></i></a>
        </div>
          <p class="code-name" style="margin-left: 8%;" v-if="qrcodeShow==false">Barcode</p>
          <p v-else>Barcode</p>
        </div>

        <div v-show="qrcodeShow">
          <div class="searchbar spec" @mouseover="barcodeShow=false; onBarCode();" @mouseout="outQRCode">
          <input class="search_input" maxlength="45" v-model="qrcodeValue" type="text" name="" placeholder="Enter the code">
          <a href="#" class="search_icon"><i class="fas fa-qrcode"></i></a>
        </div>
        <p class="code-name" v-if="barcodeShow==false">QR Code</p>
        <p v-else>QR Code</p>
        </div>
      </div>

      <barcode id="barcode-img" :value="barcodeValue" v-if="barcodeValue"></barcode>
      <!-- <p class="code-name">{{barcodeValue}}</p>     -->

      <qrcode id="qr-img" :value="qrcodeValue" v-if="qrcodeValue" :options="{ width: 300, height: 300 }" tag="img"></qrcode>
      <p class="code-name">{{qrcodeValue}}</p>

      <button class="gen-btn" @click="saveImg" v-if="qrcodeValue || barcodeValue"> <i class="fas fa-download"></i> Download</button>
      <button class="gen-btn clear" @click="qrcodeValue = ''; barcodeValue = '';" v-if="qrcodeValue || barcodeValue"> Clear</button>
      <!-- qrcodeValue = this.barcodeValue= ''; -->
    </div>

    <footer>
      <p>Designed by Nandar Oise</p>
    </footer>
  </div>
</template>

<script>
import Vue from 'vue';
import VueQrcode from '@chenfengyuan/vue-qrcode';
import VueBarcode from 'vue-barcode';

export default {
  name: 'HelloWorld',
  components: {
    'barcode': VueBarcode,
    'qrcode': VueQrcode
  },
  data () {
    return {
      barcodeValue: '',
      qrcodeValue: '',
      qrcodeShow: true,
      barcodeShow: true,
      genShow: false,
    }
  },
  methods:{
    outQRCode(){
      if(this.qrcodeValue){
        this.barcodeShow=false; 
        $('.spec').addClass('special');
        $('.spec').css('width', '100%');
      }else{
        this.barcodeShow=true; 
        $('.spec').removeClass('special');
      }
    },
    onBarCode(){
      if(this.barcodeShow==false){
        $('.spec').css('width', '100%');
      }else{
        $('.spec').css('width', '90%');
      }
    },
    outBarCode(){
      if(this.barcodeValue){
        this.qrcodeShow=false; 
        $('.specs').addClass('special');
      }else{
        this.qrcodeShow=true; 
        $('.specs').removeClass('special');
      }
    },
    saveImg(){
      var link = document.createElement("a");

      if(this.qrcodeValue){
        let img = $('#qr-img').attr('src');
        link.setAttribute("href", img);
        link.setAttribute("download", this.qrcodeValue);
        link.click()
      }
      else if(this.barcodeValue){
        let canvas = $('.vue-barcode-element')[0]
        // let img = canvas.toDataURL("image/png");
        // console.log(img)
        var context = canvas.getContext("2d");
          context.fillStyle = "green";
          context.fillRect(50, 50, 100, 100);
          // no argument defaults to image/png; image/jpeg, etc also work on some
          // implementations -- image/png is the only one that must be supported per spec.
          window.location = canvas.toDataURL("image/png");
        // link.setAttribute("href", img);
        // link.setAttribute("download", this.barcodeValue);
        // link.click();

        // let context = canvas.getContext('2d')

        // let image = new Image;
        // image.src = "fallback.svg";
        // image.onload = () =>{
        //   context.drawImage(image, 0, 0);

        //   link.download = "fallback.png";
        // };
      }
    }
  },
  mounted(){
    console.clear()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  header{
    background-color: #353b48;    
    display: flex;
    height: 75px; 
    padding: 5px 10px;
  }
  header img{
    width: 30%;
  }
  footer{
    position: fixed;
    bottom: 0px;
    background-color: #f5f5f5;
    width: 100%;
    border-top: 1px solid #eee;
    padding: 0 10px;
  }
  footer p{
    margin-top: 1rem;
    font-size: 120%;
  }

  input::placeholder{
    color: #fff;
    font-weight: 600;
  }
  .searchbar{
    margin-bottom: auto;
    margin-top: auto;
    height: 60px;
    background-color: #353b48;
    border-radius: 30px;
    padding: 10px;
  }

  .search_input{
    color: white;
    border: 0;
    outline: 0;
    background: none;
    width: 0;
    caret-color:transparent;
    line-height: 40px;
    text-align: center;
    transition: width 0.5s linear;
  }

  .searchbar:hover > .search_input{
    padding: 0 10px;
    width: 450px;
    transition: width 0.4s linear;
  }

  .searchbar:hover > .search_icon{
    background: white;
    color: #e74c3c;
  }

  .special > .search_input{
    padding: 0 10px;
    width: 450px;
    transition: width 0.4s linear;
  }

  .special > .search_icon{
    background: white;
    color: #e74c3c;
  }

  .spec{
    width: 90%
  }
  .spec:hover{
    width: 100%;
  }

  .search_icon{
    height: 40px;
    width: 40px;
    float: right;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
    color:white;
  }
  .gen-btn{
    border-radius: 30px;
    padding: 10px;
    border: 2px solid #e74c3c;
    width: 25%;
    background-color: #fff;
    color: #353b48;
  }
  .gen-btn:hover{
    color: #ffffff;
    background-color: #353b48;      
  }
  .gen-btn:focus{
    outline: 0;
  }
  .clear{
    background-color: #e74c3c;
    color: #fff;
  }
  .clear:hover{
    border: 2px solid #353b48;      
  }
  .code-name{
    font-weight: 600;
    font-size: 120%;
  }
  .vue-barcode-element{
    height: 250px;
    width: 100%;
  }
</style>
