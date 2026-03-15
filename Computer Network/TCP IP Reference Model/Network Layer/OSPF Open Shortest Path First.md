> a link-state routing protocol that was developed for IP networks and is based on the Shortest Path First (SPF) algorithm
- High level functionalities
    
    Discover neighbors and form adjacencies  
    Flood link-state database (LSDB) information  
    Compute the shortest path  
    Install routes in the route-forwarding table  
    Detect changes in the link state  
    Propagate changes to maintain link-state database  
    synchronization  
    
- **Area**:  
    All routers belonging to the same area have identical  
    **LSDB(Link State Database)**
    
    SPF calculation is performed separately for each area
    
    flooding of LSP is bounded by area
    
- **Designated Router DR**:
    
    one designated router per multiaccess network
    
    generates network link advertisements
    
    assists in database synchronization
    
    **Backup Designated Router BDR**
    
- **Packet Types**:
    
    Hello: Used to discover who the neighbors are
    
    Database Description DBD: Announces which updates the sender has
    
    Link-State Request LSR: Requests information from the partner
    
    Link-State Update LSU: Provides the sender’s costs to its neighbors
    
    Link-State Acknowledgement LSAck: Acknowledges link state update
    
- **LSDB synchronization process**
    
    Discover neighbor: A sends Hello to neighbors. neighbors replies Hello to A
    
    Establish bidirectional communication
    
    Elect a designated router, if desired
    
    Form an adjacency
    
    **adjacency**: a relationship formed between selected neighbors and allows them to exchange routing information)
    
    Two routers become adjacent if:
    
    At least one of them is DR or BDR (on multiaccess type networks), or
    
    They are interconnected by a point-to-point network type or virtual link
    
    Discover the network routes
    
    Update and synchronize link-state databases
    
    - OSPF routers progress through seven states:
        
        Down: no active neighbor detected  
        INIT: hello packet received  
        Two-way: own router ID in received hello  
        Exstart: master and slave roles determined (by higher router id)  
        Exchange: database description packets sent  
        Loading: exchange of LSRs and LSUs  
        Full: neighbors fully adjacent