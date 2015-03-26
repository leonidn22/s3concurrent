= Introduction

s3concurrent is designed to dump (maybe upload too?) files with deep file structures out from S3 in a quick and concurrent manner.

Since S3 does not have a real folder structure, all the files are put into buckets using their key names to mimic logical hierarchy.
For example, S3 files with key names a_folder/file_1.png, a_folder/file_2.png, and a_folder/file_3.png are considered to be in a logical folder `a_folder`.

This key-value model grant S3 a easier approach to handle redundancy and direct file access.
However when it comes to reconstructing folder structure from remote, current practices in multiple open source projects traverse through all the
related keys (with prefix filters in boto) multiple times to pinpoint the exact keys/files to download.
This is approach is inefficient for mass file syncing.

s3concurrent takes a different approach.
It loops through all the targeted keys only once, construct local folder structure as it goes, and en-queue all the to-be-downloaded key into a downloadable queue.
With 20 different threads, it starts to consume the queue and download all the files into their respective local locations.

= Installation

```
pip install s3concurrent
```

= Detailed Usages

s3concurrent brings you a local command you can use directly in your terminals.

Assuming you are downloading the entire clone of pip repositories into your local mirror from S3:

```
s3concurrent <your_S3_Key> <your_S3_Secret> <your_S3_bucket_name> --destination_folder my_local_mirror_dir --prefix mirror/pypi
```

== Optional
=== destination_folder
Absolute or relative path to the destination local folder. The default value is the current directory.

=== prefix
The folder prefix in S3. If your wish to download all the files under folder_a/folder_b in a bucket.
All you have to do here is to specify the prefix to `folder_a/folder_b`