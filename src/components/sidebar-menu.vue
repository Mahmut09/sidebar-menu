<template>
  <div class="sidebar">
    <nav class="sidebar-nav">
      <div class="sidebar-logo">
        <img src="/img/logo.png" alt="Logo sidebar">
      </div>
      <ul>
        <li v-for="item in menu" :key="item.id" @mouseover="selectParent(item)" @mouseleave="selectParent(null)">
          <router-link :to="'/' + item.id">
            <div class="parent-icon">
              <img :src="getParentImage(item)">
            </div>
            <p>{{ item.name }}</p>
            <div v-if="item.children && item.children.length" class="arrow-icon">
              <img src="/img/next.png" alt="">
            </div>
          </router-link>
          <div v-if="isSelectedParent(item)" class="children-menu">
            <ul>
              <li v-for="child in item.children" :key="child.id" @mouseover="selectSubmenu(child)" @mouseleave="selectSubmenu(null)">
                <router-link :to="'/' + child.id">
                  <div class="parent-icon">
                    <img :src="getParentImage(child)">
                  </div>
                  <p>{{ child.name }}</p>
                  <div v-if="child.children && child.children.length" class="arrow-icon">
                    <img src="/img/next.png" alt="">
                  </div>
                </router-link>
                <div v-if="isSelectedSubmenu(child)" class="children-menu">
                  <ul>
                    <li v-for="subitem in child.children" :key="subitem.id">
                      <router-link :to="'/' + subitem.id">
                        <p>{{ subitem.name }}</p>
                      </router-link>
                    </li>
                  </ul>
                </div>
              </li>
            </ul>
          </div>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  data() {
    return {
      menu: null,
      selectedParent: null,
      selectedSubmenu: null,
    }
  },
  methods: {
    async fetchData() {
      try {
        const response = await fetch('/api/category_response.json'); 
        if (!response.ok) {
          throw new Error('Не удалось выполнить запрос');
        }
        this.menu = await response.json();
        console.log(this.menu);
      } catch (error) {
        console.error('Произошла ошибка при загрузке данных:', error);
      }
    },
    getParentImage(item) {
      if (item.categoryImages && item.categoryImages.image) {
        return item.categoryImages.image.fileUrl;
      }
      return '/img/default-img.png'; 
    },
    selectParent(parent) {
      this.selectedParent = parent;
    },
    selectSubmenu(submenu) {
      this.selectedSubmenu = submenu;
      console.log(this.selectedSubmenu)
    },
    isSelectedParent(item) {
      return this.selectedParent === item;
    },
    isSelectedSubmenu(subitem) {
      console.log(subitem, 'Good')
      return this.selectedSubmenu === subitem;
    },
  },
  mounted() {
    this.fetchData()
  }
}
</script>

<style lang="scss">
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  max-width: 370px;
  border-right: 1px solid #eeee;
  height: 100vh;
  width: 100%;
}

.sidebar-logo {
  max-width: 210px;
  margin: 30px auto 30px auto;
  img {
    width: 100%;
  }
}

.sidebar-nav {
  max-height: 100%;
  height: 100%;
  overflow-y: auto;
  margin-bottom: 160px;
  display: block;
  &>ul {
    margin: 30px 0;
    &>li {
      list-style: none;
      display: flex;
      padding-right: 20px;
      &:not(:last-child) {
        margin-bottom: 10px;
      }
      &>a {
        color: #424242;
        display: block;
        padding: 15px 20px;
        font-family: sans-serif;
        text-decoration: none;
        font-size: 16px;
        font-weight: 500;
        background-color: #cecccc23;
        border-radius: 5px;
        border: 1px solid #a0a0a018;
        display: flex;
        align-items: center;
        justify-content: flex-start;
        position: relative;
        width: 100%;
        transition: all .3s ease;
        &:hover {
          background-color: #428f3438;
        }
        p {
          margin: 0;
          max-width: 220px;
        }
      }
    }
  }
}

.children-menu {
  position: absolute;
  top: 0;
  left: calc(100% - 10px);
  height: 100%;
  width: 350px;
  border-right: 1px solid #eeee;
  display: block;
  &>ul {
    max-height: 97%;
    padding-top: 3%;
    height: 100%;
    overflow-y: auto;
    padding-left: 30px;
    &>li {
      list-style: none;
      display: flex;
      padding-right: 20px;
      &:not(:last-child) {
        margin-bottom: 10px;
      }
      &>a {
        color: #424242;
        display: block;
        padding: 15px 20px;
        font-family: sans-serif;
        text-decoration: none;
        font-size: 16px;
        font-weight: 500;
        background-color: #cecccc23;
        border-radius: 5px;
        border: 1px solid #a0a0a018;
        display: flex;
        align-items: center;
        justify-content: flex-start;
        position: relative;
        width: 100%;
        transition: all .3s ease;
        &:hover {
          background-color: #428f3438;
        }
        p {
          margin: 0;
          max-width: 160px;
        }
      }
    }
  }
}

.parent-icon {
  max-width: 30px;
  height: 30px;
  margin-right: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  img {
    width: 100%;
    height: 100%;
  }
}

.arrow-icon {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  right: 20px;
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  img {
    width: 100%;
    height: 100%;
  }
}

.sidebar-nav::-webkit-scrollbar-track,
.children-menu ul::-webkit-scrollbar-track
{
  -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
  border-radius: 10px;
  background-color: #F5F5F5;
}

.sidebar-nav::-webkit-scrollbar,
.children-menu ul::-webkit-scrollbar
{
  width: 6px;
  background-color: #F5F5F5;
}

.sidebar-nav::-webkit-scrollbar-thumb,
.children-menu ul::-webkit-scrollbar-thumb
{
  border-radius: 10px;
  -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);
  background-color: #428f3480;
}
</style>