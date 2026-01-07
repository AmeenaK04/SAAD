## Context and Problem Statement

The CMS must be able to persist and manage large volumes of support/complaint related data for banking, telecom and airline organisations operating under a multi-tenant architecture. 
The system's database must ensure strict data isolation between organisations to maintain strong data integrity vital for organisations with massive consumer base such as 20 million users, 
with an expected 10% annual growth. Secured storage of consumers & roles within the CMS and audit data is essential to maintain integrity.

The problem I had faced was having to chose a database stategy, to first support prototyping, and then be capabale of developing a more scalable, secure and reliable major co-operate level development database.
---

## Considered Options

### PostgreSQL
- Cost effective as it is an open source, lowering the total cost of ownership compared to proprietary databases
- Co-operation level database with full ACID compliance (avoids validity errors and maintains data integrity)
- Advanced security features, which aligns with multi-tenant systems
- Well fit for large-scale, production level deployments

### SQLite
- File based relational database
- While starting up a new Postgres database is fairly involved (there are of course tools that make it easier), SQLite has no such issue
- Minimal configuration and setup overhead
- Ideal for local development and prototyping
- Not designed for large scale production workloads due to its limited scalability, performance drops when dealing with large data.

### MongoDB
- NoSQL database system, making it suitable for both structured and unstructured data
- Document oriented nature allows it to offer higher speed than traditional relational databases
- Suffer from duplicate data, making it difficult to manage your data efficiently, would make the system unreliable 
- High memory usage, which requires extra attention to keep under control. 

---

## Decision Outcome

A dual database strategy was implemented:
- **SQLite** was used during development to support rapid iteration, and fast prototyping of the CMS system.
- **PostgreSQL** However has now been implimented as the primary database for production deployment, providing the scalability, reliability, and security required for customers of major banking, telecom and airlines.
---

## Rationale

SQLite was selected for development because it enables fast, low-friction setup without requiring additional infrastructure. This supports rapid development of the CMS proof-of-concept, which is a key requirement for demonstrating system functionality to venture capital investors.
PostgreSQL was selected for production deployment due to its enterprise-grade capabilities, including strong transactional guarantees, high concurrency support, and robust data integrity mechanisms. These characteristics are essential for managing the CMS complaint lifecycle and enforcing data isolation in a multi-tenant environment.
This combined approach allows will allow ABC Limited to optimise developer productivity during early development while maintaining credibility for large banking and telecommunications clients.

---

## Consequences
- The system would require extra configurations and tools (Citus or pgpool) to enable effective horizontal scaling.
- The ACID compliance & complex query support will add inherent overhead that will reduces the performance.
- Production database requires tuning, monitoring, and operational expertise at scale.
