<template>
  <div class="wrapper">
    <div class="form-check">
      <input type="checkbox" v-model="multiple" class="form-check-input" id="exampleCheck1">
      <label class="form-check-label" for="exampleCheck1">Multiple Sort</label>
    </div>
    <table>
      <thead>
          <draggable
            :list="columns"
            ghost-class="ghost"
            @start="dragging = true"
            @end="dragging = false"
            tag="tr"
          >
            <th v-for="key in columns"
              @contextmenu.prevent="$refs.menu.open"
              @click="sortBy(key)"
              :class="{ active: sortKeyList.findIndex(x=>x.key==key) >= 0 }"
              :key="key">
              {{ key | capitalize }}
              <span class="arrow" :class="sortOrders[key] > 0 ? 'asc' : 'desc'">
              </span>
            </th>
          </draggable>
      </thead>
      <transition name="flip-list">
        <tbody>
          <tr v-for="entry in filteredRows" :key="entry.FlightId">
            <td v-for="key in columns" :key="key">
              {{entry[key]}}
            </td>
          </tr>
        </tbody>
      </transition>
    </table>
  </div>
</template>

<script>
import draggable from 'vuedraggable'
import multiColumnSort from 'multi-column-sort'

export default {
  components: {
    draggable
  },
  props: ['rows','columns','filterKey'],
  data() {
    var sortOrders = {};
    this.columns.forEach((key)=>{
      sortOrders[key] = 1;
    });
    return {
      sortKey: "",
      sortKeyList:[],
      sortOrders: sortOrders,
      dragging: false,
      multiple: false
    }
  },
  computed: {
    filteredRows(){
      var sortKey = this.sortKey;
      var filterKey = this.filterKey && this.filterKey.toLowerCase();
      var order = this.sortOrders[sortKey] || 1;
      var rows = this.rows;

      if (filterKey) {
        rows = rows.filter((row)=>{
          return Object.keys(row).some((key)=>{
            return (
              String(row[key])
                .toLowerCase()
                .indexOf(filterKey) > -1
            );
          });
        });
      }
      const getColumnValue = (column, value) => {
        return value
      }

      const sortParams=[]
      this.sortKeyList.forEach(x=>{
        sortParams.push([ x.key, x.type ])
      })
      console.log(this.sortOrders)

      if (this.sortKey && this.multiple) {
        rows = multiColumnSort(
          rows,
          sortParams,
          getColumnValue
        )
      }else if(this.sortKey && !this.multiple){
        rows = multiColumnSort(
          rows,
          [
            sortParams[0]
          ],
          getColumnValue
        )
      }
      return rows;
    },
    removeMultipleSort(){
      this.multiple == false ? this.sortKeyList.splice(1,this.sortKeyList.length) : null
    }
  },
  filters: {
    capitalize(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  },
  methods: {
    sortBy(key) {
      this.sortKey = key;
      this.sortOrders[key] = this.sortOrders[key] * -1;

      let sortType = this.sortOrders[key]>0 ? 'ASC' : 'DESC'
      let index = this.sortKeyList.findIndex((x => x.key == key));

      if (this.multiple) {
        if (index >= 0) {
        this.sortKeyList[index].type = sortType;
        }else{
          this.sortKeyList.push({ key: key, type: sortType})
        }
      }else{
        this.sortKeyList = [{ key: key, type: sortType }]
      }

    }
  }
}
</script>

<style lang="scss">
@import '~vue-context/src/sass/vue-context';

.wrapper{
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.form-check{
    width: 100%;
    margin-bottom: 15px;
}

table {
  border: 2px solid #000000;
  border-radius: 3px;
  background-color: #fff;
}

th {
  background-color: #181c57;
  color: rgba(255, 255, 255, 0.66);
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

td {
  background-color: #f9f9f9;
}

th,
td {
  min-width: 120px;
  padding: 10px 20px;
}

th.active {
  color: #fff;
}

th.active .arrow {
  opacity: 1;
}

.arrow {
  display: inline-block;
  vertical-align: middle;
  width: 0;
  height: 0;
  margin-left: 5px;
  opacity: 0.66;
}

.arrow.asc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-bottom: 4px solid #fff;
}

.arrow.desc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 4px solid #fff;
}

.flip-list-move {
  transition: transform 1s;
}

.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}

.v-context{
  min-width: auto !important;
}

</style>
