<template>
  <div>
      <select v-if="packageObj.length" v-model="selected" class="select-block" multiple>
          <option disabled value="">Select Package</option>
          <option v-for="item in packageObj" class="choose-package">{{ item.name }}</option>
      </select>
      <form action="">
          <Package v-for="(item, index) in choosePackage" :index="index" :item="item" :key="index" :packageObj="packageObj" :choosePackage="choosePackage" :namePackage="namePackage"/>
          <p class="total-price">Tổng tiền: </p>
          <p class="btnSubmit-wrap"><input type="submit" value="Mua hàng" class="btnSubmit"></p>
      </form>
  </div>
</template>

<script>
// import components
import Package from '../components/Package'

// import axios
import axios from 'axios';
const http = axios.create({
    baseURL: 'http://localhost/yii_project_01/prj/web/',
});

export default {
    name: 'Home',
    components: {
        Package,
    },
    data() {
        return {
            packageObj      : [],
            selected        : [],
            choosePackage   : [],
            strSelected     : '',
            totalPrice      : 0,
        };
    },
    mounted() {
        http.get('package/get').then((res)=>{
            this.packageObj = res.data;
            // console.log(this.packageObj);
        });
    },
    computed: {
        namePackage: function() {
            return this.selected.slice(4);
        }
    },
    watch: {
        selected() {
            this.strSelected = this.selected.toString();
            http.get('package/get-where?name-package=' + this.strSelected).then((res)=>{
                this.choosePackage = res.data;
                // console.log(this.choosePackage)
            });
        }
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    form {
        overflow: auto;
    }
    .select-block {
        width: 15%;
        height: 100%;
        position: fixed;
        padding: 4px 15px;
        font-size: 16px;
        color: #777;
        border-radius: 5px;
        outline: none;
        overflow: hidden;
    }
    option:checked {
        background:linear-gradient(to right, hotpink, deeppink, hotpink, pink);
    }
    .btnSubmit-wrap {
        text-align: right;
    }
    .btnSubmit {
        background-color: lightpink;
        border: none;
        border-radius: 3px;
        padding: 3px 10px;
        outline: none;
        font-size: 16px;
    }
    .total-price {
        text-align: right;
    }
</style>
