
# Linux

### Check a specific env
`echo $ENV_NAME`

### Run the files of .env to export them to environment variables
```.env
export BACKEND_API_BASE_URL=https://startup.maxiparis.com/api
```

```sh
source .env
```

### Print all env variables
```
printenv
```