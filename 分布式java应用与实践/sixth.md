第六章--构建高可用的系统
* 避免系统出现单点  一般的处理办法是把系统部署在多台机器上，其中每一台机器都提供相同的功能，这种系统环境称为集群。在集群的环境下，除了使得系统做到可伸缩，同时也要注意到2个问题，
  * 1.如何使得访问能够均衡的作用在这些服务器上；
  * 2.当其中的一个服务器出现问题时，使得访问不在作用在这个服务器上；
* 解决上述的问题，用到负载均衡的技术，分为硬件负载均衡和软件负载均衡，使得有一台服务器是处于预备的状态的，当出现问题时，会自动替补；
* 如何选择实际处理机器，实际是IP地址的转发：
  * 1.随机选择；
  * 2.hash选择，对资源的地址做hash；
  * 3.顺序选择；
  * 4.权重选择；
  * 5.负载选择；
  * 6.链接选择；
* 响应返回方式：
  * 1.通过负载均衡返回，返回的信息原路返回；
  * 2.响应直接返回请求方，这样就将请求包和响应包分开处理，从而缓解负载均衡器的压力