<template>
  <div id="app">

    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Title</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in tableData" :key="item.id">
          <td>{{item.id}}</td>
          <td>{{item.title}}</td>
        </tr>
      </tbody>
    </table>
    <!-- pagination -->
    <pagination :pageSize="pagesize" v-on:getlist="getList" ref="pagination"></pagination>
  </div>
</template>

<script>
import Pagination from './components/Pagination'

export default {
  name: 'App',
  components: {
    Pagination
  },
  data () {
    return {
      pagesize:5,
      totalData:[],
      tableData:[ ],

      list_json:{
        limit: 5,
        offset: 0
      }
    }
  },

  mounted () {
    this.totalData = [
      {id:1, title:"马士基董事长谈惊魂10天：重设4000台服务器/4.5万台PC"},
      {id:2, title:"马士基董事长谈惊魂10天：重设4000台服务器/4.5万台PC"},
      {id:3, title:"马士基董事长谈惊魂10天：重设4000台服务器/4.5万台PC"},
      {id:4, title:"马士基董事长谈惊魂10天：重设4000台服务器/4.5万台PC"},
      {id:5, title:"马士基董事长谈惊魂10天：重设4000台服务器/4.5万台PC"},
      {id:6, title:"马士基董事长谈惊魂10天：重设4000台服务器/4.5万台PC"},
      {id:7, title:"马士基董事长谈惊魂10天：重设4000台服务器/4.5万台PC"},
      {id:8, title:"马士基董事长谈惊魂10天：重设4000台服务器/4.5万台PC"},
      {id:9, title:"马士基董事长谈惊魂10天：重设4000台服务器/4.5万台PC"},
      {id:10, title:"马士基董事长谈惊魂10天：重设4000台服务器/4.5万台PC"},
      {id:11, title:"马士基董事长谈惊魂10天：重设4000台服务器/4.5万台PC"}
    ];


    this.getList();

  },

  methods:{
    getList () {
      const _t = this;
      _t.$refs.pagination.$emit('getOffset', function(offset){
          _t.list_json.offset = offset;
      });

      if(_t.list_json.offset == 0){
          _t.$refs.pagination.$emit('countPage', _t.totalData.length);
      }
      _t.tableData = _t.getData(_t.list_json.limit, _t.list_json.offset);
    },

    getData (limit, offset) {
      let list = [];
      let len = limit + offset;
      
      if(this.totalData.length < len){
        len = this.totalData.length;
      }
      console.log('limit, offset, len', limit, offset, len)
      for(let i = offset; i < len; i++){
        list.push(this.totalData[i]);
      }
    console.log('list',list)
      return list;
    }
  }
}
</script>
