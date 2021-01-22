
采用include 引入配置：
      
      include /etc/nginx/conf.d/*.conf

nginx 使用一个默认转发端口

nginx从1.9.0开始，新增加了一个stream模块，用来实现四层协议的转发、代理或者负载均衡等

  stream 配置mysql的代理
      
       server{
            listen   [port];
            proxy_pass    [ip]:[port];
            proxy_connect_timeout  1h;
            proxy_timeout 1h;
        }


  配置文件目录浏览：


    server {
          listen       [port];
          server_name  [域名];
          root         /home/fileIndex/;
          autoindex on; #开启目录浏览
          autoindex_format html; #以html风格将目录展示在浏览器中
          autoindex_exact_size off; #切换为 off 后，以可读的方式显示文件大小，单位为 KB、MB 或者 GB
          autoindex_localtime on; #以服务器的文件时间作为显示的时间
          charset utf-8,gbk; #展示中文文件名    
    }


   配置代理端口
  
    server {
        listen       [port];
        listen        [port];
        server_name [域名];
        #root         /opt/gsx_docker/html;
        client_max_body_size 500G;
        client_body_buffer_size 2M;
        location / {
             proxy_set_header Host [域名]:[port] ;
             proxy_set_header X-Real-IP $remote_addr;
             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
             proxy_buffering off;
             proxy_pass http://[ip]:[port];
        }
  }
