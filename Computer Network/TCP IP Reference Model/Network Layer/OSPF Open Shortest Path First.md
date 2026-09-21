> a link-state routing protocol that was developed for IP networks and is based on the **Shortest Path First (SPF)** algorithm

# LSDB synchronization process
Discover neighbor: A sends Hello to neighbors. neighbors replies Hello to A
Establish bidirectional communication
Elect a **designated router**, if desired
Form an **adjacency**
Discover the network routes
Update and synchronize **LSDB(Link State Database)** with **LSA(Link State Advertisement)**

**Adjacency**: a relationship formed between selected neighbors and allows them to exchange routing information)
    Two routers become adjacent if:
    At least one of them is DR or BDR (on multiaccess type networks), or
    They are interconnected by a point-to-point network type or virtual link

**Designated Router DR**:
    one designated router per multiaccess network
    generates network link advertisements
    assists in database synchronization
    **Backup Designated Router BDR**
# Packet Types
Hello: Used to discover who the neighbors are
Database Description DBD: Announces which updates the sender has
Link-State Request LSR: Requests information from the partner
Link-State Update LSU: Provides the sender’s costs to its neighbors
Link-State Acknowledgement LSAck: Acknowledges link state update
# Area
**Area**:  
    All routers belonging to the same area have identical  
    SPF calculation is performed separately for each area
    flooding of LSP is bounded by area
    **Area 0**: 骨干区域，非骨干区域通常必须通过 ABR 连接到 Area 0，跨区域路由一般要经过 Area 0，由 ABR 通过 Type 3 LSA 实现区域间路由通告。
    
**ABR（Area Border Router，区域边界路由器）**：OSPF 内部不同区域之间，转发区域间路由
**ASBR（Autonomous System Boundary Router，自治系统边界路由器）**：OSPF 与外部网络/其他协议之间，引入外部路由