# Static Routing Configuration - Cisco Packet Tracer

## 📋 Project Overview
This project demonstrates inter-city network connectivity using **static routing** in Cisco Packet Tracer. The topology connects three cities (Chennai, Bangalore, and Coimbatore) through routers, enabling end-to-end communication between PCs across different networks.

## 🌐 Network Topology

```
Chennai Network          Bangalore Network        Coimbatore Network
┌─────────────┐         ┌──────────────┐         ┌───────────────┐
│   PC0       │         │    PC1       │         │     PC2       │
│ (Chennai)   │         │ (Bangalore)  │         │ (Coimbatore)  │
└──────┬──────┘         └──────┬───────┘         └───────┬───────┘
       │                       │                         │
   Switch0                 Switch1                   Switch2
  (2960-24TT)             (2960-24TT)               (2960-24TT)
       │                       │                         │
       │                       │                         │
 Chennai-Router ═══════ Bangalore-Router ═══════ Coimbatore-Router
    (2911)                   (2911)                    (2911)
```

### Network Segments:
- **Chennai Network**: Connected via chennai-router
- **Bangalore Network**: Central router connecting Chennai and Coimbatore
- **Coimbatore Network**: Connected via coimbatore-router

## 🎯 Objectives
- Configure static routing between three routers
- Enable inter-VLAN communication across different cities
- Verify end-to-end connectivity using ping and traceroute
- Understand routing table configurations

## 🔧 Configuration Details

### Router Interfaces & IP Addressing

#### Chennai Router
```cisco
Router>enable
Router#configure terminal
Router(config)#hostname chennai-router
chennai-router(config)#interface GigabitEthernet0/0
chennai-router(config-if)#ip address 100.0.0.1 255.255.255.0
chennai-router(config-if)#no shutdown
chennai-router(config-if)#exit

chennai-router(config)#interface GigabitEthernet0/1
chennai-router(config-if)#ip address 10.0.0.1 255.255.255.0
chennai-router(config-if)#no shutdown
chennai-router(config-if)#exit
```

#### Bangalore Router (Central Router)
```cisco
Router>enable
Router#configure terminal
Router(config)#hostname bangalore-router
bangalore-router(config)#interface GigabitEthernet0/0
bangalore-router(config-if)#ip address 10.0.0.2 255.255.255.0
bangalore-router(config-if)#no shutdown
bangalore-router(config-if)#exit

bangalore-router(config)#interface GigabitEthernet0/1
bangalore-router(config-if)#ip address 1.0.0.1 255.255.255.0
bangalore-router(config-if)#no shutdown
bangalore-router(config-if)#exit

bangalore-router(config)#interface FastEthernet0/0
bangalore-router(config-if)#ip address 101.0.0.1 255.255.255.0
bangalore-router(config-if)#no shutdown
bangalore-router(config-if)#exit
```

#### Coimbatore Router
```cisco
Router>enable
Router#configure terminal
Router(config)#hostname coimbatore-router
coimbatore-router(config)#interface GigabitEthernet0/0
coimbatore-router(config-if)#ip address 1.0.0.2 255.255.255.0
coimbatore-router(config-if)#no shutdown
coimbatore-router(config-if)#exit

coimbatore-router(config)#interface GigabitEthernet0/1
coimbatore-router(config-if)#ip address 200.0.0.1 255.255.255.0
coimbatore-router(config-if)#no shutdown
coimbatore-router(config-if)#exit
```

### Static Routes Configuration

#### Chennai Router
```cisco
chennai-router(config)#ip route 101.0.0.0 255.255.255.0 10.0.0.2
chennai-router(config)#ip route 200.0.0.0 255.255.255.0 10.0.0.2
chennai-router(config)#ip route 1.0.0.0 255.255.255.0 10.0.0.2
```

#### Bangalore Router
```cisco
bangalore-router(config)#ip route 100.0.0.0 255.255.255.0 10.0.0.1
bangalore-router(config)#ip route 200.0.0.0 255.255.255.0 1.0.0.2
```

#### Coimbatore Router
```cisco
coimbatore-router(config)#ip route 100.0.0.0 255.255.255.0 1.0.0.1
coimbatore-router(config)#ip route 101.0.0.0 255.255.255.0 1.0.0.1
coimbatore-router(config)#ip route 10.0.0.0 255.255.255.0 1.0.0.1
```

### PC Configuration

#### PC0 (Chennai)
- **IP Address**: 100.0.0.10
- **Subnet Mask**: 255.255.255.0
- **Default Gateway**: 100.0.0.1

#### PC1 (Bangalore)
- **IP Address**: 101.0.0.10
- **Subnet Mask**: 255.255.255.0
- **Default Gateway**: 101.0.0.1

#### PC2 (Coimbatore)
- **IP Address**: 200.0.0.10
- **Subnet Mask**: 255.255.255.0
- **Default Gateway**: 200.0.0.1

## ✅ Verification & Testing

### Check Interface Status
```cisco
Router#show ip interface brief
```

### View Routing Table
```cisco
Router#show ip route
```
Expected output should show:
- Directly connected networks (C)
- Static routes (S)

### Test Connectivity
From PC0 (Chennai):
```
C:\>ping 101.0.0.10
C:\>ping 200.0.0.10
```

From PC1 (Bangalore):
```
C:\>ping 100.0.0.10
C:\>ping 200.0.0.10
```

### Trace Route Path
```
C:\>tracert 200.0.0.10
```
This shows the hop-by-hop path packets take through the routers.

## 📊 IP Addressing Scheme

| Device | Interface | IP Address | Subnet Mask | Network |
|--------|-----------|------------|-------------|---------|
| chennai-router | Gi0/0 | 100.0.0.1 | 255.255.255.0 | LAN |
| chennai-router | Gi0/1 | 10.0.0.1 | 255.255.255.0 | WAN |
| bangalore-router | Gi0/0 | 10.0.0.2 | 255.255.255.0 | WAN |
| bangalore-router | Gi0/1 | 1.0.0.1 | 255.255.255.0 | WAN |
| bangalore-router | Fa0/0 | 101.0.0.1 | 255.255.255.0 | LAN |
| coimbatore-router | Gi0/0 | 1.0.0.2 | 255.255.255.0 | WAN |
| coimbatore-router | Gi0/1 | 200.0.0.1 | 255.255.255.0 | LAN |
| PC0 | NIC | 100.0.0.10 | 255.255.255.0 | Chennai |
| PC1 | NIC | 101.0.0.10 | 255.255.255.0 | Bangalore |
| PC2 | NIC | 200.0.0.10 | 255.255.255.0 | Coimbatore |

## 🛠️ Equipment Used
- **Routers**: Cisco 2911 (x3)
- **Switches**: Cisco 2960-24TT (x3)
- **End Devices**: PC-PT (x3)
- **Cables**: Copper Straight-Through

## 📚 Key Concepts Demonstrated
1. **Static Routing**: Manual configuration of routes between networks
2. **Multi-hop Routing**: Packets traversing multiple routers
3. **Default Gateway**: Proper gateway configuration on end devices
4. **Routing Tables**: Understanding directly connected vs. static routes
5. **Network Segmentation**: Separating networks by geography/function

## 🚀 How to Run This Project
1. Download and install [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer)
2. Clone this repository
3. Open the `.pkt` file in Packet Tracer
4. Enter simulation mode to see packet flow
5. Test connectivity using ping commands from any PC

## 📁 Repository Structure
```
static-routing-cisco-packet-tracer/
├── README.md
├── topology.pkt
├── screenshots/
│   └── network-topology.png
└── configs/
    ├── chennai-router-config.txt
    ├── bangalore-router-config.txt
    └── coimbatore-router-config.txt
```

## 💡 Learning Outcomes
- Understanding static routing configuration
- Configuring router interfaces and IP addressing
- Troubleshooting network connectivity issues
- Using Cisco IOS commands effectively
- Building multi-router topologies

## 🔍 Troubleshooting Tips
- Verify all interfaces are `up/up` using `show ip interface brief`
- Check routing tables have correct static routes
- Ensure PCs have correct gateway configured
- Use `tracert` to identify where packets are being dropped
- Verify subnet masks match on both ends of connections

## 📧 Contact
Feel free to reach out if you have questions about this project!

## 📜 License
This project is for educational purposes.

---
**Note**: This project was created as part of learning network fundamentals and Cisco router configuration.
