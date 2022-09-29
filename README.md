# verdaccio-heroku-docker

A containerized verdaccio private npm registry configured with github oauth and AWS S3 storage.

## Deployment

You can deploy to heroku with one click

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

## Server configuration

### Auth (GitHub)

Once the app has been deployed, you will need to setup a github oauth app and add the necessary
oauth credentials as heroku config vars.

[Follow these instructions](https://github.com/n4bb12/verdaccio-github-oauth-ui#github-config) when
creating the oauth app.

Add the following config vars to the heroku app. Either do this from the [heroku dashboard](https://devcenter.heroku.com/articles/config-vars#using-the-heroku-dashboard)
or with the [cli](https://devcenter.heroku.com/articles/config-vars#using-the-heroku-cli).

```
GITHUB_OAUTH_ORG
GITHUB_OAUTH_CLIENT_ID
GITHUB_OAUTH_CLIENT_SECRET
```

You should now be able to visit the heroku app and login with your github account by clicking the
login button and going through the oauth flow.

### Storage (AWS S3)

AWS configuration steps:

1. create private S3 bucket
2. create policy through IAM dashboard to allow access to the bucket. Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowObjectManagement",
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": ["arn:aws:s3:::BUCKET_NAME", "arn:aws:s3:::BUCKET_NAME/*"]
    }
  ]
}
```

3. (optional) create user and write down the credentials
4. attach policy to the desired user. Alternativelly, you can attach the policy and user to a group

Once you have the user credentials, add the following config vars to the heroku app. Either do this from the [heroku dashboard](https://devcenter.heroku.com/articles/config-vars#using-the-heroku-dashboard)
or with the [cli](https://devcenter.heroku.com/articles/config-vars#using-the-heroku-cli).

```
S3_BUCKET
S3_REGION
S3_ACCESS_KEY_ID
S3_SECRET_ACCESS_KEY
```
