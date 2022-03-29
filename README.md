# pizza-client

## Project setup
```
docker build -t pizza-vue . ; 
```

### Run container
```
docker run -it p 8080:8080 --rm --name pizza-client pizza-vue; 
```

### Run test, folder src/components
```
testcafe chrome test.js;
```