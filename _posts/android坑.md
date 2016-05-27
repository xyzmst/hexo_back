title: android坑
date: 2016-05-04 15:22:29
tags:
---
- 1 android4.4 windows 添加过view之后 无法再添加其他view
场景：windowmanager 添加一个 surfaceview后 无法显示dialog
- 2 viewpager 中的 fragment 再嵌套 fragment，嵌套的fragment 无法显示
场景：不同分类的左右滑的列表，每个fragment 用封装好的 ListFragment 为了方便应用列表，
不显示
解决方法：fragment 添加 子fragment 一定要使用 getChildFragmentManager() 而非
getActivity().getSupportFragmentManager()，并且调试中 不是换上childfragment马上就好用，
怀疑是as 没有刷新代码
同样尝试过配置到xml中的方式，但是并没有解决问题，添加不同的fragment只显示1个fragment
添加同一个fragment无法获取fragment，无论通过getSupportFragmentManager还是getChildFragmentManage
时间：2016/05/19
- 3 gradle 配置会导致，应该运行慢，试过 2g 4g 6g 8g 16g， debug渠道 会忽快忽慢，不稳定
```
dexOptions {
        javaMaxHeapSize "4g"
    }
```
时间：2016/05/24


