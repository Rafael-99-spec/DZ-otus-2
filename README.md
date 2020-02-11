# DZ-otus-2
Домашнее задание 2
В Vagrantfile прописан еще один 5-й диск.
В секторе shell добавлены команды позволяющие создать RAID массив 5-го уронвя с помощью утилиты mdadm и файл конфигурации содержаюий информацию по созданному массиву - /etc/mdadm/mdadm.conf.
Виртуальная машина разворачивается сразу с подключенным рейд массивом.
В целях тестирования работаспособности рейд массива была выполнена имитация сбоя одного из дисков, в виде его удаления и добваления нового.
[root@otuslinux vagrant]# mdadm /dev/md0 --fail /dev/sdd
mdadm: set /dev/sdd faulty in /dev/md0
[root@otuslinux vagrant]# mdadm /dev/md0 --remove /dev/sdd
mdadm: hot removed /dev/sdd from /dev/md0
[root@otuslinux vagrant]# cat /proc/mdstat
Personalities : [raid6] [raid5] [raid4] 
md0 : active raid5 sdf[5] sde[3] sdc[1] sdb[0]
      1015808 blocks super 1.2 level 5, 512k chunk, algorithm 2 [5/4] [UU_UU]
      
unused devices: <none>
[root@otuslinux vagrant]# mdadm /dev/md0 --add /dev/sdd
mdadm: added /dev/sdd
[root@otuslinux vagrant]# cat /proc/mdstat
Personalities : [raid6] [raid5] [raid4] 
md0 : active raid5 sdd[6] sdf[5] sde[3] sdc[1] sdb[0]
      1015808 blocks super 1.2 level 5, 512k chunk, algorithm 2 [5/4] [UU_UU]
      [=========>...........]  recovery = 47.6% (121344/253952) finish=0.0min speed=121344K/sec
      
unused devices: <none> 
[root@otuslinux vagrant]# cat /proc/mdstat
Personalities : [raid6] [raid5] [raid4] 
md0 : active raid5 sdd[6] sdf[5] sde[3] sdc[1] sdb[0]
      1015808 blocks super 1.2 level 5, 512k chunk, algorithm 2 [5/5] [UUUUU]
      
unused devices: <none>
