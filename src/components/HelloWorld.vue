<template>
  <div class="hello">
		<h3>Porfavor ingrese la liga de enlazado al video o articulo deseado.</h3>
    <div><input id="qr-text" type="text" placeholder="Liga de enlazado para generar QR" v-model="url" /></div>
    <small v-if="urlVacio" style="color:red;">Ingresa una liga de enlaze.</small>
    <br/>
    <div>
        <button class="qr-btn" @click="generateQRCode()">Generar código QR</button>
    </div>
    <br/>
    <p id="qr-result">Código QR generado:</p>
    <canvas v-show="codeGenerated" id="qr-code"></canvas>
    <button style="margin-top:20px" v-show="codeGenerated" class="qr-btn" id="btn-download">Descargar código QR</button> 
  </div>
</template>
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrious/4.0.2/qrious.min.js"></script>
<script>

  export default {
    name: 'HelloWorld',
    'data': function() {
      return {
          "url":"",
          "codeGenerated":false,
          "urlVacio":false
      };
    },
    mounted() {
      var _self = this
      
      var scriptTag = document.createElement("script");
      scriptTag.src = "https://cdnjs.cloudflare.com/ajax/libs/qrious/4.0.2/qrious.min.js";
      scriptTag.id = "my-datatable";
      document.getElementsByTagName('head')[0].appendChild(scriptTag);
      console.log(scriptTag, "script")

      document.getElementById('btn-download').addEventListener("click", function() {
        var canvas = document.querySelector('#qr-code');

        var dataURL = canvas.toDataURL("image/jpeg", 1.0);

        downloadImage(dataURL, 'qr-code.jpeg');
      });
      function downloadImage(data, filename = 'código_QR.jpeg') {
          var a = document.createElement('a');
          a.href = data;
          a.download = filename;
          document.body.appendChild(a);
          a.click();
          _self.codeGenerated = false
          _self.urlVacio = false
          document.getElementById("qr-result").innerHTML = "Código generado:";
      }
    },
    methods:{
      "generateQRCode":function(){
        var _self = this
        if(_self.url.length > 1){
          _self.urlVacio = false
          var qr;
          (function() {
                  qr = new QRious({
                  element: document.getElementById('qr-code'),
                  size: 200,
                  value: _self.url
              });
          })();
          var qrtext = document.getElementById("qr-text").value;
          document.getElementById("qr-result").innerHTML = "Código QR para " + qrtext +":";
          
          qr.set({
              foreground: 'black',
              size: 200,
              value: qrtext
          });
          _self.codeGenerated = true
          _self.url = ""
        } else {
          _self.urlVacio = true
          return false
        }
        
      }
    },

  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  body {
      padding:20px;
  }
  input {
      padding:5px;
      background-color:transparent;
      border:none;
      border-bottom:solid 4px #8c52ff;
      width:250px;
      font-size:16px;
  }
  
  .qr-btn {
      background-color:#8c52ff;
      padding:8px;
      color:white;
      cursor:pointer;
  }

  .hello {
    display: flex;
    flex-direction: column;
    align-items: center;
  }		
</style>
