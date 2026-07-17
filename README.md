# hashi-vault-aap-demo

## Vault CLI playbook

The playbook in `approle-creds.yml` configures Vault from the CLI on the `vault`
host. It performs these steps:

- logs in with the `admin` user via `userpass`
- enables the `secret` KV engine as version 2 when it is not already present
- writes the `aws_creds` secret with `access_key` and `secret_key`
- creates the `aap-terraform-policy` ACL policy
- enables the `approle` auth method when needed
- creates the `aapterraform` AppRole and prints a generated `role_id` and `secret_id`
- validates that the AppRole can log in successfully

### Requirements

- the inventory must contain a host named `vault`
- the target host must have the Vault CLI installed and be able to reach `http://127.0.0.1:8200`
- provide AWS credentials through environment variables or extra vars

### Example

```bash
export AWS_ACCESS_KEY_ID=REPLACE_ME
export AWS_SECRET_ACCESS_KEY=REPLACE_ME
ansible-playbook -i inventory approle-creds.yml
```

Optional variables:

- `vault_addr` defaults to `http://127.0.0.1:8200`
- `vault_username` defaults to `admin`
- `vault_password` defaults to `ansible123!` and can be overridden with `VAULT_ADMIN_PASSWORD`
- `vault_approle_test_login` defaults to `true`

Each playbook run creates a new AppRole `secret_id`. Capture the reported `role_id`
and `secret_id` values and use them when creating the HashiCorp Vault Secret Lookup
credential in Ansible Automation Platform.