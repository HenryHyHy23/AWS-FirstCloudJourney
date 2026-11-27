---
title: "Connect Spring Boot to Supabase PostgreSQL"
menuTitle: "Supabase PostgreSQL & Authentication"
weight: 1
chapter: false
pre: "<b> 5.9.1. </b> "
---

1. **Open Supabase Dashboard → lab project → Connect.** Select a direct connection or session pooler according to task connectivity. If egress is IPv4-only and the direct endpoint needs IPv6, select the session pooler. Record the displayed host, port, database and user.

2. **Open Secrets Manager → backend secret → Edit.** Populate SUPABASE_DB_URL as a JDBC URL with the project's TLS settings, SUPABASE_DB_USERNAME and SUPABASE_DB_PASSWORD. Map each via ECS ValueFrom and deploy fresh tasks.

{{< btbfigure file="aws-secret-manager-step-1.webp" caption="Connection keys shown in the Secrets Manager capture." step="yes" >}}

3. **In Supabase open SQL Editor → New query → Run.** Verify the current database and role.


```sql
select current_database() as database_name, current_user as role_name;
select 1 as connection_ok;
```


4. **Open the BTB catalog and find a known demo SKU.** Compare it with the lab project data. For a connection failure check the pooler username, JDBC URL, task revision and egress.

**Verify.** SQL Editor verifies the database side. The backend catalog request verifies the connection from ECS. Determine Auth/JWT behavior from the deployed Spring Security configuration.
