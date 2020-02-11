# xOpera deploy of simple thumbnail generator example to AWS Lambda

**Note**: This is still only a sandbox - working example with a plenty of room for optimisation. 

prerequisites:
* install AWS modules `botocore`, `boto3`, configure your AWS credentials
* prepare a zip including the lambda application (or take one prepared form [here](https://github.com/radon-h2020/FaaS-thumbnail-generator-python), find it in `binary-zip` put at your machine as `/tmp/X-test-ImageRes.zip`) note the **/tmp** folder!
* similarly, as current version of xOpera does not support `lookup` function, the path is currently fixed in to `/tmp/policy.json`, please copy it there initiating the:
	 `cp playbooks/aws_role/policy.json /tmp/`
* the latest version of Ansible (`2.9` will be ok, but currently `devel` is required). To install devel version: ```pip install git+https://github.com/ansible/ansible.git@devel```
* modify values of properties in `resize_service_opera.yml`, where appropriate (aws regions, bucket names, etc.)
* find opera orchestrator here: https://github.com/xlab-si/xopera-opera, install it and run this blueprint with

`opera deploy <some deploy name> resize_service_opera.yml`

## AWS Api Gateway

prerequisites:
* if testing API Gateway copy swagger yml file to tmp with: `cp playbooks/api_gateway/RadonApiGateway.yaml /tmp/`.

Run Api gateway example with:
`opera deploy aws-api-gateway-thumbgen-example.yml`

You can test the deployment of your API Gateway by uploading the image to your original bucket and then invoking the
function in API by testing the POST method. Here you can provide the following request body:

```json
{
  "Records": [
    {
      "s3": {
        "bucket": {
          "name": "bucket-name"
        },
        "object": {
          "key": "image.jpg"
        }
      }
    }
  ]
}
```