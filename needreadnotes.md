----------------- For localhost testing ------------------
--------------- discord-frontend: 

Step 1. Edit the package.js on your frontend. Remove the "HTTPS=true SSL_CRT_FILE=./certs/cert.crt SSL_KEY_FILE=./certs/cert.key" and only the "react-scripts start" will remain.

    FILE LOCATION: /discord-app/discord-frontend/package.json

Step 2. Edit the line 5 on the api.js. Change "https://192.168.100.12:5002/api" to "http://localhost:5002/api"

- This API is very importang on the applicaton specially on the login ang authentication.

    FILE LOCATION: /discord-app/discord-frontend/src/api.js

Step 3. On socketConnection.js line 17, change "https://192.168.100.12:5002" to "http://localhost:5002". We remove the "s" on "https" and remove the IP address and change it to "localhost" because we are on localhost and using our own machine.

    FILE LOCATION: /discord-app/discord-frontend/src/realtimeCommunication/socketConnection.js

------------- discord-backend:

Step 1. Change "const https = require("http");" to "const https = require("http");".

Step 2. Remove or comment the "const fs = require("fs");" on line 4, "const key = fs.readFileSync('./certs/cert.key')" and 
"const cert = fs.readFileSync('./certs/cert.crt')" on line 23 and 24.

Step 3. Change the "const server  = https.createServer({key, cert}, app)" to "const server  = http.createServer(app)"

    FILE LOCATION: /discord-app/discord-backend/server.js





-------------------- For Production -----------------------




discord-frontend:

    1. Edit line 17 of socketConnection.js. Change the IP address to your server IP address or domain, and the port to the port used on your backend (ex. https://192.168.100.12:5002).

    - This is where your frontend connect to your backend
    - The IP is for connecting to your server
    - The port is for your frontend to know which      application to connect on your server/computer. All application have specific port on your computer when you run them, that is why when the port is being used by other applications your application(ex. like your frontend app) it will not run.
    - Port number can be change on the .env file on you backend. If you change it, change the port on socketConnection.js to the port you've used.

    FILE LOCATION: /discord-app/discord-frontend/src/realtimeCommunication/socketConnection.js
    
    2. Edit the IP address and port on the api.js. This is the same for the step 1 but this is for the API..

    - If we change the IP address/domain of the API when the app is running, we won't see any errors on the terminal or console, but when we logout the account we can then get this error and can no longer login: 
    "
    authActions.js:27 
    {
        error: true, 
        exception: 
        Error: timeout of 1000ms exceeded
    at createError (https://192.168.100.12:3000/static/js/vendors…}
    "
    this is because our frontend cannot find the IP/domain given to it.

    - If we bring back the original and working IP/address, refresh the browser and try to login an account it will work and return this to the console

    "
    authActions.js:27 {
        data: {…}, 
        status: 200, 
        statusText: 'OK', 
        headers: {…}, 
        config: {…}, …}
    "
    FILE LOCATION: /discord-frontend/src/api.js

    3. Add this line on package.js inside your frontend:
    
        "HTTPS=true SSL_CRT_FILE=./certs/cert.crt" SSL_KEY_FILE=./certs/cert.key"

        It you look like this:
    
        "start": "HTTPS=true SSL_CRT_FILE=./certs/cert.crt SSL_KEY_FILE=./certs/cert.key react-scripts start"

    - Change the "./certs/cert.crt" to the location of your crt and the "./certs/cert.key" to your key location.
    - This will allow us to run HTTPS on our localhost. (I don't know if this can be still will be use when you hosted it on web server with SSL because that's already has HTTPS)

    - This is also for our webRTC, because we cannot use the function "getUserMedia" on not secure server/ HTTP. it can only be used on HTTPS which is secure. 


discord-backend:

    1. Edit line 19 of sockerServer.js, change it to you server IP address or domain. In this case the IP address is the same to the IP address of our backend(backend-server) because we are hosting them on the same server/computer but, you can also notice that the port are different, it is because they are actually different application. On this part of the code, we are allowing this IP/domain to connect to our backend. Based on the tutorial we can allow multiple application(means change port) to connect to our backend(I don't know if we can allow different IP/domain).

    FILE LOCATION: /discord-app/discord-backend/sockerServer.js

    2. Edit the line 23 and 24 on server.js. Change the key and crt to your key and crt locations.

    - This is also for our webRTC, because we cannot use the function "getUserMedia" on not secure server/ HTTP. it can only be used on HTTPS which is secure.

    FILE LOCATION: /discord-app/discord-backend/server.js