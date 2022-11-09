<template>
  <div>
    <form method="post" @click.prevent>
      <input type="text" v-model="body.id" placeholder="id" />
      <input type="text" v-model="body.accountId" placeholder="accountId" />
      <input
        type="text"
        v-model="body.arrivingArabicName"
        placeholder="arrivingArabicName"
      />
      <input
        type="text"
        v-model="body.arrivingEnglishName"
        placeholder="arrivingEnglishName"
      />
      <input type="text" v-model="body.sort" placeholder="sort" />
      <input type="submit" value="Add" @click="postApi()" />
    </form>
    <div class="container">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>accountId</th>
            <th>arrivingArabicName</th>
            <th>arrivingEnglishName</th>
            <th>sort</th>
            <th>Update</th>
            <th>Delete</th>
          </tr>
        </thead>
        <tbody v-for="currency in info" :key="currency.id">
          <tr>
            <td>{{ currency.id }}</td>
            <td>{{ currency.accountId }}</td>
            <td>{{ currency.arrivingArabicName }}</td>
            <td>{{ currency.arrivingEnglishName }}</td>
            <td>{{ currency.sort }}</td>
            <td><button @click="PutApi(body)">Update</button></td>
            <td><button @click="delApi(body.id)">Delete</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import setApi from "../utils/setAPi"
const baseUrl = "http://40.127.194.127:777/api/Emergency/GetAllArrivingMethods?first=0&page=0&rows=12";
//! ?id=${this.body.id}&accountId=${this.body.accountId}&arrivingArabicName=${this.body.arrivingArabicName}&arrivingEnglishName=${this.body.arrivingEnglishName}&sort=${this.body.sort}
export default {
  data() {
    return {
      info: null,
      body: {
        id: "",
        accountId: "",
        arrivingArabicName: "",
        arrivingEnglishName: "",
        sort: "",
      },
      
    };
  },
  methods: {
  
    async getApi() {
      await axios
        .get(baseUrl)
        .then((res) => {
          this.info = res.data.data;
        })
        .catch((err) => console.log(err));
    },
    async postApi() {
      
      await axios
        .post(
          `http://40.127.194.127:777/api/Emergency/AddOrUpdateArrivingMethod?id=${this.body.id}&accountId=${this.body.accountId}&arrivingArabicName=${this.body.arrivingArabicName}&arrivingEnglishName=${this.body.arrivingEnglishName}&sort=${this.body.sort}`,
          
        )


        .then((res) => {
          this.body = "";
          setApi(res.data.token)
          this.getApi();
          console.log(res);
        })
        .catch((err) => console.log(err));
      
    },
    async PutApi(body) {
      await axios
        .put(
          `http://40.127.194.127:777/api/Emergency/AddOrUpdateArrivingMethod${body.id}`,
          {
            id: this.body.id,
            accountId: this.body.accountId,
            arrivingArabicName: this.body.arrivingArabicName,
            arrivingEnglishName: this.body.arrivingEnglishName,
            sort: this.body.sort,
          }
        )
        .then((res) => {
          this.getApi();
          this.body = "";
          console.log(res);
        })
        .catch((err) => console.log(err));
    },
    async delApi() {
      await axios
        .delete(
          `http://40.127.194.127:777/api/Emergency/DeleteArrivingMethod`,
          this.body.id
        )
        .then((res) => {
          this.getApi();
          console.log(res);
        })
        .catch((err) => console.log(err));
    },
  },
  mounted() {
    this.getApi();
  },
};
</script>
<style lang="scss">
form {
  display: flex;
  flex-direction: column;
  align-items: center;

  input {
    padding: 10px;
    font-size: 20px;
    margin-bottom: 10px;
    width: 50%;
  }
}
.container {
  max-width: 1000px;
  margin-right: auto;
  margin-left: auto;
  min-height: 100vh;
  table {
    width: 100%;
    border: 1px solid #eeeeee;
    thead {
      background: #f44336;
      color: #fff;
      font-weight: bold;
      th {
        padding: 10px;
      }
    }
    tbody {
      tr {
        padding: 10px;
        background: #eeeeee;
      }
    }
  }
}
</style>
