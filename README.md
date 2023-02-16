# Front-end for OpenSAS

This repository serves as the webserver for the OpenSAS GUI.

Instructions to run the webserver:

First clone the repository:

```
git clone https://github.com/CCI-NextG-Testbed/OpenSAS-dashboard/
```

Next install npm 
```
sudo apt-get install npm
```

Go into the clone directory
```
cd OpenSAS-dashboard
```
Install all the dependencies
``` npm install ```

Finally, before starting the server, the IP & port to the OpenSAS SocketIO server needs to be set. If the OpenSAS core is running on a different VM or machine, use its IP, else it will be localhost. The port on OpenSAS is set to 8000. This can be changed in the main.js file on line 29 (/src/main.js)
```
Vue.use(new VueSocketIO({
  debug: true,
  connection: 'http://10.147.20.114:8000',
  options: {  transports: ['websocket', 'polling',  'flashsocket'] } //Optional options
}))
```

Once the correct IP and port are set, the server can be started in the dev environment
```
npm run dev
```

