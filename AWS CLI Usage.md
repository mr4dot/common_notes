# Git Tutorial

### Initial configuration

**Download and install aws cli :**  [AWS CLI Download link](https://awscli.amazonaws.com/AWSCLIV2.pkg)

### Configuration 
```shell
mr4dot@machine % aws configure
AWS Access Key ID [****************S4UQ]: <your acces key>
AWS Secret Access Key [****************9BJx]: <your secrete key>
Default region name [None]: 
Default output format [None]: 
mr4dot@machine PNPT % 
```

### S3 File Upload (Normal)
```shell
mr4dot@machine % aws s3 cp wa1.mp4 s3://cdn.example.com/f/t/wa1.mp4 --acl public-read --storage-class INTELLIGENT_TIERING

out put eg :-

upload: ./<filename> to s3://cdn.example.com/<filename>
```

### S3 File Upload Download only mod
```shell
mr4dot@machine % aws s3 cp <local file path> s3://cdn.domain.com/<filename> --acl public-read --storage-class INTELLIGENT_TIERING --no-guess-mime-type --content-type="application/octet-stream" --metadata-directive="REPLACE"

out put eg :-

upload: ./<filename> to s3://cdn.example.com/<filename>

```

