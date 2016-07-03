title: hexo之next主题优化整理
date: 2016-06-21 14:31:27
tags:
---

### 多说配置
使用多说前需要先在 多说 创建一个站点。具体步骤如下：

1. 登录后在首页选择 “我要安装”。
创建站点，填写站点相关信息。注意，多说域名 这一栏填写的即是你的 duoshuo_shortname。
2. 编辑站点的 _config.yml 注意，不是主题的_config.yml文件 ，添加 duoshuo_shortname 字段，设置如下
`duoshuo_shortname: your-duoshuo-shortname`
（注）duoshuo short name: 你的多说二级域名去掉 .duoshuo.com 部分
3. 如需取消某个页面的评论，在md文件的front-matter中增加
`comments: false`


### 参考链接
- [hexo之next主题优化整理](http://michaelxiang.me/2015/11/30/hexo-next-optimize/)
- http://zhiho.github.io/2015/09/29/hexo-next/
- http://www.selfrebuild.net/2015/06/24/Github-Hexo-- Next%E4%B8%BB%E9%A2%98%E4%B8%AA%E6%80%A7%E5%8C%96%E5%AE%9A%E5%88%B6/
- http://michaelxiang.me/    
- http://michaelxiang.me/2015/11/30/hexo-next-optimize/
- https://github.com/iissnan/hexo-theme-next/wiki/%E8%AE%BE%E7%BD%AE%E5%A4%9A%E8%AF%B4-DISQUS