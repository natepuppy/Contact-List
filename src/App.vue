<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-sm-8">
        <h1>Contacts</h1>
        <ul class="list-group">
        <!-- This sections list each contact that is saved in local storage -->
          <li v-for="(item, id) in contactList" :key=id class="list-group-item">
            <div class="row">
              <div class="col-sm-8">
                <h3>{{ item.name }}</h3>
                <h6>Address: {{ item.address }} </h6><br> 
                <h6>Email: {{ item.email }} </h6><br>
                <h6>Number: {{ item.number }} </h6><br> 
                <h6>Category: {{ item.category }} </h6>
              </div>
              <!-- Update and delete buttons -->
              <div class="col-sm-4">
                <button v-on:click="delete1(item.id)" class="btn btn-secondary">Delete</button>  
                <button v-on:click="update(item.id)" class="btn btn-secondary">Update</button>
              </div>
            </div>
            <br>

            <!-- This is section shows if the user wants to update a specific user, else it is hidden -->
            <div v-if="item.show">
              <input type="text" v-model="item.name" placeholder="Name" class="form-control">
              <input type="text" v-model="item.address" placeholder="Address" class="form-control">
              <input type="text" v-model="item.email" placeholder="Email" class="form-control">
              <input type="text" v-model="item.number" placeholder="Phone Number" class="form-control">
              <input type="text" v-model="item.category" placeholder="Category" class="form-control">
              <br>
              <button v-on:click="item.show=false" class="btn btn-primary">Done</button>
            </div>
          </li>
        </ul>
      </div>

      <!-- This is the side area where you can adda new contacts -->
      <div class="col-sm-4">
        <div class="row">
          <form>
            <div class="form-group">
              <input v-model="newContact.name" class="form-control" placeholder="Name">
            </div>
            <div class="form-group">
              <input v-model="newContact.address" class="form-control" placeholder="Address">
            </div>
            <div class="form-group">
              <input v-model="newContact.email" class="form-control" placeholder="Email">
            </div>
            <div class="form-group">
              <input v-model="newContact.number" class="form-control" placeholder="Phone Number">
            </div>
            <div class="form-group">
              <input v-model="newContact.category" class="form-control" placeholder="Category">
            </div>
            <button v-on:click="addContact(newContact)" type="button" class="btn btn-primary">Add New Contact</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: 'app',
  data () {
    return {
      contactList: this.getContactList(),
      newContact: {
        id: "",
        name: "",
        address: "",
        number: "",
        email: "",
        category: "",
        show: false,
      }
    }
  },

  methods: {
    // Updates a specific contact based on the id that it recieves
    update(id) {
      var list = this.contactList;
      var contactToUpdate;
      list.forEach(function (item, index) {
        if (item.id === id) {
          contactToUpdate = list[index];
        }
      });
      contactToUpdate.show = true;

      this.postContactList();
    },

    // Deletes a specific contact based on the id that it recieves
    delete1(id) {
      var list = this.contactList;
      list.forEach(function (item, index) {
        if (item.id === id) {
          list.splice(index, 1);
        }
      });

      this.postContactList();
    },

    // Adds the contact that is sent to it to localstorage
    addContact(contact) {
      this.contactList.push(this.duplicateContact(contact));
      this.postContactList();
      this.resetContactInfo();
    },

    // get the contact list from localstorage
    getContactList() {
      return JSON.parse(window.localStorage.getItem('contactList'));
    },

    // update the contact list in localstorage
    postContactList() {
      localStorage.setItem('contactList', JSON.stringify(this.contactList));
    },

    // Set all values of "newContact" to blank
    resetContactInfo() {
      this.newContact.id = "";
      this.newContact.name = "";
      this.newContact.address = "";
      this.newContact.number = "";
      this.newContact.email = "";
      this.newContact.category = "";
      this.newContact.show = false;
    },

    // Make a hard copy of the contact
    duplicateContact(contact) {
      var personID = contact.name + contact.address;
      var duplicateContact = {
        id: "",
        name: "",
        address: "",
        number: "",
        email: "",
        category: "",
        show: false,
      };

      duplicateContact.id = personID;
      duplicateContact.name = contact.name;
      duplicateContact.address = contact.address;
      duplicateContact.number = contact.number;
      duplicateContact.email = contact.email;
      duplicateContact.category = contact.category;
      duplicateContact.show = contact.show;

      return duplicateContact;
    }
  }
}
</script>


<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>