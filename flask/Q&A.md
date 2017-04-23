### Q:同步vitualbox访问flask支持的页面，浏览器现实连接已重置
通过vagrant部署的开发环境，转发端口：

`web.vm.network "forwarded_port", guest: 5000, host: 5050, host_ip: "127.0.0.1", auto_correct: true`

页面访问地址`http://127.0.0.1:5050/`，现实连接被重置

### A:将host改为 0.0.0.0

将`app.run()`改为`app.run("0.0.0.0")` 
