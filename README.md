# carbonado-clients

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

### Test
curl --location --request POST 'http://localhost:3000/upload' \
--header 'Content-Type: multipart/form-data' \
--form 'file=@/home/somewhere/picture.png'
