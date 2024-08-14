
# **Apache**

# 1. Cài đặt môi trường	
*emphasized text*## a. Cài đặt 

		sudo apt install apache2 php libapache2-mod-php mysql-server php-mysql

## b. Khởi động dịch vụ và kích hoạt
		
	sudo systemctl start apache2	
	sudo systemctl enable apache2
	sudo systemctl start mysql
	sudo systemctl enable mysql
				
# 2. WP
## 2.1 Cài đặt WP
	
		wget https://wordpress.org/latest.tar.gz
		tar -xzf latest.tar.gz 
		sudo mv wordpress /var/www/html/ ##di  chuyển thư mục wp 
## 2.2 Cấp quyền thư mục
	
		sudo chown -R www-data:www-data /var/www/html/wordpress
		sudo chmod -R 755 /var/www/html/wordpress
##	2.3 Tạo Database

		sudo mysql -u root -p

	CREATE DATABASE wordpress_db;
	CREATE USER 'wordpress_user'@'localhost' IDENTIFIED BY 'password';
	GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wordpress_user'@'localhost';
	FLUSH PRIVILEGES;
	EXIT

 -   Thay `wordpress_db` bằng tên cơ sở dữ liệu bạn muốn.
-   Thay `wordpress_user` và `password` bằng tên người dùng và mật khẩu bạn muốn đặt.
# 3. Laravel
## 3.1 Cài đặt môi trường composer


		php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
		php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
		php composer-setup.php
		php -r "unlink('composer-setup.php');"
## 3.2 Tạo project laravel
	
		composer create-project laravel/laravel example-app
- tạo file .htaccess

		<IfModule mod_rewrite.c>
		Options +FollowSymLinks
		RewriteEngine On
		RewriteCond %{REQUEST_URI} !^/public/ 
		RewriteCond %{REQUEST_FILENAME} !-d
		RewriteCond %{REQUEST_FILENAME} !-f
		RewriteRule ^(.*)$ /public/$1 
		#RewriteRule ^ index.php [L]
		RewriteRule ^(/)?$ public/index.php [L] 
		</IfModule>



##	3.3 Cấp quyền cho thư mục
		
		cd example-app
		sudo chown -R www-data:www-data /var/www/html/example-app
		sudo chmod -R 755 /var/www/html/example-app
# 4.  Virtual Host
## 4.1 tạo file Virtual host cho wordpress

		sudo nano /etc/apache2/sites-available/wordpress.conf
	-> Có thể thay wordpress.conf thành tên tệp mong muốn
- Thêm cấu hình sau vào tệp
	

		<VirtualHost *:80>
	    ServerAdmin webmaster@example.com 
	    ServerName example.com # thay bằng tên miền
	    DocumentRoot /var/www/html/wordpress  ## Trỏ về thư mục lưu file wordpress

	    <Directory /var/www/html/wordpress> ##Trỏ về thư mục lưu file wordpress
        AllowOverride All
        Require all granted
	    </Directory>
	    ErrorLog ${APACHE_LOG_DIR}/error.log
	    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

## 4.2 Tạo file Virtual host cho Laravel

		sudo nano /etc/apache2/sites-available/laravel.conf
- Thêm cấu hình sau vào tệp


		<VirtualHost *:80>
	    ServerAdmin webmaster@example.com 
	    ServerName example.com # thay bằng tên miền
	    DocumentRoot /var/www/html/example-app  ## Trỏ về thư mục lưu file laravel

	    <Directory /var/www/html/example-app> ##Trỏ về thư mục lưu file laravel
        AllowOverride All
        Require all granted
	    </Directory>
	    ErrorLog ${APACHE_LOG_DIR}/error.log
	    CustomLog ${APACHE_LOG_DIR}/access.log combined
## 4.3 Kích hoạt file vhost

	sudo a2ensite wordpress.conf  # kích hoạt file chứ WP
	sudo a2ensite laravel.conf    #kích hoạt file chứ laravel
- Tắt vhost mặc định (nếu cần)
		
		sudo a2dissite 000-default.conf
## 4.4 Khởi động lại Apache
		
		sudo systemctl restart apache2

