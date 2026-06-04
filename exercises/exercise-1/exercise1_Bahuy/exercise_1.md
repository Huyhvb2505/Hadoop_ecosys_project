### For Students
1. **Exercise Completion Reports**: Document for each exercise with:
   - Commands executed
   - Screenshots of results
   - Explanations of concepts learned
   - Challenges faced and solutions
2. **Final Integration Project**: Combining all technologies in a mini data pipeline
3. **Performance Analysis**: Comparison of different approaches and tools
4. **Presentation**: 10-minute demo of completed work


HDFS - BASICS

1.1: create & check the directory listing output
- cmd executed: 

   hdfs dfs -mkdir -p /user/student/{input,output,backup}  => create the directory /user/student/input | /user/student/output | /user/student/backup

   hdfs dfs -ls  /user/student => print the directory list output 

1.2:
-  Upload files to HDFS
   hdfs dfs -put /data/{employees.csv,sales.csv,customers.json} /user/student/input/ => upload all file from local file to hdfs

- check all file details 
   hdfs dfs -stat "%n %o %r %u %g %s %b %y" /user/student/input/*  

1.3:
- hdfs dfs -ls -R /user/student/    => print recursive dicrectory list

1.5:
 - cmd output
   + hdfs dfs -du -h /user/student/    => Xem dung lượng các file/thư mục trong /user/student/.
   737    2.2 K  /user/student/backup
   2.9 K  8.6 K  /user/student/input 
   15     45     /user/student/output
   + hdfs dfs -df -h                   => Xem tổng dung lượng HDFS: còn bao nhiêu, đã dùng bao nhiêu.
   Filesystem                Size   Used  Available  Use%
   hdfs://namenode:9000  1006.9 G  228 K    930.9 G    0%
   + hdfs fsck /user/student/ -files -blocks -locations    => Kiểm tra file trong HDFS đang được chia thành block nào, block nằm ở DataNode nào.
   Connecting to namenode via http://namenode:9870/fsck?ugi=root&files=1&blocks=1&locations=1&path=%2Fuser%2Fstudent
   FSCK started by root (auth:SIMPLE) from /172.18.0.3 for path /user/student at Thu Jun 04 06:37:25 UTC 2026
   /user/student <dir>
   /user/student/backup <dir>
   /user/student/backup/employees_backup.csv 737 bytes, replicated: replication=3, 1 block(s):  Under replicated BP-500205640-172.18.0.10-1780281750080:blk_1073741851_1027. Target Replicas is 3 but found 1 live replica(s), 0 decommissioned replica(s), 0 decommissioning replica(s).
   0. BP-500205640-172.18.0.10-1780281750080:blk_1073741851_1027 len=737 Live_repl=1  [DatanodeInfoWithStorage[172.18.0.5:9866,DS-705c784a-4170-405c-bd4b-044b21f762d6,DISK]]

   /user/student/input <dir>
   /user/student/input/customers.json 1618 bytes, replicated: replication=3, 1 block(s):  Under replicated BP-500205640-172.18.0.10-1780281750080:blk_1073741850_1026. Target Replicas is 3 but found 1 live replica(s), 0 decommissioned replica(s), 0 decommissioning replica(s).
   0. BP-500205640-172.18.0.10-1780281750080:blk_1073741850_1026 len=1618 Live_repl=1  [DatanodeInfoWithStorage[172.18.0.5:9866,DS-705c784a-4170-405c-bd4b-044b21f762d6,DISK]]

   /user/student/input/employees.csv 737 bytes, replicated: replication=3, 1 block(s):  Under replicated BP-500205640-172.18.0.10-1780281750080:blk_1073741848_1024. Target Replicas is 3 but found 1 live replica(s), 0 decommissioned replica(s), 0 decommissioning replica(s).
   0. BP-500205640-172.18.0.10-1780281750080:blk_1073741848_1024 len=737 Live_repl=1  [DatanodeInfoWithStorage[172.18.0.5:9866,DS-705c784a-4170-405c-bd4b-044b21f762d6,DISK]]

   /user/student/input/sales.csv 568 bytes, replicated: replication=3, 1 block(s):  Under replicated BP-500205640-172.18.0.10-1780281750080:blk_1073741849_1025. Target Replicas is 3 but found 1 live replica(s), 0 decommissioned replica(s), 0 decommissioning replica(s).
   0. BP-500205640-172.18.0.10-1780281750080:blk_1073741849_1025 len=568 Live_repl=1  [DatanodeInfoWithStorage[172.18.0.5:9866,DS-705c784a-4170-405c-bd4b-044b21f762d6,DISK]]

   /user/student/output <dir>
   /user/student/output/test.txt 15 bytes, replicated: replication=3, 1 block(s):  Under replicated BP-500205640-172.18.0.10-1780281750080:blk_1073741852_1028. Target Replicas is 3 but found 1 live replica(s), 0 decommissioned replica(s), 0 decommissioning replica(s).
   0. BP-500205640-172.18.0.10-1780281750080:blk_1073741852_1028 len=15 Live_repl=1  [DatanodeInfoWithStorage[172.18.0.5:9866,DS-705c784a-4170-405c-bd4b-044b21f762d6,DISK]]


   Status: HEALTHY
   Number of data-nodes:  1
   Number of racks:               1
   Total dirs:                    4
   Total symlinks:                0

   Replicated Blocks:
   Total size:    3675 B
   Total files:   5
   Total blocks (validated):      5 (avg. block size 735 B)
   Minimally replicated blocks:   5 (100.0 %)
   Over-replicated blocks:        0 (0.0 %)
   Under-replicated blocks:       5 (100.0 %)
   Mis-replicated blocks:         0 (0.0 %)
   Default replication factor:    3
   Average block replication:     1.0
   Missing blocks:                0
   Corrupt blocks:                0
   Missing replicas:              10 (66.666664 %)

   Erasure Coded Block Groups:
   Total size:    0 B
   Total files:   0
   Total block groups (validated):        0
   Total block groups (validated):        0
   Minimally erasure-coded block groups:  0
   Minimally erasure-coded block groups:  0
   Over-erasure-coded block groups:       0
   Under-erasure-coded block groups:      0
   Unsatisfactory placement block groups: 0
   Average block group size:      0.0
   Missing block groups:          0
   Corrupt block groups:          0
   Missing internal blocks:       0
   FSCK ended at Thu Jun 04 06:37:25 UTC 2026 in 5 milliseconds


   The filesystem under path '/user/student' is HEALTHY

