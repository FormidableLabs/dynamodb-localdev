Localdev DynamoDB
=================

## Install

### Node.js

Get `nvm` and use `nvm use 10.15` for an appropriate Node.js to match Lambda runtime.

### Docker

Get `docker` and `docker-compose`. See https://docs.docker.com/compose/install/

Check you're set up:

```sh
$ docker --version
$ docker-compose --version
```

### AWS Tools

Certain administrative / development work require the AWS CLI tools to prepare and deploy our staging / production services. To get those either do:

```sh
# Install via Python
$ sudo pip install awscli --ignore-installed six

# Or brew
$ brew install awscli
```

After this you should be able to type:

```sh
$ aws --version
```

## Get localdev DB running

For a real project, feel free to script from `package.json:scripts`.

```sh
# Start up the server!
$ docker-compose up

# Test it out
# (And yes, we need the mocked AWS_* env vars...)
$ AWS_ACCESS_KEY_ID=mock AWS_SECRET_ACCESS_KEY=mock \
  aws dynamodb list-tables --endpoint-url http://0.0.0.0:8000 --output json
es --endpoint-url http://0.0.0.0:8000 --output json
{
    "TableNames": []
}

# Create a table
$ AWS_ACCESS_KEY_ID=mock AWS_SECRET_ACCESS_KEY=mock \
  aws dynamodb create-table \
    --table-name myTable \
    --attribute-definitions AttributeName=id,AttributeType=S \
    --key-schema AttributeName=id,KeyType=HASH \
    --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5  \
    --endpoint-url http://0.0.0.0:8000
{
    "TableDescription": { /* BLAH BLAH BLAH */ }
}

# Check it exists!
$ AWS_ACCESS_KEY_ID=mock AWS_SECRET_ACCESS_KEY=mock \
  aws dynamodb list-tables --endpoint-url http://0.0.0.0:8000 --output json
{
    "TableNames": [
        "myTable"
    ]
}

# Shut it down
# (Remember, currently we're in an "in memory" DB which means next `compose up` has nothing!)
$ docker-compose down
```

## Roadmap

- [ ] Switch to a persistent volume for dynamodb container to store data across `docker-compose up/down`.
- [ ] Add ingestion of initial data + table creation
- [ ] Add "reset" to set _back_ to that initial state (Can use `docker-compose down -v` or something).


## Maintenance Status

**Archived:** This project is no longer maintained by Formidable. We are no longer responding to issues or pull requests unless they relate to security concerns. We encourage interested developers to fork this project and make it their own!
