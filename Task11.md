# Task No. 11
## :memo: Created S3 bucket & uploaded files using AWS CLI

:heavy_check_mark: **Created S3 bucket:**
```console
aws s3 mb s3://ali-first-bucket-s3
```

#
:heavy_check_mark: **Uploaded files to S3 from local PC:**
```console
aws s3 cp "C:\Users\Ali\Downloads\files" s3://ali-first-bucket-s3/ --recursive
```

#
:heavy_check_mark: **Deleted all files:**
```console
aws s3 rm s3://ali-first-bucket-s3 --recursive
```

#
:heavy_check_mark: **Deleted S3 bucket:**
```console
aws s3 rb s3://ali-first-bucket-s3
```
#
📌 **Demo:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/b7423427-dec6-4527-8d76-949dccd489f7)
