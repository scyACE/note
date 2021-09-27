微信小程序websocket 需要encode，否则会报错  <font color='red'>Invalid HTTP status</font>

#### 微信公众号开发

- 授权本地测试：

  1. 更改host，将要调试的域名指向本地

     127.0.0.1 xxx.com

  2. 用nginx做端口转发

     由于微信开发者工具里只支持80和443端口，域名后续是不能带端口号的，所以仅仅做host更改是不行的

     无法调试到本地的其余端口，因此用nginx做一下端口转发

     ~~~nginx
      server {
      
               server_name www.xxxx.com
               listen 80;
      
      
               location / {
                 proxy_pass http://127.0.0.1:8080;
               }
         }
     ~~~

     



