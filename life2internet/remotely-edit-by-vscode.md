> 参考https://blog.csdn.net/HouszChina/article/details/79850644

---

### remote server

#### install rmate tool.

- wget https://raw.githubusercontent.com/sclukey/rmate-python/master/bin/rmate
- chmod +x ./rmate
- mv ./rmate /usr/local/bin/rmate

#### make sure sshd start on.


---

### local server

#### install openSSH on windows

cmd->ssh to test.

#### install remote vscode

- click extension(ctrl+shift+X)
- search remote vscode and install and reload
- 查看->命令面板->remote(start server)
- 查看->调试控制台->terminal
- connect to remote server(ssh -R 52698:127.0.0.1:52698 root@192.168.123.123)
- open file(rmate -p 52698 ~/index.js)

### remote vscode plugin self-start.

- 文件->首选项->设置->search remote
