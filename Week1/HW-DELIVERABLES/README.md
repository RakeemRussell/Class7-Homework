# Step 1: Security Group Creation
*Create a security group with only an HTTP rule*

1. **Navigate to Security Groups:**
   - Left pane → Network and Security → Security Groups
   
2. **Create Security Group:**
   - Click "Create Security Group"
   - Enter SG Name and Description
   - Verify VPC is set to default
   - Add inbound HTTP rule with "Anywhere IPv4" source (`0.0.0.0/0`)
   - **Don't modify outbound rules** - verify "All traffic" is allowed
   - *(Optional: Add tags)*
   - Click "Create Security Group"
   
3. **Verification:**
   - Verify SG is created and correctly configured

### Step 2: Obtain Startup Script
- Choose from the available scripts (mine, yours, or Theo's)
- Copy the script from GitHub

### Step 3: Launch EC2 Instance

1. **Navigate to Instances:**
   - Left pane → Instances → Instances
   - Click "Launch Instances"

2. **Configure Instance:**
   - **Name and Tags:** Enter instance name, add relevant tags
   - **AMI Selection:** Review AMI menu, ensure defaults are selected, collapse
   - **Instance Type:** Review instance type menu, ensure proper sizing, collapse
   - **Key Pair:** Select "Proceed without key pair", collapse
   
3. **Network Settings:**
   - **Don't click "Edit"**
   - Verify VPC selection
   - *Note: Subnet selection is not critical for this lab*
   - Ensure "Auto-assign public IP" is enabled
   - **Select your created Security Group** *(NOT "launch-wizard"!)*
   - Collapse section
   
4. **Storage Configuration:**
   - Review Configure Storage menu
   - *Brief discussion: What is EBS?*
   - Collapse section
   
5. **Advanced Settings:**
   - Open Advanced Settings
   - **Focus on User Data section only** - ignore everything else
   - Paste your chosen startup script
   
6. **Launch:**
   - Review configuration
   - Click "Launch Instance"

### Step 4: Test Your Web Server

1. Wait for the instance to pass status checks
2. Copy the instance's **public DNS address**
3. Open your web browser
4. Navigate to: `http://<public-DNS-address>`
   - **Important:** Use `http://` prefix, not `https://`

---

## Troubleshooting

*Listed in order from most likely to least likely causes:*

### 1. **URL Issues** *(Most Common)*
- **Missing Protocol:** You forgot to put `http://` in front of the public DNS address
  - *Without the prefix, modern browsers redirect to HTTPS (port 443 instead of 80)*
- **Wrong DNS Address:** You're using the private DNS address instead of public
  - *Private DNS only works inside the VPC*
- **Correct format:** `http://<public-DNS>`

### 2. **Security Group Configuration**
- Does your EC2 instance have a Security Group assigned?
- Does the assigned SG have an **inbound (ingress) rule** for HTTP (port 80 TCP) with IPv4 source from anywhere (`0.0.0.0/0`)?
- Does the **outbound rule** exist and permit all ports and protocols?

### 3. **User Data Script Issues**
- You incorrectly copied and pasted the user data/startup script
- You may have forgotten to copy/paste it entirely

### 4. **Less Likely Issues:**
- **Wrong VPC:** You're using an incorrect VPC
- **Broken Default VPC:** You deleted the Internet Gateway (IGW) or edited the Route Tables (RTBs)
- **No Public IP:** You disabled the auto-assign public IP feature
- **Security Group Editing:** You edited the SG without first removing it from the EC2 instance

---

## Teardown


1. **Terminate the EC2 Instance**
   - Navigate to EC2 → Instances
   - Select your instance
   - Instance State → Terminate Instance

2. **Delete Security Group** *(Optional)*
   - Navigate to EC2 → Security Groups
   - Select your created security group
   - Actions → Delete Security Group
   - *Note: Can only delete after instance termination*
