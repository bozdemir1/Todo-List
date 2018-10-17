<template>
  <div class="todos">
    <div class="header">
     <h1>Todo List</h1>
      <input class="new-todo" v-model="newTodo" v-on:keyup.enter="addTodo" placeholder="Add a todo" autofocus>
    </div>
    <div class="main">
      <ul class="todo-list">
        <li v-for="todo in filteredTodos" :class="{completed: todo.completed, editing: todo == editedTodo}">
          <div class="view">
          <input class="toggle" type="checkbox" v-model="todo.completed">
            <label @dblclick="editTodo(todo)">{{todo.title}}</label>
            <button class="destroy" @click="removeTodo(todo)"></button>
          </div>
          <input class="edit" type="text"
                 v-model="todo.title" @blur="doneEdit(todo)"
                 @keyup.enter="doneEdit(todo)"/>
        </li>
      </ul>
      <input class="toggle-all" id="toggle-all" type="checkbox">
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list"></ul>
    </div>
    <footer class="footer">
      <ul class="filters">
        <li><a href="#" @click.prevent="visibility='all'"  :class="{ selected: visibility == 'all' }">All</a></li>
        <li><a href="#" @click.prevent="visibility='active'":class="{ selected: visibility == 'active' }">Active</a></li>
        <li><a href="#" @click.prevent="visibility='completed'":class="{ selected: visibility == 'completed' }">Completed</a></li>
      </ul>
      <ul class="social">
        <li><a href="https://www.facebook.com/Bozdemir.Salih"><i class="fa fa-facebook" aria-hidden="true"></i></a></li>
        <li><a href="https://twitter.com/ssalihbozdemir"><i class="fa fa-twitter" aria-hidden="true"></i></a></li>
        <li><a href="https://plus.google.com/u/0/107033276132428048488"><i class="fa fa-google-plus" aria-hidden="true"></i></a></li>
        <li><a href="https://www.linkedin.com/in/salih-bozdemir-b7712495//"><i class="fa fa-linkedin" aria-hidden="true"></i></a></li>
        <li><a href="https://www.instagram.com/salihbozdemir1"><i class="fa fa-instagram" aria-hidden="true"></i></a></li>
      </ul>
    </footer>
    <footer class="info">
      <p>Double-click edit a todo</p>
    </footer>
  </div>
</template>

<script>
const STORAGE_KEY = 'todo_storage'
export default {
  name: 'todos',
  data: function () {
    return {
      newTodo: '',
      todos: [],
      editedTodo: null,
      visibility: 'all'
    }
  },
  created () {
    this.todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
  },
  computed: {
    filteredTodos () {
      if (this.visibility === 'all') {
        return this.todos
      } else if (this.visibility === 'active') {
        return this.todos.filter(function (todo) {
          return !todo.completed
        })
      } else {
        // completed
        return this.todos.filter(function (todo) {
          return todo.completed
        })
      }
    }
  },
  methods: {
    addTodo () {
      this.todos.push({title: this.newTodo, completed: 'false', id: this.todos.lenght})
      this.newTodo = ''
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos))
    },
    removeTodo (todo) {
      this.todos.splice(this.todos.indexOf(todo), 1)
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos))
    },
    editTodo (todo) {
      this.editedTodo = todo
    },
    doneEdit (todo) {
      if (!this.editedTodo) {
        return
      }
      this.editedTodo = null
      todo.title = todo.title.trim()
      if (!todo.title) {
        this.removeTodo(todo)
      }
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos))
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  button,
  input[type="checkbox"] {
    outline: none;
  }

  .hidden {
    display: none;
  }

  .todos {
    margin-top:200px;
    left:30%;
    position: relative;
    box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2),
    0 25px 50px 0 rgba(0, 0, 0, 0.1);
    max-width:800px;
  }

  .todos input::-webkit-input-placeholder {
    font-style: italic;
    font-weight: 300;
    color: #e6e6e6;
  }

  .todos input::-moz-placeholder {
    font-style: italic;
    font-weight: 300;
    color: #e6e6e6;
  }

  .todos input::input-placeholder {
    font-style: italic;
    font-weight: 300;
    color: #e6e6e6;
  }

  .todos h1 {
    position: absolute;
    top: -185px;
    width: 100%;
    font-size: 100px;
    font-weight: 100;
    text-align: center;
    color: rgba(15, 105, 14, 0.56);
    -webkit-text-rendering: optimizeLegibility;
    -moz-text-rendering: optimizeLegibility;
    text-rendering: optimizeLegibility;
  }

  .new-todo,
  .edit {
    position: relative;
    margin: 0;
    width: 100%;
    font-size: 24px;
    font-family: inherit;
    font-weight: inherit;
    line-height: 1.4em;
    border: 0;
    outline: none;
    color: inherit;
    padding: 6px;
    border: 1px solid #999;
    box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
    box-sizing: border-box;
    -webkit-font-smoothing: antialiased;
    -moz-font-smoothing: antialiased;
    font-smoothing: antialiased;
  }

  .new-todo {
    padding: 16px 16px 16px 40px;
    border: none;
    background: rgb(42,72,107);
    background: -moz-linear-gradient(left,  rgba(42,72,107,1) 0%, rgba(80,122,167,1) 100%);
    background: -webkit-linear-gradient(left,  rgba(42,72,107,1) 0%,rgba(80,122,167,1) 100%);
    background: linear-gradient(to right,  rgba(42,72,107,1) 0%,rgba(80,122,167,1) 100%);
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#2a486b', endColorstr='#507aa7',GradientType=1 );
    box-shadow: inset 0 2px 1px rgba(0,0,0,0.03);
  }

  .main {
    position: relative;
    z-index: 2;
    border-top: 1px solid #e6e6e6;
  }

  label[for='toggle-all'] {
    display: none;
  }

  .toggle-all {
    position: absolute;
    top: -55px;
    left: -12px;
    width: 60px;
    height: 34px;
    text-align: center;
    border: none; /* Mobile Safari */
  }

  .toggle-all:before {
    content: '❯';
    font-size: 22px;
    color: #e6e6e6;
    padding: 10px 27px 10px 27px;
  }

  .toggle-all:checked:before {
    color: #737373;
  }

  .todo-list {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  .todo-list li {
    position: relative;
    font-size: 24px;
    border-bottom: 1px solid #ededed;
  }

  .todo-list li:last-child {
    border-bottom: none;
  }

  .todo-list li.editing {
    border-bottom: none;
    padding: 0;
  }

  .todo-list li.editing .edit {
    display: block;
    width: 100%;
    padding: 13px 17px 12px 17px;
    margin: 0;
    background: none;
    border-bottom: 2px solid #1f4adc;
  }

  .todo-list li.editing .view {
    display: none;
  }

  .todo-list li .toggle {
    text-align: center;
    width: 40px;
    height: auto;
    position: absolute;
    top: 0;
    bottom: 0;
    margin: auto 0;
    border: none; /* Mobile Safari */
    -webkit-appearance: none;
    appearance: none;
  }

  .todo-list li .toggle:after {
    content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="50" fill="none" stroke="#ededed" stroke-width="3"/></svg>');
  }

  .todo-list li .toggle:checked:after {
    content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="50" fill="none" stroke="#bddad5" stroke-width="3"/><path fill="#5dc2af" d="M72 25L42 71 27 56l-4 4 20 20 34-52z"/></svg>');
  }

  .todo-list li label {
    white-space: pre-line;
    word-break: break-all;
    padding: 15px 60px 15px 15px;
    margin-left: 45px;
    display: block;
    line-height: 1.2;
    transition: color 0.4s;
  }

  .todo-list li.completed label {
    color: #d9d9d9;
    text-decoration: line-through;
  }

  .todo-list li .destroy {
    display: none;
    position: absolute;
    top: 0;
    right: 10px;
    bottom: 0;
    width: 40px;
    height: 40px;
    margin: auto 0;
    font-size: 30px;
    color: #cc545a;
    margin-bottom: 11px;
    transition: color 0.2s ease-out;
  }

  .todo-list li .destroy:hover {
    color: #af5b5e;
  }

  .todo-list li .destroy:after {
    content: 	'\d7';
  }

  .todo-list li:hover .destroy {
    display: block;
  }

  .todo-list li .edit {
    display: none;
  }

  .todo-list li.editing:last-child {
    margin-bottom: -1px;
  }

  .footer {
    color: #777;
    padding: 10px 15px;
    height: 20px;
    text-align: center;
    border-top: 1px solid #e6e6e6;
  }

  .footer:before {
    content: '';
    position: absolute;
    right: 0;
    bottom: 0;
    left: 0;
    height: 50px;
    overflow: hidden;
    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2),
    0 8px 0 -3px #f6f6f6,
    0 9px 1px -3px rgba(0, 0, 0, 0.2),
    0 16px 0 -6px #f6f6f6,
    0 17px 2px -6px rgba(0, 0, 0, 0.2);
  }

  .filters {
    margin: 0;
    padding: 0;
    list-style: none;
    position: absolute;
    right: 0;
    left: 0;
  }

  .filters li {
    display: inline;
  }

  .filters li a {
    color: #e6e6e6;
    margin: 3px;
    padding: 3px 7px;
    text-decoration: none;
    border: 1px solid transparent;
    border-radius: 3px;
  }

  .filters li a.selected,
  .filters li a:hover {
    border-color: rgba(175, 47, 47, 0.2);
  }

  .filters li a.selected {
    border-color: rgba(175, 5, 8, 0.9);
  }

  .info {
    position: absolute;
    bottom:-80%;
    left:40%;
    color: #bfbfbf;
    font-size: 18px;
    text-shadow: 0 1px 0 rgba(255, 255, 255, 0.5);
    text-align: center;
  }

  .info p {
    line-height: 1;
  }

  .info a {
    color: inherit;
    text-decoration: none;
    font-weight: 400;
  }

  .info a:hover {
    text-decoration: underline;
  }
  @media screen and (-webkit-min-device-pixel-ratio:0) {
    .toggle-all,
    .todo-list li .toggle {
      background: none;
    }

    .todo-list li .toggle {
      height: 40px;
    }

    .toggle-all {
      -webkit-transform: rotate(90deg);
      transform: rotate(90deg);
      -webkit-appearance: none;
      appearance: none;
    }
  }

  @media (max-width: 430px) {
    .footer {
      height: 50px;
    }

    .filters {
      bottom: 10px;
    }
  }

/* Social media */
  .social{
    position: absolute;
    top: 100%;
    left: 50%;
    transform: translate(-50%, -50%);
    margin-top: 100px;
    padding: 0;
    display: flex;

  }
  .social li{
    list-style: none;
    margin: 0 40px;

  }
  .social li .fa{
    font-size: 40px;
    color: #262626;
    line-height: 80px;
    transition: .5s;

  }
  .social li a{
    position: relative;
    display: block;
    width: 80px;
    height: 80px;
    background-color: #fff;
    text-align: center;
    transform: perspective(100px) rotate(-30deg) skew(25deg) translate(0,0);
    transition: .5s;
    box-shadow: -20px 20px 10px rgb(0, 0, 0, 0.5);
  }
  .social li a::before{
    content: "";
    position: absolute;
    top: 10px;
    left: -20px;
    height: 100%;
    width: 20px;
    background: #b1b1b1;
    transition: .5s;
    transform: rotate(0deg) skewY(-45deg);
  }
  .social li a::after{
    content: "";
    position: absolute;
    top: 80px;
    left: -11px;
    height: 20px;
    width: 100%;
    background: #b1b1b1;
    transition: .5s;
    transform: rotate(0deg) skewX(-45deg);
  }
  .social li a:hover{
    transform: perspective(1000px) rotate(-30deg) skew(25deg) translate(20px, -20px);
    box-shadow: -50px 50px 50px rgb(0, 0, 0, 0.5);
  }
  .social li:hover .fa{
    color: #fff;
  }
  .social li a:hover{
    transform: perspective(1000px) rotate(-30deg) skew(25deg) translate(20px, -20px);
    box-shadow: -50px 50px 50px rgb(0, 0, 0, 0.5);
  }
  .social li:hover:nth-child(1) a{
    background: #3b5999;
  }

  .social li:hover:nth-child(1) a:before{
    background: #2e4a86;
  }
  .social li:hover:nth-child(1) a:after{
    background: #4a69ad;
  }
  .social li:hover:nth-child(2) a{
    background: #55acee;
  }
  .social li:hover:nth-child(2) a:before{
    background: #4184b7;
  }
  .social li:hover:nth-child(2) a:after{
    background: #4d9fde;
  }
  .social li:hover:nth-child(3) a{
    background: #dd4b39;
  }

  .social li:hover:nth-child(3) a:before{
    background: #c13929;
  }
  .social li:hover:nth-child(3) a:after{
    background: #e83322;
  }

  .social li:hover:nth-child(4) a{
    background: #0077B5;
  }

  .social li:hover:nth-child(4) a:before{
    background: #036aa0;
  }
  .social li:hover:nth-child(4) a:after{
    background: #0d82bf;
  }

  .social li:hover:nth-child(5) a{
    background: linear-gradient(#400080, transparent), linear-gradient(200deg, #d047d1, #ff0000, #ffff00);
  }

  .social li:hover:nth-child(5) a:before{
    background: linear-gradient(#400080, transparent), linear-gradient(200deg, #d047d1, #ff0000, #ffff00);
  }
  .social li:hover:nth-child(5) a:after{
    background: linear-gradient(#400080, transparent), linear-gradient(200deg, #d047d1, #ff0000, #ffff00);
  }

  .centered{
    width:400px;
    height:400px;
    position:absolute;
    top:50%;
    left:50%;
    transform:translate(-50%,-50%);
    background:#000;
    filter: blur(10px) contrast(20);
  }
  .blob-1,.blob-2{
    width:70px;
    height:70px;
    position:absolute;
    background:#fff;
    border-radius:50%;
    top:50%;left:50%;
    transform:translate(-50%,-50%);
  }
  .blob-1{
    left:20%;
    animation:osc-l 2.5s ease infinite;
  }
  .blob-2{
    left:80%;
    animation:osc-r 2.5s ease infinite;
    background:#0ff;
  }
  @keyframes osc-l{
    0%{left:20%;}
    50%{left:50%;}
    100%{left:20%;}
  }
  @keyframes osc-r{
    0%{left:80%;}
    50%{left:50%;}
    100%{left:80%;}
  }
</style>
