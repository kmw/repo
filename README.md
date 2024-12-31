Deploying the 1 or 2 Odoo-18 each w/ Postgresql-17 setup via Portainer Stack / Git Repository / stack.env:

1. Clone the repository:
```bash
cd /home/kmw/github
git clone https://github.com/kmw/odoo18-deploy.git
cd odoo18-deploy
```

2. Create a stack.env file with your values:
```bash
# Database Configuration
ODOO_DB_USER=odoo
ODOO_DB_PASSWORD=yourpassword
ODOO_DB_FILTER_1=^domain1.*$$
ODOO_DB_FILTER_2=^domain2.*$$
note: replace domain1 and domain2

# Odoo Configuration
ODOO_ADMIN_PASSWORD_1=admin1password
ODOO_ADMIN_PASSWORD_2=admin2password

# Domain Configuration
DOMAIN_NAME_1=domain1
DOMAIN_NAME_2=domain2

# Paths
EXTRA_ADDONS_PATH=/home/kmw/extra-addons
```
note: /home/kmw is set but you may want to change it.

3. Deploy through Portainer:
- Go to Portainer dashboard
- Click Stacks → Add stack
- Name it "odoo18-dual"
- Choose "Repository" as build method
- Enter repository URL (github.com/kmw/odoo18-deploy)
- Set compose file path to docker-compose.yml.dual
- Upload stack.env
- Deploy the stack

The key configuration files and their variables are:

1. `stack.env` (for Portainer deployment):
```
ODOO_DB_USER
ODOO_DB_PASSWORD
ODOO_DB_FILTER_1
ODOO_DB_FILTER_2
ODOO_ADMIN_PASSWORD_1
ODOO_ADMIN_PASSWORD_2
DOMAIN_NAME_1
DOMAIN_NAME_2
EXTRA_ADDONS_PATH
```

2. `config/odoo-instance1.conf`:
```ini
admin_passwd = ${ODOO_ADMIN_PASSWORD_1}
db_user = ${ODOO_DB_USER}
db_password = ${ODOO_DB_PASSWORD}
dbfilter = ${ODOO_DB_FILTER_1}
```

3. `config/odoo-instance2.conf`:
```ini
admin_passwd = ${ODOO_ADMIN_PASSWORD_2}
db_user = ${ODOO_DB_USER}
db_password = ${ODOO_DB_PASSWORD}
dbfilter = ${ODOO_DB_FILTER_2}
```

The variable substitution process:
1. Portainer reads `stack.env`
2. Docker Compose uses these variables in `docker-compose.yml.dual`
3. Variables are passed to containers through environment section
4. Odoo reads its config files and substitutes variables at runtime
