hugo -D
aws s3 rm s3://blog-b0rr3g0 --recursive
aws s3 cp public/ s3://blog-b0rr3g0 --recursive
aws cloudfront create-invalidation --distribution-id {id} --paths "/*"
