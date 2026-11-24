# CLI Cheatsheet — চার cloud, একই কাজ

> একই কাজ `aws`, `gcloud`, `az` ও `oci` — এই চার CLI-তে কীভাবে করবেন, পাশাপাশি।
> সব উদাহরণ **`shopbd`** (একটি ছোট বাংলাদেশি অনলাইন শপ) দিয়ে, region: `ap-south-1` / `asia-south1` / `centralindia` / `ap-mumbai-1`।

!!! abstract "কীভাবে ব্যবহার করবেন"
    - ট্যাব বেছে নিন (`AWS` / `GCP` / `Azure` / `OCI`) — পুরো সাইটে একই ট্যাব খোলা থাকবে।
    - ID/নাম সব **placeholder**: `123456789012`, `my-project-id`, `00000000-0000-0000-0000-000000000000`, `ocid1.compartment.oc1..example`। নিজের মান বসান।
    - এখানে শুধু সবচেয়ে প্রচলিত, documented form আছে। সব flag-এর জন্য `--help` বা official docs দেখুন; CLI-র version ভেদে flag বদলাতে পারে।
    - খরচ হতে পারে এমন command (VM, database, cluster) চালানোর আগে [Cost Management](../18-cost-management.md) পড়ুন, আর কাজ শেষে নিচের "পরিষ্কার করা" অংশ দিয়ে মুছে ফেলুন।

OCI-র কমান্ডে প্রায় সব জায়গায় compartment ও অন্যান্য OCID লাগে। তাই শুরুতেই shell variable বানিয়ে রাখুন:

```bash
export COMPARTMENT_ID=ocid1.compartment.oc1..example
export AD="Uocm:AP-MUMBAI-1-AD-1"   # oci iam availability-domain list দিয়ে নিজের AD-র নাম দেখুন
```

## ১. Install, Login ও শুরুর সেটিং

### CLI install (Linux)

=== "AWS"

    ```bash
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o awscliv2.zip
    unzip awscliv2.zip && sudo ./aws/install
    aws --version
    ```

=== "GCP"

    ```bash
    curl https://sdk.cloud.google.com | bash
    exec -l $SHELL
    gcloud --version
    ```

=== "Azure"

    ```bash
    curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
    az --version
    ```

=== "OCI"

    ```bash
    bash -c "$(curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh)"
    oci --version
    ```

### Login / authentication

=== "AWS"

    ```bash
    aws configure sso          # SSO (IAM Identity Center) — সুপারিশকৃত
    aws sso login --profile shopbd-dev
    aws configure              # বিকল্প: access key (শুধু লোকাল ও শেখার জন্য)
    ```

=== "GCP"

    ```bash
    gcloud init                # প্রথমবার: login + project বাছাই
    gcloud auth login          # শুধু login
    gcloud auth application-default login   # SDK/Terraform-এর জন্য ADC
    ```

=== "Azure"

    ```bash
    az login
    az login --use-device-code   # browser নেই এমন server-এ
    ```

=== "OCI"

    ```bash
    oci setup config             # ~/.oci/config ও API key বানায়
    oci session authenticate     # browser দিয়ে অস্থায়ী session token
    ```

### আমি কে? (current identity)

=== "AWS"

    ```bash
    aws sts get-caller-identity
    ```

=== "GCP"

    ```bash
    gcloud auth list
    gcloud config list
    ```

=== "Azure"

    ```bash
    az account show
    ```

=== "OCI"

    ```bash
    oci iam user get --user-id ocid1.user.oc1..example   # নিজের user OCID দিন
    oci os ns get                                        # auth ঠিক আছে কিনা পরীক্ষা
    ```

!!! note "OCI-তে সরাসরি `whoami` নেই"
    `~/.oci/config`-এর `user=` লাইনেই আপনার user OCID থাকে। `oci os ns get` ছোট একটা call — সফল হলে credential ঠিক আছে।

### Default region / project / subscription সেট করা

=== "AWS"

    ```bash
    aws configure set region ap-south-1
    export AWS_PROFILE=shopbd-dev     # profile বদলাতে
    ```

=== "GCP"

    ```bash
    gcloud config set project my-project-id
    gcloud config set compute/region asia-south1
    gcloud config set compute/zone asia-south1-a
    ```

=== "Azure"

    ```bash
    az account set --subscription 00000000-0000-0000-0000-000000000000
    az configure --defaults location=centralindia group=shopbd-rg
    ```

=== "OCI"

    ```bash
    export OCI_CLI_REGION=ap-mumbai-1     # বা ~/.oci/config-এ region=ap-mumbai-1
    oci iam region-subscription list
    ```

### Region তালিকা

=== "AWS"

    ```bash
    aws ec2 describe-regions --output table
    ```

=== "GCP"

    ```bash
    gcloud compute regions list
    ```

=== "Azure"

    ```bash
    az account list-locations --output table
    ```

=== "OCI"

    ```bash
    oci iam region list --output table
    ```

### Output ছাঁটা ও filter করা

=== "AWS"

    ```bash
    aws ec2 describe-instances \
      --query 'Reservations[].Instances[].[InstanceId,State.Name]' --output table
    ```

=== "GCP"

    ```bash
    gcloud compute instances list --format="table(name,zone,status)" --filter="status=RUNNING"
    ```

=== "Azure"

    ```bash
    az vm list --query "[].{name:name, state:powerState}" --output table
    ```

=== "OCI"

    ```bash
    oci compute instance list --compartment-id $COMPARTMENT_ID \
      --query 'data[].{name:"display-name", state:"lifecycle-state"}' --output table
    ```

### Help

=== "AWS"

    ```bash
    aws ec2 run-instances help
    ```

=== "GCP"

    ```bash
    gcloud compute instances create --help
    ```

=== "Azure"

    ```bash
    az vm create --help
    ```

=== "OCI"

    ```bash
    oci compute instance launch --help
    ```

## ২. Account, Project ও Tags

### Resource-এর "ঘর" বানানো (project / resource group / compartment)

=== "AWS"

    ```bash
    # নতুন account বানাতে Organizations লাগে (management account থেকে)
    aws organizations create-account --email ops@example.com --account-name shopbd-prod
    ```

=== "GCP"

    ```bash
    gcloud projects create my-project-id --name="shopbd-prod"
    gcloud billing projects link my-project-id --billing-account=000000-000000-000000
    ```

=== "Azure"

    ```bash
    az group create --name shopbd-rg --location centralindia
    ```

=== "OCI"

    ```bash
    oci iam compartment create --compartment-id $COMPARTMENT_ID \
      --name shopbd-prod --description "shopbd production"
    ```

### Tag / label লাগানো

=== "AWS"

    ```bash
    aws ec2 create-tags --resources i-0123456789abcdef0 --tags Key=env,Value=prod
    ```

=== "GCP"

    ```bash
    gcloud compute instances update shopbd-api --zone=asia-south1-a --update-labels=env=prod
    ```

=== "Azure"

    ```bash
    az tag update --resource-id <resource-id> --operation merge --tags env=prod
    ```

=== "OCI"

    ```bash
    oci compute instance update --instance-id ocid1.instance.oc1..example \
      --freeform-tags '{"env":"prod"}'
    ```

### GCP API চালু / Azure resource provider register

=== "AWS"

    ```bash
    # সাধারণত আলাদা করে service চালু করতে হয় না।
    # শুধু opt-in region চালু করতে:
    aws account enable-region --region-name ap-east-1
    ```

=== "GCP"

    ```bash
    gcloud services enable compute.googleapis.com
    ```

=== "Azure"

    ```bash
    az provider register --namespace Microsoft.Compute
    ```

=== "OCI"

    ```bash
    # আলাদা করে service চালু করতে হয় না — policy থাকলেই চলে।
    oci iam region-subscription list
    ```

## ৩. Virtual Machine

### Machine size / shape তালিকা

=== "AWS"

    ```bash
    aws ec2 describe-instance-types --instance-types t3.micro t3.small
    ```

=== "GCP"

    ```bash
    gcloud compute machine-types list --zones=asia-south1-a
    ```

=== "Azure"

    ```bash
    az vm list-sizes --location centralindia --output table
    ```

=== "OCI"

    ```bash
    oci compute shape list --compartment-id $COMPARTMENT_ID --output table
    ```

### VM বানানো

=== "AWS"

    ```bash
    aws ec2 run-instances \
      --image-id ami-0abcdef1234567890 --instance-type t3.micro \
      --key-name shopbd-key --subnet-id subnet-0abc1234def567890 \
      --security-group-ids sg-0abc1234def567890 \
      --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=shopbd-api}]'
    ```

=== "GCP"

    ```bash
    gcloud compute instances create shopbd-api \
      --zone=asia-south1-a --machine-type=e2-medium \
      --image-family=debian-12 --image-project=debian-cloud
    ```

=== "Azure"

    ```bash
    az vm create --resource-group shopbd-rg --name shopbd-api \
      --image Ubuntu2204 --size Standard_B2s \
      --admin-username azureuser --generate-ssh-keys
    ```

=== "OCI"

    ```bash
    oci compute instance launch --compartment-id $COMPARTMENT_ID \
      --availability-domain "$AD" --shape VM.Standard.E5.Flex \
      --shape-config '{"ocpus":1,"memoryInGBs":8}' \
      --image-id ocid1.image.oc1..example --subnet-id ocid1.subnet.oc1..example \
      --display-name shopbd-api --ssh-authorized-keys-file ~/.ssh/id_ed25519.pub
    ```

### VM-এর তালিকা

=== "AWS"

    ```bash
    aws ec2 describe-instances --region ap-south-1
    ```

=== "GCP"

    ```bash
    gcloud compute instances list
    ```

=== "Azure"

    ```bash
    az vm list --resource-group shopbd-rg --output table --show-details
    ```

=== "OCI"

    ```bash
    oci compute instance list --compartment-id $COMPARTMENT_ID
    ```

### VM-এ SSH

=== "AWS"

    ```bash
    ssh -i shopbd-key.pem ec2-user@203.0.113.10   # Amazon Linux; Ubuntu AMI-তে ubuntu@
    aws ssm start-session --target i-0123456789abcdef0   # port 22 ছাড়া (Session Manager)
    ```

=== "GCP"

    ```bash
    gcloud compute ssh shopbd-api --zone=asia-south1-a
    ```

=== "Azure"

    ```bash
    ssh azureuser@203.0.113.10
    az ssh vm --resource-group shopbd-rg --name shopbd-api   # Entra login, ssh extension লাগে
    ```

=== "OCI"

    ```bash
    ssh opc@203.0.113.10        # Oracle Linux; Ubuntu image-এ ubuntu@
    ```

### VM বন্ধ (stop)

=== "AWS"

    ```bash
    aws ec2 stop-instances --instance-ids i-0123456789abcdef0
    ```

=== "GCP"

    ```bash
    gcloud compute instances stop shopbd-api --zone=asia-south1-a
    ```

=== "Azure"

    ```bash
    az vm deallocate --resource-group shopbd-rg --name shopbd-api
    ```

=== "OCI"

    ```bash
    oci compute instance action --instance-id ocid1.instance.oc1..example --action STOP
    ```

!!! warning "Azure: `stop` নয়, `deallocate`"
    `az vm stop` VM বন্ধ করে কিন্তু compute-এর বিল চলতে থাকে। বিল থামাতে `az vm deallocate` ব্যবহার করুন। অন্য cloud-এও শুধু compute থামে — disk ও IP-র খরচ চলতে থাকে।

### VM আবার চালু (start)

=== "AWS"

    ```bash
    aws ec2 start-instances --instance-ids i-0123456789abcdef0
    ```

=== "GCP"

    ```bash
    gcloud compute instances start shopbd-api --zone=asia-south1-a
    ```

=== "Azure"

    ```bash
    az vm start --resource-group shopbd-rg --name shopbd-api
    ```

=== "OCI"

    ```bash
    oci compute instance action --instance-id ocid1.instance.oc1..example --action START
    ```

### VM মুছে ফেলা

=== "AWS"

    ```bash
    aws ec2 terminate-instances --instance-ids i-0123456789abcdef0
    ```

=== "GCP"

    ```bash
    gcloud compute instances delete shopbd-api --zone=asia-south1-a
    ```

=== "Azure"

    ```bash
    az vm delete --resource-group shopbd-rg --name shopbd-api --yes
    ```

=== "OCI"

    ```bash
    oci compute instance terminate --instance-id ocid1.instance.oc1..example
    ```

## ৪. Networking

### Private network (VPC / VNet / VCN) বানানো

=== "AWS"

    ```bash
    aws ec2 create-vpc --cidr-block 10.0.0.0/16 \
      --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=shopbd-prod-vpc}]'
    ```

=== "GCP"

    ```bash
    gcloud compute networks create shopbd-prod-vpc --subnet-mode=custom
    ```

=== "Azure"

    ```bash
    az network vnet create --resource-group shopbd-rg --name shopbd-prod-vnet \
      --address-prefixes 10.0.0.0/16
    ```

=== "OCI"

    ```bash
    oci network vcn create --compartment-id $COMPARTMENT_ID \
      --cidr-block 10.0.0.0/16 --display-name shopbd-prod-vcn
    ```

### Subnet বানানো

=== "AWS"

    ```bash
    aws ec2 create-subnet --vpc-id vpc-0abc1234def567890 \
      --cidr-block 10.0.1.0/24 --availability-zone ap-south-1a
    ```

=== "GCP"

    ```bash
    gcloud compute networks subnets create shopbd-app-subnet \
      --network=shopbd-prod-vpc --region=asia-south1 --range=10.0.1.0/24
    ```

=== "Azure"

    ```bash
    az network vnet subnet create --resource-group shopbd-rg \
      --vnet-name shopbd-prod-vnet --name app --address-prefixes 10.0.1.0/24
    ```

=== "OCI"

    ```bash
    oci network subnet create --compartment-id $COMPARTMENT_ID \
      --vcn-id ocid1.vcn.oc1..example --cidr-block 10.0.1.0/24 \
      --display-name shopbd-app-subnet
    ```

### Firewall group (Security Group / NSG) বানানো

=== "AWS"

    ```bash
    aws ec2 create-security-group --group-name shopbd-web-sg \
      --description "shopbd web" --vpc-id vpc-0abc1234def567890
    ```

=== "GCP"

    ```bash
    # আলাদা group লাগে না — firewall rule সরাসরি VPC-তে (পরের ধাপ)
    gcloud compute firewall-rules list --filter="network:shopbd-prod-vpc"
    ```

=== "Azure"

    ```bash
    az network nsg create --resource-group shopbd-rg --name shopbd-web-nsg
    ```

=== "OCI"

    ```bash
    oci network nsg create --compartment-id $COMPARTMENT_ID \
      --vcn-id ocid1.vcn.oc1..example --display-name shopbd-web-nsg
    ```

### Firewall rule — HTTPS (443) খোলা

=== "AWS"

    ```bash
    aws ec2 authorize-security-group-ingress --group-id sg-0abc1234def567890 \
      --protocol tcp --port 443 --cidr 0.0.0.0/0
    ```

=== "GCP"

    ```bash
    gcloud compute firewall-rules create shopbd-allow-https \
      --network=shopbd-prod-vpc --direction=INGRESS \
      --allow=tcp:443 --source-ranges=0.0.0.0/0
    ```

=== "Azure"

    ```bash
    az network nsg rule create --resource-group shopbd-rg --nsg-name shopbd-web-nsg \
      --name allow-https --priority 100 --direction Inbound --access Allow \
      --protocol Tcp --destination-port-ranges 443
    ```

=== "OCI"

    ```bash
    oci network nsg rules add --nsg-id ocid1.networksecuritygroup.oc1..example \
      --security-rules '[{"direction":"INGRESS","protocol":"6","source":"0.0.0.0/0","sourceType":"CIDR_BLOCK","tcpOptions":{"destinationPortRange":{"min":443,"max":443}}}]'
    ```

!!! tip "বাস্তবে `0.0.0.0/0` শুধু public web port-এ"
    SSH (22), database (5432) ইত্যাদি কখনো `0.0.0.0/0` থেকে খুলবেন না — নিজের IP বা অন্য security group/NSG থেকে খুলুন। বিস্তারিত: [Networking](../05-networking.md)।

### DNS zone বানানো

=== "AWS"

    ```bash
    aws route53 create-hosted-zone --name shopbd.example.com --caller-reference shopbd-$(date +%s)
    ```

=== "GCP"

    ```bash
    gcloud dns managed-zones create shopbd-zone \
      --dns-name=shopbd.example.com. --description="shopbd public zone"
    ```

=== "Azure"

    ```bash
    az network dns zone create --resource-group shopbd-rg --name shopbd.example.com
    ```

=== "OCI"

    ```bash
    oci dns zone create --compartment-id $COMPARTMENT_ID \
      --name shopbd.example.com --zone-type PRIMARY
    ```

## ৫. Storage

### Disk (block volume) বানানো

=== "AWS"

    ```bash
    aws ec2 create-volume --availability-zone ap-south-1a --size 50 --volume-type gp3
    ```

=== "GCP"

    ```bash
    gcloud compute disks create shopbd-data --size=50GB --type=pd-balanced --zone=asia-south1-a
    ```

=== "Azure"

    ```bash
    az disk create --resource-group shopbd-rg --name shopbd-data --size-gb 50 --sku Premium_LRS
    ```

=== "OCI"

    ```bash
    oci bv volume create --compartment-id $COMPARTMENT_ID \
      --availability-domain "$AD" --size-in-gbs 50 --display-name shopbd-data
    ```

### Disk snapshot / backup

=== "AWS"

    ```bash
    aws ec2 create-snapshot --volume-id vol-0abc1234def567890 --description "shopbd-data snap"
    ```

=== "GCP"

    ```bash
    gcloud compute disks snapshot shopbd-data --snapshot-names=shopbd-data-snap1 --zone=asia-south1-a
    ```

=== "Azure"

    ```bash
    az snapshot create --resource-group shopbd-rg --name shopbd-data-snap1 --source shopbd-data
    ```

=== "OCI"

    ```bash
    oci bv backup create --volume-id ocid1.volume.oc1..example --display-name shopbd-data-snap1
    ```

### Bucket বানানো

=== "AWS"

    ```bash
    aws s3 mb s3://shopbd-images --region ap-south-1
    ```

=== "GCP"

    ```bash
    gcloud storage buckets create gs://shopbd-images --location=asia-south1
    ```

=== "Azure"

    ```bash
    az storage account create --name shopbdimages --resource-group shopbd-rg \
      --location centralindia --sku Standard_LRS
    az storage container create --account-name shopbdimages --name images --auth-mode login
    ```

=== "OCI"

    ```bash
    oci os bucket create --compartment-id $COMPARTMENT_ID --name shopbd-images
    ```

!!! note "নামের নিয়ম"
    AWS S3 ও GCS bucket-এর নাম সারা বিশ্বে unique। Azure storage account-এর নামও global unique, শুধু ছোট হাতের অক্ষর ও সংখ্যা (৩–২৪ অক্ষর)। OCI bucket-এর নাম tenancy-র namespace-এর ভেতরে unique।

### File upload

=== "AWS"

    ```bash
    aws s3 cp logo.png s3://shopbd-images/logo.png
    ```

=== "GCP"

    ```bash
    gcloud storage cp logo.png gs://shopbd-images/logo.png
    ```

=== "Azure"

    ```bash
    az storage blob upload --account-name shopbdimages --container-name images \
      --name logo.png --file logo.png --auth-mode login
    ```

=== "OCI"

    ```bash
    oci os object put --bucket-name shopbd-images --file logo.png --name logo.png
    ```

### Folder sync / bulk upload

=== "AWS"

    ```bash
    aws s3 sync ./images s3://shopbd-images/images
    ```

=== "GCP"

    ```bash
    gcloud storage rsync ./images gs://shopbd-images/images
    ```

=== "Azure"

    ```bash
    az storage blob upload-batch --account-name shopbdimages \
      --destination images --source ./images --auth-mode login
    ```

=== "OCI"

    ```bash
    oci os object bulk-upload --bucket-name shopbd-images --src-dir ./images --object-prefix images/
    ```

### Object তালিকা

=== "AWS"

    ```bash
    aws s3 ls s3://shopbd-images/ --recursive
    ```

=== "GCP"

    ```bash
    gcloud storage ls gs://shopbd-images/**
    ```

=== "Azure"

    ```bash
    az storage blob list --account-name shopbdimages --container-name images \
      --auth-mode login --output table
    ```

=== "OCI"

    ```bash
    oci os object list --bucket-name shopbd-images
    ```

### Versioning চালু

=== "AWS"

    ```bash
    aws s3api put-bucket-versioning --bucket shopbd-images \
      --versioning-configuration Status=Enabled
    ```

=== "GCP"

    ```bash
    gcloud storage buckets update gs://shopbd-images --versioning
    ```

=== "Azure"

    ```bash
    az storage account blob-service-properties update --account-name shopbdimages \
      --resource-group shopbd-rg --enable-versioning true
    ```

=== "OCI"

    ```bash
    oci os bucket update --bucket-name shopbd-images --versioning Enabled
    ```

### সময়-সীমিত ডাউনলোড লিংক (signed URL)

=== "AWS"

    ```bash
    aws s3 presign s3://shopbd-images/logo.png --expires-in 3600
    ```

=== "GCP"

    ```bash
    gcloud storage sign-url gs://shopbd-images/logo.png --duration=1h \
      --impersonate-service-account=shopbd-signer@my-project-id.iam.gserviceaccount.com
    ```

=== "Azure"

    ```bash
    az storage blob generate-sas --account-name shopbdimages --container-name images \
      --name logo.png --permissions r --expiry 2026-12-31T00:00Z \
      --https-only --full-uri --as-user --auth-mode login
    ```

=== "OCI"

    ```bash
    oci os preauth-request create --bucket-name shopbd-images --name logo-link \
      --access-type ObjectRead --object-name logo.png --time-expires 2026-12-31T00:00:00Z
    ```

### Bucket মুছে ফেলা (ভেতরের সব সহ)

=== "AWS"

    ```bash
    aws s3 rb s3://shopbd-images --force
    ```

=== "GCP"

    ```bash
    gcloud storage rm --recursive gs://shopbd-images
    ```

=== "Azure"

    ```bash
    az storage container delete --account-name shopbdimages --name images --auth-mode login
    ```

=== "OCI"

    ```bash
    oci os bucket delete --bucket-name shopbd-images --empty
    ```

!!! danger "মোছার আগে দুইবার ভাবুন"
    এই command-গুলো data স্থায়ীভাবে মুছে ফেলতে পারে (versioning চালু থাকলে AWS-এ `--force` সব version মোছে না — আলাদা কাজ)। Production bucket-এ কখনো না ভেবে চালাবেন না।

## ৬. Database

### Managed PostgreSQL বানানো

=== "AWS"

    ```bash
    aws rds create-db-instance --db-instance-identifier shopbd-db \
      --engine postgres --db-instance-class db.t3.micro --allocated-storage 20 \
      --master-username shopbd_admin --manage-master-user-password
    ```

=== "GCP"

    ```bash
    gcloud sql instances create shopbd-db --database-version=POSTGRES_16 \
      --tier=db-custom-1-3840 --region=asia-south1
    ```

=== "Azure"

    ```bash
    az postgres flexible-server create --resource-group shopbd-rg --name shopbd-db \
      --location centralindia --tier Burstable --sku-name Standard_B1ms \
      --admin-user shopbdadmin --admin-password '<strong-password>'
    ```

=== "OCI"

    ```bash
    # OCI Database with PostgreSQL: create-এ network/storage/credentials JSON লাগে —
    # Console বা Terraform (oci_psql_db_system) দিয়ে বানানো সহজ। দেখা:
    oci psql db-system list --compartment-id $COMPARTMENT_ID
    ```

!!! tip "Password command-এ লিখবেন না"
    AWS-এর `--manage-master-user-password` password Secrets Manager-এ রাখে। অন্য cloud-এও password shell history-তে রাখার বদলে secret store বা prompt ব্যবহার করুন। বিস্তারিত: [Database](../07-databases.md)।

### Database list

=== "AWS"

    ```bash
    aws rds describe-db-instances --query 'DBInstances[].[DBInstanceIdentifier,DBInstanceStatus]' --output table
    ```

=== "GCP"

    ```bash
    gcloud sql instances list
    ```

=== "Azure"

    ```bash
    az postgres flexible-server list --resource-group shopbd-rg --output table
    ```

=== "OCI"

    ```bash
    oci psql db-system list --compartment-id $COMPARTMENT_ID
    ```

### Database snapshot / backup

=== "AWS"

    ```bash
    aws rds create-db-snapshot --db-instance-identifier shopbd-db \
      --db-snapshot-identifier shopbd-db-snap1
    ```

=== "GCP"

    ```bash
    gcloud sql backups create --instance=shopbd-db
    ```

=== "Azure"

    ```bash
    az postgres flexible-server backup create --resource-group shopbd-rg \
      --name shopbd-db --backup-name shopbd-db-snap1
    ```

=== "OCI"

    ```bash
    oci psql backup list --compartment-id $COMPARTMENT_ID
    ```

## ৭. Container ও Kubernetes

### Container registry (repository) বানানো

=== "AWS"

    ```bash
    aws ecr create-repository --repository-name shopbd-api --region ap-south-1
    ```

=== "GCP"

    ```bash
    gcloud artifacts repositories create shopbd --repository-format=docker --location=asia-south1
    ```

=== "Azure"

    ```bash
    az acr create --resource-group shopbd-rg --name shopbdacr --sku Basic
    ```

=== "OCI"

    ```bash
    oci artifacts container repository create --compartment-id $COMPARTMENT_ID \
      --display-name shopbd-api
    ```

### Registry-তে Docker login

=== "AWS"

    ```bash
    aws ecr get-login-password --region ap-south-1 | docker login --username AWS \
      --password-stdin 123456789012.dkr.ecr.ap-south-1.amazonaws.com
    ```

=== "GCP"

    ```bash
    gcloud auth configure-docker asia-south1-docker.pkg.dev
    ```

=== "Azure"

    ```bash
    az acr login --name shopbdacr
    ```

=== "OCI"

    ```bash
    # username: <tenancy-namespace>/<user>, password: OCI auth token
    docker login ap-mumbai-1.ocir.io -u '<tenancy-namespace>/<username>'
    ```

### Kubernetes cluster বানানো

=== "AWS"

    ```bash
    eksctl create cluster --name shopbd-eks --region ap-south-1   # eksctl আলাদা tool
    ```

=== "GCP"

    ```bash
    gcloud container clusters create-auto shopbd-gke --region=asia-south1
    ```

=== "Azure"

    ```bash
    az aks create --resource-group shopbd-rg --name shopbd-aks \
      --node-count 2 --generate-ssh-keys
    ```

=== "OCI"

    ```bash
    oci ce cluster create --compartment-id $COMPARTMENT_ID --name shopbd-oke \
      --vcn-id ocid1.vcn.oc1..example --kubernetes-version <version>
    ```

### `kubectl`-এর জন্য cluster credentials

=== "AWS"

    ```bash
    aws eks update-kubeconfig --name shopbd-eks --region ap-south-1
    ```

=== "GCP"

    ```bash
    gcloud container clusters get-credentials shopbd-gke --region=asia-south1
    ```

=== "Azure"

    ```bash
    az aks get-credentials --resource-group shopbd-rg --name shopbd-aks
    ```

=== "OCI"

    ```bash
    oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1..example \
      --file $HOME/.kube/config --region ap-mumbai-1 \
      --token-version 2.0.0 --kube-endpoint PUBLIC_ENDPOINT
    ```

## ৮. Serverless

### Function deploy

=== "AWS"

    ```bash
    aws lambda create-function --function-name shopbd-order-email \
      --runtime python3.13 --handler app.handler \
      --role arn:aws:iam::123456789012:role/shopbd-lambda-role \
      --zip-file fileb://function.zip
    ```

=== "GCP"

    ```bash
    gcloud functions deploy shopbd-order-email --gen2 --runtime=python313 \
      --region=asia-south1 --source=. --entry-point=handler --trigger-http
    ```

=== "Azure"

    ```bash
    # Function app আগে বানাতে হয় (az functionapp create), তারপর Core Tools দিয়ে:
    func azure functionapp publish shopbd-order-email
    ```

=== "OCI"

    ```bash
    # Fn Project CLI (OCI Functions-এর জন্য context সেট করা থাকতে হবে)
    fn init --runtime python shopbd-order-email
    cd shopbd-order-email && fn deploy --app shopbd-app
    ```

### Serverless container service চালানো

=== "AWS"

    ```bash
    aws ecs create-service --cluster shopbd-cluster --service-name shopbd-api \
      --task-definition shopbd-api --desired-count 2 --launch-type FARGATE \
      --network-configuration "awsvpcConfiguration={subnets=[subnet-0abc1234def567890],assignPublicIp=ENABLED}"
    ```

=== "GCP"

    ```bash
    gcloud run deploy shopbd-api --region=asia-south1 --allow-unauthenticated \
      --image=asia-south1-docker.pkg.dev/my-project-id/shopbd/shopbd-api:v1
    ```

=== "Azure"

    ```bash
    az containerapp create --name shopbd-api --resource-group shopbd-rg \
      --environment shopbd-env --image shopbdacr.azurecr.io/shopbd-api:v1 \
      --target-port 8080 --ingress external
    ```

=== "OCI"

    ```bash
    oci container-instances container-instance create --compartment-id $COMPARTMENT_ID \
      --availability-domain "$AD" --shape CI.Standard.E4.Flex \
      --shape-config '{"ocpus":1,"memoryInGBs":4}' \
      --containers '[{"imageUrl":"ap-mumbai-1.ocir.io/<tenancy-namespace>/shopbd-api:v1"}]' \
      --vnics '[{"subnetId":"ocid1.subnet.oc1..example"}]'
    ```

## ৯. Messaging

### Queue / topic বানানো

=== "AWS"

    ```bash
    aws sqs create-queue --queue-name shopbd-orders
    ```

=== "GCP"

    ```bash
    gcloud pubsub topics create shopbd-orders
    gcloud pubsub subscriptions create shopbd-orders-sub --topic=shopbd-orders
    ```

=== "Azure"

    ```bash
    az servicebus namespace create --resource-group shopbd-rg --name shopbd-sb --location centralindia
    az servicebus queue create --resource-group shopbd-rg --namespace-name shopbd-sb --name shopbd-orders
    ```

=== "OCI"

    ```bash
    oci queue queue-admin queue create --compartment-id $COMPARTMENT_ID --display-name shopbd-orders
    ```

## ১০. Monitoring ও Logging

### Log tail / দেখা

=== "AWS"

    ```bash
    aws logs tail /aws/lambda/shopbd-order-email --follow
    ```

=== "GCP"

    ```bash
    gcloud logging read 'resource.type="cloud_run_revision"' --limit=20 --freshness=10m
    gcloud beta logging tail 'resource.type="cloud_run_revision"'   # live tail
    ```

=== "Azure"

    ```bash
    az containerapp logs show --name shopbd-api --resource-group shopbd-rg --follow
    ```

=== "OCI"

    ```bash
    # OCI CLI-তে সরাসরি tail নেই; search দিয়ে সাম্প্রতিক log (Console-এর Log Search-ও সহজ)
    oci logging-search search-logs \
      --search-query 'search "ocid1.compartment.oc1..example" | sort by datetime desc' \
      --time-start 2026-11-24T00:00:00Z --time-end 2026-11-24T23:59:59Z
    ```

### CPU alarm বানানো

=== "AWS"

    ```bash
    aws cloudwatch put-metric-alarm --alarm-name shopbd-api-high-cpu \
      --namespace AWS/EC2 --metric-name CPUUtilization \
      --dimensions Name=InstanceId,Value=i-0123456789abcdef0 \
      --statistic Average --period 300 --evaluation-periods 2 \
      --threshold 80 --comparison-operator GreaterThanThreshold \
      --alarm-actions arn:aws:sns:ap-south-1:123456789012:shopbd-alerts
    ```

=== "GCP"

    ```bash
    # alert policy সাধারণত YAML/JSON ফাইলে লেখা হয়
    gcloud alpha monitoring policies create --policy-from-file=high-cpu-policy.yaml
    ```

=== "Azure"

    ```bash
    az monitor metrics alert create --name shopbd-api-high-cpu --resource-group shopbd-rg \
      --scopes <vm-resource-id> --condition "avg Percentage CPU > 80" \
      --action <action-group-id>
    ```

=== "OCI"

    ```bash
    oci monitoring alarm create --compartment-id $COMPARTMENT_ID \
      --display-name shopbd-api-high-cpu --metric-compartment-id $COMPARTMENT_ID \
      --namespace oci_computeagent --query-text 'CpuUtilization[5m].mean() > 80' \
      --severity CRITICAL --destinations '["ocid1.onstopic.oc1..example"]' --is-enabled true
    ```

### Audit — কে কী করল

=== "AWS"

    ```bash
    aws cloudtrail lookup-events --max-results 5
    ```

=== "GCP"

    ```bash
    gcloud logging read 'logName:"cloudaudit.googleapis.com"' --limit=5
    ```

=== "Azure"

    ```bash
    az monitor activity-log list --max-events 5
    ```

=== "OCI"

    ```bash
    oci audit event list --compartment-id $COMPARTMENT_ID \
      --start-time 2026-11-23T00:00:00Z --end-time 2026-11-24T00:00:00Z
    ```

## ১১. Identity ও Security

### Application-এর নিজস্ব identity বানানো

=== "AWS"

    ```bash
    aws iam create-role --role-name shopbd-api-role \
      --assume-role-policy-document file://trust-policy.json
    ```

=== "GCP"

    ```bash
    gcloud iam service-accounts create shopbd-api --display-name="shopbd API"
    ```

=== "Azure"

    ```bash
    az identity create --resource-group shopbd-rg --name shopbd-api-identity
    ```

=== "OCI"

    ```bash
    oci iam dynamic-group create --name shopbd-api-instances \
      --description "shopbd API instances" \
      --matching-rule "ALL {instance.compartment.id = 'ocid1.compartment.oc1..example'}"
    ```

### Permission দেওয়া (শুধু প্রয়োজনীয়টুকু)

=== "AWS"

    ```bash
    aws iam attach-role-policy --role-name shopbd-api-role \
      --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
    ```

=== "GCP"

    ```bash
    gcloud projects add-iam-policy-binding my-project-id \
      --member="serviceAccount:shopbd-api@my-project-id.iam.gserviceaccount.com" \
      --role="roles/storage.objectViewer"
    ```

=== "Azure"

    ```bash
    az role assignment create --assignee <principal-id> --role "Storage Blob Data Reader" \
      --scope /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/shopbd-rg
    ```

=== "OCI"

    ```bash
    oci iam policy create --compartment-id $COMPARTMENT_ID --name shopbd-api-policy \
      --description "shopbd API can read buckets" \
      --statements '["Allow dynamic-group shopbd-api-instances to read object-family in compartment shopbd-prod"]'
    ```

### Secret বানানো

=== "AWS"

    ```bash
    aws secretsmanager create-secret --name shopbd/db-password --secret-string 'change-me'
    ```

=== "GCP"

    ```bash
    printf 'change-me' | gcloud secrets create shopbd-db-password --data-file=-
    ```

=== "Azure"

    ```bash
    az keyvault secret set --vault-name shopbd-kv --name db-password --value 'change-me'
    ```

=== "OCI"

    ```bash
    oci vault secret create-base64 --compartment-id $COMPARTMENT_ID \
      --secret-name shopbd-db-password --vault-id ocid1.vault.oc1..example \
      --key-id ocid1.key.oc1..example \
      --secret-content-content "$(printf 'change-me' | base64)"
    ```

### Secret পড়া

=== "AWS"

    ```bash
    aws secretsmanager get-secret-value --secret-id shopbd/db-password --query SecretString --output text
    ```

=== "GCP"

    ```bash
    gcloud secrets versions access latest --secret=shopbd-db-password
    ```

=== "Azure"

    ```bash
    az keyvault secret show --vault-name shopbd-kv --name db-password --query value -o tsv
    ```

=== "OCI"

    ```bash
    oci secrets secret-bundle get --secret-id ocid1.vaultsecret.oc1..example
    ```

!!! warning "OCI secret-bundle base64 আসে"
    `secret-bundle get` যে content দেয় তা base64-encoded; ব্যবহার করতে decode করতে হবে। আর শেখার উদাহরণের `change-me` কখনো আসল password হিসেবে ব্যবহার করবেন না।

### Encryption key বানানো

=== "AWS"

    ```bash
    aws kms create-key --description "shopbd data key"
    ```

=== "GCP"

    ```bash
    gcloud kms keyrings create shopbd-ring --location=asia-south1
    gcloud kms keys create shopbd-key --keyring=shopbd-ring --location=asia-south1 --purpose=encryption
    ```

=== "Azure"

    ```bash
    az keyvault key create --vault-name shopbd-kv --name shopbd-key
    ```

=== "OCI"

    ```bash
    oci kms management key create --compartment-id $COMPARTMENT_ID \
      --display-name shopbd-key --key-shape '{"algorithm":"AES","length":32}' \
      --endpoint https://example-management.kms.ap-mumbai-1.oraclecloud.com
    ```

## ১২. খরচ

### Budget বানানো

=== "AWS"

    ```bash
    aws budgets create-budget --account-id 123456789012 \
      --budget file://budget.json --notifications-with-subscribers file://notifications.json
    ```

=== "GCP"

    ```bash
    gcloud billing budgets create --billing-account=000000-000000-000000 \
      --display-name="shopbd-monthly" --budget-amount=100USD --threshold-rule=percent=0.8
    ```

=== "Azure"

    ```bash
    az consumption budget create --budget-name shopbd-monthly --amount 100 \
      --category cost --time-grain monthly --start-date 2026-12-01 --end-date 2027-12-01
    ```

=== "OCI"

    ```bash
    oci budgets budget create --compartment-id <tenancy-ocid> --amount 100 \
      --reset-period MONTHLY --target-type COMPARTMENT \
      --targets '["ocid1.compartment.oc1..example"]' --display-name shopbd-monthly
    ```

### এই মাসের খরচ দেখা

=== "AWS"

    ```bash
    aws ce get-cost-and-usage --time-period Start=2026-11-01,End=2026-11-24 \
      --granularity MONTHLY --metrics UnblendedCost --group-by Type=DIMENSION,Key=SERVICE
    ```

=== "GCP"

    ```bash
    # Billing export BigQuery-তে চালু থাকলে:
    bq query --use_legacy_sql=false \
      'SELECT service.description, SUM(cost) AS cost FROM `my-project-id.billing.gcp_billing_export_v1_XXXXXX` GROUP BY 1 ORDER BY 2 DESC'
    ```

=== "Azure"

    ```bash
    az consumption usage list --start-date 2026-11-01 --end-date 2026-11-24 --top 20
    ```

=== "OCI"

    ```bash
    oci usage-api usage-summary request-summarized-usages --tenant-id <tenancy-ocid> \
      --time-usage-started 2026-11-01T00:00:00Z --time-usage-ended 2026-11-24T00:00:00Z \
      --granularity DAILY --query-type COST
    ```

## ১৩. পরিষ্কার করা (cleanup)

### পুরো পরীক্ষামূলক "ঘর" মুছে ফেলা

=== "AWS"

    ```bash
    # একটা-একটা করে resource মুছতে হয় (বা CloudFormation/Terraform stack destroy)
    aws cloudformation delete-stack --stack-name shopbd-test
    ```

=== "GCP"

    ```bash
    gcloud projects delete my-project-id
    ```

=== "Azure"

    ```bash
    az group delete --name shopbd-rg --yes --no-wait
    ```

=== "OCI"

    ```bash
    # compartment আগে খালি করতে হয়, তারপর:
    oci iam compartment delete --compartment-id ocid1.compartment.oc1..example
    ```

!!! danger "Resource group / project মোছা মানে ভেতরের সবকিছু শেষ"
    `az group delete` ও `gcloud projects delete` ভেতরের সব resource মুছে দেয়। কেবল পরীক্ষামূলক ঘরে চালান। Terraform দিয়ে বানালে `terraform destroy` সবচেয়ে নিরাপদ — [Infrastructure as Code](../14-infrastructure-as-code.md)।

## আরও পড়ুন

- ধারণা ও বিস্তারিত: [VM](../04-compute-vms.md) · [Networking](../05-networking.md) · [Storage](../06-storage.md) · [Database](../07-databases.md) · [Kubernetes](../08-containers-kubernetes.md) · [Serverless](../09-serverless.md)
- সব service-এর নাম পাশাপাশি: [Service Map](service-map.md)
- অপরিচিত শব্দ: [Glossary](glossary.md)
- Official CLI docs: [AWS CLI](https://docs.aws.amazon.com/cli/) · [gcloud](https://cloud.google.com/sdk/gcloud/reference) · [Azure CLI](https://learn.microsoft.com/cli/azure/) · [OCI CLI](https://docs.oracle.com/en-us/iaas/tools/oci-cli/latest/)
