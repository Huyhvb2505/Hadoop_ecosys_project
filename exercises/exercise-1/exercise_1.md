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
