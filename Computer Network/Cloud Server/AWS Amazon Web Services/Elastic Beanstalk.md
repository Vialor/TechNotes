an **application** can have many **elastic beanstalk environments**, an **environment** can have multiple instances
an **environment** is associated with one and only one domain name, database, **Elastic Load Balancer (ELB)** and **Auto Scaling Group**
# CLI
```bash
eb status
eb list

eb create

eb deploy
```
# Local Files
## Computer
identification has been saved in `C:\Users\Admin\.ssh\aws-eb`
public key has been saved in `C:\Users\Admin\.ssh\aws-eb.pub`
`~/.aws`: config and credentials
## Project
.ebextensions
01_xxx.config

.elasticbeanstalk/config.yml
created by `eb init`
`eb status` and  `eb deploy` are based on .elasticbeanstalk/config.yml
```text
branch-defaults:
  develop:
    environment: dev-env
  main:
    environment: prod-env
global:
  application_name: postgres
  branch: null
  default_ec2_keyname: aws-eb
  default_platform: Python 3.8 running on 64bit Amazon Linux 2
  default_region: us-east-1
  include_git_submodules: true
  instance_profile: null
  platform_name: null
  platform_version: null
  profile: eb-cli
  repository: null
  sc: git
  workspace_type: Application
```
# Deploy Process
1. **Upload and Extract**: Your application's code is uploaded to the Elastic Beanstalk environment and extracted to a temporary staging directory. On Amazon Linux 2 platforms usually is `/var/app/staging`.
2. **Environment Preparation**: Elastic Beanstalk sets up the environment. This includes configuring the underlying EC2 instances, setting up networking, and ensuring that any dependencies or environment variables are in place.
   Software and Dependencies Installation happen at this stage (`pip install -r requirements.txt`)
3. **Execute config files in  `.ebextensions`
4. **Application Deployment**: 
   Execute **pre deploy Hooks** in `.platform/hooks`
   EB moves your application from the staging directory to the application directory, `/var/app/current`, and restarts the application server
   Execute **post deploy Hooks** in `.platform/hooks`
5. **Clean Up**: EB performs any necessary clean-up tasks, such as removing old versions of the application from the staging area.
# Extend Storage
grows the partition `sudo growpart /dev/nvme0n1 1`
extend file system xfs `sudo xfs_growfs -d /`