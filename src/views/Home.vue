<template>
  <div class="home">
    <MainMenu />
    <grid
      :draggable="true"
      :sortable="true"
      :items="visibleItems"
      :cellHeight="icoCfg.boxSize"
      :cellWidth="icoCfg.boxSize"
      @sort="onSort"
      >
      <template slot="cell" scope="props">
          <div class="v-grid-item" :style="[icoCfg.itemCSS, props.item.itemCSS]">
            <div class="iconWrapper" :style="icoCfg.iconWrapperCSS">
              <v-icon :name="props.item.icon" class="iconSvg" scale="1" label="Forked Repository" :style="icoCfg.iconCSS" />
            </div>
            <div :style="[icoCfg.labelCSS, props.item.labelCSS]">
              <span>{{props.item.label}} {{props.index}}/{{props.sort}}</span>
            </div>
          </div>
      </template>
    </grid>
  </div>
</template>

<style>
.v-grid-item {
  width: 100%;
  height: 100%;
  transform: scale(0.9);
  transition: all 300ms;
  overflow: hidden;
}
.v-grid-item:hover {
  transform: scale(1);
  transition: all 300ms;
}
.v-grid-item:hover svg {
  transform: scale(1.2);
  transition: all 300ms;
}
.iconWrapper {
  display: flex;
  align-items: center;
  height: 50%;
}
.iconSvg {
  margin: 0 auto;
  display: block;
  transition: all 300ms;
}
</style>

<script>

import MainMenu from '@/components/MainMenu.vue'

export default {
  data () {
    return {
      icoCfg: {
        boxSize: 128, //input = size
        itemCSS: {
          backgroundColor: "#444",
          color: "#DDD",
          lineHeight: '128px', //= size
          borderRadius: "8px" //input = gutter
        },
        iconWrapperCSS: {
          height: "96px" //input = split
        },
        labelCSS: {
          lineHeight: "32px", //= split
          backgroundColor: "rgba(0,0,0,0.3)", //input = labelStyle
          fontWeight: "800",
          textTransform: "uppercase"
        },
        iconCSS: {
          height: "64px", //input = gutterIcon(<split)
          width: "auto",
          maxWidth: "80%",
          maxHeight: "80%" //for "safety" reason, avoid icons getting big as container
        }
        //calcolare inner da size/padding
        //generare proprietà css da int con u.m. separata
      },
      visibleItemsOrder: [],
      visibleItems: [],
      items: [
        {
          sort: 0,
          label: 'Netflix',
          icon: 'netflix',
          tag: 'video',
          itemCSS: {
              backgroundColor: '#221f1f',
              color: '#e50914'
          },
          labelCSS: {
              color: '#f5f5f1'
          }
        },
        {
          sort: 1,
          label: 'Spotify',
          icon: 'brands/spotify',
          tag: 'music',
          itemCSS: {
            backgroundColor: '#1db954',
            color: '#191414'
          },
          labelCSS: {
            backgroundColor: '#191414',
            color: '#FFFFFF'
          }
        },
        {
          sort: 2,
          label: 'Youtube',
          tag: 'video',
          icon: 'brands/youtube',
          itemCSS: {
            backgroundColor: '#fff',
            color: '#ff0000'
          },
          labelCSS: {
            color: '#282828'
          }
        },
      ]
    }
  },
  components: {
    MainMenu
  },
  beforeMount: function() {
    this.visibleItems = this.items;
  },
  mounted: function() {
    this.visibleItems.sort(function(a, b) { 
        return a.sort - b.sort;
    });
    //this.removeItem(1,2);
    //this.filterItems("video");
  },
  methods: {
    filterItems: function(tag) {
      this.visibleItems = this.items.filter(function(value, index, arr){
          return value.tag.includes(tag);
      });
    },
    removeItem: function(which,howmany=1) {
      this.visibleItems.splice(which,howmany);
    },
    onSort: function(event) {
      var self = this;
      event.items.map(function(current, index, arr){
          self.items[current.index].sort = current.sort;
      });
      //now save it
    }
  }
}
</script>
