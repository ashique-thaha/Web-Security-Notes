
`intro:`

We should take a look at the company’s third-party hosting footprint. 

For example, look for the organisation’s `S3 buckets`. S3, which stands for `Simple Storage Service`, is Amazon’s online 
storage product. 

Organisations can pay to store resources in buckets to serve in their web applications, or they can use S3 buckets as a backup or storage location. 

If an organisation uses Amazon S3, its S3 buckets can contain hidden endpoints, logs, credentials, user information, source code, and other information that might be useful to us.

`content:`

==How do you find an organisation’s buckets?== One way is through ==Google dorking==. Most buckets use the URL format `BUCKET.s3.amazonaws.com` or `s3.amazonaws.com/BUCKET`

so the following search terms are likely to find results:

#AwsFinding
```
site:s3.amazonaws.com COMPANY_NAME
site:amazonaws.com COMPANY_NAME
```


If the company uses custom URLs for its S3 buckets, try more flexible search terms instead. Companies often still place keywords like `aws` and `s3` in their custom bucket URLs, 


so try these searches:
#Aws_custom
```
amazonaws s3 COMPANY_NAME
amazonaws bucket COMPANY_NAME
amazonaws COMPANY_NAME
s3 COMPANY_NAME
```

Another way of finding buckets is to search a company’s public GitHub repositories for S3 URLs. Try searching these repositories for the term s3.

#aws_enum_tool
`GrayhatWarfare`is an online search engine you can use to find publicly exposed S3 buckets. 

It allows you to search for a bucket by using a keyword. Supply keywords related to your target, such as the application, project, or organisation name, to find relevant buckets.

Finally, you can try to `brute-force buckets` by using keywords. #s3_buckets_bruteforce #aws_enum_tool
`Lazys3 (https://github.com/nahamsec/lazys3/)` is a tool that helps you do this. It relies on a wordlist to guess buckets that are permutations of common bucket names.

#aws_enum_tool #s3_cetrificate_parse
Another good tool is `Bucket Stream  (https://github.com/eth0izzle/bucket-stream/)`, which parses certificates belonging to an organisation and finds S3 buckets based on permutations of the domain names found on the certificates. 

`Bucket Stream` also automatically checks whether the bucket is
accessible, so it saves you time.

Once you’ve found a couple of buckets that belong to the target organisation, 

use the `AWS command line tool` to see if you can access one. Install the tool by using the following command:

#AwsCommandLine
```shell
pip install awscli
```

Then configure it to work with AWS by following Amazon’s documentation at `https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html`.

Now you should be able to access buckets directly from your terminal via the `aws s3` command. 

Try listing the contents of the bucket you found:

```shell
aws s3 ls s3://BUCKET_NAME/
```

If this works, see if you can read the contents of any interesting files by copying files to your local machine:

```shell
aws s3 cp s3://BUCKET_NAME/FILE_NAME/path/to/local/directory
```

Gather any useful information leaked via the bucket and use it for future exploitation! If the organisation reveals information such as active API keys or personal information, we should report this right away.


Exposed S3 buckets alone are often considered a vulnerability. we can also try to upload new files to the bucket or delete files from it. 

If you can mess with its contents, you might be able to tamper with the web application’s operations or corrupt company data.

For example, this command will copy your local file named TEST_FILE into the target’s S3 bucket:

```shell
aws s3 cp TEST_FILE s3://BUCKET_NAME/
```


And this command will remove the TEST_FILE that you just uploaded:

```shell
aws s3 rm s3://BUCKET_NAME/TEST_FILE
```

These commands are a harmless way to prove that you have write access to a bucket without actually tampering with the target company’s files.

Always upload and remove your own test files. Don’t risk deleting
important company resources during your testing.