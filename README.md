# ETL_with_AWS_EMR_Spark
This repository performs an ETL job on GitHub activity JSON data. It converts the data format from JSON to Parquet and applies partitioning to enhance data reading speed. You can download the dataset from this link (https://www.gharchive.org/) and store it in your AWS S3 bucket. First, create a s3 bucket and create two folders, raw & landing. Then, store the downloaded data into the landing folder.

To run the code, please follow these steps:

1. Create an EMR cluster on AWS. Remember to assign the appropriate IAM permissions to the cluster.
2. Clone this repository and ensure that the app.py file runs successfully in your IDE (don't forget to configure the variables beforehand).
3. Create a .zip file using the following commands:
```
python –m git-venv
source git-venv/bin/activate
zip –r benactivity.zip *.py
```

4. Copy the app.py file and the .zip file to the EMR cluster using the following command:
```
scp -i <path to your key> <path to your files> ec2-user@<ec2 instance number>:~
```

5. Connect to the EMR cluster via your terminal using the following command:
```
ssh -i <path to your key> ec2-user@<ec2 instance number>
```

6. Create a folder called "githubactivity" on your cluster and move the files into it:
```
mkdir githubactivity
mv ghactivity_spark.zip githubactivity
mv app.py githubactivity
```

5. Create a directory in HDFS and set the ownership to the user "ec2-user" for further operations and access:
```
sudo -u hdfs hdfs dfs -mkdir /user/ec2-user
sudo -u hdfs hdfs dfs -chown -R ec2-user:ec2-user /user/ec2-user
```

6. Configure the variables in your cluster environment using the following commands:
(use your own s3 bucket folder for source and target directory)
```
export ENVIRON=DEV
export SRC_DIR=s3://emr-etl-test/downloader-ben/landing/
export SRC_FILE_FORMAT=json
export TGT_DIR=s3://emr-etl-test/downloader-ben/raw/
export TGT_FILE_FORMAT=parquet
export SRC_FILE_PATTERN=2023-01-01
```

7. Submit the Spark job using the following command:
```
spark-submit --master yarn --py-files ghactivity_spark.zip app.py
```


landing directory

<img width="799" alt="image" src="https://github.com/behdad13/ETL_with_AWS_EMR_Spark/assets/58978680/01b4991e-9692-4db3-83de-725c22820fc2">




raw directory

<img width="799" alt="image" src="https://github.com/behdad13/ETL_with_AWS_EMR_Spark/assets/58978680/67b4013b-f3d6-40e5-8198-328b60b9214f">

