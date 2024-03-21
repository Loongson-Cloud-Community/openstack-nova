一.源码移植
运行以下命令在源码中加入LA架构补丁：
patch -p1 < 0001-support-loongarch64.patch

备注：nova/virt/libvirt/driver.py 文件中指定了cpu mode="custom",models="Loongson-3A5000",这里是根据virtmanage里面CPU选择来确定的，不同的系统可能会存在差异。

二.编译命令
参考https://gitee.com/src-openeuler/openstack-nova/tree/Multi-Version_OpenStack-Wallaby_openEuler-24.03-LTS-Next/   的spec文件整理的构建命令：

(1)安装依赖：
apt install -y python3-oslo.cache python3-oslo.concurrency python3-oslo.config python3-oslo.context python3-oslo.log python3-oslo.limit python3-oslo.reports python3-oslo.serialization python3-oslo.upgradecheck python3-oslo.utils python3-oslo.db python3-oslo.rootwrap  python3-oslo.messaging python3-oslo.policy python3-oslo.privsep python3-oslo.i18n python3-oslo.service python3-oslo.middleware python3-oslo.versionedobjects python3-os-brick python3-os-resource-classes python3-os-traits  python3-os-vif

apt install -y python3-nova python3-oslo.reports

(2)构建命令：
PYTHONPATH=$PYTHONPATH:/. oslo-config-generator --config-file=etc/nova/nova-config-generator.conf  //生成文件etc/nova/nova.conf.sample

PYTHONPATH=$PYTHONPATH:/. oslopolicy-sample-generator --config-file=etc/nova/nova-policy-generator.conf      //生成文件etc/nova/policy.yaml.sample

/usr/bin/python3 setup.py  build  --executable="/usr/bin/python3 -s"  //该命令会生成build目录

python3 setup.py compile_catalog -d build/lib/nova/locale -D nova   //将build目录下的xxx_MESSAGES目录下的文件编译成.mo文件

