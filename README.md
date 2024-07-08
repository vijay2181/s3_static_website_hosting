# S3 Static website hosting with `Cloudfront, Route53 and ACM by AWS`

![image](https://github.com/vijay2181/s3_static_website_hosting/assets/66196388/97c0538e-ffa9-4fdc-bced-392d1593a6dd)


```
S3 Static website hosting with Cloudfront, Route53 and ACM by AWS

- if we want to use domain name redirection to s3 bucket, then domain name and s3 bucket should be of same name
- if we want to use s3 bucket to host a static website, we need to upload all your content to s3 bucket
- to redirect traffic from http to https, we need SSL certificates(ACM by AWS)
- since cloudfront is regional service(us-west-2, for ex)to associate any ssl certificate to cloudfront,
  then we need to create ssl certificate on us-west-2 only, otherwise we cannot use them
- create a public hosted zone with same domain name, whenever we create this hosted zone,
  we will get NS,SOA,CNAME records. for NS records, we need to configure Nameservers in aws
  if your domain purchased from godaddy etc...
- now route53 hosted zone is ready, then we need to go for ACM and request for certificate
  request public certificate
  enter domain name
  use dns validation to prove ownership of the domain
  select rsa 2048 
  request
  click on request id and scroll down to validate the domain ownership(that you are the correct owner of domain)
  click on 'Create records in Route53'
  select the hosted zone where you want to create the record
  create record
  after sometime, after validation, ssl certificatewill be issued for your domain and that is amazon issued one
  goto certificates and check the status

- goto cloudfront distribution
  create cloudfront distribution
  provide origindomain: s3 bucket which is created same as domain name
  origin acces(dont make bucket public, make it assessible to only cloudfront): origin access control settings
  so only cloudfront will be allowed to access data from that s3 bucket. so bucket is secure from public access to people 
  create control setting
  name: bucket name
  create 
  we will get a bucket policy, that we need to update
  viewer: Redirect HTTP to HTTPS
  waf: Do not enable security protections
  CNAME: ADD YOUR DOMAIN HERE 
  select ssl certificate
  Default root object: index.html
  create distribution
  we will get a bucket policy(cloudfornt will create it), copy that 
  it will take some time in cloudfront to deploy
  goto target s3 bucket
  goto permissions tab, 
  block public access: enable
  bucket policy: edit and paste the copied bucket policy from cloudfornt distribution
  save changes
  so only cloudfront will be allowed to access data from that s3 bucket.
  so bucket is secure from public access to people 

- goto route53 and map our domain name to cloudfront distribution
  goto created public hostedzone previously
  create a new record of type A
  enable alias -> route traffic to (Alias to CloudFront Distribution)
  select the distribution
  create record

- so after deploying, we will access our application using domain name, then it is redirecting to cloudfront
  distribution which has access to s3 bucket content, cloudfront stores content in nearest edge locations.
  
we can also access with cloudfornt distribution url

- if we want to deliver static content directly from s3 bucket, then we need to make bucket and inside bucket objects public.
  so added cloudfornt distribution infront of s3 bucket, then have route53 infront of cloudfornt with ssl, even if you give http
  it will redirect to https, this setting we have configured at cloudfront and domain has ssl certificate 
  ```

