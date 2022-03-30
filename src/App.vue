<template>
  <div id="app">
    <header class='general-header'>
      <img src="@/assets/imgs/logo-text.png" alt="">
      <menu>
        <li v-for="item in menuItems" :key="item.name" :class="{active: item.active}" @click="activeLinckMenu(item)">
          {{ item.name }}
        </li>
      </menu>
    </header>
    <main>
      <section
        v-if="menuItems.form.active"
      >
        <div class="person-data">
          <h1>
            Персональные данные
          </h1>
          <label for="name" class="person-data-section">
            <h3>
              Имя
            </h3>
            <input
              v-model="newPerson.name"
              id="name"
              type="text"
            >
          </label>
          <label for='age' class="person-data-section">
            <h3>Возраст</h3>
            <input
              v-model.number="newPerson.age"
              id="age"
              type="text"
            >
          </label>
        </div>

        <div  class="children">
          <h2>
            <span>Дети (макс. 5)</span>
            <button 
              v-if="countChildren"
              @click="openWindowNewChildren"
            >
              + Добавить ребенка
            </button>
          </h2>
          <div
            v-if="flagNewChildren"
            class="new-children"
          >
            <div class="block-children">
              <label class="person-data-section">
                <h3>
                  Имя
                </h3>
                <input
                  v-model="newChildren.name"
                  type="text"
                >
              </label>
              <label class="person-data-section">
                <h3>Возраст</h3>
                <input
                  v-model.number="newChildren.age"
                  type="text"
                >
              </label>
            </div>            
            <div class="block-button">
              <button
                @click="addChildren"
                class="add"
              >
                Добавить
              </button>
              <button
                @click="clearNewChildren"
              >Отмена</button>
            </div>
          </div>
          <ol 
            v-if="newPerson.children.length"
            class="list-children"
          >
            <li
              v-for="(item, i) in newPerson.children"
              :key="item.name + i"
              class="block-children"
            >
              <div class="person-data-section">
                <h3>
                  Имя
                </h3>
                <p>
                  {{item.name}}
                </p>
              </div> 
              <div class="person-data-section">
                <h3>
                  Возраст
                </h3>
                <p>
                  {{item.age}}
                </p>
              </div> 
              
              <button 
                @click="removeChild(i)"
                class="remove"
              >
                удвлить
              </button>
            </li>

          </ol>
        </div>
        
        
        <button 
            @click="addPerson"
          >
            Сохранить
          </button>
      </section>
      <section
        v-else
      >
      {{person}}
      </section>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      menuItems: {
        form: {
          name: 'Форма',
          active: true
        },
        personData: {
          name: 'Превью',
          active: false
        }
      },

      person: null,

      newPerson: {
        name: null,
        age: null,
        children: []
      },

      newChildren: {
        name: null,
        age: null,
      },

      flagNewChildren: false
    }
  },
  computed: {
    countChildren() {
      return this.newPerson.children.length < 5 ? true : false
    }
  },
  methods: {
    activeLinckMenu(item) {
      const mi = this.menuItems
      for (let prop in mi){
        if (mi[prop].name === item.name) {
          mi[prop].active = true
        } else {
          mi[prop].active = false
        }
      }
    },
    openWindowNewChildren() {
      this.flagNewChildren = true;
    },
    addChildren() {
      for (const prop in this.newChildren){
        if (!this.newChildren[prop]){
          alert('заполните все поля!');
          return
        }
      }
      console.log({ ...this.newChildren})
      this.newPerson.children.push({ ...this.newChildren})
      this.clearNewChildren()
    },
    clearNewChildren() {
      for (const prop in this.newChildren){
        this.newChildren[prop] = null
      }
      this.flagNewChildren = false;
    },
    removeChild(i){
      this.newPerson.children = this.newPerson.children.filter((el, index) => i !== index)
    },
    addPerson(){
      this.person = { ... this.newPerson }
      for (const prop in this.newPerson) {
        if (!this.newPerson[prop]) {
          alert('заполните все поля!');
          this.person = null;
          return
        }
        if (prop === 'children') {
          this.newPerson[prop] = [];
        } else {
          this.newPerson[prop] = null
        }
      }

      for(const prop in this.menuItems) {
        if (prop === 'personData'){
          this.menuItems[prop].active = true
        } else {
          this.menuItems[prop].active = false
        }
      }
    }
  }
}
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;700&display=swap');
  * {
    margin: 0;
    padding: 0;
  }
  li {
    list-style-type: none;
  }
  button {
    border: none;
    background-color: inherit;
    cursor: pointer;
  }
  h1, h2 {
    font-size: 16px;
    font-weight: 500;
  }

  body {
    font-family: 'Montserrat', sans-serif;
    font-size: 14px;
    font-weight: 400;
  }

  .general-header {
    display: flex;
    align-items: center;
    box-shadow: 0px 1px 0px rgb(17 17 17 / 10%);
    padding: 23px 267px 23px  92px;

    menu {
      margin: 0 auto;
      display: flex;

      li {
        cursor: pointer;

        &.active {
          color: rgba(1, 167, 253, 1)
        }

        &:last-child {
          margin-left: 24px;
        }
      }
    }    
  }

  main {
    width: 616px;
    margin: 0 auto;

    .person-data{
      padding: 30px 0 40px;

      h1 {
        margin-bottom: 20px;
      }

      .person-data-section:last-child {
        margin-top: 10px;
      }      
    }

    .person-data-section {
        border: 1px solid #F1F1F1;
        border-radius: 4px;
        display: block;
        padding: 10px 16px;

        h3 {
          font-size: 13px;
          font-weight: 400;
          margin-bottom: 5px;
        }

        input {
          border: none;
          outline: none;
        }
      }

    .children {
      h2 {
        display: flex;
        justify-content: space-between;

        button {
          color: rgba(1, 167, 253, 1);
          padding: 5px 15px;
          border-radius: 21px;
          border: solid 1px currentColor;
        }
      }

      .new-children {
        margin: 20px 0;

        .person-data-section {
          width: 42%;
        }

        .block-button {
          display: flex;
          justify-content: flex-end;
          margin: 20px 0;

          button {
            margin-left: 15px;
          }          
        }
      } 

      .block-children {
        display: flex;
        justify-content: space-between;
        margin-top: 10px;
      }

      .list-children {
        .person-data-section {
          width: 35%;
          margin-right: 5%;
        }
      }

      .remove, .add {
        color: rgba(1, 167, 253, 1);
      }
    }
  }
</style>
