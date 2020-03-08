# 群晖NAS使用笔记

## 为Video Station改变搜刮器
1. ssh 登录群晖系统
1. 执行wget https://raw.githubusercontent.com/jswh/synology_video_station_douban_plugin/master/install.sh
1. 执行sudo bash install.sh uninstall （第一次安装可以跳过这个步骤）
1. 执行sudo bash install.sh install

### 其它源
```bash
# 豆瓣源：
sudo wget -N --no-check-certificate https://sh.9hut.cn/dsvp.sh && sudo bash dsvp.sh install


# 时光网源：
sudo wget -N --no-check-certificate https://sh.9hut.cn/dsvpmt.sh && sudo bash dsvpmt.sh install

# 暗黑更改源：
wget https://gitee.com/challengerV/dsm_javdb_patch/raw/master/dsm_javdb_patch.sh && sh dsm_javdb_patch.sh install
```
