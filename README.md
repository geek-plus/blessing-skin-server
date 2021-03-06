# Blessing Skin Server

优雅的开源 PHP Minecraft 皮肤站。[演示地址](https://skin.prinzeugen.net/)

![screenshot](https://img.prinzeugen.net/image.php?di=FIQD)

特性：
-----------

- 支持 [UniSkinAPI](https://github.com/RecursiveG/UniSkinServer/blob/master/doc/UniSkinAPI_zh-CN.md)
- 支持 CustomSkinLoader API
- 同时支持旧版样式链接

快速使用：
-----------

1. 下载源码，并在 `config.php` 中配置你的数据库连接信息
2.  运行 `./admin/install.php`
3. 如果你是用的是 Nginx，请配置你的 `nginx.conf` 并加入重写规则
4. 注册一个新账户或者使用 `admin/123456` （管理员账户）登录
5. 可以上传你的皮肤&皮肤啦
6. 在你所使用的皮肤 Mod 配置文件中加入你的地址
7. 完成啦~

服务器配置：
------------

在你的 `nginx.conf` 中加入如下 rewrite 规则**（重要）**：

```
rewrite ^/([^/]*).json$ /get.php?type=json&uname=$1 last;
rewrite ^/(skin|cape)/([^/-]*)(|-)(|alex|steve).png$ /get.php?type=$1&model=$4&uname=$2 last;
# 以下是可选内容
rewrite ^/(usm|csl)/([^/]*).json$ /get.php?type=json&uname=$2&api=$1 last;
rewrite ^/(usm|csl)/textures/(.*)$ /textures/$2 last;
```
你可以使用可选的重写规则来同时支持 CustomSkinLoader API 和 UniSkinAPI。如何同时支持会在下面 Mod 配置中说明。

如果你将皮肤站放在子目录中，你需要把重写规则改成类似于**这样**：

```
rewrite ^/subdir/([^/]*).json$ /subdir/get.php?type=json&uname=$1 last;
```

注意 `^/` 和 `/get.php` 前都要加上你的子目录名。

现在你可以访问 `http://example.com/username.json` 来得到你的首选 API 的 JSON 用户数据。另外一个 API 的 JSON 数据可以通过访问 `http://example.com/(usm|csl)/username.json` 得到（需配置可选重写规则）。

上传完皮肤后，你就可以访问 `http://example.com/skin/username.png` 得到你的首选模型皮肤啦。 披风图片在这里：`http://example.com/cape/username.png` 。你还可以访问 `http://example.com/skin/username-(alex|steve).png` 来得到用户的 Alex/Steve 模型的皮肤文件（用户没上传则返回空）。

客户端配置：
------------

#### UniSkinMod 1.3 版及以上

在你 MC 客户端的`.minecraft/config/UniSkinMod.cfg` 中加入你的皮肤站根地址：

举个栗子：

```
# SkinMe Default
Root: http://www.skinme.cc/uniskin
# Your Server
Root: http://example.com
```
如果你把皮肤站安装到子目录的话，请一起带上你的子目录。如果你的皮肤站首选 API 为 CustomSkinLoader API 的话，你需要在 UniSkinMod 配置文件中填入类似于 `http://example.com/usm` 来支持 UniSkinMod。

#### UniSkinMod 1.3 版以下

同样是在 `.minecraft/config/UniSkinMod.cfg` 中配置你的皮肤站地址，但是稍有点不一样。旧版的 UniSkinMod 是不支持 Json API 的，而是使用了传统图片链接的方式（其实这样的话皮肤站爷好实现）：

举个栗子：

```
Skin: http://skins.minecraft.net/MinecraftSkins/%s.png
Cape: http://skins.minecraft.net/MinecraftCloaks/%s.png
# Your Server
Skin: http://example.com/skin/%s.png
Cape: http://example.com/cape/%s.png
```

这是通过 URL 重写（伪静态）实现的，所以皮肤站目录下没有 `skin` 和 `cape` 目录也不要惊讶哦。

#### CustomSkinLoader 13.1 版以下：

在 `.minecraft/CustomSkinLoader/skinurls.txt` 中添加如下地址：

```
http://example.com/skin/*.png
http://skins.minecraft.net/MinecraftSkins/*.png
http://minecrack.fr.nf/mc/skinsminecrackd/*.png
http://www.skinme.cc/MinecraftSkins/*.png
```

注意你需要将你的皮肤站地址放在配置文件最上方以优先加载。

同理在 `.minecraft/CustomSkinLoader/capeurls.txt` 中加入：

```
http://example.com/cape/*.png
```

#### CustomSkinLoader 13.1

等待作者发布

常见问题：
------------

#### 游戏中皮肤不显示？

请先确认你的皮肤站 URL 重写规则已经配置正确，并且可以正常获取皮肤图片。

如果还是不能显示皮肤，请阅读您所使用的皮肤 Mod 的 FAQ。

还是不行德胡，请在启动器开启调试模式，并且查看所有关于 skin 的日志。一般来说看了就可以明白了，如果还是不明白请邮件 [联系我](mailto:h@prinzeugen.net)（带上你的日志）。

如果一切都正常工作，你就可以在游戏中看到你的皮肤啦~

![screenshot2](https://img.prinzeugen.net/image.php?di=EV1E)

版权：
------------
Blessing Skin Server 程序是基于 GUN General Public License 开放源代码的自由软件，你可以遵照 GPL 协议来修改和重新发布这一程序。

程序原作者为 [@printempw](https://prinzeugen.net/)，转载请注明。

