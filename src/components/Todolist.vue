<template>
  <div id="app">
    <h1>ToDoリスト2020</h1>
    <form>
      <el-input
        placeholder="タスク内容を入力して下さい"
        v-model="newName"
        maxlength="20"
        size="large"
      ></el-input>
      <el-date-picker
        id="newDeadline"
        v-model="newDeadline"
        type="datetime"
        placeholder="日時を選択してください"
      ></el-date-picker>
      <el-button type="primary" round v-on:click="onadd">追加</el-button>
    </form>
    <table class="table">
      <th width="120">進捗状況</th>
      <th width="360">タスク内容</th>
      <th width="200">期限</th>
      <th width="50"></th>
      <th width="50"></th>
      <tr v-for="(item, ind) in items" v-bind:key="item.id">
        <td v-if="item.status === false">
          <img src="../assets/exer_notyet.gif" />
        </td>
        <td v-else><img src="../assets/exer_yet_upload.gif" /></td>
        <td>{{ item.name }}</td>
        <td>{{ item.deadline.toLocaleString() }}</td>
        <td>
          <el-button
            type="success"
            circle
            icon="el-icon-check"
            v-on:click="onfinish(ind)"
          ></el-button>
        </td>
        <td>
          <el-button
            type="danger"
            circle
            icon="el-icon-delete"
            v-on:click="ondelete(ind)"
          ></el-button>
        </td>
      </tr>
    </table>
  </div>
</template>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.11/lodash.min.js"></script>
<script src="https://unpkg.com/element-ui/lib/index.js"></script>
<script>
export default {
  data() {
    return {
      newId: 2,
      newName: '',
      newDeadline: '',
      items: [
        {
          id: 0,
          status: false,
          name: 'なろう講習会†完全に理解†する',
          deadline: '',
        },
        {
          id: 1,
          status: true,
          name: 'ToDoリスト移植する',
          deadline: '',
        },
      ],
    };
  },
  methods: {
    onadd: function() {
      if (this.newName === '') {
        window.alert('タスク内容の入力は必須です！！！');
      } else if (this.newDeadline === '') {
        window.alert('期限日時の選択は必須です！！！');
      } else {
        this.items.unshift({
          id: this.newId,
          status: false,
          name: this.newName,
          deadline: this.newDeadline,
        });
        this.onsort();
        this.newId++;
        this.newName = '';
        this.newDeadline = '';
      }
    },
    onsort: function() {
      this.items.sort(function(a, b) {
        if (a.status > b.status) {
          return 1;
        } else if (a.status < b.status) {
          return -1;
        } else if (Date.parse(a.deadline) < Date.parse(b.deadline)) {
          return -1;
        } else {
          return 1;
        }
      });
    },
    onfinish: function(ind) {
      Vue.set(this.items[ind], 'status', true);
      this.onsort();
    },
    ondelete: function(ind) {
      this.items.splice(ind, 1);
      this.onsort();
    },
  },
};
</script>

<style scoped>
[v-cloak] {
  display: none;
}
.panel {
  border: 1px solid #000;
  width: 350px;
  overflow: hidden;
}
.v-enter-active,
.v-leave-active,
.v-move {
  transition: transform 1s;
}
.v-enter,
.v-leave-to {
  transform: translateX(80%);
}
.v-leave-active {
  position: absolute;
}
.warning {
  color: yellow;
}
</style>
