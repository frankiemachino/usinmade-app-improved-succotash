pipeline {
  agent any
  stages {
    stage('shellb') {
      steps {
        sh '''mysqldump --all-databases --user=jenkins > all_databases.sql

rm -R $( ls -I "*.sql" )

touch storage/logs/laravel.log

sudo chown -R jenkins:www-data storage/ bootstrap/

composer install

name=$JOB_NAME

name2=${name%/*}

echo "$name2"

echo "$WORKSPACE"

# create symlink between public folder & other shit

echo "trying symbolic link"

sudo ln -s "$WORKSPACE"/public /var/www/$name2 

echo "Creating Apache Site Configuration File"
sudo echo "<VirtualHost *:80>
        ServerName "$name2"
        DocumentRoot /var/www/$name2
        <Directory "/var/www/$name2">
        LogLevel warn
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
        </Directory>
</VirtualHost>" > /etc/apache2/sites-available/$name2.conf

sudo a2ensite $name2.conf

sudo service apache2 reload

sudo mv .env.example .env

php artisan key:generate'''
      }
    }
  }
}