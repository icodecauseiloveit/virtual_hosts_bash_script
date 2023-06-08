# virtual_hosts_bash_script

```sh
#!/bin/bash

# Instalar Apache2
sudo apt-get update
sudo apt-get install apache2

# Crear la estructura de carpetas
projects_dir="/home/projects"

if [ ! -d "$projects_dir" ]; then
  sudo mkdir "$projects_dir"
fi

sudo chown -R $USER:$USER "$projects_dir"
sudo chmod -R 755 "$projects_dir"

# Configurar Virtual Hosts en Apache
sudo tee /etc/apache2/sites-available/projects.conf > /dev/null <<EOF
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot $projects_dir

    <Directory "$projects_dir">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog \${APACHE_LOG_DIR}/error.log
    CustomLog \${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOF

# Activar el Virtual Host
sudo a2ensite projects.conf

# Reiniciar Apache
sudo service apache2 restart

```
