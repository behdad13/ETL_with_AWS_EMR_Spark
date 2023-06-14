# ETL_with_AWS_EMR_Spark

to run the code, follow the steps
1. create an EMR cluster in AWS. Don't forget to assign the appropriate IAM permission to the EMR cluter.
2. clone the repo and run the app.py in your IDE to make sure it works well. (configure the variables before)
3. copy app.py file and .zip file to the EMR cluster by the command:
```
scp -i <path to your key> <path to your files> ec2-user@<ec2 instance number>:~
```
4. make a folder (githubactivity) in your cluster and move files to it:
```
mkdir githubactivity
mv ghactivity_spark.zip githubactivity
mv app.py githubactivity
```
5. create a directory in HDFS and set the ownership to the user "ec2-user" for further operations and access.
```
sudo -u hdfs hdfs dfs -mkdir /user/ec2-user
sudo -u hdfs hdfs dfs -chown -R ec2-user:ec2-user /user/ec2-user
```
6. configure variables in your cluster environment by following commands:
```
export ENVIRON=DEV
export SRC_DIR=s3://emr-etl-test/downloader-ben/landing/
export SRC_FILE_FORMAT=json
export TGT_DIR=s3://emr-etl-test/downloader-ben/raw/
export TGT_FILE_FORMAT=parquet
export SRC_FILE_PATTERN=2023-01-01
```
7. submit the spark job by the command:
```
spark-submit --master yarn --py-files ghactivity_spark.zip app.py
```


landing directory
<img width="799" alt="image" src="https://github.com/behdad13/ETL_with_AWS_EMR_Spark/assets/58978680/01b4991e-9692-4db3-83de-725c22820fc2">


raw directory
<img width="799" alt="image" src="https://github.com/behdad13/ETL_with_AWS_EMR_Spark/assets/58978680/67b4013b-f3d6-40e5-8198-328b60b9214f">

