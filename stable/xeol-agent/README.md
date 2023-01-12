# xeol-agent Helm Chart

xeol-agent runs as a read-only service account in the cluster it's deployed to.

In order to report the inventory to xeol.io, xeol-agent does require an api key for your xeol.io organization.
xeol-agent's helm chart automatically creates a kubernetes secret for the xeol.io apiKey
based on the values file you use, Ex.:

```yaml
xeolAgent:
  xeol:
    apiKey: foobar
```

It will set the following environment variable based on this: `XEOL_AGENT_API_KEY=foobar`.

If you don't want to store your xeol-agent ApiKey in the values file, you can create your own secret to do this:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: xeol-agent-api-key
type: Opaque
stringData:
  XEOL_AGENT_API_KEY: foobar
```

and then provide it to the helm chart via the values file:

```yaml
xeolAgent:
  existingSecret: xeol-agent-api-key
```

You can install the chart via via:

```
helm repo add xeol https://charts.xeol.io
helm install <release-name> -f <values.yaml> xeol/xeol-agent
```

A basic values file can always be found [here](https://github.com/noqcks/xeol-charts/tree/master/stable/xeol-agent/values.yaml)

The key configurations are in the xeolAgent.xeol section. xeol-agent must be able to resolve the xeol.io URL and requires API credentials.

Note: the Xeol APIKey can be provided via a kubernetes secret, or injected into the environment of the xeol-agent container
* For injecting the environment variable, see: inject_secrets_via_env
* For providing your own secret for the Xeol APIKey, see: xeolAgent.existing_secret. xeol-agent creates it's own secret based on your values.yaml file for key xeolAgent.xeol.apiKey, but the xeolAgent.existingSecret key allows you to create your own secret and provide it in the values file.

See the [xeol-agent repo](https://github.com/noqcks/xeol-agent) for more information about the xeol-agent specific configuration
