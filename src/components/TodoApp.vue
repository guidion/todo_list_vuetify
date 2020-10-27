<template>
  <v-container>
    <v-row class="text-center">
      <v-col class="mb-4">
        <v-card>

            <v-toolbar color="blue darken-4" dark>
              <v-toolbar-title class="headline">Todo App</v-toolbar-title>
            </v-toolbar>

            <v-list two-line subheader>
              <v-subheader class="headline"></v-subheader>
              <p class="mx-12 text-right"><b>{{todos.length}}</b> Tareas</p>

              <v-list-item>
                <v-list-item-content>
                  <v-form ref="form" v-model="valid">
                    <v-container>
                      <v-list-item-title>
                        <v-row>
                          <v-col cols="5">
                            <v-text-field :rules="required" v-model="title" id="title" name="title" label="Titulo" />
                          </v-col>
                          <v-col cols="5">
                            <v-text-field :rules="required" v-model="description" id="description" name="description" label="Descripcion" />
                          </v-col>
                          <v-col cols="2">
                            <v-btn
                              class="mx-2"
                              fab
                              dark
                              color="indigo"
                              @click="addTodo"
                            >
                              <v-icon dark>
                                mdi-plus
                              </v-icon>
                            </v-btn>
                          </v-col>
                        </v-row>
                      </v-list-item-title>                    
                    </v-container>
                  </v-form>
                </v-list-item-content>
              </v-list-item>
            </v-list>

            <v-list-item two-line v-for="todo in todos" :key="todo.id">
              <v-list-item-content>
                <v-list-item-title> {{todo.title}}</v-list-item-title>
                <v-list-item-subtitle>{{todo.description}}</v-list-item-subtitle>
              </v-list-item-content>
              <v-btn fab ripple x-small color="red" @click="removeTodo(todo.id)">
                <v-icon class="white--text">mdi-close</v-icon>
              </v-btn>              
            </v-list-item>

        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  import axios from 'axios'
  const API_URL = "http://127.0.0.1:8000/todo_list/tasks/"
  export default {
    name: 'TodoApp',
    data: () => ({
      valid: false,
      required: [
        v => !!v || 'Campo obligatorio',
      ],
      title: '',
      description: '',
      todos: [],
    }),
    mounted () {
      axios
        .get(API_URL)
        .then(response => (this.todos = response.data))
        .catch(e => console.error(e))
    },
    methods: {
      addTodo() {
        /*
        if (this.$refs.form.validate()) {
          this.todos.push({
            title: this.title.trim(), description: this.description.trim()
          });
          this.$refs.form.reset()
        }
        */
        if (this.$refs.form.validate()) {
            const post_data = {title: this.title.trim(), description: this.description.trim()}
            axios
                .post(API_URL, post_data)
                .then((newObject) => {
                    console.log(newObject.data)
                    this.todos.push(newObject.data);
                    this.$refs.form.reset()
                })
                .catch(e => console.error(e))
        }
      },
      removeTodo(idToDelete) {
        axios
          .delete(API_URL + idToDelete)
          .then(() => {
            this.todos = this.todos.filter(function( obj ) {
                return obj.id !== idToDelete;
            })
          })
          .catch(e => console.error(e))
      }
    }
  }
</script>
