# Module 9 - Software Architecture
### Context Diagram
![context](assets/img/ContextDiagram.png)
### Container Diagram
![container](assets/img/ContainerDiagram.png)
### Deployment Diagram
![deployment](assets/img/DeploymentDiagram.png)

### Project Architectural Risks

| Risk | Point of Failure | Impact | Severity |
| :------: | :------: | :------: | :------: |
| Single API Gateway is a bottleneck. As the only entry point for React and Mobile, a gateway crash results in total system downtime. | API Gateway | Availability | High |
| All microservices share separate DBs but Auth DB is a dependency for every service. If every request requires a synchronous call to the User Service to verify roles, latency increases exponentially and the User Service becomes a throughput bottleneck. | Performance and Coupling | Auth Service | High |
| Payroll and Harvest records are high-volume. Without a caching layer, both services can put an unnecessary large load on the database. | Payroll and Harvest Services | Scalability | Medium |
| File storage (harvest photos) is not defined in deployment. It is thus unscalable and any data is lost if the container restarts. | Harvest Service | Scalability, Reliability, and Data Integrity | Medium |
| No rate limiting on API Gateway. Mandor/Admin bulk actions could flood services. | API Gateway | Performace | Medium |

Based on the table above, below are the expected new future architecture:
- **High Availability Gateway**: Introduce a Load Balancer (like Nginx or AWS ALB) in front of multiple instances of the Spring Cloud Gateway.
- **Distributed Caching Layer**: Integrate Redis to store session data and reduce database hits.
- **Object Storage Integration**: Move file storage from the local container filesystem to an external S3-compatible Object Storage.
- **Sidecar or JWT Local Validation**: Instead of the Gateway calling the Auth DB for every request, move JWT verification locally where each service can validate identity locally using a public key.

## Manajemen Kebun
### Component Diagram
![kebun_component](assets/img/Module%20Management%20Kebun/ModuleComponentDiagram.png)
### Code Diagrams
![kebun_repo_code](assets/img/Module%20Management%20Kebun/RepositoryCodeDiagram.png)
![kebun_business_code](assets/img/Module%20Management%20Kebun/BusinessCodeDiagram.png)

## Manajemen Pengiriman Hasil Panen
### Code Diagram
![Darrell_Code](assets/img/Module%20Pengiriman/Darrell_Code%20Diagram.png)

### Component Diagram
![Darrell_Component](assets/img/Module%20Pengiriman/Darrell_Component%20Diagrm.png)