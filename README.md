# InFlow

# 信息流应用终结方案

## 要解决的问题
1. 实现列表item样式需要配合繁多的adapter逻辑 
2. 页面中页卡数量越来越多带来的性能问题，inflate耗时严重
3. item依赖的数据源结构越来越复杂，耦合严重

## 框架提供如下核心能力：
为了实现上述目标，我们需要做如下几个方面的工作

1. `划分层级`：将信息流划分为4层结构
页面：通常为一个activity或fragment，信息流业务的基本单位；内部可能嵌套多个“频道”
频道：通常为一个fragment，列表的容器，数据加载、显示的基本单位；框架提供了基类 BaseLifecycleFragment
列表：通常为一个RecyclerView，列表item的容器，数据处理和样式展示的基本单位；框架提供了基类 BaseRecyclerAdapter
列表项：数据容器 BaseDataHolder、UI容器 BaseViewHolder，样式逻辑的基本单位，业务更新最频繁的一层

2. `抽象解耦`：面向接口的开发方式
页面生命周期：IPageLifecycle
频道生命周期：ITabPageLifecycle
列表生命周期：IListViewLifecycle
列表项生命周期：IListViewHolderLifecycle

3. `列表复用`：列表展示能力全局通用
数据结构 映射到 BaseDataHolder：GlobalDataHolderCreator
根据 BaseDataHolder 创建 BaseViewHolder：GlobalViewHolderCreator
注入式的全局列表item创建机制 GlobalListItemRegister

4. `频道复用`：fragment展示能力全局通用
支持fragment复用的pagerAdapter：GlobalFragmentStatePagerAdapter
注入式的全局fragment创建机制：GlobalFragmentCacheManager
抽象的频道数据协议：IChannelModel

5. `性能优化`：卡顿的瓶颈——inflate
跨列表item复用、ViewHolder线程预加载：GlobalRecycledViewPool