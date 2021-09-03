# vbee-docker-nginx-jwt
Đây là nginx docker được sửa dụng để download file dựa vào token bản gốc xem tại **https://github.com/max-lt/nginx-jwt-module** Bản hiện tại là bản đã sửa đổi đôi chút so với bản gốc, thêm tính năng chặn theo IP. IP sẽ được nhét vào trong jwt lúc sinh token

>1. Clone git

>2. docker load -i jwt-nginx.tar.gz

>3. Xem file docker-compose 

** Cần config nginx bên ngoài để trỏ vào nginx bên trong ***

location /secretlink{

	rewrite ^/secretlink/(.*)/video/(.*)$ /uploads/$2 break;
	
	proxy_set_header Authorization "Bearer $1";

	proxy_set_header Referer "vbee-holding";

	proxy_set_header Host $http_x_real_ip;

	proxy_pass  http://localhost:8002;

}


Thông tin **proxy_set_header Referer "vbee-holding";** để nguyên nha :v chữ ký của vbee-holding :v anh em xài bản này thì để vậy làm đóng dấu
