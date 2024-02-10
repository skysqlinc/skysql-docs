# Scaling horizontally, HA

!!! Note
    COMING SOON 


SkySQL, unlike hyperscalers, deploy replicas in a active-active configuration. When primary crashes our smart proxy(maxscale) allows us to failover near instantly to an alternate replica. Or, failback when the original primary recovers. Ensuring data consistency even when replicas have a replication lag through “causal reads”, or transaction replay. 

Our underlying k8s based operator has smarts to rebuild replicas that lag a lot using cloud native snapshots.