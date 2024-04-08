# Metabase

Metabase is an open-source business intelligence (BI) tool that allows users to easily query, visualize, and analyze their data without the need for complex SQL knowledge or coding skills. With Metabase, users can create interactive dashboards, explore datasets, and generate insightful reports to gain valuable insights into their data.

## Features

**1. Easy Querying**
**2. Visualizations**
**3. Interactive Dashboards**
**4. Collaboration**
**5. Data Exploration**
**6. Integrations**

## Installation

### Docker

I use postgres container, so I have to link metabase network directly to host network (`--network=host`) to be able to connect to the postgres container.

```bash
docker run -d --restart=always --network=host --name metabase metabase/metabase
```
