1. Instalando node y dependencias
```
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.1/install.sh | bash
```
2. Activando NVM
```
$ source ~/.bashrc
```
3. Instalando node
```
$ nvm install 7
```
4. Verificamos que se haya instalado correctamente
```
$ node --version
```
5. Creamos la ruta pública donde alojaremos nuestro proyecto
```
$ mkdir server
$ cd server
```
6. creamos el archivo package.json
```
$ npm init
```
7. Instalamos express
```
$ npm install express --save-dev
```
8. Creamos el archivo index.js con mcedit index.js
```
const express = require('express')
const app = express()
app.get('/', (req, res) => {
  res.send('HEY!')
})
app.listen(3000, () => console.log('Server running on port 3000'))
```
10. Iniciamos el servidor
```
node index.js
```
11. Realizamos las configuraciones necesarias para que resuelva en el puerto 3000 dentro de AWS
12. Instalamos nginx para que podamos resolver las aplicaciones node en el puerto 80
```
sudo apt-get install nginx
```
13. Si nginx no levanta de manera automática ejecutamos el siguiente comando
```
sudo /etc/init.d/nginx start
```
14. Eliminamos el sitio por defecto
```
sudo rm /etc/nginx/sites-enabled/default
```
15. Creamos un archivo de configuración en sites-available
```
sudo mcedit /etc/nginx/sites-available/webumss
```
Con el siguiente contenido
```
server {
  listen 80;
  server_name webumss;
  location / {
    proxy_set_header   X-Forwarded-For $remote_addr;
    proxy_set_header   Host $http_host;
    proxy_pass         "http://127.0.0.1:3000";
  }
}
```
16. realizamos el enlace de site-available con site-enabled
```
sudo ln -s /etc/nginx/sites-available/webumss /etc/nginx/sites-enabled/webumss
```
17. Reiniciamos nginx 
```
sudo service nginx restart
```
