# minIO

#### Open port 9000 which is used by minIO
```
# firewall-cmd --add-port=tcp/9000 --permanent
# firewall-cmd --reload
```

#### Start a minio server pod
```
# podman run -d -p 9000:9000 --name minio \
  -e MINIO_ROOT_USER=<USERNAME> \
  -e MINIO_ROOT_PASSWORD=<PASWORD> \
  -v /payh/to/storage/to/use:Z \
  docker.io/minio/minio:edge \
  server /data
```

#### Start the mc client pod to configure minio server (you'll get a sh-4.4# prompt)
```
podman run -it --entrypoint=/bin/sh docker.io/minio/mc
sh-4.4#
```

#### Set up an alias to manage the minio server
```
sh-4.4# mc alias set minio http://<IP-OR-FQDN>:9000 <USERNAME> <PASSWORT>
mc: Configuration written to `/root/.mc/config.json`. Please update your access credentials.
mc: Successfully created `/root/.mc/share`.
mc: Initialized share uploads `/root/.mc/share/uploads.json` file.
mc: Initialized share downloads `/root/.mc/share/downloads.json` file.
Added `minio` successfully.
sh-4.4#
```

#### Create a bucket `test-bucket-a` and `test-bucket-b`
```
sh-4.4# mc mb minio/test-bucket-a minio/test-bucket-b
Bucket created successfully `minio/test-bucket-a`.
Bucket created successfully `minio/test-bucket-b`.
sh-4.4#
```

#### List all existing buckets on server minio
```
sh-4.4# mc ls minio
[2021-04-29 17:10:46 UTC]     0B test-bucket-a/
[2021-04-29 17:10:51 UTC]     0B test-bucket-b/
```

#### Remove an empty bucket
```
sh-4.4# mc rb minio/test-bucket-a
Removed `minio/test-bucket-a` successfully.
```

#### Remove a non-empty bucket
```
sh-4.4# mc rb minio/testbucket-b
Removed `minio/test-bucket-b` successfully.
```
