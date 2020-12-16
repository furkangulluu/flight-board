<template>
  <div class="wrapper">
    <table>
      <thead>
        <tr>
          <th v-for="key in columns"
            @click="sortBy(key)"
            :class="{ active: sortKey == key }"
            :key="key">
            {{ key | capitalize }}
            <span class="arrow" :class="sortOrders[key] > 0 ? 'asc' : 'desc'">
            </span>
          </th>
        </tr>
      </thead>
      <transition name="flip-list">
        <draggable
          :list="filteredRows"
          ghost-class="ghost"
          @start="dragging = true"
          @end="dragging = false"
          tag="tbody"
        >
          <tr v-for="entry in filteredRows" :key="entry.FlightId">
            <td v-for="key in columns" :key="key">
              {{entry[key]}}
            </td>
          </tr>
        </draggable>
      </transition>
    </table>
  </div>
</template>

<script>
import draggable from 'vuedraggable'

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
      sortOrders: sortOrders,
      dragging: false
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
      if (sortKey) {
        rows = rows.slice().sort((a, b)=>{
          a = a[sortKey];
          b = b[sortKey];
          return (a === b ? 0 : a > b ? 1 : -1) * order;
        });
      }
      return rows;
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
      console.log("SORTBY",this.sortOrders,key)
    }
  }
}
</script>

<style lang="scss">
.wrapper{
  display: flex;
  justify-content: center;
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
  cursor: all-scroll;
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

</style>
