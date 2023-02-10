
```
docker run -v C:\Users\haiaty.varotto\Code\mycustom.env:/app/.env:ro <name_image> node ./src/services/index.js
```


In this command, I run a container from windows.

- the container is from the image <name_image'

- I load a custom .env file that is found in C:\Users\haiaty.varotto\Code\mycustom.env    to the path in the container: /app/.env

- the  :ro at the end means that the file is only in read only mode

- the node ./src/services/index.js means that this shell command will be executed (because in the dockerfile of the image I had CMD ["sh", "-c", "tail -f /dev/null"] )



NOTE: if the file is present in the container, it will be overwritten by the file that you mounting.
