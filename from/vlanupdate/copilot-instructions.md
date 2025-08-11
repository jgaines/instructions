# GitHub Copilot Instructions for vlanupdate

## Architecture Overview

This is a network automation tool that updates VLAN information for switches in
an Equipment Management Database (EMDB).q The system uses SNMP to query switch
configurations and updates a central database via REST API.

**Key Components:**

- **`vlanupdate.py`** - Main script that queries switches via SNMP and updates
  EMDB
- **Two-repo structure**: Code in `eng-tools/vlanupdate`, deployment configs in
  `administrator/vlanupdate`
- **Docker containerization** with cron scheduling (hourly execution)
- **Multi-vendor support**: Cisco and Alcatel switches with different SNMP MIB
  handling

**Testing Commands**:

```bash
# Doctests with coverage
pytest --doctest-modules --cov=vlanupdate --cov-report=term-missing -v vlanupdate.py

# Local testing against specific switch
./vlanupdate.py -vvvn <switch_name>

# Docker testing (requires EMDB stack)
cd ~/git/administrator/vlanupdate && just start
docker exec vlanupdate-app-1 ./run_vlanupdate.sh -vvvn <switch_name>
```

## Environment Setup

**Dependencies**: Requires proprietary `arbor.*` libraries from internal PyPI
indexes:

```bash
uv pip install -e ~/git/qa-tools/qa \
--index http://nexus.eng.netscout.com/repository/qa-tools-group/simple \
--index https://arb-artifactory.eng.netscout.com/artifactory/api/pypi/qa-pypi-local/simple
```

**Critical Environment Variables**:
- `BBTOKEN` - Required for Bitbucket API authentication (never commit to git)
- `EMDB_API_HOST`/`EMDB_API_PORT` - Target EMDB API endpoint

## Code Patterns

**SNMP Operations**: Uses `arbor.net.snmp` wrapper around pysnmp:
```python
result, data = snmpWalk(fqdn, snmp_auth, oid)
if result is None and data:  # Success check pattern
```

**Vendor-Specific Logic**:
- Cisco: Uses `CISCO-VTP-MIB` and `CISCO-VLAN-MEMBERSHIP-MIB`
- Alcatel: Uses `ALCATEL-IND1-VLAN-MGR-MIB`
- Different OID structures require separate processing functions

**Error Handling**: Uses specific exception types and detailed logging:
```python
try:
    # SNMP operations
except PySnmpError:
    LOG.exception("Exception getting snmp data")
    fail_count["SNMP errors"] += 1
```

**Dry Run Pattern**: `@dry_run` decorator for safe testing without database
updates

## Testing Integration

**Multi-container testing**: Requires EMDB stack (API, DB, UI) + vlanupdate
container with shared Docker network (`emdb_default`). Use `just start` in
deployment repo to orchestrate startup order.

**Port mapping gotcha**: Inside container network, connect to
`emdb-api-dev:443`, not `:9443` (host port mapping).

## Deployment Structure

- Production uses `production.yml` with real EMDB endpoints
- Development uses `docker-compose.yml` with `-dev` endpoints
- Base configuration in `base.yml` with image version pinning
- Cron job defined in `cronjob` file, copied to `/etc/cron.d/` in container
