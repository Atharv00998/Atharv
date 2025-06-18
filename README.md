#Image uploader 
import boto3
import os
from dotenv import load_dotenv

load_dotenv()

s3 = boto3.client(
    's3',
    aws_access_key_id=os.getenv('AWS_ACCESS_KEY_ID'),
    aws_secret_access_key=os.getenv('AWS_SECRET_ACCESS_KEY'),
    region_name=os.getenv('AWS_REGION')
)

BUCKET_NAME = os.getenv('S3_BUCKET_NAME')

def upload_file(file_path):
    file_name = os.path.basename(file_path)
    s3.upload_file(file_path, BUCKET_NAME, file_name)
    print(f"Uploaded {file_name} to {BUCKET_NAME}")

def list_files():
    response = s3.list_objects_v2(Bucket=BUCKET_NAME)
    for obj in response.get('Contents', []):
        print(obj['Key'])
