see https://docs.aws.amazon.com/systems-manager/latest/userguide/distributor.html

Simple usage of SSM Distributor to deploy a config file to managed instances.

1. Create zip files and update `manifest.json`:
   ```shell
   zip -j windows.zip windows/*
   cat windows.zip | openssl dgst -sha256
   # update manifest.json with checksum

   zip -j linux.zip linux/*
   cat linux.zip | openssl dgst -sha256
   # update manifest.json with checksum
   ```
1. Increment version number at top of `manifest.json`
1. Upload `manifest.json`, `linux.zip`, `windows.zip` files to an S3 Bucket (should all be contained in same prefix)
1. Create the distributor package (or new version of existing package)
   1. "Advanced"
   1. Details - Version name should match version in `manifest.json`
   1. Location - Enter S3 bucket prefix of uploaded files
   1. Manifest - Extract from package
1. Set new version as default version
1. Install using Run Command or State Manager document "AWS-ConfigureAWSPackage"
