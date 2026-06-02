# 🔗 AWS VPC Peering Connection

**SIT706 Cloud Computing — Deakin University**
**Student:** Mitali Kaur | s224582251
**Grade:** Distinction (5.3D) ✅ | Lab Score: 30/30 🎯 | Quiz Score: 90% 🎯

---

## 📌 Project Overview

This project demonstrates the creation and configuration of a **VPC Peering Connection** on AWS, enabling private network communication between two VPCs without traversing the public internet. The lab covers peering setup, route table configuration, VPC Flow Logs, and end-to-end connectivity testing.

---

## 🏗️ Architecture

```
┌─────────────────────────┐         ┌─────────────────────────┐
│        Lab VPC          │         │       Shared VPC         │
│    (10.0.0.0/16)        │◄───────►│     (10.5.0.0/16)        │
│                         │  VPC    │                          │
│  Lab Public Subnet      │ Peering │  Shared Private Subnets  │
│  (EC2 + Inventory App)  │  pcx-   │  (RDS Database)          │
│                         │ Lab-Peer│                          │
└─────────────────────────┘         └─────────────────────────┘
         │                                      │
         └──────── VPC Flow Logs ───────────────┘
                  (CloudWatch Logs)
```

**Traffic Flow:** EC2 in Lab VPC → VPC Peering Connection → RDS in Shared VPC

---

## 🛠️ Technologies Used

| Service | Purpose |
|---|---|
| Amazon VPC | Virtual Private Cloud networking |
| VPC Peering | Private cross-VPC connectivity |
| Route Tables | Traffic routing between VPCs |
| VPC Flow Logs | Network traffic monitoring |
| Amazon CloudWatch | Flow log analysis and storage |
| Amazon EC2 | Inventory application server |
| Amazon RDS | Shared database (cross-VPC access) |

---

## ✅ Tasks Completed

### Task 1 — Creating a VPC Peering Connection
- Navigated to **VPC → Peering Connections** in AWS Console
- Created peering connection `Lab-Peer`:
  - **Requester VPC:** Lab VPC (`vpc-0c814ae4e1bdca7c4`)
  - **Accepter VPC:** Shared VPC (`vpc-0ffc1225d8a2cacd2`)
- Accepted the pending peering request
- Status confirmed: **Active** ✅

### Task 2 — Configuring Route Tables
Updated route tables in **both VPCs** to enable bidirectional traffic:

| Route Table | Destination | Target |
|---|---|---|
| Lab Public Route Table | 10.5.0.0/16 | pcx-0d970677e50ef14a1 |
| Shared VPC Route Table | 10.0.0.0/16 | pcx-0d970677e50ef14a1 |

### Task 3 — Enabling VPC Flow Logs
- Created flow log `SharedVPCLogs` on the Shared VPC
- **Destination:** CloudWatch Logs (`ShareVPCFlowLogs` log group)
- **Traffic type:** All (Accept + Reject)
- **Aggregation interval:** 1 minute
- **IAM Role:** `pc-flow-logs-role`
- Status: **Active** ✅

### Task 4 — Testing the VPC Peering Connection
- Accessed the Inventory application running on EC2 in Lab VPC
- Configured database settings to connect to RDS in Shared VPC via the peering connection
- Application successfully retrieved inventory data from the cross-VPC database ✅

### Task 5 — Analyzing VPC Flow Logs
- Opened **CloudWatch → Log Management → ShareVPCFlowLogs**
- Analysed log stream `eni-07ce8288c1a643a54-all`
- Observed both `ACCEPT` and `NODATA` entries confirming traffic flow through the peering connection

---

## 💡 Key Concepts Learned

### Q1: Can you peer 2 VPCs with the same CIDR ranges?
**No.** Routing between the VPCs would become ambiguous — AWS cannot determine the correct destination for network traffic. Each VPC must have a unique, non-overlapping CIDR block for peering to work.

### Q2: What route changes are needed after creating a peering connection?
Route tables in **both VPCs** must be updated. A new route must be added in each route table where:
- **Destination** = CIDR block of the other VPC
- **Target** = the VPC peering connection ID

This enables bidirectional traffic flow through the peering link.

### Q3: Can VPCA reach VPCC through VPCB?
**No.** VPC peering does **not** support transitive routing. Even if VPCA↔VPCB and VPCB↔VPCC are both peered, traffic from VPCA cannot pass through VPCB to reach VPCC. Each VPC pair requires its own direct peering connection.

---

## 📊 Results

| Component | Result |
|---|---|
| VPC Peering Connection | ✅ Active |
| Lab Public Route Table | ✅ Correct |
| Shared VPC Route Table | ✅ Correct |
| Application → Database | ✅ Connected |
| VPC Flow Logs | ✅ Active in CloudWatch |
| **Lab Score** | **30 / 30** |
| **Quiz Score** | **90% (90/100 points)** |

---

## 📚 References

- [AWS VPC Peering Guide](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)
- [VPC Flow Logs Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)
- [AWS Route Tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)

---

*Deakin University — School of Information Technology | SIT706 Cloud Computing*
