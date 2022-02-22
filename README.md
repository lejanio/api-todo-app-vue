# api-todo-app-vue

A working project can be found here: https://lejanio.github.io/api-todo-app-vue/

## General description

This app is based on API requests fetching the initial data from a mock API, as well
as providing the possibility to add a new to-do item, set an item as completed, and
delete an item.

The app is implemented using Vue.js, Vue router and Axios.

## Project structure

1. The entry file is HomeView.vue:
   - Contains multiple methods for various API requests - getting data on initial render, posting
     new items, updating items, as well as deleting them from the API
   - After clicking on a particular item, the user is redirected to a separate item page
2. ItemView.vue provides additional information regarding specific items, namely, the creation date
   and the id number


## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
