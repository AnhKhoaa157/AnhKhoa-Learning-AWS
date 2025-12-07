---
title: "Database Setup (RDS MySQL)"
date: "2025-12-06T00:00:00Z"
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---

#### Overview

In this section, we'll set up Amazon RDS MySQL database for our Game Store application. RDS provides managed relational database service with automated backups, patching, and high availability.

**What we'll configure:**
- MySQL 8.0 RDS instance (db.t3.micro)
- Security groups for database access
- Database initialization
- Connection testing from local development

#### Architecture

```
┌──────────────┐         ┌──────────────┐
│  Spring Boot │────────►│  RDS MySQL   │
│   Backend    │  3306   │  db.t3.micro │
└──────────────┘         └──────────────┘
                                │
                         ┌──────┴───────┐
                         │ Security     │
                         │ Group        │
                         │ (Port 3306)  │
                         └──────────────┘
```

#### Step 1: Create RDS Subnet Group

Before creating RDS instance, we need a subnet group:

1. **Navigate to RDS Console**
   - Go to: https://console.aws.amazon.com/rds/
   - Region: **ap-southeast-1** (Singapore)

2. **Create Subnet Group**
   - Left menu → **Subnet groups** → **Create DB subnet group**
   - Name: `game-store-db-subnet`
   - Description: `Subnet group for Game Store database`
   - VPC: Select your **default VPC**
   - Availability Zones: Select **ap-southeast-1a** and **ap-southeast-1b**
   - Subnets: Select all available subnets
   - Click **Create**

#### Step 2: Create Security Group

1. **Navigate to EC2 Console**
   - Go to: https://console.aws.amazon.com/ec2/
   - Left menu → **Security Groups** → **Create security group**

2. **Configure Security Group**
   - Name: `game-store-rds-sg`
   - Description: `Security group for Game Store RDS MySQL`
   - VPC: Select your **default VPC**

3. **Add Inbound Rule**
   - Click **Add rule**
   - Type: **MySQL/Aurora**
   - Protocol: **TCP**
   - Port range: **3306**
   - Source: **0.0.0.0/0** (for development - restrict in production!)
   - Description: `MySQL access from anywhere`

4. **Outbound Rules**
   - Leave default (All traffic)

5. Click **Create security group**

{{% notice warning %}}
**Security:** For production, restrict the source to your specific IP addresses or VPC CIDR only. Never expose databases publicly in production!
{{% /notice %}}

#### Step 3: Create RDS MySQL Instance

1. **Navigate to RDS Dashboard**
   - Click **Create database**

2. **Choose Database Creation Method**
   - Select: ⚪ **Standard create**

3. **Engine Options**
   - Engine type: **MySQL**
   - Version: **MySQL 8.0.39** (or latest 8.0.x)

4. **Templates**
   - Select: ⚪ **Free tier** (if eligible)
   - Or: ⚪ **Dev/Test** (for better performance)

5. **Settings**
   - DB instance identifier: `game-store-db`
   - Master username: `admin`
   - Master password: `GameStore2025!` (or your secure password)
   - Confirm password: `GameStore2025!`

{{% notice tip %}}
**Password Requirements:** Minimum 8 characters, at least one uppercase, one lowercase, one number, and one special character.
{{% /notice %}}

6. **Instance Configuration**
   - DB instance class: **db.t3.micro** (Free Tier eligible)
   - Storage type: **General Purpose SSD (gp3)**
   - Allocated storage: **20 GB**
   - ☑️ **Disable** storage autoscaling (for cost control)

7. **Connectivity**
   - Compute resource: **Don't connect to an EC2 compute resource**
   - VPC: **Default VPC**
   - DB subnet group: **game-store-db-subnet** (created earlier)
   - Public access: **Yes** (for development)
   - VPC security group: **Choose existing**
   - Select: **game-store-rds-sg**
   - Availability Zone: **ap-southeast-1a**

8. **Database Authentication**
   - Authentication: **Password authentication**

9. **Additional Configuration**
   - Initial database name: `identity_service`
   - DB parameter group: **default.mysql8.0**
   - Backup retention: **7 days** (adjust as needed)
   - ☑️ **Enable automated backups**
   - Backup window: **Preferred time** (e.g., 03:00-04:00 UTC)
   - ☑️ **Enable encryption** (recommended)
   - Monitoring: ☑️ **Enable Enhanced Monitoring** (optional)
   - Log exports: ☑️ **Error log**, ☑️ **Slow query log**

10. **Maintenance**
    - ☑️ **Enable auto minor version upgrade**
    - Maintenance window: **Preferred time** (e.g., Sun 04:00-05:00 UTC)

11. **Estimated Monthly Costs**
    - Review the cost estimate
    - Click **Create database**

{{% notice info %}}
**Creation Time:** RDS instance creation takes about 5-10 minutes. Status will change from "Creating" → "Available".
{{% /notice %}}

#### Step 4: Get RDS Endpoint

1. **Wait for DB Instance to be Available**
   - RDS Dashboard → **Databases**
   - Wait until status shows **Available**

2. **Copy Endpoint**
   - Click on **game-store-db**
   - In **Connectivity & security** tab
   - Copy the **Endpoint** value
   - Example: `game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com`
   - Port: `3306`

3. **Save Connection Details**
   ```
   Endpoint: game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com
   Port: 3306
   Database: identity_service
   Username: admin
   Password: GameStore2025!
   ```

#### Step 5: Test Connection

**Option A: Using MySQL CLI**

```bash
# Install MySQL client (if not installed)
# Windows: Download from https://dev.mysql.com/downloads/mysql/
# macOS: brew install mysql-client
# Linux: sudo apt-get install mysql-client

# Test connection
mysql -h game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com \
      -P 3306 \
      -u admin \
      -p

# Enter password when prompted: GameStore2025!

# You should see:
# mysql> 

# List databases
SHOW DATABASES;

# Select database
USE identity_service;

# Exit
EXIT;
```

**Option B: Using MySQL Workbench**

1. Download MySQL Workbench: https://dev.mysql.com/downloads/workbench/
2. Open MySQL Workbench
3. Click **+** to add new connection
4. Configure:
   - Connection Name: `Game Store RDS`
   - Hostname: `game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com`
   - Port: `3306`
   - Username: `admin`
   - Password: Click **Store in Vault** → Enter `GameStore2025!`
5. Click **Test Connection** → Should show "Successfully connected"
6. Click **OK** to save

**Option C: Using DBeaver (Recommended)**

1. Download DBeaver: https://dbeaver.io/download/
2. New Database Connection → MySQL
3. Fill in connection details
4. Test Connection → Should succeed
5. Click **Finish**

#### Step 6: Initialize Database Schema

Our Spring Boot application uses **JPA/Hibernate** with `spring.jpa.hibernate.ddl-auto=update`, so tables will be created automatically when the backend first connects.

However, you can manually verify or initialize:

```sql
-- Connect to RDS
mysql -h YOUR_RDS_ENDPOINT -u admin -p identity_service

-- Verify database
SHOW DATABASES;

-- Use database
USE identity_service;

-- List tables (should be empty initially)
SHOW TABLES;

-- After running Spring Boot app once, you'll see:
-- +----------------------------+
-- | Tables_in_identity_service |
-- +----------------------------+
-- | cart                       |
-- | cart_item                  |
-- | category                   |
-- | game                       |
-- | invalidated_token          |
-- | orders                     |
-- | order_detail               |
-- | payment                    |
-- | role                       |
-- | transaction                |
-- | user                       |
-- | user_role                  |
-- +----------------------------+
```

#### Step 7: Configure Spring Boot Application

1. **Update `application.yaml`**

```yaml
spring:
  datasource:
    url: jdbc:mysql://game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com:3306/identity_service
    username: admin
    password: GameStore2025!
    driver-class-name: com.mysql.cj.jdbc.Driver
  
  jpa:
    hibernate:
      ddl-auto: update  # Auto-create/update tables
    show-sql: true      # Log SQL queries
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQLDialect
        format_sql: true
```

2. **Or use Environment Variables** (Recommended)

```yaml
spring:
  datasource:
    url: jdbc:mysql://${RDS_ENDPOINT:localhost}:${RDS_PORT:3306}/${RDS_DATABASE:identity_service}
    username: ${RDS_USERNAME:root}
    password: ${RDS_PASSWORD:password}
```

Then set environment variables:
```bash
export RDS_ENDPOINT=game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com
export RDS_PORT=3306
export RDS_DATABASE=identity_service
export RDS_USERNAME=admin
export RDS_PASSWORD=GameStore2025!
```

#### Step 8: Test Backend Connection

1. **Navigate to Backend Directory**
   ```bash
   cd Back-End
   ```

2. **Set Environment Variables**
   ```bash
   # Windows PowerShell
   $env:RDS_ENDPOINT="game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com"
   $env:RDS_PORT="3306"
   $env:RDS_DATABASE="identity_service"
   $env:RDS_USERNAME="admin"
   $env:RDS_PASSWORD="GameStore2025!"
   ```

3. **Run Spring Boot Application**
   ```bash
   mvn spring-boot:run
   ```

4. **Verify Connection in Logs**
   Look for:
   ```
   Hikari Pool-1 - Starting...
   Hikari Pool-1 - Start completed.
   HHH000204: Processing PersistenceUnitInfo [name: default]
   HHH000412: Hibernate ORM core version ...
   HHH000400: Using dialect: org.hibernate.dialect.MySQLDialect
   ```

5. **Check Database Tables**
   ```bash
   mysql -h YOUR_RDS_ENDPOINT -u admin -p identity_service

   SHOW TABLES;
   # Should now see all entity tables created by Hibernate
   ```

#### Step 9: Verify Database Operations

Test CRUD operations through your API:

```bash
# Create a test user (via Postman or curl)
curl -X POST http://localhost:8080/identity/users \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "password": "Test123!",
    "email": "test@example.com"
  }'

# Verify in database
mysql -h YOUR_RDS_ENDPOINT -u admin -p identity_service

SELECT * FROM user;
# Should show the test user
```

#### RDS Monitoring & Maintenance

**CloudWatch Metrics:**
- RDS Dashboard → Select `game-store-db` → **Monitoring** tab
- Key metrics:
  - CPU Utilization
  - Database Connections
  - Read/Write IOPS
  - Free Storage Space

**Automated Backups:**
- RDS Dashboard → `game-store-db` → **Maintenance & backups** tab
- Backups are taken daily during backup window
- Retention: 7 days (configurable)

**Manual Snapshots:**
```bash
# Via AWS CLI
aws rds create-db-snapshot \
    --db-instance-identifier game-store-db \
    --db-snapshot-identifier game-store-db-snapshot-$(date +%Y%m%d)
```

#### Troubleshooting

**Issue: Cannot connect to RDS**

✅ **Check Security Group:**
```bash
# Verify security group allows port 3306
aws ec2 describe-security-groups \
    --group-names game-store-rds-sg \
    --query 'SecurityGroups[0].IpPermissions'
```

✅ **Check RDS Public Accessibility:**
- RDS Dashboard → `game-store-db` → **Connectivity & security**
- Publicly accessible: **Yes**

✅ **Test Network Connectivity:**
```bash
# Test if port is reachable
telnet game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com 3306
# Or
nc -zv game-store-db.c9xyz123456.ap-southeast-1.rds.amazonaws.com 3306
```

**Issue: "Access denied for user 'admin'"**

✅ Check credentials match exactly
✅ Reset password in RDS Console → **Modify** → **New master password**

**Issue: "Unknown database 'identity_service'"**

✅ Database wasn't created during RDS setup
✅ Create manually:
```sql
mysql -h YOUR_RDS_ENDPOINT -u admin -p
CREATE DATABASE identity_service;
```

#### Cost Optimization Tips

1. **Use Free Tier:** db.t3.micro is Free Tier eligible for 12 months
2. **Stop when not in use:** 
   ```bash
   aws rds stop-db-instance --db-instance-identifier game-store-db
   ```
3. **Reduce backup retention:** 1-3 days for dev/test
4. **Disable Multi-AZ:** Single-AZ is sufficient for development
5. **Monitor storage:** Delete old logs and data

#### Summary

You have successfully:
- ✅ Created RDS MySQL instance (db.t3.micro)
- ✅ Configured security groups for database access
- ✅ Tested connection using MySQL client
- ✅ Initialized database schema
- ✅ Connected Spring Boot backend to RDS
- ✅ Verified CRUD operations

**Next Step:** Configure S3 Storage for image uploads →

{{% notice info %}}
**Estimated Time:** 15-20 minutes for RDS setup and testing
{{% /notice %}}
