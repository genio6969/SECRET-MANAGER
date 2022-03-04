# SECRET-MANAGER
Codificación en python para extraer secretos en aws secret manager 


try:
    import json
    import os
    import boto3
    import sys

    print('all module are loaded')

except Exception as e:
    print("Error : {}".format(e))

global AWS_ACCESS_KEY
global AWS_SECRET_KEY
global AWS_REGION_NAME
global SECRET_NAME
global secrets

SECRET_NAME = "XXXXX"

AWS_ACCESS_KEY = "AKIAUFSZLQTEJTS5FT6I"
AWS_SECRET_KEY = "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"
AWS_REGION_NAME = "us-east-1"


class AWSSecretManager(object):

    def __init__(self):
        self._session = boto3.session.Session()
        self.client = self._session.client(
            service_name='secretsmanager',
            aws_access_key_id=AWS_ACCESS_KEY,
            aws_secret_access_key=AWS_SECRET_KEY,
            region_name=AWS_REGION_NAME
        )
        self.secrets = self.get()

    def get(self):
        try:
            get_secret_value_response = self.client.get_secret_value(
                SecretId=SECRET_NAME
            )

            if 'SecretString' in get_secret_value_response:
                secret = get_secret_value_response['SecretString']
                secret = json.loads(secret)
                return secret
        except Exception as e:
            return 'error'


_instance = AWSSecretManager()
response = _instance.get()
print(response)
