# Antes de instalar WordPress

---

## 1. Instalamos Apache2

### Escribimos:
```bash
sudo apt install apache2
```

### Para comprobar si lo hemos hecho correctamente, escribimos en el navegador: `http://localhost`

<img width="1828" height="894" alt="image" src="https://github.com/user-attachments/assets/66702d5a-cd2b-4d59-a3cd-9e6bf3a2253c" />

---

## 2. Instalamos MySQL

### Escribimos:
```bash
sudo apt install mysql-server
```

### Para verificar que está funcionando correctamente, escribimos:
```bash
sudo systemctl status mysql
```
Después, lo reiniciaremos con:
```bash
sudo systemctl restart mysql
```
Se debería ver un mensaje como el siguiente:

<img width="1684" height="496" alt="image" src="https://github.com/user-attachments/assets/612e9b93-b0f9-4488-b749-1ecd78f4b6e0" />

---

## 3. Instalamos PHP

### Escribimos:
```bash
sudo apt install php libapache2-mod-php
```

### Lo reiniciamos con:
```bash
sudo systemctl restart apache2
```
Después, con:
```bash
sudo systemctl status apache2
```
volvemos a ver si está funcionando bien.

<img width="1684" height="496" alt="image" src="https://github.com/user-attachments/assets/2d96a9fc-7eb3-4d58-ad6f-b3611e9b25d4" />

### Escribimos:
```bash
sudo nano /var/www/html/info.php
```
para abrir el archivo y escribimos cualquier cosa, como "Prueba". Para salir, damos `Ctrl + X`.

<img width="1684" height="496" alt="image" src="https://github.com/user-attachments/assets/f8b6b83b-9c78-4f42-8fcc-cb15af151ed8" />

### Podemos escribir en el navegador `http://localhost/info.php` para comprobar si funciona.

<img width="1684" height="496" alt="image" src="https://github.com/user-attachments/assets/bb2bffae-a10d-4005-95c2-bd077ef1d552" />

---

## 4. Instalamos phpMyAdmin

### Escribimos:
```bash
sudo mysql_secure_installation
```
y seleccionamos la configuración a su gusto.

<img width="1684" height="496" alt="image" src="https://github.com/user-attachments/assets/255bcf4f-7051-4b80-b8ef-120e858add56" />

### Escribimos ahora:
```bash
sudo mysql -u root -p
```
para entrar a MySQL.

<img width="1684" height="496" alt="image" src="https://github.com/user-attachments/assets/b816ee6d-fef7-4767-836d-49508a32c64c" />

### Ahora nos toca crear una base de datos, lo haremos escribiendo:
```sql
CREATE DATABASE wordpress;
```

### Y un usuario al que le otorgaremos permisos:
```sql
CREATE USER 'sandark'@'localhost' IDENTIFIED BY 'Gatita673#';
```

### Con:
```sql
GRANT ALL PRIVILEGES ON wordpress.* TO 'sandark'@'localhost';
```
le otorgaremos privilegios y con:
```sql
FLUSH PRIVILEGES;
```
actualizamos. Y salimos con `\q`.

### Procedemos a instalar phpMyAdmin:
```bash
sudo apt install phpmyadmin
```

<img width="1684" height="496" alt="image" src="https://github.com/user-attachments/assets/e7dca15f-1f58-4a81-a4ba-7336941fe482" />

<img width="1684" height="496" alt="image" src="https://github.com/user-attachments/assets/cf0fda54-4264-41b5-a141-04281092e7f0" />

### Escribimos:
```bash
ls /usr/share/phpmyadmin
```
para comprobar que se instaló correctamente.

### Escribimos:
```bash
sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
```
para crear un enlace para entrar por web.

### Reiniciamos Apache2:
```bash
sudo systemctl restart apache2
```

### Por último, comprobamos que el enlace ha sido creado:
```bash
ls -l /var/www/html/phpmyadmin
```

### Ahora podremos comprobar si funciona entrando a `http://localhost/phpmyadmin`.

<img width="1684" height="496" alt="image" src="https://github.com/user-attachments/assets/ee8adee1-5bcb-4591-b92e-2b36ddce9d2c" />

---

# Instalación de WordPress

### Nos movemos a un directorio temporal:
```bash
cd /tmp
```
Si quieres, puedes escribir `pwd` para comprobar.

### Descargamos la última versión de WordPress:
```bash
wget https://wordpress.org/latest.tar.gz
```

### Descomprimimos el archivo:
```bash
tar -xvzf latest.tar.gz
```

### Copiamos los archivos de WordPress y lo metemos en Apache:
```bash
sudo cp -a wordpress /var/www/html/
```

### Cambiamos el propietario de la carpeta www-data:
```bash
sudo chown -R www-data:www-data /var/www/html/wordpress
```

### Cambiamos los permisos de la carpeta para que no haya ningún problema:
```bash
sudo chmod -R 777 /var/www/html/wordpress
```

<img width="1684" height="496" alt="image" src="https://github.com/user-attachments/assets/7180c53c-dd9f-4bd4-bbba-0a7cfb09aa43" />

### Copiamos el archivo de configuración:
```bash
sudo cp /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php
```

### Con:
```bash
sudo nano /var/www/html/wordpress/wp-config.php
```
entramos para editar el archivo. Buscamos los comandos escritos de `define`, y cambiamos la `DB_NAME` a `wordpress`, el usuario y la contraseña a los que creamos en el SQL.

<img width="1684" height="496" alt="image" src="https://github.com/user-attachments/assets/f8f57079-1426-486f-956e-ac7225642983" />

### Instalamos curl:
```bash
sudo snap install curl
```

### Con:
```bash
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```
nos dará unas líneas de código de `define`. Copiamos todas y volvemos a abrir el archivo de config y las pegamos.

<img width="1918" height="513" alt="image" src="https://github.com/user-attachments/assets/4f09bba0-8470-4856-b326-2d7507f278f5" />

<img width="1843" height="496" alt="image" src="https://github.com/user-attachments/assets/0fd0eaed-664c-4ef8-bcb2-85ad663e0c0b" />

### Reiniciamos Apache2:
```bash
systemctl restart apache2
```

### Por último, entramos en `http://localhost/wordpress` desde el navegador, y si sale WordPress, ya lo tendremos instalado.

<img width="1918" height="513" alt="image" src="https://github.com/user-attachments/assets/de482d61-8c21-4d1d-bf38-e59212455676" />
