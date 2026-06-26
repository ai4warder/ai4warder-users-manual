# 如何设置Claude Desktop App

[Claude Desktop App是什么，TBD]

## 下载并安装Claude Desktop App

请自行到[Claude官网](https://claude.com/download)下载并安装Claude Desktop App。

## 设置Claude Desktop App连接Ai4Warder服务器

- 启动Claude Desktop，先不要登录；
- Mac端在顶部菜单栏寻找（Windows按官方文档，点击桌面版左上角三条横线）：
`Help -> Troubleshooting -> Enable Developer mode`
软件可能会自动重启；
- 下载[ai4warder_prod.json](./ai4warder_prod.json)配置文件。注意存储的文件名必须是`*.json`。
- 重新在菜单栏找`Developer -> Configure third-party inference`,点击进入配置界面, 点击右上角的“Default”按钮, 选择“import configuration”，导入上一步下载的配置文件,点击确认按钮；
![](./imgs/desktop-connections.png)
![](./imgs/import-conf.png)
![](./imgs/import-conf2.png)
- 填入你在本站创建的API KEY（可以与Claude Code共用KEY），保存。点击“Test connection”按钮，如果小圆点显示绿色则表示连接成功；如果显示橙色则表示测试返回错误。
![](./imgs/set-api-key.png)
- 点击窗口右下方应用和保存按钮，软件会自动登录。
- 此时，可以在Desktop软件中聊天测试。
![](./imgs/desktop-test.png)

- 结束。