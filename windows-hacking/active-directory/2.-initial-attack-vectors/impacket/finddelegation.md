# findDelegation

## `Impacket-findDelegation`

_Simple script to quickly list all delegation relationships (unconstrained, constrained, resource-based constrained) in an AD environment._

```bash
findDelegation.py "DOMAIN"/"USER":"PASSWORD"

# --user feature in 2021
findDelegation.py -user "account" "DOMAIN"/"USER":"PASSWORD"
```

## More about delegations

{% embed url="https://www.thehacker.recipes/ad/movement/kerberos/delegations" %}
