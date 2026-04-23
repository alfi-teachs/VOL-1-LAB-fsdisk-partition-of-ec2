# VOL-1-LAB-fsdisk-partition-of-ec2

Objective

Create an EC2 instance, attach a new EBS volume, partition it, format it, and mount it permanently.

# Step 1: Launch EC2 Instance
Go to AWS Console → EC2

Click Launch Instance

Choose AMI (Amazon Linux recommended)

Select instance type (t2.micro for free tier)

Configure security group (allow SSH - port 22)

Launch and download key pair

# Step 2: Create EBS Volume

Go to Elastic Block Store → Volumes

Click Create Volume

Size: 5–10 GB

Type: gp2/gp3

AZ: Same as EC2 instance

Click Create
# Step 3: Connect to EC2 Instance

```bash
ssh -i your-key.pem ec2-user@your-public-ip

```
# Step 4: Verify Attached Disk

Check available disks:
```bash
lsblk
```

or
```bash
sudo fdisk -l
```

You should see something like:
```bash
/dev/xvdbb   10G
```


# Step 5: Partition the Disk using fdisk

Start fdisk:
```bash
sudo fdisk /dev/xvdbb
```

Inside fdisk:
Create new partition:

```bash
n
```

Choose partition type:
```bash
p   (primary)
```

Partition number:
```bash
1

```

Press Enter for default first sector

Press Enter for default last sector

Save changes:
```bash
w
```

# Step 6: Verify Partition

```bash
lsblk
```

Output will show:
```bash
/dev/xvdbb1
```

# Step 7: Format the Partition

Format using ext4:

```bash
sudo mkfs.ext4 /dev/xvdbb1
```


# Step 8: Create Mount Point
```bash
sudo mkdir /data
```


# Step 9: Mount the Partition

# sudo mount /dev/xvdbb1 /data
Verify:

```bash
df -h
```

# Step 10: Permanent Mount (Optional)

Get UUID:

```bash
sudo blkid
```

Edit fstab:

```bash
sudo vi /etc/fstab
```


Add entry:

```bash
UUID=xxxx-xxxx  /data  ext4  defaults,nofail  0  2
```


Save and test:

```bash
sudo mount -a
```

# Result
EBS volume successfully created and attached
Disk partition created using fdisk
File system created and mounted

# Notes
Device name may appear as /dev/nvme1n1 in newer instances (NVMe-based)
Always ensure correct device before partitioning to avoid data loss









