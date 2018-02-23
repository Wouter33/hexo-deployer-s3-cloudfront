# hexo-deployer-s3-cloudfront

Amazon S3 and Cloudfront deployer plugin for [Hexo](http://hexo.io/). Based on Josh Strange's orginial plugin.

## Installation

``` bash
$ npm install hexo-deployer-s3-cloudfront --save
```

## Usage

Add the plugin in the plugins list in `_config.yml`:

```plugins:
- hexo-deployer-s3-cloudfront
```

and configure the plugin in the same file with:

``` yaml
# You can use this:
deploy:
  type: s3-cloudfront
  bucket: <S3 bucket>
  aws_key: <AWS id key>  // Optional, if the environment variable `AWS_KEY` is set
  aws_secret: <AWS secret key>  // Optional, if the environment variable `AWS_SECRET` is set
  concurrency: <number of connections> // Optional
  force_overwrite: <true/false> // Optional: If existing files should be forcefully overwritten on S3. Default: true
  region: <region>  // Optional, default: us-standard
  prefix: <cloudfront distribution> // Optional: S3 key prefix ending in /
  cf_distribution: <cloudfront distribution> // Optional: Which distribution should be invalidated?
  headers: <headers in JSON format> // pass any headers to S3, usefull for metadata cache setting of Hexo assets
```

NOTE: If you are switching from hexo-deployer-s3, note that the environment variables changed.
NOTE: cf_distribution should be all-caps and about 14 characters with no domain - don't include the cloudfront.net
NOTE: You'll want to add cloudfront permissions to the policy used to deploy the site:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::example.com"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:*"
            ],
            "Resource": [
                "arn:aws:s3:::example.com/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudfront:CreateInvalidation",
                "cloudfront:GetInvalidation",
                "cloudfront:ListInvalidations",
                "cloudfront:UnknownOperation"
            ],
            "Resource": "*"
        }
    ]
}
```

#### Example: header Cache-Control

``` yaml
deploy:
  type: s3-cloudfront
  bucket: my-site-bucket
  cf_distribution: mydistributionid
  headers: {CacheControl: 'max-age=604800, public'}
```

This will set "Cache-Control" header in every file deployed to max-age 1 week. This solves "Leverage browser caching" on most page speed analyzers. For custom metadata use:

``` yaml
  headers: {Metadata : { x-amz-meta-mykey: "my value" }}
```

## Contributors

- Wouter van Lent ([wouter33](https://github.com/wouter33))
- Josh Strange ([joshstrange](https://github.com/joshstrange); original implementation)
- Josenivaldo Benito Jr. ([JrBenito](https://github.com/jrbenito))

## License

MIT
