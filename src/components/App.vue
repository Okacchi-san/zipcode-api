<template>
  <div id="container">
    <form>
      <table>
        <tbody>
          <tr>
            <th>郵便番号</th>
            <td>
              <input id="zipcode" class="zipcode" type="text" name="zipcode" v-model="zipcode" placeholder="例)1110000">
              <button @click="onSearchclicked" id="seach" type="button">住所検索</button>
              <p id="error"></p>
            </td>
          </tr>
          <tr>
            <th>都道府県</th>
            <td>
              <input id="address1" type="text" name="address1" :value="address1">
            </td>
          </tr>
          <tr>
            <th>市町村</th>
            <td>
              <input id="address2" type="text" name="address2" :value="address2">
            </td>
          </tr>
          <tr>
            <th>町域</th>
            <td>
              <input id="address3" type="text" name="address3" :value="address3">
            </td>
          </tr>
        </tbody>
      </table>
    </form>
  </div>  
</template>

<script>
export default{
  data: function(){
    return{
      zipcode: '',
      addressData: {},
      address1: '',
      address2: '',
      address3: '',
    }
  },
  methods: {
    onSearchclicked(){

    //Do: 入力文字のバリデーション
    const pref = (this.zipcode).slice(0,3);
    const city = (this.zipcode).slice(-4);

    console.log(typeof(city))
    const response = fetch(`https://madefor.github.io/postal-code-api/api/v1/${pref}/${city}.json`);
   

    response.then((value) => {
      const json = value.json();
      json.then(v => {
        this.addressData = v.data[0];
        console.log(this.addressData)
        this.address1 = `${this.addressData.ja.prefecture}`;
        this.address2 = `${this.addressData.ja.address1}`;
        this.address3 = `${this.addressData.ja.address2}`;
      })
    })
    }
  }
};
</script>

<style scoped>

#container {
  width: 96%;
  max-width: 500px;
  margin: 40px auto;
  padding: 20px;
  padding: 28px;
  line-height: 1.5;
  border: 1px solid #ffffff;
  border-radius: 5px;
  background-color: transparent;
  box-shadow: 2px 2px 5px 0px rgba(200, 200, 200, 1);
  font-family: "Times New Roman";
}

#error {
  margin: 5px 0 0 0;
  color: #ff0000;
  font-size: .84rem;
  font-weight: bold;
}

table {
  display: table;
  width: 100%;
  border-collapse: collapse;
}   
th {
  width: 80px;
  padding: 20px 10px;
  text-align: left;
  border-bottom: 2px solid #384878;
}
          
td {
  width: calc(100% - 80px);
  padding: 20px 10px;
  border-bottom: 2px solid #cccccc;
}
input {
  padding: 5px;
  border: 0;
  border: 1px solid #cccccc;
}

button {
  padding: 3px 10px;
  color: #ffffff;
  background-color: #00c2bc;
  border: 0;
  box-shadow: none;
  cursor: pointer;
}

.zipcode {
  width: 75px;
}
</style>