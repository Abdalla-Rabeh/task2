<template>
  <div>
    <form method="post" @click.prevent>
      <input type="text" v-model="id" placeholder="id" />
      <input type="text" v-model="accountId" placeholder="accountId" />
      <input
        type="text"
        v-model="arrivingArabicName"
        placeholder="arrivingArabicName"
      />
      <input
        type="text"
        v-model="arrivingEnglishName"
        placeholder="arrivingEnglishName"
      />
      <input type="text" v-model="sort" placeholder="sort" />
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
        <tbody v-for="Data in info" :key="Data.id">
          <tr>
            <td>{{ Data.id }}</td>
            <td>{{ Data.accountId }}</td>
            <td>{{ Data.arrivingArabicName }}</td>
            <td>{{ Data.arrivingEnglishName }}</td>
            <td>{{ Data.sort }}</td>
            <td><button @click="PutApi()">Update</button></td>
            <td><button @click="delApi(body.id)">Delete</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import axios from "axios";
const baseUrl =
  "http://40.127.194.127:777/api/Emergency/GetAllArrivingMethods?first=0&page=0&rows=12";
export default {
  data() {
    return {
      info: null,
      id: 0,
      accountId: 1,
      arrivingArabicName: "",
      arrivingEnglishName: "",
      sort: 0,
      err: false,
      body: [],
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
      let body = {
        id: parseInt(this.id),
        accountId: this.accountId,
        arrivingArabicName: this.arrivingArabicName,
        arrivingEnglishName: this.arrivingEnglishName,
        sort: parseInt(this.sort),
      };
      console.log(body);
      await axios
        .post(
          "http://40.127.194.127:777/api/Emergency/AddOrUpdateArrivingMethod",
          body
        )
        .then((res) => {
          console.log(res);
          this.body.push(this.info);
          const fullData = [...this.info , body];
          console.log(fullData);
          this.id ='',
          this.arrivingArabicName = '',
          this.arrivingEnglishName = '',
          this.getApi()
        })
        .catch(() => {
          this.err = true;
        });
    },
    async PutApi() {
      await axios
        .put(
          `http://40.127.194.127:777/api/Emergency/AddOrUpdateArrivingMethod${this.id}`,
          {
            accountId: this.body.accountId,
            arrivingArabicName: this.body.arrivingArabicName,
            arrivingEnglishName: this.body.arrivingEnglishName,
            sort: this.body.sort,
          },
          {headers: 'Access-Control-Allow-Origin : *'},
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
          this.id
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
