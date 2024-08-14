# Nginx

 ## 1. Cài đặt nginx
	
			sudo apt update
			sudo apt install nginx

	
### 1.1 khởi động Nginx
	
		sudo systemctl start nginx
		sudo systemctl enable nginx

## 2. Cấu hình Nginx làm Reserve Proxy

### 2.1 Tạo file vhost

		sudo nano /etc/nginx/sites-available/example.com

-	Thêm cấu hình sau vào file

		server {
	    listen 80;
	    server_name example.com;  ## Thay bằng DNS cần trỏ

	    location / {
	        proxy_pass https://Địa_chỉ_ip:8080;  # Cổng mà Apache đang lắng nghe
	        proxy_set_header Host $host;
	        proxy_set_header X-Real-IP $remote_addr;
	        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	        proxy_set_header X-Forwarded-Proto $scheme;
	    }
		}
### 2.2 Kích hoạt file vHost
	
		sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
### 2.3 Kiểm tra cấu hình Nginx

		sudo nginx -t
		sudo systemctl reload nginx
# 3. Kích hoạt SSL
## 3.1 Cài đặt certbot	
	
	
		sudo apt update
		sudo apt install certbot python-3-certbot-nginx
##	3.2 Tạo chứng chỉ 

		sudo certbot --nginx -d example.com -d www.example.com
##	3.3 Thêm cấu hình SSL  

-	Nếu bạn sử dụng Certbot, cấu hình SSL sẽ tự động được thêm vào. 
- Nếu bạn tự cấu hình, thêm hoặc sửa đổi phần cấu hình như sau:


		server {
		   listen 80;
	    server_name example.com www.example.com;

	    #Chuyển hướng tất cả các yêu cầu HTTP sang HTTPS
	    return 301 https://$host$request_uri;
		}

		server {
	    listen 443 ssl;
	    server_name example.com www.example.com;

	    ssl_certificate /etc/ssl/certs/example.com.crt;
	    ssl_certificate_key /etc/ssl/private/example.com.key;

	    ssl_protocols TLSv1.2 TLSv1.3;
	    ssl_prefer_server_ciphers on;
	    ssl_ciphers HIGH:!aNULL:!MD5;

	    root /var/www/html;
	    index index.php index.html index.htm;

	    location / {
	        try_files $uri $uri/ =404;
	    }

	    location ~ \.php$ {
	        include snippets/fastcgi-php.conf;
	        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
	    }

	    location ~ /\.ht {
	        deny all;
	    }
		}
# 4. Firewall
- Nếu có tường lửa đang chạy trên máy chủ của bạn, hãy đảm bảo cổng mới đã được mở để cho phép lưu lượng truy cập.

		sudo ufw allow 8080/tcp

### Notes:
- Khi cấu hình reserve proxy, phải thêm SSL trên các file vhost của **Apache** và **Nginx**
- Đổi port **Listen** của **Apche** để tránh xung đột
	
		sudo nano /etc/apache2/ports.conf

	Tìm dòng `Listen 80` và thay đổi thành cổng mà bạn muốn Apache lắng nghe. Ví dụ, để Apache lắng nghe trên cổng 8080, thay đổi thành:
	
		Listen 8080


