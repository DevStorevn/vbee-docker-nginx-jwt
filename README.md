# vbee-docker-nginx-jwt
![alt text](https://github.com/DevStorevn/vbee-docker-nginx-jwt/blob/master/images/follow.png?raw=true)
Đây là nginx docker được sửa dụng để download file dựa vào token bản gốc xem tại **https://github.com/max-lt/nginx-jwt-module** Bản hiện tại là bản đã sửa đổi đôi chút so với bản gốc, thêm tính năng chặn theo IP. IP sẽ được nhét vào trong jwt lúc sinh token

>1. Clone git

>2. docker load -i jwt-nginx.tar.gz

>3. Xem file docker-compose 

** Cần config nginx bên ngoài (gọi là nginx firewall như trong hình) để trỏ vào nginx bên trong ***

location /secretlink{

	rewrite ^/secretlink/(.*)/video/(.*)$ /uploads/$2 break;
	
	proxy_set_header Authorization "Bearer $1";

	proxy_set_header Referer "vbee-holding";

	proxy_set_header Host $http_x_real_ip;

	proxy_pass  http://localhost:8002;

}

> Ví Dụ với nginx bên trên https://api.vface.ai/secretlink/eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJpcCI6IjE3MS4yNDEuNTMuNTMiLCJpYXQiOjE2MzA2NjM2OTcsImV4cCI6MTYzMDY2Mzk5N30.Oei9LRGHcV85Gvhy54gNr5FdymX3jNojiuOcxdyU-RUI-ow9XTkwp18-mCEnBwkX7ZCLxfiM9O-Ep69wfCoxlQqn8YqIwWu7LN38AYhiMDu-xxHNptcAjA2j0w1QGbWqwG-FQJlluhnhHSbHWD8co7WnlZzSCVBTs_JFPPL8iyqyg5aquhumLpT4Na2hzlXknZQMF3WqjiPjMU5stTEN-4U9GmaOotgBtldrOZGaxn77s9wcVBaOct7Q1KVTIrftRe4KTa5vLYLrucxLE-MDGaVXCQvGHmfWorSpH8LrxwRYrS4gGhPa_SIu-ZKoxDr3gRt9CnQ2RuqywoLrZdq0Hg/video/3bcfc50d-3070-4b4a-bb39-c81c89d935be.mp4

Thông tin **proxy_set_header Referer "vbee-holding";** để nguyên nha :v chữ ký của vbee-holding :v anh em xài bản này thì để vậy làm đóng dấu

** Hàm tạo token trên Server backend **


function gen_jwt_token({info = {ip:'127.0.0.1'}, expiresIn='1minute'}){

  const privateKey =`-----BEGIN RSA PRIVATE KEY---- Nội dung file key-----END RSA PRIVATE KEY-----`;
 
 return   jwt.sign(info, privateKey, { algorithm: 'RS256',  expiresIn});

}

Cái token sinh ra sẽ được ném vào cái proxy_set_header Authorization "Bearer $1"; 