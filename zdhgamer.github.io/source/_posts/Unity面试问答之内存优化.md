---
title: Unity面试问答之内存优化
date: 2019-02-19 21:22:57
tags:
---

# 内存优化
1. 资源优化
   1.1 ab包在完全没有引用的时候，AssetBound.Unload(true)，这样会把资源全部在内存中卸载掉，有引用该资源的地方也会被卸载掉。
   1.2 Resources加载的时候，在资源使用完毕之后，需要把资源Resources.UnloadAsset();卸载非 GameObject类型的资源，会将内存中已加载资源及其克隆体卸载。 Resources.UnloadUnusedAssets(),这是一个异步操作，会卸载掉内存中和加载出来的没有使用的资源。
   1.3 贴图资源打包图集最大为1024*1024，图集打包合理一些。
   1.4 纹理和图集的压缩格式。比如Android平台的ETC2、iOS平台的PVRTC、Windows PC上的DXT等等。
   1.4 图集和纹理尽量不开MinMap，在没有特殊需求下，也不要开启Read/Write Enable,
   1.5 打包ab的时候，去除冗余资源。
2. 代码优化
   2.1 缓存获取到的组件，不要频繁的GetComponent，在Unity的API中返回数组和集合的情况，也要缓存起来，这样能减少GC和内存。
   2.2 使用对象池。
   2.3 不要频繁的new 对象和数组。
   2.4 log日志输出也会占用大量内存。
   2.5 string拼接多用stringBuilder
   2.6 避免ui网格重建，动静分离；NGUI是分离到不同的UIPanel下，UGUI是分离到不同的Canvas下。
   2.7 缓存获取到的主相机
3. 手动GC
   3.1 在场景加载完之后手动调用一次GC.Collect()