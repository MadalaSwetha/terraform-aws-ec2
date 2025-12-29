# Terraform AWS Infrastructure

This Terraform project provisions a basic AWS infrastructure with VPC, subnet, internet gateway, security group, and an EC2 instance.

## Prerequisites

- **Terraform**: v1.14.3 or later
- **AWS Account**: With appropriate IAM permissions
- **AWS CLI**: Configured with credentials
- **Access Keys**: AWS Access Key ID and Secret Access Key

## Project Structure

```
terraform-aws-project/
├── main.tf              # Main Terraform configuration
├── README.md            # This file
└── .gitignore          # Git ignore file (optional)
```

## Resources Created

This Terraform configuration creates:

1. **VPC** - Virtual Private Cloud with CIDR block `10.0.0.0/16`
2. **Subnet** - Public subnet with CIDR block `10.0.0.0/24` in `us-west-2a`
3. **Internet Gateway** - Enables internet connectivity for the VPC
4. **Route Table** - Routes traffic to the internet gateway
5. **Security Group** - Allows inbound traffic on ports 22 (SSH) and 80 (HTTP)
6. **EC2 Instance** - Amazon Linux 2 instance (t3.micro - Free Tier eligible)

## Configuration Details

### AWS Region
- **Region**: `us-west-2`
- **Availability Zone**: `us-west-2a`

### EC2 Instance
- **AMI**: Latest Amazon Linux 2 (x86_64)
- **Instance Type**: `t3.micro` (Free Tier eligible)
- **Public IP**: Automatically assigned

### Security Rules
- **Inbound SSH (22)**: Open to `0.0.0.0/0`
- **Inbound HTTP (80)**: Open to `0.0.0.0/0`
- **Outbound**: All traffic allowed

## Setup Instructions

### 1. Set AWS Credentials

**Option A: Using CSV file**
```powershell
$csv = Import-Csv "C:\Users\Swetha\Downloads\Swetha_Admin_accessKeys.csv"
$env:AWS_ACCESS_KEY_ID = $csv[0].'Access key ID'
$env:AWS_SECRET_ACCESS_KEY = $csv[0].'Secret access key'
```

**Option B: Using AWS CLI**
```powershell
aws configure
```

### 2. Initialize Terraform
```powershell
cd "C:\Users\Swetha\Terraform Project\terraform-aws-project"
terraform init
```

### 3. Validate Configuration
```powershell
terraform validate
```

### 4. Review Plan
```powershell
terraform plan
```

### 5. Apply Configuration
```powershell
terraform apply -auto-approve
```

## Outputs

After successful deployment, you'll see:
- VPC ID
- Subnet ID
- Security Group ID
- EC2 Instance ID
- EC2 Public IP Address

## Accessing the EC2 Instance

### Via SSH
```bash
ssh -i /path/to/key.pem ec2-user@<public-ip>
```

### Via AWS Console
1. Go to EC2 Dashboard
2. Find instance named "Terraform-EC2"
3. Copy the public IP from instance details

## Destroying Infrastructure

To remove all resources:
```powershell
terraform destroy -auto-approve
```

**Warning**: This will delete all created resources. Ensure you have backups if needed.

## Troubleshooting

### Error: No valid credential sources found
- Ensure AWS credentials are set in environment variables or AWS CLI
- Check Access Key ID and Secret Access Key are correct

### Error: InvalidAMIID.NotFound
- The AMI may not exist in your region
- Data source automatically fetches the latest Amazon Linux 2 AMI

### Error: The specified instance type is not eligible for Free Tier
- Ensure your AWS account is within Free Tier period
- Change `instance_type` to another Free Tier eligible type

### Error: Unauthorized Operation
- Verify IAM user has EC2 and VPC permissions
- Attach `AmazonEC2FullAccess` policy to your IAM user

## Security Considerations

⚠️ **Warning**: The current configuration allows SSH access from anywhere (`0.0.0.0/0`). For production:
- Restrict SSH access to specific IP ranges
- Use security groups more restrictively
- Consider using AWS Systems Manager Session Manager instead of SSH

## Cost Estimation

All resources created fall within AWS Free Tier:
- VPC: Free
- Subnet: Free
- Internet Gateway: Free
- Route Table: Free
- Security Group: Free
- EC2 t3.micro: 750 hours/month free (within 12-month Free Tier)

**Total Monthly Cost**: $0 (if within Free Tier limits)

## Future Enhancements

- [ ] Add variables.tf for parameterized configuration
- [ ] Create separate environments (dev, staging, prod)
- [ ] Implement modular structure with modules/
- [ ] Add outputs.tf for resource information
- [ ] Add monitoring and logging
- [ ] Implement auto-scaling groups
- [ ] Add RDS database

## Support

For issues or questions:
1. Check AWS CloudFormation events in AWS Console
2. Review Terraform logs: `terraform show`
3. Consult AWS documentation: https://docs.aws.amazon.com/

## License

This project is provided as-is for educational purposes.
